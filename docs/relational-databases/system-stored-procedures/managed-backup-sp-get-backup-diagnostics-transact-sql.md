---
description: managed_backup.sp_get_backup_diagnostics managed_backup (Transact-SQL)
title: managed_backup managed_backup.sp_get_backup_diagnostics (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_get_backup_diagnostics_TSQL
- sp_get_backup_diagnostics
- smart_admin.sp_get_backup_diagnostics_TSQL
- smart_admin.sp_get_backup_diagnostics
dev_langs:
- TSQL
helpviewer_keywords:
- sp_get_backup_diagnostics
- smart_admin.sp_get_backup_diagnostics
ms.assetid: 2266a233-6354-464b-91ec-824ca4eb9ceb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fbda9abd944665d3f0d52c174e0b1a5c1e488fa0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205286"
---
# <a name="managed_backupsp_get_backup_diagnostics-transact-sql"></a>managed_backup.sp_get_backup_diagnostics managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Retourne les événements étendus enregistrés par Smart Admin.  
  
 Utilisez cette procédure stockée pour surveiller les événements étendus enregistrés par Smart admin. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] les événements sont enregistrés dans ce système et peuvent être examinés et surveillés à l’aide de cette procédure stockée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
managed_backup.sp_get_backup_diagnostics [@xevent_channel = ] 'event type' [, [@begin_time = ] 'time1' ] [, [@end_time = ] 'time2'VARCHAR(255) = 'Xevent',@begin_time DATETIME = NULL,@end_time DATETIME = NULL  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Arguments  
 @xevent_channel  
 Type d'événement étendu. La valeur par défaut retourne tous les événements enregistrés au cours des 30 dernières minutes. Les événements enregistrés dépendent des types d'événements étendus activés. Vous pouvez utiliser ce paramètre pour filtrer la procédure stockée et afficher uniquement les événements d'un type spécifique. Vous pouvez spécifier le nom complet de l’événement ou spécifier une sous-chaîne telle que : **« admin**», « Analytics **»**, **« Operational »** et **« Debug »**. Est de type @event_channel **VARCHAR (255)**.  
  
 Pour obtenir la liste des types d’événements actuellement activés, utilisez la fonction **managed_backup. fn_get_current_xevent_settings** .  
  
 [@begin_time  
 Début de la période à partir de laquelle les événements doivent être affichés. Le @begin_time paramètre est DateTime avec NULL comme valeur par défaut. S'il n'est pas spécifié, les événements correspondant aux dernières 30 minutes sont affichés.  
  
 @end_time  
 Fin de la période jusqu'à laquelle les événements doivent être affichés. Le @end_time paramètre est DATATIME avec NULL comme valeur par défaut.  S'il n'est pas spécifié, les événements qui se sont produits jusqu'à l'heure actuelle sont affichés.  
  
## <a name="table-returned"></a>Table retournée  
 Cette procédure stockée retourne une table contenant les informations suivantes :  
  
| Nom de la colonne | Type de données | Description |  
| ----------- | --------- | ----------- |
|event_type|NVARCHAR (512)|Type d’événement étendu.|  
|Événement|NVARCHAR (512)|Récapitulatif des journaux d'événements.|  
|Timestamp|timestamp|Horodateur de l'événement indiquant à quel moment l'événement s'est produit.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert des autorisations **Execute** sur la procédure stockée. Elle requiert également des autorisations **View Server State** , car elle appelle en interne d’autres objets système qui nécessitent cette autorisation.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne tous les événements enregistrés au cours des 30 dernières minutes.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics  
  
```  
  
 L'exemple suivant retourne tous les événements enregistrés au cours d'une plage de temps spécifique.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics @xevent_channel = 'Admin',  
  @begin_time = '2013-06-01', @end_time = '2013-06-10'  
  
```  
  
 L'exemple suivant retourne tous les événements analytiques enregistrés au cours des 30 dernières minutes.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics @xevent_channel = 'Analytic'  
  
```  
  
  
