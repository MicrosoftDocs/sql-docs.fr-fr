---
title: Configurer le stockage NFS pour une instance de cluster de basculement - SQL Server sur Linux
description: Apprenez à configurer une instance de cluster de basculement avec le stockage NFS pour SQL Server sur Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 90240b383e19fefd254cd5f28b6d113b40b20d98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346313"
---
# <a name="configure-failover-cluster-instance---nfs---sql-server-on-linux"></a>Configurer l’instance de cluster de basculement - NFS - SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment configurer le stockage NFS pour une instance de cluster de basculement (FCI) sur Linux. 

NFS (Network File System) est une méthode courante pour partager des disques dans le monde Linux, mais pas dans Windows. À l’instar d’iSCSI, NFS peut être configuré sur un serveur ou une sorte d’appliance ou d’unité de stockage, à condition qu’il réponde aux exigences de stockage de SQL Server.

## <a name="important-nfs-server-information"></a>Informations importantes sur le serveur NFS

Le NFS d’hébergement source (un serveur Linux ou autre) doit utiliser/être conforme à la version 4.2 ou ultérieure. Des versions antérieures ne fonctionnent pas avec SQL Server sur Linux.

Quand vous configurez le ou les dossiers à partager sur le serveur NFS, assurez-vous qu’ils respectent les instructions générales suivantes :
- `rw` pour vous assurer que le dossier peut être lu et écrit
- `sync` pour garantir que les écritures sont garanties dans le dossier
- N’utilisez pas `no_root_squash` comme option ; cela est considéré comme un risque pour la sécurité
- Assurez-vous que le dossier dispose des droits complets (777) appliqués

Assurez-vous que vos normes de sécurité sont appliquées pour l’accès. Quand vous configurez le dossier, assurez-vous que seuls les serveurs participant à l’instance de cluster de basculement peuvent consulter le dossier NFS. Vous trouverez ci-dessous un exemple de /etc/exports modifié sur une solution NFS basée sur Linux dans laquelle le dossier est limité à FCIN1 et FCIN2.

![Vous trouverez ci-dessous une capture d’écran d’un exemple de /etc/exports modifié sur une solution NFS basée sur Linux dans laquelle le dossier est limité à FCIN1 et FCIN2.][1]

## <a name="instructions"></a>Instructions

1. Choisissez un des serveurs qui fera partie de la configuration de l’instance de cluster de basculement. Vous pouvez choisir n’importe lequel. 

2. Vérifiez que le serveur peut voir le ou les montages sur le serveur NFS.

    ```bash
    sudo showmount -e <IPAddressOfNFSServer>
    ```

    \<IPAddressOfNFSServer> est l’adresse IP du serveur NFS que vous allez utiliser.

3. Pour les bases de données système ou tout ce qui est stocké dans l’emplacement des données par défaut, procédez comme suit. Sinon, ignorez l’étape 4.
 
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

    \<TempDir> est le nom du dossier. L’exemple suivant crée un dossier nommé /var/opt/mssql/tmp.

    ```bash
    mkdir /var/opt/mssql/tmp
    ```

   * Copiez les données et les fichiers journaux de SQL Server dans le répertoire temporaire. Vous ne recevrez pas d’accusé de réception en cas de réussite.
    
    ```bash
    cp /var/opt/mssql/data/* <TempDir>
    ```

    \<TempDir> est le nom du dossier de l’étape précédente.

   * Vérifiez que les fichiers se trouvent dans le répertoire.

    ```bash
    ls TempDir
    ```

    \<TempDir> est le nom du dossier de l’étape d.

   * Supprimez les fichiers du répertoire de données SQL Server existant. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    rm - f /var/opt/mssql/data/*
    ```

   * Vérifiez que les fichiers ont été supprimés. 

    ```bash
    ls /var/opt/mssql/data
    ```
    
   * Tapez Quitter pour revenir à l’utilisateur racine.

   * Montez le partage SMB dans le dossier de données SQL Server. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    mount -t nfs4 <IPAddressOfNFSServer>:<FolderOnNFSServer> /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

    \<IPAddressOfNFSServer> est l’adresse IP du serveur NFS que vous allez utiliser 

    \<FolderOnNFSServer> est le nom du partage NFS. L’exemple de syntaxe suivant correspond aux informations NFS de l’étape 2.

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci1 /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

   * Vérifiez que le montage a réussi en émettant un montage sans commutateurs.

    ```bash
    mount
    ```

    ![Capture d’écran de la commande mount et de la réponse à la commande montrant l’absence de commutateurs.][2]

   * Basculez vers l’utilisateur mssql. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    su mssql
    ```

   * Copiez les fichiers à partir du répertoire temporaire /var/opt/mssql/data. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    cp /var/opt/mssql/tmp/* /var/opt/mssqldata
    ```

   * Vérifiez que les fichiers sont présents.

    ```bash
    ls /var/opt/mssql/data
    ```

   * Entrez Quitter pour ne pas être mssql 
    
   * Entrez Quitter pour ne pas être la racine

   * Démarrez SQL Server. Si tout a été copié correctement et que la sécurité est appliquée correctement, SQL Server doit s’afficher comme étant démarré.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ```
    
   * Créez une base de données pour vérifier que la sécurité est configurée correctement. L’exemple suivant explique le processus via Transact-SQL ; vous pouvez le faire via SSMS.
 
    ![CreateTestdatabase][3]

   * Arrêtez SQL Server et vérifiez qu’il est arrêté.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```

   * Si vous ne créez aucun autre montage NFS, démontez le partage. Si c’est le cas, ne démontez pas.

    ```bash
    sudo umount <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn>
    ```

    \<IPAddressOfNFSServer> est l’adresse IP du serveur NFS que vous allez utiliser

    \<FolderOnNFSServer> est le nom du partage NFS

    \<FolderMountedIn> est le dossier créé à l’étape précédente. 

4. Pour des éléments autres que les bases de données système, tels que les bases de données utilisateur ou les sauvegardes, procédez comme suit. Si vous utilisez uniquement l’emplacement par défaut, passez à l’étape 5.

   * Basculez pour être le superutilisateur. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    sudo -i
    ```

   * Créez un dossier qui sera utilisé par SQL Server. 

    ```bash
    mkdir <FolderName>
    ```

    \<FolderName> est le nom du dossier. Le chemin d’accès complet du dossier doit être spécifié s’il n’est pas à l’emplacement approprié. L’exemple suivant crée un dossier nommé /var/opt/mssql/userdata.

    ```bash
    mkdir /var/opt/mssql/userdata
    ```

   * Montez le partage NFS dans le dossier créé à l’étape précédente. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    Mount -t nfs4 <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn> -o nfsvers=4.2,timeo=14,intr
    ```

    \<IPAddressOfNFSServer> est l’adresse IP du serveur NFS que vous allez utiliser

    \<FolderOnNFSServer> est le nom du partage NFS

    \<FolderToMountIn> est le dossier créé à l’étape précédente. Voici un exemple. 

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci2 /var/opt/mssql/userdata -o nfsvers=4.2,timeo=14,intr
    ```

   * Vérifiez que le montage a réussi en émettant un montage sans commutateurs.
  
   * Tapez Quitter pour ne plus être le superutilisateur.

   * Pour tester, créez une base de données dans ce dossier. L’exemple suivant utilise sqlcmd pour créer une base de données, en changer le contexte, vérifier que les fichiers existent au niveau du système d’exploitation, puis supprime l’emplacement temporaire. Vous pouvez utiliser SSMS.

    ![Capture d’écran de la commande sqlcmd et de la réponse à la commande.][4]
 
   * Démonter le partage 

    ```bash
    sudo umount <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn>
    ```

    \<IPAddressOfNFSServer> est l’adresse IP du serveur NFS que vous allez utiliser
    
    \<FolderOnNFSServer> est le nom du partage NFS

    \<FolderMountedIn> est le dossier créé à l’étape précédente. Voici un exemple. 
 
5. Répétez les étapes pour l'autre ou les autres nœuds.


## <a name="next-steps"></a>Étapes suivantes

[Configurer l’instance de cluster de basculement - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/05-nfsacl.png
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/10-mountnoswitches.png
[3]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/20-createtestdatabase.png
[4]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/15-createtestdatabase.png
