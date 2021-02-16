---
description: sys.dm_db_missing_index_group_stats_query (Transact-SQL)
title: sys.dm_db_missing_index_group_stats_query (Transact-SQL)
ms.custom: ''
ms.date: 02/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_group_stats_query_TSQL
- sys.dm_db_missing_index_group_stats_query
- dm_db_missing_index_group_stats_query_TSQL
- dm_db_missing_index_group_stats_query
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_group_stats_query dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_group_stats_query dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current
ms.openlocfilehash: f54143b965847c83cd07ee07543873fcd7359900
ms.sourcegitcommit: c6cc0b669b175ae290cf5b08952010661ebd03c3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100531049"
---
# <a name="sysdm_db_missing_index_group_stats_query-transact-sql"></a>sys.dm_db_missing_index_group_stats_query (Transact-SQL)
[!INCLUDE [SQL Server 2019 SQL Database](../../includes/applies-to-version/sqlserver2019-asdb-asdbmi.md)]

  Retourne des informations sur les requêtes qui nécessitaient un index manquant à partir de groupes d’index manquants, à l’exclusion des index spatiaux. Plusieurs requêtes peuvent être retournées par groupe d’index manquant. Un groupe d’index manquant peut avoir plusieurs requêtes qui ont besoin du même index.
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les vues de gestion dynamique ne peuvent pas exposer des informations qui ont un impact sur la relation contenant-contenu de la base de données, ou exposer des informations concernant d'autres bases de données auxquelles l'utilisateur a accès. Pour éviter d’exposer ces informations, chaque ligne qui contient des données qui n’appartiennent pas au locataire connecté est filtrée.  
    
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**group_handle**|**int**|Identifie un groupe d'index manquant. Cet identificateur est unique sur le serveur.<br /><br /> Les autres colonnes fournissent des informations sur toutes les requêtes pour lesquelles l'index du groupe est considéré comme manquant.<br /><br /> Un groupe d'index ne contient qu'un seul index.<BR><BR>Peut être joint à **index_group_handle** dans [sys.dm_db_missing_index_groups](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md).|  
|**query_hash**|**Binary(8**|La valeur de hachage binaire calculée sur la requête et utilisée pour identifier des requêtes avec une logique similaire. Vous pouvez utiliser le hachage de requête pour déterminer l'utilisation des ressources globale pour les requêtes qui diffèrent uniquement par les valeurs littérales.|  
|**query_plan_hash**|**Binary(8**|Valeur de hachage binaire calculée sur le plan d'exécution de requête et utilisée pour identifier des plans d'exécution de requête semblables. Vous pouvez utiliser le hachage de plan de requête pour rechercher le coût cumulatif de requêtes avec les plans d'exécution semblables.<br /><br /> Sa valeur est toujours 0x000 lorsqu'une procédure stockée compilée en mode natif interroge une table optimisée en mémoire.|  
|**last_sql_handle**|**varbinary(64)**|Jeton qui identifie de façon unique le lot ou la procédure stockée de la dernière instruction compilée qui a requis cet index.<BR><BR>**last_sql_handle** peut être utilisé pour récupérer le texte SQL de la requête en appelant la fonction de gestion dynamique [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) .|
|**last_statement_start_offset**|**int**|Indique, en octets, à partir de 0, la position de départ de la requête que la ligne décrit dans le texte de son traitement ou de son objet persistant pour la dernière instruction compilée qui avait besoin de cet index dans son lot SQL.|
|**last_statement_end_offset**|**int**|Indique, en octets, à partir de 0, la position de fin de la requête que la ligne décrit dans le texte de son traitement ou de son objet persistant pour la dernière instruction compilée qui avait besoin de cet index dans son lot SQL.|
|**last_statement_sql_handle**|**varbinary(64)**|Jeton qui identifie de façon unique le lot ou la procédure stockée de la dernière instruction compilée qui a requis cet index.<BR><BR>**last_statement_sql_handle**, ainsi que **last_statement_start_offset** et **last_statement_end_offset**, peuvent être utilisés pour récupérer le texte SQL de la requête en appelant la fonction de gestion dynamique [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) .<BR><BR>Si Magasin des requêtes n’a pas été activée lors de la compilation de la requête, retourne 0.|
|**user_seeks**|**bigint**|Nombre de recherches résultant de requêtes utilisateur pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**user_scans**|**bigint**|Nombre d'analyses résultant de requêtes utilisateur pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**last_user_seek**|**datetime**|Date et heure de la dernière recherche résultant de requêtes utilisateur pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**last_user_scan**|**datetime**|Date et heure de la dernière analyse résultant de requêtes utilisateur pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**avg_total_user_cost**|**float**|Coût moyen des requêtes utilisateur qui pourrait être réduit grâce à l'index du groupe.|  
|**avg_user_impact**|**float**|Bénéfice moyen (en pourcentage) dont les requêtes utilisateur pourraient tirer parti si ce groupe d'index manquants était implémenté. Cela signifie que le coût des requêtes diminuerait, en moyenne, de la valeur de ce pourcentage si ce groupe d'index manquants était implémenté.|  
|**system_seeks**|**bigint**|Nombre de recherches résultant de requêtes système (telles que les requêtes de statistiques automatiques) pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé. Pour plus d’informations, consultez [classe d’événements auto stats](../../relational-databases/event-classes/auto-stats-event-class.md).|  
|**system_scans**|**bigint**|Nombre d'analyses résultant de requêtes système pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**last_system_seek**|**datetime**|Date et heure de la dernière recherche système résultant de requêtes système pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**last_system_scan**|**datetime**|Date et heure de la dernière analyse système résultant de requêtes système pour lesquelles l'index recommandé du groupe pourrait avoir été utilisé.|  
|**avg_total_system_cost**|**float**|Coût moyen des requêtes système qui pourrait être réduit grâce à l'index du groupe.|  
|**avg_system_impact**|**float**|Bénéfice moyen (en pourcentage) dont les requêtes système pourraient tirer parti si ce groupe d'index manquants était implémenté. Cela signifie que le coût des requêtes diminuerait, en moyenne, de la valeur de ce pourcentage si ce groupe d'index manquants était implémenté.|  
  
## <a name="remarks"></a>Notes  
 Les informations retournées par **sys.dm_db_missing_index_group_stats_query** sont mises à jour par chaque exécution de requête, et non par chaque compilation ou recompilation de requête. Les statistiques d'utilisation ne sont pas conservées de manière permanente ; elles sont simplement conservées jusqu'au redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les administrateurs de base de données doivent effectuer régulièrement des copies de sauvegarde des informations sur les index manquants s'ils souhaitent conserver les statistiques d'utilisation après le recyclage du serveur.  
 
  >[!NOTE]
  >Le jeu de résultats pour cette DMV est limité à 600 lignes. Chaque ligne contient un index manquant. Si vous avez plus de 600 index manquants, vous devez traiter les index manquants existants afin de pouvoir en afficher les plus récents.

## <a name="permissions"></a>Autorisations  
 Pour interroger cette vue de gestion dynamique, les utilisateurs doivent bénéficier de l'autorisation VIEW SERVER STATE ou de tout privilège qui implique l'autorisation VIEW SERVER STATE.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent l’utilisation de l' **sys.dm_db_missing_index_group_stats_query** vue de gestion dynamique.  
  
  
### <a name="a-find-the-latest-query-text-for-the-top-10-highest-anticipated-improvements-for-user-queries"></a>R. Rechercher le dernier texte de requête pour les 10 principales améliorations attendues pour les requêtes utilisateur 
 La requête suivante renvoie le dernier texte de requête enregistré pour les 10 index manquants qui produirait l’amélioration cumulative la plus élevée attendue, dans l’ordre décroissant.  

```sql
SELECT TOP 10 
    SUBSTRING
    (
            sql_text.text,
            misq.last_statement_start_offset / 2 + 1,
            (
            CASE misq.last_statement_Start_offset
                WHEN -1 THEN datalength(sql_text.text)
                ELSE misq.last_statement_end_offset
            END - misq.last_statement_start_offset
            ) / 2 + 1
    ),
    misq.*
FROM sys.dm_db_missing_index_group_stats_query AS misq
CROSS APPLY sys.dm_exec_sql_text(last_sql_handle) AS sql_text
ORDER BY avg_total_user_cost * avg_user_impact * (user_seeks + user_scans) DESC; 
```
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_db_missing_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
