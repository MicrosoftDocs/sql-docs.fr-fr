---
description: sp_changemergefilter (Transact-SQL)
title: sp_changemergefilter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergefilter_TSQL
- sp_changemergefilter
helpviewer_keywords:
- sp_changemergefilter
ms.assetid: e08fdfdd-d242-4e85-817b-9f7a224fe567
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8081fc9a95452d68540acf3b2a5a412910f75219
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159848"
---
# <a name="sp_changemergefilter-transact-sql"></a>sp_changemergefilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie certaines propriétés du filtre de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changemergefilter [ @publication= ] 'publication'  
        , [ @article= ] 'article'  
        , [ @filtername= ] 'filtername'  
        , [ @property= ] 'property'  
        , [ @value= ] 'value'  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @article = ] 'article'` Nom de l’article. *article* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @filtername = ] 'filtername'` Nom actuel du filtre. *FilterName* est de **type sysname**, sans valeur par défaut.  
  
`[ @property = ] 'property'` Nom de la propriété à modifier. *Property* est de **type sysname**, sans valeur par défaut.  
  
`[ @value = ] 'value'` Nouvelle valeur de la propriété spécifiée. la *valeur* est de type **nvarchar (1000)**, sans valeur par défaut.  
  
 Le tableau ci-dessous décrit les propriétés des articles et les valeurs de ces propriétés.  
  
|Propriété|Valeur|Description|  
|--------------|-----------|-----------------|  
|**filter_type**|**1**|Filtre de jointure.<br /><br /> Cette option est nécessaire pour la prise en charge d'Abonnés [!INCLUDE[ssEW](../../includes/ssew-md.md)].|  
||**2**|Relation d'enregistrement logique.|  
||**3**|Un filtre de jointure est également une relation d'enregistrement logique.|  
|**FilterName**||Nom du filtre.|  
|**join_articlename**||Nom de l'article de jointure.|  
|**join_filterclause**||Clause Filtre.|  
|**join_unique_key**|**true**|La jointure se fait sur une clé unique.|  
||**false**|La jointure ne se fait pas sur une clé unique.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirme que l’action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bit**, avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article de fusion n’entraînent pas la non-validité de l’instantané. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** signifie que les modifications apportées à l’article de fusion peuvent entraîner la non-validité de l’instantané, et s’il existe des abonnements qui nécessitent un nouvel instantané, accorde l’autorisation de marquer l’instantané existant comme obsolète et de générer un nouvel instantané.  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bit** avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article de fusion n’entraînent pas la réinitialisation de l’abonnement. Si la procédure stockée détecte que la modification nécessite la réinitialisation des abonnements existants, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** signifie que les modifications apportées à l’article de fusion entraînent la réinitialisation des abonnements existants et autorisent la réinitialisation de l’abonnement.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changemergefilter** est utilisé dans la réplication de fusion.  
  
 La modification d'un filtre sur un article de fusion nécessite de recréer l'instantané, s'il existe. Pour ce faire, affectez la valeur **1** à la **\@ force_invalidate_snapshot** . De même, s'il existe des abonnements à cet article, les abonnements doivent être réinitialisés. Pour ce faire, affectez la valeur **1** à la **\@ force_reinit_subscription** .  
  
 Pour utiliser des enregistrements logiques, la publication et les articles doivent répondre à certaines conditions. Pour plus d’informations, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_changemergefilter**.  
  
## <a name="see-also"></a>Voir aussi  
 [Changer les propriétés des publications et des articles](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)   
 [sp_dropmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [sp_helpmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergefilter-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
