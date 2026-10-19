# Journal des versions - CHAPE-MEDIA

## v1.0.5 remix - 2026-10-19

- **Securite (correctif majeur)** : le serveur de medias ecoute sur 0.0.0.0 (partage reseau) mais servait n'importe quel fichier du disque via `/media?path=`, `/share?path=` et `/poster?path=`. Ces trois points d'entree sont desormais limites a `APP_DATA/thumbs` et au dossier de telechargement (sous-dossiers compris, liens et `..` resolus). La base `chape_media.db` n'est plus accessible.
- `/poster` accepte encore une video choisie hors mediatheque : son dossier est autorise en lecture seule (`Api.poster_url`, nouvelle methode REST).
- Code retour : 404 pour un fichier absent, 403 hors des dossiers autorises (le comportement d'un chemin legitime ne change pas).
- Version : `APP_VERSION` passe a 1.0.5 ; `server.py` lit desormais la version depuis `core/config.py` au lieu de la coder en dur.
- Documentation : ports corriges (API 8928, streaming 8929, verrou d'instance 8907) dans la doc developpeur, la fiche d'installation et les scripts de test.
- Tests : `smoke_player.py` declare ses racines autorisees ; `smoke_thumbs.py` obtient l'URL `/poster` via l'API. 73 verifications vertes, audit 38/38.


## v1.0.0 - 2026-09-29

- Paquet Windows 64 bits (SHA-256 : 3E23E4A4C78A41147A203884EF39BF5CCDD842B6FEEB43B17F7D5D5877F72772)

## v1.0.1 - 2026-09-29

- Historique fiable : apres chaque telechargement l'app revient sur l'onglet Historique, vide la recherche, reinitialise les filtres et surligne la nouvelle ligne. Nouveautes : filtres avances (Tous, 24 h, 7 jours, Erreurs, Manquants) avec compteur, badges OK / ERREUR / MANQUANT sur chaque ligne, bouton Nettoyer les entrees orphelines. Corrections : plus aucun echec silencieux (message affiche + nouvelle tentative automatique), entree marquee MANQUANT quand le fichier a disparu du disque, nettoyage de l'historique refuse tant que le scan du dossier est incomplet.

## v1.0.2 - 2026-10-02

- Optimisation memoire et stabilite ; correction des noms de fichiers trop longs pour Windows ; messages de mise a jour explicites. Auteur : Dane hk \Manifest.

## v1.0.3 - 2026-10-14

- Nouveau : suppression du son d'une video - option Sans son au telechargement et bouton Muet.

## v1.0.4 remix - 2026-10-15

- Meme version technique 1.0.4, edition d'affichage "remix" : badge v1.0.4 remix dans le bandeau + mentions legales (display_version via get_version).
- UI/UX : couche non-destructive remix.css (topbar sticky, grille stable, cartes flex, lecteur borne, responsive 960/560px, lien "aller au contenu", toasts fixes, modales centrees) ; remix.js (badge dynamique, boucle persistante d'un media au suivant via window.DNA_LOOP, avatars file:/// + repli, port media effectif stocke au boot).
- Correctif boucle : app4.js initialise loopOn depuis window.DNA_LOOP et le republic a chaque toggle (openPlayer reappliquait deja window.DNA_LOOP).
- Aucun id renomme, aucune API cassee : check_ids.js reste vert (65 ids, 35 appels api).

## v1.0.4 - 2026-10-16

- Backend : pont pywebview remplacé par une API REST locale FastAPI (POST/GET /api/<nom>, port 8898) ; le front communique via bridge.js (proxy window.backend -> fetch HTTP, healthcheck + événement backendready).
- Release publiée : ChapeMedia-v1.0.4-win64.zip (38 999 752 octets, SHA-256 5CA1D2207F87AD599DBB9E2B239247E48D08E96F3DC20C83E49039C75ED2D334).
- Documentation réécrite à l'état actuel de l'application (README, fiches installation/utilisation/dépannage, doc développeur).
