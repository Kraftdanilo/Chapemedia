# Journal des versions - CHAPE-MEDIA

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

- v1.0.4 remix : UI remix (topbar sticky, cartes flex, lecteur borne, responsive), boucle persistante window.DNA_LOOP, correctif boucle app4, badge v1.0.4 remix

