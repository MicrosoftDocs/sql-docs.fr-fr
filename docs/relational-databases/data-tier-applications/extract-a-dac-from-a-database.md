---
description: Extraire une DAC d'une base de données
title: Extraire une DAC d’une base de données | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2016
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.extractdacwizard.validationandsummary.f1
- sql13.swb.extractdacwizard.introduction.f1
- sql13.swb.extractdacwizard.selectdatapage.f1
- sql13.swb.extractdacwizard.setdacproperties.f1
- sql13.swb.extractdacwizard.buildandsave.f1
helpviewer_keywords:
- extract DAC
- How to [DAC], extract
- data-tier application [SQL Server], extract
- wizard [DAC], extract
ms.assetid: ae52a723-91c4-43fd-bcc7-f8de1d1f90e5
author: stevestein
ms.author: sstein
ms.openlocfilehash: 5091c3437d0e74f3c108e13efb3789997cadb2d3
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489463"
---
# <a name="extract-a-dac-from-a-database"></a>Extraire une DAC d'une base de données
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Utilisez **l’Assistant Extraire l’application de la couche Données** ou un script Windows PowerShell pour extraire un package d’application de la couche Données (DAC) d’une base de données SQL Server existante. Le processus d'extraction crée un fichier de package de DAC qui contient les définitions des objets de base de données et de leurs éléments associés au niveau de l'instance. Par exemple, un fichier de package DAC contient les tables de base de données, procédures stockées, vues, utilisateurs, ainsi que les connexions mappées aux utilisateurs de base de données.  
  
 
## <a name="before-you-begin"></a>Avant de commencer  
 Vous pouvez extraire une DAC des bases de données qui résident sur les instances de [!INCLUDE[ssSDS](../../includes/sssds-md.md)]ou [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 4 (SP4) ou version ultérieure. Si le processus d'extraction est exécuté sur une base de données déployée à partir d'une DAC, seules les définitions des objets de la base de données sont extraites. Le processus ne référence pas la DAC enregistrée dans **msdb** (**master** , dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)]). Le processus d'extraction n'inscrit pas la définition de la DAC dans l'instance actuelle du moteur de base de données. Pour plus d'informations sur l'inscription d'une DAC, consultez [Register a Database As a DAC](../../relational-databases/data-tier-applications/register-a-database-as-a-dac.md).  
  
##  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> Limitations et restrictions  
 Une DAC peut uniquement être extraite d'une base de données dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)]ou [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) ou version ultérieure. Vous ne pouvez pas extraire une DAC si la base de données possède des objets qui ne sont pas pris en charge dans une DAC, ou des utilisateurs à relation contenant-contenu. Pour plus d'informations sur les types d'objets pris en charge dans une DAC, consultez [DAC Support For SQL Server Objects and Versions](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110)).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 L’extraction d’un DAC requiert au moins des autorisations ALTER ANY LOGIN et VIEW DEFINITION de la portée de la base de données, ainsi que des autorisations SELECT sur **sys.sql_expression_dependencies**. L'extraction d'un DAC peut être réalisée par les membres du rôle serveur fixe securityadmin également membres du rôle de base de données fixe database_owner dans la base de données de laquelle est extrait le DAC. Les membres du rôle serveur fixe sysadmin ou le compte d’administrateur système intégré de SQL Server nommé **sa** peuvent également extraire une DAC.  
  
##  <a name="using-the-extract-data-tier-application-wizard"></a><a name="UsingDACExtractWizard"></a> Utilisation de l'Assistant Extraire l'application de la couche Données  
 **Pour extraire une DAC à l'aide d'un Assistant**  
  
1.  Dans l' **Explorateur d'objets**, développez le nœud de l'instance contenant la base de données à partir de laquelle la DAC doit être extraite.  
  
2.  Développez le nœud **Bases de données** .  
  
3.  Cliquez avec le bouton droit sur le nœud de la base de données à partir de laquelle la DAC doit être extraite, pointez sur **Tâches**, puis sélectionnez **Extraire une application de la couche Données**.  
  
4.  Renseignez les boîtes de dialogue de l'Assistant :  
  
    1.  [Page Introduction](#Introduction)  
  
    2.  [Page Sélectionner les données](#SelectData)  
  
    3.  [Page Définir les propriétés](#SetProperties)  
  
    4.  [Page Validation et résumé](#ValidateSummary)  
  
    5.  [Page Générer le package](#BuildPackage)  
  
###  <a name="wizard-introduction-page"></a><a name="Introduction"></a> Page Introduction à l’Assistant  
 Cette page décrit les étapes à suivre pour extraire une application de la couche Données.  
  
 **Ne plus afficher cette page.** - Cochez la case pour ne plus afficher la page à l'avenir.  
  
 **Suivant >** : passe à la page **Choisir une méthode**.  
  
 **Annuler** - Termine l’Assistant sans extraire une application de la couche Données de la base de données.  
  
 [&#91;Assistant d’extraction&#93;](#UsingDACExtractWizard)  
  
###  <a name="select-data-page"></a><a name="SelectData"></a> Select data page  
Sélectionnez les données de référence que vous voulez inclure dans votre fichier de package d’application de la couche Données (DAC). L'inclusion de données dans votre package DAC est facultative. Le package DAC inclura déjà le schéma de tous les objets de base de données pris en charge, ainsi que les objets d'instance relatifs à votre base de données.  
  
 Vous pouvez inclure jusqu'à 10 Mo de données de référence dans votre fichier de package DAC. Toutefois, pour que les tables soient inclues dans la DAC, elles ne doivent pas contenir de types de données d’objet BLOB tels que **image** ou **varchar(max)**. Pour extraire des quantités de données plus importantes pour le transfert vers une autre base de données, utilisez SQL Server Integration Services, l'utilitaire de copie en bloc ou l'un des nombreuses autres techniques de migration des données.  
  
 **Table de base de données** - Cochez la case en regard des tables de base de données qui contiennent les données que vous souhaitez inclure dans votre package DAC. Vous pouvez sélectionner jusqu'à dix tables qui ont 10 000 lignes au maximum.  
  
 [&#91;Assistant d’extraction&#93;](#UsingDACExtractWizard)  
  
###  <a name="set-properties-page"></a><a name="SetProperties"></a> Set properties page  
 Utilisez cette page de l'Assistant pour décrire l'application de la couche Données (DAC). Ces propriétés sont utilisées pour identifier la DAC et aider à la distinguer des autres.  
  
 **Nom** - Ce nom identifie la DAC. Il peut être différent du nom du fichier de package de DAC et doit décrire votre application. Par exemple, si la base de données est utilisée pour une application financière, vous pouvez nommer la DAC Finance.  
  
 **Version (utiliser x.x.x.x, où x est un nombre)** - Valeur numérique qui identifie la version de la DAC. La version de la DAC est utilisée dans Visual Studio pour identifier la version de la DAC sur laquelle les développeurs travaillent. Lors du déploiement d’une DAC, la version est stockée dans la base de données **msdb** et peut être affichée ultérieurement sous le nœud **Applications de la couche Données** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Description** : Facultatif. Décrit la DAC. Lors du déploiement d’une DAC, la description est stockée dans la base de données **msdb** et peut être affichée ultérieurement sous le nœud **Applications de la couche Données** dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 **Enregistrer le fichier de package DAC (inclure l’extension .dacpac dans le nom du fichier)** - Enregistre la DAC dans un fichier de package DAC, avec une extension .dacpac. Cliquez sur le bouton **Parcourir** pour spécifier le nom et l'emplacement du fichier.  
  
 **Remplacer le fichier existant** - Cochez la case pour remplacer le fichier du package DAC s’il en existe déjà un du même nom.  
  
###  <a name="validation-and-summary-page"></a><a name="ValidateSummary"></a> Validation and summary page  
 Dans cette page, l'Assistant vérifie que tous les objets de base de données sont pris en charge par une application de la couche Données (DAC). Il vérifie également les dépendances entre les objets de base de données afin de déterminer l'ensemble d'objets qui peuvent être inclus avec succès dans la DAC. Après cela, il affiche le rapport de validation et résume les options que vous avez sélectionnées dans cet Assistant. Pour modifier une option, cliquez sur **Précédent**. Pour commencer à extraire une DAC, cliquez sur **Suivant**.  
  
> **Remarque** Si un ou plusieurs objets ne sont pas pris en charge par une DAC, le bouton **Suivant** est désactivé et le processus d’extraction ne peut pas continuer. Dans ces cas-là, il est recommandé de supprimer les objets non pris en charge, puis de réexécuter cet Assistant.  
  
 **Résumé** - Un résumé des options que vous avez sélectionnées est affiché sous **Propriétés de la DAC**. Les résultats de la validation apparaissent sous **Objets de DAC**. Il existe trois types de résultats de validation :  
  
-   **Objets inclus dans la DAC avec succès**: ces objets et leurs dépendances sont pris en charge et peuvent être inclus avec succès dans la DAC.  
  
-   **Objets inclus dans la DAC avec avertissements**: ces objets sont pris en charge, mais dépendent d'autres objets qui ne sont pas pris en charge dans une DAC.  
  
-   **Objets non inclus dans la DAC**: ces objets ne sont pas pris en charge et doivent être supprimés de la base de données avant d'extraire une DAC avec succès.  
  
 Le processus de validation vérifie plusieurs niveaux de dépendances. Par exemple, si une procédure stockée dépend d'une table qui utilise le type de données CLR non pris en charge, la procédure stockée apparaît sous **Objets inclus dans la DAC avec avertissements**.  
  
 Si un ou plusieurs objets ne sont pas pris en charge par une DAC, le bouton **Suivant** est désactivé et le processus d'extraction est interrompu. Dans ces cas-là, il est recommandé de supprimer les objets non pris en charge, puis de réexécuter cet Assistant.  
  
 **Enregistrer le rapport** - Vous permet d’enregistrer un fichier HTML qui répertorie tous les objets sous le nœud **Objets de DAC** dans le résumé. Ce rapport peut être utile lorsque certains de vos objets de base de données ne sont pas pris en charge dans une DAC. Utilisez le rapport pour changer ou supprimer des objets qui ne sont pas pris en charge, avant d'essayer d'extraire de nouveau la DAC.  
  
 ###  <a name="build-package-page"></a><a name="BuildPackage"></a> Build package page  
 Utilisez cette page pour surveiller la progression de l'Assistant à mesure qu'il extrait l'application de la couche Données (DAC).  
  
 **Action** - Pendant l’action **Créer et enregistrer le fichier de package de DAC** , l’Assistant extrait une DAC de votre base de données SQL Server. Ensuite, un package de DAC est créé en mémoire et enregistré à l'emplacement que vous avez spécifié. Cliquez sur les liens dans la colonne **Résultat** pour consulter le résultat de l'étape correspondante.  
  
 **Enregistrer le rapport** - Enregistre les résultats de la progression de l’Assistant dans un fichier.  
  
 **Terminer** - Ferme l’Assistant une fois le traitement terminé ou si une erreur se produit.  
   
##  <a name="extract-a-dac-using-powershell"></a><a name="ExtractDACPowerShell"></a> Extraire une DAC à l’aide de PowerShell  
 **Pour extraire une DAC d’une base de données à l’aide de la méthode Extract() dans un script PowerShell**  
  
1.  Créez un objet serveur SMO et affectez-lui l'instance qui contient la base de données à partir de laquelle la DAC doit être extraite.  
  
2.  Ajoutez une variable qui spécifie le nom de la base de données.  
  
3.  Spécifiez les métadonnées pour la DAC, par exemple le nom de la DAC, la version et la description.  
  
4.  Spécifiez le chemin d'accès et le nom du fichier de package DAC extrait.  
  
5.  Exécutez la méthode Extract avec les informations spécifiées ci-dessus.  
  
### <a name="example-powershell"></a>Exemple (PowerShell)  
 L'exemple suivant extrait une DAC nommée MyApplication d'une base de données nommée MyDB.  
  
```  
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$srv = get-item .  
  
## Specify the database to extract to a DAC.  
$dbname = "MyDB"  
  
## Specify the DAC metadata.  
$applicationname = "MyApplication"  
$version = "1.0.0.0"  
$description = "This DAC defines the database used by my application."  
  
## Specify the location and name for the extracted DAC package.  
$dacpacPath = "C:\MyDACs\MyApplication.dacpac"  
  
## Extract the DAC.  
$extractionunit = New-Object Microsoft.SqlServer.Management.Dac.DacExtractionUnit($srv, $dbname, $applicationname, $version)  
$extractionunit.Description = $description  
$extractionunit.Extract($dacpacPath)  
```  
  
## <a name="see-also"></a>Voir aussi  
 - [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md) 
 - [SqlPackage dans les pipelines de développement](../../tools/sqlpackage/sqlpackage-pipelines.md)