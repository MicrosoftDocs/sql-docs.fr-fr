---
description: sys.dm_db_log_info (Transact-SQL)
title: sys.dm_db_log_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_log_info
- sys.dm_db_log_info_TSQL
- dm_db_log_info
- dm_db_log_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_info dynamic management view
ms.assetid: f6b40060-c17d-472f-b0a3-3b350275d487
author: savjani
ms.author: pariks
manager: ajayj
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5cb1966c7b861c60bf9cd7106480ff4b30e4844b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071627"
---
# <a name="sysdm_db_log_info-transact-sql"></a>sys.dm_db_log_info (Transact-SQL)
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Retourne les informations de [fichier journal virtuel (fichier journal virtuel)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) du journal des transactions. Notez que tous les fichiers journaux des transactions sont combinés dans la sortie de la table. Chaque ligne de la sortie représente un fichier journal virtuel dans le journal des transactions et fournit des informations relatives à ce fichier journal virtuel dans le journal.

## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_db_log_info ( database_id )  
``` 

## <a name="arguments"></a>Arguments  
 *database_id* | NULL | VALEURS  
 ID de la base de données. *database_id* est de **type int**. Les entrées valides sont le numéro d’identification d’une base de données, la valeur NULL ou la valeur par défaut. La valeur par défaut est NULL. NULL et DEFAULT sont des valeurs équivalentes dans le contexte de la base de données active.
 
 Spécifiez NULL pour retourner les informations de fichier journal virtuel de la base de données actuelle.

 Vous pouvez spécifier la fonction intégrée [DB_ID](../../t-sql/functions/db-id-transact-sql.md). Lorsque vous utilisez `DB_ID` sans spécifier de nom de base de données, le niveau de compatibilité de la base de données actuelle doit être supérieur ou égal à 90.  

## <a name="table-returned"></a>Table retournée  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID de la base de données.|
|file_id|**smallint**|ID de fichier du journal des transactions.|  
|vlf_begin_offset|**bigint** |Emplacement de décalage du [fichier journal virtuel (fichier journal virtuel)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) à partir du début du fichier journal des transactions.|
|vlf_size_mb |**float** |taille du [fichier journal virtuel (fichier journal virtuel)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) en Mo, arrondie à 2 décimales.|     
|vlf_sequence_number|**bigint** |Numéro de séquence du [fichier journal virtuel (fichier journal virtuel)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) dans l’ordre de création. Utilisé pour identifier de manière unique fichiers journaux virtuels dans le fichier journal.|
|vlf_active|**bit** |Indique si le [fichier journal virtuel (fichier journal virtuel)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) est en cours d’utilisation ou non. <br />0-fichier journal virtuel n’est pas en cours d’utilisation.<br />1-fichier journal virtuel est actif.|
|vlf_status|**int** |État du [fichier journal virtuel (fichier journal virtuel)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch). Les valeurs possibles sont les suivants : <br />0-fichier journal virtuel est inactif <br />1-fichier journal virtuel est initialisé mais non utilisé <br /> 2-fichier journal virtuel est actif.|
|vlf_parity|**tinyint** |Parité du [fichier journal virtuel (fichier journal virtuel)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch). Utilisé en interne pour déterminer la fin du journal au sein d’un fichier journal virtuel.|
|vlf_first_lsn|**nvarchar (48)** |[Numéro séquentiel dans le journal (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) du premier enregistrement du journal dans le [fichier journal virtuel (fichier journal virtuel)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch).|
|vlf_create_lsn|**nvarchar (48)** |[Numéro séquentiel dans le journal (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) de l’enregistrement de journal qui a créé le [fichier journal virtuel (fichier journal virtuel)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch).|
|vlf_encryptor_thumbprint|**varbinary(20)**| **S’applique à :** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] <br><br> Affiche l’empreinte numérique du chiffreur du fichier journal virtuel si le fichier journal virtuel est chiffré à l’aide de [transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md), sinon null. |

## <a name="remarks"></a>Notes
La `sys.dm_db_log_info` fonction de gestion dynamique remplace l' `DBCC LOGINFO` instruction.    
 
## <a name="permissions"></a>Autorisations  
Nécessite l' `VIEW DATABASE STATE` autorisation dans la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-determing-databases-in-a-sql-server-instance-with-high-number-of-vlfs"></a>R. Détermination des bases de données dans une instance SQL Server avec un nombre élevé de fichiers journaux virtuels
La requête suivante détermine les bases de données avec plus de 100 fichiers journaux virtuels dans les fichiers journaux, ce qui peut affecter le temps de démarrage, de restauration et de récupération de la base de données.

```sql
SELECT [name], COUNT(l.database_id) AS 'vlf_count' 
FROM sys.databases s
CROSS APPLY sys.dm_db_log_info(s.database_id) l
GROUP BY [name]
HAVING COUNT(l.database_id) > 100
```

### <a name="b-determing-the-position-of-the-last-vlf-in-transaction-log-before-shrinking-the-log-file"></a>B. Détermination de la position de la dernière `VLF` dans le journal des transactions avant de réduire le fichier journal

La requête suivante peut être utilisée pour déterminer la position du dernier fichier journal virtuel actif avant d’exécuter SHRINKFILE dans le journal des transactions pour déterminer si le journal des transactions peut être réduit.

```sql
USE AdventureWorks2016
GO

;WITH cte_vlf AS (
SELECT ROW_NUMBER() OVER(ORDER BY vlf_begin_offset) AS vlfid, DB_NAME(database_id) AS [Database Name], vlf_sequence_number, vlf_active, vlf_begin_offset, vlf_size_mb
    FROM sys.dm_db_log_info(DEFAULT)),
cte_vlf_cnt AS (SELECT [Database Name], COUNT(vlf_sequence_number) AS vlf_count,
    (SELECT COUNT(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 0) AS vlf_count_inactive,
    (SELECT COUNT(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS vlf_count_active,
    (SELECT MIN(vlfid) FROM cte_vlf WHERE vlf_active = 1) AS ordinal_min_vlf_active,
    (SELECT MIN(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS min_vlf_active,
    (SELECT MAX(vlfid) FROM cte_vlf WHERE vlf_active = 1) AS ordinal_max_vlf_active,
    (SELECT MAX(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS max_vlf_active
    FROM cte_vlf
    GROUP BY [Database Name])
SELECT [Database Name], vlf_count, min_vlf_active, ordinal_min_vlf_active, max_vlf_active, ordinal_max_vlf_active,
((ordinal_min_vlf_active-1)*100.00/vlf_count) AS free_log_pct_before_active_log,
((ordinal_max_vlf_active-(ordinal_min_vlf_active-1))*100.00/vlf_count) AS active_log_pct,
((vlf_count-ordinal_max_vlf_active)*100.00/vlf_count) AS free_log_pct_after_active_log
FROM cte_vlf_cnt
GO
```

## <a name="see-also"></a>Voir aussi  
[Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)   
[sys.dm_db_log_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-stats-transact-sql.md)

