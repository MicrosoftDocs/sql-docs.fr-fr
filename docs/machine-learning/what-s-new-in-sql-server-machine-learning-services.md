---
title: Nouveautés de SQL Server Machine Learning Services
titleSuffix: ''
description: Annonces concernant les nouvelles fonctionnalités pour chaque version de SQL Server Machine Learning Services et de SQL Server 2016 R Services.
ms.date: 11/17/2020
ms.topic: overview
author: dphansen
ms.author: davidph
ms.custom: sqlseattle
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 31ecd76efc4796aebe2a3ee40a8bf40891d38a83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272930"
---
# <a name="whats-new-in-sql-server-machine-learning-services"></a>Nouveautés de SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and later](../includes/applies-to-version/sqlserver2016.md)]

Cet article décrit les nouvelles fonctionnalités de chaque version de [SQL Server Machine Learning Services](sql-server-machine-learning-services.md). Des fonctionnalités de machine learning sont ajoutées à SQL Server dans chaque version, car nous continuons à développer, étendre et approfondir l’intégration entre la plateforme de données, l’analytique avancée et la science des données. 

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="new-in-sql-server-2019"></a>Nouveautés de SQL Server 2019

Cette version ajoute les fonctionnalités les plus demandées pour les opérations de machine learning Python et R dans SQL Server. Pour plus d’informations sur toutes les fonctionnalités de cette version, consultez [Nouveautés de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md) et [Notes de publication pour SQL Server 2019](../sql-server/sql-server-version-15-release-notes.md).

> [!NOTE]
> Pour obtenir la documentation sur les nouveautés Java dans SQL Server 2019, consultez les [nouveautés des extensions de langage SQL Server](../language-extensions/language-extensions-whats-new.md).

Vous trouverez ci-dessous les nouvelles fonctionnalités de SQL Server Machine Learning Services, disponibles à la fois sur **Windows** et sur **Linux** :

- La prise en charge de la plateforme Linux a été ajoutée dans Machine Learning Services pour Python et R. Commencez par [Installation de SQL Server Machine Learning Services sur Linux](../linux/sql-server-linux-setup-machine-learning.md).
- [Connexion de bouclage à SQL Server à partir d’un script Python ou R](connect/loopback-connection.md). 
- [CREATE EXTERNAL LIBRARY (Transact-SQL)](../t-sql/statements/create-external-library-transact-sql.md) pour Python et R.
- [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) introduit deux nouveaux paramètres qui vous permettent de générer facilement plusieurs modèles à partir de données partitionnées. Pour plus d’informations, consultez le tutoriel [Créer des modèles basés sur des partitions en R](tutorials/r-tutorial-create-models-per-partition.md).
- La prise en charge des clusters de basculement est disponible pour le service SQL Server Launchpad, à condition qu’il soit démarré sur tous les nœuds. Pour plus d’informations, consultez [Installation d’un cluster de basculement SQL Server](../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md).
- Modification des mécanismes d’isolation pour Machine Learning Services. Pour plus d’informations, consultez [SQL Server 2019 sur Windows : Modifications de l’isolation dans Machine Learning Services](install/sql-server-machine-learning-services-2019.md).

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="new-in-sql-server-2017"></a>Nouveautés de SQL Server 2017

Cette version ajoute [la prise en charge de Python et les algorithmes de machine learning de pointe](https://cloudblogs.microsoft.com/sqlserver/2017/04/19/python-in-sql-server-2017-enhanced-in-database-machine-learning/). Renommé pour refléter la nouvelle étendue, SQL Server 2017 marque l’introduction de [SQL Server Machine Learning Services (dans la base de données)](sql-server-machine-learning-services.md) avec prise en charge linguistique pour Python et R. 

Pour voir la totalité des annonces concernant les fonctionnalités, consultez [Nouveautés de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md).

### <a name="r-enhancements"></a>Améliorations de R

Le composant R de SQL Server Machine Learning Services est la prochaine génération de SQL Server 2016 R Services, avec des versions mises à jour de base R, RevoScaler et d’autres packages.

Les nouvelles fonctionnalités de R incluent la [**gestion des packages**](package-management/install-r-packages-with-tsql.md), avec les points importants suivants : 

+ Les rôles de base de données aident les DBA à gérer les packages et à affecter des autorisations pour l’installation des packages.
+ [CREATE EXTERNAL LIBRARY](../t-sql/statements/create-external-library-transact-sql.md) aide les DBA à gérer les packages dans le langage T-SQL familier.
+ Les fonctions [RevoScaleR](package-management/install-r-packages-with-revoscaler.md) permettent d’installer, de supprimer ou de lister les packages détenus par les utilisateurs. Pour plus d’informations, consultez [Guide pratique pour utiliser les fonctions RevoScaleR pour rechercher ou installer des packages R sur SQL Server](package-management/install-r-packages-with-revoscaler.md).

### <a name="r-libraries"></a>Bibliothèques R

| Package | Description |
|---------|-------------|
| [**MicrosoftML**](r/ref-r-microsoftml.md) | Dans cette version, MicrosoftML est inclus dans une installation de R par défaut, ce qui élimine l’étape de mise à niveau requise dans la précédente version de SQL Server 2016 R Services. MicrosoftML fournit des algorithmes de machine learning et des transformations de données de pointe qui peuvent être mis à l’échelle ou exécutés dans des contextes de calcul distants. Les algorithmes incluent des réseaux neuronaux profonds personnalisables, des arbres de décision et des forêts d’arbres décisionnels rapides ainsi que les régressions linéaires et logistiques.  |

### <a name="python-integration-for-in-database-analytics"></a>Intégration de Python pour l’analytique dans la base de données

Python est un langage qui offre une grande flexibilité et efficacité pour un large éventail de tâches de machine learning. Les bibliothèques open source pour Python incluent plusieurs plateformes pour les réseaux neuronaux personnalisables ainsi que des bibliothèques populaires pour le traitement en langage naturel. 

Étant donné que Python est intégré au moteur de base de données, vous pouvez conserver l’analytique proche des données et éliminer les coûts et les risques de sécurité liés au déplacement des données. Vous pouvez déployer des solutions machine learning basées sur Python à l’aide d’outils tels que Visual Studio. Vos applications de production peuvent se procurer des prédictions, des modèles ou des visuels à partir du runtime Python 3.5 à l’aide des méthodes d’accès aux données SQL Server.

L’intégration de T-SQL et Python est prise en charge via la procédure stockée système [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Vous pouvez appeler n’importe quel code Python à l’aide de cette procédure stockée. Le code s’exécute dans une architecture sécurisée et double qui permet le déploiement à l’échelle de l’entreprise de modèles et de scripts Python pouvant être appelés à partir d’une application à l’aide d’une procédure stockée simple. Des gains de performances supplémentaires sont réalisés en diffusant des données de SQL vers des processus Python et une parallélisation de sonnerie MPI.

Vous pouvez utiliser la fonction T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md) pour effectuer un [scoring en natif](predictions/native-scoring-predict-transact-sql.md) sur un modèle préentraîné qui a déjà été enregistré au format binaire requis.

### <a name="python-libraries"></a>Bibliothèques Python

| Package | Description |
|---------|-------------|
|[**revoscalepy**](python/ref-py-revoscalepy.md)| Équivalent Python de RevoScaleR. Vous pouvez créer des modèles Python pour les régressions linéaires et logistiques, les arbres de décision, les arbres amplifiés et les forêts aléatoires, tous parallélisables et capables d’être exécutés dans des contextes de calcul distants. Ce package prend en charge l’utilisation de plusieurs sources de données et de contextes de calcul distants. Le scientifique de données ou le développeur peut exécuter du code Python sur un serveur SQL Server distant pour explorer des données ou créer des modèles sans déplacer les données. |
|[**microsoftml**](python/ref-py-microsoftml.md) |Équivalent Python du package R MicrosoftML. |

### <a name="pre-trained-models"></a>Modèles dont l’apprentissage a déjà été effectué

Des [**modèles préentraînés**](install/sql-pretrained-models-install.md) sont disponibles pour Python et R. Utilisez ces modèles pour la reconnaissance d’images et l’analyse des sentiments positifs-négatifs afin de générer des prédictions sur vos propres données. 

### <a name="standalone-server-as-a-shared-feature-in-sql-server-setup"></a>Serveur autonome en tant que fonctionnalité partagée dans le programme d’installation de SQL Server

Cette version ajoute également [SQL Server Machine Learning Server (autonome)](r/r-server-standalone.md), un serveur de science des données entièrement indépendant, prenant en charge l’analytique statistique et prédictive dans R et Python. Comme avec R Services, ce serveur est la prochaine version de SQL Server 2016 R Server (autonome). Avec le serveur autonome, vous pouvez distribuer et mettre à l’échelle des solutions R ou Python sans dépendances sur SQL Server.
::: moniker-end

::: moniker range=">=sql-server-2016"
## <a name="new-in-sql-server-2016"></a>Nouveautés de SQL Server 2016

Cette version a introduit des fonctionnalités de machine learning dans SQL Server par le biais de **SQL Server 2016 R Services**, un moteur d’analytique dans la base de données pour le traitement de script R sur des données résidentes au sein d’une instance du moteur de base de données.

De plus, **SQL Server 2016 R Server (autonome)** a été publié comme moyen d’installer R Server sur un serveur Windows. Initialement, le programme d’installation de SQL Server fournissait la seule façon d’installer R Server pour Windows. Dans les versions ultérieures, les développeurs et les scientifiques de données qui souhaitaient R Server sur Windows pouvaient utiliser un autre programme d’installation autonome pour atteindre le même objectif. Le serveur autonome dans SQL Server est fonctionnellement équivalent au produit serveur autonome, [Microsoft R Server pour Windows](/machine-learning-server/install/r-server-install-windows).

Pour voir la totalité des annonces concernant les fonctionnalités, consultez [Nouveautés de SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md).

| Libérer |Mise à jour des fonctionnalités |
|---------|----------------|
| Ajouts de CU | Le [**scoring en temps réel**](predictions/real-time-scoring.md) repose sur les bibliothèques C++ natives pour lire un modèle stocké dans un format binaire optimisé, puis générer des prédictions sans devoir appeler le runtime R. Cela rend les opérations de scoring beaucoup plus rapides. Avec le scoring en temps réel, vous pouvez exécuter une procédure stockée ou effectuer un scoring en temps réel à partir du code R. Le scoring en temps réel est également disponible pour SQL Server 2016, si l’instance est mise à niveau vers la dernière version de [!INCLUDE[rsql-platform-md](../includes/rsql-platform-md.md)]. |
| Version initiale | [**Intégration de R pour l’analytique dans la base de données**](r/sql-server-r-services.md). <br/><br/> Packages R pour appeler des fonctions R dans T-SQL, et vice versa. Les fonctions RevoScaleR fournissent l’analytique R à l’échelle en segmentant les données en composants du produit, en coordonnant et en gérant le traitement distribué et en agrégeant les résultats. Dans SQL Server 2016 R Services (dans la base de données), le moteur RevoScaleR est intégré à une instance du moteur de base de données, ce qui permet de rassembler les données et l’analytique dans le même contexte de traitement. <br/><br/>Intégration de T-SQL et R via [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Vous pouvez appeler n’importe quel code R à l’aide de cette procédure stockée. Cette infrastructure sécurisée permet le déploiement à l’échelle de l’entreprise de modèles et de scripts R pouvant être appelés à partir d’une application à l’aide d’une procédure stockée simple. Des gains de performances supplémentaires sont réalisés en diffusant des données de SQL vers des processus R et une parallélisation de sonnerie MPI. <br/><br/>Vous pouvez utiliser la fonction T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md) pour effectuer un [scoring en natif](predictions/native-scoring-predict-transact-sql.md) sur un modèle préentraîné qui a déjà été enregistré au format binaire requis.|

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="linux-support"></a>Prise en charge Linux

SQL Server 2019 ajoute la prise en charge Linux pour R et Python quand vous installez les packages machine learning avec une instance du moteur de base de données. Pour plus d’informations, consultez [Installer SQL Server Machine Learning Services sur Linux](../linux/sql-server-linux-setup-machine-learning.md).

Sur Linux, SQL Server 2017 ne dispose pas de l’intégration de R ni Python, mais vous pouvez utiliser le [scoring en natif](predictions/native-scoring-predict-transact-sql.md) sur Linux, car cette fonctionnalité est disponible via la fonction T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md), qui s’exécute sur Linux. Le scoring en natif permet un scoring à hautes performances à partir d’un modèle préentraîné, sans appeler ni même exiger un runtime R.
::: moniker-end

## <a name="next-steps"></a>Étapes suivantes

+ [Installer SQL Server Machine Learning Services (dans la base de données)](install/sql-machine-learning-services-windows-install.md)
