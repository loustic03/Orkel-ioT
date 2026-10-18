# Versions

## V1.6.0
- **Calendrier de la batterie** (Automatisation → 📅 Calendrier → onglet « 🔋 Batterie »), sur la même grille par demi-heure que le ballon :
  - **Veille** : ni charge ni décharge ;
  - **Décharge seulement** : la batterie alimente la maison mais ne repasse pas en charge. Pratique aux heures des repas, quand le four ou les plaques à induction font des à-coups qui font basculer la batterie charge / décharge et claquer ses relais ;
  - **SOC mini** : la batterie ne descend pas sous le SOC choisi (une valeur réglable).
  - Pour une batterie pilotée par Orkel-ioT : Zendure (les trois), Izypower par le cloud (veille et SOC mini).
  - Compatible avec le F1ATB : pendant une veille ou une décharge seulement, la batterie ne prend pas le surplus, qui va au ballon si son calendrier l'autorise.
- **Orkel-ioT sans F1ATB** (page « Routeur F1ATB » → « Je n'ai pas de F1ATB ») : Orkel-ioT pilote seulement la batterie. Le compteur de la maison est lu sur un **Shelly EM / Pro 3EM / EM Gen3** (HTTP, adresse remplie toute seule), par **MQTT**, ou sur le **cloud Izypower** (lent). Les pages et réglages du ballon sont masqués ; l'écran de la JC3248W535 n'affiche plus de ballon ; Telegram et ses alertes s'adaptent (« compteur muet »).
- **🔋 Test réel de la mise en veille de la batterie** (page 🧪 Simulateur) : Orkel-ioT met vraiment la batterie en veille pendant 5, 10 ou 15 min (même en mode simulation, après confirmation), relit le réglage dans le cloud, suit la puissance de la batterie toutes les 30 s et affiche le déroulé minute par minute, puis la remet dans son mode habituel. Résultat : la batterie obéit-elle, et en combien de temps ? Pour une Izypower, un bouton applique directement le réglage d'avance conseillé. Batteries : Izypower (cloud) et Zendure pilotée (réseau local).
- **Mise en veille en avance** (Izypower, bloc « Batterie en veille quand le ballon chauffe ») : 0 à 3 min. Le cloud met 1 à 3 min à transmettre la veille ; pendant ce temps la batterie se vidait dans le ballon. Créneau forcé du calendrier et garantie d'eau chaude : veille demandée X min avant l'heure. Marche forcée manuelle (page, Telegram) : veille tout de suite, le ballon démarre X min après (durée rallongée d'autant).

## V1.5.0
- **Bot Telegram** (page Système → « 💬 Telegram ») : suivre et commander Orkel-ioT depuis le téléphone, sans rien ouvrir dans la box ni installer de serveur. Le module se connecte lui-même à Telegram (connexion chiffrée, certificat vérifié).
  - **Boutons** : 📊 État (production, maison, réseau, batterie, ballon, décision et sa raison, bilan du jour en euros), 📅 Hier (bilan de la veille), 🔥 Boost 1 h / 2 h, ⛔ Arrêt, 🤖 Auto. Messages écrits compris aussi (« boost 90 min », « arrêt 2 h »…).
  - **Alertes de panne** : F1ATB muet depuis 5 min, batterie non lue depuis 10 min, liaison MQTT coupée en boucle, redémarrage (avec la cause), Wi-Fi revenu — et le retour à la normale.
  - **Sécurité** : seuls les téléphones associés (code à 6 chiffres valable 10 min, 3 téléphones au plus) peuvent lire ou commander ; jeton et téléphones jamais affichés ni exportés.
  - Guide pas à pas : `Orkel-ioT_guide_Telegram.pdf`.
- **Page web compressée** : 69 Ko envoyés au lieu de 242 Ko (page plus rapide à ouvrir) et environ 170 Ko de mémoire de programme libérés (ESP32 classique et ESP32-S3 4 Mo).

## V1.4.0
- **Marstek Venus (C / E) par RS485** (lecture seule) : SOC, puissance, tension, côté maison, état, mode, versions, lus toutes les 2 s par la prise RS485 de la batterie, en local. Matériel : un module TTL ↔ RS485 à sens automatique (MAX3485…). Broches réglables : JC3248W535 et ESP32-S3 → GPIO17 (TX) / GPIO18 (RX), ESP32 classique → GPIO17 / GPIO16. Orkel-ioT n'écrit jamais dans la batterie : elle garde son mode, Orkel-ioT décide quand le F1ATB prend le surplus.
- **Bloc « 🔌 Liaison RS485 »** : questions, réponses, sans réponse, trames abîmées, temps de réponse, dernières trames envoyées / reçues, valeurs lues, tableau des registres bruts, bouton « Lire maintenant ». Case **« Tester la liaison RS485 sans changer de batterie »** pour vérifier le câblage avec une autre batterie déclarée.
- **Écran RS485 sur la JC3248W535** (4e écran, seulement si la liaison tourne) : voyants TX / RX qui clignotent à chaque échange, compteurs, valeurs lues, dernières trames ; badge « RS485 OK » ou « PAS DE RÉPONSE ».
- **Page Batterie réorganisée** : choix de la batterie par tuiles (Zendure, Marstek, Izypower, Autre marque, Pas de batterie), puis les réglages de la batterie choisie, puis ce qui est commun à toutes (priorité, verrou du ballon et protections, publication MQTT). Les protections ont leur propre bloc.
- **Surveillance MQTT** (broker externe) :
  - identifiant MQTT unique par module (nom + fin de l'adresse matérielle) : deux Orkel-ioT sur le même broker ne peuvent plus s'éjecter l'un l'autre, même avec une configuration copiée ;
  - alerte sur l'accueil et dans le journal si la liaison est coupée en boucle ;
  - les déconnexions sont notées dans le journal ;
  - F1ATB muet depuis 5 min alors que la liaison semble ouverte : la liaison est refaite ; après 15 min, un redémarrage (une seule fois tant que le F1ATB ne reparle pas).

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
