---
description: sp_add_operator (Transact-SQL)
title: sp_add_operator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_operator
- sp_add_operator_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_operator
ms.assetid: 817cd98a-4dff-4ed8-a546-f336c144d1e0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6cb3aed762c87430e75c14c954c80e58ab70d81e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195887"
---
# <a name="sp_add_operator-transact-sql"></a>sp_add_operator (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée un opérateur (destinataire de la notification) à utiliser pour les alertes et les travaux.  
  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_operator [ @name = ] 'name'   
     [ , [ @enabled = ] enabled ]   
     [ , [ @email_address = ] 'email_address' ]   
     [ , [ @pager_address = ] 'pager_address' ]   
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
`[ @name = ] 'name'` Nom d’un opérateur (destinataire de la notification). Ce nom doit être unique et ne peut pas contenir le caractère de pourcentage ( **%** ). *Name* est de **type sysname**, sans valeur par défaut.  
  
`[ @enabled = ] enabled` Indique l’état actuel de l’opérateur. *Enabled* est de **type tinyint**, avec **1** comme valeur par défaut (activé). Si la **valeur est 0**, l’opérateur n’est pas activé et ne reçoit pas de notifications.  
  
`[ @email_address = ] 'email_address'` Adresse de messagerie de l’opérateur. Cette chaîne est transmise directement au système de messagerie électronique. *email_address* est de type **nvarchar (100)**, avec NULL comme valeur par défaut.  
  
 Vous pouvez spécifier une adresse de messagerie physique ou un alias pour *email_address*. Par exemple :  
  
 '**jdoe**'ou'**jdoe \@ xyz.com**'  
  
> [!NOTE]  
>  Vous devez utiliser l'adresse de messagerie pour Messagerie de base de données.  
  
`[ @pager_address = ] 'pager_address'` Adresse de radiomessagerie de l’opérateur. Cette chaîne est transmise directement au système de messagerie électronique. *pager_address* est de type **nvarchar (100)**, avec NULL comme valeur par défaut.  
  
`[ @weekday_pager_start_time = ] weekday_pager_start_time` Heure après laquelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’agent envoie une notification par radiomessagerie à l’opérateur spécifié sur les jours de la semaine, du lundi au vendredi. *weekday_pager_start_time* est de **type int**, avec **090000** comme valeur par défaut, qui indique 9:00 A.M. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
`[ @weekday_pager_end_time = ] weekday_pager_end_time` Heure après laquelle le service **SQLServerAgent** n’envoie plus de notification par radiomessagerie à l’opérateur spécifié sur les jours de la semaine, du lundi au vendredi. *weekday_pager_end_time* est de **type int**, avec 180000 comme valeur par défaut, qui indique 6:00 P.M. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
`[ @saturday_pager_start_time = ] saturday_pager_start_time` Heure après laquelle le service **SQLServerAgent** envoie une notification par radiomessagerie à l’opérateur spécifié sur les samedis. *saturday_pager_start_time* est de **type int**, avec 090000 comme valeur par défaut, qui indique 9:00 A.M. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
`[ @saturday_pager_end_time = ] saturday_pager_end_time` Heure après laquelle le service **SQLServerAgent** n’envoie plus de notification par radiomessagerie à l’opérateur spécifié sur les samedis. *saturday_pager_end_time* est de **type int**, avec **180000** comme valeur par défaut, qui indique 6:00 P.M. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
`[ @sunday_pager_start_time = ] sunday_pager_start_time` Heure après laquelle le service **SQLServerAgent** envoie une notification par radiomessagerie à l’opérateur spécifié le dimanche. *sunday_pager_start_time* est de **type int**, avec **090000** comme valeur par défaut, qui indique 9:00 A.M. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
`[ @sunday_pager_end_time = ] sunday_pager_end_time` Heure après laquelle le service **SQLServerAgent** n’envoie plus de notification par radiomessagerie à l’opérateur spécifié le dimanche. *sunday_pager_end_time* est de **type int**, avec **180000** comme valeur par défaut, qui indique 6:00 P.M. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
`[ @pager_days = ] pager_days` Nombre qui indique les jours pendant lesquels l’opérateur est disponible pour les pages (selon les heures de début et de fin spécifiées). *pager_days* est de **type tinyint**, avec **0** comme valeur par défaut indiquant que l’opérateur n’est jamais disponible pour recevoir une page. Les valeurs valides sont comprises entre **0** et **127**. *pager_days* est calculé en ajoutant les valeurs individuelles pour les jours requis. Par exemple, du lundi au vendredi, il s’agit de **2** + **4** + **8** + **16** + **32**  =  **62**. Le tableau ci-après indique la valeur correspondant à chaque jour de la semaine.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Dimanche|  
|**2**|Lundi|  
|**4**|Mardi|  
|**8**|Mercredi|  
|**16**|Jeudi|  
|**32**|Vendredi|  
|**64**|Samedi|  
  
`[ @netsend_address = ] 'netsend_address'` Adresse réseau de l’opérateur à qui le message réseau est envoyé. *netsend_address* est de type **nvarchar (100)**, avec NULL comme valeur par défaut.  
  
`[ @category_name = ] 'category'` Nom de la catégorie pour cet opérateur. *Category* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_add_operator** doit être exécuté à partir de la base de données **msdb** .  
  
 Les appels de radiomessagerie reposent sur le système de courrier électronique qui doit pouvoir passer du courrier électronique au récepteur de radiomessagerie si vous désirez utiliser ce dernier.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un outil dont l'interface graphique permet de gérer facilement les travaux. Son utilisation est recommandée pour créer et gérer l'infrastructure des travaux.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_add_operator**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant définit les informations relatives à l'opérateur `danwi`. L'opérateur est activé. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent envoie des informations par radiomessagerie du lundi au vendredi de 8 heures à 17 heures.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_operator  
    @name = N'Dan Wilson',  
    @enabled = 1,  
    @email_address = N'danwi',  
    @pager_address = N'5551290AW@pager.Adventure-Works.com',  
    @weekday_pager_start_time = 080000,  
    @weekday_pager_end_time = 170000,  
    @pager_days = 62 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_delete_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)   
 [sp_help_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [sp_update_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-operator-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
