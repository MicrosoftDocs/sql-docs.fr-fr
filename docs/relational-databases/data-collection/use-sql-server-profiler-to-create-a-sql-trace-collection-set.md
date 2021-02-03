---
description: Utiliser SQL Server Profiler pour créer un jeu d'éléments de collecte Trace SQL
title: Créer un jeu d’éléments de collecte Trace SQL avec Profiler
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- SQL Trace collector set
ms.assetid: b6941dc0-50f5-475d-82eb-ce7c68117489
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 4b2f20965b3bdff081368c2e1fcaa2bc255aca47
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250088"
---
# <a name="use-sql-server-profiler-to-create-a-sql-trace-collection-set"></a>Utiliser SQL Server Profiler pour créer un jeu d'éléments de collecte Trace SQL
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , vous pouvez exploiter les fonctions de trace côté serveur de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour exporter une définition de trace que vous pouvez utiliser afin de créer un jeu d’éléments de collecte qui utilise le type de collecteur Trace SQL générique. Ce processus se décompose en deux parties :  
  
1.  Créer et exporter une trace [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  
  
2.  Écrire un nouveau script de jeu d'éléments de collecte basé sur une trace exportée.  

 Le scénario des procédures suivantes implique la collecte de données relatives à toute procédure stockée dont l'exécution requiert 80 millisecondes ou plus. Pour effectuer ces procédures, vous devez être en mesure :  
  
-   d'utiliser [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour créer et configurer une trace ;  
  
-   d'utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour ouvrir, modifier et exécuter une requête.  
  
### <a name="create-and-export-a-sql-server-profiler-trace"></a>Créer et exporter une trace SQL Server Profiler  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ouvrez [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. (Dans le menu **Outils** , cliquez sur **SQL Server Profiler**.)  
  
2.  Dans la boîte de dialogue **Se connecter au serveur** , cliquez sur **Annuler**.  
  
3.  Pour ce scénario, veillez à ce que les valeurs de durée soient configurées pour s'afficher en millisecondes (la valeur par défaut). Pour ce faire, procédez comme suit :  
  
    1.  Dans le menu **Outils** , cliquez sur **Options**.  
  
    2.  Dans la zone **Options d'affichage** , assurez-vous que la case à cocher Affiche les valeurs dans la colonne Durée en microsecondes est désactivée.  
  
    3.  Cliquez sur **OK** pour fermer la boîte de dialogue **Options générales** .  
  
4.  Dans le menu **Fichier** , cliquez sur **Nouvelle trace**.  
  
5.  Dans la boîte de dialogue **Se connecter au serveur** , sélectionnez le serveur auquel vous souhaitez vous connecter, puis cliquez sur **Se connecter**.  
  
     La boîte de dialogue **Propriétés de la trace** s'affiche.  
  
6.  Sous l’onglet **Général**, effectuez les actions suivantes :  
  
    1.  Dans la zone **Nom de la trace** , tapez le nom que vous souhaitez utiliser pour la trace. Pour cet exemple, le nom de la trace est **SPgt80**.  
  
    2.  Dans la liste **Utiliser le modèle**, sélectionnez le modèle à utiliser pour la trace. Pour cet exemple, cliquez sur **TSQL_SPs**.  
  
7.  Sous l'onglet **Sélection des événements** , procédez comme suit :  
  
    1.  Identifiez les événements à utiliser pour la trace. Pour cet exemple, désactivez toutes les cases à cocher de la colonne **Événements** , sauf **ExistingConnection** et **SP:Completed**.  
  
    2.  Dans l’angle inférieur droit, cochez la case **Afficher toutes les colonnes** .  
  
    3.  Cliquez sur la ligne **SP:Completed** .  
  
    4.  Faites défiler la ligne jusqu'à la colonne **Durée** , puis activez la case à cocher **Durée** .  
  
8.  Dans l’angle inférieur droit, cliquez sur **Filtres de colonnes** pour ouvrir la boîte de dialogue **Modifier le filtre** . Dans la boîte de dialogue **Modifier le filtre** , procédez comme suit :  
  
    1.  Dans la liste de filtres, cliquez sur **Durée**.  
  
    2.  Dans la fenêtre d’opérateur booléen, développez le nœud **Supérieur ou égal à** , tapez la valeur **80** , puis cliquez sur **OK**.  
  
9. Cliquez sur **Exécuter** pour démarrer la trace.  
  
10. Dans la barre d'outils, cliquez sur **Arrêter la trace sélectionnée** ou **Suspendre la trace sélectionnée**.  
  
11. Dans le menu **Fichier** , pointez sur **Exporter**, sur **Générer un script de la définition de la trace**, puis cliquez sur **Pour le jeu d'éléments de collecte Trace SQL**.  
  
12. Dans la boîte de dialogue **Enregistrer sous** , tapez le nom que vous souhaitez utiliser pour la définition de la trace dans la zone **Nom de fichier** , puis enregistrez-le à l'emplacement de votre choix. Pour cet exemple, le nom du fichier est identique au nom de la trace (SPgt80).  
  
13. Cliquez sur **OK** lorsqu'un message indiquant que le fichier a bien été enregistré s'affiche, puis fermez [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="script-a-new-collection-set-from-a-sql-server-profiler-trace"></a>Générer un nouveau jeu d'éléments de collecte à partir d'une trace SQL Server Profiler  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans le menu **Fichier** , pointez sur **Ouvrir** , puis cliquez sur **Fichier**.  
  
2.  Dans la boîte de dialogue **Ouvrir un fichier** , recherchez et ouvrez le fichier que vous avez créé au cours de la procédure précédente (SPgt80).  
  
     Les informations de trace que vous avez enregistrées sont ouvertes dans une fenêtre de requête et fusionnées dans un script que vous pouvez exécuter pour créer le jeu d'éléments de collecte.  
  
3.  Faites défiler le script et effectuez les remplacements suivants, notés dans le texte de commentaire du script :  
  
    -   Remplacez **SQLTrace Collection Set Name Here** par le nom que vous souhaitez utiliser pour le jeu d'éléments de collecte. Pour cet exemple, nommez le jeu d’éléments de collecte **SPROC_CollectionSet**.  
  
    -   Remplacez **SQLTrace Collection Item Name Here** par le nom que vous souhaitez utiliser pour l'élément de collecte. Pour cet exemple, nommez l’élément de collecte **SPROC_Collection_Item**.  
  
4.  Cliquez sur **Exécuter** pour exécuter la requête et créer le jeu d'éléments de collecte.  
  
5.  Dans l'Explorateur d'objets, vérifiez que le jeu d'éléments de collecte a été créé. Pour ce faire, procédez comme suit :  
  
    1.  Cliquez avec le bouton droit sur **Gestion**, puis cliquez sur **Actualiser**.  
  
    2.  Développez **Gestion**, puis **Collecte de données**.  
  
     Le jeu d’éléments de collecte **SPROC_CollectionSet** apparaît au même niveau que le nœud **Jeux d’éléments de collecte de données système** . Par défaut, le jeu d'éléments de collecte est désactivé.  
  
6.  Utilisez l'Explorateur d'objets pour modifier les propriétés de SPROC_CollectionSet, tel que le mode de collecte et la planification de téléchargement. Procédez de la même manière que pour les jeux d'éléments de collecte de données système fournis avec le collecteur de données.  
  
## <a name="example"></a>Exemple  
 L'exemple de code suivant est le script final obtenu après l'exécution des étapes documentées dans les procédures précédentes.  
  
```sql
/*************************************************************/  
-- SQL Trace collection set generated from SQL Server Profiler  
-- Date: 11/19/2007  12:55:31 AM  
/*************************************************************/  
  
USE msdb;
GO  
  
BEGIN TRANSACTION  
BEGIN TRY  
  
-- Define collection set  
-- **_  
-- _*_ Replace 'SqlTrace Collection Set Name Here' in the   
-- _*_ following script with the name you want  
-- _*_ to use for the collection set.  
-- _*_  
DECLARE @collection_set_id int;  
EXEC [dbo].[sp_syscollector_create_collection_set]  
    @name = N'SPROC_CollectionSet',  
    @schedule_name = N'CollectorSchedule_Every_15min',  
    @collection_mode = 0, -- cached mode needed for Trace collections  
    @logging_level = 0, -- minimum logging  
    @days_until_expiration = 5,  
    @description = N'Collection set generated by SQL Server Profiler',  
    @collection_set_id = @collection_set_id output;  
SELECT @collection_set_id;  
  
-- Define input and output variables for the collection item.  
DECLARE @trace_definition xml;  
DECLARE @collection_item_id int;  
  
-- Define the trace parameters as an XML variable  
SELECT @trace_definition = convert(xml,   
N'<ns:SqlTraceCollector xmlns:ns"DataCollectorType" use_default="0">  
<Events>  
  <EventType name="Sessions">  
    <Event id="17" name="ExistingConnection" columnslist="1,2,14,26,3,35,12" />  
  </EventType>  
  <EventType name="Stored Procedures">  
    <Event id="43" name="SP:Completed" columnslist="1,2,26,34,3,35,12,13,14,22" />  
  </EventType>  
</Events>  
<Filters>  
  <Filter columnid="13" columnname="Duration" logical_operator="AND" comparison_operator="GE" value="80000L" />  
</Filters>  
</ns:SqlTraceCollector>  
');  
  
-- Retrieve the collector type GUID for the trace collector type.  
DECLARE @collector_type_GUID uniqueidentifier;  
SELECT @collector_type_GUID = collector_type_uid
  FROM [dbo].[syscollector_collector_types]
  WHERE name = N'Generic SQL Trace Collector Type';  
  
-- Create the trace collection item.  
-- _*_  
-- _*_ Replace 'SqlTrace Collection Item Name Here' in   
-- _*_ the following script with the name you want to  
-- _*_ use for the collection item.  
-- _**  
EXEC [dbo].[sp_syscollector_create_collection_item]  
   @collection_set_id = @collection_set_id,  
   @collector_type_uid = @collector_type_GUID,  
   @name = N'SPROC_Collection_Item',  
   @frequency = 900, -- specified the frequency for checking to see if trace is still running  
   @parameters = @trace_definition,  
   @collection_item_id = @collection_item_id output;  
SELECT @collection_item_id;  
  
COMMIT TRANSACTION;  
END TRY  
  
BEGIN CATCH  
ROLLBACK TRANSACTION;  
DECLARE @ErrorMessage nvarchar(4000);  
DECLARE @ErrorSeverity int;  
DECLARE @ErrorState int;  
DECLARE @ErrorNumber int;  
DECLARE @ErrorLine int;  
DECLARE @ErrorProcedure nvarchar(200);  
SELECT @ErrorLine = ERROR_LINE(),  
       @ErrorSeverity = ERROR_SEVERITY(),  
       @ErrorState = ERROR_STATE(),  
       @ErrorNumber = ERROR_NUMBER(),  
       @ErrorMessage = ERROR_MESSAGE(),  
       @ErrorProcedure = ISNULL(ERROR_PROCEDURE(), '-');  
RAISERROR (14684, @ErrorSeverity, 1 , @ErrorNumber,
  @ErrorSeverity, @ErrorState, @ErrorProcedure, @ErrorLine, @ErrorMessage);  
END CATCH;  
GO  
```  
  
  
