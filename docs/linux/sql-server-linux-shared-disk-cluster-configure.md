---
title: Configurer une instance de cluster de basculement - SQL Server sur Linux (RHEL)
description: Apprenez à configurer une instance de cluster de basculement sur RHEL (Red Hat Enterprise Linux) pour SQL Server.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 31c8c92e-12fe-4728-9b95-4bc028250d85
ms.openlocfilehash: b2b97fa250b5af1a0030b51b0ea29a89324db9c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346305"
---
# <a name="configure-failover-cluster-instance---sql-server-on-linux-rhel"></a>Configurer l’instance de cluster de basculement - SQL Server sur Linux (RHEL)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Une instance de cluster de basculement de disque partagé à deux nœuds SQL Server fournit une redondance au niveau du serveur pour la haute disponibilité. Dans ce didacticiel, vous allez apprendre à créer une instance de cluster de basculement à deux nœuds de SQL Server sur Linux. Les étapes spécifiques que vous allez effectuer sont les suivantes :

> [!div class="checklist"]
> * Installer et configurer Linux
> * Installer et configurer SQL Server
> * Configurer le fichier hosts
> * Configurer le stockage partagé et déplacer les fichiers de base de données
> * Installer et configurer Pacemaker sur chaque nœud de cluster
> * Configurer l’instance de cluster de basculement

Cet article explique comment créer une instance de cluster de basculement de disque partagé à deux nœuds (FCI) pour SQL Server. Cet article contient des instructions et des exemples de scripts pour Red Hat Enterprise Linux (RHEL). Les distributions Ubuntu sont similaires à RHEL afin que les exemples de script fonctionnent également sur Ubuntu. 

Pour plus d’informations, consultez [Instance de cluster de basculement (FCI) SQL Server sur Linux](sql-server-linux-shared-disk-cluster-concepts.md).

## <a name="prerequisites"></a>Prérequis

Pour effectuer le scénario de bout en bout suivant, vous avez besoin de deux machines pour déployer le cluster à deux nœuds et d’un autre serveur pour le stockage. Les étapes ci-dessous décrivent comment ces serveurs seront configurés.

## <a name="set-up-and-configure-linux"></a>Installer et configurer Linux

La première étape consiste à configurer le système d'exploitation sur les nœuds de cluster. Sur chaque nœud du cluster, configurez une distribution Linux. Utilisez la même distribution et la même version sur les deux nœuds. Utilisez l’une ou l’autre des distributions suivantes :
    
* RHEL avec un abonnement valide pour le module complémentaire HA

## <a name="install-and-configure-sql-server"></a>Installer et configurer SQL Server

1. Installez et configurez des SQL Server sur les deux nœuds.  Pour obtenir des instructions détaillées, consultez [Installer SQL Server sur Linux](sql-server-linux-setup.md).
1. Désignez un nœud comme principal et l’autre comme secondaire, à des fins de configuration. Utilisez ces termes pour le présent guide.  
1. Sur le nœud secondaire, arrêtez et désactivez SQL Server.
    L’exemple suivant arrête et désactive SQL Server : 
    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl disable mssql-server
    ```

    > [!NOTE] 
    > Au moment de la configuration, une clé principale de serveur est générée pour l’instance SQL Server et placée à l’adresse `var/opt/mssql/secrets/machine-key`. Sur Linux, SQL Server s’exécute toujours en tant que compte local appelé mssql. Étant donné qu’il s’agit d’un compte local, son identité n’est pas partagée entre les nœuds. Par conséquent, vous devez copier la clé de chiffrement du nœud principal sur chaque nœud secondaire afin que chaque compte mssql local puisse y accéder pour déchiffrer la clé principale du serveur. 

1.  Sur le nœud principal, créez une connexion SQL Server pour Pacemaker et octroyez l’autorisation de connexion pour exécuter `sp_server_diagnostics`. Pacemaker utilise ce compte pour vérifier le nœud en cours d’exécution SQL Server. 

    ```bash
    sudo systemctl start mssql-server
    ```
   
   Connectez-vous à la base de données `master` SQL Server avec le compte sa et exécutez la commande suivante :

   ```sql
   USE [master]
   GO
   CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'
   ALTER SERVER ROLE [sysadmin] ADD MEMBER [<loginName>]
   ```

   Vous pouvez aussi définir les autorisations à un niveau plus granulaire. La connexion à Pacemaker requiert `VIEW SERVER STATE` pour demander le statut d’intégrité avec sp_server_diagnostics, `setupadmin` et `ALTER ANY LINKED SERVER` pour mettre à jour le nom de l’instance FCI avec le nom de la ressource en exécutant sp_dropserver et sp_addserver. 

1. Sur le nœud principal, arrêtez et désactivez SQL Server. 

## <a name="configure-the-hosts-file"></a>Configurer le fichier hosts

Sur chaque nœud de cluster, configurez le fichier hôtes. Le fichier hôtes doit inclure l’adresse IP et le nom de chaque nœud de cluster.

1. Vérifiez l’adresse IP de chaque nœud. Le script suivant affiche l’adresse IP de votre nœud actuel. 

    ```bash
    sudo ip addr show
    ```

1. Définissez le nom de l’ordinateur sur chaque nœud. Donnez à chaque nœud un nom unique de 15 caractères ou moins. Définissez le nom de l’ordinateur en l'ajoutant à `/etc/hosts`. Le script suivant vous permet de modifier `/etc/hosts` avec `vi`. 

   ```bash
   sudo vi /etc/hosts
   ```
   L’exemple suivant présente `/etc/hosts` avec des ajouts pour deux nœuds nommés `sqlfcivm1` et `sqlfcivm2`.

   ```bash
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1       localhost localhost6 localhost6.localdomain6
   10.128.18.128 sqlfcivm1
   10.128.16.77 sqlfcivm2
   ```

## <a name="configure-storage--move-database-files"></a>Configurer le stockage et déplacer des fichiers de base de données  

Vous devez fournir un stockage auquel les deux nœuds peuvent accéder. Vous pouvez utiliser iSCSI, NFS ou SMB. Configurez le stockage, présentez le stockage aux nœuds de cluster, puis déplacez les fichiers de base de données vers le nouveau stockage. Les articles suivants expliquent les étapes pour chaque type de stockage :

- [Configurer l’instance de cluster de basculement - iSCSI - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure-iscsi.md)
- [Configurer l’instance de cluster de basculement - NFS - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure-nfs.md)
- [Configurer l’instance de cluster de basculement - SMB - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure-smb.md)

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installer et configurer Pacemaker sur chaque nœud de cluster

1. Sur les deux nœuds du cluster, créez un fichier pour stocker le nom d’utilisateur et le mot de passe SQL Server du compte de connexion Pacemaker. 

    La commande suivante a pour effet de créer et remplir ce fichier :

    ```bash
    sudo touch /var/opt/mssql/secrets/passwd
    sudo echo '<loginName>' >> /var/opt/mssql/secrets/passwd
    sudo echo '<loginPassword>' >> /var/opt/mssql/secrets/passwd
    sudo chown root:root /var/opt/mssql/secrets/passwd 
    sudo chmod 600 /var/opt/mssql/secrets/passwd    
    ```

1. Sur les deux nœuds de cluster, ouvrez les ports de pare-feu Pacemaker. Pour ouvrir ces ports avec `firewalld`, exécutez la commande suivante :

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > Si vous utilisez un autre pare-feu qui n’intègre pas de configuration à haute disponibilité intégrée, les ports suivants doivent être ouverts pour permettre à Pacemaker de communiquer avec les autres nœuds du cluster
   >
   > * TCP : Ports 2224, 3121, 21064
   > * UDP : Port 5405

1. Installez les packages Pacemaker sur chaque nœud.

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```
1. Définissez le mot de passe pour l’utilisateur par défaut qui est créé pendant l’installation des packages Pacemaker et Corosync. Utilisez le même mot de passe sur les deux nœuds. 

   ```bash
   sudo passwd hacluster
   ```
1. Activez et démarrez le service `pcsd` et Pacemaker. Cela permettra aux nœuds de rejoindre le cluster après le redémarrage. Exécutez la commande suivante sur les deux nœuds.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

1. Installez l’agent de ressources FCI pour SQL Server. Exécutez les commandes suivantes sur les deux nœuds. 

   ```bash
   sudo yum install mssql-server-ha
   ```

## <a name="configure-the-failover-cluster-instance"></a>Configurer l’instance de cluster de basculement

Le FCI sera créé dans un groupe de ressources. C’est un peu plus facile puisque le groupe de ressources atténue le besoin de contraintes. Toutefois, ajoutez les ressources dans le groupe de ressources dans l’ordre dans lequel elles doivent démarrer. L’ordre dans lequel elles doivent démarrer est le suivant : 

1. Ressource de stockage
2. Ressource de réseau
3. Ressource d'application

Cet exemple crée un FCI dans le groupe NewLinFCIGrp. Le nom du groupe de ressources doit être unique à partir de n’importe quelle ressource créée dans Pacemaker.

1.  Créez la ressource disque. Vous n’obtiendrez aucune réponse s’il n’y a pas de problème. La méthode de création de la ressource disque dépend du type de stockage. Voici un exemple pour chaque type de stockage. Utilisez l’exemple qui s’applique au type de stockage de votre stockage en cluster.

    **iSCSI**

    ```bash
    sudo pcs resource create <iSCSIDiskResourceName> Filesystem device="/dev/<VolumeGroupName>/<LogicalVolumeName>" directory="<FolderToMountiSCSIDisk>" fstype="<FileSystemType>" --group RGName
    ```

    \<iSCSIDIskResourceName> est le nom de la ressource associée au disque iSCSI

    \<VolumeGroupName> est le nom du groupe de volumes  

    \<LogicalVolumeName> est le nom du volume logique qui a été créé  

    \<FolderToMountiSCSIDIsk> est le dossier de montage du disque (pour les bases de données système et l’emplacement par défaut, il s’agit de /var/opt/mssql/data)

    \<FileSystemType> serait EXT4 ou XFS en fonction de la façon dont les éléments ont été mis en forme et de ce que la distribution prend en charge. 

    **NFS**

    ```bash
    sudo pcs resource create <NFSDiskResourceName> Filesystem device="<IPAddressOfNFSServer>:<FolderOnNFSServer>" directory="<FolderToMountNFSShare>" fstype=nfs4 options=" nfsvers=4.2,timeo=14,intr" --group RGName
    mount -t nfs4 IPAddressOfNFSServer:FolderOnNFSServer /var/opt/mssql/data -o 
    ```

    \<NFSDIskResourceName> est le nom de la ressource associée au partage NFS

    \<IPAddressOfNFSServer> est l’adresse IP du serveur NFS que vous allez utiliser

    \<FolderOnNFSServer> est le nom du partage NFS

    \<FolderToMountNFSShare> est le dossier de montage du disque (pour les bases de données système et l’emplacement par défaut, il s’agit de /var/opt/mssql/data)

    En voici un exemple :

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci1 /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

    **SMB**

    ```bash
    sudo pcs resource create SMBDiskResourceName Filesystem device="//<ServerName>/<ShareName>" directory="<FolderName>" fstype=cifs options="vers=3.0,username=<UserName>,password=<Password>,domain=<ADDomain>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777" --group <RGName>
    ```

    \<ServerName> est le nom du serveur avec le partage SMB

    \<ShareName> est le nom du partage

    \<FolderName> est le nom du dossier créé à la dernière étape
    
    \<UserName> est le nom de l’utilisateur pour accéder au partage

    \<Password> est le mot de passe de l’utilisateur

    \<ADDomain> est le domaine AD DS (le cas échéant, lors de l’utilisation d’un partage SMB basé sur Windows Server)

    \<mssqlUID> est l’UID de l’utilisateur mssql

    \<mssqlGID> est le GID de l’utilisateur mssql

    \<RGName> est le nom du groupe de ressources
 
2.  Créez l’adresse IP qui sera utilisée par le FCI. Vous n’obtiendrez aucune réponse s’il n’y a pas de problème.

    ```bash
    sudo pcs resource create <IPResourceName> ocf:heartbeat:IPaddr2 ip=<IPAddress> nic=<NetworkCard> cidr_netmask=<NetMask> --group <RGName>
    ```

    \<IPResourceName> est le nom de la ressource associée à l’adresse IP

    \<IPAddress> est l’adresse IP du FCI

    \<NetworkCard> est la carte réseau associée au sous-réseau (c.-à-d. eth0)

    \<NetMask> est le masque réseau du sous-réseau (c.-à-d. 24)

    \<RGName> est le nom du groupe de ressources
 
3.  Créer la ressource FCI. Vous n’obtiendrez aucune réponse s’il n’y a pas de problème.

    ```bash
    sudo pcs resource create FCIResourceName ocf:mssql:fci op defaults timeout=60s --group RGName
    ```

    \<FCIResourceName> n’est pas seulement le nom de la ressource, mais aussi le nom convivial associé au FCI. Il s’agit de ce que les utilisateurs et les applications utilisent pour se connecter. 

    \<RGName> est le nom du groupe de ressources.
 
4.  Exécutez la commande `sudo pcs resource`. FCI doit être en ligne.
 
5.  Connectez-vous à FCI avec SSMS ou sqlcmd en utilisant le nom DNS/ressource du FCI.

6.  Émettez l'instruction `SELECT @@SERVERNAME`. Elle doit retourner le nom du FCI.

7.  Émettez l'instruction `SELECT SERVERPROPERTY('ComputerNamePhysicalNetBIOS')`. Elle doit retourner le nom du nœud sur lequel le FCI s’exécute.

8.  Faites échouer manuellement le FCI aux autres nœuds. Consultez les instructions sous [Opérer l’instance de cluster de basculement - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-operate.md).

9.  Enfin, rebasculez le FCI vers le nœud d’origine et supprimez la contrainte de colocalisation.

<!---

|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)

-->
## <a name="summary"></a>Résumé

Dans ce tutoriel, vous avez effectué les tâches suivantes.

> [!div class="checklist"]
> * Installer et configurer Linux
> * Installer et configurer SQL Server
> * Configurer le fichier hosts
> * Configurer le stockage partagé et déplacer les fichiers de base de données
> * Installer et configurer Pacemaker sur chaque nœud de cluster
> * Configurer l’instance de cluster de basculement

## <a name="next-steps"></a>Étapes suivantes

- [Opérer l’instance de cluster de basculement - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-operate.md)

<!--Image references-->
