# CHAPE-MEDIA

Application de téléchargement de médias (vidéo / audio) pour **Windows**.

Interface locale, rapide et légère. Aucun compte, aucun abonnement, aucune donnée envoyée à un service tiers : tout reste sur votre machine.

---

## 📥 Téléchargement

**Windows 10 / 11 — 64 bits**

| Fichier | Description |
|---|---|
| [`ChapeMedia-v1.0.2-win64.zip`](https://github.com/Kraftdanilo/Chapemedia/releases) | Paquet prêt à l'emploi |

1. Téléchargez l'archive.
2. **Extrayez-la** où vous voulez (ne lancez pas l'application depuis l'archive).
3. Ouvrez le dossier extrait et double-cliquez sur **`ChapeMedia.exe`**.

> **« Windows a protégé votre PC » ?**
> L'application n'est pas signée avec un certificat payant. Cliquez sur *Plus d'informations* puis *Exécuter quand même*.

### Configuration requise

- Windows 10 ou 11 (64 bits)
- **Microsoft Edge WebView2 Runtime** — déjà présent sur un Windows à jour. Sinon : https://developer.microsoft.com/microsoft-edge/webview2/
- Une connexion Internet (pour chercher et télécharger)
- Rien d'autre à installer : **Python n'est pas nécessaire**, tous les composants sont inclus.

---

## ✨ Fonctionnalités

- **Vidéo** — MP4, MKV, WEBM, MOV jusqu'en 4K
- **Audio** — MP3, M4A, OPUS, OGG, WAV, FLAC
- Lecteur intégré, avec compteur de lectures
- Historique, médiathèque, favoris, statistiques et journal
- Recherche instantanée, insensible aux accents et à la casse
- Export des données en JSON

## ⌨️ Raccourcis

| Touche | Action |
|---|---|
| `Entrée` | Télécharger |
| `/` | Rechercher |
| `Échap` | Fermer |

## 📁 Vos données

Historique, médiathèque, favoris et réglages sont enregistrés sur votre ordinateur, dans :

```
%USERPROFILE%\.chape_media
```

- **Sauvegarder** = copier ce dossier
- **Repartir de zéro** = supprimer ce dossier

Les téléchargements vont dans le dossier « Médias » de votre session, modifiable dans *Réglages*.

## 🔄 Mises à jour

L'application vérifie les mises à jour toute seule. Si une version plus récente est publiée, elle est téléchargée puis **vérifiée** avant d'être installée.

Si le remplacement échoue, l'ancienne version est automatiquement remise en place. **Vos données ne sont jamais touchées** : seuls les fichiers du programme changent.

> L'application doit être placée dans un dossier où vous pouvez écrire (Documents, Bureau…), **pas dans `C:\Program Files`**.

---

## ⚖️ Licence et usage

Licence **libre d'utilisation** : vous pouvez utiliser l'application librement, gratuitement et sans limitation.

Cette licence **n'autorise pas la copie, la modification, la redistribution ou la revente** du projet, ni la réutilisation de son code sous quelque forme que ce soit. Voir [`LICENSE`](LICENSE).

**Windows uniquement.** Aucune version macOS, Linux, iOS ou Android n'est prévue.

## ⚠️ Responsabilité

Cette application a été créée pour un **besoin personnel** et comme **défi technique**. Elle est fournie *telle quelle*, sans garantie d'aucune sorte.

L'utilisateur est **seul responsable de son utilisation** et de ce qu'il télécharge. Respectez les conditions d'utilisation des plateformes consultées et la législation en vigueur dans votre pays. L'auteur décline toute responsabilité quant à l'usage que vous faites de l'application et du contenu que vous obtenez.

L'application n'est affiliated à aucun service de streaming ni à aucune plateforme. N'utilisez pas son nom pour désigner un produit officiel.

## 📨 Problèmes

Toute question ou tout signalement : **battiment64@gmail.com**

---

<div align="center"><b>CHAPE-MEDIA</b> — disponible pour Windows uniquement.</div>

