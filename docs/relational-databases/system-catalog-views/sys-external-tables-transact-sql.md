---
description: sys.external_tables (Transact-SQL)
title: sys.external_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: fac4720c-b679-4ab2-864b-ff7810a9b559
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 443086b2deb37b8c1cce1170e2869fe09db81560
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098347"
---
# <a name="sysexternal_tables-transact-sql"></a>sys.external_tables (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne pour chaque table externe de la base de données active.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|\<inherited columns>||Pour obtenir la liste des colonnes héritées par cette vue, consultez [sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).||  
|max_column_id_used|**int**|ID de colonne maximal jamais utilisé pour cette table.||  
|uses_ansi_nulls|**bit**|Lorsque la table a été créée, l'option de base de données SET ANSI_NULLS avait pour valeur ON.||  
|data_source_id|**int**|ID d’objet de la source de données externe.||  
|file_format_id|**int**|Pour les tables externes sur une source de données externe HADOOP, il s’agit de l’ID d’objet pour le format de fichier externe.||  
|location|**nvarchar(4000)**|Pour les tables externes sur une source de données externe HADOOP, il s’agit du chemin d’accès aux données externes dans HDFS.||  
|reject_type|**tinyint**|Pour les tables externes sur une source de données externe HADOOP, il s’agit de la façon dont les lignes rejetées sont comptabilisées lors de l’interrogation de données externes.|VALEUR : nombre de lignes rejetées.<br /><br /> POURCENTAGE-pourcentage de lignes rejetées.|  
|reject_value|**float**|Pour les tables externes sur une source de données externe HADOOP :<br /><br /> Pour *reject_type =* value, il s’agit du nombre de rejets de ligne à autoriser avant l’échec de la requête.<br /><br /> Pour *reject_type* = Percentage, il s’agit du pourcentage de rejets de ligne à autoriser avant l’échec de la requête.||  
|reject_sample_value|**int**|Pour *reject_type* = Percentage, il s’agit du nombre de lignes à charger, soit avec succès, soit sans succès, avant le calcul du pourcentage de lignes rejetées.|NULL si reject_type = VALUE.|  
|distribution_type|**int**|Pour les tables externes sur une source de données externe SHARD_MAP_MANAGER, il s’agit de la distribution des données des lignes sur les tables de base sous-jacentes.|0-partitionnée<br /><br /> 1-répliqué<br /><br /> 2-tourniquet (Round Robin)|  
|distribution_desc|**nvarchar(120)**|Pour les tables externes sur une source de données externe SHARD_MAP_MANAGER, il s’agit du type de distribution affiché sous forme de chaîne.||  
|sharding_column_id|**int**|Pour les tables externes sur une source de données externe SHARD_MAP_MANAGER et une distribution partitionnée, il s’agit de l’ID de colonne de la colonne qui contient les valeurs de clé partitionnement.||  
|remote_schema_name|**sysname**|Pour les tables externes sur une source de données externe SHARD_MAP_MANAGER, il s’agit du schéma dans lequel se trouve la table de base sur les bases de données distantes (si elles diffèrent du schéma dans lequel la table externe est définie).||  
|remote_object_name|**sysname**|Pour les tables externes sur une source de données externe SHARD_MAP_MANAGER, il s’agit du nom de la table de base sur les bases de données distantes (s’il est différent du nom de la table externe).||  
  
## <a name="permissions"></a>Autorisations  
 La visibilité des métadonnées dans les affichages catalogue est limitée aux éléments sécurisables qu'un utilisateur détient ou pour lesquels des autorisations lui ont été accordées. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [sys.external_file_formats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)   
 [sys.external_data_sources &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)  
  
  
