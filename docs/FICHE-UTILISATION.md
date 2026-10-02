# 🎬 Fiche d'utilisation — CHAPE-MEDIA v1.0.2

Mode d'emploi de l'application : téléchargement, lecture, bibliothèque et
raccourcis. Tout se passe dans **une seule fenêtre**, en deux zones :
**⬇ TÉLÉCHARGEMENT** (haut) et **📦 BIBLIOTHÈQUE** (bas).

---

## 1. Télécharger un média

1. **Collez le lien** dans le champ `COLLE TON LIEN…`
   - 📋 = coller depuis le presse-papiers, 🔍 = prévisualiser (titre + vignette sans télécharger).
   - Sites pris en charge par yt-dlp : YouTube, TikTok, Instagram, X…
2. **Choisissez le type** :
   - 🎥 **VIDÉO** : format `MP4` / `MKV` / `WEBM` / `MOV`, qualité `BEST` (max), `4K`, `1440P`, `1080P`, `720P`, `480P`.
   - 🎵 **AUDIO** : format `MP3` / `M4A` / `OPUS` / `OGG` / `WAV` / `FLAC`, débit `128K`, `192K` (défaut), `256K` ou `320K`.
3. **Destination** : le chemin affiché est le dossier d'arrivée ; bouton **CHANGER** pour le modifier.
4. Cliquez sur **⬇ TÉLÉCHARGEMENT** (ou appuyez sur **Entrée**) :
   - la barre de progression se remplit avec la vitesse en Mo/s ;
   - en fin de course, l'application bascule automatiquement sur l'onglet
     **HISTORIQUE**, vide la recherche, réinitialise les filtres et
     **surligne la nouvelle ligne**.

> Astuce : la pastille **▶ LECTURE INTÉGRÉE** (en haut à droite) active ou coupe
> le lecteur intégré ; le bouton **📂 DOSSIER** ouvre le dossier de destination.

### Badges de l'historique

| Badge | Signification |
|---|---|
| ✅ OK | Le fichier est présent sur le disque |
| ⚠ ERREUR | Le téléchargement a échoué (motif dans la ligne) |
| 🗑 MANQUANT | Le fichier a disparu du disque (déplacé/supprimé) |

---

## 2. Lire un média

- Cliquez sur une ligne de l'historique ou de la médiathèque : le lecteur s'ouvre
  (vidéo `<video>` ou audio `<audio>` selon le format).
- **↗ OUVRIR EXTERNE** : lance le fichier dans le lecteur système.
- **📂 DOSSIER** : le montre dans l'explorateur.
- **✖** ou touche **Échap** : ferme le lecteur.
- Le compteur de lectures est cumulé dans les statistiques (📊).

---

## 3. Les onglets de la bibliothèque

| Onglet | Contenu |
|---|---|
| 🕘 HISTORIQUE | Chaque tentative de téléchargement, avec date, format, état |
| 📚 MÉDIATHÈQUE | Les fichiers **présents** dans le dossier de destination (scan disque) |
| 📋 JOURNAL | Journal technique détaillé (dépannage, activité interne) |

---

## 4. Filtres, recherche et outils

**Filtres de l'historique** (une ligne de pastilles) :

| Filtre | Affiche |
|---|---|
| ☑ TOUS | Tout l'historique |
| 📅 AUJOURD'HUI | Les entrées du jour |
| 🗓 7 JOURS | Les 7 derniers jours |
| ⚠ ERREURS | Les échecs uniquement |
| 🗑 MANQUANTS | Les fichiers disparus du disque |
| NETTOYER | Retire les entrées orphelines (favoris conservés) |

Un compteur à droite des filtres indique le nombre d'entrées affichées.

**Recherche** : champ `🔎 RECHERCHER…` — instantanée, **insensible à la casse et
aux accents** (« cafe » trouve « Café »). Porte sur le titre, le format, etc.

**Barre d'outils** :

| Bouton | Rôle |
|---|---|
| ⭐ FAVORIS | N'affiche que les favoris (bannière + bouton « Tout afficher ») |
| 📊 STATS | Statistiques : lectures, volumes, répartition |
| ↻ ACTUALISER | Re-scanne la médiathèque |
| 🗑 VIDER | Vide l'historique affiché (confirmation demandée) |
| 💾 EXPORT | Exporte l'historique en **JSON** |
| 💾 SAUVEGARDE | Crée une sauvegarde des données (base SQLite) |
| 📂 DONNÉES | Ouvre le dossier `%USERPROFILE%\.chape_media` |
| 🧰 OPTIM | Optimise la base et vérifie son intégrité |

---

## 5. Raccourcis clavier

| Touche | Action |
|---|---|
| `Entrée` (dans le champ lien) | Lance le téléchargement |
| `/` | Place le curseur dans la recherche |
| `Entrée` (dans la recherche) | Lance la recherche |
| `Échap` | Ferme, dans l'ordre : le lecteur → les mentions légales → les stats |

---

## 6. Vos données

Tout est stocké **localement**, dans `%USERPROFILE%\.chape_media` :

```
%USERPROFILE%\.chape_media\
├── chape_media.db      ← historique, médiathèque, favoris, réglages, journal
├── backups\            ← sauvegardes automatiques (8 conservées)
├── cache\              ← cache d'analyse (accélère les recherches suivantes)
├── update\             ← paquets de mise à jour + journal update.log
└── webview\            ← cache d'affichage
```

- **Sauvegarder** = copier ce dossier (ou bouton 💾 SAUVEGARDE).
- **Repartir de zéro** = le supprimer (l'application le recrée au démarrage).
- Les médias téléchargés vivent dans le **dossier de destination** (« Médias »
  par défaut) — indépendants des données de l'application.

> Aucune donnée n'est envoyée sur un serveur : ni compte, ni télémétrie.
> Seule la vérification de mise à jour contacte GitHub (une fois par 24 h).

---

## 7. Mentions légales et mise à jour

Bouton **© MANYFEST** (en bas à droite) : version installée, licence,
et **🔄 Vérifier les mises à jour** → voir la
[fiche de dépannage](FICHE-DEPANNAGE.md) § 2.

---

**Précédent** : [Fiche d'installation](FICHE-INSTALLATION.md) ·
**Suivant** : [Fiche de dépannage](FICHE-DEPANNAGE.md) ·
[Documentation](README.md)
