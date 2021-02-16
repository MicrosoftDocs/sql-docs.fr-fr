---
title: Charger des données dans SQL Server ou Azure SQL Database avec SSIS (SQL Server Integration Services) | Microsoft Docs
description: Indique comment créer un package SSIS (SQL Server Integration Services) pour déplacer des données vers Azure SQL Database ou SQL Server à partir d’un large éventail de sources de données.
documentationcenter: NA
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
ms.custom: loading
ms.date: 08/20/2018
ms.author: chugu
author: chugugrace
ms.openlocfilehash: 4bdd992e4cea1f2c869f3669905709f91727c874
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074183"
---
# <a name="load-data-into-sql-server-or-azure-sql-database-with-sql-server-integration-services-ssis"></a>Charger des données dans SQL Server ou Azure SQL Database avec SSIS (SQL Server Integration Services)

[!INCLUDE[sqlserver-ssis](../includes/ssis-appliesto-ssvrpluslinux-asdb-xxxx-xxx.md)]

Créez un package SSIS (SQL Server Integration Services) pour charger des données dans SQL Server ou [Azure SQL Database](/azure/sql-database/). Vous pouvez éventuellement restructurer, transformer et nettoyer les données qui traversent le flux de données SSIS.

Cet article vous montre comment effectuer les opérations suivantes :

* Créer un projet Integration Services dans Visual Studio.
* Concevoir un package SSIS qui charge des données de la source vers la destination.
* Exécuter le package SSIS pour charger les données.

## <a name="basic-concepts"></a>Concepts de base

Le package est l’unité de travail de base dans SSIS. Les packages associés sont regroupés en projets. Vous créez des projets et concevez des packages dans Visual Studio avec SQL Server Data Tools. Le processus de conception est un processus visuel au cours duquel vous faites glisser et déposez des composants issus de la boîte à outils dans l’aire de conception, les connectez et définissez leurs propriétés. Une fois que vous avez terminé votre package, vous pouvez l’exécuter et éventuellement le déployer sur SQL Server ou SQL Database pour le gérer, le superviser et le sécuriser complètement.

La présentation détaillée de SSIS dépasse le cadre de cet article. Pour en savoir plus, consultez les articles suivants :

- [SQL Server Integration Services](sql-server-integration-services.md)

- [SSIS : comment créer un package ETL](ssis-how-to-create-an-etl-package.md)

## <a name="about-the-solution"></a>À propos de la solution

La solution est un package classique qui utilise une tâche de flux de données qui contient une source et une destination. Cette approche prend en charge un large éventail de sources de données, dont SQL Server et Azure SQL Database.

Ce didacticiel utilise SQL Server comme source de données. SQL Server est exécuté localement ou sur une machine virtuelle Azure.

Pour vous connecter à SQL Server et SQL Database, vous pouvez utiliser un gestionnaire de connexions ADO.NET ainsi que la source et la destination, ou un gestionnaire de connexions OLE DB ainsi que la source et la destination. Ce tutoriel utilise ADO.NET, car il présente le moins d’options de configuration. OLE DB peut fournir des performances légèrement meilleures qu’ADO.NET.

Pour aller plus vite, vous pouvez utiliser l’Assistant Importation et Exportation SQL Server pour créer le package de base. Ensuite, enregistrez le package et ouvrez-le dans Visual Studio ou SSDT pour l’afficher et le personnaliser. Pour plus d’informations, consultez [Importer et exporter des données avec l’Assistant Importation et Exportation SQL Server](import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

## <a name="prerequisites"></a>Conditions préalables requises
Pour exécuter pas à pas ce tutoriel, vous avez besoin des éléments suivants :

1. **SQL Server Integration Services (SSIS)** . SSIS est un composant de SQL Server et requiert une version sous licence, ou la version de développeur ou d’évaluation, de SQL Server. Pour obtenir une version d’évaluation de SQL Server, consultez [Évaluer SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm).
2. **Visual Studio** (facultatif). Pour obtenir l’édition Visual Studio Community gratuite, consultez [Visual Studio Community][Visual Studio Community]. Si vous ne souhaitez pas installer Visual Studio, vous pouvez installer uniquement SSDT (SQL Server Data Tools). SSDT installe une version de Visual Studio avec des fonctionnalités limitées.
3. **SQL Server Data Tools pour Visual Studio (SSDT)** . Pour obtenir SQL Server Data Tools pour Visual Studio, consultez [Télécharger SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].
4. Dans ce tutoriel, vous vous connectez à une instance SQL Server ou SQL Database et y chargez des données. Vous devez disposer des autorisations pour vous connecter, créer une table et charger des données sur :
   - **Une base de données Azure SQL Database**. Pour plus d’informations, consultez [Azure SQL Database](/azure/sql-database/).  
      or
   - **Une instance SQL Server**. SQL Server est exécuté localement ou sur une machine virtuelle Azure. Pour télécharger une édition d’évaluation ou développeur gratuite de SQL Server, consultez [Téléchargements SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads).

5. **Exemples de données**. Ce tutoriel utilise des exemples de données stockées dans SQL Server, dans l’exemple de base de données AdventureWorks, en tant que données sources. Pour obtenir l’exemple de base de données AdventureWorks, consultez [Exemples de bases de données AdventureWorks][AdventureWorks 2014 Sample Databases].
6. **Une règle de pare-feu** si vous chargez des données dans SQL Database. Vous devez créer une règle de pare-feu sur SQL Database avec l’adresse IP de votre ordinateur local afin de pouvoir charger des données dans SQL Database.

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

## <a name="create-the-basic-data-flow"></a>Créer le flux de données de base
1. Faites glisser une tâche de flux de données de la boîte à outils jusqu’au centre de l’aire de conception (dans l’onglet **Flux de contrôle**).
   
    ![Capture d’écran de Visual Studio montrant une tâche de flux de données déplacée sous l’onglet Flux de contrôle du volet de conception.][02]
2. Double-cliquez sur la tâche de flux de données pour basculer vers l'onglet Flux de données.
3. Dans la liste Autres sources, dans la boîte à outils, faites glisser une source ADO.NET jusqu’à l’aire de conception. Lorsque l’adaptateur de source est encore sélectionné, remplacez son nom par **Source SQL Server** dans le volet **Propriétés**.
4. Dans la liste Autres destinations, dans la boîte à outils, faites glisser une destination ADO.NET jusqu’à l’aire de conception sous la source ADO.NET. Lorsque l’adaptateur de destination est encore sélectionné, remplacez son nom par **Destination SQL** dans le volet **Propriétés**.
   
    ![Capture d’écran d’un adaptateur de destination déplacé vers un emplacement situé juste sous l’adaptateur source.][09]

## <a name="configure-the-source-adapter"></a>Configurer l’adaptateur de source
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

## <a name="connect-the-source-adapter-to-the-destination-adapter"></a>Connecter l’adaptateur de source à l’adaptateur de destination
1. Sélectionnez l’adaptateur de source dans l’aire de conception.
2. Sélectionnez la flèche bleue qui s’étend de l’adaptateur de source et faites-la glisser vers l’éditeur de destination jusqu'à ce qu’il s’enclenche.
   
    ![Capture d’écran montrant les adaptateurs source et de destination. Une flèche bleue pointe de l’adaptateur source vers l’adaptateur de destination.][10]
   
    Dans un package SSIS standard, vous utilisez plusieurs autres composants de la boîte à outils SSIS entre la source et la destination pour restructurer, transformer et nettoyer vos données lorsqu’elles traversent le flux de données SSIS. Pour que cet exemple reste aussi simple que possible, nous connectons directement la source à la destination.

## <a name="configure-the-destination-adapter"></a>Configurer l’adaptateur de destination
1. Double-cliquez sur l’adaptateur de destination pour ouvrir l’**Éditeur de destination ADO.NET**.
   
    ![Capture d’écran de l’Éditeur de destination ADO.NET. L’onglet Gestionnaire de connexions est visible et contient des contrôles pour la configuration des propriétés du flux de données.][11]
2. Sous l’onglet **Gestionnaire de connexions** de l’**Éditeur de destination ADO.NET**, cliquez sur le bouton **Nouveau** situé en regard de la liste **Gestionnaire de connexions** pour afficher la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET** et créer des paramètres de connexion pour la base de données dans laquelle ce tutoriel charge des données.
3. Dans la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**, cliquez sur le bouton **Nouveau** pour ouvrir la boîte de dialogue **Gestionnaire de connexions** et créer une nouvelle connexion de données.
4. Dans la boîte de dialogue **Gestionnaire de connexions**, effectuez les actions suivantes.
   1. Pour **Fournisseur**, sélectionnez le fournisseur de données SqlClient.
   2. Pour **Nom du serveur**, entrez le nom du serveur SQL Server ou du serveur SQL Database.
   3. Dans la section **Connexion au serveur**, sélectionnez **Utiliser l'authentification SQL Server** et entrez les informations d’authentification.
   4. Dans la section **Se connecter à une base de données**, sélectionnez une base de données existante.
    a. Cliquez sur **Tester la connexion**.
    b. Dans la boîte de dialogue indiquant les résultats du test de connexion, cliquez sur **OK** pour revenir à la boîte de dialogue **Gestionnaire de connexions**.
    c. Dans la boîte de dialogue **Gestionnaire de connexions**, cliquez sur **OK** pour revenir à la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**.
5. Dans la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET**, cliquez sur **OK** pour revenir à l’**Éditeur de destination ADO.NET**.
6. Dans l’**Éditeur de destination ADO.NET**, cliquez sur **Nouveau** en regard de la liste **Utiliser une table ou une vue** pour ouvrir la boîte de dialogue **Créer une table** afin de créer une nouvelle table de destination avec une liste de colonnes qui correspond à la table source.
   
    ![Capture d’écran de la boîte de dialogue Créer une table. Le code SQL pour la création d’une table de destination est visible.][12a]
7. Dans la boîte de dialogue **Créer une table**, effectuez les actions suivantes.
   
   1. Remplacez le nom de la table de destination par **SalesOrderDetail**.
      
       ![Capture d’écran de la boîte de dialogue Créer une table. Le code SQL de création d’une table nommée SalesOrderDetail est visible.][12b]

   2. Cliquez sur **OK** pour créer la table et revenir à l’**Éditeur de destination ADO.NET**.
8. Dans l’**Éditeur de destination ADO.NET**, sélectionnez l’onglet **Mappages** pour voir comment les colonnes de la source sont mappées aux colonnes de la destination.
   
    ![Capture d’écran de l’onglet Mappages de l’Éditeur de destination ADO.NET. Les lignes connectent les colonnes dont le nom est identique dans les tables source et de destination.][13]
9. Cliquez sur **OK** pour terminer la configuration de la destination.

## <a name="run-the-package-to-load-the-data"></a>Exécuter le package pour charger les données
Exécutez le package en cliquant sur le bouton **Démarrer** dans la barre d’outils ou en sélectionnant l’une des options **Exécuter** dans le menu **Débogage**.

Les paragraphes suivants décrivent ce que vous voyez si vous avez créé le package avec la deuxième option décrite dans cet article, autrement dit avec un flux de données qui contient une source et une destination.

Lorsque le package commence à s’exécuter, des roues dentées jaunes tournent pour indiquer l’activité et vous voyez le nombre de lignes traitées jusque là.

![Capture d’écran montrant les adaptateurs source et de destination. Des roues tournantes jaunes sont placées sur chaque adaptateur, et le texte « 89748 rows » figure entre eux.][14]

Quand le package a fini de s’exécuter, vous voyez des coches vertes qui indiquent la réussite de l’opération, ainsi que le nombre total de lignes de données chargées de la source vers la destination.

![Capture d’écran montrant les adaptateurs source et de destination. Des coches vertes sont placées sur chaque adaptateur, et le texte « 121317 rows » figure entre eux.][15]

Félicitations ! Vous avez utilisé SQL Server Integration Services pour charger des données dans SQL Server ou Azure SQL Database.

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment déboguer et dépanner vos packages directement dans l’environnement de conception. Commencez ici : [Outils de résolution des problèmes pour le développement de packages][Troubleshooting Tools for Package Development].

- Découvrez comment déployer vos projets SSIS et vos packages sur le serveur Integration Services ou dans un autre emplacement de stockage. Commencez ici : [Déploiement de projets et de packages][Deployment of Projects and Packages].

<!-- Image references -->
[01]:  ./media/load-data-to-sql-database-with-ssis/ssis-designer-01.png
[02]:  ./media/load-data-to-sql-database-with-ssis/ssis-data-flow-task-02.png
[03]:  ./media/load-data-to-sql-database-with-ssis/ado-net-source-03.png
[04]:  ./media/load-data-to-sql-database-with-ssis/ado-net-connection-manager-04.png
[05]:  ./media/load-data-to-sql-database-with-ssis/ado-net-connection-05.png
[06]:  ./media/load-data-to-sql-database-with-ssis/test-connection-06.png
[07]:  ./media/load-data-to-sql-database-with-ssis/ado-net-source-07.png
[08]:  ./media/load-data-to-sql-database-with-ssis/preview-data-08.png
[09]:  ./media/load-data-to-sql-database-with-ssis/source-destination-09.png
[10]:  ./media/load-data-to-sql-database-with-ssis/connect-source-destination-10.png
[11]:  ./media/load-data-to-sql-database-with-ssis/ado-net-destination-11.png
[12a]: ./media/load-data-to-sql-database-with-ssis/destination-query-before-12a.png
[12b]: ./media/load-data-to-sql-database-with-ssis/destination-query-after-12b.png
[13]:  ./media/load-data-to-sql-database-with-ssis/column-mapping-13.png
[14]:  ./media/load-data-to-sql-database-with-ssis/package-running-14.png
[15]:  ./media/load-data-to-sql-database-with-ssis/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[Download SQL Server Data Tools (SSDT)]: ../ssdt/download-sql-server-data-tools-ssdt.md
[Data Flow]: ./data-flow/data-flow.md
[Troubleshooting Tools for Package Development]: ./troubleshooting/troubleshooting-tools-for-package-development.md
[Deployment of Projects and Packages]: ./packages/deploy-integration-services-ssis-projects-and-packages.md

<!--Other Web references-->
[Microsoft SQL Server 2017 Integration Services Feature Pack for Azure]: https://www.microsoft.com/download/details.aspx?id=54798
[SQL Server Evaluations]: https://www.microsoft.com/evalcenter/evaluate-sql-server-2017
[Visual Studio Community]: https://www.visualstudio.com/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks
