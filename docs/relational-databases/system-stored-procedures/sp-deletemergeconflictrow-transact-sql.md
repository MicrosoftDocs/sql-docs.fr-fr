---
description: sp_deletemergeconflictrow (Transact-SQL)
title: sp_deletemergeconflictrow (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_deletemergeconflictrow
- sp_deletemergeconflictrow_TSQL
helpviewer_keywords:
- sp_deletemergeconflictrow
ms.assetid: 64cf1186-28b8-4cd9-88f1-a7808a9c8d60
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 392d64a854db224542438f28d821a3ade597bdc8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203692"
---
# <a name="sp_deletemergeconflictrow-transact-sql"></a>sp_deletemergeconflictrow (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime des lignes d’une table de conflits ou le [MSmerge_conflicts_info &#40;table Transact-SQL&#41;](../../relational-databases/system-tables/msmerge-conflicts-info-transact-sql.md) . Cette procédure stockée est exécutée dans n'importe quelle base de données de l'ordinateur sur lequel la table de conflits est stockée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_deletemergeconflictrow [ [ @conflict_table = ] 'conflict_table' ]  
    [ , [ @source_object = ] 'source_object' ]  
    { , [ @rowguid = ] 'rowguid'  
        , [ @origin_datasource = ] 'origin_datasource' ] }  
    [ , [ @drop_table_if_empty = ] 'drop_table_if_empty' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @conflict_table = ] 'conflict_table'` Nom de la table de conflits. *conflict_table* est de **type sysname**, avec la valeur par défaut **%** . Si le *conflict_table* est spécifié comme null ou **%** , il est supposé qu’il s’agit d’un conflit de suppression et que la ligne qui correspond à *rowguid* et à *origin_datasource* et à *source_object* est supprimée de la table [MSmerge_conflicts_info &#40;Transact-SQL&#41;](../../relational-databases/system-tables/msmerge-conflicts-info-transact-sql.md) .  
  
`[ @source_object = ] 'source_object'` Nom de la table source. *source_object* est de type **nvarchar (386)**, avec NULL comme valeur par défaut.  
  
`[ @rowguid = ] 'rowguid'` Identificateur de ligne pour le conflit de suppression. *rowguid* est de type **uniqueidentifier** et n’a pas de valeur par défaut.  
  
`[ @origin_datasource = ] 'origin_datasource'` Est l’origine du conflit. *origin_datasource* est de type **varchar (255)**, sans valeur par défaut.  
  
`[ @drop_table_if_empty = ] 'drop_table_if_empty'` Indicateur qui spécifie que le *conflict_table* doit être supprimé si est vide. *drop_table_if_empty* est de type **varchar (10)**, avec false comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_deletemergeconflictrow** est utilisé dans la réplication de fusion.  
  
 [MSmerge_conflicts_info &#40;table&#41;Transact-SQL ](../../relational-databases/system-tables/msmerge-conflicts-info-transact-sql.md) est une table système et n’est pas supprimée de la base de données, même si elle est vide.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_deletemergeconflictrow**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
