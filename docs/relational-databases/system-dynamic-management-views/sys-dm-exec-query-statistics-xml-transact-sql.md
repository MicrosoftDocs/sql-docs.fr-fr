---
description: sys.dm_exec_query_statistics_xml (Transact-SQL)
title: sys.dm_exec_query_statistics_xml (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_query_statistics_xml
- sys.dm_exec_query_statistics_xml_TSQL
- dm_exec_query_statistics_xml_TSQL
- dm_exec_query_statistics_xml
helpviewer_keywords:
- sys.dm_exec_query_statistics_xml management view
ms.assetid: fdc7659e-df41-488e-b2b5-0d79734dfecb
author: pmasl
ms.author: pelopes
ms.openlocfilehash: a74539209e56e1e2da9053cfffccd7d528684a30
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235842"
---
# <a name="sysdm_exec_query_statistics_xml-transact-sql"></a>sys.dm_exec_query_statistics_xml (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Retourne le plan d’exécution de requête pour les demandes en cours. Utilisez cette DMV pour récupérer Showplan XML avec des statistiques temporaires. 

## <a name="syntax"></a>Syntaxe

```
sys.dm_exec_query_statistics_xml(session_id)  
``` 

## <a name="arguments"></a>Arguments 
*session_id*  
 ID de session exécutant le lot à rechercher. *session_id* est de type **smallint**. *session_id* peut être obtenu à partir des objets de gestion dynamique suivants :  
  
-   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
-   [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
  
-   [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)  

## <a name="table-returned"></a>Table retournée

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|
|session_id|**smallint**|ID de la session. N'accepte pas la valeur NULL.|
|request_id|**int**|ID de la demande. N'accepte pas la valeur NULL.|
|sql_handle|**varbinary(64)**|Jeton qui identifie de façon unique le lot ou la procédure stockée dont fait partie la requête. Autorise la valeur Null.|
|plan_handle|**varbinary(64)**|Jeton qui identifie de façon unique un plan d’exécution de requête pour un lot en cours d’exécution. Autorise la valeur Null.|
|query_plan|**xml**|Contient la représentation Showplan du runtime du plan d’exécution de requête spécifié avec *plan_handle* contenant des statistiques partielles. Le plan d'exécution de requêtes est au format XML. Un plan est généré pour chaque traitement contenant par exemple des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, des appels de procédures stockées et des appels de fonctions définies par l'utilisateur. Autorise la valeur Null.|

## <a name="remarks"></a>Notes
Cette fonction système est disponible à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1. Voir KB [3190871](https://support.microsoft.com/help/3190871)

Cette fonction système fonctionne à la fois dans l’infrastructure de profilage des statistiques d’exécution de requête **standard** et **légère** . Pour plus d’informations, consultez [Infrastructure du profilage de requête](../../relational-databases/performance/query-profiling-infrastructure.md).  

Dans les conditions suivantes, aucune sortie Showplan n’est retournée dans la colonne **query_plan** de la table retournée pour **sys.dm_exec_query_statistics_xml**:  
  
-   Si le plan de requête qui correspond au *session_id* spécifié n’est plus en cours d’exécution, la colonne **query_plan** de la table retournée est null. Par exemple, cette condition peut se produire s’il existe un délai entre le moment où le descripteur de plan a été capturé et celui où il a été utilisé avec **sys.dm_exec_query_statistics_xml**.  
    
En raison d’une limitation du nombre de niveaux imbriqués autorisés dans le type de données **XML** , **sys.dm_exec_query_statistics_xml** ne peut pas retourner des plans de requête qui remplissent ou dépassent 128 niveaux d’éléments imbriqués. Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette condition empêchait les retours par le plan de requête et générait l'erreur 6335. Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 et versions ultérieures, la colonne **query_plan** retourne la valeur null.   

## <a name="permissions"></a>Autorisations  
Sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , nécessite `VIEW SERVER STATE` l’autorisation sur le serveur.  
Sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] les niveaux Premium, requiert l' `VIEW DATABASE STATE` autorisation dans la base de données. Sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] les niveaux standard et de base, nécessite l' **administrateur du serveur** ou un compte d' **administrateur Azure Active Directory** .

## <a name="examples"></a>Exemples  
  
### <a name="a-looking-at-live-query-plan-and-execution-statistics-for-a-running-batch"></a>R. Examen du plan de requête en direct et des statistiques d’exécution d’un lot en cours d’exécution  
 L’exemple suivant interroge **sys.dm_exec_requests** pour trouver la requête intéressante et copier son `session_id` à partir de la sortie.  
  
```sql  
SELECT * FROM sys.dm_exec_requests;  
GO  
```  
  
 Ensuite, pour obtenir le plan de requête en direct et les statistiques d’exécution, utilisez le `session_id` **sys.dm_exec_query_statistics_xml** copié avec la fonction système.  
  
```sql  
--Run this in a different session than the session in which your query is running.
SELECT * FROM sys.dm_exec_query_statistics_xml(< copied session_id >);  
GO  
```   

 Ou combiné pour toutes les demandes en cours d’exécution.  
  
```sql  
--Run this in a different session than the session in which your query is running.
SELECT * FROM sys.dm_exec_requests
CROSS APPLY sys.dm_exec_query_statistics_xml(session_id);  
GO  
```   
  
## <a name="see-also"></a>Voir aussi
  [Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  

