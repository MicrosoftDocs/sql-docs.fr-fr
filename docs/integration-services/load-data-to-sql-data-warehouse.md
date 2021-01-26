---
title: Chargement de données dans Azure Synapse Analytics avec SQL Server Integration Services (SSIS)
description: Montre comment créer un package SQL Server Integration Services (SSIS) pour déplacer des données dans un pool SQL dédié dans Azure Synapse Analytics à partir d’un large éventail de sources de données.
documentationcenter: NA
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
ms.custom: loading
ms.date: 08/09/2018
ms.author: chugu
author: chugugrace
ms.openlocfilehash: 06c69fb6b40fad1f6440583b693719767d6ad1d2
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597273"
---
# <a name="load-data-into-a-dedicated-sql-pool-in-azure-synapse-analytics-with-sql-server-integration-services-ssis"></a>Charger des données dans un pool SQL dédié dans Azure Synapse Analytics avec SQL Server Integration Services (SSIS)

[!INCLUDE[asa](../includes/applies-to-version/asa.md)]

Créez un package SSIS (SQL Server Integration Services) pour charger des données dans un pool SQL dédié dans Azure Synapse Analytics](/azure/sql-data-warehouse/index). Vous pouvez éventuellement restructurer, transformer et nettoyer les données qui traversent le flux de données SSIS.

Cet article vous montre comment effectuer les opérations suivantes :

* Créer un projet Integration Services dans Visual Studio.
* Concevoir un package SSIS qui charge des données de la source vers la destination.
* Exécuter le package SSIS pour charger les données.

## <a name="basic-concepts"></a>Concepts de base

Le package est l’unité de travail de base dans SSIS. Les packages associés sont regroupés en projets. Vous créez des projets et concevez des packages dans Visual Studio avec SQL Server Data Tools. Le processus de conception est un processus visuel au cours duquel vous faites glisser et déposez des composants issus de la boîte à outils dans l’aire de conception, les connectez et définissez leurs propriétés. Une fois que vous avez terminé votre package, vous pouvez l’exécuter et éventuellement le déployer sur SQL Server ou SQL Database pour le gérer, le superviser et le sécuriser complètement.

La présentation détaillée de SSIS dépasse le cadre de cet article. Pour en savoir plus, consultez les articles suivants :

- [SQL Server Integration Services](sql-server-integration-services.md)

- [SSIS : comment créer un package ETL](ssis-how-to-create-an-etl-package.md)

## <a name="options-for-loading-data-into-azure-synapse-analytics-with-ssis"></a>Options de chargement de données dans Azure Synapse Analytics avec SSIS
SQL Server Integration Services (SSIS) est un ensemble d’outils flexible qui fournit diverses options pour se connecter à Azure Synapse Analytics et y charger des données.

1. La méthode recommandée, qui offre des performances optimales, consiste à créer un package qui utilise la [tâche de chargement Azure SQL DW](control-flow/azure-sql-dw-upload-task.md) pour charger les données. Cette tâche encapsule à la fois les informations sur la source et la destination. Elle suppose que vos données sources sont stockées localement dans des fichiers texte délimité.

2. Vous pouvez également créer un package qui utilise une tâche de flux de données qui contient une source et une destination. Cette approche accepte un large éventail de sources de données, dont SQL Server et Azure Synapse Analytics.

## <a name="prerequisites"></a>Conditions préalables requises
Pour exécuter pas à pas ce tutoriel, vous avez besoin des éléments suivants :

1. **SQL Server Integration Services (SSIS)** . SSIS est un composant de SQL Server et requiert une version sous licence, ou la version de développeur ou d’évaluation, de SQL Server. Pour obtenir une version d’évaluation de SQL Server, consultez [Évaluer SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm).
2. **Visual Studio** (facultatif). Pour obtenir l’édition Visual Studio Community gratuite, consultez [Visual Studio Community][Visual Studio Community]. Si vous ne souhaitez pas installer Visual Studio, vous pouvez installer uniquement SSDT (SQL Server Data Tools). SSDT installe une version de Visual Studio avec des fonctionnalités limitées.
3. **SQL Server Data Tools pour Visual Studio (SSDT)** . Pour obtenir SQL Server Data Tools pour Visual Studio, consultez [Télécharger SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].
4. **Une base de données Azure Synapse Analytics et des autorisations**. Ce tutoriel se connecte à un pool SQL dédié dans l’instance Azure Synapse Analytics et y charge des données. Vous devez disposer des autorisations pour vous connecter, créer une table et charger des données.

## <a name="create-a-new-integration-services-project"></a>Créer un projet Integration Services
1. Lancez Visual Studio.
2. Dans le menu **Fichier**, sélectionnez **Nouveau | Projet**.
3. Accédez aux types de projet **Installé | Modèles | Business Intelligence | Integration Services**.
4. Sélectionnez **Projet Integration Services**. Fournissez des valeurs pour **Nom** et **Emplacement**, puis sélectionnez **OK**.

Visual Studio s’ouvre et crée un nouveau projet Integration Services (SSIS). Ensuite, Visual Studio ouvre le concepteur pour le nouveau package SSIS individuel (Package.dtsx) dans le projet. Vous voyez les zones d’écran suivantes :

* À gauche, la **boîte à outils** des composants SSIS.
* Au milieu, l’aire de conception avec plusieurs onglets. Vous utilisez généralement au moins les onglets **Flux de contrôle** et **Flux de données**.
* À droite, les volets **Explorateur de solutions** et **Propriétés**.
  
    ![Capture d’écran de Visual Studio montrant le volet Boîte à outils, le volet de conception, le volet Explorateur de solutions et le volet Propriétés.][01]

## <a name="option-1---use-the-sql-dw-upload-task"></a>Option 1 : Utiliser la tâche de chargement SQL DW

La première approche est un package qui utilise la tâche de chargement SQL DW. Cette tâche encapsule à la fois les informations sur la source et la destination. Elle suppose que vos données sources sont stockées dans des fichiers texte délimité, localement ou dans le stockage Blob Azure.

### <a name="prerequisites-for-option-1"></a>Prérequis pour l’option 1

Pour continuer le tutoriel avec cette option, vous avez besoin des éléments suivants :

- [Microsoft SQL Server Integration Services Feature Pack pour Azure][Microsoft SQL Server 2017 Integration Services Feature Pack for Azure]. La tâche de chargement SQL DW est un composant de Feature Pack.

- Compte de [stockage Blob Azure](/azure/storage/). La tâche de chargement SQL DW charge des données du Stockage Blob Azure dans Azure Synapse Analytics. Vous pouvez charger des fichiers qui se trouvent déjà dans Stockage Blob ou vous pouvez charger des fichiers à partir de votre ordinateur. Si vous sélectionnez des fichiers sur votre ordinateur, la tâche de chargement SQL DW les charge d’abord sur Stockage Blob pour la mise en lots, puis les charge dans votre pool SQL dédié.

### <a name="add-and-configure-the-sql-dw-upload-task"></a>Ajouter et configurer la tâche de chargement SQL DW

1. Faites glisser une tâche de chargement SQL DW de la boîte à outils jusqu’au centre de l’aire de conception (sous l’onglet **Flux de contrôle**).

2. Double-cliquez sur la tâche pour ouvrir **l’éditeur de tâche de chargement SQL DW**.

    ![Page Général de l’éditeur de tâche de chargement SQL DW](media/load-data-to-sql-data-warehouse/azure-sql-dw-upload-task-editor.png)

3. Configurez la tâche en suivant les conseils de l’article [Tâche de chargement Azure SQL DW](control-flow/azure-sql-dw-upload-task.md). Comme cette tâche encapsule à la fois les informations sur la source et la destination ainsi que les mappages entre les tables source et de destination, l’éditeur de tâche comporte plusieurs pages de paramètres à configurer.

### <a name="create-a-similar-solution-manually"></a>Créer une solution similaire manuellement

Pour plus de contrôle, vous pouvez créer manuellement un package qui émule le travail effectué par la tâche de chargement SQL DW. 

1. Utilisez la tâche de chargement d'objet blob Azure pour effectuer une copie intermédiaire des données dans Stockage Blob Azure. Pour obtenir la tâche de chargement d’objets blob Azure, téléchargez [Microsoft SQL Server Integration Services Feature Pack pour Azure][Microsoft SQL Server 2017 Integration Services Feature Pack for Azure].

2. Ensuite, utilisez la tâche SSIS d’exécution de requêtes SQL pour lancer un script PolyBase qui charge les données dans votre pool SQL dédié. Pour obtenir un exemple qui charge des données de Stockage Blob Azure dans un pool SQL dédié (mais pas avec SSIS), consultez [Tutoriel : Chargement de données dans Azure Synapse Analytics](/azure/sql-data-warehouse/load-data-wideworldimportersdw).

## <a name="option-2---use-a-source-and-destination"></a>Option 2 : Utiliser une source et une destination

La deuxième approche est un package classique qui utilise une tâche de flux de données qui contient une source et une destination. Cette approche accepte un large éventail de sources de données, dont SQL Server et Azure Synapse Analytics.

Ce didacticiel utilise SQL Server comme source de données. SQL Server est exécuté localement ou sur une machine virtuelle Azure.

Pour vous connecter à SQL Server et à un pool SQL dédié, vous pouvez utiliser un gestionnaire de connexions ADO.NET ainsi que la source et la destination, ou un gestionnaire de connexions OLE DB ainsi que la source et la destination. Ce tutoriel utilise ADO.NET, car il présente le moins d’options de configuration. OLE DB peut fournir des performances légèrement meilleures qu’ADO.NET.

Pour aller plus vite, vous pouvez utiliser l’Assistant Importation et Exportation SQL Server pour créer le package de base. Ensuite, enregistrez le package et ouvrez-le dans Visual Studio ou SSDT pour l’afficher et le personnaliser. Pour plus d’informations, consultez [Importer et exporter des données avec l’Assistant Importation et Exportation SQL Server](import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

### <a name="prerequisites-for-option-2"></a>Prérequis pour l’option 2

Pour continuer le tutoriel avec cette option, vous avez besoin des éléments suivants :

1. **Exemples de données**. Ce tutoriel utilise des exemples de données stockées dans SQL Server, dans la base de données AdventureWorks, en tant que données sources à charger dans un pool SQL dédié. Pour obtenir l’exemple de base de données AdventureWorks, consultez [Exemples de bases de données AdventureWorks][AdventureWorks 2014 Sample Databases].

2. **Règle de pare-feu**. Vous devez créer une règle de pare-feu sur votre pool SQL dédié avec l’adresse IP de votre ordinateur local pour pouvoir charger des données dans le pool SQL dédié.

### <a name="create-the-basic-data-flow"></a>Créer le flux de données de base
1. Faites glisser une tâche de flux de données de la boîte à outils jusqu’au centre de l’aire de conception (dans l’onglet **Flux de contrôle**).
   
    ![Capture d’écran de Visual Studio montrant une tâche de flux de données déplacée sous l’onglet Flux de contrôle du volet de conception.][02]
2. Double-cliquez sur la tâche de flux de données pour basculer vers l'onglet Flux de données.
3. Dans la liste Autres sources, dans la boîte à outils, faites glisser une source ADO.NET jusqu’à l’aire de conception. Lorsque l’adaptateur de source est encore sélectionné, remplacez son nom par **Source SQL Server** dans le volet **Propriétés**.
4. Dans la liste Autres destinations, dans la boîte à outils, faites glisser une destination ADO.NET jusqu’à l’aire de conception sous la source ADO.NET. Lorsque l’adaptateur de destination est encore sélectionné, remplacez son nom par **Destination SQL DW** dans le volet **Propriétés**.
   
    ![Capture d’écran d’un adaptateur de destination déplacé vers un emplacement situé juste sous l’adaptateur source.][09]

### <a name="configure-the-source-adapter"></a>Configurer l’adaptateur de source
1. Double-cliquez sur l’adaptateur de source pour ouvrir l’**Éditeur de source ADO.NET**.
   
    ![Capture d’écran de l’Éditeur de source ADO.NET. L’onglet Gestionnaire de connexions est visible, et des contrôles sont disponibles pour la configuration des propriétés du flux de données.][03]
2. Dans l’onglet **Gestionnaire de connexions** de l’**Éditeur de source ADO.NET**, cliquez sur le bouton **Nouveau** situé en regard de la liste **Gestionnaire de connexions ADO.NET** pour afficher la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET** et créer des paramètres de connexion pour la base de données SQL Server à partir de laquelle ce didacticiel charge les données.
   
    ![Capture d’écran de la boîte de dialogue Configurer le gestionnaire de connexions ADO.NET. Des contrôles sont disponibles pour la configuration des gestionnaires de connexions.][04]
3. Dans la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**, cliquez sur le bouton **Nouveau** pour ouvrir la boîte de dialogue **Gestionnaire de connexions** et créer une nouvelle connexion de données.
   
    ![Capture d’écran de la boîte de dialogue Gestionnaire de connexions. Des contrôles sont disponibles pour la configuration d’une connexion de données.][05]
4. Dans la boîte de dialogue **Gestionnaire de connexions**, effectuez les actions suivantes.
   
   1. Pour **Fournisseur**, sélectionnez le fournisseur de données SqlClient.
   2. Pour **Nom du serveur**, entrez le nom du serveur SQL Server.
   3. Dans la section **Connexion au serveur**, sélectionnez ou entrez les informations d’authentification.
   4. Dans la section **Se connecter à une base de données**, sélectionnez l’exemple de base de données AdventureWorks.
   5. Cliquez sur **Tester la connexion**.
      
       ![Capture d’écran d’une boîte de dialogue affichant un bouton OK et un texte indiquant que le test de la connexion a réussi.][06]
   6. Dans la boîte de dialogue indiquant les résultats du test de connexion, cliquez sur **OK** pour revenir à la boîte de dialogue **Gestionnaire de connexions**.
   7. Dans la boîte de dialogue **Gestionnaire de connexions**, cliquez sur **OK** pour revenir à la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**.
5. Dans la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**, cliquez sur **OK** pour revenir à l’**Éditeur de source ADO.NET**.
6. Dans l’**Éditeur de source ADO.NET**, dans la liste **Nom de la table ou de la vue**, sélectionnez la table **Sales.SalesOrderDetail**.
   
    ![Capture d’écran de l’Éditeur de source ADO.NET. Dans la liste Nom de la table ou de la vue, la table Sales.SalesOrderDetail est sélectionnée.][07]
7. Cliquez sur **Aperçu** pour afficher les 200 premières lignes de données dans la table source de la boîte de dialogue **Visualiser les résultats de la requête**.
   
    ![Capture d’écran de la boîte de dialogue Visualiser les résultats de la requête. Plusieurs lignes de données de ventes de la table source sont visibles.][08]
8. Dans la boîte de dialogue **Visualiser les résultats de la requête**, cliquez sur **Fermer** pour revenir à l’**Éditeur de source ADO.NET**.
9. Dans l’**Éditeur de source ADO.NET**, cliquez sur **OK** pour terminer la configuration de la source de données.

### <a name="connect-the-source-adapter-to-the-destination-adapter"></a>Connecter l’adaptateur de source à l’adaptateur de destination
1. Sélectionnez l’adaptateur de source dans l’aire de conception.
2. Sélectionnez la flèche bleue qui s’étend de l’adaptateur de source et faites-la glisser vers l’éditeur de destination jusqu'à ce qu’il s’enclenche.
   
    ![Capture d’écran montrant les adaptateurs source et de destination. Une flèche bleue pointe de l’adaptateur source vers l’adaptateur de destination.][10]
   
    Dans un package SSIS standard, vous utilisez plusieurs autres composants de la boîte à outils SSIS entre la source et la destination pour restructurer, transformer et nettoyer vos données lorsqu’elles traversent le flux de données SSIS. Pour que cet exemple reste aussi simple que possible, nous connectons directement la source à la destination.

### <a name="configure-the-destination-adapter"></a>Configurer l’adaptateur de destination
1. Double-cliquez sur l’adaptateur de destination pour ouvrir l’**Éditeur de destination ADO.NET**.
   
    ![Capture d’écran de l’Éditeur de destination ADO.NET. L’onglet Gestionnaire de connexions est visible et contient des contrôles pour la configuration des propriétés du flux de données.][11]
2. Dans l’onglet **Gestionnaire de connexions** de **l’Éditeur de destination ADO.NET**, cliquez sur le bouton **Nouveau** situé à côté de la liste **Gestionnaire de connexions** afin d’ouvrir la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET** et de créer des paramètres de connexion pour la base de données Azure Synapse Analytics dans laquelle des données sont chargées dans le cadre de ce tutoriel.
3. Dans la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**, cliquez sur le bouton **Nouveau** pour ouvrir la boîte de dialogue **Gestionnaire de connexions** et créer une nouvelle connexion de données.
4. Dans la boîte de dialogue **Gestionnaire de connexions**, effectuez les actions suivantes.
   1. Pour **Fournisseur**, sélectionnez le fournisseur de données SqlClient.
   2. Pour **Nom du serveur**, entrez le nom du pool SQL dédié.
   3. Dans la section **Connexion au serveur**, sélectionnez **Utiliser l'authentification SQL Server** et entrez les informations d’authentification.
   4. Dans la section **Se connecter à une base de données**, sélectionnez une base de données existante dans le pool SQL dédié.
   5. Cliquez sur **Tester la connexion**.
   6. Dans la boîte de dialogue indiquant les résultats du test de connexion, cliquez sur **OK** pour revenir à la boîte de dialogue **Gestionnaire de connexions**.
   7. Dans la boîte de dialogue **Gestionnaire de connexions**, cliquez sur **OK** pour revenir à la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**.
5. Dans la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**, cliquez sur **OK** pour revenir à l’**Éditeur de destination ADO.NET**.
6. Dans l’**Éditeur de destination ADO.NET**, cliquez sur **Nouveau** en regard de la liste **Utiliser une table ou une vue** pour ouvrir la boîte de dialogue **Créer une table** afin de créer une nouvelle table de destination avec une liste de colonnes qui correspond à la table source.
   
    ![Capture d’écran de la boîte de dialogue Créer une table. Le code SQL pour la création d’une table de destination est visible.][12a]
7. Dans la boîte de dialogue **Créer une table**, effectuez les actions suivantes.
   
   1. Remplacez le nom de la table de destination par **SalesOrderDetail**.
   2. Supprimez la colonne **rowguid**. Le type de données **uniqueidentifier** n’est pas pris en charge dans un pool SQL dédié.
   3. Changez le type de données de la colonne **LineTotal** en spécifiant **money**. Le type de données **decimal** n’est pas pris en charge dans un pool SQL dédié. Pour plus d’informations sur les types de données pris en charge, consultez [CREATE TABLE (Azure Synapse Analytics, Parallel Data Warehouse)][CREATE TABLE (Azure Synapse Analytics, Parallel Data Warehouse)].
      
       ![Capture d’écran de la boîte de dialogue Créer une table, avec le code permettant de créer une table nommée SalesOrderDetail avec LineTotal en tant que colonne de montant et aucune colonne rowguid.][12b]
   4. Cliquez sur **OK** pour créer la table et revenir à l’**Éditeur de destination ADO.NET**.
8. Dans l’**Éditeur de destination ADO.NET**, sélectionnez l’onglet **Mappages** pour voir comment les colonnes de la source sont mappées aux colonnes de la destination.
   
    ![Capture d’écran de l’onglet Mappages de l’Éditeur de destination ADO.NET. Les lignes connectent les colonnes dont le nom est identique dans les tables source et de destination.][13]
9. Cliquez sur **OK** pour terminer la configuration de la destination.

## <a name="run-the-package-to-load-the-data"></a>Exécuter le package pour charger les données
Exécutez le package en cliquant sur le bouton **Démarrer** dans la barre d’outils ou en sélectionnant l’une des options **Exécuter** dans le menu **Débogage**.

Les paragraphes suivants décrivent ce que vous voyez si vous avez créé le package avec la deuxième option décrite dans cet article, autrement dit avec un flux de données qui contient une source et une destination.

Lorsque le package commence à s’exécuter, des roues dentées jaunes tournent pour indiquer l’activité et vous voyez le nombre de lignes traitées jusque là.

![Capture d’écran montrant les adaptateurs source et de destination en jaune, des roues tournantes sur chaque adaptateur et le texte « 29916 rows » entre eux.][14]

Quand le package a fini de s’exécuter, vous voyez des coches vertes qui indiquent la réussite de l’opération, ainsi que le nombre total de lignes de données chargées de la source vers la destination.

![Capture d’écran montrant les adaptateurs source et de destination. Des coches vertes sont placées sur chaque adaptateur, et le texte « 121317 rows » figure entre eux.][15]

Félicitations ! Vous avez utilisé SQL Server Integration Services pour charger des données dans Azure Synapse Analytics.

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment déboguer et dépanner vos packages directement dans l’environnement de conception. Commencez ici : [Outils de résolution des problèmes pour le développement de packages][Troubleshooting Tools for Package Development].

- Découvrez comment déployer vos projets SSIS et vos packages sur le serveur Integration Services ou dans un autre emplacement de stockage. Commencez ici : [Déploiement de projets et de packages][Deployment of Projects and Packages].

<!-- Image references -->
[01]:  ./media/load-data-to-sql-data-warehouse/ssis-designer-01.png
[02]:  ./media/load-data-to-sql-data-warehouse/ssis-data-flow-task-02.png
[03]:  ./media/load-data-to-sql-data-warehouse/ado-net-source-03.png
[04]:  ./media/load-data-to-sql-data-warehouse/ado-net-connection-manager-04.png
[05]:  ./media/load-data-to-sql-data-warehouse/ado-net-connection-05.png
[06]:  ./media/load-data-to-sql-data-warehouse/test-connection-06.png
[07]:  ./media/load-data-to-sql-data-warehouse/ado-net-source-07.png
[08]:  ./media/load-data-to-sql-data-warehouse/preview-data-08.png
[09]:  ./media/load-data-to-sql-data-warehouse/source-destination-09.png
[10]:  ./media/load-data-to-sql-data-warehouse/connect-source-destination-10.png
[11]:  ./media/load-data-to-sql-data-warehouse/ado-net-destination-11.png
[12a]: ./media/load-data-to-sql-data-warehouse/destination-query-before-12a.png
[12b]: ./media/load-data-to-sql-data-warehouse/destination-query-after-12b.png
[13]:  ./media/load-data-to-sql-data-warehouse/column-mapping-13.png
[14]:  ./media/load-data-to-sql-data-warehouse/package-running-14.png
[15]:  ./media/load-data-to-sql-data-warehouse/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase Guide]: ../relational-databases/polybase/polybase-guide.md
[Download SQL Server Data Tools (SSDT)]: ../ssdt/download-sql-server-data-tools-ssdt.md
[CREATE TABLE (Azure Synapse Analytics, Parallel Data Warehouse)]: ../t-sql/statements/create-table-azure-sql-data-warehouse.md
[Data Flow]: ./data-flow/data-flow.md
[Troubleshooting Tools for Package Development]: ./troubleshooting/troubleshooting-tools-for-package-development.md
[Deployment of Projects and Packages]: ./packages/deploy-integration-services-ssis-projects-and-packages.md

<!--Other Web references-->
[Microsoft SQL Server 2017 Integration Services Feature Pack for Azure]: https://www.microsoft.com/download/details.aspx?id=54798
[SQL Server Evaluations]: https://www.microsoft.com/evalcenter/evaluate-sql-server-2017
[Visual Studio Community]: https://www.visualstudio.com/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks