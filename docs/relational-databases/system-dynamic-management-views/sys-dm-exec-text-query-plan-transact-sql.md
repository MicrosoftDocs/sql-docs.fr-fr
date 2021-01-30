---
description: sys.dm_exec_text_query_plan (Transact-SQL)
title: sys.dm_exec_text_query_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_text_query_plan
- sys.dm_exec_text_query_plan_TSQL
- dm_exec_text_query_plan_TSQL
- sys.dm_exec_text_query_plan
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_text_query_plan dynamic management function
ms.assetid: 9d5e5f59-6973-4df9-9eb2-9372f354ca57
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 95259700c5235ebe748ae4eaaa0f320df8c7a595
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192947"
---
# <a name="sysdm_exec_text_query_plan-transact-sql"></a>sys.dm_exec_text_query_plan (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne le plan d'exécution de requêtes au format texte pour un lot [!INCLUDE[tsql](../../includes/tsql-md.md)] ou pour une instruction spécifique dans le lot. Le plan de requête spécifié par le descripteur de plan peut être en cache ou en cours d'exécution. Cette fonction à valeur de table est similaire à [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md), mais présente les différences suivantes :  
  
-   La sortie du plan de requête est au format texte.  
-   La taille de cette sortie n'est pas limitée.  
-   Les instructions individuelles dans le traitement peuvent être spécifiées.  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_exec_text_query_plan   
(   
    plan_handle   
    , { statement_start_offset | 0 | DEFAULT }  
        , { statement_end_offset | -1 | DEFAULT }  
)  
```  
  
## <a name="arguments"></a>Arguments  
*plan_handle*  
Jeton qui identifie de façon unique un plan d’exécution de requête pour un lot exécuté et dont le plan réside dans le cache du plan, ou qui est en cours d’exécution. *plan_handle* est **de type varbinary (64)**.   

Le *plan_handle* peut être obtenu à partir des objets de gestion dynamique suivants : 
  
-   [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)  
  
*statement_start_offset* | 0 | VALEURS  
Indique, en octets, la position de début de la requête que la ligne décrit dans le texte de son traitement ou de son objet persistant. *statement_start_offset* est de **type int**. La valeur 0 indique le début du traitement. La valeur par défaut est 0.  
  
Le décalage de début de l'instruction peut être obtenu à partir des objets de gestion dynamiques suivants :  
  
-  [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-  [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
*statement_end_offset* | -1 | VALEURS  
Indique, en octets, la position de fin de la requête que la ligne décrit dans le texte de son traitement ou de son objet persistant.  
  
*statement_start_offset* est de **type int**.  
  
La valeur -1 indique la fin du traitement. La valeur par défaut est -1.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**dbid**|**smallint**|ID de la base de données de contexte qui était en fonction lorsque l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondant à ce plan a été compilée. Pour les instructions SQL ad hoc et préparées, l'ID de la base de données où les instructions ont été compilées.<br /><br /> Colonne acceptant la valeur NULL.|  
|**objectid**|**int**|ID de l'objet (par exemple, procédure stockée ou fonction définie par l'utilisateur) pour ce plan de requête. Pour les traitements ad hoc et préparés, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.|  
|**number**|**smallint**|Entier servant à la numérotation des procédures stockées. Par exemple, un groupe de procédures pour l'application **orders** peuvent être appelées **orderproc;1**, **orderproc;2**, etc. Pour les traitements ad hoc et préparés, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.|  
|**chiffrées**|**bit**|Indique si la procédure stockée correspondante est chiffrée.<br /><br /> 0 = Non chiffrée.<br /><br /> 1 = Chiffrée.<br /><br /> Colonne n'acceptant pas la valeur NULL.|  
|**query_plan**|**nvarchar(max)**|Contient la représentation Showplan au moment de la compilation du plan d’exécution de requête spécifié avec *plan_handle*. Le Showplan est au format texte. Un plan est généré pour chaque traitement contenant par exemple des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, des appels de procédures stockées et des appels de fonctions définies par l'utilisateur.<br /><br /> Colonne acceptant la valeur NULL.|  
  
## <a name="remarks"></a>Notes  
 Aucune sortie Showplan n'est retournée dans la colonne **plan** de la table retournée pour l'objet **sys.dm_exec_text_query_plan** dans les conditions suivantes :  
  
-   Si le plan de requête spécifié à l’aide de *plan_handle* a été supprimé de la mémoire cache des plans, la colonne **query_plan** de la table retournée est null. Ceci peut se produire par exemple s'il existe un délai entre le moment où le descripteur de plan est capturé et le moment de son utilisation avec **sys.dm_exec_text_query_plan**.  
  
-   Certaines instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ne sont pas mises en mémoire cache, par exemple les instructions d'opérations en bloc ou les instructions contenant des littéraux de chaîne dont la taille est supérieure à 8 Ko. Il est impossible de récupérer pour de telles instructions des plans d'exécution XML à l'aide de **sys.dm_exec_text_query_plan** car elles n'existent pas dans la mémoire cache.  
  
-   Si une [!INCLUDE[tsql](../../includes/tsql-md.md)] procédure stockée ou un traitement contient un appel à une fonction définie par l’utilisateur ou un appel à du code SQL dynamique, par exemple à l’aide de exec (*String*), le plan d’exécution de requêtes XML compilé pour la fonction définie par l’utilisateur n’est pas inclus dans la table retournée par **sys.dm_exec_text_query_plan** pour le traitement ou la procédure stockée. Au lieu de cela, vous devez effectuer un appel distinct à **sys.dm_exec_text_query_plan** pour le *plan_handle* qui correspond à la fonction définie par l’utilisateur.  
  
Lorsqu’une requête ad hoc utilise un [Paramétrage](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) [simple](../../relational-databases/query-processing-architecture-guide.md#SimpleParam) ou forcé, la **query_plan** colonne contient uniquement le texte de l’instruction et non le plan de requête réel. Pour retourner le plan de requête, appelez **sys.dm_exec_text_query_plan** pour le descripteur de plan de la requête paramétrable préparée. Vous pouvez déterminer si la requête a été rendue paramétrable par le référencement de la colonne **sql** de la vue [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) ou la colonne de texte de la vue de gestion dynamique [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter **sys.dm_exec_text_query_plan**, l'utilisateur doit être membre du rôle serveur fixe **sysadmin** ou disposer de l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-retrieving-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>R. Récupération du plan de requête mis en mémoire cache pour un lot ou une requête Transact-SQL à exécution lente  
 Si une requête ou un traitement [!INCLUDE[tsql](../../includes/tsql-md.md)] s'exécute longtemps sur une connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifique, récupérez le plan d'exécution de cette requête ou de ce traitement pour trouver la raison de ce retard. L'exemple suivant montre la récupération du plan d'exécution de requêtes pour une requête ou un traitement s'exécutant lentement.  
  
> [!NOTE]  
> Pour exécuter cet exemple, remplacez les valeurs de *session_id* et *plan_handle* par des valeurs spécifiques à votre serveur.  
  
 Récupérez tout d'abord à l'aide de la procédure stockée `sp_who` l'ID de processus serveur (SPID) pour le processus exécutant la requête ou le traitement.  
  
```sql  
USE master;  
GO  
EXEC sp_who;  
GO  
```  
  
 Le jeu de résultats renvoyé par `sp_who` indique que le SPID est `54`. Utilisez cet identificateur avec la vue de gestion dynamique `sys.dm_exec_requests` pour récupérer le descripteur de plan via la requête suivante :  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_requests  
WHERE session_id = 54;  
GO  
```  
  
 La table retournée par **sys.dm_exec_requests** indique que le descripteur de plan pour la requête ou le traitement à exécution lente est `0x06000100A27E7C1FA821B10600`. L'exemple suivant retourne le plan de requête pour le descripteur de plan spécifié et utilise les valeurs par défaut 0 et -1 pour retourner toutes les instructions dans la requête ou le lot.  
  
```sql  
USE master;  
GO  
SELECT query_plan   
FROM sys.dm_exec_text_query_plan (0x06000100A27E7C1FA821B10600,0,-1);  
GO  
```  
  
### <a name="b-retrieving-every-query-plan-from-the-plan-cache"></a>B. Récupération de chaque plan de requête à partir de la mémoire cache des plans  
 Pour récupérer un instantané de tous les plans de requête résidant dans la mémoire cache des plans, procurez-vous les descripteurs de tous les plans de requête dans la mémoire cache via une requête dans la vue de gestion dynamique `sys.dm_exec_cached_plans`. Les descripteurs de plan sont stockés dans la colonne `plan_handle` de `sys.dm_exec_cached_plans`. Utilisez ensuite l'opérateur CROSS APPLY pour transmettre les descripteurs à `sys.dm_exec_text_query_plan` comme suit. La sortie Showplan pour chaque plan actuellement dans le cache du plan est dans la `query_plan` colonne de la table retournée.  
  
```sql  
USE master;  
GO  
SELECT *   
FROM sys.dm_exec_cached_plans AS cp   
CROSS APPLY sys.dm_exec_text_query_plan(cp.plan_handle, DEFAULT, DEFAULT);  
GO  
```  
  
### <a name="c-retrieving-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>C. Récupération de chaque plan de requête pour lequel le serveur a rassemblé des statistiques de requête à partir du cache du plan  
 Pour récupérer un instantané de tous les plans de requête pour lesquels le serveur a regroupé des statistiques actuellement dans la mémoire cache des plans, procurez-vous les descripteurs de ces plans dans la mémoire cache via une requête formulée dans la vue de gestion dynamique `sys.dm_exec_query_stats`. Les descripteurs de plan sont stockés dans la colonne `plan_handle` de `sys.dm_exec_query_stats`. Utilisez ensuite l'opérateur CROSS APPLY pour transmettre les descripteurs à `sys.dm_exec_text_query_plan` comme suit. La sortie du plan d'exécution de requêtes pour chaque plan est dans la colonne `query_plan` de la table retournée.  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_query_stats AS qs   
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset);  
GO  
```  
  
### <a name="d-retrieving-information-about-the-top-five-queries-by-average-cpu-time"></a>D. Récupération d’informations sur les cinq premières requêtes d’après le temps processeur moyen  
 L'exemple suivant retourne les plans de requête et le temps processeur moyen pour les cinq premières requêtes. La fonction **sys.dm_exec_text_query_plan** spécifie les valeurs par défaut 0 et -1 pour retourner toutes les instructions dans le traitement du plan de requête.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
Plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, 0, -1)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)  
