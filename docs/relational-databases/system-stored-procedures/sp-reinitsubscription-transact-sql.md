---
description: sp_reinitsubscription (Transact-SQL)
title: sp_reinitsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_reinitsubscription
- sp_reinitsubscription_TSQL
helpviewer_keywords:
- sp_reinitsubscription
ms.assetid: d56ae218-6128-4ff9-b06c-749914505c7b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0d8c12b3ca1b27f7560aeff466e51b7f43a9a42e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185667"
---
# <a name="sp_reinitsubscription-transact-sql"></a>sp_reinitsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Marque l'abonnement pour la réinitialisation. Cette procédure stockée est exécutée sur le serveur de publication pour les abonnements par envoi de données (push).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_reinitsubscription [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
        , [ @subscriber = ] 'subscriber'  
    [ , [ @destination_db = ] 'destination_db']  
    [ , [ @for_schema_change = ] 'for_schema_change']  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @ignore_distributor_failure = ] ignore_distributor_failure ]   
    [ , [ @invalidate_snapshot = ] invalidate_snapshot ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec All comme valeur par défaut.  
  
`[ @article = ] 'article'` Nom de l’article. *article* est de **type sysname**, avec All comme valeur par défaut. Pour une publication avec mise à jour immédiate, *l’article* doit être **All**; dans le cas contraire, la procédure stockée ignore la publication et signale une erreur.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, sans valeur par défaut.  
  
`[ @destination_db = ] 'destination_db'` Nom de la base de données de destination. *destination_db* est de **type sysname**, avec All comme valeur par défaut.  
  
`[ @for_schema_change = ] 'for_schema_change'` Indique si la réinitialisation se produit à la suite d’une modification de schéma au niveau de la base de données de publication. *for_schema_change* est de **bit**, avec 0 comme valeur par défaut. Si la **valeur est 0**, les abonnements actifs pour les publications qui autorisent la mise à jour immédiate sont réactivés tant que la publication entière, et pas seulement certains de ses articles, sont réinitialisées. Ceci implique que la réinitialisation est effectuée à la suite de modifications du schéma. Si la condition est **1**, les abonnements actifs ne sont pas réactivés tant que le agent d’instantané n’est pas exécuté.  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé pour les serveurs de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @ignore_distributor_failure = ] ignore_distributor_failure` Autorise la réinitialisation même si le serveur de distribution n’existe pas ou est hors connexion. *ignore_distributor_failure* est de **bit**, avec 0 comme valeur par défaut. Si la **valeur est 0**, la réinitialisation échoue si le serveur de distribution n’existe pas ou est hors connexion.  
  
`[ @invalidate_snapshot = ] invalidate_snapshot` Invalide l’instantané existant de la publication. *invalidate_snapshot* est de **bit**, avec 0 comme valeur par défaut. Si la variable est **1**, un nouvel instantané est généré pour la publication.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_reinitsubscription** est utilisé dans la réplication transactionnelle.  
  
 **sp_reinitsubscription** n’est pas pris en charge pour la réplication transactionnelle d’égal à égal.  
  
 Pour les abonnements dans lesquels l'instantané initial est appliqué automatiquement et où la publication n'autorise pas les abonnements pouvant être mis à jour, l'Agent d'instantané doit être exécuté après l'exécution de cette procédure stockée pour que les fichiers du schéma et du programme de copie en bloc soient préparés et que les Agents de distribution puissent ensuite resynchroniser les abonnements.  
  
 Lorsque l'instantané initial est appliqué automatiquement et que la publication autorise les abonnements pouvant être mis à jour, l'Agent de distribution resynchronise l'abonnement à partir des fichiers de schéma et de programme de copie en bloc les plus récents préalablement créés par l'Agent d'instantané. Le Agent de distribution resynchronise l’abonnement immédiatement après l’exécution de **sp_reinitsubscription** par l’utilisateur, si le agent de distribution n’est pas occupé ; Sinon, la synchronisation peut avoir lieu après l’intervalle de message (spécifié par Agent de distribution paramètre d’invite de commandes : **MessageInterval**).  
  
 **sp_reinitsubscription** n’a aucun effet sur les abonnements où l’instantané initial est appliqué manuellement.  
  
 Pour resynchroniser les abonnements anonymes à une publication, transmettez **All** ou null en tant qu' *abonné*.  
  
 La réplication transactionnelle prend en charge la réinitialisation d'abonnements au niveau de l'article. L'instantané de l'article est réappliqué sur l'Abonné au cours de la synchronisation suivante après que l'article a été marqué pour la réinitialisation. Toutefois, si le même Abonné a également souscrit à des articles dépendants, la réapplication de l'instantané sur l'article peut échouer sauf si les articles dépendants de la publication sont également automatiquement réinitialisés sous certaines conditions :  
  
-   Si la commande de pré-création de l'article est 'drop', les articles des vues liées au schéma et les procédures stockées liées au schéma sur l'objet de base de cet article sont également marqués pour la réinitialisation.  
  
-   Si l'option de schéma sur l'article inclut le script d'intégrité référentielle déclarée sur les clés primaires, les articles qui ont des tables de base avec des relations de clés étrangères à des tables de base de l'article réinitialisé sont également marqués pour la réinitialisation.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_reinittranpushsub](../../relational-databases/replication/codesnippet/tsql/sp-reinitsubscription-tr_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** , les membres du rôle de base de données fixe **db_owner** ou le créateur de l’abonnement peuvent exécuter **sp_reinitsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Réinitialiser un abonnement](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [Réinitialiser des abonnements](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
