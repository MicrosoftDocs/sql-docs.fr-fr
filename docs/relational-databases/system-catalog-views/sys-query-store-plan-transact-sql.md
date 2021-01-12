---
description: sys.query_store_plan (Transact-SQL)
title: sys.query_store_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- QUERY_STORE_PLAN_TSQL
- SYS.QUERY_STORE_PLAN
- SYS.QUERY_STORE_PLAN_TSQL
- QUERY_STORE_PLAN
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_plan catalog view
- sys.query_store_plan catalog view
ms.assetid: b4d05439-6360-45db-b1cd-794f4a64935e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1b13c6f42eb967c6ef5d01e189860c9de3204646
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095465"
---
# <a name="sysquery_store_plan-transact-sql"></a>sys.query_store_plan (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Contient des informations sur chaque plan d’exécution associé à une requête.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|
|**plan_id**|**bigint**|Clé primaire|  
|**query_id**|**bigint**|Clé étrangère. Jointures à [sys.query_store_query &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md).|  
|**plan_group_id**|**bigint**|ID du groupe de plans. Les requêtes de curseur nécessitent généralement plusieurs plans (remplissage et extraction). Les plans de remplissage et de récupération qui sont compilés ensemble se trouvent dans le même groupe.<br /><br /> 0 signifie que le plan n’est pas dans un groupe.|  
|**engine_version**|**nvarchar(32)**|Version du moteur utilisée pour compiler le plan dans le format **« major. mineure. Build. Revision »** .|  
|**compatibility_level**|**smallint**|Niveau de compatibilité de base de données de la base de données référencée dans la requête.|  
|**query_plan_hash**|**Binary(8**|Hachage MD5 du plan individuel.|  
|**query_plan**|**nvarchar(max)**|Showplan XML pour le plan de requête.|  
|**is_online_index_plan**|**bit**|Le plan a été utilisé lors de la création d’un index en ligne. <br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**is_trivial_plan**|**bit**|Le plan est un plan trivial (sortie à l’étape 0 de l’optimiseur de requête). <br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**is_parallel_plan**|**bit**|Le plan est parallèle. <br/>**Remarque :** Azure Synapse Analytics retourne toujours un (1).|  
|**is_forced_plan**|**bit**|Le plan est marqué comme forcé lorsque l’utilisateur exécute la procédure stockée **sys.sp_query_store_force_plan**. Le mécanisme forcé *ne garantit pas* que exactement ce plan sera utilisé pour la requête référencée par **query_id**. Le forçage de plan force la compilation de la requête et produit en général exactement le même plan ou un plan similaire au plan référencé par **plan_id**. Si le forçage du plan échoue, **force_failure_count** est incrémenté et **last_force_failure_reason** est rempli avec la raison de l’échec. <br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**is_natively_compiled**|**bit**|Le plan comprend des procédures optimisées en mémoire compilées en mode natif. (0 = FALSE, 1 = TRUE). <br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**force_failure_count**|**bigint**|Nombre de fois où l’application forcée de ce plan a échoué. Il peut être incrémenté uniquement lorsque la requête est recompilée (*pas à chaque exécution*). Elle est réinitialisée à 0 chaque fois que **is_plan_forced** passe de **false** à **true**. <br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**last_force_failure_reason**|**int**|Raison pour laquelle le forçage du plan a échoué.<br /><br /> 0 : aucun échec, sinon le numéro d’erreur de l’erreur qui a provoqué l’échec de l’application<br /><br /> 8637 : ONLINE_INDEX_BUILD<br /><br /> 8683 : INVALID_STARJOIN<br /><br /> 8684 : TIME_OUT<br /><br /> 8689 : NO_DB<br /><br /> 8690 : HINT_CONFLICT<br /><br /> 8691 : SETOPT_CONFLICT<br /><br /> 8694 : DQ_NO_FORCING_SUPPORTED<br /><br /> 8698 : NO_PLAN<br /><br /> 8712 : NO_INDEX<br /><br /> 8713 : VIEW_COMPILE_FAILED<br /><br /> \<other value>: GENERAL_FAILURE <br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**last_force_failure_reason_desc**|**nvarchar(128)**|Description textuelle de last_force_failure_reason_desc.<br /><br /> ONLINE_INDEX_BUILD : une requête tente de modifier des données alors que la table cible a un index en cours de création en ligne<br /><br /> INVALID_STARJOIN : le plan contient une spécification StarJoin non valide<br /><br /> TIME_OUT : l’optimiseur a dépassé le nombre d’opérations autorisées lors de la recherche du plan spécifié par le plan forcé<br /><br /> NO_DB : aucune base de données spécifiée dans le plan n’existe<br /><br /> HINT_CONFLICT : impossible de compiler la requête, car le plan est en conflit avec un indicateur de requête<br /><br /> DQ_NO_FORCING_SUPPORTED : impossible d’exécuter la requête, car le plan est en conflit avec l’utilisation de la requête distribuée ou des opérations de texte intégral.<br /><br /> NO_PLAN : le processeur de requêtes n’a pas pu générer le plan de requête car le plan forcé n’a pas pu être vérifié pour la requête<br /><br /> NO_INDEX : l’index spécifié dans le plan n’existe plus<br /><br /> VIEW_COMPILE_FAILED : impossible de forcer le plan de requête en raison d’un problème dans une vue indexée référencée dans le plan<br /><br /> GENERAL_FAILURE : erreur de forçage général (non couverte par les raisons ci-dessus) <br/>**Remarque :** Azure Synapse Analytics ne retourne jamais *aucun*.|  
|**count_compiles**|**bigint**|Planifiez les statistiques de compilation.|  
|**initial_compile_start_time**|**datetimeoffset**|Planifiez les statistiques de compilation.|  
|**last_compile_start_time**|**datetimeoffset**|Planifiez les statistiques de compilation.|  
|**last_execution_time**|**datetimeoffset**|La dernière heure d’exécution fait référence à la dernière heure de fin de la requête ou du plan.|  
|**avg_compile_duration**|**float**|Planifiez les statistiques de compilation. <br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**last_compile_duration**|**bigint**|Planifiez les statistiques de compilation. <br/>**Remarque :** Azure Synapse Analytics retourne toujours la valeur zéro (0).|  
|**plan_forcing_type**|**int**|Type de forçage du plan.<br /><br />0 : AUCUN<br /><br />1 : MANUEL<br /><br />2 : AUTO|  
|**plan_forcing_type_desc**|**nvarchar(60)**|Description textuelle de plan_forcing_type.<br /><br />AUCUN : aucun plan forcé<br /><br />Manuel : plan forcé par l’utilisateur<br /><br />AUTO : plan forcé par réglage automatique|  

## <a name="plan-forcing-limitations"></a>Limites de l'application forcée d'un plan
Le Magasin des requêtes a un mécanisme qui permet de forcer l’optimiseur de requête à utiliser un certain plan d’exécution. Toutefois, il existe certaines limitations qui peuvent empêcher l’application d’un plan. 

Premièrement, si le plan contient les constructions suivantes :
* Instruction Insert bulk.
* Référence à une table externe
* Requête distribuée ou opérations de recherche en texte intégral
* Utilisation de requêtes globales 
* Curseurs dynamiques ou de jeu de clés 
* Spécification de jointure en étoile non valide 

> [!NOTE]
> Azure SQL Database et SQL Server plan de support 2019 forcé pour les curseurs de transfert statique et rapide.

Deuxièmement, quand les objets sur lesquels s’appuie le plan ne sont plus disponibles :
* Base de données (si la base de données d'où provient le plan n’existe plus)
* Index (absent ou désactivé)

Enfin, s’il y a des problèmes avec le plan lui-même :
* Non conforme pour la requête
* L’optimiseur de requête a dépassé le nombre d’opérations autorisées
* Code XML du plan incorrect

## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **View Database State** .  
  
## <a name="see-also"></a>Voir aussi  
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Procédures stockées du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
  
  
