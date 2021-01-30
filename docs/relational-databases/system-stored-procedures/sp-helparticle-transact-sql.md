---
description: sp_helparticle (Transact-SQL)
title: sp_helparticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helparticle_TSQL
- sp_helparticle
helpviewer_keywords:
- sp_helparticle
ms.assetid: 9c4a1a88-56f1-45a0-890c-941b8e0f0799
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 14baf63cc2944396a13bbc911fb6be7670244216
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176488"
---
# <a name="sp_helparticle-transact-sql"></a>sp_helparticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Affiche des informations sur un article. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication. Dans le cas des serveurs de publication Oracle, cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helparticle [ @publication = ] 'publication'   
    [ , [ @article = ] 'article' ]  
    [ , [ @returnfilter = ] returnfilter ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @found = ] found OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @article = ] 'article'` Nom d’un article de la publication. *article* est de **type sysname**, avec la valeur par défaut **%** . Si *l’article* n’est pas fourni, des informations sur tous les Articles de la publication spécifiée sont retournées.  
  
`[ @returnfilter = ] returnfilter` Spécifie si la clause de filtre doit être retournée. *returnfilter* est de **bits**, avec **1** comme valeur par défaut, qui retourne la clause de filtre.  
  
`[ @publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être spécifié lors de la demande d’informations sur un article publié par un serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication.  
  
`[ @found = ] found OUTPUT` À usage interne uniquement.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**ID de l’article**|**int**|ID de l’article.|  
|**article name**|**sysname**|Nom de l'article.|  
|**base object**|**nvarchar (257)**|Nom de la table sous-jacente représentée par l'article ou la procédure stockée.|  
|**objet de destination**|**sysname**|Nom de la table de destination (abonnement)|  
|**synchronization object**|**nvarchar (257)**|Nom de la vue qui définit l’article publié.|  
|**type**|**smallint**|Type d'article :<br /><br /> **1** = basé sur un journal.<br /><br /> **3** = basé sur un journal avec filtre manuel.<br /><br /> **5** = basé sur un journal avec vue manuelle.<br /><br /> **7** = basé sur un journal avec filtre manuel et vue manuelle.<br /><br /> **8** = exécution de la procédure stockée.<br /><br /> **24** = exécution d’une procédure stockée sérialisable.<br /><br /> **32** = procédure stockée (schéma uniquement).<br /><br /> **64** = vue (schéma uniquement).<br /><br /> **96** = fonction d’agrégation (schéma uniquement).<br /><br /> **128** = fonction (schéma uniquement).<br /><br /> **257** = vue indexée basée sur un journal.<br /><br /> **259** = vue indexée basée sur un journal avec filtre manuel.<br /><br /> **261** = vue indexée basée sur un journal avec vue manuelle.<br /><br /> **263** = vue indexée basée sur un journal avec filtre manuel et vue manuelle.<br /><br /> **320** = vue indexée (schéma uniquement).<br /><br />|  
|**statut**|**tinyint**|Peut être le résultat de l' [& (and au niveau du bit)](../../t-sql/language-elements/bitwise-and-transact-sql.md) d’une ou de plusieurs ou de ces propriétés d’article :<br /><br /> **0x00** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> **0x01** = l’article est actif.<br /><br /> **0x08** = inclure le nom de colonne dans les instructions INSERT.<br /><br /> **0x16** = utilise des instructions paramétrables.<br /><br /> **0x32** = utilise des instructions paramétrées et inclut le nom de colonne dans les instructions INSERT.|  
|**filter**|**nvarchar (257)**|Procédure stockée utilisée pour filtrer la table horizontalement. Cette procédure stockée doit avoir été créée à l'aide de la clause FOR REPLICATION.|  
|**description**|**nvarchar(255)**|Entrée descriptive de l'article|  
|**insert_command**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des insertions avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**update_command**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des mises à jour avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**delete_command**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des suppressions avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**creation script path**|**nvarchar(255)**|Chemin d'accès et nom d'un script de schéma d'article utilisé pour créer des tables cibles.|  
|**vertical partition**|**bit**|Indique si le partitionnement vertical est activé pour l’article ; la valeur **1** signifie que le partitionnement vertical est activé.|  
|**pre_creation_cmd**|**tinyint**|Commande de pré-création pour les instructions DROP TABLE, DELETE TABLE, ou TRUNCATE TABLE.|  
|**filter_clause**|**ntext**|Clause WHERE spécifiant le filtrage horizontal.|  
|**schema_option**|**Binary(8**|Bitmap de l’option de génération de schéma pour l’article donné. Pour obtenir la liste complète des valeurs de **schema_option** , consultez [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).|  
|**dest_owner**|**sysname**|Nom du propriétaire de l’objet de destination.|  
|**source_owner**|**sysname**|Propriétaire de l'objet source.|  
|**unqua_source_object**|**sysname**|Nom de l'objet source sans le nom du propriétaire.|  
|**sync_object_owner**|**sysname**|Propriétaire de la vue qui définit l'article publié. .|  
|**unqualified_sync_object**|**sysname**|Nom de la vue qui définit l'article publié, sans le nom du propriétaire.|  
|**filter_owner**|**sysname**|Propriétaire du filtre.|  
|**unqua_filter**|**sysname**|Nom du filtre, sans le nom du propriétaire.|  
|**auto_identity_range**|**int**|Indicateur signalant si la gestion automatique de plages d'identité était activée sur la publication au moment de sa création. **1** signifie que la plage d’identités automatique est activée ; **0** signifie qu’elle est désactivée.|  
|**publisher_identity_range**|**int**|Taille de la plage d’identité sur le serveur de publication si l’article a *identityrangemanagementoption* défini sur **auto** ou **auto_identity_range** défini sur **true**.|  
|**identity_range**|**bigint**|Taille de la plage d’identité de l’abonné si l’article a *identityrangemanagementoption* défini sur **auto** ou **auto_identity_range** défini sur **true**.|  
|**threshold**|**bigint**|Valeur de pourcentage indiquant le moment où l'Agent de distribution affecte une nouvelle plage d'identité.|  
|**identityrangemanagementoption**|**int**|Indique la gestion des plages d'identité appliquée à l'article.|  
|**fire_triggers_on_snapshot**|**bit**|Indique si les déclencheurs de l'utilisateur répliqués sont exécutés lorsque l'instantané initial est appliqué.<br /><br /> **1** = les déclencheurs utilisateur sont exécutés.<br /><br /> **0** = les déclencheurs de l’utilisateur ne sont pas exécutés.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helparticle** est utilisé dans la réplication d’instantané et la réplication transactionnelle.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** , du rôle de base de données fixe **db_owner** ou la liste d’accès à la publication pour la publication actuelle peuvent exécuter **sp_helparticle**.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_helptranarticle](../../relational-databases/replication/codesnippet/tsql/sp-helparticle-transact-_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un article](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
