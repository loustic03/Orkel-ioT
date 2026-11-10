# Publier Orkel-ioT sur GitHub — procédure

Même principe que l'afficheur F1ATB :

| Quoi | Où | Sert à |
|---|---|---|
| Page d'installation (esp-web-tools) | `docs/index.html` → https://loustic03.github.io/Orkel-ioT/ | première installation par câble, depuis le navigateur |
| Fichiers complets de chaque version | `docs/firmware/Vx.y.z/` (+ `manifest.json`, `manifest_s3_8mo.json`) | utilisés par la page d'installation |
| Liste des versions de la page | `docs/versions.json` | menu « Version » de la page |
| Liste des mises à jour | `docs/updates/updates.json` | lue plus tard par Orkel-ioT pour se mettre à jour seul |
| Fichiers de mise à jour (OTA) | **Releases** GitHub, une par version (tag `v1.0.1`) | téléchargement des `.bin` de chaque version |
| Dernière version | dossier `bin/` | téléchargement simple (cité dans les guides) |

Chaque livraison d'Orkel-ioT contient un dossier **`Orkel-ioT`** déjà prêt (tous ces fichiers à jour) :
il n'y a rien à éditer à la main.

---

## A. La première fois (une seule fois) — nouveau dépôt https://github.com/loustic03/Orkel-ioT

> Nom du dépôt : **`Orkel-ioT`**, exactement (O et T majuscules, tiret) : l'adresse de la page d'installation
> (https://loustic03.github.io/Orkel-ioT/) et la mise à jour en un clic respectent les majuscules.

1. **Créer le dépôt** : sur GitHub, **+** (en haut à droite) → **New repository** → nom **`Orkel-ioT`**, **Public**,
   ne rien cocher (pas de README, pas de licence) → **Create repository**.
2. **Envoyer les fichiers** : lien « uploading an existing file » (ou **Add file → Upload files**). Glisser **tout le contenu**
   du dossier `Orkel-ioT` fourni (les dossiers `bin`, `docs` et les fichiers de la racine) → **Commit changes**.
   - Si le navigateur refuse les dossiers : envoyer d'abord les fichiers de la racine, puis glisser chaque dossier entier.
3. **Activer GitHub Pages** : **Settings → Pages** → *Build and deployment* → Source **Deploy from a branch**,
   Branch **`main`**, dossier **`/docs`** → **Save**. Après 1 à 2 minutes : https://loustic03.github.io/Orkel-ioT/
4. **Créer la Release `v1.0.0`** (partie B, étape 1) avec le texte de `RELEASE_v1.0.0.md` et les 6 fichiers de `bin/`.
5. **L'ancien dépôt Orkelys** : une fois tout vérifié, le supprimer (Settings → tout en bas → *Delete this repository*)
   ou l'archiver. Les modules encore en Orkelys ne verront pas Orkel-ioT dans « Vérifier » : il faut leur envoyer
   **une fois** le fichier `OTA_…_Orkel-ioT.ino.bin` (page Système → Mise à jour du firmware) ; ensuite le bouton GitHub marche.

## B. À chaque nouvelle version (exemple : V1.0.1)

> **Ordre important :** les modules regardent `updates.json` chaque jour. Faire **d'abord la Release**
> (les fichiers OTA à télécharger), **ensuite** envoyer les fichiers du dépôt (qui contiennent le nouveau `updates.json`).
> Dans l'autre ordre, un module pourrait annoncer la version avant que son fichier existe (son téléchargement échouerait
> proprement, sans rien abîmer, mais autant l'éviter).

1. **Créer la Release** : page du dépôt → **Releases** (colonne de droite) → **Draft a new release**.
   - **Choose a tag** → taper **`v1.0.1`** (v minuscule, points) → *Create new tag*.
   - **Release title** : `Orkel-ioT V1.0.1`.
   - Description : coller le texte du fichier `RELEASE_v1.0.1.md` fourni (les nouveautés).
   - **Attach binaries** : glisser les **8 fichiers** du dossier `bin/` :
     `OTA_ESP32-classique_Orkel-ioT.ino.bin`, `OTA_ESP32-S3_4Mo_Orkel-ioT.ino.bin`, `OTA_ESP32-S3_8Mo_Orkel-ioT.ino.bin`,
     `Orkel-ioT_ESP32-classique_4Mo.bin`, `Orkel-ioT_ESP32-S3_universel-4Mo.bin`, `Orkel-ioT_ESP32-S3_8Mo-PSRAM.bin`,
     `OTA_JC3248W535_Orkel-ioT.ino.bin`, `Orkel-ioT_JC3248W535.bin` (carte à écran, depuis la V1.1.1).
   - Laisser **Set as the latest release** coché → **Publish release**.
2. **Mettre à jour les fichiers du dépôt** : glisser le contenu du nouveau dossier `Orkel-ioT` fourni
   (**Add file → Upload files**), **Commit changes**. Les fichiers existants sont remplacés, le nouveau dossier
   `docs/firmware/V1.0.1/` est ajouté.
3. Vérifier : https://loustic03.github.io/Orkel-ioT/ (le menu « Version » montre la nouvelle) et
   `https://github.com/loustic03/Orkel-ioT/releases/download/v1.0.1/OTA_ESP32-classique_Orkel-ioT.ino.bin`
   (le navigateur doit télécharger le fichier). Puis, sur un module : Système → « 🔎 Vérifier maintenant »
   (GitHub Pages peut mettre jusqu'à 10 minutes à servir le nouveau `updates.json`).

> ⚠️ Le tag de la Release doit être **exactement** celui écrit dans `docs/updates/updates.json`
> (`"url": ".../releases/download/v1.0.1/"`), sinon les liens de mise à jour ne marcheront pas.

## C. Ménage (de temps en temps)

Chaque version ajoute environ 16 Mo dans `docs/firmware/`. Garder les **3 dernières** :
supprimer les anciens dossiers `docs/firmware/Vx.y.z/` (ouvrir un fichier du dossier → **…** → **Delete file**,
répéter pour chaque fichier) **et** retirer la ligne correspondante de `docs/versions.json`
(les livraisons fournissent `versions.json` déjà à jour). Les Releases, elles, restent toutes disponibles.

## D. Le format de `updates.json` (pour mémoire)

```json
{
  "derniere": "V1.0.1",
  "versions": [
    {
      "version": "V1.0.1",
      "date": "2026-10-14",
      "notes": "…",
      "url": "https://github.com/loustic03/Orkel-ioT/releases/download/v1.0.1/",
      "fichiers": {
        "esp32":       "OTA_ESP32-classique_Orkel-ioT.ino.bin",
        "esp32s3_4mo": "OTA_ESP32-S3_4Mo_Orkel-ioT.ino.bin",
        "esp32s3_8mo": "OTA_ESP32-S3_8Mo_Orkel-ioT.ino.bin",
        "jc3248w535":  "OTA_JC3248W535_Orkel-ioT.ino.bin"
      },
      "md5": {
        "esp32":       "…32 caractères…",
        "esp32s3_4mo": "…",
        "esp32s3_8mo": "…"
      }
    }
  ]
}
```

`md5` (facultatif) : empreinte de chaque fichier OTA ; Orkel-ioT refuse un fichier arrivé abîmé.
La version la plus récente est **en premier**. Adresse de téléchargement = `url` + nom du fichier de la carte.
Attention aux virgules entre deux versions (comme pour l'afficheur) : les livraisons fournissent le fichier complet.
