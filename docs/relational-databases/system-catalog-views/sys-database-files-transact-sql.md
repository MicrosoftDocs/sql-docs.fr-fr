---
description: sys.database_files (Transact-SQL)
title: sys.database_files (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_files
- sys.database_files_TSQL
- database_files
- database_files_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_files catalog view
ms.assetid: 0f5b0aac-c17d-4e99-b8f7-d04efc9edf44
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6d58532ca43e18ae45828b9d647857e629cd7658
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195915"
---
# <a name="sysdatabase_files-transact-sql"></a>sys.database_files (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne par fichier d'une base de données telle qu'elle est stockée dans la base de données. C'est une vue par base de données.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**file_id**|**int**|ID du fichier dans la base de données.|  
|**file_guid**|**uniqueidentifier**|GUID du fichier<br /><br /> NULL = la base de données a été mise à niveau à partir d’une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (valide pour SQL Server 2005 et versions antérieures).|  
|**type**|**tinyint**|Type de fichier :<br/><br /> 0 = lignes<br /><br/> 1 = journal<br/><br /> 2 = FILESTREAM<br /><br /> 3 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 4 = texte intégral|  
|**type_desc**|**nvarchar(60)**|Description du type de fichier :<br /><br /> ROWS <br /><br /> LOG<br /><br /> FILESTREAM<br /><br /> FULLTEXT|  
|**data_space_id**|**int**|La valeur peut être supérieure ou égale à 0. La valeur 0 représente le fichier journal de la base de données et une valeur supérieure à 0 représente l'ID du groupe de fichiers où ce fichier de données est stocké.|  
|**name**|**sysname**|Nom logique du fichier dans la base de données.|  
|**physical_name**|**nvarchar(260)**|Nom de fichier du système d'exploitation. Si la base de données est hébergée par un [réplica secondaire accessible en lecture](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)AlwaysOn, **physical_name** indique l’emplacement du fichier de la base de données du réplica principal. Pour obtenir l’emplacement de fichier approprié d’une base de données secondaire accessible en lecture, interrogez [sys.sysaltfiles](../../relational-databases/system-compatibility-views/sys-sysaltfiles-transact-sql.md).|  
|**state**|**tinyint**|État du fichier :<br /><br /> 0 = ONLINE<br /><br /> 1 = RESTORING<br /><br /> 2 = RECOVERING<br /><br /> 3 = RECOVERY_PENDING<br /><br /> 4 = SUSPECT<br /><br /> 5 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 6 = OFFLINE<br /><br /> 7 = DEFUNCT|  
|**state_desc**|**nvarchar(60)**|Description de l'état du fichier :<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING<br /><br /> SUSPECT<br /><br /> OFFLINE<br /><br /> DEFUNCT<br /><br /> Pour plus d’informations, consultez [États des fichiers](../../relational-databases/databases/file-states.md).|  
|**size**|**int**|Taille actuelle du fichier (en pages de 8 Ko)<br /><br /> 0 = Non applicable<br /><br /> Dans le cas d'un instantané de base de données, size reflète l'espace maximal que celle-ci peut utiliser pour le fichier.<br /><br /> Pour les conteneurs de groupe de fichiers FILESTREAM, Size reflète la taille actuelle utilisée du conteneur.|  
|**max_size**|**int**|Taille maximale du fichier, en pages de 8 Ko :<br /><br /> 0 = aucune croissance n'est autorisée.<br /><br /> -1 = Le fichier peut croître tant que le disque n'est pas saturé.<br /><br /> 268435456 = Le fichier journal peut croître pour atteindre une taille maximale de 2 To.<br /><br /> Pour les conteneurs de groupe de fichiers FILESTREAM, max_size reflète la taille maximale du conteneur.<br /><br /> Notez que les bases de données mises à niveau avec une taille de fichier journal illimitée signalent-1 pour la taille maximale du fichier journal.|  
|**future**|**int**|0 = la taille du fichier est fixe et celui-ci ne croît pas.<br /><br /> >0 = le fichier va croître automatiquement.<br /><br /> Si is_percent_growth = 0, l'incrément de croissance est en unités de pages de 8 Ko, arrondies aux 64 kilo-octets les plus proches.<br /><br /> Si is_percent_growth a pour valeur 1, l'incrément de croissance est exprimé sous la forme d'un pourcentage à nombre entier.|  
|**is_media_read_only**|**bit**|1 = le fichier se trouve sur un support en lecture seule.<br /><br /> 0 = Le fichier se trouve sur un support en lecture-écriture.|  
|**is_read_only**|**bit**|1 = le fichier est marqué comme étant accessible en lecture seule.<br /><br /> 0 = Le fichier est marqué en lecture-écriture.|  
|**is_sparse**|**bit**|1 = le fichier est un fichier partiellement alloué.<br /><br /> 0 = le fichier n'est pas un fichier partiellement alloué.<br /><br /> Pour plus d’informations, consultez [Afficher la taille du fichier partiellement alloué d’un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md).|  
|**is_percent_growth**|**bit**|1 = la croissance du fichier est exprimée en pourcentage.<br /><br /> 0 = importance de croissance absolue en pages.|  
|**is_name_reserved**|**bit**|1 = Le nom du fichier supprimé (name ou physical_name) ne peut être réutilisé qu'après la prochaine sauvegarde du journal. Lorsque des fichiers sont supprimés d'une base de données, les noms logiques restent réservés jusqu'à la prochaine sauvegarde du journal. Cette colonne concerne uniquement le mode de restauration complète et le mode de récupération utilisant les journaux de transactions.|  
|**create_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal (LSN) auquel le fichier a été créé.|  
|**drop_lsn**|**numeric(25,0)**|LSN auquel le fichier a été supprimé.<br /><br /> 0 = Le nom de fichier n'est pas disponible pour réutilisation.|  
|**read_only_lsn**|**numeric(25,0)**|LSN auquel le groupe de fichiers qui contient le fichier est passé de l'état lecture/écriture à l'état lecture seule (changement le plus récent).|  
|**read_write_lsn**|**numeric(25,0)**|LSN auquel le groupe de fichiers qui contient le fichier est passé de l'état lecture seule à l'état lecture/écriture (changement le plus récent).|  
|**differential_base_lsn**|**numeric(25,0)**|Base des sauvegardes différentielles. Les étendues de données modifiées après ce LSN sont incluses dans une sauvegarde différentielle.|  
|**differential_base_guid**|**uniqueidentifier**|Identificateur unique de la sauvegarde de base sur laquelle repose une sauvegarde différentielle.|  
|**differential_base_time**|**datetime**|Heure correspondant à differential_base_lsn.|  
|**redo_start_lsn**|**numeric(25,0)**|LSN auquel doit démarrer la restauration par progression suivante.<br /><br /> A pour valeur NULL sauf si state a pour valeur RESTORING ou RECOVERY_PENDING.|  
|**redo_start_fork_guid**|**uniqueidentifier**|Identificateur unique du branchement de récupération. Le first_fork_guid de la prochaine sauvegarde de fichier journal restaurée doit correspondre à cette valeur. Cela représente l'état actuel du fichier.|  
|**redo_target_lsn**|**numeric(25,0)**|LSN auquel peut s'arrêter la restauration par progression en ligne sur ce fichier.<br /><br /> A pour valeur NULL sauf si state a pour valeur RESTORING ou RECOVERY_PENDING.|  
|**redo_target_fork_guid**|**uniqueidentifier**|Fourchette de récupération dans laquelle le fichier peut être récupéré. Associé à redo_target_lsn.|  
|**backup_lsn**|**numeric(25,0)**|LSN de la sauvegarde de données ou différentielle du fichier la plus récente.|  
  
> [!NOTE]  
>  Lorsque vous supprimez ou reconstruisez des index volumineux ou lorsque vous supprimez ou tronquez des tables volumineuses, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] diffère les désallocations des pages actives et de leurs blocs associés jusqu'à ce que la transaction soit validée. Les opérations de suppression différées ne libèrent pas immédiatement l'espace alloué. Par conséquent, les valeurs retournées par sys.database_files immédiatement après avoir supprimé ou tronqué un objet volumineux ne reflètent pas l'espace disque réel.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** . Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="examples"></a>Exemples  
L’instruction suivante retourne le nom, la taille du fichier et la quantité d’espace vide pour chaque fichier de base de données.

```
SELECT name, size/128.0 FileSizeInMB,
size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0 
   AS EmptySpaceInMB
FROM sys.database_files;
```
Pour plus d’informations sur l’utilisation de [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] , consultez la rubrique [détermination de la taille de base de données dans Azure SQL Database V12](/archive/blogs/sqlcat/determining-database-size-in-azure-sql-database-v12) sur le blog de l’équipe de conseil clientèle SQL.
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de bases de données et de fichiers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [États des fichiers](../../relational-databases/databases/file-states.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)   
 [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)  
  
