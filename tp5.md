# TP 5 : Systèmes de fichiers, partitions et disques
*Mathilde Rubi*

## Exercice 1. Disques et partitions

1. Sur VSphère, on clique droit sur la machine et on fait modifier les paramètres. On ajoute un périphérique, disque dur et on ccoche le type de partitionnement dynamique.

2. Pour vérifier que le disque dur est bien détecté, on peut lister les disques et donc faire soit :
- la commande ll /dev/sd* pour voir les droits
- la commande lsblk pour voir les partitions des disques
- fdisk -l /dev/sa pour voir le début et la fin de chaque partition ainsi que son type.
- blkid
- en allant voir dans le dossier /dev/disk/by-uuid/ avec la commande `ls -l /dev/disk/by-uuid/`
Avec la commande lsblk on voit le nouveau disque de 5 Go tout en bas.

3. On exécute la commande fdisk /dev/sdb, on fait n pour créer une nouvelle partition, on fait p pour la mettre en primary, partition n°1, secteur par défaut, dernier secteur on entre +2G. De la même manière, pour créer la deuxième partition, on fait n, p pour la mettre en primaire, partition n°2, secteur par défaut, dernier secteur en +3000M. Ensuite, pour changer son type, on fait t, on sélectionne la partition 2, on entre le HEX code du type NTFS c'est à dire 07. Avec la commande fdisk -l on voit que les deux partitions ont bien été créées.

4. Pour formater les partitions, en lisant le manuel on voit que mkfs est déprécié, on fait donc les commandes mke2fs /dev/sdb1 pour la partition 1 qui est en Linux et la commande mkntfs /dev/sdb2 pour la partition 2 en NTFS.

5. La commande df -t ne fonctionne pas sur notre disque car notre disque n'est pas encore monté.

6. Pour faire en sorte que les deux partitions créées soient montées automatiquement au démarrage de la machine, dans les points de montage /data et /win, on modifie le fichier /etc/fstab en insérant les lignes `/dev/sdb1 /data ext4 defaults 0 1` et `/dev/sdb2 /win ntfs defaults 0 1`

7. On exécute ensuite la commande `mount` et on redémarre la VM avec la commande `sudo reboot`. En exécutant la commande df -T, on constate que les deux partitions ont été monté.

## Exercice 2. Partitionnement LVM

1. Pour démonter les systèmes de fichiers montés dans /data et /win on exécute les commandes `umount /data` et `umount /win`. On supprime aussi les lignes dans le fichier /etc/fstab.

2. Pour supprimer les deux partitions du disque on exécute les commandes `fdisk /dev/sdb ; d ; puis on indique la partition a supprimer : 2. On refait d et la première partition se supprime aussi. Puis on crée une partition avec n, on laisse les paramètres par défaut puis on change son type avec t et on le met en LVM c'est à dire le code 8e.

3. On crée un volume physique LVM avec la commande `sudo pvcreate /dev/sdb1`. En exécutant la commande pvdisplay on voit que le volume a bien été créé.

4. On crée un groupe de volume qui pour l'instant ne contient que le volume physique créé à l'étape précédente avec la commande `sudo vgcreate vg00 /dev/sdb1`. En exécutant la commande `sudo vgdisplay`, on voit que le groupe vg00 apparait.

5.
