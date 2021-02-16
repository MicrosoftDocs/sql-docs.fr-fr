---
title: Configurer une base de données de distribution dans un groupe de disponibilité
description: Configurez la base de données de distribution pour la réplication SQL Server avec un groupe de disponibilité AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 01/16/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], distribution
- distribution configuration [SQL Server replication]
- remote Distributors [SQL Server replication]
- transactional replication, configuring distribution
- distribution databases [SQL Server replication], sizing
- Distributors [SQL Server replication], configuring
- distribution databases [SQL Server replication], about distribution databases
- distribution databases [SQL Server replication]
- merge replication [SQL Server replication], configuring distribution
ms.assetid: 94d52169-384e-4885-84eb-2304e967d9f7
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 36e2f12793e08211e285c907f26d99e0fcc9262d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348003"
---
# <a name="set-up-replication-distribution-database-in-always-on-availability-group"></a>Configurer la base de données de distribution de réplication dans un groupe de disponibilité AlwaysOn
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cet article explique comment configurer une base de données distribution de réplication SQL Server dans un groupe de disponibilité AlwaysOn.

SQL Server 2017 CU6 et SQL Server 2016 SP2-CU3 prennent désormais en charge l’utilisation de bases de données de distribution de réplication dans un groupe de disponibilité, grâce aux mécanismes suivants :

- Le groupe de disponibilité de base de données de distribution doit avoir un écouteur. Lorsque le serveur de publication ajoute le serveur de distribution, il utilise le nom de l’écouteur pour nommer le serveur de distribution.
- Les travaux de réplication sont créés avec le nom de l’écouteur comme nom du serveur de distribution. Les travaux de capture instantanée de réplication, du lecteur de journaux et de l’agent de distribution (abonnement Push) créés sur le serveur de distribution sont créés sur tous les réplicas secondaires du groupe de disponibilité pour la base de données de distribution.
 >[!NOTE]
 >Les travaux de l’agent de distribution pour les abonnements Pull sont créés sur le serveur de l’abonné et non sur le serveur de distribution. 
- Un nouveau travail surveille l’état (principal ou secondaire dans le groupe de disponibilité) des bases de données de distribution, et active ou désactive les travaux de réplication en fonction de l’état des bases de données de distribution.

Une fois qu’une base de données de distribution du groupe de disponibilité a été configurée selon les étapes décrites ci-dessous, les travaux liés à la configuration de la réplication et à l’exécution peuvent s’exécuter correctement avant et après le basculement du groupe de disponibilité de la base de données.

## <a name="supported-scenarios"></a>Scénarios pris en charge

- Configuration de la base de données de distribution à inclure dans un groupe de disponibilité
- Configuration de la réplication, par exemple, pour les publications et les abonnements, avant et après le basculement du groupe de disponibilité
- Travaux de réplication fonctionnels avant et après le basculement
- Suppression de la réplication au niveau du serveur de distribution et du serveur de publication quand la base de données de distribution se trouve dans un groupe de disponibilité
- Ajout ou suppression de nœuds dans un groupe de disponibilité de base de données de distribution existant
- Un serveur de distribution peut comprendre plusieurs bases de données de distribution. Chaque base de données de distribution peut se trouver dans son propre groupe de disponibilité et n’être comprise dans aucun groupe de disponibilité. Plusieurs bases de données de distribution peuvent partager un même groupe de disponibilité.
- Le serveur de publication et le serveur de distribution doivent se trouver sur des instances distinctes de SQL Server.
- Si l’écouteur du groupe de disponibilité qui héberge la base de données de distribution est configuré pour utiliser un port non défini par défaut, il est nécessaire de configurer un alias pour l’écouteur et le port non défini par défaut.

## <a name="limitations-or-exclusions"></a>Limitations ou exclusions

- Le serveur de distribution local n’est pas pris en charge. Par exemple, le serveur de publication et le serveur de distribution doivent être des instances différentes de SQL Server. Ces instances peuvent être hébergées sur les mêmes ensembles de nœuds.  Un serveur de publication qui s’utilise lui-même comme serveur de distribution (aussi appelé serveur de distribution local) ne peut pas prendre en charge les bases de données de distribution qui se trouvent dans un groupe de disponibilité.
- Les serveurs de publication Oracle ne sont pas pris en charge.
- La réplication de fusion n'est pas prise en charge.
- La réplication transactionnelle impliquant un abonné avec mise à jour immédiate ou en attente n’est pas prise en charge.
- La réplication d’égal à égal n’est pas prise en charge.
- Toutes les instances de SQL Server 2017 qui hébergent des réplicas de base de données de distribution doivent être des instances SQL Server 2017 Cumulative 6 ou version ultérieure. 
- Toutes les instances de SQL Server 2016 qui hébergent des réplicas de base de données de distribution doivent être des instances SQL Server 2016 SP2 Cumulative 3 ou version ultérieure.
- Toutes les instances de SQL Server qui hébergent des réplicas de base de données de distribution doivent être de la même version, sauf pendant le court laps de temps de la mise à niveau.
- La base de données de distribution doit être en mode de récupération complète.
- Pour la récupération et pour permettre la troncation du journal des transactions, configurez des sauvegardes complètes du journal des transactions.
- Vous devez configurer un écouteur pour le groupe de disponibilité de base de données de distribution.
- Les réplicas secondaires d’un groupe de disponibilité de base de données de distribution peuvent être synchrones ou asynchrones. Le mode synchrone est recommandé.
- La réplication transactionnelle bidirectionnelle n'est pas prise en charge.
- SSMS n’indique pas que la base de données de distribution est en cours de synchronisation/synchronisée, lorsque la base de données de distribution est ajoutée à un groupe de disponibilité.


   >[!NOTE]
   >Avant d’exécuter l’une des procédures stockées de réplication (par exemple, `sp_dropdistpublisher`, `sp_dropdistributiondb`, `sp_dropdistributor`, `sp_adddistributiondb`, `sp_adddistpublisher`) sur un réplica secondaire, vérifiez que le réplica est entièrement synchronisé.

- Tous les réplicas secondaires d’un groupe de disponibilité de base de données de distribution doivent être accessibles en lecture. Si un réplica secondaire n’est pas lisible, les propriétés du serveur de distribution dans SQL Server Management Studio sur le réplica secondaire particulier ne sont pas accessibles, mais la réplication continue à fonctionner correctement. 
- Tous les nœuds du groupe de disponibilité de base de données de distribution doivent utiliser le même compte de domaine pour exécuter l’Agent SQL Server, et ce compte de domaine doit avoir les mêmes privilèges sur chaque nœud.
- Si des agents de réplication s’exécutent sous un compte proxy, le compte proxy doit se trouver sur chaque nœud du groupe de disponibilité de base de données de distribution et avoir les mêmes privilèges sur chaque nœud.
- Modifiez les propriétés du serveur de distribution et de la base de données dans tous les réplicas qui se trouvent dans le groupe de disponibilité de base de données.
- Apportez des modifications aux travaux de réplication à l’aide de procédures stockées msdb ou de SQL Server Management Studio dans tous les réplicas qui se trouvent dans le groupe de disponibilité de base de données.
- La configuration du serveur de distribution sur le serveur de publication doit être effectuée à l’aide de scripts. L’Assistant Réplication ne peut pas être utilisé. Les Assistants Réplication et les feuilles de propriétés servant d’autres fins sont pris en charge.
- Vous pouvez uniquement configurer le groupe de disponibilité pour les bases de données de distribution à l’aide de scripts.
- La configuration des bases de données de distribution d’un groupe de disponibilité doit être réalisée comme une nouvelle configuration de réplication. Le remplacement d’une base de données de distribution existante dans un groupe de disponibilité n’est pas pris en charge. En outre, une fois qu’une base de données de distribution est supprimée d’un groupe de disponibilité, elle ne fonctionne plus comme une base de données de distribution valide et doit être supprimée.

## <a name="configuration-architecture"></a>Architecture de la configuration

Les noms et les paramètres de serveur suivants sont utilisés dans les exemples de cet article.

- DIST1, DIST2, DIST3 sont des serveurs de distribution
- PUB est le serveur de publication
- Une fois que le groupe de disponibilité de base de données de distribution est créé, le nom de l’écouteur est DISTLISTENER
- DIST1 est destiné à être le premier réplica principal du groupe de disponibilité de base de données de distribution.

## <a name="configure-distributor-distribution-database-and-publisher"></a>Configurer le serveur de distribution, la base de données de distribution et le serveur de publication

Cet exemple configure un nouveau serveur de distribution et un nouveau serveur de publication, et place la base de données de distribution dans un groupe de disponibilité.

### <a name="distributors-workflow"></a>Flux de travail des serveurs de distribution

1. Configurez DIST3 DIST1, DIST2 en tant que serveurs de distribution avec `sp_adddistributor @@servername`. Spécifiez le mot de passe pour `distributor_admin` à l’aide de `@password`. La valeur de `@password` doit être identique sur DIST1, DIST2 et DIST3.
2. Créez la base de données de distribution sur DIST1 avec `sp_adddistributiondb`. Le nom de la base de données de distribution est `distribution`. Remplacez le mode de récupération simple de la base de données `distribution` par le mode complet (full).
3. Créez un groupe de disponibilité pour la base de données `distribution` avec des réplicas sur DIST1, DIST2 et DIST3. De préférence, tous les réplicas doivent être synchrones. Configurez les réplicas secondaires pour qu’ils soient accessibles en lecture ou autorisent la lecture. À ce stade, les bases de données de distribution correspondent au groupe de disponibilité (DG), DIST1 est le réplica principal, et DIST2 et DIST3 sont des réplicas secondaires.
4. Configurez un écouteur nommé `DISTLISTENER` pour le groupe de disponibilité.
5. Pour la récupération et pour permettre la troncation du journal des transactions, configurez des sauvegardes complètes du journal des transactions.
6. Sur DIST2 et DIST3, exécutez ce qui suit :

   ```sql
   sp_adddistributiondb 'distribution'
   ```

1. Pour ajouter `PUB` en tant que serveur de publication sur DIST1, exécutez ce qui suit :
   
   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   La valeur de `@working_directory` doit être un chemin réseau indépendant de DIST1, DIST2 et DIST3.

1. Sur DIST2 et DIST3, si le réplica est lisible en tant que réplica secondaire, exécutez :  

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   Si un réplica n’est pas lisible en tant que réplica secondaire, effectuez un basculement de sorte que le réplica devienne le réplica principal et exécutez 

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```
   
   La valeur de `@working_directory` doit être identique à celle de l’étape précédente.

### <a name="publisher-workflow"></a>Flux de travail du serveur de publication

Pour ajouter l’écouteur du groupe de disponibilité de la base de données `distribution` en tant que serveur de distribution sur le serveur de publication PUB, exécutez ce qui suit : 

   ```sql
   sp_adddistributor @distributor = 'DISTLISTENER', @password = <distributor_admin password> 
   ```

   La valeur de @password doit être celle qui a été spécifiée lorsque les serveurs de distribution ont été configurés dans le flux de travail du serveur de distribution.

## <a name="remove-distributor-and-publisher"></a>Supprimer le serveur de distribution et le serveur de publication

Cet exemple supprime le serveur de publication et le serveur de distribution lorsque la base de données de distribution se trouve dans le groupe de disponibilité (AG).

### <a name="publisher-workflow"></a>Flux de travail du serveur de publication

Sur le serveur de publication PUB, supprimez tous les abonnements et toutes les publications de ce serveur de publication, puis appelez `sp_dropdistributor`.

### <a name="distributors-workflow"></a>Flux de travail des serveurs de distribution

Dans cet exemple, DIST1 est le principal actuel du groupe de disponibilité de la base de données `distribution`. DIST2 et DIST3 sont des réplicas secondaires.

1. Sur DIST2 et DIST3, exécutez ce qui suit :

   ```sql
   sp_dropdistpublisher 'PUB',  @no_checks = 1
   ```

1. Sur DIST1, exécutez ce qui suit :

   ```sql
   sp_dropdistpublisher 'PUB'
   ```

1. Supprimez le groupe de disponibilité.
2. Sur DIST2 et DIST3, configurez le mode lecture/écriture de la base de données `distribution` en restaurant la base de données à l’aide d’une récupération.
   
   ```sql
   RESTORE DATABASE distribution WITH RECOVERY, KEEP_REPLICATION
   ```

1. Pour supprimer la base de données `distribution` et conserver le répertoire de fichiers de captures instantanées, exécutez ce qui suit : 

   ```sql
   sp_dropdistributiondb 'distribution' , @former_ag_secondary=1
   ```

  Cette procédure supprime tous les travaux en cours présents sur le réplica.

1. Pour supprimer la base de données `distribution` sur DIST1, exécutez ce qui suit :

   ```sql
   sp_dropdistributiondb 'distribution'
   ``` 

1. Si aucune autre base de données de distribution ne se trouve dans le groupe de disponibilité, exécutez `sp_dropdistributor` sur DIST1, DIST2 et DIST3.

## <a name="add-a-replica-to-distribution-database-ag"></a>Ajouter un réplica au groupe de disponibilité de base de données de distribution

Cet exemple ajoute un nouveau serveur de distribution à une configuration de réplication existante avec la base de données de distribution du groupe de disponibilité. Dans cet exemple, une base de données de distribution existante se trouve dans un groupe de disponibilité. DIST1 et DIST2 sont les serveurs de distribution, `distribution` est la base de données de distribution du groupe de disponibilité (AG), et PUB correspond au serveur de publication. Ajoutez DIST3 comme réplica dans le groupe de disponibilité.

### <a name="distributors-workflow"></a>Flux de travail des serveurs de distribution

1. DIST3 doit être configuré comme un serveur de distribution à l’aide de `sp_adddistributor @@servername`. Le mot de passe de `distributor_admin` doit être spécifié à l’aide du paramètre @password. Le mot de passe doit être identique à ce qui a été spécifié pour DIST1 et DIST2.
2. Ajoutez DIST3 au groupe de disponibilité pour la base de données de distribution actuelle.
3. Sur DIST3, exécutez ce qui suit :

   ```sql
   sp_adddistributiondb 'distribution'
   ```

4. Sur DIST3, si le réplica est lisible en tant que réplica secondaire, exécutez : 

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   Si le réplica n’est pas lisible en tant que réplica secondaire, effectuez un basculement de sorte que le réplica devienne le réplica principal et exécutez :

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```
   
   La valeur de `@working_directory` doit être identique à celle spécifiée pour DIST1 et DIST2.

4. Sur DIST3, vous devez recréer les serveurs liés pour les abonnés.

## <a name="remove-a-replica-from-distribution-database-ag"></a>Supprimer un réplica du groupe de disponibilité de base de données de distribution

Cet exemple supprime un serveur de distribution du groupe de disponibilité d’une base de données de distribution, sans que les autres réplicas du groupe de disponibilité ne soient pas affectés. Dans cet exemple, une base de données de distribution se trouve dans le groupe de disponibilité (AG). DIST1, DIST2 et DIST3 sont les serveurs de distribution, `distribution` est la base de données de distribution du groupe de disponibilité (AG), et PUB correspond au serveur de publication. Supprimez DIST3 du groupe de disponibilité.

### <a name="distributors-workflow"></a>Flux de travail des serveurs de distribution

1. Vérifiez que DIST3 est un réplica secondaire pour le groupe de disponibilité de la base de données `distribution`.
2. Supprimez DIST3 du groupe de disponibilité de la base de données `distribution`.
3. Sur DIST3, configurez le mode lecture/écriture de la base de données `distribution` en restaurant la base de données à l’aide d’une récupération. Par exemple, examinez la commande suivante :  
   
   ```sql
   RESTORE DATABASE distribution WITH RECOVERY, KEEP_REPLICATION.
   ```
   
1. Pour supprimer tous les travaux orphelins de DIST3, exécutez ce qui suit : 

   ```sql
   sp_dropdistpublisher 'PUB', @no_checks = 1
   ```

1. Sur DIST3, exécutez ce qui suit :

   ```sql
   sp_dropdistributiondb 'distribution', @former_ag_secondary=1
   ```

1. Sur DIST3, exécutez ce qui suit : 

   ```sql
   sp_dropdistributor
   ```

## <a name="remove-a-publisher-from-distribution-database-ag"></a>Supprimer un serveur de publication du groupe de disponibilité de base de données de distribution

Cet exemple supprime un serveur de publication du groupe de disponibilité de base de données de distribution actuel d’un serveur de distribution, sans que les autres serveurs de publication servis par ce groupe de disponibilité ne soient affectés. Dans cet exemple, la configuration existante est constituée d’une base de données située dans un groupe de disponibilité. DIST1, DIST2 et DIST3 sont les serveurs de distribution, `distribution` est la base de données de distribution du groupe de disponibilité (AG), et PUB1 et PUB2 correspondent aux serveurs de publication servis par la base de données `distribution`. L’exemple supprime PUB1 de ces serveurs de distribution.

### <a name="publisher-workflow"></a>Flux de travail du serveur de publication

Sur le serveur de publication PUB1, supprimez tous les abonnements et toutes les publications de ce serveur de publication, puis appelez `sp_dropdistributor`.

### <a name="distributor-workflow"></a>Flux de travail du serveur de distribution

DIST1 est le principal actuel du groupe de disponibilité de la base de données `distribution`.

1. Sur DIST2 et DIST3, exécutez ce qui suit :

   ```sql
   sp_dropdistpublisher 'PUB1',  @no_checks = 1
   ```

1. Sur DIST1, exécutez ce qui suit :

   ```sql
   sp_dropdistpublisher 'PUB1'
   ```

1. À ce stade, certains travaux orphelins peuvent être associés à PUB1 sur DIST2 ou DIST3. Chaque fois qu’un basculement se produit vers DIST2 et DIST3, des travaux orphelins associés à toutes les publications de PUB1 sont supprimés par le travail `Monitor and sync replication agent jobs`.

## <a name="add-subscription"></a>Ajouter un abonnement

Cet exemple montre comment configurer correctement des informations relatives aux abonnés sur les différents serveurs de distribution. L’exemple ajoute un abonné. DIST1 est le réplica principal actuel de la base de données de distribution du groupe de disponibilité. DIST2 et DIST3 sont des réplicas secondaires de la base de données de distribution du groupe de disponibilité. Le nom de l’abonné est SUB.

### <a name="publisher-workflow"></a>Flux de travail du serveur de publication

Sur le serveur de publication PUB, ajoutez l’abonnement à l’abonné `SUB`.

### <a name="distributor-workflow"></a>Flux de travail du serveur de distribution

Sur DIST2 et DIST3, ajoutez un serveur lié pour 'SUB' s’il n’a pas déjà été inscrit auprès de DIST2 ou DIST3. Voici un exemple TSQL concernant la création du serveur lié.

   ```sql 
   EXEC master.dbo.sp_addlinkedserver@server =N'SUB', @srvproduct=N'SQL Server'
   /* For security reasons the linked server remote logins password is changed with ######## */
   EXEC master.dbo.sp_addlinkedsrvlogin@rmtsrvname=N'SUB', @useself=N'True',@locallogin=NULL,@rmtuser=NULL,@rmtpassword=NULL
   ```

## <a name="add-a-pull-subscription"></a>Ajouter un abonnement par extraction

### <a name="subscriber-workflow"></a>Flux de travail de l’abonné

Pour ajouter un abonnement par extraction pour une publication dans la base de données de distribution du groupe de disponibilité, utilisez le nom de l’écouteur du groupe de disponibilité dans le paramètre `@distributor` de `sp_addpullsubscription_agent`.

## <a name="sample-t-sql-create-distribution-db-in-ag"></a>Exemple T-SQL pour la création d’une base de données de distribution dans le groupe de disponibilité

Le script suivant active une base de données de distribution dans un groupe de disponibilité. 

```sql
--- WorkFlow to Enable Distribution Database In AG.

-- SECTION 1 ---- CONFIGURE THE DISTRIBUTOR SERVERS

-- Step1 - Configure the Distribution DB nodes (AG Replicas) to act as a distributor
:Connect SQLNode1
sp_adddistributor @distributor = @@ServerName, @password = 'Pass@word1'
Go 
:Connect SQLNode2
sp_adddistributor @distributor = @@ServerName, @password = 'Pass@word1'
Go

-- Step2 - Configure the Distribution Database
:Connect SQLNode1
USE master
EXEC sp_adddistributiondb @database = 'DistributionDB', @security_mode = 1;
GO
Alter Database [DistributionDB] Set Recovery Full
Go
Backup Database [DistributionDB] to Disk = 'Nul'
Go
-- Step 3 - Create AG for the Distribution DB.
:Connect SQLNode1
USE [master]
GO
CREATE ENDPOINT [Hadr_endpoint] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)   
    FOR DATA_MIRRORING (ROLE = ALL, AUTHENTICATION = WINDOWS NEGOTIATE
, ENCRYPTION = REQUIRED ALGORITHM AES)
GO

:Connect SQLNode2
USE [master]
GO
CREATE ENDPOINT [Hadr_endpoint] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)   
    FOR DATA_MIRRORING (ROLE = ALL, AUTHENTICATION = WINDOWS NEGOTIATE
, ENCRYPTION = REQUIRED ALGORITHM AES)
GO

:Connect SQLNode1
-- Create the Availability Group
CREATE AVAILABILITY GROUP [DistributionDB_AG]
FOR DATABASE [DistributionDB]
REPLICA ON 'SQLNode1'
WITH (ENDPOINT_URL = N'TCP://SQLNode1.contoso.com:5022', 
         FAILOVER_MODE = AUTOMATIC, 
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
         BACKUP_PRIORITY = 50, 
         SECONDARY_ROLE(ALLOW_CONNECTIONS = ALL), 
         SEEDING_MODE = AUTOMATIC),
N'SQLNode2' WITH (ENDPOINT_URL = N'TCP://SQLNode2.contoso.com:5022', 
     FAILOVER_MODE = AUTOMATIC, 
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
     BACKUP_PRIORITY = 50, 
     SECONDARY_ROLE(ALLOW_CONNECTIONS = ALL), 
     SEEDING_MODE = AUTOMATIC);
 GO


:Connect SQLNode2
ALTER AVAILABILITY GROUP [DistributionDB_AG] JOIN
GO  
ALTER AVAILABILITY GROUP [DistributionDB_AG] GRANT CREATE ANY DATABASE
Go

--STEP4 - Create the Listener for the Availability Group. This is very important.
:Connect SQLNode1

USE [master]
GO
ALTER AVAILABILITY GROUP [DistributionDB_AG]
ADD LISTENER N'DistributionDBList' (
WITH IP
((N'10.0.0.8', N'255.255.255.0')) , PORT=1500);
GO

-- STEP 5 - Enable SQLNode2 also as a Distributor
:CONNECT SQLNODE2
EXEC sp_adddistributiondb @database = 'DistributionDB', @security_mode = 1;
GO

--STEP 6 - On all Distributor Nodes Configure the Publisher Details 
:CONNECT SQLNODE1
EXEC sp_addDistPublisher @publisher = 'SQLNode4', @distribution_db = 'DistributionDB', 
    @working_directory = '\\sqlfileshare\Dist_Work_Directory\'
GO
:CONNECT SQLNODE2
EXEC sp_addDistPublisher @publisher = 'SQLNode4', @distribution_db = 'DistributionDB', 
    @working_directory = '\\sqlfileshare\Dist_Work_Directory\'
GO

-- SECTION 2 ---- CONFIGURE THE PUBLISHER SERVER
:CONNECT SQLNODE4
EXEC sp_addDistributor @distributor = 'DistributionDBList', -- Listener for the Distribution DB.    
    @password = 'Pass@word1'
Go

-- SECTION 3 ---- CONFIGURE THE SUBSCRIBERS 
-- On Publisher, create the publication as one would normally do.
-- On the Secondary replicas of the Distribution DB, add the Subscriber as a linked server.
:CONNECT SQLNODE2
EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE5', @srvproduct=N'SQL Server'
 /* For security reasons the linked server remote logins password is changed with ######## */
EXEC master.dbo.sp_addlinkedsrvlogin @rmtsrvname=N'SQLNODE5',@useself=N'True',@locallogin=NULL,@rmtuser=NULL,@rmtpassword=NULL 
```

## <a name="see-also"></a>Voir aussi  
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Sécuriser le serveur de distribution](../../relational-databases/replication/security/secure-the-distributor.md)  
  
## <a name="next-steps"></a>Étapes suivantes
 [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](view-and-modify-distributor-and-publisher-properties.md)  
 [Désactiver la publication et la distribution](disable-publishing-and-distribution.md)  
 [Activer une base de données pour la réplication (SQL Server Management Studio)](enable-a-database-for-replication-sql-server-management-studio.md) 
