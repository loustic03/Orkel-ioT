#  Todo list Orkel-ioT

Pense-bête pour les prochaines versions. Cocher une case : remplacer `[ ]` par `[x]`
(GitHub affiche alors une case cochée). Quand une version sort, déplacer les lignes cochées dans « Fait ».

##  Bugs à corriger

- [ ] Remonter d'info mqtt production + routeur , si les 2 ne remontent pas il n'y a aucunes infos qui remonte 

## À vérifier

- [x] V1.7.7 : tester l'**adresse IP fixe** sur un vrai module (bloc Wi-Fi)
- [ ] V1.7.5 : tester la **charge d'équilibrage** sur la Zendure (surtout la détection de fin de charge à 100 %)
- [ ] Izypower : contrôler sur une vraie journée les **compteurs du Bilan** (énergie injectée `meter_energy_p` : bonne valeur ? kWh ou Wh ?) avec la ligne de journal « [Izypower] Rapport du jour »
- [ ] Wi-Fi : le bouton « Essayer » de la page (point d'accès Orkel-ioT-Setup) ne fait qu'un essai simple ; la séquence complète (point d'accès le plus fort, puissance réduite) n'a lieu qu'au redémarrage

## Idées pour la suite

- [ ] Charge d'équilibrage : option « arrêter à la fin des heures creuses »
- [ ] Licence : bouton « Changer de clé »

##  Utilisateurs

- [ ] Jean : passage de l'ESP32 classique à l'ESP32-S3 N8R8 → sauvegarde / restauration de la configuration, puis **nouvelle clé** (l'identifiant du module change)

##  À chaque publication

- [ ] `docs/firmware/Vx.y.z/` : les 3 .bin d'installation + les 3 manifestes
- [ ] `docs/versions.json` et `docs/updates/updates.json`
- [ ] `README.md` (« Version actuelle ») et `CHANGELOG.md` à la racine
- [ ] Release `vx.y.z` avec les 3 fichiers `OTA_…_Orkel-ioT.ino.bin`
- [ ] Ouvrir la page d'installation et vérifier que la nouvelle version apparaît

## Fait

- V1.7.7 : adresse IP fixe ; connexion au point d'accès le plus fort ; diagnostic Wi-Fi « mot de passe accepté mais pas d'adresse IP »
