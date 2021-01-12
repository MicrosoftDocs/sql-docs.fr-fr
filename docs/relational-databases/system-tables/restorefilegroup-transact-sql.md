---
description: restorefilegroup (Transact-SQL)
title: restorefilegroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- restorefilegroup_TSQL
- restorefilegroup
dev_langs:
- TSQL
helpviewer_keywords:
- filegroups [SQL Server], restorefilegroup system table
- restorefilegroup system table
ms.assetid: 3aa15c55-6b72-4f76-97d7-bd88391d105c
author: cawrites
ms.author: chadam
ms.openlocfilehash: b4be126afb404fcff25dfa5ce036a70189d7b640
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096169"
---
# <a name="restorefilegroup-transact-sql"></a>restorefilegroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne par groupe de fichiers restaurés. Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**restore_history_id**|**int**|Numéro d'identification unique de l'opération de restauration correspondante. Fait référence à **RestoreHistory (restore_history_id)**.|  
|**filegroup_name**|**nvarchar(128)**|Nom du groupe de fichiers restaurés. Sa valeur peut être NULL.<br /><br /> Si un instantané d'une base de données est rétabli, cette valeur est renseignée de la même façon que pour une restauration complète.|  
  
## <a name="remarks"></a>Notes  
 Pour réduire le nombre de lignes dans cette table et dans d’autres tables de sauvegarde et d’historique, exécutez la procédure stockée [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) .  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde et restauration de tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [restorefile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/restorefile-transact-sql.md)   
 [restorehistory &#40;Transact-SQL&#41;](../../relational-databases/system-tables/restorehistory-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
