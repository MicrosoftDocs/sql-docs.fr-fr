---
title: Déployer du code R dans les procédures stockées
description: Incorporez le code de langage R dans une procédure stockée SQL Server pour le rendre accessible à toute application cliente ayant accès à une base de données SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: a959bf5c29a125aefb390bef1428e6a35745e665
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353751"
---
# <a name="operationalize-r-code-using-stored-procedures-in-sql-server-machine-learning-services"></a>Faire fonctionner votre code R à l’aide des procédures stockées dans SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Lorsque vous utilisez les fonctionnalités R et Python dans SQL Server Machine Learning Services, l’approche la plus courante pour déplacer des solutions vers un environnement de production consiste à incorporer du code dans des procédures stockées. Cet article résume les points clés que le développeur SQL peut prendre en compte lorsqu’il fait fonctionner du code R à l’aide de SQL Server.

## <a name="deploy-production-ready-script-using-t-sql-and-stored-procedures"></a>Déployer un script prêt pour la production à l’aide de T-SQL et de procédures stockées

Traditionnellement, l’intégration de solutions de science des données impliquait un recodage extensif pour la prise en charge des performances et de l’intégration. SQL Server Machine Learning Services simplifie cette tâche, car le code R et Python peuvent être exécutés dans SQL Server et appelés à l’aide de procédures stockées. Pour plus d’informations sur le mécanisme d’incorporation de code dans des procédures stockées, consultez :

+ [Créer et exécuter des scripts R simples dans SQL Server](../tutorials/quickstart-r-create-script.md)
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)

Vous trouverez un exemple plus complet de déploiement de code R en production à l’aide des procédures stockées dans [Tutoriel R : Prédire les tarifs des taxis de New York avec classification binaire](../tutorials/r-taxi-classification-introduction.md).

## <a name="guidelines-for-optimizing-r-code-for-sql"></a>Recommandations pour l’optimisation du code R pour SQL

La conversion de votre code R dans SQL est plus facile si certaines optimisations sont effectuées au préalable dans le code R ou Python. Il s’agit notamment d’éviter les types de données qui provoquent des problèmes, d’éviter les conversions de données inutiles et de réécrire le code R comme un appel de fonction unique pouvant être facilement paramétré. Pour plus d'informations, consultez les pages suivantes :

+ [Bibliothèques et types de données R](r-libraries-and-data-types.md)
+ [Utiliser des fonctions d’assistance sqlrutils](ref-r-sqlrutils.md)

## <a name="integrate-r-and-python-with-applications"></a>Intégrer R et Python aux applications

Étant donné que vous pouvez exécuter R ou Python à partir d’une procédure stockée, vous pouvez exécuter des scripts à partir de n’importe quelle application qui peut envoyer une instruction T-SQL et gérer les résultats. Par exemple, vous pouvez reformer un modèle selon une planification à l’aide de la [tâche Execute T-SQL](../../integration-services/control-flow/execute-t-sql-statement-task.md) dans Integration Services ou à l’aide d’un autre planificateur de travaux qui peut exécuter une procédure stockée.

Le scoring est une tâche importante qui peut facilement être automatisée ou démarrée à partir d’applications externes. Vous formez le modèle au préalable, en utilisant R ou Python ou une procédure stockée, et [enregistrer le modèle au format binaire](../tutorials/walkthrough-build-and-save-the-model.md) dans une table. Ensuite, le modèle peut être chargé dans une variable dans le cadre d’un appel de procédure stockée, à l’aide de l’une de ces options pour le scoring à partir de T-SQL :

+ [Scoring en temps réel, optimisé pour les lots de petite taille
+ Scoring sur une ligne, pour appeler à partir d’une application
+ [Scoring natif](../predictions/native-scoring-predict-transact-sql.md), pour la prédiction de lot rapide à partir de SQL Server sans appeler R

Le tutoriel suivant fournit un exemple de scoring à l’aide d’une procédure stockée à la fois dans les modes par lot et sur une ligne :

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ [Procédure pas à pas de la science des données de bout en bout pour R dans SQL Server](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ [Didacticiel R : Prédire les tarifs des taxis de New York avec classification binaire](../tutorials/r-taxi-classification-introduction.md)
::: moniker-end

## <a name="boost-performance-and-scale"></a>Stimuler les performances et mettre à l'échelle

Si le langage R open source a des limites bien connues concernant les jeux de données volumineux, les [API du package RevoScaleR](ref-r-revoscaler.md) fournies avec SQL Server Machine Learning Service peuvent prendre en charge des jeux de données plus volumineux et tirer parti des calculs multithreads, multicœurs et multiprocessus au sein de la base de données.

Si votre solution R utilise des agrégations complexes ou des jeux de données volumineux, vous pouvez tirer parti des index columnstore et des agrégations en mémoire très efficaces de SQL Server, et utiliser le code R pour les calculs statistiques et les calculs de score.

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"

## <a name="adapt-r-code-for-other-platforms-or-compute-contexts"></a>Adapter le code R pour d’autres plateformes ou contextes de calcul

Le même code R que vous exécutez sur des données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être utilisé sur d’autres sources de données, comme Spark sur HDFS, lorsque vous utilisez l’[option de serveur autonome](../install/sql-machine-learning-standalone-windows-install.md) dans l’installation de SQL Server ou lorsque vous installez le produit non-SQL, Microsoft Machine Learning Server (anciennement **Microsoft R Server**) :

+ [Machine Learning Server - Documentation](/r-server/)

::: moniker-end
