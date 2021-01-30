---
description: managed_backup.fn_get_current_xevent_settings managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_get_current_xevent_settings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_get_current_xevent_settings
- smart_admin.fn_get_current_xevent_settings_TSQL
- fn_get_current_xevent_settings_TSQL
- smart_admin.fn_get_current_xevent_settings
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_get_current_xevent_settings
- fn_get_current_xevent_settings
ms.assetid: 95d3adaa-bb9d-4833-b8b4-3d9fd4f9c82a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e0bd15419c867134a66cf678cbb6355df0c4142f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207408"
---
# <a name="managed_backupfn_get_current_xevent_settings-transact-sql"></a>managed_backup.fn_get_current_xevent_settings managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Retourne une ligne pour chaque type d'événement étendu pris en charge par Smart Admin.  
  
 Utilisez cette fonction pour retourner ou consulter les paramètres des événements étendus actuels afin d'identifier le type des événements qui sont configurables et les configurations actuelles.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
smart_admin.fn_get_current_xevent_settings ()   
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Arguments  
 Cette fonction n'a aucun argument.  
  
## <a name="table-returned"></a>Table retournée  
 Les canaux d'administration, analytique et opérationnel des événements étendus sont nécessaires, activés par défaut et non configurables.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|Event_name|NVARCHAR(128)|Type d'événement étendu|  
|is_configurable|NVARCHAR(128)|Cette propriété a la valeur **true** si l’événement est configurable, sinon elle a la valeur **false**.|  
|is_enabled|NVARCHAR(128)|A la valeur True si l'événement est activé, sinon, a la valeur False. Utilisez smart_admin.sp_set_parameter pour activer les événements de débogage.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert des autorisations **Select** sur la fonction.  
  
## <a name="examples"></a>Exemples  
 Cet exemple renvoie tous les événements étendus avec leur état actuel.  
  
```  
SELECT *   
FROM smart_admin.fn_get_current_xevent_settings ()  
  
```  
  
  
