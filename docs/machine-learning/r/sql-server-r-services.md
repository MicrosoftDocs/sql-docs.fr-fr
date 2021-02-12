---
title: Qu’est-ce que SQL Server 2016 R Services ?
titleSuffix: ''
description: R Services est une fonctionnalité de SQL Server 2016 qui permet d’exécuter des scripts R avec des données relationnelles. Vous pouvez utiliser des infrastructures et des packages open source, ainsi que les packages Microsoft R, pour l’analyse prédictive et l’apprentissage automatique. Les scripts sont exécutés dans la base de données sans déplacer de données en dehors de SQL Server ou sur le réseau. Cet article présente les notions de base de SQL Server R Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: =sql-server-2016
ms.openlocfilehash: 87c3afd4e8fc96bf753067c06de2a60741bbbadb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100019983"
---
# <a name="what-is-sql-server-2016-r-services"></a>Qu’est-ce que SQL Server 2016 R Services ?

[!INCLUDE[SQL Server 2016 only](../../includes/applies-to-version/sqlserver2016-only.md)]

R Services est une fonctionnalité de SQL Server 2016 qui permet d’exécuter des scripts R avec des données relationnelles. Vous pouvez utiliser des infrastructures et des packages open source, ainsi que les [packages Microsoft R](#packages), pour l’analyse prédictive et l’apprentissage automatique. Les scripts sont exécutés dans la base de données sans déplacer de données en dehors de SQL Server ou sur le réseau. Cet article présente les notions de base de SQL Server R Services.

> [!Note]
> R Services a été renommé [Machine Learning Services](../sql-server-machine-learning-services.md) dans SQL Server 2017 et versions ultérieures, et prend en charge à la fois Python et R.

## <a name="what-is-r-services"></a>Qu’est-ce que R Services ?

SQL Server R Services vous permet d’exécuter des scripts R dans la base de données. Vous pouvez vous en servir pour préparer et nettoyer des données, effectuer l’ingénierie des fonctionnalités et entraîner, évaluer et déployer des modèles Machine Learning dans une base de données. La fonctionnalité exécute vos scripts là où résident les données, ce qui vous évite d’avoir à transférer les données vers un autre serveur à travers le réseau.

Les distributions de base de R sont incluses dans R Services. Vous pouvez utiliser des infrastructures et des packages open source en plus des packages Microsoft [RevoScaleR](../r/ref-r-revoscaler.md), [MicrosoftML](../r/ref-r-microsoftml.md), [olapR]../r/ref-r-olapr.md) et [sqlrutils](../r/ref-r-sqlrutils.md) pour R.

R Services utilise une infrastructure d’extensibilité pour exécuter les scripts R dans SQL Server. Pour en savoir plus, consultez :

+ [Framework d’extensibilité](../concepts/extensibility-framework.md)
+ [Extension R](../concepts/extension-r.md)

## <a name="what-can-i-do-with-r-services"></a>Que puis-je faire avec R Services ?

Vous pouvez utiliser R Services pour générer et former des modèles Machine Learning et Deep Learning dans SQL Server. Vous pouvez également déployer des modèles existants sur R Services et utiliser des données relationnelles pour les prédictions.

Voici des exemples de types de prédictions pour lesquels vous pouvez utiliser SQL Server R Services :

|Type de prédiction| Exemple|
|-|-|
|Classification/catégorisation|Diviser automatiquement les commentaires des clients en catégories positives et négatives|
|Régression/prédiction de valeurs continues|Prédire le prix de maisons en fonction de la taille et de l’emplacement|
|Détection des anomalies|Détecter les transactions bancaires frauduleuses |
|Recommandations|Suggérer des produits que les acheteurs en ligne peuvent souhaiter acheter, en fonction de leurs achats précédents|

### <a name="how-to-execute-r-scripts"></a>Comment exécuter les scripts R

Il existe deux façons d’exécuter des scripts R dans R Services :

+ La méthode la plus courante consiste à utiliser la procédure stockée T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

+ Vous pouvez également utiliser votre client R préféré et écrire des scripts qui poussent l’exécution (appelée *contexte de calcul distant*) vers un serveur SQL Server distant. Pour plus d’informations, découvrez comment [configurer un développement R client pour la science des données](../r/set-up-a-data-science-client.md).

<a name="version"></a>

## <a name="r-versions"></a>Versions de R

La liste suivante répertorie les versions du runtime R incluses dans SQL Server 2016 R Services.

Version de SQL Server | Versions du runtime R par défaut |
|-|-|
| SQL Server 2016 RTM - SP2 CU13 | 3.2.2 |
| SQL Server 2016 SP2 CU14 et versions ultérieures | 3.2.2 et 3.5.2 |

La mise à jour cumulative (CU) 14 pour SQL Server 2016 Service Pack (SP) 2 et les versions ultérieures incluent des runtimes R plus récents. Pour plus d’informations, consultez [Modifier la version du runtime de langage par défaut](../install/change-default-language-runtime-version.md).

Si vous avez une autre version de R ou pour exécuter Python, utilisez [Machine Learning Services pour SQL Server 2017 ou une version ultérieure](../sql-server-machine-learning-services.md).

<a name="packages"></a>

## <a name="r-packages"></a>Packages R

Vous pouvez utiliser des frameworks et des packages open source, en plus des packages d’entreprise Microsoft. Les packages R open source les plus courants sont pré-installés dans R Services. Les packages R suivants de Microsoft sont également inclus :

| Package | Description |
|-|-|
| [RevoScaleR](../r/ref-r-revoscaler.md) | Package principal pour créer du code Python scalable : transformations et manipulations de données, totalisation statistique, visualisation et nombreuses formes de modélisation. De plus, les fonctions de ce package distribuent automatiquement les charges de travail entre les cœurs disponibles pour un traitement parallèle. |
| [MicrosoftML (R)](../r/ref-r-microsoftml.md) | Ajoute des algorithmes de machine learning pour créer des modèles personnalisés pour l’analyse des textes, l’analyse des images et l’analyse des sentiments. |
| [olapR](../r/ref-r-olapr.md) | Fonctions R utilisées pour les requêtes MDX sur un cube OLAP SQL Server Analysis Services. |
| [sqlrutils](../r/ref-r-sqlrutils.md) | Mécanisme permettant d’utiliser les scripts R dans une procédure stockée T-SQL, d’inscrire cette procédure auprès d’une base de données et d’exécuter la procédure stockée à partir d’un [environnement de développement R](../r/set-up-a-data-science-client.md). |
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open (MRO) est la distribution améliorée de R fournie par Microsoft. Il s’agit d’une plate-forme open source complète pour l’analyse statistique et la science des données. Basée sur R et entièrement compatible avec ce langage, elle comprend des fonctionnalités supplémentaires visant à améliorer les performances et la reproductibilité. |

## <a name="how-do-i-get-started-with-rservices"></a>Comment commencer à utiliser R Services ?

1. [Installer SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)

1. Configurez vos outils de développement. Vous pouvez utiliser :

    + [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) ou [SQL Server Management Studio (SSMS)](../../ssms/sql-server-management-studio-ssms.md) pour utiliser T-SQL et la procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) afin d’exécuter votre script R.
    + R sur votre propre ordinateur portable ou station de travail de développement pour exécuter des scripts. Vous pouvez extraire les données localement ou envoyer l’exécution à distance vers SQL Server avec [RevoScaleR](../r/ref-r-revoscaler.md). Pour plus d’informations, découvrez comment [configurer un développement R client pour la science des données](../r/set-up-a-data-science-client.md).

1. Écrire votre premier script R

    + Démarrage rapide : [Créer et exécuter des scripts R simples dans SQL Server](../tutorials/quickstart-r-create-script.md)
    + Démarrage rapide : [Créer et entraîner un modèle prédictif dans R](../tutorials/quickstart-r-train-score-model.md)
    + Tutoriel : [Utiliser R dans T-SQL](../tutorials/r-taxi-classification-introduction.md). Explorez les données, effectuez l’ingénierie des caractéristiques, entraînez et déployez des modèles, et faites des prédictions (série en cinq parties)
    + Tutoriel : [Utilisez R Services dans les outils R](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md) : Explorez les données, créez des graphes et des tracés, effectuez l’ingénierie des caractéristiques, entraînez et déployez des modèles, et faites des prédictions (série en six parties)

## <a name="next-steps"></a>Étapes suivantes

+ [Installer SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)
+ [Configurer un client de science des données pour le développement R](../r/set-up-a-data-science-client.md)