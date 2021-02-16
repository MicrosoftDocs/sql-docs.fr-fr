---
title: 'Tutoriel : Configurer la réplication (T-SQL)'
description: Configurez la réplication de capture instantanée SQL Server sur Linux avec deux instances de SQL Server à l’aide de Transact-SQL (T-SQL).
ms.custom: seo-dt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/09/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
titleSuffix: SQL Server on Linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: 825fec3eeb0abbfacd4bb5f1311e13b6f99189e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346459"
---
# <a name="configure-replication-with-t-sql"></a>Configurer la réplication avec T-SQL

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)] 

Dans ce tutoriel, vous allez configurer la réplication de capture instantanée SQL Server sur Linux avec deux instances de SQL Server à l’aide de Transact-SQL. Le serveur de publication et le serveur de distribution sont identiques et l’abonné se trouve sur une instance distincte.

> [!div class="checklist"]
> * Activer les agents de réplication SQL Server sur Linux
> * Créer un exemple de base de données
> * Configurer le dossier de captures instantanées pour l’accès aux agents SQL Server
> * Configurer le serveur de distribution
> * Configurer le serveur de publication
> * Configurer une publication et des articles
> * Configurer l’abonné 
> * Exécuter des tâches de réplication

Toutes les configurations de réplication peuvent être configurées avec des [procédures stockées de réplication](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).

## <a name="prerequisites"></a>Prérequis
Pour exécuter ce didacticiel, les éléments suivants sont nécessaires :

- Deux instances de SQL Server avec la dernière version de SQL Server sur Linux
- Un outil permettant d’émettre des requêtes T-SQL pour configurer la réplication, telle que SQLCMD ou SSMS

   Consultez [Utiliser SSMS pour gérer SQL Server sur Linux](./sql-server-linux-manage-ssms.md).

   >[!NOTE]
   >[!INCLUDE[SQL Server 2017](../includes/sssql17-md.md)] ([CU18](https://support.microsoft.com/help/4527377)) et les versions ultérieures prennent en charge la réplication SQL Server pour les instances de SQL Server sur Linux.

## <a name="detailed-steps"></a>Procédure détaillée

1. Activez les agents de réplication SQL Server sur Linux. Sur les deux machines hôtes, exécutez les commandes suivantes dans le terminal. 

   ```bash
   sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
   sudo systemctl restart mssql-server
   ```

1. Créez l’exemple de base de données et de table. Sur le serveur de publication, créez un exemple de base de données et de table qui servira d’articles pour une publication.

   ```sql
   CREATE DATABASE Sales
   GO
   USE [SALES]
   GO 
   CREATE TABLE CUSTOMER([CustomerID] [int] NOT NULL, [SalesAmount] [decimal] NOT NULL)
   GO 
   INSERT INTO CUSTOMER (CustomerID, SalesAmount) VALUES (1,100),(2,200),(3,300)
   ```

   Sur l’autre instance de SQL Server, l’abonné, créez la base de données pour recevoir les articles.

   ```sql
   CREATE DATABASE Sales
   GO
   ```

1. Créez le dossier de captures instantanées pour SQL Server Agents à lire/écrire sur le serveur de distribution, créez le dossier de captures instantanées et octroyez l’accès à l’utilisateur « mssql ». 

   ```bash
   sudo mkdir /var/opt/mssql/data/ReplData/
   sudo chown mssql /var/opt/mssql/data/ReplData/
   sudo chgrp mssql /var/opt/mssql/data/ReplData/
   ```

1. Configurez le serveur de distribution. Dans cet exemple, le serveur de publication est également le serveur de distribution. Exécutez les commandes suivantes sur le serveur de publication pour configurer également l’instance pour la distribution.

   ```sql
   DECLARE @distributor AS sysname
   DECLARE @distributorlogin AS sysname
   DECLARE @distributorpassword AS sysname
   -- Specify the distributor name. Use 'hostname' command on in terminal to find the hostname
   SET @distributor = N'<distributor instance name>'--in this example, it will be the name of the publisher
   SET @distributorlogin = N'<distributor login>'
   SET @distributorpassword = N'<distributor password>'
   -- Specify the distribution database. 
   
   use master
   exec sp_adddistributor @distributor = @distributor -- this should be the hostname

   -- Log into distributor and create Distribution Database. In this example, our publisher and distributor is on the same host
   exec sp_adddistributiondb @database = N'distribution', @log_file_size = 2, @deletebatchsize_xact = 5000, @deletebatchsize_cmd = 2000, @security_mode = 0, @login = @distributorlogin, @password = @distributorpassword
   GO

   DECLARE @snapshotdirectory AS nvarchar(500)
   SET @snapshotdirectory = N'/var/opt/mssql/data/ReplData/'

   -- Log into distributor and create Distribution Database. In this example, our publisher and distributor is on the same host
   use [distribution] 
   if (not exists (select * from sysobjects where name = 'UIProperties' and type = 'U ')) 
          create table UIProperties(id int) 
   if (exists (select * from ::fn_listextendedproperty('SnapshotFolder', 'user', 'dbo', 'table', 'UIProperties', null, null))) 
          EXEC sp_updateextendedproperty N'SnapshotFolder', @snapshotdirectory, 'user', dbo, 'table', 'UIProperties' 
   else 
         EXEC sp_addextendedproperty N'SnapshotFolder', @snapshotdirectory, 'user', dbo, 'table', 'UIProperties'
   GO
   ```

1. Configurez le serveur de publication. Exécutez les commandes TSQL suivantes sur le serveur de publication.

   ```sql
   DECLARE @publisher AS sysname
   DECLARE @distributorlogin AS sysname
   DECLARE @distributorpassword AS sysname
   -- Specify the distributor name. Use 'hostname' command on in terminal to find the hostname
   SET @publisher = N'<instance name>' 
   SET @distributorlogin = N'<distributor login>'
   SET @distributorpassword = N'<distributor password>'
   -- Specify the distribution database. 

   -- Adding the distribution publishers
   exec sp_adddistpublisher @publisher = @publisher, 
   @distribution_db = N'distribution', 
   @security_mode = 0, 
   @login = @distributorlogin, 
   @password = @distributorpassword, 
   @working_directory = N'/var/opt/mssql/data/ReplData', 
   @trusted = N'false', 
   @thirdparty_flag = 0, 
   @publisher_type = N'MSSQLSERVER'
   GO
   ```

1. Configurez un travail de publication. Exécutez les commandes TSQL suivantes sur le serveur de publication.

   ```sql
   DECLARE @replicationdb AS sysname
   DECLARE @publisherlogin AS sysname
   DECLARE @publisherpassword AS sysname
   SET @replicationdb = N'Sales'
   SET @publisherlogin = N'<Publisher login>'
   SET @publisherpassword = N'<Publisher Password>'

   use [Sales]
   exec sp_replicationdboption @dbname = N'Sales', @optname = N'publish', @value = N'true'
   
   -- Add the snapshot publication
   exec sp_addpublication 
   @publication = N'SnapshotRepl', 
   @description = N'Snapshot publication of database ''Sales'' from Publisher ''<PUBLISHER HOSTNAME>''.',
   @retention = 0, 
   @allow_push = N'true', 
   @repl_freq = N'snapshot', 
   @status = N'active', 
   @independent_agent = N'true'

   exec sp_addpublication_snapshot @publication = N'SnapshotRepl', 
   @frequency_type = 1, 
   @frequency_interval = 1, 
   @frequency_relative_interval = 1, 
   @frequency_recurrence_factor = 0, 
   @frequency_subday = 8, 
   @frequency_subday_interval = 1, 
   @active_start_time_of_day = 0,
   @active_end_time_of_day = 235959, 
   @active_start_date = 0, 
   @active_end_date = 0, 
   @publisher_security_mode = 0, 
   @publisher_login = @publisherlogin, 
   @publisher_password = @publisherpassword
   ```

1. Créez des articles à partir de la table des ventes, exécutez les commandes TSQL suivantes sur le serveur de publication.

   ```sql
   use [Sales]
   exec sp_addarticle 
   @publication = N'SnapshotRepl', 
   @article = N'customer', 
   @source_owner = N'dbo', 
   @source_object = N'customer', 
   @type = N'logbased', 
   @description = null, 
   @creation_script = null, 
   @pre_creation_cmd = N'drop', 
   @schema_option = 0x000000000803509D,
   @identityrangemanagementoption = N'manual', 
   @destination_table = N'customer', 
   @destination_owner = N'dbo', 
   @vertical_partition = N'false'
   ```

1. Configurez l’abonnement. Exécutez les commandes TSQL suivantes sur le serveur de publication.

   ```sql
   DECLARE @subscriber AS sysname
   DECLARE @subscriber_db AS sysname
   DECLARE @subscriberLogin AS sysname
   DECLARE @subscriberPassword AS sysname
   SET @subscriber = N'<Instance Name>' -- for example, MSSQLSERVER
   SET @subscriber_db = N'Sales'
   SET @subscriberLogin = N'<Subscriber Login>'
   SET @subscriberPassword = N'<Subscriber Password>'

   use [Sales]
   exec sp_addsubscription 
   @publication = N'SnapshotRepl', 
   @subscriber = @subscriber,
   @destination_db = @subscriber_db, 
   @subscription_type = N'Push', 
   @sync_type = N'automatic', 
   @article = N'all', 
   @update_mode = N'read only', 
   @subscriber_type = 0

   exec sp_addpushsubscription_agent 
   @publication = N'SnapshotRepl', 
   @subscriber = @subscriber,
   @subscriber_db = @subscriber_db, 
   @subscriber_security_mode = 0, 
   @subscriber_login = @subscriberLogin,
   @subscriber_password = @subscriberPassword,
   @frequency_type = 1,
   @frequency_interval = 0, 
   @frequency_relative_interval = 0, 
   @frequency_recurrence_factor = 0, 
   @frequency_subday = 0, 
   @frequency_subday_interval = 0, 
   @active_start_time_of_day = 0, 
   @active_end_time_of_day = 0, 
   @active_start_date = 0, 
   @active_end_date = 19950101
   GO
   ```

1. Exécutez les travaux de l’agent de réplication. Exécutez la requête suivante pour obtenir la liste des tâches :

   ```sql
   SELECT name, date_modified FROM msdb.dbo.sysjobs order by date_modified desc
   ```

   Exécutez la tâche de réplication de capture instantanée pour générer la capture instantanée :

   ```sql
   USE msdb;   
   --generate snapshot of publications, for example
   EXEC dbo.sp_start_job N'PUBLISHER-PUBLICATION-SnapshotRepl-1'
   GO
   ```

   Exécutez le travail de réplication de capture instantanée pour générer la capture instantanée :

   ```sql
   USE msdb;
   --distribute the publication to subscriber, for example
   EXEC dbo.sp_start_job N'DISTRIBUTOR-PUBLICATION-SnapshotRepl-SUBSCRIBER'
   GO
   ```

1. Connectez l’abonné et interrogez les données répliquées.    

   Sur l’abonné, vérifiez que la réplication fonctionne en exécutant la requête suivante :

   ```sql
   SELECT * from [Sales].[dbo].[CUSTOMER]
   ```

Dans ce didacticiel, vous avez configuré la réplication de capture instantanée SQL Server sur Linux avec deux instances de SQL Server à l’aide de Transact-SQL.

> [!div class="checklist"]
> * Activer les agents de réplication SQL Server sur Linux
> * Créer un exemple de base de données
> * Configurer le dossier de captures instantanées pour l’accès aux agents SQL Server
> * Configurer le serveur de distribution
> * Configurer le serveur de publication
> * Configurer une publication et des articles
> * Configurer l’abonné 
> * Exécuter des tâches de réplication

## <a name="see-also"></a>Voir aussi

Pour plus d’informations sur la réplication, consultez la [documentation sur la réplication SQL Server](../relational-databases/replication/sql-server-replication.md).

## <a name="next-steps"></a>Étapes suivantes

[Concepts : Réplication SQL Server sur Linux](sql-server-linux-replication.md)

[Procédures stockées de réplication](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).
