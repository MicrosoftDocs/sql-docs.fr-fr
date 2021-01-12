---
description: sys.dm_filestream_non_transacted_handles (Transact-SQL)
title: sys.dm_filestream_non_transacted_handles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_filestream_non_transacted_handles_TSQL
- dm_filestream_non_transacted_handles
- dm_filestream_non_transacted_handles_TSQL
- sys.dm_filestream_non_transacted_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_filestream_non_transacted_handles dynamic management view
ms.assetid: 507ec125-67dc-450a-9081-94cde5444a92
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e6b5082f5ad2105d91c23d3d1cca3e959b01343a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097639"
---
# <a name="sysdm_filestream_non_transacted_handles-transact-sql"></a>sys.dm_filestream_non_transacted_handles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche les descripteurs de fichiers non transactionnels actuellement ouverts associés aux données FileTable.  
  
 Cette vue contient une ligne par descripteur de fichier ouvert. Étant donné que les données de cette vue correspondent à l'état interne actif du serveur, les données changent constamment à mesure que les descripteurs sont ouverts et fermés. Cette vue ne contient pas d'informations d'historique.  
  
 Pour plus d’informations, consultez [Gérer des FileTables](../../relational-databases/blob/manage-filetables.md).  
  
|**Colonne**|**Type**|**Description**|  
|----------------|--------------|---------------------|  
|database_id|int|ID de la base de données associée au descripteur.|  
|object_id|int|ID d'objet du FileTable auquel le descripteur est associé.|  
|handle_id|int|Identificateur de contexte de descripteur unique. Utilisé par le [sp_kill_filestream_non_transacted_handles &#40;procédure stockée Transact-SQL&#41;](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md) pour supprimer un handle spécifique.|  
|file_object_type|int|Type du descripteur. Il indique le niveau de la hiérarchie par rapport auquel le descripteur a été ouvert, c.-à-d. base de données ou élément.|  
|file_object_type_desc|nvarchar(120)|« NON défini »,<br />« SERVER_ROOT »,<br />« DATABASE_ROOT »,<br />« TABLE_ROOT »,<br />« TABLE_ITEM »|  
|correlation_process_id|varbinary (8)|Contient un identificateur unique pour le processus d'où émane la demande.|  
|correlation_thread_id|varbinary (8)|Contient un identificateur unique pour le thread d'où émane la demande.|  
|file_context|varbinary (8)|Pointeur vers l'objet fichier utilisé par ce descripteur.|  
|state|int|État actuel du descripteur. Peut être actif, fermé ou supprimé.|  
|state_desc|nvarchar(120)|« ACTIF »,<br />« FERMÉ »,<br />DÉCÈS|  
|current_workitem_type|int|État dans lequel ce descripteur est actuellement traité.|  
|current_workitem_type_desc|nvarchar(120)|"NoSetWorkItemType",<br />"FFtPreCreateWorkitem",<br />"FFtGetPhysicalFileNameWorkitem",<br />"FFtPostCreateWorkitem",<br />"FFtPreCleanupWorkitem",<br />"FFtPostCleanupWorkitem",<br />"FFtPreCloseWorkitem",<br />"FFtQueryDirectoryWorkItem",<br />"FFtQueryInfoWorkItem",<br />"FFtQueryVolumeInfoWorkItem",<br />"FFtSetInfoWorkitem",<br />"FFtWriteCompletionWorkitem"|  
|fcb_id|bigint|ID du bloc de contrôle de fichiers du FileTable.|  
|item_id|varbinary (892)|ID d'élément d'un fichier ou répertoire. Peut être Null pour les descripteurs racine du serveur.|  
|is_directory|bit|Il s'agit d'un répertoire.|  
|item_name|nvarchar(512)|Nom de l'élément.|  
|opened_file_name|nvarchar(512)|Chemin d'accès à ouvrir demandé à l'origine.|  
|database_directory_name|nvarchar(512)|Partie d'opened_file_name qui représente le nom de répertoire de base de données.|  
|table_directory_name|nvarchar(512)|Partie d'opened_file_name qui représente le nom de répertoire de table.|  
|remaining_file_name|nvarchar(512)|Partie d'opened_file_name qui représente le nom de répertoire restant.|  
|open_time|DATETIME|Heure à laquelle le descripteur a été ouvert.|  
|flags|int|ShareFlagsUpdatedToFcb = 0x1,<br />DeleteOnClose = 0x2,<br />NewFile = 0x4,<br />PostCreateDoneForNewFile = 0x8,<br />StreamFileOverwritten = 0x10,<br />RequestCancelled = 0x20,<br />NewFileCreationRolledBack = 0x40|  
|login_id|int|ID du principal qui a ouvert le descripteur.|  
|login_name|nvarchar(512)|Nom du principal qui a ouvert le descripteur.|  
|login_sid|varbinary(85)|SID du principal qui a ouvert le descripteur.|  
|read_access|bit|Ouvert pour l'accès en lecture.|  
|write_access|bit|Ouvert pour l'accès en écriture.|  
|delete_access|bit|Ouvert pour l'accès en suppression.|  
|share_read|bit|Ouvert avec share_read autorisé.|  
|share_write|bit|Ouvert avec share_write autorisé.|  
|share_delete|bit|Ouvert avec share_delete autorisé.|  
  
## <a name="see-also"></a>Voir aussi  
 [Gérer des FileTables](../../relational-databases/blob/manage-filetables.md)  
  
  
