---
description: sp_update_operator (Transact-SQL)
title: sp_update_operator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_operator_TSQL
- sp_update_operator
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_operator
ms.assetid: 231750a6-4828-4d03-afe6-b91d38c42ed3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2d664bbfc012ee262528397f735525138e91e82b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187303"
---
# <a name="sp_update_operator-transact-sql"></a>sp_update_operator (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met à jour des informations sur un opérateur (destinataire de la notification) à utiliser pour les alertes et les travaux.  
  
   ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_update_operator   
     [ @name =] 'name'   
     [ , [ @new_name = ] 'new_name' ]   
     [ , [ @enabled = ] enabled]   
     [ , [ @email_address = ] 'email_address' ]  
     [ , [ @pager_address = ] 'pager_number']   
     [ , [ @weekday_pager_start_time = ] weekday_pager_start_time ]  
     [ , [ @weekday_pager_end_time = ] weekday_pager_end_time ]   
     [ , [ @saturday_pager_start_time = ] saturday_pager_start_time ]  
     [ , [ @saturday_pager_end_time = ] saturday_pager_end_time ]   
     [ , [ @sunday_pager_start_time = ] sunday_pager_start_time ]  
     [ , [ @sunday_pager_end_time = ] sunday_pager_end_time ]   
     [ , [ @pager_days = ] pager_days ]   
     [ , [ @netsend_address = ] 'netsend_address' ]   
     [ , [ @category_name = ] 'category' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @name =] '*nom*'  
 Nom de l'opérateur à modifier. *Name* est de **type sysname**, sans valeur par défaut.  
  
 [ @new_name =] '*new_name*'  
 Nouveau nom de l'opérateur. Ce nom doit être unique. *new_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
 [ @enabled =] *activé*  
 Nombre indiquant l’état actuel de l’opérateur (**1** s’il est actuellement activé, **0** dans le cas contraire). *Enabled* est de **type tinyint**, avec NULL comme valeur par défaut. S'il n'est pas activé, l'opérateur ne recevra pas de notifications d'alerte.  
  
 [ @email_address =] '*email_address*'  
 Adresse de courrier électronique de l'opérateur. Cette chaîne est transmise directement au système de messagerie électronique. *email_address* est de type **nvarchar (100)**, avec NULL comme valeur par défaut.  
  
 [ @pager_address =] '*pager_number*'  
 Adresse de radiomessagerie de l’opérateur. Cette chaîne est transmise directement au système de messagerie électronique. *pager_number* est de type **nvarchar (100)**, avec NULL comme valeur par défaut.  
  
 [ @weekday_pager_start_time =] *weekday_pager_start_time*  
 Indique l'heure à partir de laquelle une notification peut être envoyée à cet opérateur sur son récepteur de radiomessagerie, du lundi au vendredi. *weekday_pager_start_time* est de **type int**, avec NULL comme valeur par défaut et doit être entré au format HHMMSS pour une utilisation avec une horloge de 24 heures.  
  
 [ @weekday_pager_end_time =] *weekday_pager_end_time*  
 Indique l'heure à partir de laquelle une notification ne peut pas être envoyée à l'opérateur spécifié sur son récepteur de radiomessagerie, du lundi au vendredi. *weekday_pager_end_time* est de **type int**, avec NULL comme valeur par défaut et doit être entré au format HHMMSS pour une utilisation avec une horloge de 24 heures.  
  
 [ @saturday_pager_start_time =] *saturday_pager_start_time*  
 Indique l'heure à partir de laquelle une notification peut être envoyée le samedi à l'opérateur spécifié sur son récepteur de radiomessagerie. *saturday_pager_start_time* est de **type int**, avec NULL comme valeur par défaut et doit être entré au format HHMMSS pour une utilisation avec une horloge de 24 heures.  
  
 [ @saturday_pager_end_time =] *saturday_pager_end_time*  
 Indique l'heure à partir de laquelle une notification ne peut pas être envoyée le samedi à l'opérateur spécifié sur son récepteur de radiomessagerie. *saturday_pager_end_time* est de **type int**, avec NULL comme valeur par défaut et doit être entré au format HHMMSS pour une utilisation avec une horloge de 24 heures.  
  
 [ @sunday_pager_start_time =] *sunday_pager_start_time*  
 Indique l'heure à partir de laquelle une notification peut être envoyée le dimanche à l'opérateur spécifié sur son récepteur de radiomessagerie. *sunday_pager_start_time* est de **type int**, avec NULL comme valeur par défaut et doit être entré au format HHMMSS pour une utilisation avec une horloge de 24 heures.  
  
 [ @sunday_pager_end_time =] *sunday_pager_end_time*  
 Indique l'heure à partir de laquelle une notification ne peut pas être envoyée le dimanche à l'opérateur spécifié sur son récepteur de radiomessagerie. *sunday_pager_end_time* est de **type int**, avec NULL comme valeur par défaut et doit être entré au format HHMMSS pour une utilisation avec une horloge de 24 heures.  
  
 [ @pager_days =] *pager_days*  
 Indique les jours où l'opérateur est en mesure de recevoir des notifications par radiomessagerie (en fonction des heures de début/fin précisées). *pager_days* est de **type tinyint**, avec NULL comme valeur par défaut et doit être une valeur comprise entre **0** et **127**. *pager_days* est calculé en ajoutant les valeurs individuelles pour les jours requis. Par exemple, du lundi au vendredi, il s’agit de **2** + **4** + **8** + **16** + **32**  =  **64**.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Dimanche|  
|**2**|Lundi|  
|**4**|Mardi|  
|**8**|Mercredi|  
|**16**|Jeudi|  
|**32**|Vendredi|  
|**64**|Samedi|  
  
 [ @netsend_address =] '*netsend_address*'  
 Adresse réseau de l'opérateur à qui est envoyé le message réseau. *netsend_address* est de type **nvarchar (100)**, avec NULL comme valeur par défaut.  
  
 [ @category_name =] '*catégorie*'  
 Nom de la catégorie pour cette alerte. *Category* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 La procédure sp_update_operator doit être exécutée à partir de la base de données msdb.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'exécution de cette procédure sont octroyées par défaut aux membres du rôle de serveur fixe sysadmin.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant met à jour l'état de l'opérateur en l'activant et définit les jours (du lundi au vendredi, de 8h00 à 17h00) pendant lesquels il peut être averti par radiomessagerie.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_operator   
    @name = N'François Ajenstat',  
    @enabled = 1,  
    @email_address = N'françoisa',  
    @pager_address = N'5551290AW@pager.Adventure-Works.com',  
    @weekday_pager_start_time = 080000,  
    @weekday_pager_end_time = 170000,  
    @pager_days = 64 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md)   
 [sp_delete_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)   
 [sp_help_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
