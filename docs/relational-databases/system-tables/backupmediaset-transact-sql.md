---
title: backupmediaset (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- backupmediaset
- backupmediaset_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], backupmediaset system table
- backupmediaset system table
ms.assetid: d9c18a93-cab9-4db8-ae09-c6bd8145ab8f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fded40f11cfc094e3af89295496787413e3fd4cd
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89540367"
---
# <a name="backupmediaset-transact-sql"></a>backupmediaset (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque support de sauvegarde. Cette table est stockée dans la base de données **msdb** .  
 
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**media_set_id**|**int**|Numéro d'identification unique du support de sauvegarde. Identité, clé primaire.|  
|**media_uuid**|**uniqueidentifier**|Identificateur UUID du support de sauvegarde. Tous les [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] jeux de supports ont un UUID.<br /><br /> Toutefois, pour les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , si un support de sauvegarde contient une seule famille de supports, la colonne **media_uuid** peut avoir la valeur null (**media_family_count** 1).|  
|**media_family_count**|**tinyint**|Nombre de familles de supports dans le support de sauvegarde. Sa valeur peut être NULL.|  
|**name**|**nvarchar(128)**|Nom du support de sauvegarde. Sa valeur peut être NULL.<br /><br /> Pour plus d’informations, consultez MEDIANAME et MEDIADESCRIPTION dans [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).|  
|**description**|**nvarchar(255)**|Description textuelle du support de sauvegarde. Sa valeur peut être NULL.<br /><br /> Pour plus d’informations, consultez MEDIANAME et MEDIADESCRIPTION dans [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).|  
|**software_name**|**nvarchar(128)**|Nom du logiciel de sauvegarde qui a écrit l’étiquette du support. Sa valeur peut être NULL.|  
|**software_vendor_id**|**int**|Numéro d'identification du fournisseur du logiciel qui a écrit l'étiquette du support de sauvegarde. Sa valeur peut être NULL.<br /><br /> La valeur de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est hexadécimale, 0x1200.|  
|**MTF_major_version**|**tinyint**|Numéro de la version principale de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Tape Format utilisée pour créer ce support de sauvegarde. Sa valeur peut être NULL.|  
|**mirror_count**|**tinyint**|Nombre de miroirs dans le support de sauvegarde.|  
|**is_password_protected**|**bit**|Mot de passe protégé du support de sauvegarde :<br /><br /> 0 = Non protégé<br /><br /> 1 = Protégé|  
|**is_compressed**|**bit**|Indique si la sauvegarde est compressée :<br /><br /> 0 = Non compressée<br /><br /> 1 = Compressée<br /><br /> Au cours d’une mise à niveau de **msdb** , cette valeur est définie sur null. ce qui indique une sauvegarde non compressée.|  
|**is_encrypted**|**64bits**|Indique si la sauvegarde est chiffrée :<br /><br /> 0 = Non chiffrée<br /><br /> 1 = Chiffrée|  
  
## <a name="remarks"></a>Notes  
 RESTORE VERIFYONLY à partir de *backup_device* avec LoadHistory remplit les colonnes de la table **backupmediaset** avec les valeurs appropriées de l’en-tête Media-Set.  
  
 Pour réduire le nombre de lignes dans cette table et dans d’autres tables de sauvegarde et d’historique, exécutez la procédure stockée [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) .  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde et restauration de tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
