---
title: Surveiller T-SQL avec des événements étendus
description: Découvrez comment utiliser des événements étendus pour surveiller et résoudre les problèmes liés aux instructions PREDICT T-SQL dans SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/24/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 099e01e8e626b7a812ebcf2135b7d5e126a49e4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337136"
---
# <a name="monitor-predict-t-sql-statements-with-extended-events-in-sql-server-machine-learning-services"></a>Surveiller les instructions PREDICT T-SQL avec des événements étendus dans SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Découvrez comment utiliser des événements étendus pour surveiller et résoudre les problèmes liés aux instructions [PREDICT](../../t-sql/queries/predict-transact-sql.md) T-SQL dans SQL Server Machine Learning Services.

## <a name="table-of-extended-events"></a>Tableau des événements étendus

Les événements étendus suivants sont disponibles dans toutes les versions de SQL Server prenant en charge l’instruction [PREDICT](../../t-sql/queries/predict-transact-sql.md) T-SQL. 

| name                       | object_type | description |
|----------------------------|-------------|-------------|
| predict_function_completed | événement       | Décomposition intégrée du temps d’exécution|
| predict_model_cache_hit    | événement       | Se produit quand un modèle est récupéré auprès du cache des modèles de la fonction PREDICT. Utilisez cet événement avec d’autres événements predict_model_cache_* pour résoudre les problèmes provoqués par le cache des modèles de la fonction PREDICT.|
| predict_model_cache_insert | événement       | Se produit quand un modèle est inséré dans le cache des modèles de la fonction PREDICT. Utilisez cet événement avec d’autres événements predict_model_cache_* pour résoudre les problèmes provoqués par le cache des modèles de la fonction PREDICT.   |
| predict_model_cache_miss   | événement       | Se produit quand un modèle est introuvable dans le cache des modèles de la fonction PREDICT. De fréquentes occurrences de cet événement peuvent indiquer que SQL Server nécessite davantage de mémoire. Utilisez cet événement avec d’autres événements predict_model_cache_* pour résoudre les problèmes provoqués par le cache des modèles de la fonction PREDICT.|
| predict_model_cache_remove | événement       | Se produit quand un modèle est supprimé du cache des modèles de la fonction PREDICT. Utilisez cet événement avec d’autres événements predict_model_cache_* pour résoudre les problèmes provoqués par le cache des modèles de la fonction PREDICT.|

## <a name="query-for-related-events"></a>Requêtes pour les événements connexes

Pour afficher la liste de toutes les colonnes retournées pour ces événements, exécutez la requête suivante dans SQL Server Management Studio :

```sql
SELECT *
FROM sys.dm_xe_object_columns
WHERE object_name LIKE 'predict%'
```

## <a name="examples"></a>Exemples

Pour collecter des informations sur les performances d’une session de scoring à l’aide de PREDICT :

1. Créez une session d’événements étendus à l’aide de Management Studio ou d’un autre [outil](../../relational-databases/extended-events/extended-events-tools.md) pris en charge.
2. Ajoutez les événements `predict_function_completed` et `predict_model_cache_hit` à la session.
3. Lancez la session d’événements étendus.
4. Exécutez la requête utilisant PREDICT.

Dans les résultats, examinez les colonnes suivantes :

+ La valeur de `predict_function_completed` indique le temps que la requête a consacré au chargement du modèle et au scoring.
+ La valeur booléenne de `predict_model_cache_hit` indique si la requête a utilisé un modèle mis en cache. 

### <a name="native-scoring-model-cache"></a>Cache du modèle de scoring natif

En plus des événements spécifiques de PREDICT, vous pouvez utiliser les requêtes suivantes pour obtenir plus d’informations sur le modèle mis en cache et l’utilisation du cache :

Afficher le cache du modèle de scoring natif :

```sql
SELECT *
FROM sys.dm_os_memory_clerks
WHERE type = 'CACHESTORE_NATIVESCORING';
```

Affichez les objets dans le cache du modèle :

```sql
SELECT *
FROM sys.dm_os_memory_objects
WHERE TYPE = 'MEMOBJ_NATIVESCORING';
```

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les événements étendus (parfois appelés XEvents) et sur les méthodes permettant de suivre les événements dans une session, consultez les articles suivants :

+ [Surveiller les scripts Python et R avec des événements étendus dans SQL Server Machine Learning Services](extended-events.md)
+ [Extended Events concepts and architecture](../../relational-databases/extended-events/extended-events.md) (Architecture et concepts des événements étendus)
+ [Set up event capture in SSMS](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md) (Configurer la capture d’événements dans SSMS)
+ [Gérer les sessions d’événements dans l’Explorateur d’objets](../../relational-databases/extended-events/manage-event-sessions-in-the-object-explorer.md)