# Versions

## V1.7.8
- **Écran JC3248W535 : page « Tableau de bord »**, affichée en premier : production (en direct, kWh du jour, réalisé / prévu), consommation (en direct, kWh du jour, part du réseau, température du ballon), économies (€ du jour et du mois, autonomie), batterie (SOC, charge ou décharge, « pleine vers » / « reste environ ») et, au centre, un anneau qui montre d'où vient l'électricité de la maison (solaire, batterie, réseau). Fonctionne avec toutes les batteries.
- Nouveau réglage (page Système → Écran intégré) : **capacité utile de la batterie** (kWh), pour le temps restant. Page de démarrage par défaut des nouvelles installations : le tableau de bord (les installations existantes gardent leur choix).
- **🔌 Charge forcée jusqu'à un SOC** (page Batterie, bloc « Charge forcée et équilibrage ») : charger la batterie à la puissance choisie jusqu'au SOC choisi (ex. 20 %), maintenant, à une date et heure ou aux prochaines heures creuses, puis retour au pilotage normal. Zendure et Izypower pilotées.
- **Mot de passe demandé** pour lancer ou programmer une charge forcée ou d'équilibrage (le mot de passe de mise à jour), contre les erreurs de manipulation. Arrêter et annuler restent libres.
- **Sonde du 2e F1ATB** (bloc « Deuxième charge en cascade ») : choix du canal de température affiché pour la 2e charge (avant : toujours le canal 0).
- **Noms des charges dans la page** : les titres et menus (consigne, règles, priorité, « qui passe en premier ») reprennent les noms choisis (« Ce que chauffe le F1ATB », nom de la 2e charge) au lieu de « ballon » : ex. « ➕ Tampon d'abord, puis Bech ».
- **Changement des noms MQTT des F1ATB** (routeur principal, action, routeur de la 2e charge) : Orkel-ioT redémarre aussitôt pour s'abonner aux nouveaux sujets (avant : il fallait le redémarrer à la main pour recevoir les mesures).
- **🧠 Apprentissage du ballon** : Orkel-ioT mesure la vitesse de chauffe réelle (°C/h, à chaque chauffe forcée d'au moins 8 min, puisages écartés) et les pertes de la nuit. Dès 2 chauffes mesurées, la **garantie d'eau chaude** s'en sert pour démarrer au bon moment (au lieu d'une estimation par le volume et la puissance, souvent trop prudente). Affiché dans « 🧠 Ballon et prévisions » ; bouton « Oublier l'apprentissage » (changement de ballon ou de sonde).
- **Garantie d'eau chaude** : plus de relance à moins de 5 min de l'heure cible (une chauffe d'une minute ne sert à rien).
- **♻️ Ne rien renvoyer au réseau** (Règles du ballon, activée d'origine) : si le ballon est bloqué par la consigne, l'hystérésis ou l'arrêt du jour mais que du surplus part au réseau depuis 60 s (batterie pleine ou bridée), le F1ATB est libéré quand même, jusqu'à la limite de température réglée dans son action et la sécurité température maxi ; rebloqué dès qu'il prend sur le réseau ou la batterie.
- **Puissance du ballon** affichée dans le schéma des flux (≈ ouverture du triac × puissance de la résistance).
- **Zendure : régulation sans emballement** : quand la batterie met quelques secondes à suivre une consigne (grosse charge qui démarre : four, bouilloire), Orkel-ioT repart de la puissance mesurée au lieu d'empiler les corrections ; fini les consignes qui montent à 1600 W puis repartent au réseau.
- **IP de secours automatique** : Orkel-ioT retient la dernière adresse donnée par la box. Si la box n'en distribue plus (DHCP en panne : Orkel associé au Wi-Fi mais sans adresse, ou adresse non renouvelée), il se reconnecte tout seul avec cette adresse au lieu de rester injoignable ; journal, page Système et message Telegram le signalent (« pensez à redémarrer la box »). Sans effet avec une adresse IP fixe.

## V1.7.7
- **Adresse IP fixe** (page Système → Wi-Fi → « Adresse IP fixe ») : adresse, passerelle, masque et DNS. Pour une box qui associe Orkel-ioT sans lui attribuer d'adresse (DHCP), ou pour une adresse qui ne change jamais. Non copiée par la restauration d'une sauvegarde quand « Garder le Wi-Fi » est coché.
- **Diagnostic Wi-Fi** : quand le mot de passe est accepté mais qu'aucune adresse IP n'arrive, le journal et la page le disent clairement au lieu de « connexion refusée ».
- **Connexion Wi-Fi plus robuste** : quand la box a des répéteurs (plusieurs points d'accès du même nom), Orkel-ioT s'accroche au plus fort au lieu du premier trouvé ; 2e essai directement sur ce point d'accès, 3e essai à puissance d'émission réduite (certaines cartes ESP32-S3 n'arrivent pas à s'associer à pleine puissance près de la box).
- Journal : sécurité de chaque point d'accès (WPA2, WPA2/WPA3, WPA3 seul…) et étape « associé, attente de l'adresse IP » pour distinguer un refus du mot de passe d'un problème de DHCP.

## V1.7.5
- **Charge d'équilibrage de la batterie** (page Batterie → « ⚖️ Charge d'équilibrage ») pour une batterie pilotée par Orkel-ioT (Zendure, Izypower) : lancée **par l'utilisateur** — maintenant, à une **date et heure** choisies, ou au début des **prochaines heures creuses** (tarif HP/HC ou Tempo) —, elle charge la batterie (sur le réseau si besoin) jusqu'au SOC de passage, puis lentement jusqu'à 100 %, puis la garde **en veille à 100 %** le temps choisi (60 min par défaut) pour que le BMS équilibre les cellules, avant de reprendre le pilotage normal. Puissances, SOC de passage et durée réglables ; bouton Arrêter ; 12 h au plus.
- **Rappel** : Orkel-ioT note la date de la dernière charge complète ; au-delà du nombre de jours choisi (14 par défaut), un bandeau sur l'accueil et un message Telegram (une fois par jour) proposent l'équilibrage.

## V1.7.4
- **Demander sa clé** (Système → 🔑 Licence) : le **nom** et l'**e-mail** du demandeur sont obligatoires (la clé est renvoyée à cette adresse), puis trois boutons au choix — **Gmail** (nouveau message déjà rempli dans le navigateur), **Messagerie de l'appareil** (Outlook, Mail…) et **📋 Copier la demande** (à coller dans n'importe quelle messagerie).
- Le don a quitté le bloc Licence : il est désormais dans un bloc discret « 💛 Soutenir Orkel-ioT », en bas de la page Système (avant les guides).

## V1.7.3
- **Bouton « ☕ Faire un don »** (Système → 🔑 Licence) : il ouvre maintenant la page de don PayPal officielle d'Orkel-ioT.

## V1.7.2
- **Demander sa clé en un clic** (Système → « 🔑 Licence ») : le bouton « ✉️ Demander ma clé » ouvre un e-mail tout prêt vers thierry260sg@gmail.com, avec l'identifiant du module, la carte et la version.
- **Bouton « ☕ Faire un don (PayPal) »** dans le même bloc, pour soutenir Orkel-ioT.

## V1.7.1
- **Licence par module** (page Système → « 🔑 Licence ») : Orkel-ioT affiche l'identifiant du module (bouton « Copier ») ; l'auteur fabrique la clé correspondante. La clé ne vaut que pour ce module, pour toutes les versions, et reste en mémoire lors des mises à jour.
- **Sans clé : mode démo** — pages, réglages, simulateur, scénario 24 h, prévisions et bilan restent disponibles, mais la simulation est imposée : rien n'est envoyé au F1ATB ni à la batterie (le test réel de la mise en veille est aussi bloqué). Bandeau rouge sur la page web, badge « MODE DÉMO » sur l'écran de la JC3248W535, mention dans Telegram.

## V1.6.3
- **Trois cartes prises en charge** : ESP32 classique, ESP32-S3 N8R8 / N16R8 (avec PSRAM) et JC3248W535. La version « ESP32-S3 universelle 4 Mo » n'est plus fournie (un ESP32-S3 installé avec elle se réinstalle une fois par câble en « ESP32-S3 avec PSRAM »).
- **Graphique de la production solaire prévue** (page d'accueil, sous le soleil) : production attendue heure par heure pour aujourd'hui, demain et après-demain (rayonnement prévu par Open-Meteo × rendement appris de l'installation, ou puissance crête en attendant), avec la **production réelle du jour** par-dessus et le total prévu / produit. Nécessite la météo activée.
- **Bilan : compteurs du jour du cloud Izypower** (batterie Izypower lue par le cloud) : production, consommation de la maison, énergie entrée et sortie de la batterie, injection et soutirage du jour, lus toutes les 15 min, à côté de ce qu'Orkel-ioT a mesuré.

## V1.6.2
- **Réinstaller une version republiée** : si une version est republiée sous le même numéro avec des corrections (fichier différent), la page Système → « 🌐 Mise à jour depuis GitHub » le détecte et propose « 🔁 Réinstaller ». (Valable à partir de la V1.6.2 : pour passer de la V1.6.1, cette version porte un nouveau numéro.)
- Regroupe les corrections publiées en cours de route sous le numéro V1.6.1 (ci-dessous) : simulateur réaliste, nom de la charge, surveillance MQTT, tuile de la 2e charge, guides PDF.

## V1.6.1
- **Flux en direct (page d'accueil) : tuile de la 2e charge** (ballon tampon, radiateur…), reliée à la maison comme sur l'écran de la JC3248W535 : son nom, sa température, « libre · X % » ou « bloquée », et la flèche animée quand elle chauffe. État et consigne de la batterie sous sa tuile.
- **Nom de la charge du F1ATB** (bloc « Consigne ballon d'eau chaude ») : radiateur, spa, piscine… au lieu de « Ballon », sur la page web, l'écran de la JC3248W535 et Telegram.
- **Guides PDF** : bloc « 📚 Guides » dans la page Système, et liens directs dans les blocs Telegram et Izypower.
- **MQTT** : si plus aucun message n'arrive du broker pendant 3 min (F1ATB, production, 2e routeur, compteur) alors que la liaison semble ouverte, elle est refaite toute seule. Avant, seul le F1ATB était surveillé, et au bout de 5 min.
- **Simulateur : la maison réagit aux décisions** (case « 🔁 », cochée par défaut) : la batterie pilotée suit les consignes (Zendure aussitôt, Izypower 2 min après comme le cloud) et l'écart se reporte sur le compteur ; F1ATB libre, le ballon prend l'injection restante ; forcé, toute sa puissance ; à consigne, plus rien. On voit enfin l'effet d'une décision (veille de la batterie, surplus vers le ballon) au lieu de valeurs figées.

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
