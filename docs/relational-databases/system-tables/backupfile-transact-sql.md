---
description: backupfile (Transact-SQL)
title: backupfile (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- backupfile
- backupfile_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- file backups [SQL Server], backupfile system table
- backupfile system table
ms.assetid: f1a7fc0a-f4b4-47eb-9138-eebf930dc9ac
author: cawrites
ms.author: chadam
ms.openlocfilehash: aba3bb5753569f0d75c8f8f8627beb31b5a68474
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211302"
---
# <a name="backupfile-transact-sql"></a>backupfile (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque fichier de données ou fichier journal d'une base de données. Les colonnes décrivent la configuration des fichiers au moment où la sauvegarde a été effectuée. Le fait que le fichier soit inclus ou non dans la sauvegarde est déterminé par la colonne **is_present** . Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**backup_set_id**|**int**|Numéro d'identification unique du fichier contenant le jeu de sauvegarde. Fait référence à une **backup_set_id**.|  
|**first_family_number**|**tinyint**|Numéro de famille du premier support contenant ce fichier de sauvegarde Sa valeur peut être NULL.|  
|**first_media_number**|**smallint**|Numéro de support du premier support contenant ce fichier de sauvegarde. Sa valeur peut être NULL.|  
|**filegroup_name**|**nvarchar(128)**|Nom du groupe de fichiers contenant un fichier de base de données sauvegardée. Sa valeur peut être NULL.|  
|**page_size**|**int**|Taille de la page en octets.|  
|**file_number**|**numérique (10, 0)**|Numéro d’identification de fichier unique au sein d’une base de données (correspond à **sys.database_files**.**file_id**).|  
|**backed_up_page_count**|**numérique (10, 0)**|Nombre de pages sauvegardées. Sa valeur peut être NULL.|  
|**file_type**|**char(1)**|Fichier sauvegardé, avec une des valeurs suivantes :<br /><br /> D = Fichier de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> L = Journal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> F = Catalogue de texte intégral.<br /><br /> Sa valeur peut être NULL.|  
|**source_file_block_size**|**numérique (10, 0)**|Unité sur laquelle le fichier de données ou le fichier journal d'origine se trouvaient au moment de la sauvegarde. Sa valeur peut être NULL.|  
|**file_size**|**numeric(20,0)**|Longueur en octets du fichier sauvegardé. Sa valeur peut être NULL.|  
|**logical_name**|**nvarchar(128)**|Nom logique du fichier sauvegardé. Sa valeur peut être NULL.|  
|**physical_drive**|**nvarchar(260)**|Nom de l'unité physique ou de la partition. Sa valeur peut être NULL.|  
|**physical_name**|**nvarchar(260)**|Suite du nom physique du fichier (système d'exploitation). Sa valeur peut être NULL.|  
|**state**|**tinyint**|État du fichier, avec une des valeurs suivantes :<br /><br /> 0 = ONLINE<br /><br /> 1 = RESTORING<br /><br /> 2 = RECOVERING<br /><br /> 3 = RECOVERY PENDING<br /><br /> 4 = SUSPECT<br /><br /> 6 = OFFLINE<br /><br /> 7 = DEFUNCT<br /><br /> 8 = SUPPRIMÉ<br /><br /> Remarque : la valeur 5 est ignorée afin que ces valeurs correspondent aux valeurs des États de base de données.|  
|**state_desc**|**nvarchar (64)**|Description de l'état du fichier, avec une des valeurs suivantes :<br /><br /> ONLINE RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING<br /><br /> SUSPECT OFFLINE DEFUNCT|  
|**create_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal auquel le fichier a été créé.|  
|**drop_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal auquel le fichier a été supprimé. Sa valeur peut être NULL.<br /><br /> Si le fichier n'a pas été supprimé, cette valeur est NULL.|  
|**file_guid**|**uniqueidentifier**|Identificateur unique du fichier.|  
|**read_only_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal auquel le groupe de fichiers contenant le fichier est passé de lecture-écriture à lecture seule (modification la plus récente). Sa valeur peut être NULL.|  
|**read_write_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal auquel le groupe de fichiers contenant le fichier est passé de lecture seule à lecture-écriture (modification la plus récente). Sa valeur peut être NULL.|  
|**differential_base_lsn**|**numeric(25,0)**|Numéro de séquence d'enregistrement de base pour les sauvegardes différentielles. Une sauvegarde différentielle comprend uniquement des étendues de données dont le numéro séquentiel dans le journal est supérieur ou égal à **differential_base_lsn**.<br /><br /> Pour les autres types de sauvegarde, la valeur est NULL.|  
|**differential_base_guid**|**uniqueidentifier**|Pour une sauvegarde différentielle, il s'agit de l'identificateur unique de la sauvegarde de données la plus récente qui compose la base différentielle du fichier ; si la valeur est NULL, le fichier a été inclus dans la sauvegarde différentielle, mais il a été ajouté après la création de la base.<br /><br /> Pour les autres types de sauvegarde, la valeur est NULL.|  
|**backup_size**|**numeric(20,0)**|Taille de la sauvegarde en octets pour ce fichier.|  
|**filegroup_guid**|**uniqueidentifier**|Identificateur du groupe de fichiers. Pour rechercher des informations de groupe de fichiers dans la table backupfilegroup, utilisez **filegroup_guid** avec **backup_set_id**.|  
|**is_readonly**|**bit**|1 = Le fichier est en lecture seule.|  
|**is_present**|**bit**|1 = Le fichier est contenu dans le jeu de sauvegarde.|  
  
## <a name="remarks"></a>Notes  
 RESTORE VERIFYONLY à partir de *backup_device* avec LoadHistory remplit les colonnes de la table **backupmediaset** avec les valeurs appropriées de l’en-tête Media-Set.  
  
 Pour réduire le nombre de lignes dans cette table et dans d’autres tables de sauvegarde et d’historique, exécutez la procédure stockée [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) .  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde et restauration de tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
