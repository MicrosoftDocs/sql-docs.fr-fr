---
description: DBCC FREEPROCCACHE (Transact-SQL)
title: DBCC FREEPROCCACHE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/13/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FREEPROCCACHE_TSQL
- FREEPROCCACHE
- DBCC_FREEPROCCACHE_TSQL
- DBCC FREEPROCCACHE
dev_langs:
- TSQL
helpviewer_keywords:
- freeing procedure cache
- removing procedure cache elements
- deleting procedure cache elements
- DBCC FREEPROCCACHE statement
- procedure cache [SQL Server]
- clearing procedure cache
ms.assetid: 0e09d210-6f23-4129-aedb-3d56b2980683
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 635cbfe8b21f60bf2bb9cec49efac62c627a6ae4
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172621"
---
# <a name="dbcc-freeproccache-transact-sql"></a>DBCC FREEPROCCACHE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Supprime tous les éléments du cache du plan, supprime un plan spécifique du cache du plan en spécifiant un handle de plan ou un handle SQL, ou supprime toutes les entrées de cache associées à un pool de ressources spécifié.

> [!NOTE]
> DBCC FREEPROCCACHE n'efface pas les statistiques d'exécution pour les procédures stockées compilées en mode natif. Le cache de procédures ne contient pas d'informations relatives aux procédures stockées compilées en mode natif. Toutes les statistiques d’exécution collectées à partir des exécutions de procédure s’affichent dans les vues de gestion dynamique de statistiques d’exécution : [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) et [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md).  
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
Syntaxe pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :

```sql
DBCC FREEPROCCACHE [ ( { plan_handle | sql_handle | pool_name } ) ] [ WITH NO_INFOMSGS ]  
```  

Syntaxe pour [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] :
  
```sql
DBCC FREEPROCCACHE [ ( COMPUTE | ALL ) ] 
     [ WITH NO_INFOMSGS ]   
[;]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 ( { *plan_handle* | *sql_handle* | *pool_name* } )  
*plan_handle* identifie de façon unique un plan de requête pour un lot exécuté et dont le plan réside dans le cache du plan. *plan_handle* est de type **varbinary(64)** et peut être obtenu à partir des objets de gestion dynamique suivants :  
 -   [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  

*sql_handle* est le handle SQL du lot à effacer. *sql_handle* est de type **varbinary(64)** et peut être obtenu à partir des objets de gestion dynamique suivants :  
 -   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
 -   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
 -   [sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)  
 -   [sys.dm_exec_xml_handles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)  
 -   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  

*pool_name* est le nom d’un pool de ressources de Resource Governor. *pool_name* est de type **sysname** et peut être obtenu en interrogeant la vue de gestion dynamique [sys.dm_resource_governor_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
 Pour associer un groupe de charge de travail de Resource Governor à un pool de ressources, interrogez la vue de gestion dynamique [sys.dm_resource_governor_workload_groups](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md). Pour plus d’informations sur le groupe de charge de travail pour une session, interrogez la vue de gestion dynamique [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md).  

  
 WITH NO_INFOMSGS  
 Supprime tous les messages d'information.  
  
 COMPUTE  
 Vide le cache du plan de requête à partir de chaque nœud de calcul. Valeur par défaut.  
  
 ALL  
 Vide le cache du plan de requête à partir de chaque nœud de calcul et du nœud de contrôle.  

> [!NOTE]
> À compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], utilisez l’instruction `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE` pour effacer le cache (du plan) de procédure pour la base de données dans l’étendue.

## <a name="remarks"></a>Remarques  
Utilisez l'instruction DBCC FREEPROCCACHE pour effacer le cache du plan avec précaution. L’effacement du cache (du plan) de procédure entraîne la suppression de tous les plans et la compilation d’un nouveau plan par les exécutions de requêtes entrantes, et non la réutilisation d’un plan mis en cache précédemment. 

Cette opération peut entraîner une baisse temporaire et brutale des performances des requêtes comme le nombre de nouvelles compilations augmente. Pour chaque mémoire cache effacée du cache du plan, le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient le message d’information suivant :

> `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to 'DBCC FREEPROCCACHE' or 'DBCC FREESYSTEMCACHE' operations.` 

Ce message est enregistré toutes les cinq minutes si le cache est vidé au cours de cet intervalle de temps.

Les opérations de reconfiguration suivantes effacent également le cache de procédures :
-   access check cache bucket count  
-   access check cache quota  
-   clr enabled  
-   cost threshold for parallelism  
-   cross db ownership chaining  
-   index create memory  
-   max degree of parallelism  
-   max server memory  
-   max text repl size  
-   max worker threads  
-   min memory per query  
-   min server memory  
-   query governor cost limit  
-   query wait  
-   remote query timeout  
-   user options  
  
## <a name="result-sets"></a>Jeux de résultats  
Quand la clause WITH NO_INFOMSGS n’est pas spécifiée, l’instruction DBCC FREEPROCCACHE retourne : « Exécution de DBCC terminée. Si DBCC vous a adressé des messages d'erreur, contactez l'administrateur système. »
  
## <a name="permissions"></a>Autorisations  
S’applique à : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 
- Nécessite l'autorisation ALTER SERVER STATE sur le serveur.  

S’applique à : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]
- Nécessite l'appartenance au rôle serveur fixe DB_OWNER.  

## <a name="general-remarks-for-sssdw-and-sspdw"></a>Remarques d’ordre général pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Plusieurs commandes DBCC FREEPROCCACHE peuvent être exécutées simultanément.
Dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], l’effacement du cache du plan peut entraîner une baisse temporaire des performances des requêtes comme les requêtes entrantes compilent un nouveau plan au lieu de réutiliser un plan mis en cache précédemment. 

DBCC FREEPROCCACHE (COMPUTE) contraint uniquement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à recompiler les requêtes quand elles sont exécutées sur les nœuds de calcul. Ni [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ni [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ne doivent recompiler le plan de requête parallèle qui est généré sur le nœud de contrôle.
Il est possible d’annuler DBCC FREEPROCCACHE lors de l’exécution.
  
## <a name="limitations-and-restrictions-for-sssdw-and-sspdw"></a>Limitations et restrictions pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Il n’est pas possible d’exécuter DBCC FREEPROCCACHE dans une transaction.
DBCC FREEPROCCACHE n’est pas pris en charge dans une instruction EXPLAIN.
  
## <a name="metadata-for-sssdw-and-sspdw"></a>Métadonnées pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Une nouvelle ligne est ajoutée à la vue système sys.pdw_exec_requests lors de l’exécution de DBCC FREEPROCCACHE.

## <a name="examples-ssnoversion"></a>Exemples : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
### <a name="a-clearing-a-query-plan-from-the-plan-cache"></a>R. Effacement d'un plan de requête du cache du plan  
L'exemple suivant efface un plan de requête du cache du plan en spécifiant le descripteur du plan de requête. Pour vérifier que l'exemple de requête se trouve dans le cache du plan, la requête est d'abord exécutée. Les vues de gestion dynamique `sys.dm_exec_cached_plans` et `sys.dm_exec_sql_text` sont interrogées pour retourner le handle de plan de la requête. 

Puis, la valeur du descripteur de plan extraite du jeu de résultats est insérée dans l'instruction `DBCC FREEPROCACHE` pour supprimer uniquement le plan du cache du plan.
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT * FROM Person.Address;  
GO  
SELECT plan_handle, st.text  
FROM sys.dm_exec_cached_plans   
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS st  
WHERE text LIKE N'SELECT * FROM Person.Address%';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
plan_handle                                         text  
--------------------------------------------------  -----------------------------  
0x060006001ECA270EC0215D05000000000000000000000000  SELECT * FROM Person.Address;  
  
(1 row(s) affected)
 ```
 
```sql  
-- Remove the specific plan from the cache.  
DBCC FREEPROCCACHE (0x060006001ECA270EC0215D05000000000000000000000000);  
GO  
```  
  
### <a name="b-clearing-all-plans-from-the-plan-cache"></a>B. Effacement de tous les plans du cache du plan  
L'exemple suivant efface tous les éléments du cache du plan. La clause WITH `NO_INFOMSGS` est spécifiée pour empêcher l’affichage du message d’information.
  
```sql  
DBCC FREEPROCCACHE WITH NO_INFOMSGS;  
```  
  
### <a name="c-clearing-all-cache-entries-associated-with-a-resource-pool"></a>C. Effacement de toutes les entrées de cache associées à un pool de ressources  
L'exemple suivant efface toutes les entrées de cache associées à un pool de ressources spécifié. La vue `sys.dm_resource_governor_resource_pools` est d’abord interrogée pour obtenir la valeur de *pool_name*.
  
```sql  
SELECT * FROM sys.dm_resource_governor_resource_pools;  
GO  
DBCC FREEPROCCACHE ('default');  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-dbcc-freeproccache-basic-syntax-examples"></a>D. Exemples de syntaxe de base DBCC FREEPROCCACHE  
L’exemple suivant supprime tous les caches des plans de requête existants des nœuds de calcul. Même si le contexte est défini sur UserDbSales, les caches des plans de requête des nœuds de calcul pour toutes les bases de données seront supprimés. La clause WITH NO_INFOMSGS empêche l’affichage des messages d’information dans les résultats.  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE) WITH NO_INFOMSGS;
```  
  
 L’exemple suivant présente les mêmes résultats que l’exemple précédent, à ceci près que les messages d’information s’affichent dans les résultats.  
  
```sql
USE UserDbSales;  
DBCC FREEPROCCACHE (COMPUTE);  
```  
  
Quand les messages d’information sont demandés et que l’exécution est réussie, les résultats de requête occupent une ligne par nœud de calcul.
  
### <a name="e-granting-permission-to-run-dbcc-freeproccache"></a>E. Octroi de l’autorisation d’exécuter DBCC FREEPROCCACHE  
L’exemple suivant donne à l’utilisateur connecté David l’autorisation d’exécuter DBCC FREEPROCCACHE.  
  
```sql
GRANT ALTER SERVER STATE TO David; 
GO
```  
  
## <a name="see-also"></a>Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[gouverneur de ressources](../../relational-databases/resource-governor/resource-governor.md)  
[ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)
  
  
