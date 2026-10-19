﻿# CHAPE-MEDIA

Application de téléchargement de médias (vidéo / audio) pour **Windows** — v1.0.5 remix.

Interface locale, rapide et légère. Aucun compte, aucun abonnement, aucune donnée envoyée à un service tiers : tout reste sur votre machine.

---

## 📥 Téléchargement

### Windows 10 / 11 — 64 bits

[`ChapeMedia-v1.0.5-win64.zip`](https://github.com/Kraftdanilo/Chapemedia/raw/main/release/ChapeMedia-v1.0.5-win64.zip)
— 38,4 Mo, SHA-256 `8459E69910395E7D3A8F91B9FF48DECB6C619CEA21CA0E9761EFDE104A91D9B7`
(vérifiable avec `Get-FileHash`).

### Version actuelle : v1.0.5 remix

Première version de départ du dépôt : les paquets 1.0.0 à 1.0.4 ne sont plus distribués.
Les installations existantes reçoivent la 1.0.5 automatiquement (vérification SHA-256,
retour arrière en cas d'échec).

Cette version corrige une faille : le serveur de lecture (ouvert sur le réseau local
pour le partage de lien) pouvait servir **n'importe quel fichier du disque** via
`/media?path=...`. Désormais, seuls le dossier de téléchargement et les données
multimédia de l'application (vignettes) sont servis. Mise à jour recommandée.

1. Téléchargez l'archive.
2. **Extrayez-la** où vous voulez (ne lancez pas l'application depuis l'archive).
3. Ouvrez le dossier extrait et double-cliquez sur **`lancer.bat`**.

> **« Windows a protégé votre PC » ?**
> L'application n'est pas signée avec un certificat payant. Cliquez sur *Plus d'informations* puis *Exécuter quand même*.

### Configuration requise

- Windows 10 ou 11 (64 bits)
- **Microsoft Edge WebView2 Runtime** — déjà présent sur un Windows à jour. Sinon : [WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/)
- **Python 3.11+** (une seule installation : [python.org](https://www.python.org/downloads/), cochez « Add python.exe to PATH »)
- Une connexion Internet (pour chercher et télécharger)
- `ffmpeg.exe` est **inclus** dans le paquet (rien à installer)

---

## 📚 Documentation

| Fiche                                                      | Contenu                                                                                    |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| [`docs/FICHE-INSTALLATION.md`](docs/FICHE-INSTALLATION.md) | Configuration requise, téléchargement, extraction, première mise en route, désinstallation |
| [`docs/FICHE-UTILISATION.md`](docs/FICHE-UTILISATION.md)   | Interface, téléchargement vidéo/audio, lecteur, bibliothèque, raccourcis, données          |
| [`docs/FICHE-DEPANNAGE.md`](docs/FICHE-DEPANNAGE.md)       | Messages d'erreur, mise à jour, réinitialisation, contact                                  |
| [`docs/DEVELOPPEUR.md`](docs/DEVELOPPEUR.md)               | Architecture du code, lancement, release, tests (réservé aux développeurs)  |

Sommaire : [`docs/README.md`](docs/README.md)

---

## ✨ Fonctionnalités

- **Vidéo** — MP4, MKV, WEBM, MOV, ainsi que AVI/M4V détectés automatiquement
- **Audio** — MP3, M4A, OPUS, OGG, WAV, FLAC
- **Sans son (optionnel)** — au téléchargement ou plus tard depuis le lecteur : la copie muette est créée à côté de l'original, jamais de modification destructive
- **Profil utilisateur** — nom et photo personnalisés, affichés dans le bandeau (stockés localement)
- **Édition « remix »** — couche UI non destructive : bandeau v1.0.5 remix, toasts fixes, modales centrées, responsive 960/560 px, lien « aller au contenu » — aucun identifiant renommé, aucune API cassée
- **Lecture en boucle** — enchaînement automatique d'un média au suivant (boucle persistante de la bibliothèque)
- Lecteur intégré, avec compteur de lectures
- Historique, médiathèque, favoris, statistiques et journal (badges OK / ERREUR / MANQUANT)
- Recherche instantanée, insensible aux accents et à la casse
- Export de l'historique en JSON, sauvegarde de la base SQLite (8 conservées)
- **Moteurs de secours** : si yt-dlp échoue (site qui a changé, captcha…), l'app réessaie automatiquement avec Cobalt puis gallery-dl

## ⌨️ Raccourcis

| Touche   | Action                                                                     |
| -------- | -------------------------------------------------------------------------- |
| `Entrée` | Télécharger (champ lien) / lancer la recherche (champ recherche)           |
| `/`      | Place le curseur dans la recherche                                          |
| `Échap`  | Ferme, dans l'ordre : le lecteur → les mentions légales → les statistiques |

## 📁 Vos données

Historique, médiathèque, favoris et réglages sont enregistrés sur votre ordinateur, dans :

```text
%USERPROFILE%\.chape_media
```

Structure : `chape_media.db` (SQLite, base unique), `backups\` (sauvegardes automatiques), `cache\`, `thumbs\` (vignettes locales), `update\` (paquets + journal), `webview\` (cache d'affichage).

- **Sauvegarder** = copier ce dossier (ou bouton 💾 SAUVEGARDE dans l'application)
- **Repartir de zéro** = supprimer ce dossier (l'application le recrée au démarrage)

Les téléchargements vont par défaut dans `~\Downloads\CHAPE-MEDIA`, dossier modifiable dans l'interface (bouton CHANGER).

## 🔄 Mises à jour

L'application vérifie les mises à jour **à chaque démarrage** (une fois par 24 h). Si une version plus récente est publiée dans `manifest.json`, elle est proposée et reste à installer manuellement : vous gardez le contrôle. Une fois téléchargée, elle est **vérifiée (SHA-256)** avant d'être installée — une empreinte différente détruit le paquet.

Si le remplacement échoue, l'ancienne version est automatiquement remise en place (retour arrière). **Vos données ne sont jamais touchées** : seuls les fichiers du programme changent.

> L'application doit être placée dans un dossier où vous pouvez écrire (Documents, Bureau…), **pas dans `C:\Program Files`**.

---

## ⚖️ Licence et usage

Licence **libre d'utilisation** : vous pouvez utiliser l'application librement, gratuitement et sans limitation.

**Auteur** : **Dane hk \Manifest**

Cette licence **n'autorise pas la copie, la modification, la redistribution ou la revente** du projet, ni la réutilisation de son code sous quelque forme que ce soit. Voir [`LICENSE`](LICENSE).

**Windows uniquement.** Aucune version macOS, Linux, iOS ou Android n'est prévue.

## ⚠️ Responsabilité

Cette application a été créée pour un **besoin personnel** et comme **défi technique**. Elle est fournie *telle quelle*, sans garantie d'aucune sorte.

L'utilisateur est **seul responsable de son utilisation** et de ce qu'il télécharge. Respectez les conditions d'utilisation des plateformes consultées et la législation en vigueur dans votre pays. L'auteur décline toute responsabilité quant à l'usage que vous faites de l'application et du contenu que vous obtenez.

L'application n'est affiliated à aucun service de streaming ni à aucune plateforme. N'utilisez pas son nom pour désigner un produit officiel.

## 📨 Problèmes

Toute question ou tout signalement : via les [issues du dépôt](https://github.com/Kraftdanilo/Chapemedia/issues).

---

**CHAPE-MEDIA** — disponible pour Windows uniquement.
