---
description: sp_add_notification (Transact-SQL)
title: sp_add_notification (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_notification_TSQL
- sp_add_notification
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_notification
ms.assetid: 0525e0a2-ed0b-4e69-8a4c-a9e3e3622fbd
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f09a5638e55ee6bd8e0dfd6e07defc92b0ed365d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208006"
---
# <a name="sp_add_notification-transact-sql"></a>sp_add_notification (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit une notification pour une alerte.  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_notification [ @alert_name = ] 'alert' ,   
    [ @operator_name = ] 'operator' ,   
    [ @notification_method = ] notification_method  
```  
  
## <a name="arguments"></a>Arguments  
`[ @alert_name = ] 'alert'` Alerte pour cette notification. *alerte* est de **type sysname**, sans valeur par défaut.  
  
`[ @operator_name = ] 'operator'` Opérateur à notifier lorsque l’alerte se produit. l' *opérateur* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @notification_method = ] notification_method` Méthode par laquelle l’opérateur est notifié. *notification_method* est de **type tinyint**, sans valeur par défaut. *notification_method* peut être une ou plusieurs de ces valeurs associées à un opérateur logique **or** .  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Messagerie électronique|  
|**2**|Récepteur de radiomessagerie|  
|**4**|**net send**|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_add_notification** doit être exécuté à partir de la base de données **msdb** .  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un outil simple, basé sur une interface graphique, qui permet de gérer le système d'alertes dans sa totalité. L’utilisation de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] est recommandée pour configurer l’infrastructure d’alertes.  
  
 Pour envoyer une notification en réponse à une alerte, vous devez d'abord configurer l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'envoi de messages électroniques.  
  
 En cas d’échec au moment de l’envoi d’un message par e-mail ou d’une notification par radiomessagerie, l’échec est consigné dans le journal des erreurs du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_add_notification**.  
  
## <a name="examples"></a>Exemples  
 Cet exemple ajoute une notification envoyée par courrier électronique pour l'alerte spécifiée (`Test Alert`).  
  
> **Remarque :** Cet exemple suppose que `Test Alert` existe déjà et qu’il `François Ajenstat` s’agit d’un nom d’opérateur valide.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_notification  
 @alert_name = N'Test Alert',  
 @operator_name = N'François Ajenstat',  
 @notification_method = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_delete_notification &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-notification-transact-sql.md)   
 [sp_help_notification &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-notification-transact-sql.md)   
 [sp_update_notification &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-notification-transact-sql.md)   
 [sp_add_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
