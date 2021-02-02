---
description: sys.system_columns (Transact-SQL)
title: sys.system_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- system_columns_TSQL
- system_columns
- sys.system_columns
- sys.system_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_columns catalog view
ms.assetid: 4ab1d48a-d57a-4e76-a08c-9627eeaf4588
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 92d65f7307e96cb1261688a5da3443c3d4861f84
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236167"
---
# <a name="syssystem_columns-transact-sql"></a>sys.system_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne pour chaque colonne des objets système ayant des colonnes.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Identificateur de l'objet auquel appartient cette colonne.|  
|**name**|**sysname**|Nom de la colonne. Unique dans l'objet.|  
|**column_id**|**int**|Identificateur de la colonne. Unique dans l'objet.<br /><br /> Les ID de colonnes peuvent ne pas être séquentiels.|  
|**system_type_id**|**tinyint**|ID du type de système de la colonne|  
|**user_type_id**|**int**|ID du type de colonne tel que défini par l'utilisateur.<br /><br /> Pour retourner le nom du type, Joignez-vous à l’affichage catalogue [sys. types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) sur cette colonne.|  
|**max_length**|**smallint**|Longueur maximale (en octets) de la colonne.<br /><br /> -1 = le type de données de la colonne est **varchar (max)**, **nvarchar (max)**, **varbinary (max)** ou **XML**.<br /><br /> Pour les colonnes de **texte** , la valeur **max_length** sera 16 ou la valeur définie par **sp_tableoption** 'texte dans la ligne'.|  
|**precision**|**tinyint**|Précision de la colonne si elle est numérique ; sinon, 0.|  
|**scale**|**tinyint**|Échelle de la colonne si numérique ; sinon, 0.|  
|**collation_name**|**sysname**|Nom du classement de la colonne si elle est basée sur des caractères ; sinon, NULL.|  
|**is_nullable**|**bit**|1 = La colonne accepte les valeurs NULL.|  
|**is_ansi_padded**|**bit**|1 = La colonne utilise le comportement ANSI_PADDING ON si elle est de type caractère, binaire ou variant.<br /><br /> 0 = La colonne n'est pas de type caractère, binaire ou variant.|  
|**is_rowguidcol**|**bit**|1 = La colonne est un ROWGUIDCOL déclaré.|  
|**is_identity**|**bit**|1 = La colonne comporte des valeurs d'identité.|  
|**is_computed**|**bit**|1 = La colonne est calculée.|  
|**is_filestream**|**bit**|1 = La colonne est déclarée utiliser un stockage de flux de fichier.|  
|**is_replicated**|**bit**|1 = La colonne est répliquée.|  
|**is_non_sql_subscribed**|**bit**|1 = La colonne possède un abonné non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**is_merge_published**|**bit**|1 = La colonne est associée à une publication fusionnée.|  
|**is_dts_replicated**|**bit**|1 = La colonne est répliquée à l'aide de [!INCLUDE[ssIS](../../includes/ssis-md.md)].|  
|**is_xml_document**|**bit**|1 = Le contenu est un document XML complet.<br /><br /> 0 = le contenu est un fragment de document ou le type de données de la colonne n’est pas **XML**.|  
|**xml_collection_id**|**int**|Valeur différente de zéro si le type de données de la colonne est **XML** et que le XML est typé. La valeur correspondra à l'ID de la collection qui contient l'espace de noms de schéma XML de validation de la colonne.<br /><br /> 0 = Aucune collection de schéma XML.|  
|**default_object_id**|**int**|ID de l’objet par défaut, qu’il s’agisse d’un [sys.sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)autonome ou d’une contrainte par défaut en ligne de niveau colonne. La **parent_object_id** colonne d’un objet par défaut au niveau de la colonne Inline fait référence à la table elle-même. Il aura la valeur 0 en l'absence de valeur par défaut.|  
|**rule_object_id**|**int**|ID de la règle autonome liée à la colonne à l’aide de **sys.sp_bindrule**.<br /><br /> 0 = Aucune règle autonome.<br /><br /> Pour les contraintes de vérification au niveau des colonnes, consultez [sys.check_constraints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md).|  
|is_sparse|**bit**|1 = La colonne est éparse. Pour plus d’informations, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).|  
|is_column_set|**bit**|1 = La colonne est un jeu de colonnes. Pour plus d’informations, consultez [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).|  
|generated_always_type|**tinyint**|Valeur numérique représentant le type de colonne :<br /><br /> 0 = NOT_APPLICABLE<br /><br /> 1 = AS_ROW_START<br /><br /> 2 = AS_ROW_END|  
|generated_always_type_desc|**nvarchar(60)**|Description textuelle du type de colonne :<br /><br /> NOT_APPLICABLE<br /><br /> AS_ROW_START<br /><br /> AS_ROW_END<br /><br /> **S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)  
  
  
