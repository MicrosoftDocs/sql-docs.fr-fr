---
title: Configurer la mise en miroir de bases de données
description: Procédure de configuration d’une relation de mise en miroir de bases de données entre un serveur principal et un serveur miroir avec l’authentification Windows.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 143c68a5-589f-4e7f-be59-02707e1a430a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7584e4611d957f48efcb73d12b5965f0a4727b45
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642022"
---
# <a name="configure-database-mirroring"></a>Configurer la mise en miroir de bases de données
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] à la place.  
  
 Après avoir préparé la base de données miroir (voir [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)), vous pouvez établir une session de mise en miroir de bases de données. Le principal, le serveur miroir et le serveur témoin doivent être trois instances distinctes, ayant chacune un système d'hébergement qui lui est propre.  
  
> [!IMPORTANT]
>  Il est recommandé de configurer la mise en miroir de la base de données pendant les heures creuses, car la configuration de la mise en miroir peut avoir une incidence sur les performances.  
> 
> [!NOTE]
>  Une instance de serveur peut participer à plusieurs sessions simultanées de mise en miroir de bases de données avec des partenaires identiques ou différents. Une instance de serveur peut être partenaire dans certaines sessions et témoin dans d'autres. L'instance du serveur miroir doit être en train d'exécuter la même édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comme instance de serveur principal. La mise en miroir de bases de données n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md). En outre, nous vous conseillons vivement d'exécuter les instances sur des systèmes comparables pouvant gérer des charges de travail identiques.  
  
### <a name="to-establish-a-database-mirroring-session"></a>Pour établir une session de mise en miroir de bases de données  
  
1.  Création de la base de données miroir Pour plus d’informations, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
2.  Configurez la sécurité sur chaque instance de serveur.  
  
     Chaque instance de serveur participant à une session de mise en miroir de bases de données requiert un point de terminaison de mise en miroir de bases de données. Si le point de terminaison n'existe pas, vous devez le créer.  
  
    > [!NOTE]  
    >  Le type d'authentification utilisé pour la mise en miroir de la base de données par une instance de serveur est une propriété de son point de terminaison de mise en miroir de bases de données. Deux types de sécurité de transport sont disponibles pour la mise en miroir de bases de données : l’authentification Windows ou l’authentification basée sur les certificats. Pour plus d’informations, consultez [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md).  
  
     Sur chaque serveur partenaire, assurez-vous de la présence d'un point de terminaison pour la mise en miroir. Indépendamment du nombre de sessions de mise en miroir à prendre en charge, l'instance du serveur ne peut avoir qu'un seul point de terminaison de mise en miroir de bases de données. Si vous envisagez d’utiliser cette instance de serveur exclusivement pour les partenaires de sessions de mise en miroir, vous pouvez attribuer le rôle de partenaire au point de terminaison (ROLE **=** PARTNER). Si vous comptez aussi utiliser ce serveur comme témoin dans d'autres sessions, attribuez au point de terminaison le rôle ALL.  
  
     Pour exécuter une instruction SET PARTNER, le paramètre STATE des points de terminaison des deux partenaires doit avoir la valeur STARTED.  
  
     Pour savoir si une instance de serveur dispose d'un point de terminaison de mise en miroir de bases de données et pour connaître son rôle et son état (sur cette instance), utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :  
  
    ```  
    SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints  
    ```  
  
    > [!IMPORTANT]  
    >  Ne reconfigurez pas un point de terminaison de mise en miroir de base de données en cours d'utilisation. Si le point de terminaison de mise en miroir de base de données existe et est déjà utilisé, nous vous recommandons d'utiliser ce point de terminaison pour toute session établie sur l'instance du serveur. La suppression d'un point de terminaison en cours d'utilisation peut entraîner son redémarrage et perturber les connexions des sessions existantes, ce qui peut être perçu comme une erreur par les autres instances de serveurs. Cela est particulièrement important en mode haute sécurité avec basculement automatique, où la reconfiguration d'un point de terminaison sur un partenaire peut déclencher un basculement. Par ailleurs, si un témoin a été défini pour une session, la suppression du point de terminaison de la mise en miroir de bases de données peut provoquer la perte du quorum par le serveur principal de cette session ; si cela se produit, la base de données est mise en mode hors connexion et ses utilisateurs sont déconnectés. Pour plus d’informations, consultez [Quorum : effets d’un témoin sur la disponibilité de la base de données &#40;Mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
     Si l’un ou l’autre partenaire ne dispose pas d’un point de terminaison, consultez [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
3.  Si des instances de serveurs s'exécutent sous différents comptes d'utilisateurs de domaine, chaque instance requiert une connexion à la base de données **master** de toutes les autres. Si la connexion n'existe pas, vous devez la créer. Pour plus d’informations, consultez [Autoriser l’accès sur le réseau à un point de terminaison de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
4.  Pour définir le serveur principal comme partenaire sur la base de données miroir, connectez-vous au serveur miroir et exécutez l'instruction suivante :  
  
     ALTER DATABASE *\<database_name\>* SET PARTNER **=** _\<server\_network\_address\>_  
  
     où _\<database\_name\>_ est le nom de la base de données à mettre en miroir (ce nom est identique sur les deux partenaires) et _\<server\_network\_address\>_ , l’adresse réseau du serveur principal.  
  
     La syntaxe pour une adresse réseau de serveur est la suivante :  
  
     TCP <b>\://</b> _\<system-address\>_ <b>\:</b> _\<port\>_  
  
     où _\<system-address>_ est une chaîne qui identifie de façon non ambiguë l’ordinateur de destination, et _\<port>_ le numéro de port utilisé par le point de terminaison de mise en miroir de l’instance de serveur partenaire. Pour plus d’informations, consultez [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
     Par exemple, sur l'instance de serveur miroir, l'instruction ALTER DATABASE suivante définit le partenaire comme instance de serveur principal d'origine. Le nom de la base de données est **AdventureWorks**, l’adresse système est DBSERVER1 (nom du système du partenaire) et le port utilisé par le point de terminaison de la mise en miroir de la base de données du partenaire est 7022 :  
  
    ```  
    ALTER DATABASE AdventureWorks   
       SET PARTNER = 'TCP://DBSERVER1:7022'  
    ```  
  
     Cette instruction prépare le serveur miroir à former une session lorsqu'il sera contacté par le serveur principal.  
  
5.  Pour définir le serveur miroir comme partenaire sur la base de données principale, connectez-vous au serveur principal et exécutez l'instruction suivante :  
  
     ALTER DATABASE _\<database\_name\>_ SET PARTNER **=** _\<server\_network\_address\>_  
  
     Pour plus d'informations, consultez l'étape 4.  
  
     Par exemple, sur l'instance de serveur principal, l'instruction ALTER DATABASE suivante définit le partenaire comme instance de serveur miroir d'origine. Le nom de la base de données est **AdventureWorks**, l’adresse système est DBSERVER2 (nom du système du partenaire) et le port utilisé par le point de terminaison de la mise en miroir de la base de données du partenaire est 7025 :  
  
    ```  
    ALTER DATABASE AdventureWorks SET PARTNER = 'TCP://DBSERVER2:7022'  
    ```  
  
     La saisie de cette instruction sur le serveur principal démarre la session de mise en miroir des bases de données.  
  
6.  Par défaut, les sessions sont définies sur une sécurité des transactions totale (valeur de SAFETY définie sur FULL), la session est donc démarrée en mode haute sécurité sans basculement automatique. Pour reconfigurer la session pour qu'elle s'exécute en mode haute sécurité avec basculement automatique ou en mode haute performance asynchrone, procédez comme suit :  
  
    -   **Mode haute sécurité avec basculement automatique**  
  
         Si vous souhaitez qu'une session en mode haute sécurité prenne en charge le basculement automatique, ajoutez une instance de serveur témoin. Pour plus d’informations, consultez [Ajouter un témoin de mise en miroir de bases de données à l’aide de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md).  
  
    -   **Mode hautes performances**  
  
         Si vous ne voulez pas utiliser le basculement automatique et si vous préférez privilégier les performances par rapport à la disponibilité, désactivez la sécurité des transactions. Pour plus d’informations, consultez [Modifier la sécurité des transactions dans une session de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md).  
  
        > [!NOTE]  
        >  En mode hautes performances, la valeur de WITNESS doit être définie sur OFF. Pour plus d’informations, consultez [Quorum : effets d’un témoin sur la disponibilité de la base de données &#40;Mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).  
  
## <a name="example"></a>Exemple  
  
> [!NOTE]  
>  L'exemple suivant établit une session de mise en miroir de base de données entre partenaires d'une base de données miroir existante. Pour plus d’informations sur la création d’une base de données miroir, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 L'exemple montre les étapes de base à suivre pour créer une session de mise en miroir de bases de données sans témoin. Les deux partenaires sont les instances de serveur par défaut présentes sur deux systèmes informatiques (PARTNERHOST1 et PARTNERHOST5). Ces deux instances partenaires exécutent le même compte d'utilisateur de domaine Windows (MYDOMAIN\dbousername).  
  
1.  Sur l'instance de serveur principal (instance par défaut sur PARTNERHOST1), créez un point de terminaison qui prend en charge tous les rôles par le biais du port 7022 :  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
    > [!NOTE]  
    >  Pour obtenir un exemple illustrant comment configurer une connexion, consultez [Autoriser l’accès sur le réseau à un point de terminaison de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md).  
  
2.  Sur l'instance de serveur miroir (instance par défaut sur PARTNERHOST5), créez un point de terminaison qui prend en charge tous les rôles par le biais du port 7022 :  
  
    ```  
    --create an endpoint for this instance  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    ```  
  
3.  Sur l'instance de serveur principal (sur PARTNERHOST1), procédez à la sauvegarde de la base de données :  
  
    ```  
    BACKUP DATABASE AdventureWorks   
        TO DISK = 'C:\AdvWorks_dbmirror.bak'   
        WITH FORMAT  
    GO  
    ```  
  
4.  Sur l'instance de serveur miroir (sur `PARTNERHOST5`), restaurez la base de données :  
  
    ```  
    RESTORE DATABASE AdventureWorks   
        FROM DISK = 'Z:\AdvWorks_dbmirror.bak'   
        WITH NORECOVERY  
    GO  
    ```  
  
5.  Après avoir créé la sauvegarde complète de base de données, vous devez créer une sauvegarde du journal sur la base de données principale. Par exemple, l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante sauvegarde le journal dans le même fichier utilisé par la sauvegarde de base de données antérieure :  
  
    ```  
    BACKUP LOG AdventureWorks   
        TO DISK = 'C:\AdventureWorks.bak'   
    GO  
    ```  
  
6.  Avant de démarrer la mise en miroir, vous devez appliquer la sauvegarde du journal requise (et toutes les sauvegardes de journal ultérieures).  
  
     Par exemple, l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante restaure le premier journal à partir de C:\AdventureWorks.bak :  
  
    ```  
    RESTORE LOG AdventureWorks   
        FROM DISK = 'C:\ AdventureWorks.bak'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  Sur l'instance de serveur miroir, définissez l'instance de serveur sur PARTNERHOST1 comme étant le partenaire (ce qui en fait le serveur principal initial) :  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER =   
        'TCP://PARTNERHOST1:7022'  
    GO  
    ```  
  
    > [!IMPORTANT]  
    >  Une session de mise en miroir de bases de données s'exécute par défaut en mode synchrone, ce qui dépend de la sécurité des transactions totale (SAFETY défini sur FULL). Pour qu'une session s'exécute en mode hautes performances asynchrone, définissez SAFETY sur OFF. Pour plus d'informations, voir [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
8.  Dans l'instance de serveur principal, définissez l'instance de serveur sur `PARTNERHOST5` comme étant le partenaire (ce qui en fait le serveur miroir initial) :  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://PARTNERHOST5:7022'  
    GO  
    ```  
  
9. Si vous avez l'intention d'utiliser le mode haute sécurité avec basculement automatique, vous pouvez éventuellement définir l'instance de serveur témoin. Pour plus d’informations, consultez [Ajouter un témoin de mise en miroir de bases de données à l’aide de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md).  
  
> [!NOTE]  
>  Pour voir un exemple illustrant la configuration de la sécurité, la préparation de la base de données miroir, la définition des serveurs partenaires et l’ajout d’un témoin, consultez [Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Autoriser l’accès sur le réseau à un point de terminaison de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-allow-network-access-windows-authentication.md)   
 [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)   
 [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)   
 [Mise en miroir de bases de données et copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-log-shipping-sql-server.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Mise en miroir de bases de données et réplication &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)   
 [Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Modes de fonctionnement de la mise en miroir de bases de données](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)  
  
  

