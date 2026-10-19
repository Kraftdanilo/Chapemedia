# Journal des versions - CHAPE-MEDIA

## v1.0.5 remix - 2026-10-19

Première version distribuée du dépôt. Le paquet de référence est
`release/ChapeMedia-v1.0.5-win64.zip`, construit par `scripts/make_release.ps1` ;
les paquets 1.0.0 à 1.0.4 ne sont plus distribués.

- **Sécurité (correctif majeur)** : le serveur de médias écoute sur 0.0.0.0 (partage réseau) mais servait n'importe quel fichier du disque via `/media?path=`, `/share?path=` et `/poster?path=`. Ces trois points d'entrée sont désormais limités à `APP_DATA/thumbs` et au dossier de téléchargement (sous-dossiers compris, liens et `..` résolus). La base `chape_media.db` n'est plus accessible.
- `/poster` accepte encore une vidéo choisie hors médiathèque : son dossier est autorisé en lecture seule (`Api.poster_url`, nouvelle méthode REST).
- Code retour : 404 pour un fichier absent, 403 hors des dossiers autorisés ; le comportement pour un chemin légitime ne change pas.
- Version : `APP_VERSION` passe à 1.0.5 ; `server.py` lit désormais la version depuis `core/config.py` au lieu de la coder en dur.
- Documentation : ports corrigés (API 8928, streaming 8929, verrou d'instance 8907) dans la doc développeur, la fiche d'installation et les scripts de test.
- Tests : `smoke_player.py` déclare ses racines autorisées ; `smoke_thumbs.py` obtient l'URL `/poster` via l'API. 73 vérifications vertes, audit 38/38.
- Paquet Windows : `release/ChapeMedia-v1.0.5-win64.zip` — 39 011 868 octets, SHA-256 `8459E69910395E7D3A8F91B9FF48DECB6C619CEA21CA0E9761EFDE104A91D9B7`. Démarré et testé depuis le zip : API en `1.0.5 remix`, `/media`, `/share` et `/poster` refusent tout fichier hors des dossiers autorisés (403).
