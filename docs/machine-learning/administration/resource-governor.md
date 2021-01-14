---
title: Gérer avec Resource Governor
description: Découvrez comment utiliser Resource Governor pour gérer le processeur, les E/S physiques et l’allocation des ressources de mémoire pour les charges de travail Python et R dans SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 3e71b33eb08fd386232e992e9b47da7b5057aa32
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170721"
---
# <a name="manage-python-and-r-workloads-with-resource-governor-in-sql-server-machine-learning-services"></a>Gérer les charges de travail Python et R avec Resource Governor dans SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Découvrez comment utiliser [Resource Governor](../../relational-databases/resource-governor/resource-governor.md) pour gérer le processeur, les E/S physiques et l’allocation des ressources de mémoire pour les charges de travail Python et R dans SQL Server Machine Learning Services.

Les algorithmes d’apprentissage automatique dans Python et R nécessitent beaucoup ressources de calcul. En fonction des priorités de votre charge de travail, vous devrez peut-être augmenter ou diminuer les ressources disponibles pour Machine Learning Services.

Pour plus d’informations générales, consultez [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).

> [!NOTE] 
> Resource Governor est une fonctionnalité Enterprise Edition.

## <a name="default-allocations"></a>Allocations par défaut

Par défaut, les runtimes de script externes pour Machine Learning sont limités à 20 % de la mémoire totale de l’ordinateur. Cela dépend de votre système, mais en général, cette limite peut s’avérer inadaptée pour des tâches de Machine Learning importantes telles que la formation d’un modèle ou la prédiction de nombreuses lignes de données. 

## <a name="manage-resources-with-resource-governor"></a>Gérer des ressources avec Resource Governor
 
Par défaut, les processus externes utilisent jusqu’à 20 % de la mémoire totale de l’ordinateur hôte sur le serveur local. Vous pouvez modifier le pool de ressources par défaut pour apporter des modifications à l’ensemble du serveur, où les processus R et Python utilisent la capacité que vous mettez à disposition des processus externes.

Vous pouvez également créer des pools de ressources **externes personnalisés**, avec des classifieurs et des groupes de charges de travail associés, pour déterminer l’allocation des ressources pour les demandes provenant de programmes spécifiques, d’hôtes ou d’autres critères que vous fournissez. Un pool de ressources externe est un type de pool de ressources inauguré dans [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] pour faciliter la gestion des processus R et Python externes au moteur de base de données.

1. [Activez la de gouvernance des ressources](../../relational-databases/resource-governor/enable-resource-governor.md) (elle est désactivée par défaut).

2. Exécutez [CREATE EXTERNAL RESOURCE POOL](../../t-sql/statements/create-external-resource-pool-transact-sql.md) pour créer et configurer le pool de ressources, puis [ALTER RESOURCE GOVERNOR](../../t-sql/statements/alter-resource-governor-transact-sql.md) pour l’implémenter.

3. Créez un groupe de charge de travail pour les allocations granulaires, par exemple entre la formation et le scoring.

4. Créez un classifieur pour intercepter les appels pour le traitement externe.

5. Exécutez des requêtes et des procédures à l’aide des objets que vous avez créés.

Pour obtenir une procédure pas à pas, consultez [Créer un pool de ressources pour SQL Server Machine Learning Services](create-external-resource-pool.md) pour obtenir des instructions pas à pas.

Vous trouverez une introduction à la terminologie et aux concepts généraux dans la rubrique [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md).

## <a name="processes-under-resource-governance"></a>Processus sous gouvernance des ressources
  
 Vous pouvez utiliser un *pool de ressources externe* pour gérer les ressources utilisées par les exécutables suivants sur l’instance d’un moteur de base de données :

+ Rterm.exe lorsqu’appelé localement à partir de SQL Server ou appelé à distance avec SQL Server comme contexte de calcul distant
+ Python.exe lorsqu’appelé localement à partir de SQL Server ou appelé à distance avec SQL Server comme contexte de calcul distant
+ BxlServer.exe et les processus satellites
+ Processus satellites lancés par LaunchPad, tels que PythonLauncher.dll
  
> [!NOTE]
> La gestion directe du service Launchpad à l’aide de Resource Governor n’est pas prise en charge. Launchpad est un service approuvé qui peut héberger uniquement les lanceurs fournis par Microsoft. Les lanceurs approuvés sont explicitement configurés pour éviter de consommer trop de ressources.
  
## <a name="next-steps"></a>Étapes suivantes

+ [Créer un pool de ressources pour Machine Learning](create-external-resource-pool.md)
+ [Pools de ressources Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)