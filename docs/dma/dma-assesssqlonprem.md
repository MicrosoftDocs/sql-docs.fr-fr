---
title: Effectuer une évaluation de la migration SQL Server
titleSuffix: Data Migration Assistant
description: Découvrez comment utiliser Assistant Migration de données pour évaluer un SQL Server local avant de migrer vers un autre SQL Server ou Azure SQL Database
ms.date: 01/15/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 300d88b66c2105235ab04ff616d9fcf81b24e944
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247333"
---
# <a name="perform-a-sql-server-migration-assessment-with-data-migration-assistant"></a>Effectuer une évaluation de migration SQL Server avec l’assistant Migration de données

Les instructions pas à pas suivantes vous aident à effectuer la première évaluation de la migration vers des SQL Server locaux, SQL Server s’exécutant sur une machine virtuelle Azure ou Azure SQL Database à l’aide de Assistant Migration de données.

Assistant Migration de données v 5.0 introduit la prise en charge de l’analyse de la connectivité de base de données et des requêtes SQL incorporées dans le code de l’application. Pour plus d’informations, consultez le billet de blog [Using Assistant Migration de données pour évaluer la couche d’accès aux données d’une application](https://techcommunity.microsoft.com/t5/Microsoft-Data-Migration/Using-Data-Migration-Assistant-to-assess-an-application-s-data/ba-p/990430).

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

## <a name="create-an-assessment"></a>Créer une évaluation

1. Sélectionnez l’icône **nouveau** (+), puis sélectionnez le type de projet **évaluation** .

2. Définissez le type de serveur source et cible.

    Si vous effectuez la mise à niveau de votre instance locale de SQL Server vers une instance SQL Server locale moderne ou SQL Server hébergée sur une machine virtuelle Azure, définissez le type de serveur source et cible sur **SQL Server**. Si vous effectuez une migration vers Azure SQL Database, définissez plutôt le type de serveur cible sur **Azure SQL Database**.

3. Cliquez sur **Créer**.

   ![Créer une évaluation](../dma/media/dma-assesssqlonprem/new-assessment.png)

## <a name="choose-assessment-options"></a>Choisir les options d’évaluation

1. Sélectionnez la version de SQL Server cible vers laquelle vous envisagez de migrer.

2. Sélectionnez le type de rapport.

   Lorsque vous évaluez votre instance de SQL Server source pour la migration vers un SQL Server local ou vers SQL Server hébergée sur des cibles de machines virtuelles Azure, vous pouvez choisir l’un des types de rapport d’évaluation suivants, ou les deux :

    - **Problèmes de compatibilité**
    - **Recommandations relatives aux nouvelles fonctionnalités**

   ![Sélectionner un type de rapport d’évaluation pour SQL Server cible](../dma/media/dma-assesssqlonprem/assessment-types.png)

   Lors de l’évaluation de votre instance de SQL Server source pour la migration vers Azure SQL Database, vous pouvez choisir l’un des types de rapport d’évaluation suivants, ou les deux :

    - **Vérifier la compatibilité de la base de données**
    - **Vérifier la parité de fonctionnalité**

    ![Sélectionner le type de rapport d’évaluation pour SQL Database cible](../dma/media/dma-assesssqlonprem/assessment-types-azure.png)

## <a name="add-databases-and-extended-events-trace-to-assess"></a>Ajouter des bases de données et une trace des événements étendus pour évaluer

1. Sélectionnez **Ajouter des sources** pour ouvrir le menu contextuel connexion.

2. Entrez le nom de l’instance SQL Server, choisissez le type d’authentification, définissez les propriétés de connexion appropriées, puis sélectionnez **se connecter**.

3. Sélectionnez les bases de données à évaluer, puis sélectionnez **Ajouter**.

    > [!NOTE]
    > Vous pouvez supprimer plusieurs bases de données en les sélectionnant tout en maintenant la touche Maj ou CTRL enfoncée, puis en cliquant sur **Supprimer les sources**. Vous pouvez également ajouter des bases de données à partir de plusieurs instances de SQL Server en sélectionnant **Ajouter des sources**.

4. Si vous avez des requêtes SQL ad hoc ou dynamiques ou des instructions DML lancées par le biais de la couche de données d’application, entrez le chemin d’accès au dossier dans lequel vous avez placé tous les fichiers de session d’événements étendus que vous avez collectés pour capturer la charge de travail sur le SQL Server source.

     L’exemple suivant montre comment créer une session d’événements étendus sur votre SQL Server source pour capturer la charge de travail de la couche de données de l’application.  Capturez la charge de travail pour la durée qui représente votre charge de travail de pointe.

    ```
    DROP EVENT SESSION [DatalayerSession] ON SERVER
    go
    CREATE EVENT SESSION [DatalayerSession] ON SERVER  
    ADD EVENT sqlserver.sql_batch_completed( 
        ACTION (sqlserver.sql_text,sqlserver.client_app_name,sqlserver.client_hostname,sqlserver.database_id))
    ADD TARGET package0.asynchronous_file_target(SET filename=N'C:\temp\Demos\DataLayerAppassess\DatalayerSession.xel')  
    WITH (MAX_MEMORY=2048 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=3 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)
    go
    ---Start the session
    ALTER EVENT SESSION [DatalayerSession]
          ON SERVER
        STATE = START;
    ---Wait for few minutes
    
    ---Query events
        
        SELECT 
        object_name,
        CAST(event_data as xml) as event_data,
        file_name, 
        file_offset
    FROM sys.fn_xe_file_target_read_file('C:\temp\Demos\DataLayerAppassess\DatalayerSession*xel', 
                'C:\\temp\\Demos\\DataLayerAppassess\\DatalayerSession*xem', 
                null,
                null)
    ---Stop the session after capturing the peak load.
    ALTER EVENT SESSION [DatalayerSession]
          ON SERVER
        STATE = STOP;
        
        go
    ```

5. Cliquez sur **Suivant** pour démarrer l’évaluation.

    ![Ajouter des sources et démarrer l’évaluation](../dma/media/dma-assesssqlonprem/select-database1.png)

> [!NOTE]
> Vous pouvez exécuter plusieurs évaluations simultanément et afficher l’état des évaluations en ouvrant la page **All Assessments** (Toutes les évaluations).

## <a name="view-results"></a>Afficher les résultats

La durée de l’évaluation dépend du nombre de bases de données ajoutées et de la taille du schéma de chaque base de données. Les résultats sont affichés pour chaque base de données dès qu’ils sont disponibles.

1. Sélectionnez la base de données qui a terminé l’évaluation, puis basculez entre les **problèmes de compatibilité** et les **recommandations relatives aux fonctionnalités** à l’aide du sélecteur.

2. Examinez les problèmes de compatibilité entre tous les niveaux de compatibilité pris en charge par la version de SQL Server cible que vous avez sélectionnée dans la page **options** .

Vous pouvez examiner les problèmes de compatibilité en analysant l’objet affecté, ses détails et éventuellement un correctif pour chaque problème identifié sous **modifications avec rupture**, **changements de comportement** et **fonctionnalités dépréciées**.

![Afficher les résultats de l’évaluation](../dma/media/dma-assesssqlonprem/review-results.png)

De même, vous pouvez passer en revue les recommandations relatives aux fonctionnalités dans les domaines des **performances**, du **stockage** et de la **sécurité** .

Les recommandations relatives aux fonctionnalités couvrent différents types de fonctionnalités, telles que In-Memory OLTP, ColumnStore, Stretch Database, Always Encrypted, Dynamic Data Masking et Transparent Data Encryption.

![Afficher les recommandations relatives aux fonctionnalités](../dma/media/dma-assesssqlonprem/feature-recommendations.png)

Par Azure SQL Database, les évaluations fournissent des problèmes de blocage de la migration et des problèmes de parité de fonctionnalité. Passez en revue les résultats des deux catégories en sélectionnant les options spécifiques.

- La catégorie de **parité de fonctionnalité de SQL Server** fournit un ensemble complet de recommandations, d’approches alternatives disponibles dans Azure et d’atténuation des étapes. Il vous aide à planifier cet effort dans vos projets de migration.

  ![Afficher des informations sur SQL Server parité des fonctionnalités](../dma/media/dma-assesssqlonprem/sql-feature-parity.png)

- La catégorie de **problèmes de compatibilité** fournit des fonctionnalités partiellement prises en charge ou non prises en charge qui bloquent la migration de bases de données SQL Serveres locales vers des bases de données SQL Azure. Il fournit ensuite des recommandations pour vous aider à résoudre ces problèmes.

  ![Afficher les problèmes de compatibilité](../dma/media/dma-assesssqlonprem/compatibility-issues.png)

## <a name="assess-a-data-estate-for-target-readiness"></a>Évaluer un patrimoine de données pour la préparation cible

Si vous souhaitez étendre davantage ces évaluations à l’ensemble de l’espace de données et rechercher la disponibilité relative de SQL Server instances et de bases de données pour la migration vers Azure SQL Database, téléchargez les résultats dans le Hub Azure Migrate en sélectionnant **Télécharger pour Azure Migrate**.

Cela vous permet d’afficher les résultats consolidés sur le projet Azure Migrate Hub.

Des instructions pas à pas détaillées sur les évaluations de la disponibilité cible sont disponibles [ici](./dma-assess-sql-data-estate-to-sqldb.md).

   ![Charger les résultats dans Azure Migrate](../dma/media/dma-assesssqlonprem/upload-to-azure-migrate.png)

## <a name="export-results"></a>Exporter les résultats

Une fois que toutes les bases de données ont terminé l’évaluation, sélectionnez **Exporter le rapport** pour exporter les résultats vers un fichier JSON ou un fichier CSV. Vous pouvez ensuite analyser les données à votre convenance.

## <a name="save-and-load-assessments"></a>Enregistrer et charger des évaluations

Outre l’exportation des résultats d’une évaluation, vous pouvez enregistrer les détails de l’évaluation dans un fichier et charger un fichier d’évaluation en vue d’une révision ultérieure.  Pour plus d’informations, consultez l’article [enregistrer et charger des évaluations avec Assistant Migration de données](../dma/dma-save-load-assessments.md).
