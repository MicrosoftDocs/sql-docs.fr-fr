---
description: sp_subscription_cleanup (Transact-SQL)
title: sp_subscription_cleanup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_subscription_cleanup_TSQL
- sp_subscription_cleanup
helpviewer_keywords:
- sp_subscription_cleanup
ms.assetid: bdc8aaa0-ff2d-40c2-84b2-4ba513ced279
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: d75293627b229b942e6d81753320a4bbb0fffbf9
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88489086"
---
# <a name="sp_subscription_cleanup-transact-sql"></a>sp_subscription_cleanup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime les métadonnées en cas de suppression d'un abonnement sur l'Abonné. En cas d'abonnement de transaction de synchronisation, elles comprennent également les déclencheurs à mise à jour immédiate. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_subscription_cleanup [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
    [ , [ @publication = ] 'publication']  
    [ , [ @reserved = ] 'reserved']  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec NULL comme valeur par défaut. Si la valeur est NULL, les abonnements utilisant une publication d'Agent partagé dans la base de données de publication sont supprimés.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_subscription_cleanup** est utilisé dans la réplication transactionnelle et d’instantané.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_subscription_cleanup**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_expired_subscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-expired-subscription-cleanup-transact-sql.md)   
 [sp_mergesubscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-mergesubscription-cleanup-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
