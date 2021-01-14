---
title: Qu’est-ce que SQL Server Machine Learning Services (Python et R) ?
titleSuffix: ''
description: Machine Learning Services est une fonctionnalité de SQL Server qui permet d’exécuter des scripts Python et R avec des données relationnelles. Vous pouvez utiliser des frameworks et des packages open source, ainsi que les packages Microsoft Python et R, pour l’analyse prédictive et le machine learning. Les scripts sont exécutés dans la base de données sans déplacer de données en dehors de SQL Server ou sur le réseau. Cet article présente les notions de base de SQL Server Machine Learning Services vous indique comment bien démarrer.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/10/2020
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 98e9dd928f2639bfd1df3e6782bcc9aaed60f31e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102862"
---
# <a name="what-is-sql-server-machine-learning-services-with-python-and-r"></a>Qu’est-ce que SQL Server Machine Learning Services avec Python et R ?
[!INCLUDE [SQL Server 2017 SQL MI](../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Machine Learning Services est une fonctionnalité de SQL Server qui permet d’exécuter des scripts Python et R avec des données relationnelles. Vous pouvez utiliser des frameworks et des packages open source, ainsi que les [packages Microsoft Python et R](#packages), pour l’analyse prédictive et le machine learning. Les scripts sont exécutés dans la base de données sans déplacer de données en dehors de SQL Server ou sur le réseau. Cet article présente les notions de base de SQL Server Machine Learning Services vous indique comment bien démarrer.

Pour plus l’apprentissage automatique sur d’autres plateformes SQL, consultez la [documentation sur l’apprentissage automatique SQL](index.yml).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Pour exécuter Java dans SQL Server, consultez la [documentation de l’extension de langage Java](../language-extensions/java-overview.md).
::: moniker-end

## <a name="execute-python-and-r-scripts-in-sql-server"></a>Exécuter des scripts Python et R dans SQL Server

SQL Server Machine Learning Services vous permet d’exécuter des scripts Python et R en base de données. Vous pouvez vous en servir pour préparer et nettoyer des données, effectuer l’ingénierie des fonctionnalités et entraîner, évaluer et déployer des modèles Machine Learning dans une base de données. La fonctionnalité exécute vos scripts là où résident les données, ce qui vous évite d’avoir à transférer les données vers un autre serveur à travers le réseau.

Vous pouvez exécuter des scripts Python et R sur une instance de SQL Server avec la procédure stockée [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Les distributions de base de Python et de R sont incluses dans Machine Learning Services. Vous pouvez installer et utiliser des frameworks et des packages open source comme PyTorch, TensorFlow et scikit-learn, en plus des packages Microsoft.

Machine Learning Services utilise un framework d’extensibilité pour exécuter les scripts Python et R dans SQL Server. Pour en savoir plus, consultez :

+ [Framework d’extensibilité](concepts/extensibility-framework.md)
+ [Extension Python](concepts/extension-python.md)
+ [Extension R](concepts/extension-r.md)

## <a name="get-started-with-machine-learning-services"></a>Prise en main de Machine Learning Services

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
1. [Installez SQL Server Machine Learning Services sur Windows](install/sql-machine-learning-services-windows-install.md) ou [sur Linux](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json). Vous pouvez également utiliser [Machine Learning Services sur Clusters Big Data](../big-data-cluster/machine-learning-services.md) et [Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).

1. Configurez vos outils de développement. Vous pouvez utiliser [Exécuter des scripts Python et R dans des notebooks Azure Data Studio](install/sql-machine-learning-azure-data-studio.md). Vous pouvez également exécuter T-SQL dans [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

1. Écrivez votre premier script Python ou R.

   + [Tutoriels Python pour l’apprentissage automatique SQL](tutorials/python-tutorials.md)
   + [Tutoriels R pour l’apprentissage automatique SQL](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ Écrivez votre premier script Python ou R.

   + [Tutoriels Python pour l’apprentissage automatique SQL](tutorials/python-tutorials.md)
   + [Tutoriels R pour l’apprentissage automatique SQL](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=sql-server-2017"
1. [Installez SQL Server Machine Learning Services sur Windows](install/sql-machine-learning-services-windows-install.md).

1. Configurez vos outils de développement. Vous pouvez utiliser [Exécuter des scripts Python et R dans des notebooks Azure Data Studio](install/sql-machine-learning-azure-data-studio.md). Vous pouvez également utiliser T-SQL dans [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

1. Écrivez votre premier script Python ou R.

   + [Tutoriels Python pour l’apprentissage automatique SQL](tutorials/python-tutorials.md)
   + [Tutoriels R pour l’apprentissage automatique SQL](tutorials/r-tutorials.md)
::: moniker-end

<a name="versions"></a>

## <a name="python-and-r-versions"></a>Versions de Python et de R

La liste suivante répertorie les versions de Python et de R incluses dans Machine Learning Services.

| Version de SQL Server | Version Python | Version de R |
|-|-|-|
| SQL Server 2017 | 3.5.2 | 3.3.3 |
| SQL Server 2019 | 3.7.3 | 3.5.2 |

Pour savoir quelle version de R est fournie dans SQL Server 2016, consultez la [section sur les versions de R dans Qu’est-ce que R Services ?](r/sql-server-r-services.md?view=sql-server-2016&preserve-view=true#version)

<a name="packages"></a>

## <a name="python-and-r-packages"></a>Packages Python et R

Vous pouvez utiliser des frameworks et des packages open source, en plus des packages d’entreprise Microsoft. Les packages Python et R open source les plus courants sont préinstallés dans Machine Learning Services. Les packages Python et R suivants de Microsoft sont également inclus :

| Langage | Package | Description |
|-|-|-|
| Python | [revoscalepy](python/ref-py-revoscalepy.md) | Package principal pour créer du code Python scalable : transformations et manipulations de données, totalisation statistique, visualisation et nombreuses formes de modélisation. De plus, les fonctions de ce package distribuent automatiquement les charges de travail entre les cœurs disponibles pour un traitement parallèle. |
| Python | [microsoftml](python/ref-py-microsoftml.md) | Ajoute des algorithmes de machine learning pour créer des modèles personnalisés pour l’analyse des textes, l’analyse des images et l’analyse des sentiments. | 
| R | [RevoScaleR](r/ref-r-revoscaler.md) | Package principal pour créer du code Python scalable : transformations et manipulations de données, totalisation statistique, visualisation et nombreuses formes de modélisation. De plus, les fonctions de ce package distribuent automatiquement les charges de travail entre les cœurs disponibles pour un traitement parallèle. |
| R | [MicrosoftML (R)](r/ref-r-microsoftml.md) | Ajoute des algorithmes de machine learning pour créer des modèles personnalisés pour l’analyse des textes, l’analyse des images et l’analyse des sentiments. |
| R | [olapR](r/ref-r-olapr.md) | Fonctions R utilisées pour les requêtes MDX sur un cube OLAP SQL Server Analysis Services. |
| R | [sqlrutils](r/ref-r-sqlrutils.md) | Mécanisme permettant d’utiliser les scripts R dans une procédure stockée T-SQL, d’inscrire cette procédure auprès d’une base de données et d’exécuter la procédure stockée à partir d’un [environnement de développement R](r/set-up-a-data-science-client.md). |
| R | [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open (MRO) est la distribution améliorée de R fournie par Microsoft. Il s’agit d’une plate-forme open source complète pour l’analyse statistique et la science des données. Basée sur R et entièrement compatible avec ce langage, elle comprend des fonctionnalités supplémentaires visant à améliorer les performances et la reproductibilité. |

Pour plus d’informations sur les packages installés avec Machine Learning Services et sur l’installation d’autres packages, consultez :

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ [Obtenir des informations sur les packages Python](package-management/python-package-information.md)
+ [Installer des packages Python avec sqlmlutils](package-management/install-additional-python-packages-on-sql-server.md)
+ [Obtenir des informations sur les packages R](package-management/r-package-information.md)
+ [Installer de nouveaux packages R avec sqlmlutils](package-management/install-additional-r-packages-on-sql-server.md)
::: moniker-end
::: moniker range="=sql-server-2017"
+ [Obtenir des informations sur les packages Python](package-management/python-package-information.md)
+ [Installer des packages avec des outils Python sur SQL Server](package-management/install-python-packages-standard-tools.md)
+ [Obtenir des informations sur les packages R](package-management/r-package-information.md)
+ [Utiliser T-SQL (CREATE EXTERNAL LIBRARY) pour installer des packages R sur SQL Server](package-management/install-r-packages-with-tsql.md).
::: moniker-end

## <a name="next-steps"></a>Étapes suivantes

+ [Installation de SQL Server Machine Learning Services sur Windows](install/sql-machine-learning-services-windows-install.md) ou [sur Linux](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json)
+ [Tutoriels Python pour l’apprentissage automatique SQL](tutorials/python-tutorials.md)
+ [Tutoriels R pour l’apprentissage automatique SQL](tutorials/r-tutorials.md)