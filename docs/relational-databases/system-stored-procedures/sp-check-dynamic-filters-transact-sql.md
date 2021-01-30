---
description: sp_check_dynamic_filters (Transact-SQL)
title: sp_check_dynamic_filters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- dynamic_filters_TSQL
- sp_check_TSQL
- check
- sp_check_dynamic filter
- check_TSQL
- filters_TSQL
- check_dynamic_filters_TSQL
- dynamic filters
- filters
- check dynamic filters
- sp_check_dynamic filter_TSQL
- sp_check_for_sync_trigger_TSQL
- sp_check
helpviewer_keywords:
- sp_check_dynamic_filters
ms.assetid: dd7760db-a3a5-460f-bd97-b8d436015e19
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1f458f3dcf629ee6f77b8237edb0a9defa516b8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158954"
---
# <a name="sp_check_dynamic_filters-transact-sql"></a>sp_check_dynamic_filters (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Affiche des informations sur les propriétés du filtre de lignes paramétrable d'une publication, notamment les fonctions utilisées pour générer une partition de données filtrées pour une publication, et indique si la publication peut utiliser des partitions précalculées. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_check_dynamic_filters [ @publication = ] 'publication'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|Indique si la publication est qualifiée pour l’utilisation de partitions précalculées ; où **1** signifie que les partitions précalculées peuvent être utilisées, et **0** signifie qu’elles ne peuvent pas être utilisées.|  
|**has_dynamic_filters**|**bit**|Si au moins un filtre de lignes paramétrable a été défini dans la publication ; où **1** signifie qu’il existe un ou plusieurs filtres de lignes paramétrés, et **0** signifie qu’il n’existe aucun filtre dynamique.|  
|**dynamic_filters_function_list**|**nvarchar (500)**|Indique les fonctions utilisées pour filtrer les articles dans une publication, chaque fonction étant séparée par un point-virgule.|  
|**validate_subscriber_info**|**nvarchar (500)**|Indique les fonctions utilisées pour filtrer les articles dans une publication, chaque fonction étant séparée par un signe plus (+).|  
|**uses_host_name**|**bit**|Si la fonction [HOST_NAME ()](../../t-sql/functions/host-name-transact-sql.md) est utilisée dans les filtres de lignes paramétrables, où **1** indique que cette fonction est utilisée pour le filtrage dynamique.|  
|**uses_suser_sname**|**bit**|Si la fonction [SUSER_SNAME ()](../../t-sql/functions/suser-sname-transact-sql.md) est utilisée dans les filtres de lignes paramétrables, où **1** indique que cette fonction est utilisée pour le filtrage dynamique.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_check_dynamic_filters** est utilisé dans la réplication de fusion.  
  
 Si une publication a été définie pour utiliser des partitions précalculées, **sp_check_dynamic_filters** vérifie toute violation des restrictions des partitions précalculées. S'il en existe, une erreur est renvoyée. Pour plus d’informations, consultez [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
 Si une publication est définie comme ayant des filtres de lignes paramétrables et qu'aucun filtrage n'est trouvé, une erreur est renvoyée.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_check_dynamic_filters**.  
  
## <a name="see-also"></a>Voir aussi  
 [Gérer les partitions d’une publication de fusion avec des filtres paramétrés](../../relational-databases/replication/publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)   
 [sp_check_join_filter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-check-join-filter-transact-sql.md)   
 [sp_check_subset_filter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-check-subset-filter-transact-sql.md)  
  
  
