---
title: sp_dropsubscription (Transact-SQL) | Microsoft Docs
description: Supprime les abonnements à un article, à une publication ou à des abonnements sur le serveur de publication. Cette procédure stockée s’exécute sur le serveur de publication de la base de données de publication.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropsubscription
- sp_dropsubscription_TSQL
helpviewer_keywords:
- sp_dropsubscription
ms.assetid: 7551f345-5510-4684-ab53-f9057249d13a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3097c61204e0a9ae310af87fb83901a252c50e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157947"
---
# <a name="sp_dropsubscription-transact-sql"></a>sp_dropsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Supprime les abonnements relatifs à un article, une publication ou un ensemble d'abonnements particuliers sur le serveur de publication. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dropsubscription [ [ @publication= ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
        , [ @subscriber= ] 'subscriber'  
    [ , [ @destination_db= ] 'destination_db' ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @reserved= ] 'reserved' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication associée. *publication* est de **type sysname**, avec NULL comme valeur par défaut. Si la **totalité** est, tous les abonnements de toutes les publications de l’abonné spécifié sont annulés. la *publication* est un paramètre obligatoire.  
  
`[ @article = ] 'article'` Nom de l’article. *article* est de **type sysname**, avec NULL comme valeur par défaut. Si la valeur est **All**, les abonnements à tous les articles pour chaque publication et abonné spécifiés sont supprimés. Utilisez **All pour les** publications qui autorisent la mise à jour immédiate.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné dont les abonnements seront supprimés. *Subscriber* est de **type sysname**, sans valeur par défaut. Si la **totalité** est, tous les abonnements de tous les abonnés sont supprimés.  
  
`[ @destination_db = ] 'destination_db'` Nom de la base de données de destination. *destination_db* est de **type sysname**, avec NULL comme valeur par défaut. Si la valeur est NULL, tous les abonnements de cet Abonné seront supprimés.  
  
`[ @ignore_distributor = ] ignore_distributor`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @reserved = ] 'reserved'`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_dropsubscription** est utilisé dans la réplication transactionnelle et d’instantané.  
  
 Si vous supprimez l'abonnement à un article dans une publication à synchronisation immédiate, vous ne pourrez le rajouter que si vous supprimez les abonnements relatifs à tous les articles de la publication et que vous les rajoutez tous en même temps.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_droptransubscription](../../relational-databases/replication/codesnippet/tsql/sp-dropsubscription-tran_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** , du rôle de base de données fixe **db_owner** ou de l’utilisateur qui a créé l’abonnement peuvent exécuter **sp_dropsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Supprimer un abonnement par envoi de notification](../../relational-databases/replication/delete-a-push-subscription.md)   
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
