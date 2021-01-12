---
title: Évaluer une entreprise et consolider les rapports d’évaluation avec Assistant Migration de données
description: Découvrez comment utiliser DMA pour évaluer une entreprise et consolider les rapports d’évaluation avant de mettre à niveau SQL Server ou de migrer vers Azure SQL Database.
ms.date: 06/21/2019
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
ms.openlocfilehash: 4d1bcf2cdc9997ea33be87184c38447b7cd62973
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091605"
---
# <a name="assess-an-enterprise-and-consolidate-assessment-reports-with-dma"></a>Évaluer une entreprise et consolider les rapports d’évaluation à l’aide de DMA

Les instructions pas à pas suivantes vous aident à utiliser le Assistant Migration de données pour effectuer une évaluation à l’échelle réussie pour la mise à niveau d’un SQL Server local ou d’un SQL Server s’exécutant sur des machines virtuelles Azure, ou pour la migration vers Azure SQL Database.

## <a name="prerequisites"></a>Prérequis

- Désignez un ordinateur d’outils sur votre réseau à partir duquel le transfert DMA sera initié. Assurez-vous que cet ordinateur dispose d’une connectivité à vos cibles de SQL Server.
- Téléchargez et installez :
  - [Assistant Migration de données](https://www.microsoft.com/download/details.aspx?id=53595) v 3.6 ou version ultérieure.
  - [PowerShell](https://aka.ms/wmf5download) v 5.0 ou version ultérieure.
  - [.NET Framework](https://www.microsoft.com/download/details.aspx?id=30653) v 4.5 ou version ultérieure.
  - [SSMS](../ssms/download-sql-server-management-studio-ssms.md) 17,0 ou version ultérieure.
  - [Power bi Desktop](/power-bi/fundamentals/desktop-get-the-desktop).
  - [Modules Azure PowerShell](/powershell/azure/install-az-ps)
- Télécharger et extraire :
  - Les [rapports DMA Power bi modèle](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/4/PowerBI-Reports.zip).
  - [Script LoadWarehouse](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/3/LoadWarehouse1.zip).

## <a name="loading-the-powershell-modules"></a>Chargement des modules PowerShell

L’enregistrement des modules PowerShell dans le répertoire des modules PowerShell vous permet d’appeler les modules sans avoir à les charger explicitement avant de les utiliser.

Pour charger les modules, procédez comme suit :

1. Accédez à C:\Program Files\WindowsPowerShell\Modules, puis créez un dossier nommé **DataMigrationAssistant**.
2. Ouvrez [PowerShell-modules](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/1/PowerShell-Modules2.zip), puis enregistrez-les dans le dossier que vous avez créé.

      ![Modules PowerShell](../dma/media//dma-consolidatereports/dma-powershell-modules.png)

    Chaque dossier contient le fichier psm1 associé, comme indiqué dans le graphique suivant :

   ![Fichiers psm1 des modules PowerShell](../dma/media//dma-consolidatereports/dma-powershell-modules-psm1-files.png)

   > [!NOTE]
   > Le dossier et le fichier psm1 qu’il contient doivent avoir le même nom.

   > [!IMPORTANT]
   > Vous devrez peut-être débloquer les fichiers PowerShell après les avoir enregistrés dans le répertoire WindowsPowerShell pour garantir le chargement correct des modules. Pour débloquer un fichier PowerShell, cliquez avec le bouton droit sur le fichier, sélectionnez **Propriétés**, sélectionnez la zone de texte **débloquer** , puis cliquez sur **OK**.

   ![Propriétés du fichier psm1](../dma/media//dma-consolidatereports/dma-psm1-file-properties.png)

    PowerShell doit maintenant charger ces modules automatiquement lors du démarrage d’une nouvelle session PowerShell.

## <a name="create-an-inventory-of-sql-servers"></a><a name="create-inventory"></a> Créer un inventaire des serveurs SQL

Avant d’exécuter le script PowerShell pour évaluer vos serveurs SQL, vous devez créer un inventaire des serveurs SQL que vous souhaitez évaluer.

Cet inventaire peut être de l’une des deux formes suivantes :

- Fichier CSV Excel
- Table SQL Server

### <a name="if-using-a-csv-file"></a>Si vous utilisez un fichier CSV

> [!IMPORTANT]
> Assurez-vous que le fichier d’inventaire est enregistré sous la forme d’un fichier de valeurs séparées par des virgules (CSV).
>
> Pour les instances par défaut, définissez le nom de l’instance sur MSSQLServer.

Lorsque vous utilisez un fichier CSV pour importer les données, assurez-vous qu’il n’y a que deux colonnes de **nom d’instance** de données et **de nom de base de** données, et que les colonnes n’ont pas de lignes d’en-tête.

 ![contenu du fichier CSV](../dma/media//dma-consolidatereports/dma-csv-file-contents.png)

### <a name="if-using-a-sql-server-table"></a>Si vous utilisez une table SQL Server

> [!IMPORTANT]
> Pour les instances par défaut, définissez le nom de l’instance sur MSSQLServer.

Créez une base de données appelée **EstateInventory** et une table appelée **DatabaseInventory**. La table contenant ces données d’inventaire peut comporter un nombre quelconque de colonnes, tant que les quatre colonnes suivantes existent :

- ServerName
- InstanceName
- nom_base_de_données
- AssessmentFlag

![Capture d’écran d’un contenu de table SQL Server si vous utilisez une table SQL Server.](../dma/media//dma-consolidatereports/dma-sql-server-table-contents-database-inventory.png)

Si cette base de données ne se trouve pas sur l’ordinateur des outils, assurez-vous que l’ordinateur des outils dispose d’une connectivité réseau avec cette instance de SQL Server.

L’avantage de l’utilisation d’une table SQL Server sur un fichier CSV est que vous pouvez utiliser la colonne Indicateur d’évaluation pour contrôler l’instance ou la base de données qui est sélectionnée pour l’évaluation, ce qui facilite la séparation des évaluations en plus petits segments.  Vous pouvez ensuite étendre plusieurs évaluations (consultez la section sur l’exécution d’une évaluation plus loin dans cet article), qui est plus facile que la gestion de plusieurs fichiers CSV.

Gardez à l’esprit que, en fonction du nombre d’objets et de leur complexité, une évaluation peut prendre beaucoup de temps (heures +), il est donc prudent de séparer l’évaluation en segments gérables.

### <a name="if-using-an-instance-inventory"></a>Si vous utilisez un inventaire d’instances

Créez une base de données appelée **EstateInventory** et une table appelée **InstanceInventory**. La table contenant ces données d’inventaire peut comporter un nombre quelconque de colonnes, tant que les quatre colonnes suivantes existent :

- ServerName
- InstanceName
- Port
- AssessmentFlag

![Capture d’écran d’un contenu de table SQL Server en cas d’utilisation d’un inventaire d’instance.](../dma/media//dma-consolidatereports/dma-sql-server-table-contents-instance-inventory.png)

## <a name="running-a-scaled-assessment"></a>Exécution d’une évaluation avec montée en charge

Après avoir chargé les modules PowerShell dans le répertoire Modules et créé un inventaire, vous devez exécuter une évaluation mise à l’échelle en ouvrant PowerShell et en exécutant la fonction dmaDataCollector.

  ![listes de fonctions dmaDataCollector](../dma/media//dma-consolidatereports/dma-dmaDataCollector-function-listing.png)

Les paramètres associés à la fonction dmaDataCollector sont décrits dans le tableau suivant.

|Paramètre  |Description |
|---------|---------|
|**getServerListFrom** | Votre inventaire. Les valeurs possibles sont **SqlServer** et **CSV**.<br/>Pour plus d’informations, consultez [créer un inventaire des serveurs SQL](#create-inventory). |
|**csvPath** | Chemin d’accès à votre fichier d’inventaire CSV.  Utilisé uniquement lorsque **getServerListFrom** a la valeur  **CSV**. |
|**serverName** | Nom de l’instance de SQL Server de l’inventaire lors de l’utilisation de **SqlServer** dans le paramètre **getServerListFrom** . |
|**databaseName** | Base de données hébergeant la table d’inventaire. |
|**useInstancesOnly** | Indicateur binaire pour spécifier s’il faut utiliser une liste d’instances à des fins d’évaluation.  Si la valeur est 0, la table DatabaseInventory sera utilisée pour générer la liste des cibles d’évaluation. |
|**AssessmentName** | Nom de l’évaluation DMA. |
|**TargetPlatform** | Type de cible d’évaluation que vous souhaitez effectuer.  Les valeurs possibles sont **AzureSQLDatabase**, **ManagedSqlServer**, **SQLServer2012**, **SQLServer2014**, **SQLServer2016**, **SQLServerLinux2017**, **SQLServerWindows2017**,  **SqlServerWindows2019** et **SqlServerLinux2019**.  |
|**AuthenticationMethod** | La méthode d’authentification pour la connexion aux cibles de SQL Server que vous souhaitez évaluer. Les valeurs possibles  sont SQLAuth **et l'** interversion. |
|**OutputLocation** | Répertoire dans lequel stocker le fichier de sortie de l’évaluation JSON. En fonction du nombre de bases de données en cours d’évaluation et du nombre d’objets dans les bases de données, les évaluations peuvent prendre beaucoup de temps. Le fichier sera écrit une fois toutes les évaluations terminées. |

S’il y a une erreur inattendue, la fenêtre de commande qui est lancée par ce processus va être arrêtée.  Examinez le journal des erreurs pour déterminer la raison de l’échec.

  ![Emplacement du journal des erreurs](../dma/media//dma-consolidatereports/dma-error-log-file-location.png)

## <a name="consuming-the-assessment-json-file"></a>Consommation du fichier JSON d’évaluation

Une fois votre évaluation terminée, vous êtes maintenant prêt à importer les données dans SQL Server pour l’analyse. Pour utiliser le fichier JSON d’évaluation, ouvrez PowerShell et exécutez la fonction dmaProcessor.

  ![liste de fonctions dmaProcessor](../dma/media//dma-consolidatereports/dma-dmaProcessor-function-listing.png)

Les paramètres associés à la fonction dmaProcessor sont décrits dans le tableau suivant.

|Paramètre  |Description |
|---------|---------|
|**processTo** | Emplacement dans lequel le fichier JSON sera traité. Les valeurs possibles sont **SqlServer** et **AzureSQLDatabase**. |
|**serverName** | Instance SQL Server vers laquelle les données seront traitées.  Si vous spécifiez **AzureSQLDatabase** pour le paramètre **processTo** , ajoutez uniquement le nom du SQL Server (n’incluez pas. Database.Windows.net). Vous serez invité à entrer deux connexions quand vous ciblez Azure SQL Database ; la première est celle de vos informations d’identification de locataire Azure, tandis que la seconde est votre connexion d’administrateur pour le SQL Server Azure. |
|**CreateDMAReporting** | Base de données de mise en lots à créer pour traiter le fichier JSON.  Si la base de données que vous spécifiez existe déjà et que vous définissez ce paramètre sur un, les objets ne sont pas créés.  Ce paramètre est utile pour recréer un objet unique qui a été supprimé. |
|**CreateDataWarehouse** | Crée l’entrepôt de données qui sera utilisé par le rapport Power BI. |
|**databaseName** | Nom de la base de données DMAReporting. |
|**warehouseName** | nom de la base de données de l'entrepôt de données. |
|**jsonDirectory** | Répertoire contenant le fichier d’évaluation JSON.  S’il y a plusieurs fichiers JSON dans le répertoire, ils sont traités un par un. |

La fonction dmaProcessor ne doit prendre que quelques secondes pour traiter un fichier unique.

## <a name="loading-the-data-warehouse"></a>Chargement de l’entrepôt de données

Une fois que le dmaProcessor a terminé le traitement des fichiers d’évaluation, les données sont chargées dans la base de données DMAReporting de la table ReportData. À ce stade, vous devez charger l’entrepôt de données.

1. Utilisez le script LoadWarehouse pour renseigner les valeurs manquantes dans les dimensions.

    Le script extrait les données de la table ReportData dans la base de données DMAReporting et les charge dans l’entrepôt.  Si des erreurs se produisent pendant ce processus de chargement, elles sont probablement le résultat d’entrées manquantes dans les tables de dimension.

2. Chargez l’entrepôt de données.

  ![Contenu LoadWarehouse chargé](../dma/media//dma-consolidatereports/dma-load-warehouse-loaded.png)

## <a name="set-your-database-owners"></a>Définir les propriétaires de votre base de données

Bien qu’il ne soit pas obligatoire, pour tirer le meilleur parti des rapports, il est recommandé de définir les propriétaires de base de données dans la dimension **dimDBOwner** , puis de mettre à jour **DBOwnerKey** dans la table **FactAssessment** .  Le processus suivant permet de découper et de filtrer les Power BI rapport en fonction de propriétaires de bases de données spécifiques.

Vous pouvez également utiliser le script LoadWarehouse pour fournir les instructions TSQL de base pour vous permettre de définir les propriétaires de base de données.

  ![Propriétaires de paramètres LoadWarehouse](../dma/media//dma-consolidatereports/dma-load-warehouse-set-owners.png)

## <a name="dma-reports"></a>Rapports DMA

1. Ouvrez le modèle de Power BI de rapports DMA dans le Power BI Desktop.
2. Entrez les détails du serveur qui pointent vers votre base de données **DMAWarehouse** , puis sélectionnez **charger**.

   ![Rapports DMA Power BI le modèle chargé](../dma/media//dma-consolidatereports/dma-reports-powerbi-template-loaded.png)

   Une fois que le rapport a actualisé les données de la base de données **DMAWarehouse** , un rapport semblable au suivant s’affiche.

   ![Vue rapport DMAWarehouse](../dma/media//dma-consolidatereports/dma-DMAWarehouse-report1.png)

   > [!TIP]
   > Si vous ne voyez pas les données attendues, essayez de modifier le signet actif.  Pour plus d’informations, consultez les détails dans la section suivante.

## <a name="working-with-dma-reports"></a>Utilisation des rapports DMA

Pour utiliser des rapports DMA, utilisez des signets et des segments pour filtrer par :

- Types d’évaluation (Azure SQL Database, Managed Instance SQL Azure, SQL Server) 
- Nom de l’instance
- Nom de la base de données
- Nom de l'équipe

Pour accéder au panneau signets et filtres, sélectionnez le signet filtres dans la page principale du rapport :

![Filtres et signets de rapport DMA](../dma/media//dma-consolidatereports/dma-report-bookmarks-filters.png)

La sélection du signet Filters active le panneau suivant :

![Panneau vues des rapports DMA](../dma/media//dma-consolidatereports/dma-report-views-blade.png)

Vous pouvez utiliser des signets pour basculer le contexte de création de rapports entre :

- Évaluations du Cloud Azure SQL Database
- Évaluations du Cloud Azure SQL Managed Instance
- Évaluations locales

![Signets de vue de rapport DMA](../dma/media//dma-consolidatereports/dma-report-bookmarks1.png)

Pour masquer le panneau filtres, cliquez sur le bouton précédent :

![Bouton précédent des vues de rapport DMA](../dma/media//dma-consolidatereports/dma-report-bookmarks-back.png)

Une invite s’affiche en bas à gauche de la page de rapport pour indiquer si un filtre est actuellement appliqué à l’un des éléments suivants :

- FactAssessment – nom_instance
- FactAssessment – DatabaseName
- dimDBOwner-DBOwner

![Invite filtre appliqué](../dma/media//dma-consolidatereports/dma-filter-applied-prompt.png)

> [!NOTE]
> Si vous effectuez uniquement une évaluation de Azure SQL Database, seuls les rapports Cloud sont remplis. À l’inverse, si vous effectuez uniquement une évaluation locale, seuls les rapports sur site sont remplis. Toutefois, si vous effectuez une évaluation Azure et locale, puis chargez les deux évaluations dans votre entrepôt, vous pouvez basculer entre les rapports Cloud et les rapports locaux en cliquant avec le bouton droit sur l’icône associée.

## <a name="reports-visuals"></a>Visuels de rapports

Les détails affichés dans les rapports Power BI sont présentés dans les sections suivantes.

### <a name="readiness-"></a>Préparation

  ![Pourcentage de disponibilité de DMA](../dma/media//dma-consolidatereports/dma-readiness-percentage.png)

Cet élément visuel est mis à jour en fonction du contexte de sélection (tout, instance, base de données [multiple de]).

### <a name="readiness-count"></a>Nombre de disponibilités

  ![Nombre de disponibilités de DMA](../dma/media//dma-consolidatereports/dma-readiness-count.png)

Cet visuel indique le nombre de bases de données qui sont prêtes à migrer le nombre de bases de données qui ne sont pas encore prêtes à migrer.

### <a name="readiness-bucket"></a>Compartiment de préparation

  ![Compartiment de disponibilité DMA](../dma/media//dma-consolidatereports/dma-readiness-bucket.png)

Cet visuel illustre la répartition des bases de données par les compartiments de disponibilité suivants :

- 100% PRÊT
- 75-99% PRÊT
- 50-75% PRÊT
- NON PRÊT

### <a name="issues-word-cloud"></a>Problèmes liés au Cloud

  ![Problèmes DMA WordCloud](../dma/media//dma-consolidatereports/dma-issues-word-cloud.png)

Cet élément visuel indique les problèmes qui se produisent actuellement dans le contexte de sélection (tout, instance, base de données [multiple de]). Plus le mot s’affiche à l’écran, plus le nombre de problèmes dans cette catégorie est élevé. Placez le pointeur de la souris sur un mot pour afficher le nombre de problèmes survenant dans cette catégorie.

### <a name="database-readiness"></a>Préparation de la base de données

  ![Rapport de disponibilité de la base de données DMA](../dma/media//dma-consolidatereports/dma-database-readiness-report.png)

Cette section est la partie principale du rapport, qui indique la disponibilité d’une base de données d’instance. Ce rapport présente une hiérarchie descendante :

- InstanceDatabase
- ChangeCategory
- Intitulé
- ObjectType
- ImpactedObjectName

 ![Exploration du rapport d’avancement de la base de données DMA](../dma/media//dma-consolidatereports/dma-database-readiness-report-drilldown.png)

Ce rapport sert également de point de filtre pour la création du rapport de plan de correction.

Pour approfondir le rapport de plan de correction, cliquez avec le bouton droit sur un point de données dans ce graphique, pointez sur **extraction**, puis sélectionnez **plans de correction**.

Cette tâche filtre le rapport de plan de correction au niveau de la hiérarchie actuelle en fonction du point auquel vous sélectionnez l’option d’extraction.

  ![Exploration du rapport d’avancement de la base de données DMA filtrée](../dma/media//dma-consolidatereports/dma-database-readiness-report-drilldown-filtered.png)

  ![Rapport du plan de correction DMA](../dma/media//dma-consolidatereports/dma-remediation-plan-report.png)

Vous pouvez également utiliser le rapport de plan de correction en soi pour créer un plan de correction personnalisé à l’aide des filtres du panneau **filtres de visualisation** .

  ![Options de filtre de rapport du plan de correction DMA](../dma/media//dma-consolidatereports/dma-remediation-plan-report-filter-options.png)

### <a name="script-disclaimer"></a>Exclusion de scripts

*Les exemples de scripts fournis dans cet article ne sont pris en charge par aucun programme ou service de support standard de Microsoft. Tous les scripts sont fournis en l’État sans garantie d’aucune sorte. Microsoft décline toute garantie implicite, y compris, sans limitation, toute garantie implicite de qualité marchande ou d’adéquation à un usage spécifique. L’ensemble des risques liés à l’utilisation ou aux performances des exemples de scripts et de la documentation vous incombe. En aucun cas, Microsoft, ses auteurs, ou toute autre personne participant à la création, à la production ou à la livraison des scripts est responsable de tout dommage (y compris, sans limitation, les dommages liés à la perte de bénéfices, l’interruption d’activité, la perte d’informations commerciales ou toute autre perte pécuniaire) résultant de l’utilisation ou de l’impossibilité d’utiliser les exemples de scripts ou de la documentation, même si Microsoft a été  Demandez l’autorisation avant de reposter ces scripts sur d’autres sites/référentiels/blogs.*