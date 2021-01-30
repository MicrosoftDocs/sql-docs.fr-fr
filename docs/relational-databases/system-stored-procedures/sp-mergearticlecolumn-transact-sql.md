---
description: sp_mergearticlecolumn (Transact-SQL)
title: sp_mergearticlecolumn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_mergearticlecolumn
- sp_mergearticlecolumn_TSQL
helpviewer_keywords:
- sp_mergearticlecolumn
ms.assetid: b4f2b888-e094-4759-a472-d893638995eb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b5d8d2f63afd421081058dd1b53ec8084f4b5e13
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185316"
---
# <a name="sp_mergearticlecolumn-transact-sql"></a>sp_mergearticlecolumn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Partitionne une publication de fusion verticalement. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_mergearticlecolumn [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @column = ] 'column'  
    [ , [ @operation = ] 'operation'   
    [ , [ @schema_replication = ] 'schema_replication' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]   
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *Publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @article = ] 'article'` Nom de l’article dans la publication. *article* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @column = ] 'column'` Identifie les colonnes sur lesquelles la partition verticale doit être créée. *Column* est de **type sysname**, avec NULL comme valeur par défaut. Si la valeur NULL est spécifiée et que `@operation = N'add'`, toutes les colonnes de la table source sont ajoutées à l'article par défaut. la *colonne* ne peut pas être NULL lorsque l' *opération* est définie sur **Drop**. Pour exclure des colonnes d’un article, exécutez **sp_mergearticlecolumn** et spécifiez la *colonne* et `@operation = N'drop'` pour chaque colonne à supprimer de l' *article* spécifié.  
  
`[ @operation = ] 'operation'` État de la réplication. *operation* est de type **nvarchar (4)**, avec Add comme valeur par défaut. **Ajouter** marque la colonne pour la réplication. **Drop** efface la colonne.  
  
`[ @schema_replication = ] 'schema_replication'` Spécifie qu’une modification de schéma sera propagée lors de l’exécution du Agent de fusion. *schema_replication* est de type **nvarchar (5)**, avec false comme valeur par défaut.  
  
> [!NOTE]  
>  Seule la **valeur false** est prise en charge pour *schema_replication*.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Active ou désactive la possibilité d’invalider un instantané. *force_invalidate_snapshot* est un **bit**, avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article de fusion n’entraînent pas la non-validité de l’instantané.  
  
 **1** indique que les modifications apportées à l’article de fusion peuvent entraîner la non-validité de l’instantané et, si tel est le cas, la valeur **1** accorde l’autorisation de se produire pour le nouvel instantané.  
  
`[ @force_reinit_subscription = ]force_reinit_subscription_` Active ou désactive la possibilité d’avoir l’abonnement réinitialiser. *force_reinit_subscription* est un bit avec **0** comme valeur par défaut.  
  
 **0** indique que les modifications apportées à l’article de fusion n’entraînent pas la réinitialisation de l’abonnement.  
  
 **1** indique que les modifications apportées à l’article de fusion peuvent entraîner la réinitialisation de l’abonnement. Si tel est le cas, la valeur **1** accorde l’autorisation de réinitialisation de l’abonnement.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_mergearticlecolumn** est utilisé dans la réplication de fusion.  
  
 La colonne d'identité ne peut pas être supprimée de l'article si la gestion automatique des plages d'identité est utilisée. Pour plus d’informations, consultez [ Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
 Si une application définit une nouvelle partition verticale après la création de l'instantané initial, un nouvel instantané doit être généré et réappliqué à chaque abonnement. Les instantanés sont appliqués lors de l'exécution de l'instantané planifié suivant et de l'Agent de distribution ou de fusion.  
  
 Si le suivi de lignes est utilisé pour la détection de conflits (valeur par défaut), la table de base peut inclure 1 024 colonnes au maximum, mais les colonnes doivent être filtrées à partir de l'article afin que 246 colonnes au maximum soient publiées. Si le suivi de colonnes est utilisé, la table de base peut inclure 246 colonnes au maximum.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_AddMergeArticle](../../relational-databases/replication/codesnippet/tsql/sp-mergearticlecolumn-tr_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_mergearticlecolumn**.  
  
## <a name="see-also"></a>Voir aussi  
 [Définir et modifier un filtre de jointure entre des articles de fusion](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Filtrer des données publiées](../../relational-databases/replication/publish/filter-published-data.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
