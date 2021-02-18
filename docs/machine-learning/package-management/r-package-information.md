---
title: Obtenir des informations sur les packages R
description: Découvrez comment en savoir plus sur les packages R installés dans SQL Server Machine Learning Services et SQL Server R Services.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/27/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 3669b46fa88e8d59fbb3eeb5ff91bc8bb9542ce7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352528"
---
# <a name="get-r-package-information"></a>Obtenir des informations sur les packages R

[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Cet article explique comment obtenir des informations sur les packages R installés sur [Machine Learning Services sur SQL Server](../sql-server-machine-learning-services.md) et sur [Clusters Big Data](../../big-data-cluster/machine-learning-services.md). Les exemples de scripts R vous montrent comment répertorier des informations de package telles que le chemin d’installation et la version.
::: moniker-end
::: moniker range="<=sql-server-2017"
Cet article explique comment obtenir des informations sur les packages R installés sur [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Les exemples de scripts R vous montrent comment répertorier des informations de package telles que le chemin d’installation et la version.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Cet article explique comment obtenir des informations sur les packages R installés sur [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview). Les exemples de scripts R vous montrent comment répertorier des informations de package telles que le chemin d’installation et la version.
::: moniker-end

## <a name="default-r-library-location"></a>Emplacement par défaut de la bibliothèque R

Lorsque vous installez Machine Learning avec SQL Server, une seule bibliothèque de packages est créée au niveau de l’instance pour chaque langage que vous installez. Sur Windows, la bibliothèque d’instances est un dossier sécurisé enregistré avec SQL Server.

Tout script qui s’exécute dans la base de données sur SQL Server doit charger les fonctions à partir de la bibliothèque d’instances. SQL Server ne peut pas accéder aux packages installés dans d’autres bibliothèques. Cela s’applique également aux clients distants : tout script R qui s’exécute dans le contexte de calcul du serveur peut uniquement utiliser des packages installés dans la bibliothèque d’instances.
Pour protéger les ressources du serveur, la bibliothèque d’instances par défaut ne peut être modifiée que par un administrateur de l’ordinateur.

::: moniker range="=sql-server-2016"
Le chemin d’accès par défaut aux éléments binaires pour R est le suivant :

`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\library`

Cela nécessite d’utiliser l’instance SQL par défaut, MSSQLSERVER. Si SQL Server est installé en tant qu’instance nommée définie par l’utilisateur, le nom donné est utilisé à la place.
::: moniker-end

::: moniker range="=sql-server-2017"
Le chemin d’accès par défaut aux éléments binaires pour R est le suivant :

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library`

Cela nécessite d’utiliser l’instance SQL par défaut, MSSQLSERVER. Si SQL Server est installé en tant qu’instance nommée définie par l’utilisateur, le nom donné est utilisé à la place.
::: moniker-end

::: moniker range=">=sql-server-ver15"
Le chemin d’accès par défaut aux éléments binaires pour R est le suivant :

`C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\library`

Cela nécessite d’utiliser l’instance SQL par défaut, MSSQLSERVER. Si SQL Server est installé en tant qu’instance nommée définie par l’utilisateur, le nom donné est utilisé à la place.
::: moniker-end

Exécutez l’instruction suivante pour déterminer la bibliothèque de packages R par défaut pour l’instance actuelle :

```sql
EXECUTE sp_execute_external_script  
  @language = N'R',
  @script = N'OutputDataSet <- data.frame(.libPaths());'
WITH RESULT SETS (([DefaultLibraryName] VARCHAR(MAX) NOT NULL));
GO
```

## <a name="default-microsoft-r-packages"></a>Packages Microsoft R par défaut

::: moniker range="=sql-server-2016"

Les packages Microsoft R suivants sont installés avec SQL Server R Services.

|. | Version | Description |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 8.0.3 | Utilisé pour les contextes de calcul distants, la diffusion en continu, l’exécution parallèle des fonctions rx pour l’importation et la transformation des données, la modélisation, la visualisation et l’analyse. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Utilisé pour inclure le script R dans les procédures stockées. |

::: moniker-end

::: moniker range="=sql-server-2017"

Les packages Microsoft R suivants sont installés avec SQL Server Machine Learning Services lorsque vous sélectionnez la fonctionnalité R au moment de l’installation.

|. | Version | Description |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 9.2 | Utilisé pour les contextes de calcul distants, la diffusion en continu, l’exécution parallèle des fonctions rx pour l’importation et la transformation des données, la modélisation, la visualisation et l’analyse. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Utilisé pour inclure le script R dans les procédures stockées. |
| [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package)| 1.4.0 | Ajoute des algorithmes d’apprentissage automatique dans R. | 
| [olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0.0 | Utilisé pour écrire des instructions MDX dans R. |

::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"

Les packages Microsoft R suivants sont installés avec SQL Server Machine Learning Services lorsque vous sélectionnez la fonctionnalité R au moment de l’installation.

|. | Version | Description |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 9.4.7 | Utilisé pour les contextes de calcul distants, la diffusion en continu, l’exécution parallèle des fonctions rx pour l’importation et la transformation des données, la modélisation, la visualisation et l’analyse. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Utilisé pour inclure le script R dans les procédures stockées. |
| [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package)| 9.4.7 | Ajoute des algorithmes d’apprentissage automatique dans R. |
| [olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0.0 | Utilisé pour écrire des instructions MDX dans R. |

::: moniker-end

### <a name="component-upgrades"></a>Mise à niveau des composants

Par défaut, les packages R sont actualisés par le biais des Service Packs et des mises à jour cumulatives. Les packages supplémentaires et les mises à niveau de version complètes des composants de base de R ne sont possibles que par le biais de mises à niveau de produit ou par liaison de la prise en charge de R avec Microsoft Machine Learning Server.

::: moniker range="=sql-server-2016"
En outre, vous pouvez ajouter des packages MicrosoftML et olapR à une instance SQL Server à l’aide d’une mise à niveau des composants.
::: moniker-end

Pour plus d’informations, consultez la section [Mettre à niveau les composants R et Python dans SQL Server](../install/upgrade-r-and-python.md).

## <a name="default-open-source-r-packages"></a>Packages R open source par défaut

La prise en charge de R comprend R open source. Cela vous permet d’appeler les fonctions R de base et d’installer des packages open source et tiers supplémentaires. La prise en charge du langage R comprend des fonctionnalités de base telles que **base**, **stats**, **utils**, et d’autres encore. L’installation de base de R comprend également un grand nombre d’exemples de jeux de données et d’outils R standard tels que **RGui** (un éditeur interactif léger) et **RTerm** (une invite de commandes R).

La distribution du contenu R open source inclus dans votre installation est [Microsoft R Open (MRO)](https://mran.microsoft.com/open). MRO ajoute de la valeur au contenu R de base en incluant des packages open source supplémentaires, tels que la bibliothèque [Intel Math Kernel Library](https://en.wikipedia.org/wiki/Math_Kernel_Library).

Pour plus d’informations sur la version de R incluse avec chaque version de SQL Server, consultez les [Versions de Python et de R](../sql-server-machine-learning-services.md#versions).

> [!IMPORTANT]
> Vous ne devez jamais remplacer manuellement la version de R installée par le programme d’installation de SQL Server par des versions plus récentes obtenues sur le Web. Les packages Microsoft R sont basés sur des versions spécifiques de R. La modification de votre installation pourrait les déstabiliser.

## <a name="list-all-installed-r-packages"></a>Répertorier tous les packages R installés

L’exemple suivant utilise la fonction R `installed.packages()` dans une procédure stockée [!INCLUDE[tsql](../../includes/tsql-md.md)] pour afficher une liste des packages R qui ont été installés dans la bibliothèque R_SERVICES de l’instance SQL actuelle. Ce script renvoie les champs de nom et de version du package dans le fichier DESCRIPTION.

```sql
EXECUTE sp_execute_external_script
  @language=N'R',
@script = N'str(OutputDataSet);
packagematrix <- installed.packages();
Name <- packagematrix[,1];
Version <- packagematrix[,3];
OutputDataSet <- data.frame(Name, Version);',
@input_data_1 = N'
  '
WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
```

Pour plus d’informations sur les champs facultatifs et par défaut du champ DESCRIPTION du package R, consultez [https://cran.r-project.org](https://cran.r-project.org/doc/manuals/R-exts.html#The-DESCRIPTION-file).

## <a name="find-a-single-r-package"></a>Rechercher un package R unique

Si vous avez installé un package R et que vous souhaitez vous assurer qu’il est disponible pour une instance SQL Server donnée, vous pouvez exécuter une procédure stockée pour charger le package et renvoyer des messages.

Par exemple, l’instruction suivante recherche et charge le package[glue](https://cran.r-project.org/web/packages/glue/), s’il est disponible.
Si le package est introuvable ou ne peut pas être chargé, vous obtenez une erreur.

```sql
EXECUTE sp_execute_external_script  
  @language =N'R',
  @script=N'
require("glue")
'
```

Pour plus d’informations sur le package, voir `packageDescription`.
L’instruction suivante renvoie des informations sur le package **MicrosoftML**.

```sql
EXECUTE sp_execute_external_script
  @language = N'R',
  @script = N'
print(packageDescription("MicrosoftML"))
'
```

## <a name="next-steps"></a>Étapes suivantes

::: moniker range="<=sql-server-2017"
+ [Installer des packages avec les outils R](install-r-packages-standard-tools.md)
::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Installer de nouveaux packages R avec sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end