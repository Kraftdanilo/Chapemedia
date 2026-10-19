# 📥 Fiche d'installation — CHAPE-MEDIA v1.0.5

Paquet Windows 64 bits (exécutable autonome) : aucun compte, aucun abonnement,
aucune installation de Python. Comptez quelques secondes : extraire, double-cliquer.

---

## 1. Configuration requise

| Élément | Détail |
|---|---|
| Système | Windows 10 ou 11, **64 bits** |
| WebView2 | Microsoft Edge WebView2 Runtime — déjà présent sur tout Windows à jour. Sinon : <https://developer.microsoft.com/microsoft-edge/webview2/> |
| Internet | Obligatoire pour rechercher et télécharger les médias |
| Espace disque | ~250 Mo pour l'application + l'espace nécessaire à vos téléchargements |

---

## 2. Télécharger le paquet

Téléchargement direct (dépôt) :
**[ChapeMedia-v1.0.5-win64.zip](https://github.com/Kraftdanilo/Chapemedia/raw/main/release/ChapeMedia-v1.0.5-win64.zip)**

La 1.0.5 est la première version de départ de ce dépôt : les paquets antérieurs
(1.0.0 à 1.0.4) ne sont plus distribués.

Fiche technique du paquet (source : [`release/manifest.json`](../release/manifest.json)) :

| Valeur | Contenu |
|---|---|
| Version | 1.0.5 remix (canal `stable`) |
| Taille | 77397911 octets (~73,8 Mo) |
| SHA-256 | `4876D4370CC9ABAEA9DCB9C20BB4E626FC92EFC1010793463DC22CEF74BAF298` |

### Vérifier l'intégrité (facultatif)

Ouvrez PowerShell, placez-vous dans le dossier du zip puis tapez :

```powershell
Get-FileHash .\ChapeMedia-v1.0.5-win64.zip -Algorithm SHA256
```

L'empreinte affichée doit être **identique** à celle du tableau ci-dessus. En cas
de différence, le fichier est corrompu ou altéré : téléchargez-le à nouveau.

---

## 3. Extraire

1. Faites un clic droit sur l'archive → **Tout extraire…** (ou l'outil de votre choix).
2. Choisissez le dossier de destination : **Documents** ou le **Bureau**.
   > ⚠️ **Ne placez pas l'application dans `C:\Program Files`** : la mise à jour
   > automatique a besoin d'écrire dans le dossier du programme.
3. ⚠️ **Ne lancez jamais l'application depuis l'archive** : extrayez d'abord.

Contenu du paquet :

```
ChapeMedia-v1.0.5-win64\
├── ChapeMedia.exe       ← double-cliquez ici pour lancer
├── _internal\           ← moteur de l'application (ne pas déplacer ni renommer)
│   ├── web\             ← interface
│   └── ffmpeg.exe       ← moteur de conversion (inclus)
├── LISEZMOI.txt         ← mode d'emploi condensé
├── NOTICE.txt           ← licences des composants inclus
└── LICENSE.txt          ← licence d'utilisation
```

> Aucune installation de Python requise : l'application est un exécutable autonome.

---

## 4. Première mise en route

1. Ouvrez le dossier extrait et double-cliquez sur **`ChapeMedia.exe`**.
   L'application démarre directement : aucune installation, aucun Python,
   aucune bibliothèque à télécharger.
2. La fenêtre s'ouvre et la barre d'état affiche **`PRÊT. 🌳`** : tout fonctionne.
3. Collez un lien dans le champ et appuyez sur **Entrée** pour un premier essai.

> Si rien ne s'affiche : voir la [fiche de dépannage](FICHE-DEPANNAGE.md)
> (WebView2 manquant — Python n'est plus concerné).

---

## 5. Mettre à jour

Deux méthodes (détails dans la [fiche de dépannage](FICHE-DEPANNAGE.md) § 2) :

- **Automatique** : bouton `© Dane hk \Manifest` → *Vérifier les mises à jour*.
- **Manuelle** : extraire la nouvelle archive à la place de l'ancienne version.
  Aucune donnée n'est perdue : seuls les fichiers du programme changent.

---

## 6. Désinstaller

1. Fermez l'application.
2. Supprimez le dossier `ChapeMedia-v1.0.5-win64` (rien d'autre n'est installé).
3. Pour tout effacer, supprimez aussi vos données :
   `%USERPROFILE%\.chape_media`
   (historique, médiathèque, favoris, réglages, sauvegardes).
4. Vos téléchargements, eux, restent dans le dossier **« Médias »** de votre
   session (visible dans l'application) : à conserver ou à supprimer selon vos besoins.

---

**Suite** : [Fiche d'utilisation](FICHE-UTILISATION.md) ·
[Retour à la documentation](README.md)
