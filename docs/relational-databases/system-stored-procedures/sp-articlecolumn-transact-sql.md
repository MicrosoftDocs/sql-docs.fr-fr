---
description: sp_articlecolumn (Transact-SQL)
title: sp_articlecolumn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_articlecolumn
- sp_articlecolumn_TSQL
helpviewer_keywords:
- sp_articlecolumn
ms.assetid: 8abaa8c1-d99e-4788-970f-c4752246c577
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da57a5aa4ae6645c7b1b2ff6377b79160fa110c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203205"
---
# <a name="sp_articlecolumn-transact-sql"></a>sp_articlecolumn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de spécifier les colonnes incluses dans un article pour filtrer verticalement des données dans une table publiée. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_articlecolumn [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @column = ] 'column' ]  
    [ , [ @operation = ] 'operation' ]  
    [ , [ @refresh_synctran_procs = ] refresh_synctran_procs ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @change_active = ] change_actve ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @internal = ] 'internal' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication qui contient cet article. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @article = ] 'article'` Nom de l’article. *article* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @column = ] 'column'` Nom de la colonne à ajouter ou à supprimer. *Column* est de **type sysname**, avec NULL comme valeur par défaut. Si la valeur est NULL, toutes les colonnes sont publiées.  
  
`[ @operation = ] 'operation'` Spécifie s’il faut ajouter ou supprimer des colonnes dans un article. *operation* est de type **nvarchar (5)**, avec Add comme valeur par défaut. **Ajouter** marque la colonne pour la réplication. **Drop** décoche la colonne.  
  
`[ @refresh_synctran_procs = ] refresh_synctran_procs` Spécifie si les procédures stockées prenant en charge les abonnements mis à jour immédiatement sont régénérées pour correspondre au nombre de colonnes répliquées. *refresh_synctran_procs* est de **bits**, avec **1** comme valeur par défaut. Si la condition est **1**, les procédures stockées sont régénérées.  
  
`[ @ignore_distributor = ] ignore_distributor` Indique si cette procédure stockée s’exécute sans connexion au serveur de distribution. *ignore_distributor* est de **bit**, avec **0** comme valeur par défaut. Si la **valeur est 0**, la base de données doit être activée pour la publication et le cache de l’article doit être actualisé pour refléter les nouvelles colonnes répliquées par l’article. Si la valeur est **1**, les colonnes de l’article peuvent être supprimées pour les articles qui résident dans une base de données non publiée ; doit être utilisé uniquement dans les situations de récupération.  
  
`[ @change_active = ] change_active` Autorise la modification des colonnes dans les publications qui ont des abonnements. *change_active* est de **type int** avec **0** comme valeur par défaut. Si la **valeur est 0**, les colonnes ne sont pas modifiées. Si la colonne est **1**, les colonnes peuvent être ajoutées ou supprimées des articles actifs qui ont des abonnements.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirme que l’action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bit**, avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article n’entraînent pas la non-validité de l’instantané. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article peuvent entraîner la non-validité de l’instantané, et s’il existe des abonnements qui nécessitent un nouvel instantané, donne l’autorisation de marquer l’instantané existant comme obsolète et de générer un nouvel instantané.  
  
 [**@force_reinit_subscription =** ] *force_reinit_subscription*  
 Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bit**, avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article n’entraînent pas la réinitialisation de l’abonnement. Si la procédure stockée détecte que la modification requiert la réinitialisation des abonnements, une erreur se produit et aucune modification n'est effectuée. **1** indique que les modifications apportées à l’article entraînent la réinitialisation des abonnements existants et autorise la réinitialisation de l’abonnement.  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé avec un serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication.  
  
`[ @internal = ] 'internal'` À usage interne uniquement.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_articlecolumn** est utilisé dans la réplication d’instantané et la réplication transactionnelle.  
  
 Seul un article non abonné peut être filtré à l’aide de **sp_articlecolumn**.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-articlecolumn-transac_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_articlecolumn**.  
  
## <a name="see-also"></a>Voir aussi  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [Définir et modifier un filtre de colonne](../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)   
 [Filtrer des données publiées](../../relational-databases/replication/publish/filter-published-data.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articleview &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
