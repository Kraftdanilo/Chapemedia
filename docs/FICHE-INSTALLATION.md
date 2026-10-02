# 📥 Fiche d'installation — CHAPE-MEDIA v1.0.2

Paquet Windows 64 bits prêt à l'emploi : **aucune installation**, aucun compte,
aucun abonnement. Comptez 2 minutes.

---

## 1. Configuration requise

| Élément | Détail |
|---|---|
| Système | Windows 10 ou 11, **64 bits** |
| WebView2 | Microsoft Edge WebView2 Runtime — déjà présent sur tout Windows à jour. Sinon : <https://developer.microsoft.com/microsoft-edge/webview2/> |
| Internet | Obligatoire pour rechercher et télécharger les médias |
| Python | **Non requis** — Python, yt-dlp et ffmpeg sont embarqués dans le paquet |
| Espace disque | ~150 Mo pour le programme + l'espace nécessaire à vos téléchargements |

---

## 2. Télécharger le paquet

Lien direct : **[ChapeMedia-v1.0.2-win64.zip](https://github.com/Kraftdanilo/Chapemedia/raw/main/release/ChapeMedia-v1.0.2-win64.zip)** (environ 55 Mo)

Fiche technique du paquet (aussi dans [`release/manifest.json`](../release/manifest.json)) :

| Valeur | Contenu |
|---|---|
| Version | 1.0.2 (canal `stable`) |
| Taille | 57 273 952 octets |
| SHA-256 | `D37DC8F360DB35D3242992E70C30A15E0A08D6F196D114E2B5FEF592D0392648` |

### Vérifier l'intégrité (facultatif)

Ouvrez PowerShell, placez-vous dans le dossier du zip puis tapez :

```powershell
Get-FileHash .\ChapeMedia-v1.0.2-win64.zip -Algorithm SHA256
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
ChapeMedia-v1.0.2-win64\
├── ChapeMedia.exe      ← l'application
├── _internal\          ← programme + dépendances + ffmpeg.exe
├── LISEZMOI.txt        ← mode d'emploi condensé
├── NOTICE.txt          ← licences des composants inclus
└── LICENSE.txt         ← licence d'utilisation
```

---

## 4. Première mise en route

1. Ouvrez le dossier extrait et double-cliquez sur **`ChapeMedia.exe`**.
2. Si Windows affiche **« Windows a protégé votre PC »** (application non signée) :
   *Plus d'informations* → **Exécuter quand même**.
3. La fenêtre s'ouvre et la barre d'état affiche **`PRÊT. 🌳`** : tout fonctionne.
4. Collez un lien dans le champ et appuyez sur **Entrée** pour un premier essai.

> Si rien ne s'affiche : voir la [fiche de dépannage](FICHE-DEPANNAGE.md)
> (WebView2 manquant).

---

## 5. Mettre à jour

Deux méthodes (détails dans la [fiche de dépannage](FICHE-DEPANNAGE.md) § 2) :

- **Automatique** : bouton `© Dane hk \Manifest` → *Vérifier les mises à jour*.
- **Manuelle** : extraire la nouvelle archive à la place de l'ancienne version.
  Aucune donnée n'est perdue : seuls les fichiers du programme changent.

---

## 6. Désinstaller

1. Fermez l'application.
2. Supprimez le dossier `ChapeMedia-v1.0.2-win64` (rien d'autre n'est installé).
3. Pour tout effacer, supprimez aussi vos données :
   `%USERPROFILE%\.chape_media`
   (historique, médiathèque, favoris, réglages, sauvegardes).
4. Vos téléchargements, eux, restent dans le dossier **« Médias »** de votre
   session (visible dans l'application) : à conserver ou à supprimer selon vos besoins.

---

**Suite** : [Fiche d'utilisation](FICHE-UTILISATION.md) ·
[Retour à la documentation](README.md)
