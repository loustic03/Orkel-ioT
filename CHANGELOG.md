# Versions

## V1.3.0
- **Nouvelle page « 💶 Tarifs & économies »** (tuile sur l'accueil). Le bloc « Tarif d'électricité » y est déplacé depuis la page Automatisation.
- **Prix du kWh** selon le contrat : Base (1 prix), heures pleines / creuses (2 prix), Tempo (6 prix : bleu, blanc, rouge × HP / HC), plus le prix de revente du surplus (0 si pas de vente). Prix du kWh seul, sans l'abonnement.
- **Heures creuses : qui donne l'heure ?** Au choix : les plages horaires (comme avant), **le compteur Linky** (libellé tarif lu par le F1ATB quand il mesure sur le Linky ; la couleur Tempo du compteur prime sur celle de RTE) ou **la domotique** (topic MQTT qui publie HC / HP, ou on / off). Sans information du compteur depuis 15 min : retour aux plages. Sert aussi aux décisions (chauffe en heures creuses).
- **Bilan en euros, jour par jour**, calculé toutes les 5 s avec le prix du moment : dépense sur le réseau (et part soutirée en heures creuses), gain du solaire (achat évité), gain de la batterie (ce qu'elle rend, moins ce que sa charge a coûté : charge depuis le réseau au prix du moment, surplus stocké au prix de revente), revente, valeur de l'eau chaude solaire, économie totale. Aujourd'hui, le mois en cours et un tableau sur 62 jours ; aussi dans le fichier tableur et dans le **rapport du mois** (tuiles « En euros » et colonnes Dépense / Économie).
- Pages plus étroites sur téléphone : plus de défilement de côté dans les pages Bilan et Automatisation.

## V1.2.1
- **Correctif** : sur une carte compilée avec un schéma de partitions sans zone « spiffs » (par exemple « 16M Flash 3MB APP/9.9MB FATFS »), les réglages étaient effacés au redémarrage suivant. Ils sont maintenant gardés, et le journal signale l'absence de système de fichiers (le bilan et la journée ne peuvent alors pas être conservés : utiliser « 8M with spiffs » ou la version « JC3248W535 » de la page d'installation).

## V1.2.0
- **Rapport du mois** (page 📊 Bilan → « 📄 Rapport du mois ») : une page claire mise en page pour une feuille A4, avec le résumé du mois (production, consommation, autoconsommation, autonomie, ballon solaire / réseau, réseau, batterie), un graphique jour par jour, le tableau détaillé et les incidents. Bouton « 🖨️ Imprimer / Enregistrer en PDF ». Disponible sur toutes les cartes pour les 62 derniers jours.
- **Bilan plus complet** : énergie chargée et déchargée par la batterie, coupures du Wi-Fi, redémarrages et minutes sans mesures du F1ATB, jour par jour (aussi dans le fichier tableur). Les bilans déjà enregistrés sont repris.
- **Carte microSD de la JC3248W535** (seulement si une carte est montée au démarrage) : chaque journée détaillée y est archivée à minuit, le bilan de chaque jour aussi, et le **rapport du mois y est écrit tout seul le 1er du mois suivant**. Liste des rapports et des journées archivées dans la page Bilan (ouvrir, lire, télécharger), sans limite de durée.

## V1.1.1
- **Programmateur de l'écran** (carte JC3248W535) : jusqu'à 4 plages (jours, heure de début et de fin, page affichée : Flux, Batterie ou État). En dehors des plages l'écran est éteint ; un appui le rallume 1 minute. Remplace le réglage « nuit » de la V1.1.0.
- **Version dédiée JC3248W535** dans la page d'installation et dans les Releases (`Orkel-ioT_JC3248W535.bin`, mise à jour `OTA_JC3248W535_Orkel-ioT.ino.bin`) : écran démarré d'office, journal aussi sur le port USB.

## V1.1.0
- **Écran intégré pour la carte JC3248W535** (ESP32-S3, écran 3,5" tactile 480 × 320) : Orkel-ioT pilote et affiche dans un seul boîtier. Trois écrans : **Flux** (solaire, réseau, maison, batterie, ballon, 2e charge, points animés dans le sens réel), **Batterie** (grand anneau du SOC, charge / décharge) et **État** (décision et sa raison, Wi-Fi, MQTT, F1ATB, batterie, Tempo, version). Toucher le bord droit / gauche ou glisser pour changer d'écran. Réglages dans Système → « 🖥️ Écran intégré » : luminosité, nuit (baissée ou éteinte, un appui rallume), écran de départ, défilement automatique, écran retourné. L'écran est reconnu tout seul ; une carte sans écran n'est pas concernée. Version à installer : « ESP32-S3 8 Mo PSRAM ».
- **Nom dans la box** : le module s'annonce enfin sous son nom (orkel-iot) au lieu de « esp32s3-XXXXXX ». Les modules encore nommés « orkelys » passent tout seuls à « orkel-iot » (adresse http://orkel-iot.local/).
- **Topics MQTT** : `orkel-iot/etat` et `orkel-iot/batterie` (les anciens `orkelys/…` enregistrés sont convertis). Option (cochée) pour publier aussi sur les anciens topics, le temps de mettre à jour les afficheurs.
- **Seuil Pw du F1ATB** : quand la Zendure est pilotée par Orkel-ioT (ou en « eau chaude d'abord »), le cadre indique que le seuil est fixe (« batterie pleine ») et grise les réglages qui ne servent pas.

## V1.0.0 — premier départ sous le nom Orkel-ioT
Orkel-ioT est le nouveau nom d'**Orkelys** (le programme est le même ; la numérotation repart à V1.0.0).
Cette première version reprend tout ce qui existait, plus :

- **Journée enregistrée aussi en mode réel** : page 📊 Bilan → « 📖 Journée (réel et simulation) ». Graphique, décisions et actions réellement envoyées (F1ATB, batterie), gardées dans la mémoire du module : on relit la nuit même après une coupure ou un redémarrage.
- On y voit aussi les **pertes de Wi-Fi**, les connexions / déconnexions MQTT, les **redémarrages et leur cause**, et une ligne « santé » par heure (mémoire, signal Wi-Fi).
- **Wi-Fi perdu en route** (box redémarrée…) : reconnexion relancée chaque minute, redémarrage tout seul après 10 min sans Wi-Fi.
- **Chien de garde** : si le programme se bloque plus de 90 s, le module redémarre tout seul (cause notée).
- 2e charge / 2e F1ATB : commande au choix **tOnOff** ou **Mode** ({"Mode":"Inactif"} ↔ {"Mode":"Decoupe"}).
- Calendrier : **plages « Interdit » avec les routeurs en Mode Inactif** (seuil de charge de départ envoyé avant), retour au mode actif à la fin.
- Nom réseau par défaut **orkel-iot.local**, Wi-Fi de secours **Orkel-ioT-Setup**.

Fonctions principales : pilotage du F1ATB selon la batterie (Zendure en local, Izypower par le cloud, autre marque ou aucune), la production PV, la météo, le tarif (Base, heures creuses, Tempo) et vos règles (calendrier, températures, anti-légionelle, vacances) ; deuxième F1ATB en cascade ; simulateur sur mesures réelles ; bilan sur 62 jours ; état MQTT pour l'afficheur ; mise à jour en un clic depuis GitHub.
