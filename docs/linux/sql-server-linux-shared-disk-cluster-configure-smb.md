---
title: Configurer le stockage SMB pour une instance de cluster de basculement - SQL Server sur Linux
description: Apprenez à configurer une instance de cluster de basculement avec le stockage SMB pour SQL Server sur Linux.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 6cc84164c2829d2cf6d5e66f40fd13921fdb105b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346324"
---
# <a name="configure-failover-cluster-instance---smb---sql-server-on-linux"></a>Configurer l’instance de cluster de basculement - SMB - SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment configurer le stockage SMB pour une instance de cluster de basculement (FCI) sur Linux. 
 
Dans le monde non Windows, SMB est souvent appelé partage CIFS (Common Internet File System) et implémenté via Samba. Dans le monde de Windows, l’accès à un partage SMB est effectué de la façon suivante : \\SERVERNAME\SHARENAME. Pour les installations SQL Server basées sur Linux, le partage SMB doit être monté en tant que dossier.

## <a name="important-source-and-server-information"></a>Informations importantes sur la source et le serveur

Voici quelques conseils et remarques pour l’utilisation réussie de SMB :
- Le partage SMB peut être sur Windows, Linux ou même à partir d’une appliance, à condition qu’il utilise SMB 3.0 ou une version ultérieure. Pour plus d’informations sur Samba et SMB 3.0, consultez [SMB 3.0](https://wiki.samba.org/index.php/Samba3/SMB2#SMB_3.0) pour voir si votre implémentation Samba est conforme à SMB 3.0.
- Le partage SMB doit être hautement disponible.
- La sécurité doit être correctement définie sur le partage SMB. Voici un exemple de /etc/samba/smb.conf, où SQLData1 est le nom du partage.

![Capture d’écran montrant que SQLData1 est le nom du partage.][1]

## <a name="instructions"></a>Instructions

1. Choisissez un des serveurs qui fera partie de la configuration de l’instance de cluster de basculement. Vous pouvez choisir n’importe lequel.
   
1. Obtenez des informations sur l'utilisateur mssql.
   
   ```bash
    sudo id mssql
   ```
   
   Notez les UID, GID et groupes. 
   
1. Exécutez `sudo smbclient -L //NameOrIP/ShareName -U User`.
   
   \<NameOrIP> est le nom DNS ou l’adresse IP du serveur hébergeant le partage SMB.
   
   \<ShareName> est le nom du partage SMB. 
   
1. Pour les bases de données système ou tout ce qui est stocké dans l’emplacement des données par défaut, procédez comme suit. Sinon, ignorez l’étape 5. 
   
   1. Assurez-vous que SQL Server est arrêté sur le serveur sur lequel vous travaillez.
      ```bash
      sudo systemctl stop mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Basculez entièrement pour être le superutilisateur. Vous ne recevrez pas d’accusé de réception en cas de réussite.
      
      ```bash
      sudo -i
      ```
      
   1. Basculez pour être l’utilisateur mssql. Vous ne recevrez pas d’accusé de réception en cas de réussite.
      
      ```bash
      su mssql
      ```
      
   1. Créez un répertoire temporaire pour stocker les données et les fichiers journaux SQL Server. Vous ne recevrez pas d’accusé de réception en cas de réussite.
      
      ```bash
      mkdir <TempDir>
      ```
      
      \<TempDir> est le nom du dossier. L’exemple suivant crée un dossier nommé /var/opt/mssql/tmp.
      
      ```bash
      mkdir /var/opt/mssql/tmp
      ```
      
   1. Copiez les données et les fichiers journaux de SQL Server dans le répertoire temporaire. Vous ne recevrez pas d’accusé de réception en cas de réussite.
      
      ```bash
      cp /var/opt/mssql/data/* <TempDir>
      ```
      
      \<TempDir> est le nom du dossier de l’étape précédente.
      
   1. Vérifiez que les fichiers se trouvent dans le répertoire.
      
      ```bash
      ls <TempDir>
      ```
      
      \<TempDir> est le nom du dossier de l’étape d.
      
   1. Supprimez les fichiers du répertoire de données SQL Server existant. Vous ne recevrez pas d’accusé de réception en cas de réussite.
      
      ```bash
      rm - f /var/opt/mssql/data/*
      ```
      
   1. Vérifiez que les fichiers ont été supprimés. 
      
      ```bash
      ls /var/opt/mssql/data
      ```
      
   1. Tapez Quitter pour revenir à l’utilisateur racine.
      
   1. Montez le partage SMB dans le dossier de données de SQL Server. Vous ne recevrez pas d’accusé de réception en cas de réussite. Cet exemple illustre la syntaxe de connexion à un partage SMB 3.0 basé sur Windows Server.
      
      ```bash
      Mount -t cifs //<ServerName>/<ShareName> /var/opt/mssql/data -o vers=3.0,username=<UserName>,password=<Password>,domain=<domain>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777
      ```
      
      \<ServerName> est le nom du serveur avec le partage SMB
      
      \<ShareName> est le nom du partage
      
      \<UserName> est le nom de l’utilisateur pour accéder au partage
      
      \<Password> est le mot de passe de l’utilisateur
      
      \<domain> est le nom d’Active Directory
      
      \<mssqlUID> est l’UID de l’utilisateur mssql 
      
      \<mssqlGID> est le GID de l’utilisateur mssql
      
   1. Vérifiez que le montage a réussi en émettant un montage sans commutateurs.
      
      ```bash
      mount
      ```
      
   1. Basculez vers l’utilisateur mssql. Vous ne recevrez pas d’accusé de réception en cas de réussite.
      
      ```bash
      su mssql
      ```
      
   1. Copiez les fichiers à partir du répertoire temporaire /var/opt/mssql/data. Vous ne recevrez pas d’accusé de réception en cas de réussite.
      
      ```bash
      cp /var/opt/mssql/tmp/* /var/opt/mssql/data
      ```
      
   1. Vérifiez que les fichiers sont présents.
      
      ```bash
      ls /var/opt/mssql/data
      ```
      
   1. Entrez Quitter pour ne pas être mssql 
      
   1. Entrez Quitter pour ne pas être la racine
   
   1. Démarrez SQL Server. Si tout a été copié correctement et que la sécurité est appliquée correctement, SQL Server doit s’afficher comme étant démarré.
      
      ```bash
      sudo systemctl start mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Pour effectuer des tests supplémentaires, créez une base de données pour vous assurer que les autorisations sont correctes. L’exemple suivant utilise Transact-SQL ; vous pouvez utiliser SSMS.
      
      ![10_testcreatedb][2] 
      
   1. Arrêtez SQL Server et vérifiez qu’il est arrêté. Si vous envisagez d’ajouter ou de tester d’autres disques, n’arrêtez pas SQL Server tant que ceux-ci n’ont pas été ajoutés ni testés.
      
      ```bash
      sudo systemctl stop mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Démontez le partage uniquement si vous avez terminé. Si ce n’est pas le cas, démontez-le après avoir terminé de tester ou d’ajouter des disques supplémentaires.
      
      ```bash
      sudo umount //<IPAddressorServerName>/<ShareName /<FolderMountedIn>
      ```
      
      \<IPAddressOrServerName> est l’adresse IP ou le nom de l’hôte SMB
      
      \<ShareName> est le nom du partage
      
      \<FolderMountedIn> est le nom du dossier dans lequel SMB est monté
      
5. Pour des éléments autres que les bases de données système, tels que les bases de données utilisateur ou les sauvegardes, procédez comme suit. Si vous utilisez uniquement l’emplacement par défaut, passez à l’étape 14.
   
   1. Basculez pour être le superutilisateur. Vous ne recevrez pas d’accusé de réception en cas de réussite.
      
      ```bash
      sudo -i
      ```
      
   1. Créez un dossier qui sera utilisé par SQL Server. 
      
      ```bash
      mkdir <FolderName>
      ```
      
      \<FolderName> est le nom du dossier. Le chemin d’accès complet du dossier doit être spécifié s’il n’est pas à l’emplacement approprié. L’exemple suivant crée un dossier nommé /var/opt/mssql/userdata.
      
      ```bash
      mkdir /var/opt/mssql/userdata
      ```
      
   1. Montez le partage SMB dans le dossier de données de SQL Server. Vous ne recevrez pas d’accusé de réception en cas de réussite. Cet exemple illustre la syntaxe de connexion à un partage SMB 3.0 basé sur Samba.
      
      ```bash
      Mount -t cifs //<ServerName>/<ShareName> <FolderName> -o vers=3.0,username=<UserName>,password=<Password>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777
      ```
      
      \<ServerName> est le nom du serveur avec le partage SMB
      
      \<ShareName> est le nom du partage
      
      \<FolderName> est le nom du dossier créé à la dernière étape  
      
      \<UserName> est le nom de l’utilisateur pour accéder au partage
      
      \<Password> est le mot de passe de l’utilisateur
      
      \<mssqlUID> est l’UID de l’utilisateur mssql
      
      \<mssqlGID> est le GID de l’utilisateur mssql
      
   1. Vérifiez que le montage a réussi en émettant un montage sans commutateurs.
   
   1. Tapez Quitter pour ne plus être le superutilisateur.
   
   1. Pour tester, créez une base de données dans ce dossier. L’exemple suivant utilise sqlcmd pour créer une base de données, en changer le contexte, vérifier que les fichiers existent au niveau du système d’exploitation, puis supprime l’emplacement temporaire. Vous pouvez utiliser SSMS.
   
   1. Démonter le partage 
      
      ```bash
      sudo umount //<IPAddressorServerName>/<ShareName> /<FolderMountedIn>
      ```
      
      \<IPAddressOrServerName> est l’adresse IP ou le nom de l’hôte SMB
      
      \<ShareName> est le nom du partage
      
      \<FolderMountedIn> est le nom du dossier dans lequel SMB est monté
   
1. Répétez les étapes pour l'autre ou les autres nœuds.

Vous êtes maintenant prêt à configurer l’instance de cluster de basculement (FCI).

## <a name="next-steps"></a>Étapes suivantes

[Configurer l’instance de cluster de basculement - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-smb/05-smbsource.png 
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-smb/10-testcreatedb.png 
