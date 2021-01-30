---
description: sysmail_delete_mailitems_sp (Transact-SQL)
title: sysmail_delete_mailitems_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_mailitems_sp_TSQL
- sysmail_delete_mailitems_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_mailitems_sp
ms.assetid: f87c9f4a-bda1-4bce-84b2-a055a3229ecd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 14e9d1fa942fc41f4865e70460465691e4aef6ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182043"
---
# <a name="sysmail_delete_mailitems_sp-transact-sql"></a>sysmail_delete_mailitems_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime définitivement des messages électroniques des tables internes de la messagerie de base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_delete_mailitems_sp  [ [ @sent_before = ] 'sent_before' ]  
    [ , [ @sent_status = ] 'sent_status' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ \@sent_before = ] 'sent_before'` Supprime les messages électroniques jusqu’à la date et à l’heure spécifiées comme argument *sent_before* . *sent_before* est de **type DateTime** avec NULL comme valeur par défaut. La valeur NULL correspond à toutes les dates.  
  
`[ \@sent_status = ] 'sent_status'` Supprime les messages électroniques du type spécifié par *sent_status*. *sent_status* est de type **varchar (8)** et n’a pas de valeur par défaut. Les entrées valides sont **Envoyer**, non **envoyé**, **nouvelle tentative** et **échec**. La valeur NULL correspond à tous les états.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Database Mail messages et leurs pièces jointes sont stockés dans la base de données **msdb** . Les messages doivent être supprimés régulièrement pour empêcher que la base de données **msdb** ne devienne plus volumineuse que prévu et pour se conformer au programme de rétention de documents de votre organisation. Utilisez la procédure stockée **sysmail_delete_mailitems_sp** pour supprimer définitivement les messages électroniques des tables Database mail. Un argument facultatif vous permet de supprimer uniquement les messages les plus anciens en fournissant une date et une heure. Les messages antérieurs à cet argument sont alors supprimés. Un autre argument facultatif vous permet de supprimer uniquement les messages électroniques d’un certain type, spécifiés comme argument **sent_status** . Vous devez fournir un argument pour **\@ sent_before** ou **\@ sent_status**. Pour supprimer tous les messages, utilisez **\@ sent_before = GETDATE ()**.  
  
 La suppression d'un message entraîne également la suppression des pièces jointes qui sont associées à ce message. La suppression du courrier électronique ne supprime pas les entrées correspondantes dans **sysmail_event_log**. Utilisez [sysmail_delete_log_sp](../../relational-databases/system-stored-procedures/sysmail-delete-log-sp-transact-sql.md) pour supprimer des éléments du journal.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, l’exécution de cette procédure stockée est accordée aux membres du rôle serveur fixe **sysadmin** et à **DatabaseMailUserRole**. Les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure pour supprimer les messages électroniques envoyés par tous les utilisateurs. Les membres de **DatabaseMailUserRole** peuvent uniquement supprimer les messages électroniques envoyés par cet utilisateur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-deleting-all-e-mails"></a>R. Suppression de tous les messages électroniques  
 L'exemple suivant supprime tous les messages électroniques du système de la messagerie de base de données.  
  
```  
DECLARE @GETDATE datetime  
SET @GETDATE = GETDATE();  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp @sent_before = @GETDATE;  
GO  
```  
  
### <a name="b-deleting-the-oldest-e-mails"></a>B. Suppression des messages électroniques les plus anciens  
 L'exemple suivant supprime les messages électroniques du journal de la messagerie de base de données qui sont antérieurs au `October 9, 2005`.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp   
    @sent_before = 'October 9, 2005' ;  
GO  
```  
  
### <a name="c-deleting-all-e-mails-of-a-certain-type"></a>C. Suppression d'un certain type de message électronique  
 L'exemple suivant supprime tous les messages électroniques qui ont échoué du système de la messagerie de base de données.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp   
    @sent_status = 'failed' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)   
 [sysmail_event_log &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)   
 [sysmail_mailattachments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-mailattachments-transact-sql.md)   
 [Créer un travail SQL Server Agent pour archiver les messages et les journaux d’événements de la messagerie de base de données](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md)  
  
  
