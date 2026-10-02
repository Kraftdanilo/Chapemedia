# 🛠 Fiche de dépannage — CHAPE-MEDIA v1.0.2

Problèmes fréquents, messages d'erreur et procédures de mise à jour.
Contact en fin de fiche.

---

## 1. Problèmes courants

### « Windows a protégé votre PC »
Application non signée par un certificat commercial : *Plus d'informations* →
**Exécuter quand même**. (Voir aussi la [fiche d'installation](FICHE-INSTALLATION.md) § 4.)

### L'application ne démarre pas / fenêtre vide
1. Vérifiez la présence du **Microsoft Edge WebView2 Runtime** (indispensable) :
   <https://developer.microsoft.com/microsoft-edge/webview2/>
2. Vérifiez que vous bien **extrait** le zip (ne jamais lancer depuis l'archive) et
   que le dossier n'est **pas** dans `C:\Program Files`.
3. Démarrez `ChapeMedia.exe` une seconde fois : si rien ne bouge, redémarrez la
   machine (mémoire tampon WebView2 en mémoire).

### « Licence expiree (30/12/2026) » dans la console
L'application refuse de démarrer car sa date limite est atteinte.
Réinstallez la version la plus récente (voir § 2) ou contactez l'auteur.

### Un téléchargement échoue (badge ⚠ ERREUR)
Relisez le motif affiché dans la ligne de l'historique, puis :

| Motif probable | Que faire |
|---|---|
| Lien invalide / privé / supprimé | Revérifiez l'URL copiée |
| Lien expiré (YouTube) | Relancez : le lien régénéré à chaque lecture |
| Site bloquant / captcha | Réessayez plus tard, autre réseau |
| Format demandé absent | Choisissez `BEST` ou une qualité inférieure |
| Espace disque insuffisant | Libérez de la place dans le dossier de destination |

Après un échec, l'application **réessaie automatiquement** une fois ; si elle
échoue encore, l'erreur reste visible dans l'historique (filtre ⚠ ERREURS).

### Une ligne affiche 🗑 MANQUANT
Le fichier enregistré n'est plus sur le disque (déplacé, renommé, supprimé).
- Filtre **🗑 MANQUANTS** pour les voir toutes ;
- bouton **NETTOYER** pour retirer les entrées orphelines (favoris conservés).

### Lentude / animation lourde
- Fermez les onglets/autres applications ; le fond animé ralentit
  automatiquement quand la fenêtre est inactive ;
- en cas de besoin, ajoutez `?eco=1` à l'URL pour couper les polices distantes.

### Rien ne se télécharge, aucun message
Ouvrez l'onglet **📋 JOURNAL** : le motif y est écrit. Vérifiez aussi votre
connexion et votre antivirus (le téléchargement est légitime : fichier média).

---

## 2. Mises à jour

L'application vérifie automatiquement la dernière version publiée (1×/24 h) et
vous **informe** seulement : rien ne s'installe jamais sans votre accord.

### Méthode automatique (recommandée)

1. Bouton **© Dane hk \Manifest** (en bas à droite).
2. **🔄 Vérifier les mises à jour**.
3. Si une version plus récente existe : **⬇️ Installer la mise à jour**
   (cliquez dessus — deux clics si la première passe inaperçue).
4. Le paquet est téléchargé puis **vérifié (SHA-256)** : toute empreinte
   différente détruit le fichier (`Empreinte SHA-256 differente : paquet abandonne`).
5. L'application se ferme, se remplace toute seule et **redémarre**.
6. En cas d'échec du remplacement, **l'ancienne version est automatiquement
   remise en place** (retour arrière).

Journal détaillé : `%USERPROFILE%\.chape_media\update\update.log`.
**Vos données ne sont jamais touchées** : seul le dossier programme change.

### Méthode manuelle

1. Téléchargez la nouvelle archive (voir [installation](FICHE-INSTALLATION.md) § 2).
2. Extrayez-la **au même endroit que l'ancienne version**.
3. Remplacez le dossier de l'application (en fermant l'app d'abord).
4. Aucune donnée n'est perdue.

> L'application doit se trouver dans un dossier où vous pouvez écrire
> (Documents, Bureau…), **pas dans `C:\Program Files`**.

### Messages d'erreur de mise à jour

| Message | Cause | Solution |
|---|---|---|
| `Manifeste introuvable (404)` | Aucun fichier `manifest.json` à l'adresse configurée | Attendez une publication ou corrigez la source (`update_url`) |
| `Acces refuse (401/403)` | Source privée / authentification requise | Utilisez la source publique |
| `Trop de requetes (429)` | GitHub limite les appels | Réessayez dans quelques minutes |
| `Le serveur ... ne repond pas (delai depasse)` | Connexion ou serveur indisponible | Vérifiez Internet, réessayez |
| `Empreinte SHA-256 differente` | Paquet tronqué ou falsifié | Il est rejeté : réessayez plus tard |

---

## 3. Sauvegarde et réinitialisation

| Objectif | Manipulation |
|---|---|
| Sauvegarder | 💾 SAUVEGARDE (dans l'app) ou copier `%USERPROFILE%\.chape_media` |
| Restaurer | Remettre ce dossier à sa place (app fermée) |
| Réinitialiser | Supprimer `%USERPROFILE%\.chape_media` (app fermée) — l'application le recrée |
| Réparer une base lente | 🧰 OPTIM (optimisation + vérification d'intégrité) |

Les médias téléchargés ne sont **pas** dans ce dossier : ils restent dans le
dossier de destination (« Médias »), même après une réinitialisation.

---

## 4. Signaler un problème

Via les [issues du dépôt](https://github.com/Kraftdanilo/Chapemedia/issues) (même contact que dans le README et la licence).

En cas de signalement, joignez si possible :
1. la version (`© Dane hk \Manifest` → Version) ;
2. les lignes du **📋 JOURNAL** concernées ;
3. le message exact (copié-collé) et les étapes pour reproduire.

---

**Précédent** : [Fiche d'utilisation](FICHE-UTILISATION.md) ·
[Documentation](README.md)
