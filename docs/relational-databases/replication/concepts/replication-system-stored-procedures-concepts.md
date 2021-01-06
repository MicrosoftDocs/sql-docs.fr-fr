---
description: Concepts liés aux procédures stockées système de réplication
title: Concepts liés aux procédures stockées système de réplication | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- stored procedures [SQL Server replication], programming
- programming [SQL Server replication], system stored procedures
- programming interfaces [SQL Server replication]
- system stored procedures [SQL Server replication]
- replication [SQL Server], how-to topics
ms.assetid: 816d2bda-ed72-43ec-aa4d-7ee3dc25fd8a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: eb082b29411b136edca7ed14f06effa354a5f0e6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643095"
---
# <a name="replication-system-stored-procedures-concepts"></a>Concepts liés aux procédures stockées système de réplication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'accès par programme à toutes les fonctionnalités d'une topologie de réplication configurables par l'utilisateur est opéré par les procédures stockées système. Bien que les procédures stockées puissent être exécutées individuellement à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de l'utilitaire en ligne de commande sqlcmd, il peut être judicieux d'écrire des fichiers de script [!INCLUDE[tsql](../../../includes/tsql-md.md)] qui peuvent être exécutés pour effectuer une séquence logique de tâches de réplication.  
  
 La génération de scripts pour des tâches de réplication offre les avantages suivants :  
  
-   conservation d'une copie définitive des étapes utilisées pour déployer votre topologie de réplication ;  
  
-   utilisation d'un même script pour configurer plusieurs abonnés ;  
  
-   formation rapide des nouveaux administrateurs de base de données en leur permettant d'évaluer, de comprendre et de modifier le code, ou de résoudre des problèmes liés au code.  
  
    > [!IMPORTANT]  
    >  Les scripts peuvent causer des failles de sécurité ; ils peuvent appeler des fonctions système sans que l'utilisateur le sache ou intervienne. En outre, ils sont susceptibles de contenir des informations d'identification de sécurité sous forme de texte brut. Examinez les scripts pour détecter d'éventuels problèmes de sécurité avant de les utiliser.  
  
## <a name="creating-replication-scripts"></a>Création de scripts de réplication  
 Du point de la réplication, un script consiste en une série d'instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)], chacune exécutant une procédure stockée de réplication. Les scripts sont des fichiers texte, souvent dotés d'une extension de fichier .sql, qui peuvent être exécutés à l'aide de l'utilitaire sqlcmd. Lorsqu'un fichier de script est exécuté, l'utilitaire exécute les instructions SQL stockées dans le fichier. De même, un script peut être stocké sous la forme d’un objet de requête dans un projet [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
 Les méthodes suivantes peuvent être employées pour créer des scripts de réplication :  
  
-   création manuelle du script ;  
  
-   utilisation des fonctionnalités de génération de script fournies dans les Assistants de réplication ou  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Pour plus d'informations, voir [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
-   utilisation de Replication Management Objects pour générer le script par programme et créer un objet RMO.  
  
 Lorsque vous créez des scripts de réplication manuellement, gardez à l'esprit les points suivants :  
  
-   Les scripts [!INCLUDE[tsql](../../../includes/tsql-md.md)] comportent un ou plusieurs lots. La commande GO signale la fin d'un lot. Si un script [!INCLUDE[tsql](../../../includes/tsql-md.md)] ne comporte pas de commande GO, il est exécuté comme un lot isolé.  
  
-   Lors de l'exécution de plusieurs procédures stockées de réplication dans un lot unique, après la première procédure, toutes les procédures suivantes du lot doivent être précédées du mot clé EXECUTE.  
  
-   Toutes les procédures stockées d'un lot doivent être compilées pour que le lot puisse être exécuté. Toutefois, lorsque le lot a été compilé et qu'un plan d'exécution a été créé, une erreur d'exécution peut éventuellement se produire.  
  
-   Lorsque vous créez des scripts pour configurer la réplication, vous devez utiliser l'authentification Windows de sorte que les informations d'identification de sécurité ne soient pas stockées dans le fichier de script. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
## <a name="sample-replication-script"></a>Exemple de script de réplication  
 Le script suivant peut être exécuté pour configurer la publication et la distribution sur un serveur.  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
-- Specify the replication working directory.  
SET @directory = N'\\' + $(DistPubServer) + '\repldata';  
-- Specify the publication database.  
SET @publicationDB = N'AdventureWorks2012';   
  
-- Install the server MYDISTPUB as a Distributor using the defaults,  
-- including autogenerating the distributor password.  
USE master  
EXEC sp_adddistributor @distributor = @distributor;  
  
-- Create a new distribution database using the defaults, including  
-- using Windows Authentication.  
USE master  
EXEC sp_adddistributiondb @database = @distributionDB,   
    @security_mode = 1;  
GO  
  
-- Create a Publisher and enable AdventureWorks2012 for replication.  
-- Add MYDISTPUB as a publisher with MYDISTPUB as a local distributor  
-- and use Windows Authentication.  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
USE [distribution]  
EXEC sp_adddistpublisher @publisher=@publisher,   
    @distribution_db=@distributionDB,   
    @security_mode = 1;  
GO  
  
```  
  
 Ce script peut ensuite être enregistré localement sous le nom `instdistpub.sql` afin d'être exécuté ou réexécuté en cas de besoin.  
  
 Le script précédent inclut des variables de script **sqlcmd**, lesquelles sont utilisées dans de nombreux exemples de code de réplication dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les variables de script sont définies à l’aide de la syntaxe `$(MyVariable)`. Les valeurs des variables peuvent être transmises à un script sur la ligne de commande ou dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Pour plus d'informations, consultez la section suivante de cette rubrique, « Exécution de scripts de réplication ».  
  
## <a name="executing-replication-scripts"></a>Exécution de scripts de réplication  
 Une fois qu'un script de réplication a été créé, il est possible d'utiliser l'une des méthodes suivantes pour l'exécuter.  
  
### <a name="creating-a-sql-query-file-in-sql-server-management-studio"></a>Création d'un fichier de requête SQL dans SQL Server Management Studio  
 Il est possible de créer un fichier de script [!INCLUDE[tsql](../../../includes/tsql-md.md)] de réplication sous la forme d’un fichier de requête SQL dans un projet [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Après l'écriture du script, une connexion à la base de données peut être établie pour ce fichier de requête, et le script peut être exécuté. Pour plus d’informations sur la manière de créer des scripts [!INCLUDE[tsql](../../../includes/tsql-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], consultez [Éditeurs de texte et de requête &#40;SQL Server Management Studio&#41;](../../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md).  
  
 Pour utiliser un script qui inclut des variables de script, [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] doit s’exécuter en mode **sqlcmd**. En mode **sqlcmd**, l’éditeur de requête accepte une syntaxe supplémentaire spécifique à **sqlcmd**, par exemple `:setvar`, qui est utilisée pour fournir une valeur à une variable. Pour plus d’informations sur le mode **sqlcmd**, consultez [Modifier des scripts SQLCMD à l’aide de l’Éditeur de requête](../../../ssms/scripting/edit-sqlcmd-scripts-with-query-editor.md). Dans le script suivant, la syntaxe `:setvar` est utilisée afin de fournir une valeur pour la variable `$(DistPubServer)`.  
  
```  
:setvar DistPubServer N'MyPublisherAndDistributor';  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
--  
-- Additional code goes here  
--  
```  
  
### <a name="using-the-sqlcmd-utility-from-the-command-line"></a>Utilisation de l'utilitaire sqlcmd à partir de la ligne de commande  
 L’exemple suivant montre comment la ligne de commande est utilisée pour exécuter le fichier de script `instdistpub.sql` à l’aide de l’[utilitaire sqlcmd](../../../tools/sqlcmd-utility.md):  
  
```  
sqlcmd.exe -E -S sqlserverinstance -i C:\instdistpub.sql -o C:\output.log -v DistPubServer="N'MyDistributorAndPublisher'"  
```  
  
 Dans cet exemple, le commutateur `-E` indique que l'authentification Windows est employée lors de la connexion à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Avec l'authentification Windows, il est inutile de stocker un nom d'utilisateur et un mot de passe dans le fichier de script. Le nom et le chemin d'accès du fichier de script sont spécifiés par le commutateur `-i`, et le nom du fichier de sortie est spécifié par le commutateur `-o` (la sortie de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est écrite dans ce fichier et non dans la console lorsque ce commutateur est utilisé). L'utilitaire `sqlcmd` vous permet de passer des variables de script à un script [!INCLUDE[tsql](../../../includes/tsql-md.md)] lors de l'exécution, à l'aide du commutateur `-v`. Dans cet exemple, `sqlcmd` remplace chaque instance de `$(DistPubServer)` dans le script par la valeur `N'MyDistributorAndPublisher'` avant l’exécution.  
  
> [!NOTE]  
>  Le commutateur `-X` désactive les variables de script.  
  
### <a name="automating-tasks-in-a-batch-file"></a>Automatisation des tâches dans un fichier de commandes  
 Le recours à un fichier de commandes permet d'automatiser les tâches d'administration et de synchronisation de la réplication, entre autres, dans le même fichier de commandes. Le fichier de commandes suivant utilise l’utilitaire **sqlcmd** pour supprimer la base de données d’abonnement et la recréer, ainsi que pour ajouter un abonnement de fusion par extraction de données (pull). Le fichier appelle ensuite l'agent de fusion pour synchroniser le nouvel abonnement :  
  
```  
REM ----------------------Script to synchronize merge subscription ----------------------  
REM -- Creates subscription database and   
REM -- synchronizes the subscription to MergeSalesPerson.  
REM -- Current computer acts as both Publisher and Subscriber.  
REM -------------------------------------------------------------------------------------  
  
SET Publisher=%computername%  
SET Subscriber=%computername%  
SET PubDb=AdventureWorks  
SET SubDb=AdventureWorksReplica  
SET PubName=AdvWorksSalesOrdersMerge  
  
REM -- Drop and recreate the subscription database at the Subscriber  
sqlcmd /S%Subscriber% /E /Q"USE master IF EXISTS (SELECT * FROM sysdatabases WHERE name='%SubDb%' ) DROP DATABASE %SubDb%"  
sqlcmd /S%Subscriber% /E /Q"USE master CREATE DATABASE %SubDb%"  
  
REM -- Add a pull subscription at the Subscriber  
sqlcmd /S%Subscriber% /E /Q"USE %SubDb% EXEC sp_addmergepullsubscription @publisher = %Publisher%, @publication = %PubName%, @publisher_db = %PubDb%"  
sqlcmd /S%Subscriber% /E /Q"USE %SubDb%  EXEC sp_addmergepullsubscription_agent @publisher = %Publisher%, @publisher_db = %PubDb%, @publication = %PubName%, @subscriber = %Subscriber%, @subscriber_db = %SubDb%, @distributor = %Publisher%"  
  
REM -- This batch file starts the merge agent at the Subscriber to   
REM -- synchronize a pull subscription to a merge publication.  
REM -- The following must be supplied on one line.  
"\Program Files\Microsoft SQL Server\130\COM\REPLMERG.EXE"  -Publisher  %Publisher% -Subscriber  %Subscriber%  -Distributor %Publisher%  -PublisherDB  %PubDb% -SubscriberDB %SubDb% -Publication %PubName% -PublisherSecurityMode 1 -OutputVerboseLevel 1  -Output  -SubscriberSecurityMode 1  -SubscriptionType 1 -DistributorSecurityMode 1 -Validate 3  
  
```  
  
## <a name="scripting-common-replication-tasks"></a>Génération de scripts pour les tâches de réplication courantes  
 Les tâches de réplication les plus courantes pour lesquelles il est possible de générer des scripts à l'aide de procédures stockées système sont les suivantes :  
  
-   Configuration de la publication et de la distribution  
  
-   Modification des propriétés d'un serveur de distribution et d'un serveur de publication  
  
-   Désactivation de la publication et de la distribution  
  
-   Création de publications et définition d'articles  
  
-   Suppression de publications et d'articles  
  
-   Création d'un abonnement par extraction de données  
  
-   Modification d'un abonnement par extraction de données  
  
-   Suppression d'un abonnement par extraction de données  
  
-   Création d'un abonnement par émission de données  
  
-   Modification d'un abonnement par émission de données  
  
-   Suppression d'un abonnement par émission de données  
  
-   Synchronisation d'un abonnement par extraction de données  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de programmation en matière de réplication](../../../relational-databases/replication/concepts/replication-programming-concepts.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Création de scripts de réplication](../../../relational-databases/replication/scripting-replication.md)  
  
