---
description: sp_update_notification (Transact-SQL)
title: sp_update_notification (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_notification_TSQL
- sp_update_notification
dev_langs:
- TSQL
helpviewer_keywords:
- sp_updatenotification
ms.assetid: 3e1c3d40-8c24-46ce-a68e-ce6c6a237fda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7e1f54c6933945c6031e48d4f97ab89f0f966779
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184317"
---
# <a name="sp_update_notification-transact-sql"></a>sp_update_notification (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met à jour la méthode de notification d'une notification d'alerte.  

  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_update_notification  
          [@alert_name =] 'alert' ,  
     [@operator_name =] 'operator' ,  
     [@notification_method =] notification  
```  
  
## <a name="arguments"></a>Arguments  
`[ @alert_name = ] 'alert'` Nom de l’alerte associée à cette notification. *alerte* est de **type sysname**, sans valeur par défaut.  
  
`[ @operator_name = ] 'operator'` Opérateur qui sera averti lorsque l’alerte se produit. l' *opérateur* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @notification_method = ] notification` Méthode par laquelle l’opérateur est notifié. la *notification* est de **type tinyint**, sans valeur par défaut et peut être une ou plusieurs de ces valeurs.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Messagerie électronique|  
|**2**|Récepteur de radiomessagerie|  
|**4**|**net send**|  
|**7**|Toutes les méthodes|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_update_notification** doit être exécuté à partir de la base de données **msdb** .  
  
 Vous pouvez mettre à jour une notification pour un opérateur qui ne dispose pas des informations d’adresse nécessaires à l’aide de la *notification_method* spécifiée. En cas d'échec de l'envoi d'une notification par courrier électronique ou par radiomessagerie, l'échec est consigné dans le journal des erreurs de l'Agent Microsoft SQL Server.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter cette procédure stockée, les utilisateurs doivent disposer du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant modifie la méthode de notification pour les notifications envoyées à `François Ajenstat` pour l’alerte `Test Alert` .  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_notification  
   @alert_name = N'Test Alert',  
   @operator_name = N'François Ajenstat',  
   @notification_method = 7;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_notification &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_delete_notification &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-notification-transact-sql.md)   
 [sp_help_notification &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-notification-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
