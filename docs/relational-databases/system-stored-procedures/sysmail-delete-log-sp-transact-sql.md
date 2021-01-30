---
description: sysmail_delete_log_sp (Transact-SQL)
title: sysmail_delete_log_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_log_sp_TSQL
- sysmail_delete_log_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_log_sp
ms.assetid: e94b37a1-70ad-46a5-86c0-721892156f7c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1f3921d220f367d15849eab080369db986d8f53e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160017"
---
# <a name="sysmail_delete_log_sp-transact-sql"></a>sysmail_delete_log_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime des événements du journal de la messagerie de base de données. Supprime tous les événements du journal ou uniquement les événements correspondant à des critères de date ou de type spécifiés.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_delete_log_sp  [ [ @logged_before = ] 'logged_before' ]  
    [, [ @event_type = ] 'event_type' ]  
  
```  
  
## <a name="arguments"></a>Arguments  
`[ @logged_before = ] 'logged_before'` Supprime les entrées jusqu’à la date et l’heure spécifiées par l’argument *logged_before* . *logged_before* est de **type DateTime** avec NULL comme valeur par défaut. La valeur NULL correspond à toutes les dates.  
  
`[ @event_type = ] 'event_type'` Supprime les entrées de journal du type spécifié en tant que *event_type*. *event_type* est de type **varchar (15)** et n’a pas de valeur par défaut. Les entrées valides sont **réussite**, **Avertissement**, **erreur** et **information**. La valeur NULL correspond à tous les types d'événements.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Utilisez la procédure stockée **sysmail_delete_log_sp** pour supprimer définitivement des entrées du journal des Database mail. Un argument facultatif vous permet de supprimer uniquement les enregistrements les plus anciens en fournissant une date et une heure. Les événements antérieurs à cet argument sont alors supprimés. Un argument facultatif vous permet de supprimer uniquement les événements d’un certain type, spécifiés comme argument **event_type** .  
  
 La suppression d'entrées dans le journal de la messagerie de base de données n'entraîne pas la suppression d'entrées de messages électroniques dans les tables de la messagerie de base de données. Utilisez [sysmail_delete_mailitems_sp](../../relational-databases/system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md) pour supprimer le courrier électronique des tables Database mail.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent accéder à cette procédure.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-deleting-all-events"></a>R. Suppression de tous les événements  
 L'exemple suivant supprime tous les événements du journal de la messagerie de base de données.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp ;  
GO  
```  
  
### <a name="b-deleting-the-oldest-events"></a>B. Suppression des événements les plus anciens  
 L'exemple suivant supprime les événements du journal de la messagerie de base de données qui sont antérieurs au 9 octobre 2005.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp  
    @logged_before = 'October 9, 2005' ;  
GO  
```  
  
### <a name="c-deleting-all-events-of-a-certain-type"></a>C. Suppression d'un certain type d'événement  
 L'exemple suivant supprime tous les messages de succès du journal de la messagerie de base de données.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_log_sp  
    @event_type = 'success' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sysmail_event_log &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)   
 [sysmail_delete_mailitems_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md)   
 [Créer un travail SQL Server Agent pour archiver les messages et les journaux d’événements de la messagerie de base de données](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md)  
  
  
