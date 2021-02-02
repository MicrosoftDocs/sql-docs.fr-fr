---
description: sys.dm_db_page_info (Transact-SQL)
title: sys.dm_db_page_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/18/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_page_info
- sys.dm_db_page_info_TSQL
- dm_db_page_info
- dm_db_page_info_TSQL
- dbcc page
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_page_info dynamic management view
author: bluefooted
ms.author: pamela
manager: amitban
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 10756b0297cdc731468ea8983c5378d5b380fef9
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236016"
---
# <a name="sysdm_db_page_info-transact-sql"></a>sys.dm_db_page_info (Transact-SQL)

[!INCLUDE[tsql-appliesto-ssver15-asdb-xxxx-xxx](../../includes/tsql-appliesto-ssver15-asdb-xxxx-xxx.md)]

Retourne des informations sur une page d’une base de données.  La fonction retourne une ligne qui contient les informations d’en-tête de la page, y compris `object_id` , `index_id` et `partition_id` .  Cette fonction rend superflue l’utilisation de `DBCC PAGE` dans la plupart des cas.

> [!NOTE]
> `sys.dm_db_page_info` est actuellement pris en charge uniquement dans [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] et versions ultérieures.


## <a name="syntax"></a>Syntaxe   
```  
sys.dm_db_page_info ( DatabaseId, FileId, PageId, Mode )  
``` 

## <a name="arguments"></a>Arguments  
*DatabaseID* | NULL | VALEURS     
ID de la base de données. *DatabaseID* est de type **smallint**. L’entrée valide est le numéro d’identification d’une base de données. La valeur par défaut est NULL, mais l’envoi d’une valeur NULL pour ce paramètre génère une erreur.
 
*Fileid* | NULL | VALEURS   
Identificateur du fichier. *Fileid* est de **type int**.  Entrée valide est le numéro d’identification d’un fichier dans la base de données spécifiée par *DatabaseID*. La valeur par défaut est NULL, mais l’envoi d’une valeur NULL pour ce paramètre génère une erreur.

*Pageid* | NULL | VALEURS   
ID de la page.  *Pageid* est de **type int**.  Entrée valide est le numéro d’identification d’une page dans le fichier spécifié par *fileid*. La valeur par défaut est NULL, mais l’envoi d’une valeur NULL pour ce paramètre génère une erreur.

*Mode* | NULL | VALEURS   
Détermine le niveau de détail dans la sortie de la fonction. 'LIMITED’renverra des valeurs NULL pour toutes les colonnes de description, 'DETAILed’remplira les colonnes de description.  La valeur par défaut est « LIMITED ».

## <a name="table-returned"></a>Table retournée  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|database_id |int |ID de base de données |
|file_id |int |ID de fichier |
|page_id |int |ID de page |
|page_header_version |int |Version d’en-tête de page |
|page_type |int |Type de page |
|page_type_desc |nvarchar (64) |Description du type de page |
|page_type_flag_bits |nvarchar (64) |Taper les bits des indicateurs dans l’en-tête de page |
|page_type_flag_bits_desc |nvarchar (64) |Entrer l’indicateur de description bits dans l’en-tête de page |
|page_flag_bits |nvarchar (64) |Marquer les bits dans l’en-tête de page |
|page_flag_bits_desc |nvarchar(256) |Baliser la description bits dans l’en-tête de page |
|page_lsn |nvarchar (64) |Numéro séquentiel dans le journal/horodateur |
|page_level |int |Niveau de la page dans l’index (feuille = 0) |
|object_id |int |ID de l’objet propriétaire de la page |
|index_id |int |ID de l’index (0 pour les pages de données du tas) |
|partition_id |bigint |ID de la partition |
|alloc_unit_id |bigint |ID de l’unité d’allocation |
|is_encrypted |bit |Bit pour indiquer si la page est chiffrée ou non |
|has_checksum |bit |Bit pour indiquer si la page a une valeur de somme de contrôle |
|somme de contrôle |int |Stocke la valeur de somme de contrôle utilisée pour détecter les données endommagées |
|is_iam_pg |bit |Bit pour indiquer si la page est une page IAM  |
|is_mixed_ext |bit |Bit pour indiquer si alloué dans une extension mixte |
|has_ghost_records |bit |Bit pour indiquer si la page contient des enregistrements fantômes <br> Un enregistrement fantôme est un enregistrement qui a été marqué pour suppression mais qui n’a pas encore été supprimé.|
|has_version_records |bit |Bit indiquant si la page contient des enregistrements de version utilisés pour la [récupération accélérée de la base de données](../backup-restore/restore-and-recovery-overview-sql-server.md#adr) |
|pfs_page_id |int |ID de page de la page PFS correspondante |
|pfs_is_allocated |bit |Bit indiquant si la page est marquée comme allouée dans la page PFS correspondante |
|pfs_alloc_percent |int |Pourcentage d’allocation comme indiqué par l’octet PFS correspondant |
|pfs_status |nvarchar (64) |Octet PFS |
|pfs_status_desc |nvarchar (64) |Description de l’octet PFS |
|gam_page_id |int |ID de page de la page GAM correspondante |
|gam_status |bit |Bit pour indiquer si alloué dans GAM |
|gam_status_desc |nvarchar (64) |Description du bit d’État GAM |
|sgam_page_id |int |ID de page de la page SGAM correspondante |
|sgam_status |bit |Bit pour indiquer si alloué dans SGAM |
|sgam_status_desc |nvarchar (64) |Description du bit d’État SGAM |
|diff_map_page_id |int |ID de page de la page bitmap différentielle correspondante |
|diff_status |bit |Bit pour indiquer si l’État diff est modifié |
|diff_status_desc |nvarchar (64) |Description du bit d’État diff |
|ml_map_page_id |int |ID de page de la page bitmap de journalisation minimale correspondante |
|ml_status |bit |Bit indiquant si la page est journalisée au minimum |
|ml_status_desc |nvarchar (64) |Description du bit d’État minimal de la journalisation |
|prev_page_file_id |SMALLINT |ID du fichier de page précédente |
|prev_page_page_id |int |ID de page de page précédente |
|next_page_file_id |SMALLINT |ID de fichier de la page suivante |
|next_page_page_id |int |ID de page de page suivante |
|fixed_length |SMALLINT |Longueur des lignes de taille fixe |
|slot_count |SMALLINT |Nombre total d’emplacements (utilisés et inutilisés) <br> Pour une page de données, ce nombre est équivalent au nombre de lignes. |
|ghost_rec_count |SMALLINT |Nombre d’enregistrements marqués comme fantômes sur la page <br> Un enregistrement fantôme est un enregistrement qui a été marqué pour suppression mais qui n’a pas encore été supprimé. |
|free_bytes |SMALLINT |Nombre d’octets disponibles sur la page |
|free_data_offset |int |Décalage de l’espace libre à la fin de la zone de données |
|reserved_bytes |SMALLINT |Nombre d’octets libres réservés par toutes les transactions (si Heap) <br> Nombre de lignes dupliquées (sous la feuille d’index) |
|reserved_bytes_by_xdes_id |SMALLINT |Espace fourni par m_xdesID à m_reservedCnt <br> À des fins de débogage uniquement |
|xdes_id |nvarchar (64) |Dernière transaction fournie par m_reserved <br> À des fins de débogage uniquement |
||||

## <a name="remarks"></a>Notes
La `sys.dm_db_page_info` fonction de gestion dynamique retourne des informations `page_id` de page comme, `file_id` , `index_id` , `object_id` etc. qui sont présentes dans un en-tête de page. Ces informations sont utiles pour le dépannage et le débogage des performances (verrouillage et contention de verrous internes) et des problèmes d’altération.

`sys.dm_db_page_info` peut être utilisé à la place de l' `DBCC PAGE` instruction dans de nombreux cas, mais retourne uniquement les informations d’en-tête de page, et non le corps de la page. `DBCC PAGE` est toujours nécessaire pour les cas d’usage où le contenu entier de la page est requis.

## <a name="using-in-conjunction-with-other-dmvs"></a>Utilisation conjointe avec d’autres DMV
L’un des cas d’usage importants de `sys.dm_db_page_info` consiste à le joindre à d’autres DMV qui exposent les informations de la page.  Pour faciliter ce cas d’utilisation, une nouvelle colonne nommée `page_resource` a été ajoutée, qui expose les informations de page dans un format hexadécimal de 8 octets. Cette colonne a été ajoutée à `sys.dm_exec_requests` et `sys.sysprocesses` et sera ajoutée à d’autres DMV à l’avenir en fonction des besoins.

Une nouvelle fonction, `sys.fn_PageResCracker` , prend le `page_resource` comme entrée et génère une ligne unique qui contient `database_id` `file_id` et `page_id` .  Cette fonction peut ensuite être utilisée pour faciliter les jointures entre `sys.dm_exec_requests` ou `sys.sysprocesses` et `sys.dm_db_page_info` .

## <a name="permissions"></a>Autorisations  
Nécessite l' `VIEW DATABASE STATE` autorisation dans la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-all-the-properties-of-a-page"></a>R. Affichage de toutes les propriétés d’une page
La requête suivante retourne une ligne avec toutes les informations de page pour une `database_id` combinaison donnée, `file_id` , `page_id` avec le mode par défaut ('Limited')

```sql
SELECT *  
FROM sys.dm_db_page_info (5, 1, 15, DEFAULT)
```

### <a name="b-using-sysdm_db_page_info-with-other-dmvs"></a>B. Utilisation de sys.dm_db_page_info avec d’autres DMV 

La requête suivante retourne une ligne par `wait_resource` exposée par `sys.dm_exec_requests` lorsque la ligne contient une valeur non null `page_resource`

```sql
SELECT page_info.* 
FROM sys.dm_exec_requests AS d  
CROSS APPLY sys.fn_PageResCracker (d.page_resource) AS r  
CROSS APPLY sys.dm_db_page_info(r.db_id, r.file_id, r.page_id, 'LIMITED') AS page_info
```

## <a name="see-also"></a>Voir aussi  
[Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)     
[sys.fn_PageResCracker](../../relational-databases/system-functions/sys-fn-pagerescracker-transact-sql.md)


