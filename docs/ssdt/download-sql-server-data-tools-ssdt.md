---
title: Télécharger SSDT (SQL Server Data Tools)
description: En savoir plus sur SQL Server Data Tools (SSDT). Consultez comment installer ce jeu d’outils de développement de base de données avec Visual Studio 2019 et Visual Studio 2017.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssdt
ms.topic: conceptual
keywords: installer ssdt, télécharger ssdt, dernière version ssdt
ms.assetid: b0fc4987-d260-4d0a-9dd1-98099835b361
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 02/20/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: cf088c407a95d32f3cfc7413b55ec41661126e78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077234"
---
# <a name="download-sql-server-data-tools-ssdt-for-visual-studio"></a>Télécharger SSDT (SQL Server Data Tools) pour Visual Studio

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

**SSDT (SQL Server Data Tools)** est un outil de développement moderne permettant de générer des bases de données relationnelles SQL Server, des bases de données dans SQL Azure, des modèles de données AS (Analysis Services), des packages IS (Integration Services) et des rapports RS (Reporting Services). Avec SSDT, vous pouvez concevoir et déployer tout type de contenu SQL Server avec la même facilité que lorsque vous développez une application dans Visual Studio.

## <a name="ssdt-for-visual-studio-2019"></a>SSDT pour Visual Studio 2019

### <a name="changes-in-ssdt-for-visual-studio-2019"></a>Modifications apportées à SSDT pour Visual Studio 2019

La fonctionnalité SSDT de base pour la création de projets de base de données est toujours intégrée à Visual Studio.

Avec Visual Studio 2019, les fonctionnalités nécessaires pour activer des projets Analysis Services, Integration Services et Reporting Services ont été déplacées dans les extensions Visual Studio (VSIX) respectives.

> [!NOTE]
> Il n’existe pas de programme d’installation autonome de SSDT pour Visual Studio 2019.

### <a name="install-ssdt-with-visual-studio-2019"></a>Installer SSDT avec Visual Studio 2019

Si [Visual Studio 2019](/visualstudio/install/install-visual-studio?preserve-view=true&view=vs-2019) est déjà installé, vous pouvez modifier la liste des charges de travail pour inclure SSDT. Si vous n’avez pas encore installé Visual Studio 2019, vous pouvez télécharger et installer [Visual Studio 2019 Community](https://visualstudio.microsoft.com/downloads/).

Pour modifier les charges de travail Visual Studio installées afin d’inclure SSDT, utilisez le programme d’installation de Visual Studio.

1. Lancez le programme d’installation de Visual Studio. Dans le menu Démarrer de Windows, vous pouvez rechercher « programme d’installation ».

   ![Programme d’installation de Visual Studio dans le menu Démarrer de Windows 2019](../ssdt/media/visual-studio-installer.png)

2. Dans le programme d’installation, sélectionnez pour l’édition de Visual Studio à laquelle vous voulez ajouter SSDT, puis choisissez **Modifier**.

3. Sélectionnez **SQL Server Data Tools** sous **Stockage et traitement des données** dans la liste des charges de travail.

   ![Charge de travail de stockage et de traitement des données 2019](../ssdt/media/download-sql-server-data-tools-ssdt/data-workload-2019.png)

Pour les projets Analysis Services, Integration Services ou Reporting Services, vous pouvez installer les [extensions](/visualstudio/ide/finding-and-using-visual-studio-extensions) appropriées à partir de Visual Studio avec **Extensions** > **Gérer les extensions** ou à partir de la [Place de marché](https://marketplace.visualstudio.com/search?term=services&target=VS&category=All%20categories&vsVersion=&sortBy=Relevance).

* [Analysis Services](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftAnalysisServicesModelingProjects)
* [Services d’intégration](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects)
* [Reporting Services](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftReportProjectsforVisualStudio)

## <a name="ssdt-for-visual-studio-2017"></a>SSDT pour Visual Studio 2017

### <a name="changes-in-ssdt-for-visual-studio-2017"></a>Modifications apportées à SSDT pour Visual Studio 2017

À partir de Visual Studio 2017, la fonctionnalité de création de projets de base de données a été intégrée dans l’installation de Visual Studio. Il est inutile d’installer le programme d’installation autonome SSDT pour bénéficier d’une expérience SSDT de base.

Pour créer des projets Analysis Services, Integration Services ou Reporting Services, vous avez toujours besoin du programme d’installation autonome SSDT.

### <a name="install-ssdt-with-visual-studio-2017"></a>Installer SSDT avec Visual Studio 2017

Pour installer SSDT dans le cadre de [l’installation de Visual Studio](/visualstudio/install/install-visual-studio), sélectionnez la charge de travail **Stockage et traitement des données**, puis **SQL Server Data Tools**.

Si Visual Studio est déjà installé, utilisez le programme d’installation pour modifier les charges de travail afin d’inclure SSDT.

1. Lancez le programme d’installation de Visual Studio. Dans le menu Démarrer de Windows, vous pouvez rechercher « programme d’installation ».

   ![Programme d’installation de Visual Studio dans le menu Démarrer de Windows 2017](../ssdt/media/visual-studio-installer.png)

2. Dans le programme d’installation, sélectionnez pour l’édition de Visual Studio à laquelle vous voulez ajouter SSDT, puis choisissez **Modifier**.

3. Sélectionnez **SQL Server Data Tools** sous **Stockage et traitement des données** dans la liste des charges de travail.

   ![Charge de travail de stockage et de traitement des données 2017](../ssdt/media/download-sql-server-data-tools-ssdt/data-workload-2017.png)

### <a name="install-analysis-services-integration-services-and-reporting-services-tools"></a>Installer les outils Analysis Services, Integration Services et Reporting Services

Pour bénéficier de la prise en charge des projets Analysis Services, Integration Services et Reporting Services, exécutez le [programme d’installation autonome SSDT](#ssdt-for-vs-2017-standalone-installer).

Le programme d’installation liste les instances disponibles de Visual Studio auxquelles vous pouvez ajouter les outils SSDT. Si Visual Studio n’est pas encore installé, vous pouvez sélectionner **Installer une nouvelle instance de SQL Server Data Tools** pour installer SSDT avec une version minimale de Visual Studio. Toutefois, pour bénéficier d’une expérience optimale, nous vous recommandons d’utiliser SSDT avec [la dernière version de Visual Studio](https://www.visualstudio.com/downloads).

![Sélectionner AS, IS, RS](../ssdt/media/download-sql-server-data-tools-ssdt/select-services.png)

## <a name="ssdt-for-vs-2017-standalone-installer"></a>SSDT pour Visual Studio 2017 (programme d’installation autonome)

:::image type="icon" source="media/download.png" border="false"::: **[Télécharger SSDT pour Visual Studio 2017 (15.9.7)](https://go.microsoft.com/fwlink/?linkid=2151759)**

> [!IMPORTANT]
> * Avant d’installer SSDT pour Visual Studio 2017 (15.9.7), désinstallez les extensions *Projets Analysis Services* et *Projets Reporting Services* si elles sont déjà installées, puis fermez toutes les instances de VS. 
> * Suppression du composant de boîte de réception Source Power Query pour SQL Server 2017. Nous avons maintenant annoncé la source Power Query comme composant prêt à l’emploi pour SQL Server 2017 & 2019, et vous pouvez le télécharger [ici](https://www.microsoft.com/download/details.aspx?id=100619).
> * Pour concevoir des packages à l’aide de connecteurs Oracle et Teradata et ciblant une version de SQL Server antérieure à SQL 2019, en plus du [Connecteur Microsoft Oracle pour SQL 2019](https://www.microsoft.com/download/details.aspx?id=58228) et du [Connecteur Microsoft Teradata pour SQL 2019](https://www.microsoft.com/download/details.aspx?id=100599), vous devez également installer la version correspondante du Connecteur Microsoft pour Oracle et Teradata par Attunity.
>    * [Connecteur Microsoft Version 5.0 pour Oracle and Teradata par Attunity ciblant SQL Server 2017](https://www.microsoft.com/download/details.aspx?id=55179)
>    * [Connecteur Microsoft Version 4.0 pour Oracle and Teradata par Attunity ciblant SQL Server 2016](https://www.microsoft.com/download/details.aspx?id=52950)
>    * [Connecteur Microsoft Version 3.0 pour Oracle and Teradata par Attunity ciblant SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=44582)
>    * [Connecteur Microsoft Version 2.0 pour Oracle and Teradata par Attunity ciblant SQL Server 2012](https://www.microsoft.com/download/details.aspx?id=29283)

### <a name="release-notes"></a>Notes de publication

Pour obtenir une liste complète des modifications, consultez [Notes de publication pour SQL Server Data Tools (SSDT)](release-notes-ssdt.md).

### <a name="system-requirements"></a>Configuration système requise

SSDT pour Visual Studio 2017 nécessite la même [configuration](/visualstudio/productinfo/vs2017-system-requirements-vs) que Visual Studio.

### <a name="available-languages---ssdt-for-vs-2017"></a>Langues disponibles : SSDT pour VS 2017

Vous pouvez installer cette version de **SSDT pour VS 2017** dans les langues suivantes :

* [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x804)
* [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x404)
* [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x409)
* [Français](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x40c)
* [Allemand](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x407)
* [Italien](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x410)
* [Japonais](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x411)
* [Coréen](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x412)
* [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x416)
* [Russe](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x419)
* [Espagnol](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x40a)

### <a name="considerations-and-limitations"></a>Observations et limitations

* Vous ne pouvez pas installer la version Community hors connexion

* Pour mettre à niveau SSDT, vous devez effectuer les mêmes étapes que celles suivies pour installer SSDT. Par exemple, si vous avez ajouté SSDT à l’aide des extensions VSIX, vous devez effectuer la mise à niveau par le biais des extensions VSIX. Si vous avez installé SSDT par le biais d’une installation distincte, vous devez effectuer la mise à niveau à l’aide de cette méthode.

## <a name="offline-install"></a>Installation hors connexion

Pour installer SSDT quand vous n’êtes pas connecté à Internet, suivez les étapes de cette section. Pour plus d’informations, consultez [Créer une installation réseau de Visual Studio 2017](/visualstudio/install/create-a-network-installation-of-visual-studio).

Commencez par effectuer les étapes suivantes tant que vous êtes **en ligne** :

1. [Télécharger le programme d’installation autonome SSDT](#ssdt-for-vs-2017-standalone-installer).

2. [Télécharger vs_sql.exe](https://aka.ms/vs/15/release/vs_sql.exe).

3. Pendant que vous êtes toujours en ligne, exécutez l’une des commandes suivantes pour télécharger tous les fichiers nécessaires à l’installation hors connexion. L’utilisation de l’option `--layout` est primordiale. Elle permet de télécharger les fichiers en vue d’une installation hors connexion. Remplacez `<filepath>` par le vrai chemin des dispositions pour y enregistrer les fichiers.
   1. Pour une langue spécifique, passez les paramètres régionaux : `vs_sql.exe --layout c:\<filepath> --lang en-us` (une seule langue correspond environ à 1 Go).
   1. Pour toutes les langues, omettez l’argument `--lang` : `vs_sql.exe --layout c:\<filepath>` (toutes les langues correspondent environ à 3,9 Go).

Après avoir effectué les étapes précédentes, vous pouvez effectuer celles-ci **hors connexion** :

1. Exécutez `vs_setup.exe --NoWeb` pour installer l’interpréteur de commandes de VS 2017 et le projet de données SQL Server.

2. À partir du dossier de dispositions, exécutez `SSDT-Setup-ENU.exe /install`, puis sélectionnez SSRS/SSIS/SSAS.
   a. Pour une installation sans assistance, exécutez `SSDT-Setup-ENU.exe /INSTALLALL[:vsinstances] /passive`.

Pour les options disponibles, exécutez `SSDT-Setup-ENU.exe /help`

> [!NOTE]
> Si vous utilisez une version complète de Visual Studio 2017, créez un dossier en mode hors connexion pour SSDT uniquement et exécutez `SSDT-Setup-ENU.exe` à partir de ce dossier nouvellement créé. (N’ajoutez pas SSDT à une autre disposition hors connexion de Visual Studio 2017). Si vous ajoutez la disposition SSDT à une disposition hors connexion existante de Visual Studio, les composants d’exécution (.exe) nécessaires ne sont pas créés ici.

## <a name="supported-sql-versions"></a>Versions de SQL prises en charge

|Modèles de projet|Plateformes SQL prises en charge|
|-------------------|--------------------|
|Bases de données relationnelles| SQL Server 2005\* - SQL Server 2017<br> (utilisez SSDT 17.x ou SSDT pour Visual Studio 2017 pour vous connecter à [SQL Server sur Linux](../linux/sql-server-linux-overview.md))<br /><br />Azure SQL Database<br /><br />Azure Synapse Analytics (prend uniquement en charge les requêtes ; les projets de base de données ne sont pas encore pris en charge)<br /><br /> \* La prise en charge de SQL Server 2005 est dépréciée,<br /><br /> passez à une version de SQL officiellement prise en charge|
|Modèles Analysis Services<br /><br />Reporting Services, rapports | SQL Server 2008 - SQL Server 2017|
|Integration Services, packages| SQL Server 2012 - SQL Server 2019 |

## <a name="dacfx"></a>DacFx

SSDT pour Visual Studio 2015 et 2017 utilisent tous deux DacFx 17.4.1 : [Télécharger le framework d’application de la couche Données (DacFx) 17.4.1](https://www.microsoft.com/download/details.aspx?id=56508).

## <a name="previous-versions"></a>Versions précédentes

Pour télécharger et installer SSDT pour Visual Studio 2015 ou une version précédente de SSDT, consultez [Versions précédentes de SQL Server Data Tools (SSDT et SSDT-BI)](previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi.md).

## <a name="see-also"></a>Voir aussi

* [Forum MSDN SSDT](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=ssdt) 

* [Blog de l’équipe SSDT](/archive/blogs/ssdt/)

* [Référence de l’API DACFx](/previous-versions/sql/sql-server-2014/dn645454(v=sql.120))

* [Télécharger SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)

## <a name="next-steps"></a>Étapes suivantes

Après l’installation de SSDT, parcourez ces tutoriels pour apprendre à créer des bases de données, des packages, des modèles de données et des rapports à l’aide de SSDT.

* [Développement de base de données hors connexion orienté projet](project-oriented-offline-database-development.md)

* [Tutoriel SSIS : Créer un package ETL simple](../integration-services/ssis-how-to-create-an-etl-package.md)

* [Didacticiels sur Analysis Services](/analysis-services/analysis-services-tutorials-ssas)

* [Créer un rapport de tableau de base (didacticiel SSRS)](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
