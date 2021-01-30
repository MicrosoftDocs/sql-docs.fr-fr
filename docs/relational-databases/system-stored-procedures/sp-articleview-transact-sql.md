---
description: sp_articleview (Transact-SQL)
title: sp_articleview (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_articleview
- sp_articleview_TSQL
helpviewer_keywords:
- sp_articleview
ms.assetid: a3d63fd6-f360-4a2f-8a82-a0dc15f650b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 014ccf37727534d16fa5b16bc280da5b76dfb76e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203201"
---
# <a name="sp_articleview-transact-sql"></a>sp_articleview (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée la vue qui définit l'article publié lorsqu'une table est filtrée verticalement ou horizontalement. Cette vue est utilisée comme source filtrée du schéma et des données des tables de destination. Seuls les articles ne faisant pas l'objet d'un abonnement peuvent être modifiés par cette procédure stockée. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_articleview [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @view_name = ] 'view_name']  
    [ , [ @filter_clause = ] 'filter_clause']  
    [ , [ @change_active = ] change_active ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @refreshsynctranprocs = ] refreshsynctranprocs ]  
    [ , [ @internal = ] internal ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication qui contient l’article. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @article = ] 'article'` Nom de l’article. *article* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @view_name = ] 'view_name'` Nom de la vue qui définit l’article publié. *view_name* est de type **nvarchar (386)**, avec NULL comme valeur par défaut.  
  
`[ @filter_clause = ] 'filter_clause'` Clause de restriction (WHERE) qui définit un filtre horizontal. Quand vous entrez la clause de restriction, omettez le mot clé WHERE. *filter_clause* est de type **ntext**, avec NULL comme valeur par défaut.  
  
`[ @change_active = ] change_active` Autorise la modification des colonnes dans les publications qui ont des abonnements. *change_active* est de **type int**, avec **0** comme valeur par défaut. Si la **valeur est 0**, les colonnes ne sont pas modifiées. Si la fonction est **1**, des vues peuvent être créées ou recréées sur des articles actifs qui ont des abonnements.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirme que l’action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bit**, avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article n’entraînent pas la non-validité de l’instantané. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article peuvent entraîner la non-validité de l’instantané, et s’il existe des abonnements qui nécessitent un nouvel instantané, donne l’autorisation de marquer l’instantané existant comme obsolète et de générer un nouvel instantané.  
  
`[ @force_reinit_subscription = ] _force_reinit_subscription_` Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bit** avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article n’entraînent pas la réinitialisation de l’abonnement. Si la procédure stockée détecte que la modification requiert la réinitialisation des abonnements, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article entraînent la réinitialisation de l’abonnement existant et accorde l’autorisation de réinitialisation de l’abonnement.  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé lors de la publication à partir d’un serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @refreshsynctranprocs = ] refreshsynctranprocs` Indique si les procédures stockées utilisées pour synchroniser la réplication sont automatiquement recréées. *refreshsynctranprocs* est de **bits**, avec 1 comme valeur par défaut.  
  
 **1** signifie que les procédures stockées sont recréées.  
  
 **0** signifie que les procédures stockées ne sont pas recréées.  
  
`[ @internal = ] internal` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_articleview** crée la vue qui définit l’article publié et insère l’ID de cette vue dans la **sync_objid** colonne de la table [sysarticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md) , puis insère le texte de la clause de restriction dans la colonne **filter_clause** . Si toutes les colonnes sont répliquées et qu’il n’y a pas de **filter_clause**, la **sync_objid** de la table [sysarticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md) est définie sur l’ID de la table de base et l’utilisation de **sp_articleview** n’est pas nécessaire.  
  
 Pour publier une table filtrée verticalement (autrement dit, pour filtrer les colonnes), exécutez d’abord **sp_addarticle** sans paramètre *sync_object* , exécutez [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) une fois pour chaque colonne à répliquer (en définissant le filtre vertical), puis exécutez **sp_articleview** pour créer la vue qui définit l’article publié.  
  
 Pour publier une table filtrée horizontalement (c’est-à-dire, pour filtrer des lignes), exécutez [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) sans paramètre de *filtre* . Exécutez [sp_articlefilter &#40;&#41;Transact-SQL](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md), en fournissant tous les paramètres, y compris *filter_clause*. Exécutez ensuite **sp_articleview**, en fournissant tous les paramètres, y compris les *filter_clause* identiques.  
  
 Pour publier une table filtrée verticalement et horizontalement, exécutez [sp_addarticle &#40;&#41;Transact-SQL](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) sans *sync_object* ni paramètres de *filtre* . Exécutez [sp_articlecolumn &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) une fois pour chaque colonne à répliquer, puis exécutez [sp_articlefilter &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) et **sp_articleview**.  
  
 Si l’article a déjà une vue qui définit l’article publié, **sp_articleview** supprime la vue existante et en crée une nouvelle automatiquement. Si la vue a été créée manuellement (**type** dans [sysarticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md) est **5**), la vue existante n’est pas supprimée.  
  
 Si vous créez une procédure stockée de filtre personnalisée et une vue qui définit l’article publié manuellement, n’exécutez pas **sp_articleview**. Au lieu de cela, fournissez-les en tant que paramètres de *filtre* et de *sync_object* pour [SP_ADDARTICLE &#40;&#41;Transact-SQL](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md), ainsi que la valeur de *type* appropriée.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-articleview-transact-_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_articleview**.  
  
## <a name="see-also"></a>Voir aussi  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [Définir et modifier un filtre de lignes statique](../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
