---
title: Vues de gestion dynamique - Statistiques d’utilisation et niveau de performance des vues
description: Découvrez comment utiliser les vues de gestion dynamique sys.dm_exec_query_optimizer_info, sys.views et sys.dmv_exec_cached_plans pour obtenir des statistiques sur les performances des requêtes SQL.
ms.custom: seo-dt-2019
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.date: 09/27/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.openlocfilehash: ed8611e945a6af654a0f1304ddee64b24af2efbb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015399"
---
# <a name="use-dmvs-to-determine-usage-statistics-and-performance-of-views"></a>Utiliser des vues de gestion dynamique pour déterminer les statistiques d’utilisation et les performances des vues
Cet article décrit la méthodologie et les scripts utilisés pour obtenir des informations sur les **performances des requêtes qui utilisent des vues**. Le but de ces scripts est de fournir des indicateurs sur l’utilisation et les performances des différentes vues trouvées dans une base de données. 

## <a name="sysdm_exec_query_optimizer_info"></a>sys.dm_exec_query_optimizer_info
La vue de gestion dynamique [sys.dm_exec_query_optimizer_info](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-optimizer-info-transact-sql.md) expose des statistiques sur les optimisations effectuées par l’optimiseur de requête de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ces valeurs sont cumulatives et leur enregistrement commence au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur l’optimiseur de requête, consultez [Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md).   

L’expression de table commune ci-dessous utilise cette vue de gestion dynamique pour fournir des informations sur la charge de travail, comme le pourcentage des requêtes qui référencent une vue. Les résultats retournés par cette requête n’indiquent pas par eux-mêmes un problème de performance, mais peuvent faire apparaître des problèmes sous-jacents quand ils sont combinés avec les plaintes des utilisateurs concernant des requêtes dont l’exécution est lente. 

```sql
WITH CTE_QO AS
(
  SELECT
    occurrence
  FROM
    sys.dm_exec_query_optimizer_info
  WHERE
    ([counter] = 'optimizations')
),
QOInfo AS
(
  SELECT
    [counter]
    ,[%] = CAST((occurrence * 100.00)/(SELECT occurrence FROM CTE_QO) AS DECIMAL(5, 2))
  FROM
    sys.dm_exec_query_optimizer_info
  WHERE
    [counter] IN ('optimizations'
                  ,'trivial plan'
                  ,'no plan'
                  ,'search 0'
                  ,'search 1'
                  ,'search 2'
                  ,'timeout'
                  ,'memory limit exceeded'
                  ,'insert stmt'
                  ,'delete stmt'
                  ,'update stmt'
                  ,'merge stmt'
                  ,'contains subquery'
                  ,'view reference'
                  ,'remote query'
                  ,'dynamic cursor request'
                  ,'fast forward cursor request'
             )
)
SELECT
  [optimizations] AS [optimizations %]
  ,[trivial plan] AS [trivial plan %]
  ,[no plan] AS [no plan %]
  ,[search 0] AS [search 0 %]
  ,[search 1] AS [search 1 %]
  ,[search 2] AS [search 2 %]
  ,[timeout] AS [timeout %]
  ,[memory limit exceeded] AS [memory limit exceeded %]
  ,[insert stmt] AS [insert stmt %]
  ,[delete stmt] AS [delete stmt]
  ,[update stmt] AS [update stmt]
  ,[merge stmt] AS [merge stmt]
  ,[contains subquery] AS [contains subquery %]
  ,[view reference] AS [view reference %]
  ,[remote query] AS [remote query %]
  ,[dynamic cursor request] AS [dynamic cursor request %]
  ,[fast forward cursor request] AS [fast forward cursor request %]
FROM
  QOInfo
PIVOT (MAX([%]) FOR [counter] 
  IN ([optimizations]
      ,[trivial plan]
      ,[no plan]
      ,[search 0]
      ,[search 1]
      ,[search 2]
      ,[timeout]
      ,[memory limit exceeded]
      ,[insert stmt]
      ,[delete stmt]
      ,[update stmt]
      ,[merge stmt]
      ,[contains subquery]
      ,[view reference]
      ,[remote query]
      ,[dynamic cursor request]
      ,[fast forward cursor request])) AS p;
GO
```

Combinez les résultats de cette requête avec les résultats de la vue système [sys.views](../../relational-databases/system-catalog-views/sys-views-transact-sql.md) pour identifier les statistiques de la requête, le texte de la requête et le plan d’exécution mis en cache. 

## <a name="sysviews"></a>sys.views
L’expression de table commune ci-dessous fournit des informations sur le nombre d’exécutions, la durée totale d’exécution et les pages lues à partir de la mémoire. Les résultats peuvent être utilisés pour identifier les requêtes candidates pour l’optimisation. 
  
> [!NOTE]
> Les résultats de cette requête peuvent varier en fonction de la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  


```sql
WITH CTE_VW_STATS AS
(
  SELECT
    SCHEMA_NAME(vw.schema_id) AS schemaname
    ,vw.name AS viewname
    ,vw.object_id AS viewid
  FROM
    sys.views AS vw
  WHERE
    (vw.is_ms_shipped = 0)
  INTERSECT
  SELECT
    SCHEMA_NAME(o.schema_id) AS schemaname
    ,o.Name AS name
    ,st.objectid AS viewid
  FROM
    sys.dm_exec_cached_plans cp
  CROSS APPLY
    sys.dm_exec_sql_text(cp.plan_handle) st
  INNER JOIN
    sys.objects o ON st.[objectid] = o.[object_id]
  WHERE
    st.dbid = DB_ID()
)
SELECT
  vw.schemaname
  ,vw.viewname
  ,vw.viewid
  ,DB_NAME(t.databaseid) AS databasename
  ,t.databaseid
  ,t.*
FROM
  CTE_VW_STATS AS vw
CROSS APPLY
  (
    SELECT
      st.dbid AS databaseid
      ,st.text
      ,qp.query_plan
      ,qs.*
    FROM
      sys.dm_exec_query_stats AS qs
    CROSS APPLY
      sys.dm_exec_sql_text(qs.plan_handle) AS st
    CROSS APPLY
      sys.dm_exec_query_plan(qs.plan_handle) AS qp
    WHERE
      (CHARINDEX(vw.schemaname, st.text, 1) > 0)
      AND (st.dbid = DB_ID())
  ) AS t;
GO
```

## <a name="sysdmv_exec_cached_plans"></a>sys.dmv_exec_cached_plans
La dernière requête fournit des informations sur les vues non utilisées avec la vue de gestion dynamique [sys.dmv_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md). Cependant, le cache de plan d’exécution étant dynamique, les résultats peuvent varier. Utilisez donc cette requête au fil du temps pour déterminer si une vue est réellement utilisée ou non. 

```sql
SELECT
  SCHEMA_NAME(vw.schema_id) AS schemaname
  ,vw.name AS name
  ,vw.object_id AS viewid
FROM
  sys.views AS vw
WHERE
  (vw.is_ms_shipped = 0)
EXCEPT
SELECT
  SCHEMA_NAME(o.schema_id) AS schemaname
  ,o.name AS name
  ,st.objectid AS viewid
FROM
  sys.dm_exec_cached_plans cp
CROSS APPLY
  sys.dm_exec_sql_text(cp.plan_handle) st
INNER JOIN
  sys.objects o ON st.[objectid] = o.[object_id]
WHERE
  st.dbid = DB_ID();
GO
```

## <a name="see-also"></a>Voir aussi
[Fonctions et vues de gestion dynamique](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) 
