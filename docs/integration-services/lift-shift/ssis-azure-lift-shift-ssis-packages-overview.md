---
title: Déployer et exécuter des packages SSIS dans Azure | Microsoft Docs
description: Découvrez comment vous pouvez déplacer vos projets, packages et charges de travail SQL Server Integration Services (SSIS) vers le cloud Microsoft Azure.
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 12b4e6c71698845314844e4cf0bcd5b371209641
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352002"
---
# <a name="lift-and-shift-sql-server-integration-services-workloads-to-the-cloud"></a>Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Vous pouvez maintenant déplacer vos projets, packages et charges de travail SQL Server Integration Services (SSIS) vers le cloud Azure. Déployez, exécutez et gérez des projets et des packages SSIS dans le catalogue SSIS (SSISDB) dans Azure SQL Database ou SQL Managed Instance avec des outils habituels, comme SQL Server Management Studio (SSMS).

## <a name="benefits"></a>Avantages
Le déplacement de vos charges de travail SSIS locales vers Azure présente les avantages potentiels suivants :
-   **Réduisez les coûts opérationnels** et la charge de gestion de l’infrastructure quand vous exécutez SSIS localement ou sur des machines virtuelles Azure.
-   **Augmentez la haute disponibilité** pour pouvoir spécifier plusieurs nœuds par cluster, et étendez les fonctionnalités de haute disponibilité d’Azure et d’Azure SQL Database.
-   **Augmentez la scalabilité** pour pouvoir spécifier plusieurs cœurs par nœud (scale-up) et plusieurs nœuds par cluster (scale-out).

## <a name="architecture-of-ssis-on-azure"></a>Architecture de SSIS sur Azure
Le tableau suivant met en évidence les différences entre une instance SSIS locale et une instance SSIS sur Azure.

La différence la plus importante est la séparation du stockage et de l’exécution. Azure Data Factory héberge le moteur de runtime des packages SSIS sur Azure. Le moteur d’exécution est appelé Azure-SSIS Integration Runtime (Azure-SSIS IR). Pour plus d’informations, consultez [Azure-SSIS Runtime Integration](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime).

| Emplacement | Stockage | Runtime | Extensibilité |
|---|---|---|---|
| Localement | SQL Server | Runtime SSIS hébergé par SQL Server | SSIS Scale Out (dans SQL Server 2017 et versions ultérieures)<br/><br/>Solutions personnalisées (dans les versions antérieures de SQL Server) |
| Sur Azure | SQL Database ou SQL Managed Instance | Azure-SSIS Integration Runtime, composant d’Azure Data Factory | Options de mise à l’échelle pour Azure-SSIS Integration Runtime |
| | | | |

## <a name="provision-ssis-on-azure"></a>Provisionner SSIS sur Azure

**Provisionnez**. Avant de pouvoir déployer et exécuter des packages SSIS dans Azure, vous devez provisionner le catalogue SSIS (SSISDB) et le runtime d’intégration Azure-SSIS.

-   Pour configurer SSIS sur Azure dans le portail Azure, suivez les étapes de configuration dans cet article : [Configurer Azure-SSIS Integration Runtime dans Azure Data Factory](/azure/data-factory/tutorial-deploy-ssis-packages-azure). 

-   Pour configurer SSIS sur Azure avec PowerShell, suivez les étapes de configuration dans cet article : [Configurer Azure-SSIS Integration Runtime dans Azure Data Factory avec PowerShell](/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell).

Il vous suffit de provisionner Azure-SSIS IR une seule fois. Vous pouvez ensuite utiliser des outils familiers tels que SQL Server Data Tools (SSDT) et SQL Server Management Studio (SSMS) pour déployer, configurer, exécuter, surveiller, planifier et gérer les packages.

> [!NOTE]
> Azure-SSIS Integration Runtime n’est pas encore disponible dans toutes les régions Azure. Pour plus d’informations sur les régions prises en charge, consultez [Produits disponibles par région - Microsoft Azure](https://azure.microsoft.com/regions/services/).

**Effectuez une augmentation de la taille des instances et une montée en puissance parallèle**. Quand vous provisionnez Azure-SSIS IR, vous pouvez effectuer un scale-up et un scale-out en spécifiant des valeurs pour les options suivantes :
-   la taille du nœud (notamment le nombre de cœurs) et le nombre de nœuds du cluster ;
-   l’instance existante d’Azure SQL Database pour héberger la base de données de catalogues SSIS (SSISDB), et le niveau de service de la base de données ;
-   le nombre maximal d’exécutions parallèles par nœud.

**Améliorez les performances**. Pour plus d’informations, consultez [Configurer le runtime d’intégration Azure-SSIS pour de hautes performances](/azure/data-factory/configure-azure-ssis-integration-runtime-performance).

**Réduisez les coûts**. Pour réduire les coûts, exécutez Azure-SSIS IR uniquement lorsque vous en avez besoin. Pour plus d’informations, voir [Guide pratique pour planifier le démarrage et l’arrêt d’un runtime d’intégration Azure-SSIS](/azure/data-factory/how-to-schedule-azure-ssis-integration-runtime).

## <a name="design-packages"></a>Concevoir des packages

Vous continuez à **concevoir et générer des packages** localement dans SSDT, ou dans Visual Studio avec SSDT installé.

### <a name="connect-to-data-sources"></a>Se connecter aux sources de données

Pour se connecter à des sources de données locales à partir du cloud avec **l’authentification Windows**, voir [Se connecter à des sources de données et à des partages de fichiers avec l’authentification Windows à partir de packages SSIS sur Azure](/azure/data-factory/ssis-azure-connect-with-windows-auth).

Pour se connecter à des fichiers et à des partages de fichiers, voir [Ouvrir et enregistrer des fichiers localement et sur Azure avec des packages SSIS déployés sur Azure](/azure/data-factory/ssis-azure-files-file-shares).

### <a name="available-ssis-components"></a>Composants SSIS disponibles

Si vous approvisionnez une instance de SQL Database pour héberger SSISDB, les composants Azure Feature Pack pour SSIS et Access Redistributable sont également installés. Ces composants fournissent une connectivité à diverses sources de données **Azure** et aux fichiers **Excel et Access**, en plus des sources de données prises en charge par les composants intégrés.

Vous pouvez également installer des composants supplémentaires. Par exemple, vous pouvez installer un pilote qui n’est pas installé par défaut. Pour plus d’informations, voir [Personnaliser l’installation du runtime d’intégration Azure-SSIS](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Des composants supplémentaires sont accessibles aux détenteurs d’une licence Enterprise Edition. Pour plus d’informations, voir [Configurer Enterprise Edition pour Azure-SSIS Integration Runtime](/azure/data-factory/how-to-configure-azure-ssis-ir-enterprise-edition).

Si vous êtes éditeur de logiciels, vous pouvez mettre à jour l’installation de vos composants sous licence pour les rendre disponibles sur Azure. Pour plus d’informations, voir [Installer des composants personnalisés payants ou sous licence pour le runtime d’intégration Azure-SSIS](/azure/data-factory/how-to-develop-azure-ssis-ir-licensed-components).

### <a name="transaction-support"></a>Prise en charge des transactions

Avec SQL Server en local et sur des machines virtuelles Azure, vous pouvez utiliser des transactions Microsoft Distributed Transaction Coordinator (MSDTC). Pour configurer MSDTC sur chaque nœud d’Azure-SSIS IR, utilisez la fonctionnalité d’installation personnalisée. Pour plus d’informations, consultez [Custom setup for the Azure-SSIS integration runtime](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) (Configuration personnalisée du runtime d’intégration Azure-SSIS).

Avec Azure SQL Database, vous pouvez utiliser uniquement des transactions élastiques. Pour plus d’informations, consultez [Transactions distribuées entre bases de données cloud](/azure/sql-database/sql-database-elastic-transactions-overview).

## <a name="deploy-and-run-packages"></a>Déployer et exécuter des packages

Pour commencer, consultez [Tutoriel : Déployer et exécuter un package SQL Server Integration Services (SSIS) sur Azure](ssis-azure-deploy-run-monitor-tutorial.md).

### <a name="prerequisites"></a>Prérequis

Pour déployer des packages SSIS sur Azure, vous devez avoir l’une des versions suivantes de SQL Server Data Tools (SSDT) :
-   Pour Visual Studio 2017, version 15.3 ou ultérieure.
-   Pour Visual Studio 2015, version 17.2 ou version ultérieure.

### <a name="connect-to-ssisdb"></a>Se connecter à SSISDB

Le **nom de l’instance SQL Database** qui héberge SSISDB devient la première partie du nom en quatre parties à utiliser quand vous déployez et exécutez des packages à partir de SSDT et SSMS, au format suivant : `<sql_database_name>.database.windows.net`. Pour plus d’informations sur la connexion à la base de données du catalogue SSIS sur Azure, voir [Se connecter au catalogue SSIS (SSISDB) sur Azure](ssis-azure-connect-to-catalog-database.md).

### <a name="deploy-projects-and-packages"></a>Déployer des projets et des packages

Vous devez utiliser le **modèle de déploiement de projet**, et non le modèle de déploiement de package, quand vous déployez des projets dans SSISDB sur Azure.

Pour déployer des projets sur Azure, vous pouvez utiliser plusieurs outils et options de script de votre choix :
-   SQL Server Management Studio (SSMS)
-   Transact-SQL (à partir de SSMS, Visual Studio Code ou tout autre outil)
-   Un outil en ligne de commande
-   PowerShell ou C# et le modèle objet de gestion SSIS

Le processus de déploiement valide chaque package pour vérifier qu’il peut s’exécuter dans Azure SSIS Integration Runtime. Pour plus d’informations, voir [Valider les packages SQL Server Integration Services (SSIS) déployés sur Azure](ssis-azure-validate-packages.md).

Pour obtenir un exemple de déploiement qui utilise SSMS et l’Assistant Déploiement d’Integration Services, consultez [Didacticiel : Déployer et exécuter un package SQL Server Integration Services (SSIS) sur Azure](ssis-azure-deploy-run-monitor-tutorial.md).

### <a name="version-support"></a>Prise en charge de la version

Vous pouvez déployer n’importe quel package créé avec l’une des versions de SSIS sur Azure. Quand vous déployez un package sur Azure sans erreur de validation, le package est automatiquement mis à niveau avec le dernier format de package. Le package est donc toujours mis à niveau avec la dernière version de SSIS.

### <a name="run-packages"></a>Exécuter des packages

Il existe différents moyens d’exécuter des packages SSIS déployés sur Azure. Pour plus d’informations, voir [Exécuter des packages SQL Server Integration Services (SSIS) déployés sur Azure](ssis-azure-run-packages.md).

### <a name="run-packages-in-an-azure-data-factory-pipeline"></a>Exécuter des packages dans un pipeline Azure Data Factory

Pour exécuter un package SSIS dans un pipeline Azure Data Factory, utilisez l’activité Exécuter un package SSIS. Pour plus d’informations, consultez [Exécuter un package SSIS à l’aide de l’activité Exécuter le package SSIS dans Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

Lorsque vous exécutez un package dans un pipeline Data Factory avec l’activité Exécuter un package SSIS, vous pouvez transmettre des valeurs au package à l’exécution. Pour transmettre une ou plusieurs valeurs de runtime, créez des environnements d’exécution SSIS dans SSISDB avec SQL Server Management Studio (SSMS). Dans chaque environnement, créez des variables et affectez des valeurs qui correspondent aux paramètres de vos projets ou packages. Configurez vos packages SSIS dans SSMS pour associer ces variables d’environnement aux paramètres de votre projet ou package. Quand vous exécutez les packages dans le pipeline, vous pouvez passer d’un environnement à un autre en spécifiant différents chemins d’environnement sous l’onglet Paramètres de l’interface utilisateur de l’activité Exécuter un package SSIS. Pour plus d’informations sur les environnements SSIS, voir [Créer et mapper un environnement serveur](../packages/deploy-integration-services-ssis-projects-and-packages.md#create-and-map-a-server-environment).

## <a name="monitor-packages"></a>Surveiller les packages

Pour effectuer le monitoring des packages en cours d’exécution, utilisez les options de création de rapports suivantes dans SSMS.
-   Cliquez avec le bouton droit sur **SSISDB**, puis sélectionnez **Opérations actives** pour ouvrir la boîte de dialogue **Opérations actives**.
-   Sélectionnez un package dans l’Explorateur d’objets, cliquez avec le bouton droit, sélectionnez **Rapports**, **Rapports standard**, puis **Toutes les exécutions**.

Pour surveiller Azure-SSIS Integration Runtime, consultez [Surveiller le runtime d’intégration Azure-SSIS](/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime).

## <a name="schedule-packages"></a>Planifier les packages
Il existe différents outils permettant de planifier l’exécution de packages déployés sur Azure. Pour plus d’informations, voir [Planifier l’exécution de packages SQL Server Integration Services (SSIS) déployés sur Azure](ssis-azure-schedule-packages.md).

## <a name="next-steps"></a>Étapes suivantes
Pour bien démarrer avec les charges de travail SSIS sur Azure, consultez les articles suivants :
-   [Tutoriel : Déployer et exécuter un package SQL Server Integration Services (SSIS) sur Azure](ssis-azure-deploy-run-monitor-tutorial.md)
-   [Configurer Azure-SSIS Integration Runtime dans Azure Data Factory](/azure/data-factory/tutorial-deploy-ssis-packages-azure)