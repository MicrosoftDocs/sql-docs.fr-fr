---
description: sp_restoremergeidentityrange (Transact-SQL)
title: sp_restoremergeidentityrange (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_restoremergeidentityrange_TSQL
- sp_restoremergeidentityrange
helpviewer_keywords:
- sp_restoremergeidentityrange
ms.assetid: 7923e422-2748-40c0-b5a8-6410c48d5b70
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2886b4b93331ed59f1943b5625ee673df64b537d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205108"
---
# <a name="sp_restoremergeidentityrange-transact-sql"></a>sp_restoremergeidentityrange (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette procédure stockée permet de mettre à jour les affectations de plage d'identité. Elle garantit le bon fonctionnement de la gestion automatique des plages d'identité lorsqu'un serveur de publication a été restauré à partir d'une sauvegarde. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_restoremergeidentityrange [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec **All** comme valeur par défaut. Lorsque cet argument est spécifié, seules les plages d'identité de la publication correspondante sont restaurées.  
  
`[ @article = ] 'article'` Nom de l’article. *article* est de **type sysname**, avec **All** comme valeur par défaut. Lorsque cet argument est spécifié, seules les plages d'identité de l'article correspondant sont restaurées.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_restoremergeidentityrange** est utilisé avec la réplication de fusion.  
  
 **sp_restoremergeidentityrange** obtient les informations d’allocation de plage d’identité maximale du serveur de distribution et met à jour les valeurs dans la colonne **max_used** de [MSmerge_identity_range_allocations &#40;Transact-SQL&#41;](../../relational-databases/system-tables/msmerge-identity-range-allocations-transact-sql.md) pour les articles qui utilisent la gestion automatique des plages d’identité.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_restoremergeidentityrange**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md)  
  
  
