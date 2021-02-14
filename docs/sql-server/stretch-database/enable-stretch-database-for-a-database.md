---
description: Enable Stretch Database for a database
title: Enable Stretch Database for a database
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, enabling database
- enabling database for Stretch Database
ms.assetid: 37854256-8c99-4566-a552-432e3ea7c6da
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 0b93f34e5a5b716c649f0902dabefe651253c9ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079980"
---
# <a name="enable-stretch-database-for-a-database"></a>Enable Stretch Database for a database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Pour configurer une base de données existante pour Stretch Database, sélectionnez **Tâches | Stretch | Activer** pour une base de données dans SQL Server Management Studio afin d’ouvrir l’Assistant **Activer la base de données pour Stretch** . Vous pouvez également utiliser Transact-SQL pour activer Stretch Database pour une base de données.  
  
 Si vous sélectionnez **Tâches | Stretch | Activer** pour une table et que vous n’avez pas encore activé la base de données pour Stretch Database, l’Assistant configure la base de données pour Stretch Database et vous permet de sélectionner des tables dans le cadre du processus. Suivez les étapes décrites dans cet article au lieu de la procédure décrite dans [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).  
  
 L’activation de Stretch Database sur une table ou une base de données nécessite les autorisations db_owner. L’activation de Stretch Database sur une base de données nécessite également des autorisations CONTROL DATABASE.  

> [!NOTE]
> Ultérieurement, n’oubliez pas que la désactivation de Stretch Database pour une table ou une base de données ne supprime pas l’objet distant. Si vous souhaitez supprimer la table distante ou la base de données distante, vous devez la supprimer à l'aide du portail de gestion Azure. Les objets distants continuent d’entraîner des coûts Azure tant qu’ils n’ont pas été supprimés manuellement. 
 
## <a name="before-you-get-started"></a>Avant de commencer  
  
-   Avant de configurer une base de données pour Stretch, nous vous recommandons d’exécuter l’Assistant Stretch Database pour identifier les bases de données et les tables qui sont éligibles pour Stretch. Le Conseiller Stretch Database identifie également les problèmes de blocage. Pour plus d’informations, consultez [Identifier des bases de données et tables pour Stretch Database en exécutant Stretch Database Advisor](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md).  
  
-   Consultez [Limitations concernant Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md).  
  
-   Stretch Database migre les données vers Azure. Par conséquent, vous devez avoir un compte Azure et un abonnement pour la facturation. Pour obtenir un compte Azure, [cliquez ici](https://azure.microsoft.com/pricing/free-trial/).  
  
-   Procurez-vous les informations de connexion nécessaires pour créer un serveur Azure ou pour sélectionner un serveur Azure existant.  
  
##  <a name="prerequisite-enable-stretch-database-on-the-server"></a><a name="EnableTSQLServer"></a> Condition préalable : Activer Stretch Database sur le serveur  
 Avant de pouvoir activer Stretch Database sur une base de données ou une table, vous devez l’activer sur le serveur local. Cette opération nécessite des autorisations sysadmin ou serveradmin.  
  
-   Si vous disposez des autorisations d’administration nécessaires, l’Assistant **Activer la base de données pour Stretch** configure le serveur pour Stretch.  
  
-   Si vous n’avez pas les autorisations nécessaires, un administrateur doit activer l’option manuellement en exécutant **sp_configure** avant que vous n’exécutiez l’Assistant ou un administrateur doit exécuter lui-même l’Assistant.  
  
 Pour activer Stretch Database sur le serveur manuellement, exécutez **sp_configure** et activez l’option **Archive de données distante** . L’exemple suivant active l’option **remote data archive** en définissant sa valeur sur 1.  
  
```sql
EXEC sp_configure 'remote data archive' , '1';  
GO

RECONFIGURE;  
GO  
```  
  
 Pour plus d’informations, consultez [Configurer l’option de configuration du serveur d’archive de données distante](../../database-engine/configure-windows/configure-the-remote-data-archive-server-configuration-option.md) et [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
##  <a name="use-the-wizard-to-enable-stretch-database-on-a-database"></a><a name="Wizard"></a> Utilisation de l’assistant pour activer Stretch Database sur une base de données  
 Pour plus d’informations sur l’Assistant Activer la base de données pour Stretch, notamment les informations que vous devez entrer et les choix que vous avez à effectuer, consultez [Mise en route en exécutant l’Assistant Activer la base de données pour Stretch](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md).  
  
##  <a name="use-transact-sql-to-enable-stretch-database-on-a-database"></a><a name="EnableTSQLDatabase"></a> Utilisation de Transact-SQL pour activer Stretch Database sur une base de données  
 Avant de pouvoir activer Stretch Database sur des tables individuelles, vous devez l’activer sur la base de données.  
  
 L’activation de Stretch Database sur une table ou une base de données nécessite les autorisations db_owner. L’activation de Stretch Database sur une base de données nécessite également des autorisations CONTROL DATABASE.  
  
1.  Avant de commencer, choisissez un serveur Azure existant pour les données que Stretch Database migre ou créez un serveur Azure.  
  
2.  Sur le serveur Azure, créez une règle de pare-feu avec la plage d’adresses IP de l’ordinateur SQL Server qui autorise SQL Server à communiquer avec le serveur distant.  

    Vous pouvez facilement trouver les valeurs dont vous avez besoin et créer la règle de pare-feu en essayant de vous connecter au serveur Azure depuis l’Explorateur d’objets dans SQL Server Management Studio (SSMS). SSMS vous aide à créer la règle en ouvrant la boîte de dialogue suivante, qui inclut déjà les valeurs d’adresse IP requises.
    
    ![Règle de pare-feu pour Stretch Database](../../sql-server/stretch-database/media/firewall-rule-for-stretch.png)
  
3.  Pour configurer une base de données SQL Server pour Stretch Database, la base de données doit avoir une clé principale de base de données. La clé principale de base de données permet de sécuriser les informations d’identification que Stretch Database utilise pour se connecter à la base de données distante. Voici un exemple qui crée une clé principale de base de données.  
  
    ```sql  
    USE <database>; 
    GO  
  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD='<password>'; 
    GO
    ```  
    Pour plus d’informations sur la clé principale de base de données, consultez [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md) et [Créer une clé principale de base de données](../../relational-databases/security/encryption/create-a-database-master-key.md).
    
4.  Lorsque vous configurez une base de données pour Stretch Database, vous devez fournir les informations d’identification que Stretch Database va utiliser pour la communication entre le serveur SQL Server local et le serveur Azure distant. Vous avez le choix entre deux options.  
  
    -   Vous pouvez fournir des informations d’identification d’administrateur.  
  
        -   Si vous activez Stretch Database en exécutant l’assistant, vous pouvez créer les informations d’identification à ce moment-là.  
  
        -   Si vous envisagez d’activer Stretch Database en exécutant **ALTER DATABASE**, vous devez créer manuellement les informations d’identification avant d’exécuter **ALTER DATABASE** pour activer Stretch Database. 
        
        Voici un exemple qui crée des informations d’identification.
  
        ```sql  
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>  
            WITH IDENTITY = '<identity>' , SECRET = '<secret>' ;
        GO   
        ```  

         Pour plus d’informations sur les informations d’identification, consultez [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md). La création des informations d’identification nécessite l’autorisation ALTER ANY CREDENTIAL.  

    -   Vous pouvez utiliser un compte de service fédéré pour que le serveur SQL Server communique avec le serveur Azure distant lorsque les conditions suivantes sont toutes vraies.  
  
        -   Le compte de service sous lequel l’instance de SQL Server s’exécute est un compte de domaine.  
  
        -   Le compte de domaine appartient à un domaine dont Active Directory est fédéré avec Azure Active Directory.  
  
        -   Le serveur Azure distant est configuré pour prendre en charge l’authentification Azure Active Directory.  
  
        -   Le compte de service sous lequel s’exécute l’instance de SQL Server doit être configuré comme un compte dbmanager ou sysadmin sur le serveur Azure distant.  
  
5.  Pour configurer une base de données pour Stretch Database, exécutez la commande ALTER DATABASE.  
  
    1.  Pour l’argument SERVER, indiquez le nom d’un serveur Azure existant, notamment la partie `.database.windows.net` du nom, par exemple `MyStretchDatabaseServer.database.windows.net`.  
  
    2.  Fournissez les informations d’identification d’un administrateur existant avec l’argument CREDENTIAL ou spécifiez FEDERATED_SERVICE_ACCOUNT = ON. L’exemple suivant fournit des informations d’identification existantes.  
  
    ```sql  
    ALTER DATABASE <database name>  
        SET REMOTE_DATA_ARCHIVE = ON  
            (  
                SERVER = '<server_name>' ,  
                CREDENTIAL = <db_scoped_credential_name>  
            ) ;  
    GO
    ```  
  
## <a name="next-steps"></a>Étapes suivantes  
-   [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) afin d’activer des tables supplémentaires.  
  
-   Pour connaître l’état de la migration de données, consultez [Surveiller et résoudre les problèmes de migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).  
  
-   [Suspension et reprise de la migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
-   [Gérer et dépanner Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
-   [Sauvegarder des bases de données Stretch](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
  
-   [Restaurer des bases de données Stretch](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Identifier des bases de données et tables pour Stretch Database en exécutant Stretch Database Advisor](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md)   
 [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
  
