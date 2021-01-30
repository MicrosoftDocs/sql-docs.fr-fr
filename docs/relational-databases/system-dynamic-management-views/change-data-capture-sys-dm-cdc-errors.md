---
description: Capture de données modifiées-sys.dm_cdc_errors
title: sys.dm_cdc_errors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_cdc_errors_TSQL
- dm_cdc_errors
- sys.dm_cdc_errors
- sys.dm_cdc_errors_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cdc_errors dynamic management view
- change data capture [SQL Server], error reporting
ms.assetid: 898f2d76-9e63-45ef-94da-8034e86004ab
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4ab244d911ffbdf590da7640001ab743ac09a60e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160627"
---
# <a name="change-data-capture---sysdm_cdc_errors"></a>Capture de données modifiées-sys.dm_cdc_errors
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque erreur rencontrée pendant la session d'analyse du journal de la capture de données modifiées.  
 
 
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID de la session.<br /><br /> 0 = l'erreur ne s'est pas produite dans une session d'analyse du journal.|  
|**phase_number**|**int**|Nombre indiquant la phase de la session au moment où l’erreur s’est produite. Pour obtenir une description de chaque phase, consultez [sys.dm_cdc_log_scan_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md).|  
|**entry_time**|**datetime**|Date et heure d'enregistrement de l'erreur. Cette valeur correspond à l'horodateur dans le journal des erreurs SQL.|  
|**error_number**|**int**|ID du message d'erreur.|  
|**error_severity**|**int**|Niveau de gravité du message, entre 1 et 25.|  
|**error_state**|**int**|Numéro d'état de l'erreur.|  
|**error_message**|**nvarchar(1024)**|Texte du message de l'erreur.|  
|**start_lsn**|**nvarchar (23)**|Valeur LSN de départ des lignes en cours de traitement lorsque l'erreur s'est produite.<br /><br /> 0 = l'erreur ne s'est pas produite dans une session d'analyse du journal.|  
|**begin_lsn**|**nvarchar (23)**|Valeur LSN de départ de la transaction en cours de traitement lorsque l'erreur s'est produite.<br /><br /> 0 = l'erreur ne s'est pas produite dans une session d'analyse du journal.|  
|**sequence_value**|**nvarchar (23)**|Valeur LSN des lignes en cours de traitement lorsque l'erreur s'est produite.<br /><br /> 0 = l'erreur ne s'est pas produite dans une session d'analyse du journal.|  
  
## <a name="remarks"></a>Notes  
 **sys.dm_cdc_errors** contient des informations d’erreur pour les sessions 32 précédentes.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation VIEW DATABASE STATE pour interroger la vue de gestion dynamique **sys.dm_cdc_errors** . Pour plus d’informations sur les autorisations sur les vues de gestion dynamique, consultez [fonctions et vues de gestion dynamique &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_cdc_log_scan_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md)   
 [sys.dm_repl_traninfo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-repl-traninfo-transact-sql.md)  
  
  

