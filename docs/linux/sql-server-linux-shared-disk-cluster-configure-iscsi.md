---
title: Configurer le stockage iSCSI pour une instance de cluster de basculement - SQL Server sur Linux
description: Apprenez à configurer une instance de cluster de basculement avec le stockage iSCSI pour SQL Server sur Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 20953eb151912f6eddf1e3ea0e13d34f3835e7d6
ms.sourcegitcommit: 3e2421ae45a8e9fa57fb590a5d1a5566721ea74a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98921407"
---
# <a name="configure-failover-cluster-instance---iscsi---sql-server-on-linux"></a>Configurer l’instance de cluster de basculement - iSCSI - SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment configurer le stockage iSCSI pour une instance de cluster de basculement (FCI) sur Linux. 

## <a name="configure-iscsi"></a>Configurer l'iSCSI 
iSCSI utilise la mise en réseau pour présenter les disques d’un serveur connu comme cible pour les serveurs. Les serveurs qui se connectent à la cible iSCSI nécessitent la configuration d’un initiateur iSCSI. Les disques sur la cible reçoivent des autorisations explicites, de sorte que seuls les initiateurs qui doivent pouvoir y accéder puissent le faire. La cible elle-même doit être hautement disponible et fiable.

### <a name="important-iscsi-target-information"></a>Informations importantes sur la cible iSCSI
Bien que cette section ne traite pas de la configuration d’une cible iSCSI dans la mesure où elle est spécifique au type de source que vous allez utiliser, assurez-vous que la sécurité pour les disques qui seront utilisés par les nœuds de cluster est configurée.  

La cible ne doit jamais être configurée sur l’un des nœuds FCI en cas d’utilisation d’une cible iSCSI Linux. Pour les performances et la disponibilité, les réseaux iSCSI doivent être séparés de ceux utilisés par le trafic réseau normal sur les serveurs source et client. Les réseaux utilisés pour iSCSI doivent être rapides. Rappelez-vous que le réseau utilise une bande passante de processeur, planifiez-la en conséquence si vous utilisez un serveur standard.
La chose la plus importante pour s’assurer qu’elle est effectuée sur la cible est que les disques qui sont créés se voient attribuer les autorisations appropriées afin que seuls les serveurs participant à l’instance de cluster de basculement aient accès à ces derniers. Un exemple est illustré ci-dessous dans la cible Microsoft iSCSI, où linuxnodes1 est le nom créé et dans ce cas, les adresses IP des nœuds sont affectées afin que NewFCIDisk1.vhdx y soit affiché.

![Initiateur][1]

### <a name="instructions"></a>Instructions

Cette section explique comment configurer un initiateur iSCSI sur les serveurs qui serviront de nœuds pour l’instance de cluster de basculement. Les instructions doivent fonctionner telles quelles pour RHEL et Ubuntu.

Pour plus d’informations sur l’initiateur iSCSI pour les distributions prises en charge, consultez les liens suivants :
- [Red Hat](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Storage_Administration_Guide/iscsi-api.html)
- [SUSE](https://www.suse.com/documentation/sles11/stor_admin/data/sec_inst_system_iscsi_initiator.html) 
- [Ubuntu](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html)

1.  Choisissez un des serveurs qui fera partie de la configuration de l’instance de cluster de basculement. Vous pouvez choisir n’importe lequel. iSCSI doit se trouver sur un réseau dédié. Vous devez donc configurer iSCSI pour reconnaître et utiliser ce réseau. Exécutez `sudo iscsiadm -m iface -I <iSCSIIfaceName> -o new` où `<iSCSIIfaceName>` est le nom unique ou convivial du réseau. L'exemple suivant utilise `iSCSINIC` :
   
    ```bash
    sudo iscsiadm -m iface -I iSCSINIC -o new
    ```
    ![Capture d’écran de la commande iface et de la réponse à la commande.][6]
 
2.  Modifier `/var/lib/iscsi/ifaces/iSCSIIfaceName`. Assurez-vous que les valeurs suivantes sont entièrement remplies :

    - iface. net _ifacename est le nom de la carte réseau, comme indiqué dans le système d’exploitation.
    - iface. hwaddress est l’adresse MAC du nom unique qui sera créé pour cette interface ci-dessous.
    - iface.ipaddress
    - iface.subnet_Mask 

    Voir l’exemple suivant :

    ![Capture d’écran du fichier avec les valeurs complètement renseignées.][2]

3.  Recherchez la cible iSCSI.

    ```bash
    sudo iscsiadm -m discovery -t sendtargets -I <iSCSINetName> -p <TargetIPAddress>:<TargetPort>
    ```

     \<iSCSINetName> est le nom unique/convivial du réseau, \<TargetIPAddress> est l’adresse IP de la cible iSCSI et \<TargetPort> est le port de la cible iSCSI. 

    ![Capture d’écran de la commande discovery et de la réponse à la commande.][3]

 
4.  Se connecter à la cible

    ```bash
    sudo iscsiadm -m node -I <iSCSIIfaceName> -p TargetIPAddress -l
    ```

    \<iSCSIIfaceName> est le nom unique/convivial du réseau et \<TargetIPAddress> est l’adresse IP de la cible iSCSI.

    ![iSCSITargetLogin][4]

5.  Assurez-vous qu’il existe une connexion à la cible iSCSI.

    ```bash
    sudo iscsiadm -m session
    ```

    ![iSCSIVerify][5]

 
6.  Vérifier les disques iSCSI joints

    ```bash
    sudo grep "Attached SCSI" /var/log/messages
    ```
    ![Capture d’écran de la commande grep et de la réponse à la commande montrant les disques SCSI attachés.][7]

7.  Créez un volume physique sur le disque iSCSI.

    ```bash
    sudo pvcreate /dev/<devicename>
    ```

    \<devicename> est le nom du périphérique de l’étape précédente. 

 
8.  Créez un groupe de volumes sur le disque iSCSI. Les disques attribués à un seul groupe de volumes sont considérés comme un pool ou un regroupement. 

    ```bash
    sudo vgcreate <VolumeGroupName> /dev/devicename
    ```

    \<VolumeGroupName> est le nom du groupe de volumes et \<devicename> est le nom du périphérique de l’étape 6. 
 
9.  Créez et vérifiez le volume logique du disque.

    ```bash
    sudo lvcreate -Lsize -n <LogicalVolumeName> <VolumeGroupName>
    ```
    
    \<size> est la taille du volume à créer et peut être spécifié avec G (gigaoctets), T (téraoctets), etc., \<LogicalVolumeName> est le nom du volume logique et \<VolumeGroupName> est le nom du groupe de volumes de l’étape précédente. 

    L’exemple ci-dessous crée un volume de 25 Go.
 
    ![Create25GBVol][10]

10. Exécutez `sudo lvs` pour consulter le LVM qui a été créé.
 
11. Formatez le volume logique avec un système de fichiers pris en charge. Pour EXT4, utilisez l’exemple suivant :

    ```bash
    sudo mkfs.ext4 /dev/<VolumeGroupName>/<LogicalVolumeName>
    ```

    \<VolumeGroupName> est le nom du groupe de volumes de l’étape précédente. \<LogicalVolumeName> est le nom du volume logique de l’étape précédente.  

12. Pour les bases de données système ou tout ce qui est stocké dans l’emplacement des données par défaut, procédez comme suit. Sinon, ignorez l’étape 13.

   * Assurez-vous que SQL Server est arrêté sur le serveur sur lequel vous travaillez.

        ```bash
        sudo systemctl stop mssql-server
        sudo systemctl status mssql-server
        ```
    
   * Basculez entièrement pour être le superutilisateur. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        sudo -i
        ```
    
   * Basculez pour être l’utilisateur mssql. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        su mssql
        ```
    
   * Créez un répertoire temporaire pour stocker les données et les fichiers journaux SQL Server. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        mkdir <TempDir>
        ```
    
        \<TempDir> est le nom du dossier. L’exemple suivant crée un dossier nommé /var/opt/mssql/TempDir.

        ```bash
        mkdir /var/opt/mssql/TempDir
        ```
        
   * Copiez les données et les fichiers journaux de SQL Server dans le répertoire temporaire. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        cp /var/opt/mssql/data/* <TempDir>
        ```
    
        \<TempDir> est le nom du dossier de l’étape précédente.
    
   * Vérifiez que les fichiers se trouvent dans le répertoire.

        ```bash
        ls \<TempDir>
        ```
 
        \<TempDir> est le nom du dossier de l’étape d.

   * Supprimez les fichiers du répertoire de données SQL Server existant. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        rm - f /var/opt/mssql/data/*
        ```
    
   * Vérifiez que les fichiers ont été supprimés. L’image ci-dessous montre un exemple de l’intégralité de la séquence de c à h.

        ```bash
        ls /var/opt/mssql/data
        ```
    
        ![Capture d’écran de la commande ls et de la réponse à la commande.][8]

   * Tapez `exit` pour revenir à l’utilisateur racine.

   * Montez le volume logique iSCSI dans le dossier de données SQL Server. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        mount /dev/<VolumeGroupName>/<LogicalVolumeName> /var/opt/mssql/data
        ```

        \<VolumeGroupName> est le nom du groupe de volumes et \<LogicalVolumeName> est le nom du volume logique qui a été créé. L’exemple de syntaxe suivant correspond au groupe de volumes et au volume logique de la commande précédente.

        ```bash
        mount /dev/FCIDataVG1/FCIDataLV1 /var/opt/mssql/data
        ```

   * Modifiez le propriétaire du montage sur mssql. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        chown mssql /var/opt/mssql/data
        ```

   * Modifiez la propriété du groupe du montage sur mssql. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        chgrp mssql /var/opt/mssql/data
        ```

   * Basculez vers l’utilisateur mssql. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        su mssql
        ``` 

   * Copiez les fichiers à partir du répertoire temporaire /var/opt/mssql/data. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        cp /var/opt/mssql/TempDir/* /var/opt/mssql/data
        ``` 
    
   * Vérifiez que les fichiers sont présents.

        ```bash
        ls /var/opt/mssql/data
        ``` 

   *    Entrez `exit` pour ne pas être mssql.

   *    Entrez `exit` pour ne pas être la racine.

   *    Démarrez SQL Server. Si tout a été copié correctement et que la sécurité est appliquée correctement, SQL Server doit s’afficher comme étant démarré.

        ```bash
        sudo systemctl start mssql-server
        sudo systemctl status mssql-server
        ``` 

   *    Arrêtez SQL Server et vérifiez qu’il est arrêté.

        ```bash
        sudo systemctl stop mssql-server
        sudo systemctl status mssql-server
        ``` 

13. Pour des éléments autres que les bases de données système, tels que les bases de données utilisateur ou les sauvegardes, procédez comme suit. Si vous utilisez uniquement l’emplacement par défaut, passez à l’étape 14.

   *    Basculez pour être le superutilisateur. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        sudo -i
        ```

   *    Créez un dossier qui sera utilisé par SQL Server. 

        ```bash
        mkdir <FolderName>
        ```

        \<FolderName> est le nom du dossier. Le chemin d’accès complet du dossier doit être spécifié s’il n’est pas à l’emplacement approprié. L’exemple suivant crée un dossier nommé /var/opt/mssql/userdata.

        ```bash
        mkdir /var/opt/mssql/userdata
        ```

   *    Montez le volume logique iSCSI dans le dossier créé à l’étape précédente. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        mount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
        ```

        \<VolumeGroupName> est le nom du groupe de volumes, \<LogicalVolumeName> est le nom du volume logique créé et \<FolderName> est le nom du dossier. Voici un exemple de syntaxe.

        ```bash
        mount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
        ```

   *    Modifiez la propriété du dossier créé sur mssql. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        chown mssql <FolderName>
        ```

        \<FolderName> est le nom du dossier créé. Voici un exemple.

        ```bash
        chown mssql /var/opt/mssql/userdata
        ```

   *    Modifiez le groupe du dossier créé sur mssql. Vous ne recevrez pas d’accusé de réception en cas de réussite.

        ```bash
        chown mssql <FolderName>
        ```

        \<FolderName> est le nom du dossier créé. Voici un exemple.

        ```bash
        chown mssql /var/opt/mssql/userdata
        ```

   *    Tapez `exit` pour ne plus être le superutilisateur.

   *    Pour tester, créez une base de données dans ce dossier. L’exemple ci-dessous utilise sqlcmd pour créer une base de données, en changer le contexte, vérifier que les fichiers existent au niveau du système d’exploitation, puis il supprime l’emplacement temporaire. Vous pouvez utiliser SSMS.
  
        ![Capture d’écran de la commande sqlcmd et de la réponse à la commande.][9]

   *    Démonter le partage 

        ```bash
        sudo umount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
        ```

        \<VolumeGroupName> est le nom du groupe de volumes, \<LogicalVolumeName> est le nom du volume logique créé et \<FolderName> est le nom du dossier. Voici un exemple de syntaxe.

        ```bash
        sudo umount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
        ```

14. Configurez le serveur de sorte que seul Pacemaker puisse activer le groupe de volumes.

    ```bash
    sudo lvmconf --enable-halvm --services -startstopservices
    ```

15. Générez une liste des groupes de volumes sur le serveur. Tout ce qui n’est pas le disque iSCSI est utilisé par le système, comme pour le disque du système d’exploitation.

    ```bash
    sudo vgs
    ```

16. Modifiez la section Configuration de l’activation du fichier /etc/lvm/lvm.conf. Configurez la ligne suivante :

    ```bash
    volume_list = [ <ListOfVGsNotUsedByPacemaker> ]
    ```

    \<ListOfVGsNotUsedByPacemaker> est la liste des groupes de volumes de la sortie de l’étape 20 qui ne sera pas utilisée par l’instance de cluster de basculement. Placez-les entre guillemets et séparez-les par une virgule. Voici un exemple.

    ![Capture d’écran montrant un exemple de valeur volume_list.][11]

17. Lors du démarrage de Linux, le système de fichiers est monté. Pour vous assurer que seul Pacemaker peut monter le disque iSCSI, régénérez l’image du système de fichiers racine. 

    Exécutez la commande suivante, ce qui peut prendre un long moment. Vous ne recevrez pas de message en cas de réussite.

    ```bash
    sudo dracut -H -f /boot/initramfs-$(uname -r).img $(uname -r)
    ```

18. Redémarrez le serveur.

19. Sur un autre serveur qui fera partie de l’instance FCI, suivez les étapes de 1 à 6. La cible iSCSI est alors présentée à SQL Server. 
 
20. Générez une liste des groupes de volumes sur le serveur. Il doit afficher le groupe de volumes créé précédemment. 

    ```bash
    sudo vgs
    ``` 

23. Démarrez SQL Server et vérifiez qu’il peut être démarré sur ce serveur.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ```

24. Arrêtez SQL Server et vérifiez qu’il est arrêté.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```

25. Répétez les étapes de 1 à 6 sur tous les autres serveurs qui feront partie à l’instance de cluster de basculement.

Vous êtes maintenant prêt à configurer l’instance de cluster de basculement (FCI).

| Distribution | Rubrique |
| :----------- | :---- |
| Red Hat Enterprise Linux avec module complémentaire haute disponibilité | [Configurer](sql-server-linux-shared-disk-cluster-configure.md)<br/>[Exploitation](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md) |
| SUSE Linux Enterprise Server avec module complémentaire haute disponibilité | [Configurer](sql-server-linux-shared-disk-cluster-sles-configure.md) |

## <a name="next-steps"></a>Étapes suivantes

[Configurer l’instance de cluster de basculement - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/05-initiator.png
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/10-iscsiTargetSettings.png
[3]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/15-iSCSITargetResults.png
[4]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/20-iSCSITargetLogin.png
[5]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/25-iSCSIVerify.png
[6]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/7-setiscsinetwork.png
[7]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/30-iSCSIattachedDisks.png
[8]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/45-CopyMove.png
[9]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/50-ExampleCreateSSMS.png
[10]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/40-Create25GBVol.png
[11]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/55-ListOfVGs.png
