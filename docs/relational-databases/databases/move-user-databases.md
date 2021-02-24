---
description: Déplacer des bases de données utilisateur
title: Déplacer des bases de données utilisateur | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- disaster recovery [SQL Server], moving database files
- database files [SQL Server], moving
- data files [SQL Server], moving
- editions [SQL Server], moving databases between
- moving full-text catalogs
- scheduled disk maintenace [SQL Server]
- moving databases
- full-text catalogs [SQL Server], moving
- moving database files
- moving user databases
- relocating database files
- planned database relocations [SQL Server]
- databases [SQL Server], moving
ms.assetid: ad9a4e92-13fb-457d-996a-66ffc2d55b79
author: stevestein
ms.author: sstein
ms.openlocfilehash: 64b8416375cdc61c43e8799b33891096858a79f7
ms.sourcegitcommit: 059722ff78a6061b801807416b312ae9f721ec7d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100636712"
---
# <a name="move-user-databases"></a>Déplacer des bases de données utilisateur
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez déplacer les fichiers de données, les fichiers journaux et les fichiers de catalogues de texte intégral d’une base de données utilisateur vers un nouvel emplacement, en spécifiant le nouvel emplacement de fichier dans la clause FILENAME de l’instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md). Cette méthode s'applique au déplacement des fichiers de base de données dans la même instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour déplacer une base de données vers une autre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou vers un autre serveur, utilisez les opérations de [sauvegarde et de restauration](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md) ou les [opérations de détachement et d'attachement](../../relational-databases/databases/move-a-database-using-detach-and-attach-transact-sql.md).  
  
## <a name="considerations"></a>Considérations  
 Lorsque vous déplacez une base de données sur une autre instance de serveur, pour garantir une expérience cohérente aux utilisateurs et aux applications, vous devrez peut-être recréer tout ou partie des métadonnées de la base de données. Pour plus d’informations, consultez [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
 Certaines fonctionnalités du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] modifient la façon dont le [!INCLUDE[ssDE](../../includes/ssde-md.md)] stocke les informations dans les fichiers de base de données. Ces fonctionnalités sont limitées à des éditions spécifiques de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une base de données qui contient ces fonctionnalités ne peut pas être déplacée vers une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne les prend pas en charge. Utilisez la vue de gestion dynamique sys.dm_db_persisted_sku_features pour répertorier toutes les fonctions spécifiques à l'édition activées dans la base de données actuelle.  
  
 Les procédures de cette rubrique requièrent le nom logique des fichiers de base de données. Pour obtenir ce nom, interrogez la colonne name dans l’affichage catalogue [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) .  
  
 À partir de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], les catalogues de texte intégral sont intégrés dans la base de données plutôt que stockés dans le système de fichiers. Les catalogues de texte intégral se déplacent automatiquement lorsque vous déplacez une base de données.  
  
## <a name="planned-relocation-procedure"></a>Procédure de déplacement planifié  
 Pour déplacer un fichier journal ou un fichier de données dans le cadre d'un déplacement planifié, procédez comme suit :  
  
1.  Pour chaque fichier à déplacer, exécutez la commande suivante.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name, FILENAME = 'new_path\os_file_name' );  
    ```  
  
2.  Exécutez la commande suivante.  
  
    ```  
    ALTER DATABASE database_name SET OFFLINE;  
    ```  
  
3.  Déplacez le ou les fichiers vers le nouvel emplacement.  
  
4.  Exécutez la commande suivante.  
  
    ```  
    ALTER DATABASE database_name SET ONLINE;  
    ```  
  
5.  Vérifiez le changement de fichier en exécutant la requête suivante.  

    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="relocation-for-scheduled-disk-maintenance"></a>Déplacement en vue d'une maintenance de disque planifiée  
 Pour déplacer un fichier dans le cadre d'un processus de maintenance de disque planifié, procédez comme suit :  
  
1.  Pour chaque fichier à déplacer, exécutez la commande suivante.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name , FILENAME = 'new_path\os_file_name' );  
    ```  
  
2.  Arrêtez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou éteignez le système pour que la maintenance ait lieu. Pour plus d'informations, consultez [Démarrer, arrêter, suspendre, reprendre, redémarrer les services SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Déplacez le ou les fichiers vers le nouvel emplacement.  
  
4.  Redémarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou le serveur. Pour plus d'informations, consultez [Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
5.  Vérifiez le changement de fichier en exécutant la requête suivante.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="failure-recovery-procedure"></a>Procédure de récupération après défaillance  
 Si un fichier doit être déplacé dans un nouvel emplacement en raison d'une défaillance matérielle, suivez la procédure décrite ci-dessous.  
  
> [!IMPORTANT]  
>  Si la base de données ne démarre pas – elle est en mode suspect ou dans un état de non récupération, seuls les membres du rôle fixe sysadmin peuvent déplacer le fichier.  
  
1.  Arrêtez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si elle est démarrée.  
  
2.  Démarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode de récupération de la base de données master uniquement en tapant une des commandes suivantes à l'invite de commandes.  
  
    -   Dans le cas d'une instance par défaut (MSSQLSERVER), exécutez la commande ci-dessous.  
  
        ```  
        NET START MSSQLSERVER /f /T3608  
        ```  
  
    -   Dans le cas d'une instance nommée, exécutez la commande ci-dessous.  
  
        ```  
        NET START MSSQL$instancename /f /T3608  
        ```  
  
     Pour plus d'informations, consultez [Démarrer, arrêter, suspendre, reprendre, redémarrer les services SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Pour chaque fichier à déplacer, utilisez les commandes **sqlcmd** ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour exécuter l’instruction suivante.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE( NAME = logical_name , FILENAME = 'new_path\os_file_name' );  
    ```  
  
     Pour plus d’informations sur l’utilisation de l’utilitaire **sqlcmd** , consultez [Utiliser l’utilitaire sqlcmd](../../ssms/scripting/sqlcmd-use-the-utility.md).  
  
4.  Quittez l’utilitaire **sqlcmd** ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
5.  Arrêtez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
6.  Déplacez le ou les fichiers vers le nouvel emplacement.  
  
7.  Démarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par exemple, exécutez : `NET START MSSQLSERVER`.  
  
8.  Vérifiez le changement de fichier en exécutant la requête suivante.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant déplace le fichier journal [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] vers un nouvel emplacement dans le cadre d'un déplacement planifié.  
  
```  
USE master;  
GO  
-- Return the logical file name.  
SELECT name, physical_name AS CurrentLocation, state_desc  
FROM sys.master_files  
WHERE database_id = DB_ID(N'AdventureWorks2012')  
    AND type_desc = N'LOG';  
GO  
ALTER DATABASE AdventureWorks2012 SET OFFLINE;  
GO  
-- Physically move the file to a new location.  
-- In the following statement, modify the path specified in FILENAME to  
-- the new location of the file on your server.  
ALTER DATABASE AdventureWorks2012   
    MODIFY FILE ( NAME = AdventureWorks2012_Log,   
                  FILENAME = 'C:\NewLoc\AdventureWorks2012_Log.ldf');  
GO  
ALTER DATABASE AdventureWorks2012 SET ONLINE;  
GO  
--Verify the new location.  
SELECT name, physical_name AS CurrentLocation, state_desc  
FROM sys.master_files  
WHERE database_id = DB_ID(N'AdventureWorks2012')  
    AND type_desc = N'LOG';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [Déplacer des bases de données système](../../relational-databases/databases/move-system-databases.md)   
 [Déplacer des fichiers de bases de données](../../relational-databases/databases/move-database-files.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
