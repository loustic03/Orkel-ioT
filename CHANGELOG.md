# Versions

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
