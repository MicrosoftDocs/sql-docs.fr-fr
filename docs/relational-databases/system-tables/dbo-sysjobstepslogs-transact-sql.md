---
description: dbo.sysjobstepslogs (Transact-SQL)
title: dbo.sysjobstepslogs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysjobstepslogs_TSQL
- sysjobstepslogs_TSQL
- sysjobstepslogs
- dbo.sysjobstepslogs
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobstepslogs system table
ms.assetid: 128c25db-0b71-449d-bfb2-38b8abcf24a0
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: f32042c01db8a35f9ce44fe938b19e4ae8e6118b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88373826"
---
# <a name="dbosysjobstepslogs-transact-sql"></a>dbo.sysjobstepslogs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient le journal des étapes de travail pour toutes les étapes de travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent configurées pour écrire la sortie des étapes dans une table. Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**log_id**|**int**|ID du journal d'étapes de travail.|  
|**Sign**|**nvarchar(max)**|Contenu du journal d'étapes de travail.|  
|**date_created**|**datetime**|Date et heure de création du journal d'étapes de travail.|  
|**date_modified**|**datetime**|Date et heure de la dernière modification du journal d'étapes de travail.|  
|**log_size**|**int**|Taille, en octets, du journal d'étapes de travail.|  
|**step_uid**|**uniqueidentifier**|Identificateur unique de l'étape de travail.|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_help_jobsteplog &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobsteplog-transact-sql.md)   
 [sp_delete_jobsteplog &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobsteplog-transact-sql.md)  
  
  
