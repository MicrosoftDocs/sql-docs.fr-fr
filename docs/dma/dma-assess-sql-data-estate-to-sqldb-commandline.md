---
title: 'DMACMD : évaluer SQL Server préparation à migrer vers Azure SQL'
titleSuffix: Data Migration Assistant
description: Découvrez comment utiliser Assistant Migration de données outil en ligne de commande (DMACMD) pour évaluer un SQL Server de données pour la migration vers Azure SQL
ms.date: 10/02/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: ''
ms.openlocfilehash: 8688fa61a1ccaede3bd370e1d19ccd66531f5739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060934"
---
# <a name="dmacmd-assess-readiness-of-a-sql-server-data-estate-migrating-to-azure-sql"></a>DMACMD : évaluer la préparation d’un SQL Server de données migration vers Azure SQL 

Dans le cas de nombreuses organisations tentant de migrer vers Azure, il est essentiel d’évaluer les instances de SQL Server locales existantes et d’identifier les Azure SQL Database Azure SQL Target-, Azure SQL Managed Instance ou SQL Server sur les machines virtuelles Azure. 

[Assistant Migration de données (DMA)](dma-overview.md) permet d’évaluer une instance SQL Server pour une cible SQL Azure spécifique et évalue la préparation des bases de données SQL Server qui migrent vers Azure SQL. Chargez les résultats de l’évaluation DMA dans Azure Migrate Hub pour une vue de préparation centralisée de l’ensemble de l’espace de données. 

Cet article vous apprend à effectuer des évaluations à l’échelle et à charger les résultats dans Azure Migrate Hub à l’aide de l’interface de ligne de commande DMA (DMACMD). Vous pouvez également utiliser l' [interface graphique utilisateur DMA](dma-assess-sql-data-estate-to-sqldb.md) pour effectuer l’évaluation à la place.

Pour plus d’informations, consultez la vidéo channel9 suivante :

>
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/How-to-Assess-Readiness-of-SQL-Server-Data-Estate-Migrating-to-Azure-SQL/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a>Prérequis 

Pour utiliser DMACMD pour effectuer une évaluation et charger les résultats dans Azure Migrate Hub, vous avez besoin des éléments suivants : 

- La [dernière version de Assistant Migration de données (DMA)](https://www.microsoft.com/en-us/download/details.aspx?id=53595).
- [Projet Azure Migrate](dma-assess-sql-data-estate-to-sqldb.md#create-a-project-and-add-a-tool) 
- Accès au rôle collaborateur pour la ressource de projet Azure Migrate.

## <a name="use-dmacmd"></a>Utiliser DMACMD

Utilisez un fichier XML comme entrée pour exécuter des évaluations à l’échelle à l’aide de l’interface de ligne de commande (DMACMD.exe). 

Utilisez l’exemple de commande suivant pour transmettre un fichier XML à DMACMD et démarrer l’évaluation :

```console
C:\Program Files\Microsoft Data Migration Assistant\DmaCmd.exe /Action=Assess /AssessmentConfiguration= C:\Demo\ScaleAssessment\Assess-for-AzureSQLMI.xml
```

Le contenu de l’exemple `Assess-for-AzureSQLMI.xml` définit les éléments pour évaluer SQL Server instances pour une cible SQL Managed instance : 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<AssessmentConfiguration xmlns="http://microsoft.com/schemas/SqlServer/Advisor/AssessmentConfiguration">
   <AssessmentName>Scale-Assessment-for-AzureSQLManagedInstance</AssessmentName>
   <AssessmentSourcePlatform>SqlOnPrem</AssessmentSourcePlatform>
   <AssessmentTargetPlatform>ManagedSqlServer</AssessmentTargetPlatform>
   <AssessmentDatabases>
      <AssessmentDatabase>Server=ServerName\SQL2017;Integrated Security=true</AssessmentDatabase>
      <AssessmentDatabase>Server=ServerName\SQL2016;Integrated Security=true;Initial Catalog=AdventureWorks2016</AssessmentDatabase>
      <AssessmentDatabase>Server=ServerName\SQL2016;Integrated Security=true;Initial Catalog=TestDB</AssessmentDatabase>
   </AssessmentDatabases>
   <AssessmentResultDma>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.dma</AssessmentResultDma>
   <AssessmentResultJson>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.json</AssessmentResultJson>
   <AssessmentResultCsv>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.csv</AssessmentResultCsv>
   <AssessmentOverwriteResult>true</AssessmentOverwriteResult>
   <AssessmentEvaluateCompatibilityIssues>true</AssessmentEvaluateCompatibilityIssues>
   <AssessmentEvaluateFeatureParity>true</AssessmentEvaluateFeatureParity>
   <AzureCloudEnvironment>Azure</AzureCloudEnvironment>
   <SubscriptionId>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx</SubscriptionId>
   <AzureMigrateProjectName>Scale-Assessment-for-AzureSQLMI</AzureMigrateProjectName>
   <ResourceGroupName>Resource-Group-Name</ResourceGroupName>
   <AzureAuthenticationInteractiveAuthentication>true</AzureAuthenticationInteractiveAuthentication>
   <AzureAuthenticationTenantId>xxxxxxxx-xxxx-xxxxxxxx</AzureAuthenticationTenantId>
   <EnableAssessmentUploadToAzureMigrate>true</EnableAssessmentUploadToAzureMigrate>
</AssessmentConfiguration>
```



## <a name="xml-elements"></a>Éléments XML 

Les éléments XML qui sont passés à DMACMD sont définis dans le tableau suivant : 


|**Élément XML** |**Definition**  |
|---------|---------|
|`AssessmentName`|Nom de l’évaluation|
|`AssessmentSourcePlatform`|Plateforme de SQL Server source. La valeur par défaut est `SqlOnPrem`.|
|`AssessmentTargetPlatform`|Plateforme de SQL Server cible.  </br> `AzureSqlDatabase` est pour une cible de Azure SQL Database. </br> `ManagedSqlServer` est destiné à une cible Azure SQL Managed Instance. </br></br>L’exemple **d’évaluation-for-AzureSQLMI** évalue une cible SQL Managed instance.|
|`AssessmentDatabases`|Si vous devez évaluer toutes les bases de données dans une instance, spécifiez simplement le nom de l’instance, listez les bases de données spécifiques de chaque ligne. </br></br>L’exemple **d’évaluation-for-AzureSQLMI** évalue toutes les bases de données dans l’instance `Servername\SQL2017` et deux bases de données spécifiques dans l’instance `Servername\SQL2016` .  |
|`AssessmentResultDma` </br> `AssessmentResultJson` </br> `AssessmentResultCsv` | Spécifie le format du fichier de résultats. `.DMA`, `.JSON` et `.CSV` respectivement. Double-cliquez `.DMA` pour ouvrir dans l’interface utilisateur DMA. <br> `AssessmentResultDma` est requis pour charger les résultats de l’évaluation dans Azure Migrate Hub.  |
|`AssessmentOverwriteResult`| Indique s’il faut remplacer un fichier de résultats d’évaluation existant avec le même chemin d’accès que `AssessmentResultJson` , `AssessmentResultDma` ou `AssessmentResultCsv` .|
|`AssessmentEvaluateCompatibilityIssues` </br> `AssessmentEvaluateFeatureParity` |Effectuez l’évaluation pour évaluer les problèmes de compatibilité et les problèmes de parité de fonctionnalité, respectivement.|
|`AzureCloudEnvironment`|Environnement Cloud Azure pour la connexion, par défaut, le cloud public Azure. </br></br> Valeurs prises en charge : </br>`Azure (default)`, `AzureChina`, `AzureGermany`, `AzureUSGovernment`.|
|`SubscriptionId`|ID d’abonnement Azure.|
|`AzureMigrateProjectName`|Azure Migrate nom du projet dans lequel télécharger les résultats de l’évaluation.|
|`ResourceGroupName`|Nom du groupe de ressources Azure Migrate.|
|`AzureAuthenticationInteractiveAuthentication`|Définissez sur `true` pour ouvrir la fenêtre d’authentification.|
|`AzureAuthenticationTenantId`|ID de locataire Azure Active Directory. </br></br>Pour obtenir cela, vous avez le panneau **vue d’ensemble** de Azure Active Directory dans le [portail Azure](https://portal.azure.com). |
|`EnableAssessmentUploadToAzureMigrate`| Définissez sur `true` pour télécharger et publier les résultats de l’évaluation dans Azure Migrate Hub.|
|   |   |


## <a name="results"></a>Résultats 

DMACMD génère un État lorsqu’il se termine correctement. 


Voici un exemple de sortie de résultat : 

```console
Assessment finished for project: Scale-Assessment-for-AzureSQLManagedInstance
DATABASES:
Succeeded             : 4
Failed                : 0
SERVER INSTANCES:
Succeeded             : 2
Failed                : 0
CSV result file       : C:\Demo\ScaleAssessment\Scale-Assessment-for-AzureSQLManagedInstance.csv
JSON result file      : C:\Demo\ScaleAssessment\Scale-Assessment-for-AzureSQLManagedInstance.json
--------------------------------------------------------------------------------
```

Affichez les résultats chargés dans [Azure Migrate](dma-assess-sql-data-estate-to-sqldb.md#view-target-readiness-assessment-results) pour une vue centralisée de l’ensemble de l’espace de données. . 

## <a name="best-practices"></a>Meilleures pratiques 

Tenez compte des meilleures pratiques suivantes lors de l’utilisation de DMACMD : 

- Regroupez de manière logique les instances et les bases de données SQL Server basées sur l’application, au lieu d’évaluer toutes les instances de SQL Server dans l’ensemble des données. 
- Créez un projet de Azure Migrate distinct pour chaque cible SQL Azure afin d’éviter le remplacement des résultats. 
- Le temps nécessaire à l’exécution d’une évaluation dépend du nombre d’objets de base de données. Si possible, évitez d’exécuter des évaluations sur le système de production et de décharger à la place un ordinateur virtuel ou un serveur intermédiaire, en particulier pour les bases de données avec un grand nombre d’objets. 


## <a name="see-also"></a>Voir également

* [Assistant Migration de données (DMA)](../dma/dma-overview.md)
* [Assistant Migration de données : paramètres de configuration](../dma/dma-configurationsettings.md)
* [Assistant Migration de données : meilleures pratiques](../dma/dma-bestpractices.md)

