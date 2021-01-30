---
description: sp_check_subset_filter (Transact-SQL)
title: sp_check_subset_filter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_check_TSQL
- sp_check_subset_filter
- filter
- subset
- subset_TSQL
- sp_check
- sp_check_subset_filter_TSQL
helpviewer_keywords:
- sp_check_subset_filter
ms.assetid: 525cfcfc-f317-478d-ba84-72e62285f160
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6d311670fca8b5514057bd122980e8647566240e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174541"
---
# <a name="sp_check_subset_filter-transact-sql"></a>sp_check_subset_filter (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Est utilisée pour vérifier une clause de filtre en fonction d'une table, afin de déterminer si cette clause est valide pour la table. Cette procédure stockée renvoie des informations sur le filtre fourni, y compris si le filtre peut être utilisé avec des partitions précalculées. Cette procédure stockée est exécutée au niveau du serveur de publication dans la base de données contenant la publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_check_subset_filter [ @filtered_table = ] 'filtered_table'  
        , [ @subset_filterclause = ] 'subset_filterclause'  
    [ , [ @has_dynamic_filters = ] has_dynamic_filters OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @filtered_table = ] 'filtered_table'` Nom d’une table filtrée. *filtered_table* est de type **nvarchar (400)**, sans valeur par défaut.  
  
`[ @subset_filterclause = ] 'subset_filterclause'` Clause de filtre testée. *subset_filterclause* est de type **nvarchar (1000)**, sans valeur par défaut.  
  
`[ @has_dynamic_filters = ] has_dynamic_filters` Indique si la clause de filtre est un filtre de lignes paramétrable. *has_dynamic_filters* est de type **bit**, avec NULL comme valeur par défaut et est un paramètre de sortie. Retourne la valeur **1** lorsque la clause de filtre est un filtre de lignes paramétrable.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|Indique si la publication est qualifiée pour l’utilisation de partitions précalculées ; où **1** signifie que les partitions précalculées peuvent être utilisées, et **0** signifie qu’elles ne peuvent pas être utilisées.|  
|**has_dynamic_filters**|**bit**|Indique si la clause de filtre fournie comprend au moins un filtre de lignes paramétrable ; où **1** signifie qu’un filtre de lignes paramétrable est utilisé, et **0** signifie qu’une telle fonction n’est pas utilisée.|  
|**dynamic_filters_function_list**|**nvarchar (500)**|Listes des fonctions de la clause de filtre qui filtrent dynamiquement un article, où chaque fonction est séparée par un point-virgule.|  
|**uses_host_name**|**bit**|Si la fonction [HOST_NAME ()](../../t-sql/functions/host-name-transact-sql.md) est utilisée dans la clause de filtre, où **1** indique que cette fonction est présente.|  
|**uses_suser_sname**|**bit**|Si la fonction [SUSER_SNAME ()](../../t-sql/functions/suser-sname-transact-sql.md) est utilisée dans la clause de filtre, où **1** indique que cette fonction est présente.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_check_subset_filter** est utilisé dans la réplication de fusion.  
  
 **sp_check_subset_filter** peut être exécutée sur n’importe quelle table, même si la table n’est pas publiée. Cette procédure stockée peut être utilisée pour vérifier une clause de filtre avant de définir un article filtré.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_check_subset_filter**.  
  
## <a name="see-also"></a>Voir aussi  
 [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)  
  
  
