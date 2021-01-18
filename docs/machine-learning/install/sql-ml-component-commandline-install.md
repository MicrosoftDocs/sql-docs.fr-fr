---
title: Installer à partir d’une invite de commandes
description: Exécutez l’installation de SQL Server à partir d’une ligne de commande pour ajouter Machine Learning Services avec Python et R à une instance du moteur de base de données SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/07/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 9b0bf3444ab09cbbbb4aa8eca225dfa9964898c3
ms.sourcegitcommit: d681796e8c012eca2d9629d3b816749e9f50f868
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98005304"
---
# <a name="install-sql-server-machine-learning-services-with-r-and-python-from-the-command-line"></a>Installer SQL Server Machine Learning Services avec R et Python à partir de la ligne de commande
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Cet article fournit des instructions pour l’installation de [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) avec Python et R à partir d’une ligne de commande.

Vous pouvez spécifier une interaction en mode silencieux, de base ou complète avec l’interface utilisateur du programme d’installation. Cet article vient compléter l’article [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md). Il couvre les paramètres propres aux composants de machine learning R et Python.

## <a name="pre-install-checklist"></a>Liste de contrôle avant l’installation

+ Exécutez les commandes dans une invite de commandes avec élévation de privilèges. 

+ Une instance du moteur de base de données est nécessaire pour les installations en base de données. Vous ne pouvez pas installer uniquement les fonctionnalités R ou Python, même si vous pouvez les [ajouter de façon incrémentielle à une instance existante](#add-existing). Si vous souhaitez installer uniquement les fonctionnalités R ou Python sans le moteur de base de données, installez le [serveur autonome](#shared-feature).

+ N’effectuez pas l’installation sur un cluster de basculement. Ce mécanisme de sécurité servant à isoler les processus R et Python n’est pas compatible avec un environnement de cluster de basculement Windows Server.

+ N’effectuez pas l’installation sur un contrôleur de domaine. La partie du programme d’installation dédiée à Machine Learning Services échouerait.

+ Évitez d’installer des instances autonomes et en base de données sur le même ordinateur. Un serveur autonome tentera d’accéder aux mêmes ressources, ce qui réduira les performances des deux installations.

## <a name="command-line-arguments"></a>Arguments de ligne de commande

L’argument **/FEATURES** est obligatoire. Vous devez également indiquer que vous acceptez les termes du contrat de licence. 

En cas d’installation à partir de l’invite de commandes, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le mode silencieux complet à l’aide du paramètre **/Q** ou le mode silencieux simple à l’aide du paramètre **/QS**. Le commutateur **/QS** affiche seulement la progression ; il n'accepte pas d'entrée et n'affiche aucun message d'erreur. Le paramètre **/QS** est pris en charge uniquement lorsque **/Action=install** est spécifié.

::: moniker range="=sql-server-2016"
| Arguments | Description |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Installe la version en base de données : SQL Server R Services (en base de données).  |
| /FEATURES = SQL_SHARED_MR | Installe la fonctionnalité R pour la version autonome : SQL Server R Server (autonome). Un serveur autonome est une « fonctionnalité partagée » non liée à une instance du moteur de base de données.|
| /IACCEPTROPENLICENSETERMS  | Indique que vous avez accepté les termes du contrat de licence pour l’utilisation des composants R open source. |
| /IACCEPTPYTHONLICENSETERMS | Indique que vous avez accepté les termes du contrat de licence pour l’utilisation des composants Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Indique que vous avez accepté les termes du contrat de licence pour l’utilisation de SQL Server.|
| /MRCACHEDIRECTORY | Pour une installation hors connexion, spécifie le dossier contenant les fichiers CAB des composants R. |
::: moniker-end

::: moniker range="=sql-server-2017"
| Arguments | Description |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Installe la version en base de données : SQL Server Machine Learning Services (en base de données).  |
| /FEATURES = SQL_INST_MR | Associez cet argument à AdvancedAnalytics. Installe la fonctionnalité R (en base de données), y compris Microsoft R Open et les packages R propriétaires. |
| /FEATURES = SQL_INST_MPY | Associez cet argument à AdvancedAnalytics. Installe la fonctionnalité Python (en base de données), y compris Anaconda et les packages Python propriétaires. |
| /FEATURES = SQL_SHARED_MR | Installe la fonctionnalité R pour la version autonome : SQL Server Machine Learning Server (autonome). Un serveur autonome est une « fonctionnalité partagée » non liée à une instance du moteur de base de données.|
| /FEATURES = SQL_SHARED_MPY | Installe la fonctionnalité Python pour la version autonome : SQL Server Machine Learning Server (autonome). Un serveur autonome est une « fonctionnalité partagée » non liée à une instance du moteur de base de données.|
| /IACCEPTROPENLICENSETERMS  | Indique que vous avez accepté les termes du contrat de licence pour l’utilisation des composants R open source. |
| /IACCEPTPYTHONLICENSETERMS | Indique que vous avez accepté les termes du contrat de licence pour l’utilisation des composants Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Indique que vous avez accepté les termes du contrat de licence pour l’utilisation de SQL Server.|
| /MRCACHEDIRECTORY | Pour une installation hors connexion, spécifie le dossier contenant les fichiers CAB des composants R. |
| MPYCACHEDIRECTORY | Réservé pour un usage futur. Utilisez %TEMP% pour stocker les fichiers .CAB des composants Python pour une installation sur un ordinateur ne disposant pas de connexion Internet. |
::: moniker-end

::: moniker range=">=sql-server-ver15"
| Arguments | Description |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Installe la version en base de données : SQL Server Machine Learning Services (en base de données).  |
| /FEATURES = SQL_INST_MR | Associez cet argument à AdvancedAnalytics. Installe la fonctionnalité R (en base de données), y compris Microsoft R Open et les packages R propriétaires. |
| /FEATURES = SQL_INST_MPY | Associez cet argument à AdvancedAnalytics. Installe la fonctionnalité Python (en base de données), y compris Anaconda et les packages Python propriétaires. |
| /FEATURES = SQL_INST_MJAVA | Associez cet argument à AdvancedAnalytics. Installe la fonctionnalité Java (en base de données), y compris Open JRE. S’applique à l’[extension de langage Java SQL Server](../../language-extensions/install/windows-java.md).|
| /FEATURES = SQL_SHARED_MR | Installe la fonctionnalité R pour la version autonome : SQL Server Machine Learning Server (autonome). Un serveur autonome est une « fonctionnalité partagée » non liée à une instance du moteur de base de données.|
| /FEATURES = SQL_SHARED_MPY | Installe la fonctionnalité Python pour la version autonome : SQL Server Machine Learning Server (autonome). Un serveur autonome est une « fonctionnalité partagée » non liée à une instance du moteur de base de données.|
| /IACCEPTROPENLICENSETERMS  | Indique que vous avez accepté les termes du contrat de licence pour l’utilisation des composants R open source. |
| /IACCEPTPYTHONLICENSETERMS | Indique que vous avez accepté les termes du contrat de licence pour l’utilisation des composants Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Indique que vous avez accepté les termes du contrat de licence pour l’utilisation de SQL Server.|
| /MRCACHEDIRECTORY | Pour une installation hors connexion, spécifie le dossier contenant les fichiers CAB des composants R. |
| MPYCACHEDIRECTORY | Réservé pour un usage futur. Utilisez %TEMP% pour stocker les fichiers .CAB des composants Python pour une installation sur un ordinateur ne disposant pas de connexion Internet. |
::: moniker-end

## <a name="in-database-instance-installations"></a><a name="indb"></a> Installation d’instances en base de données

L’analytique en base de données est disponible pour les instances du moteur de base de données, nécessaires à l’ajout de la fonctionnalité **AdvancedAnalytics** à votre installation. Vous pouvez installer une instance du moteur de base de données avec l’analytique avancée ou l’[ajouter à une instance existante](#add-existing). 

Pour voir les informations de progression sans les invites interactives à l’écran, utilisez l’argument /qs.

> [!IMPORTANT]
> Il reste deux étapes de configuration supplémentaires après l’installation. L’intégration n’est pas terminée tant que ces tâches n’ont pas été effectuées. Pour obtenir des instructions, consultez la section [Tâches consécutives à l’installation](#post-install).

::: moniker range=">=sql-server-2017"
### <a name="sql-server-machine-learning-services-database-engine-advanced-analytics-with-python-and-r"></a>SQL Server Machine Learning Services : moteur de base de données, analytique avancée avec Python et R

Pour une installation simultanée de l’instance du moteur de base de données, indiquez le nom de l’instance et le compte de connexion administrateur (Windows). Spécifiez les fonctionnalités pour l’installation des composants de base et de langage et indiquez que vous acceptez tous les termes des contrats de licence.

```cmd
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Il s’agit de la même commande, mais avec une connexion SQL Server sur un moteur de base de données utilisant une authentification mixte.

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SECURITYMODE=SQL /SAPWD="%password%" /SQLSYSADMINACCOUNTS="<sql-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Cet exemple en Python uniquement montre que vous pouvez ajouter un langage en omettant une fonctionnalité.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTPYTHONLICENSETERMS
```
::: moniker-end

::: moniker range="=sql-server-2016"
### <a name="sql-server-r-services-database-engine-and-advanced-analytics-with-r"></a>SQL Server R Services : moteur de base de données et analytique avancée avec R

Pour une installation simultanée de l’instance du moteur de base de données, indiquez le nom de l’instance et le compte de connexion administrateur (Windows). Spécifiez les fonctionnalités pour l’installation des composants de base et de langage et indiquez que vous acceptez tous les termes des contrats de licence.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS 
```
::: moniker-end

## <a name="post-install-configuration-required"></a><a name="post-install"></a> Configuration consécutive à l’installation (obligatoire)

S’applique uniquement aux installations en base de données.

Une fois l’installation terminée, vous disposez d’une instance du moteur de base de données avec R et Python, des packages Microsoft R et Python, de Microsoft R Open, d’Anaconda, des outils, des exemples et des scripts qui font partie de la distribution. 

Vous devez encore effectuer deux étapes pour terminer l’installation :


::: moniker range=">=sql-server-2017"
1. Redémarrez le service moteur de base de données.

1. SQL Server Machine Learning Services : activez les scripts externes pour pouvoir utiliser la fonctionnalité. Suivez à présent les instructions de l’article [Installer SQL Server Machine Learning Services (en base de données)](sql-machine-learning-services-windows-install.md). 
::: moniker-end

::: moniker range="=sql-server-2016"
1. Redémarrez le service moteur de base de données.

1. SQL Server R Services : activez les scripts externes pour pouvoir utiliser la fonctionnalité. Suivez à présent les instructions de l’article [Installer SQL Server R Services (en base de données)](sql-r-services-windows-install.md). 
::: moniker-end

## <a name="add-advanced-analytics-to-an-existing-database-engine-instance"></a><a name="add-existing"></a> Ajouter l’analytique avancée à une instance du moteur de base de données existante

Quand vous ajoutez l’analytique avancée en base de données à une instance du moteur de base de données existante, indiquez le nom de l’instance. Par exemple, si vous avez déjà installé un moteur de base de données SQL Server 2017 (ou version ultérieure) et Python, vous pouvez utiliser cette commande pour ajouter R.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQL_INST_MR /INSTANCENAME=MSSQLSERVER 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTROPENLICENSETERMS
```

## <a name="silent-install"></a><a name="silent"></a> Installation sans assistance

Une installation sans assistance supprime la vérification des emplacements des fichiers .cab. Vous devez donc spécifier l’emplacement où les fichiers .cab doivent être décompressés. Pour Python, les fichiers CAB doivent se trouver dans un dossier %TEMP*. Pour R, vous pouvez définir le chemin du dossier sous le répertoire temporaire.
 
```cmd  
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS 
/MRCACHEDIRECTORY=%temp% 
```

## <a name="standalone-server-installations"></a><a name="shared-feature"></a> Installations de serveur autonome

Un serveur autonome est une « fonctionnalité partagée » non liée à une instance du moteur de base de données. Les exemples suivants montrent une syntaxe valide pour l’installation du serveur autonome.

::: moniker range=">=sql-server-2017"
SQL Server Machine Learning Server prend en charge Python et R sur un serveur autonome :

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR,SQL_SHARED_MPY  
/IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end
::: moniker range="=sql-server-2016"
SQL Server R Server prend en charge R uniquement :

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR 
/IACCEPTROPENLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end

Une fois l’installation terminée, vous disposez d’un serveur, des packages Microsoft, des distributions R et Python open source, des outils, des exemples et des scripts qui font partie de la distribution. 

Pour ouvrir une fenêtre de console R, accédez au dossier `\Program files\Microsoft SQL Server\150 (or 140/130)\R_SERVER\bin\x64` et double-cliquez sur le fichier **RGui.exe**. Vous débutez avec R ? Essayez ce tutoriel : [Commandes R et fonctions RevoScaleR de base : 25 exemples courants](/machine-learning-server/r/tutorial-r-to-revoscaler).

Pour ouvrir une commande Python, accédez au dossier `\Program files\Microsoft SQL Server\150 (or 140)\PYTHON_SERVER\bin\x64` et double-cliquez sur le fichier **python.exe**.

## <a name="next-steps"></a>Étapes suivantes

Les développeurs Python peuvent apprendre à utiliser Python avec SQL Server en effectuant les didacticiels suivants :

+ [Tutoriel Python : Prédire la location de ski avec régression linéaire dans SQL Server Machine Learning Services](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Tutoriel Python : Catégoriser des clients à l’aide de k-moyennes avec SQL Server Machine Learning Services](../tutorials/python-clustering-model.md)

Les développeurs peuvent démarrer avec quelques exemples simples et découvrir les principes de base du fonctionnement de R avec SQL Server. Pour l’étape suivante, consultez les liens suivants :

+ [Démarrage rapide : Exécuter R dans T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Tutoriel : Analytique dans la base de données pour les développeurs R](../tutorials/r-taxi-classification-introduction.md)
