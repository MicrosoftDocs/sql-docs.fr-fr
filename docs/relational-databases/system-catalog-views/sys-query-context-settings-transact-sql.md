---
description: sys.query_context_settings (Transact-SQL)
title: sys.query_context_settings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/29/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS
- QUERY_CONTEXT_SETTINGS
dev_langs:
- TSQL
helpviewer_keywords:
- sys.query_context_settings catalog view
ms.assetid: 3c1887df-6bd8-491e-82fc-d25ad9589faf
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 510f508b8d0048b1e13aa544a3c07f20f3a1d44c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098339"
---
# <a name="sysquery_context_settings-transact-sql"></a>sys.query_context_settings (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Contient des informations sur la sémantique qui affecte les paramètres de contexte associés à une requête. Un certain nombre de paramètres de contexte sont disponibles dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui influencent la sémantique de requête (en définissant le résultat correct de la requête). Le même texte de requête compilé sous des paramètres différents peut produire des résultats différents (en fonction des données sous-jacentes).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**context_settings_id**|**bigint**|Clé primaire Cette valeur est exposée dans Showplan XML pour les requêtes.|  
|**set_options**|**varbinary (8)**|Masque de bits reflétant l’état de plusieurs options SET. Pour plus d’informations, consultez [sys.dm_exec_plan_attributes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md).|  
|**language_id**|**smallint**|ID de la langue. Pour plus d’informations, consultez [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).|  
|**date_format**|**smallint**|Format de date. Pour plus d’informations, consultez [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md).|  
|**date_first**|**tinyint**|Première valeur de date. Pour plus d’informations, consultez [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md).|  
|**statut**|**varbinary (2)**|Champ de masque de masque qui indique le type de requête ou de contexte dans lequel la requête a été exécutée. <br />La valeur de colonne peut être une combinaison de plusieurs indicateurs (exprimée en hexadécimal) :<br /><br /> 0x0-requête régulière (aucun indicateur spécifique)<br /><br /> 0x1-requête exécutée par le biais de l’une des procédures stockées API de curseur<br /><br /> 0X2-requête de notification<br /><br /> 0x4-requête interne<br /><br /> 0x8-requête paramétrée automatiquement sans paramétrage universel<br /><br /> 0x10-requête d’actualisation de l’extraction de curseur<br /><br /> 0x20-requête en cours d’utilisation dans les demandes de mise à jour de curseur<br /><br /> 0x40-le jeu de résultats initial est retourné lorsqu’un curseur est ouvert (récupération automatique de curseur)<br /><br /> 0x80-requête chiffrée<br /><br /> 0x100-requête dans le contexte du prédicat de sécurité au niveau des lignes|  
|**required_cursor_options**|**int**|Options de curseur spécifiées par l'utilisateur (type de curseur par exemple).|  
|**acceptable_cursor_options**|**int**|Options de curseur dans lesquelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut convertir implicitement afin de prendre en charge l'exécution de l'instruction.|  
|**merge_action_type**|**smallint**|Type de plan d’exécution de déclencheur utilisé à la suite d’une instruction **Merge** .<br /><br /> 0 indique un plan de non-déclenchement, un plan de déclencheur qui ne s’exécute pas à la suite d’une instruction **Merge** ou un plan de déclencheur qui s’exécute à la suite d’une instruction **Merge** qui spécifie uniquement une action **Delete** .<br /><br /> 1 indique un plan de déclencheur **Insert** qui s’exécute à la suite d’une instruction **Merge** .<br /><br /> 2 indique un plan de déclencheur **Update** qui s’exécute à la suite d’une instruction **Merge** .<br /><br /> 3 indique un plan de déclencheur **Delete** qui s’exécute à la suite d’une instruction **Merge** contenant une action **Insert** ou **Update** correspondante.<br /><br /> <br /><br /> Pour les déclencheurs imbriqués exécutés par des actions en cascade, cette valeur est l’action de l’instruction **Merge** qui a provoqué la cascade.|  
|**default_schema_id**|**int**|ID du schéma par défaut, utilisé pour résoudre les noms qui ne sont pas qualifiés complets.|  
|**is_replication_specific**|**bit**|Utilisé pour la réplication.|  
|**is_contained**|**varbinary (1)**|1 indique une base de données à relation contenant-contenu.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **View Database State** .  
  
## <a name="see-also"></a>Voir aussi  
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)   
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Procédures stockées du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  
