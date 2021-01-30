---
description: sp_help_notification (Transact-SQL)
title: sp_help_notification (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_notification
- sp_help_notification_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_notification
ms.assetid: 0273457f-9d2a-4a6f-9a16-6a6bf281cba0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d37d091fdb380f0a08f3f0064f2ce408f439eee8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199987"
---
# <a name="sp_help_notification-transact-sql"></a>sp_help_notification (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit une liste d'alertes pour un opérateur donné ou une liste d'opérateurs pour une alerte donnée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_notification  
     [ @object_type = ] 'object_type' ,  
     [ @name = ] 'name' ,  
     [ @enum_type = ] 'enum_type' ,   
     [ @notification_method = ] notification_method   
     [ , [ @target_name = ] 'target_name' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @object_type = ] 'object_type'` Type d’informations à retourner. *object_type* est de **type char (9)**, sans valeur par défaut. *object_type* peut être Alerts, qui répertorie les alertes assignées au nom d’opérateur fourni *,* ou Operators, qui répertorie les opérateurs responsables du nom d’alerte fourni *.*  
  
`[ @name = ] 'name'` Un nom d’opérateur (si *object_type* est Operators) ou un nom d’alerte (si *object_type* est alertes). *Name* est de **type sysname**, sans valeur par défaut.  
  
`[ @enum_type = ] 'enum_type'` Informations de *object_type* retournées. *enum_type* est réel dans la plupart des cas. *enum_type* est de **type char (10)**, sans valeur par défaut, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|ACTUAL|Répertorie uniquement les *object_types* associées au *nom*.|  
|ALL|Répertorie tous les *object_types* y compris ceux qui ne sont pas associés au *nom*.|  
|TARGET|Répertorie uniquement les *object_types* correspondant à la *target_name* fournie, quelle que soit l’association avec le *nom*.|  
  
`[ @notification_method = ] notification_method` Valeur numérique qui détermine les colonnes de méthode de notification à retourner. *notification_method* est de **type tinyint** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Courrier électronique : retourne uniquement la colonne **use_email** .|  
|**2**|Radiomessagerie : retourne uniquement la colonne **use_pager** .|  
|**4**|NetSend : retourne uniquement la colonne **use_netsend** .|  
|**7**|Tout : retourne toutes les colonnes.|  
  
`[ @target_name = ] 'target_name'` Nom d’alerte à rechercher (si *object_type* est Alerts) ou nom d’opérateur à rechercher (si *object_type* opérateur est). *target_name* n’est nécessaire que si *enum_type* est cible. *target_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-valves"></a>Valeur des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si *object_type* est **alertes**, le jeu de résultats répertorie toutes les alertes pour un opérateur donné.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**alert_id**|**int**|Numéro d'identification de l'alerte.|  
|**alert_name**|**sysname**|Nom de l’alerte.|  
|**use_email**|**int**|Un message électronique est utilisé pour avertir l'opérateur.<br /><br /> **1** = Oui<br /><br /> **0** = Non|  
|**use_pager**|**int**|La radiomessagerie est utilisée pour avertir l'opérateur.<br /><br /> **1** = Oui<br /><br /> **0** = Non|  
|**use_netsend**|**int**|Le réseau est utilisé pour avertir l'opérateur :<br /><br /> **1** = Oui<br /><br /> **0** = Non|  
|**has_email**|**int**|Nombre de notifications envoyées par messagerie électronique pour cette alerte.|  
|**has_pager**|**int**|Nombre de notifications envoyées par radiomessagerie pour cette alerte.|  
|**has_netsend**|**int**|Nombre de notifications **net send** envoyées pour cette alerte.|  
  
 Si **object_type** est **opérateurs**, le jeu de résultats répertorie tous les opérateurs pour une alerte donnée.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**operator_id**|**int**|Numéro d'identification de l'opérateur.|  
|**operator_name**|**sysname**|Nom de l’opérateur.|  
|**use_email**|**int**|Un message électronique est utilisé pour envoyer la notification à l'opérateur :<br /><br /> **1** = Oui<br /><br /> **0** = Non|  
|**use_pager**|**int**|La radiomessagerie est utilisée pour envoyer la notification à l'opérateur :<br /><br /> **1** = Oui<br /><br /> **0** = Non|  
|**use_netsend**|**int**|Fenêtre contextuelle du réseau utilisée pour notifier l’opérateur :<br /><br /> **1** = Oui<br /><br /> **0** = Non|  
|**has_email**|**int**|L'opérateur possède une adresse électronique :<br /><br /> **1** = Oui<br /><br /> **0** = Non|  
|**has_pager**|**int**|L'opérateur possède une adresse de radiomessagerie :<br /><br /> **1** = Oui<br /><br /> **0** = Non|  
|**has_netsend**|**int**|Une notification d'envoi réseau est configurée pour l'opérateur.<br /><br /> **1** = Oui<br /><br /> **0** = Non|  
  
## <a name="remarks"></a>Notes  
 Cette procédure stockée doit être exécutée à partir de la base de données **msdb** .  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter cette procédure stockée, l'utilisateur doit être membre du rôle de serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-alerts-for-a-specific-operator"></a>R. Affichage d'une liste d'alertes pour un opérateur spécifique  
 L'exemple suivant retourne toutes les alertes dont l'opérateur `François Ajenstat` est notifié.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_notification   
    @object_type = N'ALERTS',  
    @name = N'François Ajenstat',  
    @enum_type = N'ACTUAL',  
    @notification_method = 7 ;  
GO  
```  
  
### <a name="b-listing-operators-for-a-specific-alert"></a>B. Affichage d'une liste d'opérateurs pour une alerte spécifique  
 L'exemple suivant retourne tous les opérateurs qui reçoivent une notification quelconque pour l'alerte `Test Alert`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_notification  
    @object_type = N'OPERATORS',  
    @name = N'Test Alert',  
    @enum_type = N'ACTUAL',  
    @notification_method = 7 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_notification &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_delete_notification &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-notification-transact-sql.md)   
 [sp_update_notification &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-notification-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
