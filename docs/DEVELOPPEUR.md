# 🛠️ Documentation développeur — CHAPE-MEDIA

Documentation technique du projet : architecture, build, release, tests.
Pour les fiches utilisateur, voir [README.md](README.md) de ce dossier.

## 1. Arborescence du dépôt

```text
dna/
├── app/                    Application (le code source)
│   ├── app.py              Point d'entrée : fenêtre pywebview, verrou d'instance
│   │                       unique (port 8877), vérification de licence, màj auto
│   ├── core/               Modules métier (compilés Cython au build)
│   │   ├── config.py       Constantes, chemins, formats, détection ffmpeg, version
│   │   ├── database.py     SQLite (schéma, requêtes, FTS5, sauvegardes, migrations)
│   │   ├── storage.py      Persistance (réglages, historique, médiathèque, journal)
│   │   ├── stream.py       Serveur HTTP local de streaming (HTTP/1.1, 206 Range)
│   │   ├── downloader.py   Téléchargement/conversion yt-dlp (fragments parallèles)
│   │   ├── file_manager.py Scan de la médiathèque, explorateur/OS
│   │   ├── api.py          Pont JavaScript ↔ Python (long-polling, cache)
│   │   ├── updater.py      Mise à jour (manifeste, SHA-256, rollback)
│   │   └── thumb_cache.py  Cache des vignettes
│   ├── web/                Interface HTML/CSS/JS chargée par pywebview
│   │   ├── index.html      Page unique (charge app*.js + style*.css)
│   │   ├── app*.js         Logique front (4 fichiers, tous utilisés)
│   │   ├── style*.css      Styles (style.css + style2.css)
│   │   ├── icon.svg/.png/.ico  Icônes (régénérées par gen_icon.py)
│   │   └── ...
│   ├── ChapeMedia.spec     Spécification PyInstaller (SOURCE UNIQUE du packaging)
│   ├── setup_core.py       Recréé à chaque build par build_pyinstaller.ps1
│   ├── gen_icon.py         Génère icon.ico/.png depuis icon.svg (nécessite pillow)
│   ├── requirements.txt    Dépendances Python
│   └── version_info.txt    Métadonnées Windows de l'exe (généré, ne pas éditer)
├── scripts/                Scripts d'automatisation (PowerShell)
│   ├── build_all.ps1       ⭐ ORCHESTRATEUR : une commande = build complet
│   ├── build_cython.ps1    Cython seul (core\*.py -> .pyd, gcc MinGW)
│   ├── build_pyinstaller.ps1  Cython + PyInstaller (via ChapeMedia.spec)
│   ├── make_release.ps1    Zip + manifest.json + SHA-256 + CHANGELOG
│   ├── download_mingw.ps1  Installe le compilateur C MinGW-w64 (C:\mingw64)
│   ├── gen_version_file.py Injecte APP_VERSION dans les métadonnées de l'exe
│   └── release.ps1, smoke.ps1  Utilitaires divers
├── tests/                  Self-tests (db, ui, update, front, audit...)
├── docs/                   Fiches utilisateur (installation, usage, dépannage)
├── packaging/              LISEZMOI.txt et NOTICE.txt embarqués dans le zip
├── release/                Sorties : zips + manifest.json (+ archive\ des anciennes)
├── CHANGELOG.md            Historique des versions (alimenté par make_release.ps1)
└── README.md               Présentation utilisateur du projet
```

## 2. Environnement de développement

```powershell
# Venv + dépendances (une seule fois)
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r app\requirements.txt pyinstaller Cython

# Compilateur C pour Cython (une seule fois) -> C:\mingw64
powershell -NoProfile -ExecutionPolicy Bypass -File scripts\download_mingw.ps1
```

Le build force `mingw32` (gcc) : MSVC n'est pas requis.

## 3. Build et release (la commande unique)

```powershell
# Build complet : Cython -> PyInstaller -> vérification du bundle
powershell -NoProfile -ExecutionPolicy Bypass -File scripts\build_all.ps1

# Jusqu'au paquet publiable (zip + manifest + CHANGELOG)
powershell -NoProfile -ExecutionPolicy Bypass -File scripts\build_all.ps1 `
    -Release -Notes "Résumé des changements"

# Publication directe vers le dossier servi en ligne
... -Release -PublishDir D:\pub\chape-media -BaseUrl https://mon-site/chape-media

# Variantes
... -Clean          # purge complète avant de repartir de zéro
... -Skip Cython    # réutilise les .pyd déjà compilés (itération rapide)
... -Skip Exe       # ne fait que la compilation Cython
```

### Ce que fait `build_all.ps1`, étape par étape

1. **Prérequis** : venv, installe automatiquement Cython/PyInstaller/pywebview
   s'ils manquent, détecte (ou installe) gcc MinGW.
2. **Cython** : chaque `core/*.py` (sauf `__init__.py`) → `.c` → `.pyd`.
   Le `.py` source reste au dépôt, il n'est pas empaqueté dans le bundle.
3. **PyInstaller** : lit `app/ChapeMedia.spec` (ne jamais le régénérer à la main —
   datas, hiddenimports et version y sont maintenus).
4. **Contrôle qualité** : échoue si l'exe est absent, si des `.py` du core
   apparaissent dans `dist\ChapeMedia\_internal\core`, ou si les métadonnées
   de l'exe ne correspondent pas à `APP_VERSION`.
5. **Release** (option `-Release`) : version lue dans `app/core/config.py`
   (`APP_VERSION` — source unique), zip dans `release/`, `manifest.json`
   (SHA-256, taille, notes), entrée `CHANGELOG.md` sans doublon.

### La chaîne complète en un coup d'œil

```text
Code source (.py)                       [app/core/*.py]
   │  tests\*.py — vérification avant build
   ▼
1. Prérequis        (venv, Cython, PyInstaller, pywebview, gcc MinGW)
2. Cython           core\*.py ──► core\*.c ──► core\*.pyd   (code compilé)
3. PyInstaller      app\ChapeMedia.spec ──► dist\ChapeMedia\ChapeMedia.exe
   │                   + _internal\ (ffmpeg, web/, binaires)
4. Contrôle qualité exe présent ? .py du core invisibles ? version = APP_VERSION ?
   ▼
5. make_release.ps1 (-Release)
   │  zip + SHA-256 ──► release\ChapeMedia-vX.Y.Z-win64.zip
   │  manifest.json (version, hash, taille, notes, date)
   │  entrée CHANGELOG.md
   ▼
Publication        zip + manifest.json copiés ensemble vers le canal choisi
                    (dossier servi en ligne via -PublishDir, partage, clé USB)
   ▼
Mise à jour chez l'utilisateur
   app installée lit le manifest.json à chaque démarrage (≤ 24 h)
   ▸ vérifie SHA-256 après téléchargement
   ▸ remplace les fichiers du programme uniquement (données intactes)
   ▸ rollback automatique si le remplacement échoue
```

## 4. Publier une nouvelle version — procédure complète

1. Modifier le code, tester (`tests\*.py`, lancement depuis `app\`).
2. Incrémenter `APP_VERSION` (et éventuellement `APP_CHANNEL` : `stable`/`beta`)
   dans **`app/core/config.py`** — c'est le seul endroit.
3. Ajouter une entrée dans `CHANGELOG.md` (ou laisser `make_release.ps1` le faire
   via `-Notes`).
4. Lancer :

   ```powershell
   powershell -NoProfile -ExecutionPolicy Bypass -File scripts\build_all.ps1 -Release -Notes "..."
   ```

5. Publier **le zip ET `release\manifest.json`** au même endroit (serveur, partage,
   clé USB). Les apps installées détectent la nouveauté en ≤ 24 h et proposent
   la mise à jour ; rien ne s'installe sans accord de l'utilisateur.

## 5. Points sensibles à connaître

- **Verrou d'instance unique** : `app.py` bind le port TCP 8877 ; le cache
  WebView2 partagé n'est activé que si ce port est libre.
- **Licence** : sans limite de date ; l'application reste utilisable indefiniment.
- **`ffmpeg.exe`** : embarqué à la racine du bundle (datas de la spec) ;
  `config.py` le localise via `sys.frozen`/`sys._MEIPASS`.
- **Ne jamais modifier** `dist/`, `build/`, `version_info.txt`, `setup_core.py`
  à la main : tout est régénéré par le build.
- **Artefacts régénérables** (peuvent être supprimés sans risque) :
  `app/build/`, `app/dist/`, `core/*.c`, `core/*.pyd`, `__pycache__/`,
  `.pytest_cache/`. La commande `build_all.ps1 -Clean` s'en charge.
- **Ne PAS supprimer** : `app2.js`, `app3.js`, `app4.js`, `style2.css` —
  ils sont chargés par `index.html` (lignes 174-177 / 11-12).

## 6. Tests

```powershell
.venv\Scripts\Activate.ps1
pytest tests\                              # tests unitaires
python tests\db_selftest.py                # base SQLite + FTS5
python tests\ui_selftest.py                # interface pywebview
python tests\update_selftest.py            # mécanique de mise à jour
python tests\front_selftest.js             # front (Node.js requis)
```

## 7. Dépannage build

| Symptôme                               | Cause probable                        | Solution                                                          |
| -------------------------------------- | ------------------------------------- | ----------------------------------------------------------------- |
| `gcc introuvable`                      | MinGW non installé                    | `scripts\download_mingw.ps1`                                      |
| Échec Cython sur un module             | Syntaxe non supportée par Cython      | Tester `python -m cython core\<module>.py` ; simplifier le code   |
| Exe lance mais fenêtre vide            | WebView2 Runtime absent               | Installer le Runtime Edge WebView2                                |
| `.py` du core visibles dans le bundle  | Spec modifiée / .pyd périmés          | `build_all.ps1 -Clean` puis rebuild                               |
| `PyInstaller` écrit sur stderr         | Comportement normal                   | Ne pas interpréter comme une erreur ; seul `$LASTEXITCODE` compte |
| `AttributeError: 'NoneType' object has no attribute 'reconfigure'` au lancement de l'exe | Exe lancé sans console : `sys.stdout`/`sys.stderr` valent `None` (mode windowed) | Déjà géré dans `app.py` (tests `if sys.stdout is not None`) |
