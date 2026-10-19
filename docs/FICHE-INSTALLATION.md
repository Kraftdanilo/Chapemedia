# 📥 Fiche d'installation — CHAPE-MEDIA v1.0.4

Paquet Windows 64 bits (sources + dépendances) : aucun compte, aucun abonnement.
Comptez 5 minutes la première fois (installation de Python et des bibliothèques),
puis quelques secondes aux lancements suivants.

---

## 1. Configuration requise

| Élément | Détail |
|---|---|
| Système | Windows 10 ou 11, **64 bits** |
| WebView2 | Microsoft Edge WebView2 Runtime — déjà présent sur tout Windows à jour. Sinon : <https://developer.microsoft.com/microsoft-edge/webview2/> |
| Internet | Obligatoire pour rechercher et télécharger les médias |
| Python | **Python 3.11+ requis** (une seule installation : <https://www.python.org/downloads/>, cochez « Add python.exe to PATH ») |
| Espace disque | ~300 Mo pour Python + les bibliothèques + l'espace nécessaire à vos téléchargements |

---

## 2. Télécharger le paquet

Lien direct : **[ChapeMedia-v1.0.4-win64.zip](https://github.com/Kraftdanilo/Chapemedia/raw/main/release/ChapeMedia-v1.0.4-win64.zip)** (environ 39 Mo)

Fiche technique du paquet (aussi dans [`release/manifest.json`](../release/manifest.json)) :

| Valeur | Contenu |
|---|---|
| Version | 1.0.4 remix (canal `stable`) |
| Taille | 38 999 752 octets (~39 Mo) |
| SHA-256 | `5CA1D2207F87AD599DBB9E2B239247E48D08E96F3DC20C83E49039C75ED2D334` |

### Vérifier l'intégrité (facultatif)

Ouvrez PowerShell, placez-vous dans le dossier du zip puis tapez :

```powershell
Get-FileHash .\ChapeMedia-v1.0.4-win64.zip -Algorithm SHA256
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
ChapeMedia-v1.0.4-win64\
├── lancer.bat           ← double-cliquez ici pour lancer
├── app.py               ← application (Python)
├── server.py            ← serveur FastAPI (API REST locale, port 8928)
├── core\, web\          ← code + interface
├── ffmpeg.exe           ← moteur de conversion (inclus)
├── requirements.txt     ← bibliothèques installées au 1er lancement
├── LISEZMOI.txt         ← mode d'emploi condensé
├── NOTICE.txt           ← licences des composants inclus
└── LICENSE.txt          ← licence d'utilisation
```

---

## 4. Première mise en route

1. Ouvrez le dossier extrait et double-cliquez sur **`lancer.bat`**.
   Au premier lancement, il crée l'environnement Python et installe les
   bibliothèques automatiquement (fenêtre noire quelques minutes : c'est normal).
2. La fenêtre s'ouvre et la barre d'état affiche **`PRÊT. 🌳`** : tout fonctionne.
3. Collez un lien dans le champ et appuyez sur **Entrée** pour un premier essai.

> Si rien ne s'affiche : voir la [fiche de dépannage](FICHE-DEPANNAGE.md)
> (Python manquant ou WebView2 manquant).

---

## 5. Mettre à jour

Deux méthodes (détails dans la [fiche de dépannage](FICHE-DEPANNAGE.md) § 2) :

- **Automatique** : bouton `© Dane hk \Manifest` → *Vérifier les mises à jour*.
- **Manuelle** : extraire la nouvelle archive à la place de l'ancienne version.
  Aucune donnée n'est perdue : seuls les fichiers du programme changent.

---

## 6. Désinstaller

1. Fermez l'application.
2. Supprimez le dossier `ChapeMedia-v1.0.4-win64` (rien d'autre n'est installé).
3. Pour tout effacer, supprimez aussi vos données :
   `%USERPROFILE%\.chape_media`
   (historique, médiathèque, favoris, réglages, sauvegardes).
4. Vos téléchargements, eux, restent dans le dossier **« Médias »** de votre
   session (visible dans l'application) : à conserver ou à supprimer selon vos besoins.

---

**Suite** : [Fiche d'utilisation](FICHE-UTILISATION.md) ·
[Retour à la documentation](README.md)
