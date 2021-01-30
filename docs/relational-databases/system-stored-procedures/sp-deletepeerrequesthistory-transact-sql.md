---
description: sp_deletepeerrequesthistory (Transact-SQL)
title: sp_deletepeerrequesthistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_deletepeerrequesthistory
- sp_deletepeerrequesthistory_TSQL
helpviewer_keywords:
- sp_deletepeerrequesthistory
ms.assetid: 63a4ec6e-ce79-4bf1-9d37-5ac88f8d6beb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 56dad6791177093750585122eb939ba5a4ca5763
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189621"
---
# <a name="sp_deletepeerrequesthistory-transact-sql"></a>sp_deletepeerrequesthistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime l’historique lié à une demande d’état de publication, qui comprend l’historique des demandes ([MSpeer_request &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/mspeer-request-transact-sql.md)), ainsi que l’historique des réponses ([MSpeer_response &#40;transact-SQL&#41;](../../relational-databases/system-tables/mspeer-response-transact-sql.md)). Cette procédure stockée est exécutée sur la base de données de publication sur un serveur de publication participant à une topologie de réplication d’égal à égal. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_deletepeerrequesthistory [ @publication = ] 'publication'  
    [ , [ @request_id = ] request_id ]  
    [ , [ @cutoff_date = ] cutoff_date ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication pour laquelle la demande d’État a été effectuée. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @request_id = ] request_id` Spécifie une demande d’État individuelle afin que toutes les réponses à cette demande soient supprimées. *request_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @cutoff_date = ] cutoff_date` Spécifie une date de coupure, avant laquelle tous les enregistrements de réponse antérieurs sont supprimés. *cutoff_date* est de **type DateTime**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_deletepeerrequesthistory** est utilisé dans une topologie de réplication transactionnelle d’égal à égal. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 Lors de l’exécution de **sp_deletepeerrequesthistory**, vous devez spécifier *request_id* ou *cutoff_date* .  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_deletepeerrequesthistory**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_helppeerrequests &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppeerrequests-transact-sql.md)   
 [sp_helppeerresponses &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppeerresponses-transact-sql.md)   
 [sp_requestpeerresponse &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md)  
  
  
