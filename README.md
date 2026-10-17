<p align="center"><img src="docs/orkel-iot-banniere.png" width="100%" alt="Orkel-ioT — L'orchestration énergétique"></p>

**Orkel-ioT** est un module ESP32 qui pilote le routeur solaire **F1ATB** (ballon d'eau chaude) en tenant compte de la
**batterie** (Zendure, Izypower, autre marque ou aucune), de la **production photovoltaïque**, de la **météo**, du
**tarif** (Base, heures creuses, Tempo) et de vos **règles** (calendrier, températures, anti-légionelle, vacances…).
Il fonctionne seul, sans domotique, et sait aussi s'intégrer à une domotique par MQTT.

- Orkel-ioT s'appelait auparavant **Orkelys** (même programme, nouveau nom ; numérotation repartie à V1.0.0).
- Pour le moment, Orkel-ioT est un projet en développement. Il est utilisé avec un SolarFlow 2400 AC+ et une batterie Zendure ou Izypower.
- Il sert à optimiser l'usage d'une batterie par rapport au routeur F1ATB, pour ceux qui n'ont pas de domotique.
- Il envoie ses informations à l'[afficheur-routeur](https://github.com/loustic03/Afficheur-routeur) (avec un écran « déporté Orkel-ioT » à poser à côté de la batterie).
- Un **simulateur** fonctionne sur les mesures réelles, pour tester les réglages sans rien commander.

<p align="center"><img src="docs/apercu_accueil.png" width="480" alt="Page d'accueil d'Orkel-ioT"></p>

Version actuelle : **V1.0.0** · Auteur : [loustic03](https://github.com/loustic03) (je suis aussi l'auteur de l'afficheur F1ATB) — voir [CHANGELOG.md](CHANGELOG.md).

## 📖 Guide

- Installer et mettre à jour le programme (fichiers .bin), pas à pas : [Orkel-ioT_installer_mettre_a_jour.pdf](Orkel-ioT_installer_mettre_a_jour.pdf)
- Batterie Izypower et ballon d'eau chaude, pas à pas : [Orkel-ioT_guide_Izypower_ballon.pdf](Orkel-ioT_guide_Izypower_ballon.pdf)

Tout le paramétrage, page par page : **[Orkel-ioT_guide_parametrage.pdf](Orkel-ioT_guide_parametrage.pdf)**

## ⚡ Installer (première fois)

### Le plus simple : depuis le navigateur
Ouvrir la **page d'installation** (Chrome ou Edge sur ordinateur), brancher l'ESP32 en USB, cliquer sur « Installer » :
👉 **https://loustic03.github.io/Orkel-ioT/**

### Ou avec le fichier .bin
Dans le dossier [`bin`](bin), prendre le fichier de **votre carte** et l'écrire à l'adresse **0x0** avec
https://espressif.github.io/esptool-js/ :

| Carte | Fichier |
|---|---|
| ESP32 classique (4 Mo) | `Orkel-ioT_ESP32-classique_4Mo.bin` |
| ESP32-S3 (4 Mo ou plus, sans PSRAM) | `Orkel-ioT_ESP32-S3_universel-4Mo.bin` |
| ESP32-S3 8 Mo avec PSRAM | `Orkel-ioT_ESP32-S3_8Mo-PSRAM.bin` |

### Premier démarrage
1. Se connecter au Wi-Fi **Orkel-ioT-Setup**, ouvrir **http://192.168.4.1/**.
2. Page **Système → Wi-Fi** : nom et mot de passe de la box (2,4 GHz), **Enregistrer**. Orkel-ioT affiche sa nouvelle adresse.
3. Rebrancher le téléphone sur la box et ouvrir cette adresse (ou **http://orkel-iot.local/**).
4. Le module neuf démarre en **mode simulation** (bandeau orange) : il ne commande rien tant que vous ne l'avez pas
   désactivé. Suivre le guide.

## 🔄 Mettre à jour (sans câble)

**Le plus simple :** page **Système** d'Orkel-ioT → cadre « 🌐 Mise à jour depuis GitHub » →
**Vérifier maintenant**, puis **Installer la nouvelle version** (avec le mot de passe de mise à jour). Orkel-ioT télécharge
lui-même le bon fichier et redémarre. Il n'installe jamais rien sans toi.

**Ou avec le fichier :** Les fichiers de mise à jour sont dans les **[Releases](https://github.com/loustic03/Orkel-ioT/releases)** (une par version)
et dans le dossier [`bin`](bin) (dernière version). Page **Système → Mise à jour du firmware** d'Orkel-ioT : envoyer le
fichier **`OTA_…_Orkel-ioT.ino.bin`** indiqué dans le cadre vert (jamais le gros fichier de première installation).
Les réglages sont conservés.

| Carte | Fichier de mise à jour |
|---|---|
| ESP32 classique | `OTA_ESP32-classique_Orkel-ioT.ino.bin` |
| ESP32-S3 4 Mo (ou installé depuis la page internet) | `OTA_ESP32-S3_4Mo_Orkel-ioT.ino.bin` |
| ESP32-S3 8 Mo PSRAM | `OTA_ESP32-S3_8Mo_Orkel-ioT.ino.bin` |

## ⚠️ Sécurité

Orkel-ioT commande le F1ATB par des forçages temporaires : s'il s'arrête, le F1ATB reprend seul son fonctionnement
normal au bout de 60 minutes au plus. Une sécurité de température (80 °C d'origine) coupe toute chauffe.
Utilisation sous votre responsabilité ; les installations électriques doivent rester conformes.

## © Licence

© 2026 Orkel-ioT — tous droits réservés. Voir [LICENCE](LICENCE).

---

### Pour le propriétaire du dépôt
La procédure de publication d'une nouvelle version est dans [`docs/updates/LISEZMOI.md`](docs/updates/LISEZMOI.md).
