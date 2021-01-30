---
description: sp_check_join_filter (Transact-SQL)
title: sp_check_join_filter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- filter_TSQL
- sp_check_TSQL
- sp_check_join_filter
- sp_check_join_filter_TSQL
- join
- join_TSQL
- filter
- sp_check
helpviewer_keywords:
- sp_check_join_filter
ms.assetid: e9699d59-c8c9-45f6-a561-f7f95084a540
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 74e4114eca77cbbac1c2e2c2471fb0ee10a55ae1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206556"
---
# <a name="sp_check_join_filter-transact-sql"></a>sp_check_join_filter (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  S'utilise pour vérifier un filtre de jointure entre deux tables pour déterminer si la clause du filtre est valide. Cette procédure stockée renvoie également des informations sur le filtre de jointure fourni, y compris s'il est possible de l'utiliser avec des partitions précalculées pour la table donnée. Cette procédure stockée est exécutée sur la base de données du serveur de publication. Pour plus d’informations, consultez [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_check_join_filter [ @filtered_table = ] 'filtered_table'  
        , [@join_table = ] 'join_table'  
        , [ @join_filterclause = ] 'join_filterclause'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @filtered_table = ] 'filtered_table'` Nom d’une table filtrée. *filtered_table* est de type **nvarchar (400)**, sans valeur par défaut.  
  
`[ @join_table = ] 'join_table'` Nom d’une table jointe à *filtered_table*. *join_table* est de type **nvarchar (400)**, sans valeur par défaut.  
  
`[ @join_filterclause = ] 'join_filterclause'` Clause de filtre de jointure en cours de test. *join_filterclause* est de type **nvarchar (1000)**, sans valeur par défaut.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|Indique si la publication est qualifiée pour les partitions précalculées ; où **1** signifie que les partitions precomupted peuvent être utilisées, et **0** signifie qu’elles ne peuvent pas être utilisées.|  
|**has_dynamic_filters**|**bit**|Indique si la clause de filtre fournie comprend au moins une fonction de filtrage paramétrée ; où **1** signifie qu’une fonction de filtrage paramétrable est utilisée, et **0** signifie qu’une telle fonction n’est pas utilisée.|  
|**dynamic_filters_function_list**|**nvarchar (500)**|Liste des fonctions de la clause du filtre qui définissent un filtrage paramétré pour un article. Chaque fonction est séparée par un point-virgule.|  
|**uses_host_name**|**bit**|Si la fonction [HOST_NAME ()](../../t-sql/functions/host-name-transact-sql.md) est utilisée dans la clause de filtre, où **1** indique que cette fonction est présente.|  
|**uses_suser_sname**|**bit**|Si la fonction [SUSER_SNAME ()](../../t-sql/functions/suser-sname-transact-sql.md) est utilisée dans la clause de filtre, où **1** indique que cette fonction est présente.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_check_join_filter** est utilisé dans la réplication de fusion.  
  
 **sp_check_join_filter** peut être exécutée sur toutes les tables associées, même si elles ne sont pas publiées. Cette procédure stockée peut être utilisée pour vérifier une clause de filtre de jointure avant de définir un filtre de jointure entre deux articles.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_check_join_filter**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
