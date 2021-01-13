---
description: Déplacer des bases de données système
title: Déplacer des bases de données système | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- moving system databases
- disaster recovery [SQL Server], moving database files
- database files [SQL Server], moving
- data files [SQL Server], moving
- tempdb database [SQL Server], moving
- system databases [SQL Server], moving
- scheduled disk maintenace [SQL Server]
- moving databases
- msdb database [SQL Server], moving
- moving database files
- relocating database files
- planned database relocations [SQL Server]
- master database [SQL Server], moving
- model database [SQL Server], moving
- Resource database [SQL Server]
- databases [SQL Server], moving
ms.assetid: 72bb62ee-9602-4f71-be51-c466c1670878
author: stevestein
ms.author: sstein
ms.openlocfilehash: c24a98684e87eb94a3cd9e100f203509789b7a0f
ms.sourcegitcommit: 4a813a0741502c56c0cd5ecaafafad2e857a9d7f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98031105"
---
# <a name="move-system-databases"></a>Déplacer des bases de données système
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique décrit comment déplacer des bases de données système dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le déplacement des bases de données système peut être utile dans les cas suivants :  
  
-   Récupération après défaillance. Par exemple, la base de données est en mode suspect ou a été fermée en raison d'une défaillance matérielle.  
  
-   Déplacement prévu.  
  
-   Déplacement en vue d'une maintenance de disque planifiée.  
  
 Les procédures ci-dessous s'appliquent au déplacement des fichiers de base de données au sein de la même instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour déplacer une base de données vers une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou vers un autre serveur, utilisez les opérations de [sauvegarde et restauration](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md) .  

 Les procédures de cette rubrique requièrent le nom logique des fichiers de base de données. Pour obtenir ce nom, interrogez la colonne name dans l’affichage catalogue [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) .  
  
> [!IMPORTANT]  
>  Si vous déplacez une base de données système et que vous recréez ultérieurement la base de données master, vous devez redéplacer la base de données système car l'opération de recréation installe toutes les bases de données système à leur emplacement par défaut.  

> [!IMPORTANT]  
>  Une fois les fichiers déplacés, le compte de service [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] doit être autorisé à y accéder dans le nouvel emplacement de dossier de fichiers. Pour plus d’informations, consultez [Configuration des autorisations du système de fichiers pour l’accès au moteur de base de données](../../database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access.md).
    
  
##  <a name="planned-relocation-and-scheduled-disk-maintenance-procedure"></a><a name="Planned"></a> Procédure de réadressage planifié et de maintenance de disque planifiée  
 Pour déplacer des données ou un fichier journal d'une base de données système dans le cadre d'un réadressage planifié ou d'une opération de maintenance planifiée, suivez la procédure ci-dessous. Cette procédure s'applique à toutes les bases de données système à l'exception des bases de données master et Resource.  
  
1.  Pour chaque fichier à déplacer, exécutez la commande suivante.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name , FILENAME = 'new_path\os_file_name' )  
    ```  
  
2.  Arrêtez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou éteignez le système pour que la maintenance ait lieu. Pour plus d'informations, consultez [Démarrer, arrêter, suspendre, reprendre, redémarrer les services SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Déplacez le ou les fichiers vers le nouvel emplacement et vérifiez que le compte de service [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] est toujours autorisé à y accéder.

4.  Redémarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou le serveur. Pour plus d'informations, consultez [Démarrer, arrêter, suspendre, reprendre, redémarrer les services SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
5.  Vérifiez le changement de fichier en exécutant la requête suivante.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
 Si la base de données msdb est déplacée et si l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configurée pour la [messagerie de base de données](../../relational-databases/database-mail/database-mail.md), effectuez ces opérations supplémentaires.  
  
1.  Assurez-vous que [!INCLUDE[ssSB](../../includes/sssb-md.md)] est activé pour la base de données msdb en exécutant la requête ci-dessous.  
  
    ```  
    SELECT is_broker_enabled   
    FROM sys.databases  
    WHERE name = N'msdb';  
    ```  
  
     Pour plus d’informations sur l’activation de [!INCLUDE[ssSB](../../includes/sssb-md.md)], consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
2.  Vérifiez le bon fonctionnement de la messagerie de base de données en envoyant un message électronique de test.  
  
##  <a name="failure-recovery-procedure"></a><a name="Failure"></a> Procédure de récupération après défaillance  
 Si un fichier doit être déplacé dans un nouvel emplacement en raison d'une défaillance matérielle, suivez la procédure décrite ci-dessous. Cette procédure s'applique à toutes les bases de données système à l'exception des bases de données master et Resource.  
  
> [!IMPORTANT]  
>  Si la base de données ne démarre pas – elle est en mode suspect ou dans un état de non récupération, seuls les membres du rôle fixe sysadmin peuvent déplacer le fichier.  
  
1.  Arrêtez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si elle est démarrée.  
  
2.  Démarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode de récupération de la base de données master uniquement en tapant une des commandes suivantes à l'invite de commandes. Les paramètres spécifiés dans ces commandes respectent la casse. Les commandes échouent lorsque les paramètres ne sont pas spécifiés comme indiqué.  
  
    -   Dans le cas d'une instance par défaut (MSSQLSERVER), exécutez la commande ci-dessous :  
  
        ```  
        NET START MSSQLSERVER /f /T3608
        ```  
  
    -   Dans le cas d'une instance nommée, exécutez la commande ci-dessous :  
  
        ```  
        NET START MSSQL$instancename /f /T3608
        ```  
  
     Pour plus d'informations, consultez [Démarrer, arrêter, suspendre, reprendre, redémarrer les services SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Pour chaque fichier à déplacer, utilisez les commandes **sqlcmd** ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour exécuter l’instruction suivante.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE( NAME = logical_name , FILENAME = 'new_path\os_file_name' )  
    ```  
  
     Pour plus d’informations sur l’utilisation de l’utilitaire **sqlcmd** , consultez [Utiliser l’utilitaire sqlcmd](../../ssms/scripting/sqlcmd-use-the-utility.md).  
  
4.  Quittez l’utilitaire **sqlcmd** ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
5.  Arrêtez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par exemple, exécutez `NET STOP MSSQLSERVER`.  
  
6.  Déplacez le ou les fichiers vers le nouvel emplacement.  
  
7.  Redémarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par exemple, exécutez `NET START MSSQLSERVER`.  
  
8.  Vérifiez le changement de fichier en exécutant la requête suivante.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
##  <a name="moving-the-master-database"></a><a name="master"></a> Déplacement de la base de données master  
 Pour déplacer la base de données master, procédez comme suit.  
  
1.  Dans le menu **Démarrer** , pointez successivement sur **Tous les programmes**, sur **Microsoft SQL Server** et sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
2.  Dans le nœud **Services SQL Server** , cliquez avec le bouton droit sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (par exemple, **SQL Server (MSSQLSERVER)** ), puis cliquez sur **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés de SQL Server (** _nom_instance_ **)** , cliquez sur l’onglet **Paramètres de démarrage** .  
  
4.  Dans la zone **Paramètres existants**, sélectionnez le paramètre -d. Dans la zone **Spécifiez un paramètre de démarrage**, remplacez le paramètre par le nouveau chemin du fichier de *données* master. Cliquez sur **Mettre à jour** pour enregistrer les modifications.
  
5.  Dans la zone **Paramètres existants**, sélectionnez le paramètre -l. Dans la zone **Spécifiez un paramètre de démarrage**, remplacez le paramètre par le nouveau chemin du fichier *journal* master. Cliquez sur **Mettre à jour** pour enregistrer les modifications.

     La valeur du paramètre pour le fichier de données doit suivre le paramètre `-d` et la valeur pour le fichier journal doit suivre le paramètre `-l` . L’exemple suivant montre les valeurs des paramètres pour l’emplacement par défaut des fichiers de données de la base de données MASTER.  
  
     `-dC:\Program Files\Microsoft SQL Server\MSSQL<version>.MSSQLSERVER\MSSQL\DATA\master.mdf`  
  
     `-lC:\Program Files\Microsoft SQL Server\MSSQL<version>.MSSQLSERVER\MSSQL\DATA\mastlog.ldf`  
  
     Si le nouvel emplacement planifié pour les fichiers de données de la base de données MASTER correspond à `E:\SQLData`, les valeurs des paramètres sont modifiées comme suit :  
  
     `-dE:\SQLData\master.mdf`  
  
     `-lE:\SQLData\mastlog.ldf`  

6.  Cliquez sur **OK** pour enregistrer les changements de façon définitive et fermez la boîte de dialogue **Propriétés SQL Server (** **_instance_name_)** .

7.  Arrêtez l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cliquant avec le bouton droit sur le nom d’instance et en choisissant **Arrêter**.  
  
8.  Déplacez les fichiers master.mdf et mastlog.ldf vers le nouvel emplacement.  
  
9.  Redémarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
10. Vérifiez que la modification des fichiers a bien eu lieu pour la base de données master en exécutant la requête ci-dessous.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID('master');  
    GO  
    ```  

11. À ce stade, SQL Server doit fonctionner normalement. Toutefois Microsoft recommande d’ajuster également l’entrée de registre sur `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\instance_ID\Setup`, où *instance_ID* est similaire à `MSSQL13.MSSQLSERVER`. Dans cette ruche, modifiez la valeur `SQLDataRoot` en indiquant le nouveau chemin d’accès. L’échec de la mise à jour du registre peut entraîner l’échec de la mise à jour corrective et de la mise à niveau.

  
##  <a name="moving-the-resource-database"></a><a name="Resource"></a> Déplacement de la base de données Resources  
 L'emplacement de la base de données est \<*drive*>:\Program Files\Microsoft SQL Server\MSSQL\<version>.\<*instance_name*><>\MSSQL\Binn\\\ . La base de données ne peut pas être déplacée.  
  
##  <a name="follow-up-after-moving-all-system-databases"></a><a name="Follow"></a> Suivi : Après le déplacement de toutes les bases de données système  
 Si vous avez déplacé toutes les bases de données système vers un même lecteur ou volume ou vers un autre serveur utilisant une lettre de lecteur différente, effectuez les mises à jour suivantes.  
  
-   Modifiez le chemin d'accès du journal de l'Agent SQL Server. Si vous ne mettez pas à jour ce chemin d'accès, l'Agent SQL Server ne démarre pas.  
  
-   Modifiez l'emplacement par défaut de la base de données. La création d'une base de données peut échouer si la lettre de lecteur et le chemin d'accès spécifiés comme emplacement par défaut n'existent pas.  
  
#### <a name="change-the-sql-server-agent-log-path"></a>Modifier le chemin d'accès du journal de l'Agent SQL Server  
  
1.  Dans SQL Server Management Studio, dans l'Explorateur d'objets, développez **Agent SQL Server**.  
  
2.  Cliquez avec le bouton droit sur **Journaux d’erreurs** , puis cliquez sur **Configurer**.  
  
3.  Dans la boîte de dialogue **Configurer les journaux d'erreurs de l'Agent SQL Server** , spécifiez le nouvel emplacement du fichier SQLAGENT.OUT. L'emplacement par défaut est C:\Program Files\Microsoft SQL Server\MSSQL12\<version>.<nom_instance>\MSSQL\Log\\\.  
  
#### <a name="change-the-database-default-location"></a>Modifier l'emplacement par défaut de la base de données  
  
1.  Dans SQL Server Management Studio, dans l’Explorateur d’objets, cliquez avec le bouton droit sur le serveur SQL Server, puis cliquez sur **Propriétés**.  
  
2.  Dans la boîte de dialogue **Propriétés du serveur** , sélectionnez **Paramètres de base de données**.  
  
3.  Sous **Emplacements de la base de données par défaut**, accédez au nouvel emplacement des fichiers de données et des fichiers journaux.  
  
4.  Arrêtez et démarrez le service SQL Server pour terminer la modification.  
  
##  <a name="examples"></a><a name="Examples"></a> Exemples  
  
### <a name="a-moving-the-tempdb-database"></a>R. Déplacement de la base de données tempdb  
 Dans l'exemple suivant, les fichiers de données et les fichiers journaux de la base de données `tempdb` sont déplacés vers un nouvel emplacement dans le cadre d'une opération planifiée.  
  
> [!NOTE]  
>  Dans la mesure où la base de données tempdb est recréée à chaque démarrage de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous n'avez pas à déplacer physiquement les fichiers de données et les fichiers journaux. Les fichiers sont créés au nouvel emplacement lorsque le service est redémarré à l'étape 3. Tant que le service n'a pas redémarré, tempdb continue à utiliser les fichiers de données et les fichiers journaux situés à l'emplacement existant.  
  
1.  Déterminez les noms de fichiers logiques de la base de données `tempdb` et leur emplacement actuel sur le disque.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'tempdb');  
    GO  
    ```  
  
2.  Modifiez l'emplacement de chaque fichier à l'aide de `ALTER DATABASE`.  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE tempdb   
    MODIFY FILE (NAME = tempdev, FILENAME = 'E:\SQLData\tempdb.mdf');  
    GO  
    ALTER DATABASE tempdb   
    MODIFY FILE (NAME = templog, FILENAME = 'F:\SQLLog\templog.ldf');  
    GO  
    ```  
  
3.  Arrêtez et redémarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
4.  Vérifiez que la modification des fichiers a bien eu lieu.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'tempdb');  
    ```  
  
5.  Supprimez les fichiers `tempdb.mdf` et `templog.ldf` de l'emplacement d'origine.  
  
## <a name="see-also"></a>Voir aussi  
 [Base de données Resource](../../relational-databases/databases/resource-database.md)   
 [Base de données tempdb](../../relational-databases/databases/tempdb-database.md)   
 [Base de données master](../../relational-databases/databases/master-database.md)   
 [Base de données msdb](../../relational-databases/databases/msdb-database.md)   
 [Base de données model](../../relational-databases/databases/model-database.md)   
 [Déplacer des bases de données utilisateur](../../relational-databases/databases/move-user-databases.md)   
 [Déplacer des fichiers de bases de données](../../relational-databases/databases/move-database-files.md)   
 [Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Reconstruire des bases de données système](../../relational-databases/databases/rebuild-system-databases.md)  
  
