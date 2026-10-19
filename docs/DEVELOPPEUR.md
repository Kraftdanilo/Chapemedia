# 🛠️ Documentation développeur — CHAPE-MEDIA v1.0.5

Documentation technique du projet : architecture, lancement, release, tests.
Pour les fiches utilisateur, voir [README.md](README.md) de ce dossier.

## 1. Arborescence du dépôt

```text
dna/
├── app/                    Application (le code source)
│   ├── app.py              Point d'entrée : démarre le backend FastAPI puis la
│   │                       fenêtre pywebview (verrou d'instance unique port 8907)
│   ├── server.py           Backend FastAPI : expose core/api.Api en REST
│   │                       (POST/GET /api/<nom>, port 8928, sert web/)
│   ├── core/               Modules métier (Python pur)
│   │   ├── config.py       Constantes, chemins, formats media, détection ffmpeg,
│   │   │                   réglages de perf, version (APP_VERSION = source unique)
│   │   ├── database.py     Couche SQLite (schéma, requêtes, FTS5, sauvegardes,
│   │   │                   migrations)
│   │   ├── storage.py      Persistance (chape_media.db : réglages, historique,
│   │   │                   médiathèque, journal)
│   │   ├── stream.py       Serveur HTTP local de streaming (HTTP/1.1, 206 Range,
│   │   │                   keep-alive, port 8929 par défaut ; /media, /share et
│   │   │                   /poster limités à APP_DATA/thumbs + dossier de téléchargement)
│   │   ├── downloader.py   Téléchargement/conversion yt-dlp (fragments parallèles,
│   │   │                   secours Cobalt puis gallery-dl, oEmbed)
│   │   ├── file_manager.py Scan de la médiathèque, explorateur/OS
│   │   ├── api.py          Métier exposé en REST par server.py (long-polling
│   │   │                   événements, cache médiathèque)
│   │   ├── updater.py      Mise à jour auto (manifest.json, SHA-256, rollback)
│   │   └── thumb_cache.py  Cache local des vignettes (hotlinks expirés)
│   ├── web/                Interface HTML/CSS/JS servie par FastAPI
│   │   ├── index.html      Page unique
│   │   ├── bridge.js       Adaptateur : window.backend (proxy fetch HTTP)
│   │   ├── app.js          Logique front (bundle unique, chargé par index.html)
│   │   ├── style.css       Feuille unique (base + composants + remix)
│   │   └── icon.svg/.png/.ico  Icônes (régénérées par gen_icon.py)
│   ├── gen_icon.py         Génère icon.ico/.png depuis icon.svg (nécessite pillow)
│   ├── smoke_*.py          Simulations de bout en bout (lecteur, vignettes, profil)
│   ├── audit_roles.py      Contrôle d'architecture (38 règles, 0 échec attendu)
│   └── requirements.txt    Dépendances Python
├── docs/                   Fiches utilisateur (installation, usage, dépannage)
├── release/                manifest.json + paquet Windows courant (seul publié)
├── CHANGELOG.md            Historique des versions
└── README.md               Présentation utilisateur du projet
```

> Le dépôt de distribution ne contient **que** ce qui est publié (cf. `.gitignore`,
> liste blanche) : docs, manifeste et paquet courant. Le code source, les scripts
> (`scripts/make_release.ps1`, `scripts/ChapeMedia.spec`) et `packaging/` vivent
> dans le dépôt privé ; `LISEZMOI.txt` et `NOTICE.txt` sont **embarqués dans le zip**.

## 2. Environnement de développement

```powershell
# Venv + dépendances (une seule fois)
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r app\requirements.txt
```

Dépendances (`app/requirements.txt`) : `yt-dlp`, `curl_cffi` (impersonation navigateur,
requis par l'extracteur TikTok), `pywebview`, `fastapi`, `uvicorn[standard]`, `pillow`,
`gallery-dl` (moteur de secours TikTok/Facebook/Instagram).

Aucun compilateur C : le backend est Python pur (FastAPI) et l'app se lance en
sources. La compilation (PyInstaller, mode onedir) est déclenchée par
`make_release.ps1 -Build` (ou par le workflow `release.yml` dans la CI).

## 3. Lancer l'application

```powershell
.venv\Scripts\Activate.ps1
cd app
python app.py         # démarre FastAPI (port 8928) + la fenêtre pywebview
```

Backend seul (utile pour tester le front dans un navigateur) :

```powershell
cd app
python -m uvicorn server:app --port 8928        # puis ouvrir http://127.0.0.1:8928
```

## 4. Architecture front ↔ back

- **`bridge.js`** définit `window.backend`, un `Proxy` JavaScript : chaque
  `backend.methode(args...)` devient un `POST /api/methode` avec le corps
  `{"__args__": [...]}`. Il émet l'événement `backendready` après le healthcheck
  (`/api/health`), c'est lui qui déclenche le démarrage du front.
- **`server.py`** génère une route POST **et** GET par méthode publique de
  `core.api.Api` (introspection à l'import). Payload objet simple = kwargs ;
  sinon un seul argument positionnel (ex. `start_download(opts)`).
  Les méthodes sync (SQLite, subprocess) tournent dans un thread pool
  (`run_in_executor`). CORS ouvert, fichiers statiques montés en dernier.
- **Long-polling** : le front maintient une connexion `poll_events`
  (`POLL_WAIT_MAX` 25 s) pour recevoir progression de téléchargement, toasts,
  changements d'état — plus aucun rafraîchissement à intervalle.
- **Streaming** : le lecteur passe par le serveur local `core/stream.py`
  (HTTP/1.1, requêtes Range 206, keep-alive), pas par `file://`.

## 5. Release

### La chaîne complète en un coup d'œil

```text
Code source (.py)                       [app/*.py]
   │  tests\*.py — vérification avant build
   ▼
1. make_release.ps1
   │  zip des sources + SHA-256 ──► release\ChapeMedia-vX.Y.Z-win64.zip
   │  manifest.json (version, hash, taille, notes, date)
   │  entrée CHANGELOG.md
   ▼
Publication        zip + manifest.json copiés ensemble vers le canal choisi
                    (dossier servi en ligne, partage, clé USB)
   ▼
Mise à jour chez l'utilisateur
   app installée lit le manifest.json à chaque démarrage (≤ 24 h)
   ▸ vérifie SHA-256 après téléchargement
   ▸ remplace les fichiers du programme uniquement (données intactes)
   ▸ rollback automatique si le remplacement échoue
```

## 6. Publier une nouvelle version — procédure complète

1. Modifier le code, tester (voir § 7).
2. Incrémenter `APP_VERSION` (et éventuellement `APP_CHANNEL` : `stable`/`beta`)
   dans **`app/core/config.py`** — c'est le **seul endroit** : elle alimente
   l'interface, les propriétés Windows de l'exe et le nom du paquet.
3. Lancer :

   ```powershell
   powershell -NoProfile -ExecutionPolicy Bypass -File scripts\make_release.ps1 -Build -Notes "..."
   ```

   Le workflow `.github/workflows/release.yml` fait la même chose dans la CI :
   push de `app/core/config.py` (APP_VERSION) → tests + build → tag `vX.Y.Z` +
   Release GitHub → manifest complété et paquet publiés sur `main`.

4. Publier **le zip ET `release\manifest.json`** au même endroit (serveur,
   partage, clé USB). Les apps installées détectent la nouveauté en ≤ 24 h et
   proposent la mise à jour ; rien ne s'installe sans accord de l'utilisateur.
5. Sources du manifeste surchargeables **sans recompiler** : réglage
   `update_url` (base SQLite) ou variable d'environnement
   `CHAPE_MEDIA_UPDATE_URL`.

## 7. Points sensibles à connaître

- **Verrou d'instance unique** : `app.py` bind le port TCP 8907 ; le cache
  WebView2 partagé (`%USERPROFILE%\.chape_media\webview`) n'est activé que si
  ce port est libre, sinon l'app repasse en mode privé temporaire.
- **Ports** : 8928 = API REST FastAPI · 8929 = serveur de streaming (port
  effectif stocké au boot dans le réglage `media_port`) · 8907 = verrou d'instance.
- **Périmètre du serveur de streaming** : il écoute sur `0.0.0.0` (partage réseau),
  mais `/media`, `/share` et `/poster` refusent tout fichier hors de deux racines :
  `%USERPROFILE%\.chape_media\thumbs` et le dossier de téléchargement. Le filtrage
  (`MediaServer.set_roots` / `is_allowed`) résout les liens et `..`, renvoie 404 pour
  un fichier absent et 403 hors périmètre. `Api.poster_url()` ajoute en lecture seule
  le dossier d'une vidéo locale choisie hors médiathèque.
- **Encodage console** : `app.py` reconfigure `sys.stdout/stderr` en UTF-8
  (les titres TikTok/YouTube plantent sinon sous cp1252).
- **`ffmpeg.exe`** : à la racine de `app/` ; `config.py` le localise via
  `sys.frozen`/`sys._MEIPASS` (mode compilé) ou le dossier du projet (sources).
- **Artefacts régénérables** (peuvent être supprimés sans risque) :
    `app/build/`, `app/dist/`, `__pycache__/`, `.pytest_cache/`.
- **Ne PAS supprimer** : `app.js` (bundle front unique) et `bridge.js` (pont
  FastAPI) — ils sont chargés par `index.html`.
- **Sortie de build** : `release/` peut être supprimé sans risque — il est
  entièrement régénéré par `scripts\make_release.ps1`.

## 8. Tests

```powershell
.venv\Scripts\python.exe app\audit_roles.py        # 38 règles d'architecture
.venv\Scripts\python.exe app\smoke_player.py       # streaming (Range, 416, MIME)
.venv\Scripts\python.exe app\smoke_thumbs.py       # vignettes + Chromium
.venv\Scripts\python.exe app\smoke_fixes.py        # correctifs régressifs
.venv\Scripts\python.exe app\smoke_profile.py      # profils / avatars
.venv\Scripts\python.exe test_contract.py          # contrat REST (app sur 8928)
.venv\Scripts\python.exe playwright_smoke.py       # smoke UI (front réel)
```

Aucun pytest, aucune intégration continue : ces scripts sont lancés à la main.
`make_release.ps1 -Build` les exécute avant de zipper.

## 9. Dépannage build

| Symptôme                                 | Cause probable              | Solution                                                       |
| ---------------------------------------- | --------------------------- | -------------------------------------------------------------- |
| Exe lancé mais fenêtre vide              | WebView2 Runtime absent     | Installer le Runtime Edge WebView2                             |
| Le front affiche « Interface non prête » | Serveur FastAPI non démarré | Vérifier que `python app.py` (démarrage auto en thread) tourne |
| Port 8928 déjà utilisé                   | Autre instance FastAPI      | Fermer l'ancien processus python, ou changer `API_PORT`        |
| `TypeError` 500 sur un appel /api/*      | Payload objet vs `__args__` | Voir la logique de dispatch dans `server.py` (route générique) |
