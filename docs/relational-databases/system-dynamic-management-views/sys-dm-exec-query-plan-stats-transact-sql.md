---
description: sys. dm_exec_query_plan_stats (Transact-SQL)
title: sys. dm_exec_query_plan_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_query_plan_stats
- sys.dm_exec_query_plan_stats_TSQL
- dm_exec_query_plan_stats_TSQL
- dm_exec_query_plan_stats
helpviewer_keywords:
- sys.dm_exec_query_plan_stats management view
ms.assetid: fdc7659e-df41-488e-b2b5-0d79734dfacb
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 6c76005fefffdbce76309762b1d2a1cd81d83537
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88474971"
---
# <a name="sysdm_exec_query_plan_stats-transact-sql"></a>sys. dm_exec_query_plan_stats (Transact-SQL)
[!INCLUDE[SQL Server 2019](../../includes/tsql-appliesto-ssver15-asdb-xxxx-xxx.md)]

Retourne l’équivalent du dernier plan d’exécution réel connu pour un plan de requête précédemment mis en cache.

## <a name="syntax"></a>Syntaxe

```
sys.dm_exec_query_plan_stats(plan_handle)  
``` 

## <a name="arguments"></a>Arguments 
*plan_handle*  
Jeton qui identifie de façon unique un plan d’exécution de requête pour un lot exécuté et dont le plan réside dans le cache du plan, ou qui est en cours d’exécution. *plan_handle* est **de type varbinary (64)**.   

Le *plan_handle* peut être obtenu à partir des objets de gestion dynamique suivants :  
  
-   [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys. dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys. dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)  

## <a name="table-returned"></a>Table retournée

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|
|**dbid**|**smallint**|ID de la base de données de contexte qui était en fonction lorsque l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondant à ce plan a été compilée. Pour les instructions SQL ad hoc et préparées, l'ID de la base de données où les instructions ont été compilées.<br /><br /> Colonne acceptant la valeur NULL.|  
|**arguments**|**int**|ID de l'objet (par exemple, procédure stockée ou fonction définie par l'utilisateur) pour ce plan de requête. Pour les traitements ad hoc et préparés, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.|  
|**number**|**smallint**|Entier servant à la numérotation des procédures stockées. Par exemple, un groupe de procédures pour l'application **orders** peuvent être appelées **orderproc;1**, **orderproc;2**, etc. Pour les traitements ad hoc et préparés, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.|  
|**chiffrées**|**bit**|Indique si la procédure stockée correspondante est chiffrée.<br /><br /> 0 = Non chiffrée.<br /><br /> 1 = Chiffrée.<br /><br /> Colonne n'acceptant pas la valeur NULL.|  
|**query_plan**|**xml**|Contient la dernière représentation Showplan connue du plan d’exécution de requêtes réel spécifiée avec *plan_handle*. Le plan d'exécution de requêtes est au format XML. Un plan est généré pour chaque traitement contenant par exemple des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, des appels de procédures stockées et des appels de fonctions définies par l'utilisateur.<br /><br /> Colonne acceptant la valeur NULL.| 

## <a name="remarks"></a>Notes
Cette fonction système est disponible à partir de la [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] version CTP 2,4.

Il s’agit d’une fonctionnalité d’activation qui nécessite l’activation de l’[indicateur de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 2451. À compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] CTP 2.5, pour effectuer cette opération au niveau de la base de données, voir l’option LAST_QUERY_PLAN_STATS dans [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

Cette fonction système fonctionne sous l’infrastructure de profilage des statistiques d’exécution de requête **légère** . Pour plus d’informations, consultez [Infrastructure du profilage de requête](../../relational-databases/performance/query-profiling-infrastructure.md).  

La sortie Showplan de sys. dm_exec_query_plan_stats contient les informations suivantes :
-  Toutes les informations de compilation trouvées dans le plan mis en cache
-  Informations d’exécution telles que le nombre réel de lignes par opérateur, le temps processeur et la durée d’exécution de la requête totale, les avertissements de débordement, le DOP réel, la mémoire maximale utilisée et la mémoire allouée

Dans les conditions suivantes, une sortie Showplan **équivalente à un plan d’exécution réel** est retournée dans la colonne **query_plan** de la table retournée pour **sys. dm_exec_query_plan_stats**:  

-   Le plan se trouve dans [sys. dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md).     
    **AND**    
-   La requête en cours d’exécution est complexe ou consommatrice de ressources.

Dans les conditions suivantes, une sortie Showplan **simplifiée <sup>1</sup> ** est retournée dans la colonne **query_plan** de la table retournée pour **sys. dm_exec_query_plan_stats**:  

-   Le plan se trouve dans [sys. dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md).     
    **AND**    
-   La requête est assez simple, généralement classée dans le cadre d’une charge de travail OLTP.

<sup>1</sup> à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] CTP 2,5, cela fait référence à un plan d’exécution de requêtes qui ne contient que l’opérateur de nœud racine (Select). Pour [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] CTP 2,4, cela fait référence au plan mis en cache disponible via `sys.dm_exec_cached_plans` .

Dans les conditions suivantes, **aucune sortie n’est retournée** à partir de **sys. dm_exec_query_plan_stats**:

-   Le plan de requête spécifié à l’aide de *plan_handle* a été supprimé du cache du plan.     
    **OR**    
-   Le plan de requête n’a pas été mis en cache en premier lieu. Pour plus d’informations, consultez [mise en cache et réutilisation du plan d’exécution ](../../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse).
  
> [!NOTE] 
> En raison d’une limitation du nombre de niveaux imbriqués autorisés dans le type de données **XML** , **sys. dm_exec_query_plan** ne peut pas retourner des plans de requête qui remplissent ou dépassent 128 niveaux d’éléments imbriqués. Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette condition a empêché le plan de requête de retourner et génère l' [erreur 6335](../../relational-databases/errors-events/database-engine-events-and-errors.md#errors-6000-to-6999). Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 et versions ultérieures, la colonne **query_plan** retourne la valeur null.  

## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW SERVER STATE` sur le serveur.  

## <a name="examples"></a>Exemples  
  
### <a name="a-looking-at-last-known-actual-query-execution-plan-for-a-specific-cached-plan"></a>R. Examen du dernier plan d’exécution de requête réel connu pour un plan mis en cache spécifique  
 L’exemple suivant interroge **sys. dm_exec_cached_plans** pour rechercher le plan intéressant et copier son `plan_handle` à partir de la sortie.  
  
```sql  
SELECT * FROM sys.dm_exec_cached_plans;  
GO  
```  
  
Ensuite, pour obtenir le dernier plan d’exécution de requête réel connu, utilisez le copié `plan_handle` avec la fonction système **sys. dm_exec_query_plan_stats**.  
  
```sql  
SELECT * FROM sys.dm_exec_query_plan_stats(< copied plan_handle >);  
GO  
```   

### <a name="b-looking-at-last-known-actual-query-execution-plan-for-all-cached-plans"></a>B. Examen du dernier plan d’exécution de requête réel connu pour tous les plans mis en cache
  
```sql  
SELECT *   
FROM sys.dm_exec_cached_plans AS cp
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS st
CROSS APPLY sys.dm_exec_query_plan_stats(plan_handle) AS qps;  
GO  
```   

### <a name="c-looking-at-last-known-actual-query-execution-plan-for-a-specific-cached-plan-and-query-text"></a>C. Examen du dernier plan d’exécution de requête réel connu pour un plan et un texte de requête spécifiques mis en cache

```sql  
SELECT *   
FROM sys.dm_exec_cached_plans AS cp
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS st
CROSS APPLY sys.dm_exec_query_plan_stats(plan_handle) AS qps
WHERE st.text LIKE 'SELECT * FROM Person.Person%';  
GO  
```   

### <a name="d-look-at-cached-events-for-trigger"></a>D. Examiner les événements mis en cache pour le déclencheur

```sql
SELECT *
FROM sys.dm_exec_cached_plans
CROSS APPLY sys.dm_exec_query_plan_stats(plan_handle)
WHERE objtype ='Trigger';
GO
```

## <a name="see-also"></a>Voir aussi
  [Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à l’exécution &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  

