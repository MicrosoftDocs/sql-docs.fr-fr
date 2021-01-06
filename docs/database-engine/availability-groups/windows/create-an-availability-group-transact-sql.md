---
title: Créer un groupe de disponibilité avec T-SQL (Transact-SQL)
description: Utilisez Transact-SQL pour créer et configurer un groupe de disponibilité sur des instances de SQL Server 2019 (15.x) sur lesquelles la fonctionnalité de groupes de disponibilité Always On est activée.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], creating
ms.assetid: 8b0a6301-8b79-4415-b608-b40876f30066
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7223b0d8b283caf6300e2be056d399e45e637350
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643296"
---
# <a name="create-an-always-on-availability-group-using-transact-sql-t-sql"></a>Créer un groupe de disponibilité Always On à l’aide de Transact-SQL (T-SQL)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment utiliser [!INCLUDE[tsql](../../../includes/tsql-md.md)] pour créer et configurer un groupe de disponibilité sur des instances de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] sur lesquelles la fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] est activée. Un *groupe de disponibilité* définit un jeu de bases de données utilisateur qui basculent en tant qu'unité unique et un jeu de partenaires de basculement, appelés *réplicas de disponibilité*, qui prennent en charge le basculement.  
  
> [!NOTE]  
>  Pour obtenir une présentation des groupes de disponibilité, consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
> [!NOTE]  
>  En guise d'alternative à [!INCLUDE[tsql](../../../includes/tsql-md.md)], vous pouvez utiliser l'Assistant Création d'un groupe de disponibilité ou les applets de commande PowerShell [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Utiliser l’Assistant Groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md), [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)ou [Créer un groupe de disponibilité &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md).  

  
## <a name="prerequisites-restrictions-and-recommendations"></a><a name="PrerequisitesRestrictions"></a> Conditions préalables requises, restrictions et recommandations  
  
-   Avant de créer un groupe de disponibilité, vérifiez que les instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui hébergent les réplicas de disponibilité résident sur des nœuds WSFC (Windows Server Failover Clustering) différents au sein du même clustering de basculement WSFC. Vérifiez également que chaque instance du serveur répond à toutes les autres conditions requises relatives à [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Pour plus d’informations, nous vous conseillons vivement de lire la rubrique [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l’appartenance au rôle serveur fixe **sysadmin** et l’autorisation de serveur CREATE AVAILABILITY GROUP, l’autorisation ALTER ANY AVAILABILITY GROUP ou l’autorisation CONTROL SERVER.  
  
##  <a name="using-transact-sql-to-create-and-configure-an-availability-group"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL pour créer et configurer un groupe de disponibilité 

###  <a name="summary-of-tasks-and-corresponding-transact-sql-statements"></a><a name="SummaryTsqlStatements"></a> Résumé des tâches et instructions Transact-SQL correspondantes  
 Le tableau suivant répertorie les tâches de base impliquées dans la création et la configuration d'un groupe de disponibilité et indique les instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] à utiliser pour ces tâches. Les tâches [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] doivent être effectuées dans la séquence dans laquelle elles sont présentées dans le tableau.  
  
|Tâche|Instruction(s) Transact-SQL|Où effectuer la tâche **&#42;**|  
|----------|----------------------------------|---------------------------------|  
|Créer le point de terminaison de mise en miroir de bases de données (une fois par instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] )|[CREATE ENDPOINT](../../../t-sql/statements/create-endpoint-transact-sql.md) *endpointName* ... FOR DATABASE_MIRRORING|Exécutez sur chaque instance de serveur dans laquelle le point de terminaison de mise en miroir de bases de données est manquant.|  
|Créer un groupe de disponibilité|[CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)|Exécutez sur l'instance de serveur qui hébergera le réplica principal initial.|  
|Joindre le réplica secondaire au groupe de disponibilité|[ALTER AVAILABILITY GROUP](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md) *nom_groupe* JOIN|Exécutez sur chaque instance de serveur qui héberge un réplica secondaire.|  
|Préparer la base de données secondaire|[BACKUP](../../../t-sql/statements/backup-transact-sql.md) et [RESTORE](../../../t-sql/statements/restore-statements-transact-sql.md).|Créez des sauvegardes sur l'instance de serveur qui héberge le réplica principal.<br /><br /> Restaurez les sauvegardes sur chaque instance de serveur qui héberge un réplica secondaire, à l'aide de RESTORE WITH NORECOVERY.|  
|Démarrer la synchronisation des données en joignant chaque base de données secondaire au groupe de disponibilité|[ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) *nom_base_de_données* SET HADR AVAILABILITY GROUP = *nom_groupe*|Exécutez sur chaque instance de serveur qui héberge un réplica secondaire.|  
  
 *Pour effectuer une tâche donnée, connectez-vous aux instances de serveur indiquées.   
 
### <a name="using-transact-sql"></a>Utilisation de Transact-SQL 
> [!NOTE]  
>  Pour avoir un exemple de procédure de configuration contenant des exemples de code de chacune de ces instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)], consultez [Exemple : Configuration d’un groupe de disponibilité qui utilise l’authentification Windows](#ExampleConfigAGWinAuth).  
  
1.  Connectez-vous à l'instance de serveur qui hébergera le réplica principal.  
  
2.  Créez le groupe de disponibilité en utilisant l’instruction [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
3.  Joignez le nouveau réplica secondaire au groupe de disponibilité. Pour plus d’informations, consultez [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
4.  Pour chaque base de données dans le groupe de disponibilité, créez une base de données secondaire en restaurant des sauvegardes récentes de la base de données primaire, à l'aide de RESTORE WITH NORECOVERY. Pour plus d’informations, consultez [Exemple : Configuration d’un groupe de disponibilité à l’aide de l’authentification Windows (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md), en commençant par l’étape de restauration de la sauvegarde de base de données.  
  
5.  Joignez chaque nouvelle base de données secondaire au groupe de disponibilité. Pour plus d’informations, consultez [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
##  <a name="example-configuring-an-availability-group-that-uses-windows-authentication"></a><a name="ExampleConfigAGWinAuth"></a> Exemple : Configuration d’un groupe de disponibilité qui utilise l’authentification Windows  
 Cet exemple crée un exemple de procédure de configuration [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] qui utilise [!INCLUDE[tsql](../../../includes/tsql-md.md)] pour installer des points de terminaison de mise en miroir de bases de données qui utilisent l'authentification Windows, et créer et configurer un groupe de disponibilité et ses bases de données secondaires.  
  
 Cet exemple contient les sections suivantes :  
  
-   [Conditions préalables requises pour l'utilisation de l'exemple de procédure de configuration](#PrerequisitesForExample)  
  
-   [Exemple de procédure de configuration](#SampleProcedure)  
  
-   [Exemple de code complet pour l'exemple de procédure de configuration](#CompleteCodeExample)  
  
###  <a name="prerequisites-for-using-the-sample-configuration-procedure"></a><a name="PrerequisitesForExample"></a> Conditions préalables requises pour l'utilisation de l'exemple de procédure de configuration  
 Cet exemple de procédure présente les conditions suivantes :  
  
-   Les instances de serveur doivent prendre en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Pour plus d’informations, consultez [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
-   Deux exemples de bases de données, *MyDb1* et *MyDb2*, doivent exister sur l'instance de serveur qui hébergera le réplica principal. Les exemples de code suivants créent et configurent ces deux bases de données et créent une sauvegarde complète de chacune d'elles. Exécutez ces exemples de code sur l'instance de serveur sur laquelle vous envisagez de créer l'exemple de groupe de disponibilité. Cette instance de serveur hébergera le réplica principal initial de l'exemple de groupe de disponibilité.  
  
    1.  L'exemple [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivant crée ces bases de données et les modifie pour utiliser le mode de récupération complète :  
  
        ```sql  
        -- Create sample databases:  
        CREATE DATABASE MyDb1;  
        GO  
        ALTER DATABASE MyDb1 SET RECOVERY FULL;  
        GO  
  
        CREATE DATABASE MyDb2;  
        GO  
        ALTER DATABASE MyDb2 SET RECOVERY FULL;  
        GO  
        ```  
  
    2.  L'exemple de code suivant crée une sauvegarde complète des bases de données *MyDb1* et *MyDb2*. Cet exemple de code utilise un partage de sauvegarde fictif, \\\\*FILESERVER*\\*SQLbackups*.  
  
        ```sql  
        -- Backup sample databases:  
        BACKUP DATABASE MyDb1   
        TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
            WITH FORMAT;  
        GO  
  
        BACKUP DATABASE MyDb2   
        TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
            WITH FORMAT;  
        GO  
        ```  
  
 [&#91;Débutdel’exemple&#93;](#ExampleConfigAGWinAuth)  
  
###  <a name="sample-configuration-procedure"></a><a name="SampleProcedure"></a> Exemple de procédure de configuration  
 Dans cet exemple de configuration, le réplica de disponibilité sera créé sur deux instances de serveur autonomes dont les comptes de service s'exécutent sous des domaines différents, mais approuvés (`DOMAIN1` et `DOMAIN2`).  
  
 Le tableau ci-dessous récapitule les valeurs utilisées dans cet exemple de configuration.  
  
|Rôle initial|Système|Instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] hôte|  
|------------------|------------|---------------------------------------------|  
|Principal|`COMPUTER01`|`AgHostInstance`|  
|Secondary|`COMPUTER02`|Instance par défaut.|  
  
1.  Créez un point de terminaison de mise en miroir de bases de données nommé *dbm_endpoint* sur l’instance de serveur sur laquelle vous envisagez de créer le groupe de disponibilité (il s’agit d’une instance nommée `AgHostInstance` sur `COMPUTER01`). Ce point de terminaison utilise le port 7022. Notez que l'instance de serveur sur laquelle vous créez le groupe de disponibilité hébergera le réplica principal.  
  
    ```sql  
    -- Create endpoint on server instance that hosts the primary replica:  
    CREATE ENDPOINT dbm_endpoint  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL);  
    GO  
    ```  
  
2.  Créez un point de terminaison *dbm_endpoint* sur l’instance de serveur qui hébergera le réplica secondaire (il s’agit de l’instance de serveur par défaut sur `COMPUTER02`). Ce point de terminaison utilise le port 5022.  
  
    ```sql  
    -- Create endpoint on server instance that hosts the secondary replica:   
    CREATE ENDPOINT dbm_endpoint  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=5022)   
        FOR DATABASE_MIRRORING (ROLE=ALL);  
    GO  
    ```  
  
3.  > [!NOTE]  
    >  Si les comptes de service des instances de serveur qui hébergeront vos réplicas de disponibilité s'exécutent sous le même compte de domaine, cette étape est inutile. Ignorez-la et passez directement à l'étape suivante.  
  
     Si les comptes de service des instances de serveur s'exécutent sous des comptes utilisateur de domaine différents, sur chaque instance de serveur, créez une connexion pour l'autre instance de serveur et accordez cette autorisation de connexion pour accéder au point de terminaison de mise en miroir de bases de données local.  
  
     L'exemple de code suivant affiche les instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] pour créer une connexion et lui accorder l'autorisation sur un point de terminaison. Le compte de domaine de l’instance de serveur distant est représenté ici en tant que *nom_domaine*\\*nom_utilisateur*.  
  
    ```sql  
    -- If necessary, create a login for the service account, domain_name\user_name  
    -- of the server instance that will host the other replica:  
    USE master;  
    GO  
    CREATE LOGIN [domain_name\user_name] FROM WINDOWS;  
    GO  
    -- And Grant this login connect permissions on the endpoint:  
    GRANT CONNECT ON ENDPOINT::dbm_endpoint   
       TO [domain_name\user_name];  
    GO  
    ```  
  
4.  Sur l'instance de serveur où résident les bases de données utilisateur, créez le groupe de disponibilité.  
  
     L'exemple de code suivant crée un groupe de disponibilité nommé *MyAG* sur l'instance de serveur sur laquelle les exemples de bases de données, *MyDb1* et *MyDb2*, ont été créés. L'instance de serveur local, `AgHostInstance`, sur *COMPUTER01* est spécifiée en premier. Cette instance hébergera le réplica principal initial. Une instance de serveur distant, l'instance de serveur par défaut sur *COMPUTER02*, est spécifiée pour héberger un réplica secondaire. Les deux réplicas de disponibilité sont configurés de manière à utiliser le mode de validation asynchrone avec basculement manuel (pour les réplicas à validation asynchrone, le basculement manuel correspond à un basculement forcé entraînant une éventuelle perte de données).  
  
    ```sql
    -- Create the availability group, MyAG:   
    CREATE AVAILABILITY GROUP MyAG   
       FOR   
          DATABASE MyDB1, MyDB2   
       REPLICA ON   
          'COMPUTER01\AgHostInstance' WITH   
             (  
             ENDPOINT_URL = 'TCP://COMPUTER01.Adventure-Works.com:7022',   
             AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
             FAILOVER_MODE = MANUAL  
             ),  
          'COMPUTER02' WITH   
             (  
             ENDPOINT_URL = 'TCP://COMPUTER02.Adventure-Works.com:5022',  
             AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
             FAILOVER_MODE = MANUAL  
             );   
    GO  
    ```  
  
     Pour obtenir d’autres exemples de code [!INCLUDE[tsql](../../../includes/tsql-md.md)] permettant de créer un groupe de disponibilité, consultez [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md).  
  
5.  Sur l'instance de serveur qui héberge le réplica secondaire, joignez le réplica secondaire au groupe de disponibilité.  
  
     L'exemple de code suivant joint le réplica secondaire sur `COMPUTER02` au groupe de disponibilité `MyAG` .  
  
    ```sql 
    -- On the server instance that hosts the secondary replica,   
    -- join the secondary replica to the availability group:  
    ALTER AVAILABILITY GROUP MyAG JOIN;  
    GO  
    ```  
  
6.  Sur l'instance de serveur qui héberge le réplica secondaire, créez les bases de données secondaires.  
  
     L'exemple de code suivant crée les bases de données secondaires *MyDb1* et *MyDb2* en restaurant des sauvegardes de base de données à l'aide de l'option RESTORE WITH NORECOVERY.  
  
    ```sql 
    -- On the server instance that hosts the secondary replica,   
    -- Restore database backups using the WITH NORECOVERY option:  
    RESTORE DATABASE MyDb1   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
        WITH NORECOVERY;  
    GO  
  
    RESTORE DATABASE MyDb2   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
        WITH NORECOVERY;  
    GO 
    ```  
  
7.  Sur l'instance de serveur qui héberge le réplica principal, sauvegardez le journal des transactions sur chacune des bases de données primaires.  
  
    > [!IMPORTANT]  
    >  Lorsque vous configurez un vrai groupe de disponibilité, nous vous recommandons, avant d'effectuer cette sauvegarde de fichier journal, d'interrompre les tâches de sauvegarde de fichier journal pour vos bases de données primaires jusqu'à ce que vous ayez joint les bases de données secondaires correspondantes au groupe de disponibilité.  
  
     L'exemple de code suivant crée une sauvegarde du journal des transactions sur MyDb1 et MyDb2.  
  
    ```sql  
    -- On the server instance that hosts the primary replica,   
    -- Backup the transaction log on each primary database:  
    BACKUP LOG MyDb1   
    TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
        WITH NOFORMAT;  
    GO  
  
    BACKUP LOG MyDb2   
    TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
        WITHNOFORMAT;  
    GO
    ```  
  
    > [!TIP]  
    >  En général, une sauvegarde de fichier journal doit être effectuée sur chaque base de données primaire, puis doit être restaurée sur la base de données secondaire correspondante (en utilisant l'option WITH NORECOVERY). Toutefois, cette sauvegarde du fichier journal peut s'avérer superflue, si la base de données vient d'être créée et qu'aucune sauvegarde du fichier journal n'a été encore réalisée ou si le mode de récupération vient d'être modifié de SIMPLE à FULL.  
  
8.  Sur l'instance de serveur qui héberge le réplica secondaire, appliquez des sauvegardes de fichier journal aux bases de données secondaires.  
  
     L'exemple de code suivant applique les sauvegardes aux bases de données secondaires *MyDb1* et *MyDb2* en restaurant des sauvegardes de base de données à l'aide de l'option RESTORE WITH NORECOVERY.  
  
    > [!IMPORTANT]  
    >  Lorsque vous préparez une base de données secondaire réelle, vous devez appliquer chaque sauvegarde de fichier journal effectuée depuis la sauvegarde de la base de données à partir de laquelle vous avez créé la base de données secondaire, en démarrant avec la plus ancienne et en utilisant toujours l'option RESTORE WITH NORECOVERY. Bien sûr, si vous restaurez à la fois des sauvegardes de bases de données complètes et différentielles, vous devez uniquement appliquer les sauvegardes de fichier journal effectuées après la sauvegarde différentielle.  
  
    ```sql  
    -- Restore the transaction log on each secondary database,  
    -- using the WITH NORECOVERY option:  
    RESTORE LOG MyDb1   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
        WITH FILE=1, NORECOVERY;  
    GO  
    RESTORE LOG MyDb2   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
        WITH FILE=1, NORECOVERY;  
    GO  
    ```  
  
9. Sur l'instance de serveur qui héberge le réplica secondaire, joignez les nouvelles bases de données secondaires au groupe de disponibilité.  
  
     L'exemple de code suivant, joint la base de données secondaire *MyDb1* , puis les bases de données secondaires *MyDb2* au groupe de disponibilité *MyAG* .  
  
    ```sql  
    -- On the server instance that hosts the secondary replica,   
    -- join each secondary database to the availability group:  
    ALTER DATABASE MyDb1 SET HADR AVAILABILITY GROUP = MyAG;  
    GO  
  
    ALTER DATABASE MyDb2 SET HADR AVAILABILITY GROUP = MyAG;  
    GO  
    ```  
  
###  <a name="complete-code-example-for-sample-configuration-procedure"></a><a name="CompleteCodeExample"></a> Exemple de code complet pour l'exemple de procédure de configuration  
 L'exemple suivant fusionne les exemples de code de toutes les étapes de l'exemple de procédure configuration. Le tableau suivant répertorie les valeurs d'espace réservé utilisées dans cet exemple de code. Pour plus d'informations sur les étapes de cet exemple de code, consultez [Conditions préalables requises pour l'utilisation de l'exemple de procédure de configuration](#PrerequisitesForExample) et [Exemple de procédure de configuration](#SampleProcedure), plus avant dans cette rubrique.  
  
|Espace réservé|Description|  
|-----------------|-----------------|  
|\\\\*FILESERVER*\\*SQLbackups*|Partage de sauvegarde fictif.|  
|\\\\*FILESERVER*\\*SQLbackups\MyDb1.bak*|Fichier de sauvegarde pour MyDb1.|  
|\\\\*FILESERVER*\\*SQLbackups\MyDb2.bak*|Fichier de sauvegarde pour MyDb2.|  
|*7022*|Numéro de port affecté à chaque point de terminaison de mise en miroir de bases de données.|  
|*COMPUTER01\AgHostInstance*|Instance de serveur qui héberge le réplica principal initial.|  
|*COMPUTER02*|Instance de serveur qui héberge le réplica secondaire initial. Il s'agit de l'instance de serveur par défaut sur `COMPUTER02`.|  
|*dbm_endpoint*|Nom spécifié pour chaque point de terminaison de mise en miroir de bases de données.|  
|*MyAG*|Nom de l'exemple de groupe de disponibilité.|  
|*MyDb1*|Nom du premier exemple de base de données.|  
|*MyDb2*|Nom du deuxième exemple de base de données.|  
|*DOMAINE1\utilisateur1*|Compte de service de l'instance de serveur qui hébergera le réplica principal initial.|  
|*DOMAINE2\utilisateur2*|Compte de service de l'instance de serveur qui hébergera le réplica secondaire initial.|  
|TCP://*COMPUTER01.Adventure-Works.com*:*7022*|URL du point de terminaison de l'instance AgHostInstance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur COMPUTER01.|  
|TCP://*COMPUTER02.Adventure-Works.com*:*5022*|URL de point de terminaison de l'instance par défaut de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur COMPUTER02.|  
  
> [!NOTE]  
>  Pour obtenir d’autres exemples de code [!INCLUDE[tsql](../../../includes/tsql-md.md)] permettant de créer un groupe de disponibilité, consultez [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md).  
  
```sql  
-- on the server instance that will host the primary replica,   
-- create sample databases:  
CREATE DATABASE MyDb1;  
GO  
ALTER DATABASE MyDb1 SET RECOVERY FULL;  
GO  
  
CREATE DATABASE MyDb2;  
GO  
ALTER DATABASE MyDb2 SET RECOVERY FULL;  
GO  
  
-- Backup sample databases:  
BACKUP DATABASE MyDb1   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH FORMAT;  
GO  
  
BACKUP DATABASE MyDb2   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITH FORMAT;  
GO  
  
-- Create the endpoint on the server instance that will host the primary replica:  
CREATE ENDPOINT dbm_endpoint  
    STATE=STARTED   
    AS TCP (LISTENER_PORT=7022)   
    FOR DATABASE_MIRRORING (ROLE=ALL);  
GO  
  
-- Create the endpoint on the server instance that will host the secondary replica:   
CREATE ENDPOINT dbm_endpoint  
    STATE=STARTED   
    AS TCP (LISTENER_PORT=7022)   
    FOR DATABASE_MIRRORING (ROLE=ALL);  
GO  
  
-- If both service accounts run under the same domain account, skip this step. Otherwise,   
-- On the server instance that will host the primary replica,   
-- create a login for the service account   
-- of the server instance that will host the secondary replica, DOMAIN2\user2,   
-- and grant this login connect permissions on the endpoint:  
USE master;  
GO  
CREATE LOGIN [DOMAIN2\user2] FROM WINDOWS;  
GO  
GRANT CONNECT ON ENDPOINT::dbm_endpoint   
   TO [DOMAIN2\user2];  
GO  
  
-- If both service accounts run under the same domain account, skip this step. Otherwise,   
-- On the server instance that will host the secondary replica,  
-- create a login for the service account   
-- of the server instance that will host the primary replica, DOMAIN1\user1,   
-- and grant this login connect permissions on the endpoint:  
USE master;  
GO  
  
CREATE LOGIN [DOMAIN1\user1] FROM WINDOWS;  
GO  
GRANT CONNECT ON ENDPOINT::dbm_endpoint   
   TO [DOMAIN1\user1];  
GO  
  
-- On the server instance that will host the primary replica,   
-- create the availability group, MyAG:  
CREATE AVAILABILITY GROUP MyAG   
   FOR   
      DATABASE MyDB1, MyDB2   
   REPLICA ON   
      'COMPUTER01\AgHostInstance' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER01.Adventure-Works.com:7022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         ),  
      'COMPUTER02' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER02.Adventure-Works.com:7022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         );   
GO  
  
-- On the server instance that hosts the secondary replica,   
-- join the secondary replica to the availability group:  
ALTER AVAILABILITY GROUP MyAG JOIN;  
GO  
  
-- Restore database backups onto this server instance, using RESTORE WITH NORECOVERY:  
RESTORE DATABASE MyDb1   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH NORECOVERY;  
GO  
  
RESTORE DATABASE MyDb2   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITH NORECOVERY;  
GO  
  
-- Back up the transaction log on each primary database:  
BACKUP LOG MyDb1   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH NOFORMAT;  
GO  
  
BACKUP LOG MyDb2   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITHNOFORMAT  
GO  
  
-- Restore the transaction log on each secondary database,  
-- using the WITH NORECOVERY option:  
RESTORE LOG MyDb1   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH FILE=1, NORECOVERY;  
GO  
RESTORE LOG MyDb2   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITH FILE=1, NORECOVERY;  
GO  
  
-- On the server instance that hosts the secondary replica,   
-- join each secondary database to the availability group:  
ALTER DATABASE MyDb1 SET HADR AVAILABILITY GROUP = MyAG;  
GO  
  
ALTER DATABASE MyDb2 SET HADR AVAILABILITY GROUP = MyAG;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Pour configurer les propriétés du groupe de disponibilité et du réplica**  
  
-   [Modifier le mode de disponibilité d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Modifier le mode de basculement d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Configurer la stratégie de basculement flexible pour contrôler les conditions du basculement automatique &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
-   [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Configurer la sauvegarde sur des réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Configurer l’accès en lecture seule sur un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Configurer le routage en lecture seule pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Modifier le délai d’expiration de session pour un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **Pour terminer la configuration du groupe de disponibilité**  
  
-   [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **Autres méthodes de création d'un groupe de disponibilité**  
  
-   [Utiliser l’Assistant Groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Créer un groupe de disponibilité &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
 **Pour activer les groupes de disponibilité Always On**  
  
-   [Activer et désactiver les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **Pour configurer un point de terminaison pour la mise en miroir de bases de données**  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour les groupes de disponibilité Always On &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Pour résoudre des problèmes de configuration des groupes de disponibilité Always On**  
  
-   [Résoudre des problèmes de configuration des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [Résoudre une opération d’ajout de fichier ayant échoué &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   **Blogs :**  
  
     [Always On - HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
     [Blogs de l’équipe SQL Server Always On : Blog officiel de l’équipe SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
     [Blogs des ingénieurs du Service clientèle et du Support technique de SQL Server](/archive/blogs/psssql/)  
  
-   **Vidéos :**  
  
     [Microsoft SQL Server, nom de code « Denali », série Always On, Partie 1 : Introducing the Next Generation High Availability Solution](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302) (vidéo de présentation de la solution haute disponibilité de la génération suivante)  
  
     [Microsoft SQL Server, nom de code « Denali », série Always On, Partie 2 : Génération d’une solution haute disponibilité critique à l’aide d’AlwaysOn](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **Livres blancs :**  
  
     [Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la récupération d’urgence](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Livres blancs de Microsoft pour SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Livres blancs de l'équipe de consultants clients de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Voir aussi  
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
