---
title: backupmediafamily (Transact-SQL) | Microsoft Docs
description: Référence pour backupmediafamily, qui contient une ligne pour chaque famille de supports.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- backupmediafamily
- backupmediafamily_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backupmediafamily system table
- backup media [SQL Server], backupmediafamily system table
author: cawrites
ms.author: chadam
ms.openlocfilehash: df687040bf73fb1d78f15d7cca18cc0fc092ee1f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198680"
---
# <a name="backupmediafamily-transact-sql"></a>backupmediafamily (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette table contient une ligne pour chaque famille de supports de sauvegardes. Si une famille de supports réside dans un support de sauvegarde miroir, elle a une ligne distincte pour chaque miroir du support de sauvegarde. Cette table est stockée dans la base de données **msdb** .  
    
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**media_set_id**|**int**|Numéro d'identification unique du support de sauvegarde dont cette famille est membre. Références **backupmediaset (media_set_id)**|  
|**family_sequence_number**|**tinyint**|Position de la famille de supports dans le support de sauvegarde.|  
|**media_family_id**|**uniqueidentifier**|Numéro d'identification unique de la famille de supports de sauvegarde. Sa valeur peut être NULL.|  
|**media_count**|**int**|Nombre de supports de sauvegarde dans la famille de supports. Sa valeur peut être NULL.|  
|**logical_device_name**|**nvarchar(128)**|Nom de cette unité de sauvegarde dans **sys.backup_devices. Name**. S’il s’agit d’une unité de sauvegarde temporaire (par opposition à une unité de sauvegarde permanente qui existe dans **sys.backup_devices**), la valeur de **logical_device_name** est null.|  
|**physical_device_name**|**nvarchar(260)**|Nom physique de l'unité de sauvegarde Sa valeur peut être NULL. Ce champ est partagé entre le processus de sauvegarde et de restauration. Il peut contenir le chemin d’accès de destination de la sauvegarde d’origine ou le chemin source d’origine de la restauration. Selon que la sauvegarde ou la restauration s’est produite en premier sur un serveur pour une base de données. Notez que les restaurations consécutives à partir du même fichier de sauvegarde ne mettent pas à jour le chemin d’accès, quel que soit son emplacement au moment de la restauration. Pour cette raison, **physical_device_name** champ ne peut pas être utilisé pour voir le chemin d’accès de restauration utilisé.|  
|**device_type**|**tinyint**|Type de l'unité de sauvegarde :<br /><br /> 2 = Disque<br /><br /> 5 = Bande<br /><br /> 7 = Unité virtuelle<br /><br /> 9 = stockage Azure<br /><br /> 105 = Unité de sauvegarde constante.<br /><br /> Sa valeur peut être NULL.<br /><br /> Tous les noms et numéros de périphérique permanents sont disponibles dans **sys.backup_devices**.|  
|**physical_block_size**|**int**|Taille du bloc physique réservé à l'écriture de la famille de supports de sauvegarde. Sa valeur peut être NULL.|  
|**mirror**|**tinyint**|Nombre de miroirs (0-3).|  
  
## <a name="remarks"></a>Notes  
 RESTORE VERIFYONLY à partir de *backup_device* avec LoadHistory remplit les colonnes de la table **backupmediaset** avec les valeurs appropriées de l’en-tête Media-Set.  
  
 Pour réduire le nombre de lignes dans cette table et dans d’autres tables de sauvegarde et d’historique, exécutez la procédure stockée [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) .  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde et restauration de tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
