---
description: sys.all_columns (Transact-SQL)
title: sys.all_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- all_columns_TSQL
- all_columns
- sys.all_columns_TSQL
- sys.all_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.all_columns catalog view
ms.assetid: 40e04fe9-0b64-4799-84c0-57f128b2bdc2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 030c3ce52f1707f71d007e5ece73b4fe92c7fd7a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102250"
---
# <a name="sysall_columns-transact-sql"></a>sys.all_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Affiche l'union de toutes les colonnes appartenant aux objets définis par l'utilisateur et aux objets système.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Identificateur de l'objet auquel appartient cette colonne.|  
|name|**sysname**|Nom de la colonne. Unique dans l'objet.|  
|column_id|**int**|Identificateur de la colonne. Unique dans l'objet.<br /><br /> Les ID de colonnes peuvent ne pas être séquentiels.|  
|system_type_id|**tinyint**|ID du type système de la colonne.|  
|user_type_id|**int**|ID du type de colonne tel que défini par l'utilisateur.<br /><br /> Pour retourner le nom du type, Joignez-vous à l’affichage catalogue [sys. types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) sur cette colonne.|  
|max_length|**smallint**|Longueur maximale (en octets) de la colonne.<br /><br /> -1 = le type de données de la colonne est **varchar (max)**, **nvarchar (max)**, **varbinary (max)** ou **XML**.<br /><br /> Pour les colonnes de **texte** , la valeur max_length sera 16 ou la valeur définie par sp_tableoption’texte dans la ligne'.|  
|précision|**tinyint**|Précision de la colonne si elle est numérique ; sinon, 0.|  
|scale|**tinyint**|Échelle de la colonne si numérique ; sinon, 0.|  
|collation_name|**sysname**|Nom du classement de la colonne si elle est basée sur des caractères ; sinon, NULL.|  
|is_nullable|**bit**|1 = La colonne accepte les valeurs NULL.|  
|is_ansi_padded|**bit**|1 = La colonne utilise le comportement ANSI_PADDING ON si elle est de type caractère, binaire ou variant.<br /><br /> 0 = La colonne n'est pas de type caractère, binaire ou variant.|  
|is_rowguidcol|**bit**|1 = La colonne est un ROWGUIDCOL déclaré.|  
|is_identity|**bit**|1 = La colonne a des valeurs d'identité.|  
|is_computed|**bit**|1 = La colonne est calculée.|  
|is_filestream|**bit**|1 = La colonne est déclarée utiliser un stockage de flux de fichier.|  
|is_replicated|**bit**|1 = La colonne est répliquée.|  
|is_non_sql_subscribed|**bit**|1 = La colonne possède un abonné non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|is_merge_published|**bit**|1 = La colonne est associée à une publication fusionnée.|  
|is_dts_replicated|**bit**|1 = La colonne est répliquée à l'aide de [!INCLUDE[ssIS](../../includes/ssis-md.md)].|  
|is_xml_document|**bit**|1 = Le contenu est un document XML complet.<br /><br /> 0 = Le contenu est un fragment de document ou les données de colonne ne sont pas de type XML.|  
|xml_collection_id|**int**|Valeur différente de zéro si le type de données de la colonne est **XML** et que le XML est typé. La valeur sera l'ID de la collection contenant l'espace de nom du schéma XML validant la colonne.<br /><br /> 0 = Aucune collection du schéma XML.|  
|default_object_id|**int**|ID de l’objet par défaut, qu’il s’agisse d’un [sys.sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)autonome ou d’une contrainte par défaut de niveau colonne dans la ligne. La colonne parent_object_id d'un objet inline par défaut de niveau colonne est une référence à la table elle-même.<br /><br /> 0 = Aucune valeur par défaut.|  
|rule_object_id|**int**|ID de la règle autonome liée à la colonne à l'aide de sys.sp_bindrule.<br /><br /> 0 = Aucune règle autonome.<br /><br /> Pour les contraintes de vérification au niveau des colonnes, consultez [sys.check_constraints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md).|  
|is_sparse|bit|1 = La colonne est éparse. Pour plus d’informations, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).|  
|is_column_set|bit|1 = La colonne est un jeu de colonnes. Pour plus d’informations, consultez [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).|  
|generated_always_type|**tinyint**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures.<br /><br /> Valeur numérique représentant le type de colonne :<br /><br /> 0 = NOT_APPLICABLE<br /><br /> 1 = AS_ROW_START<br /><br /> 2 = AS_ROW_END|  
|generated_always_type_desc|**nvarchar(60)**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures.<br /><br /> Description textuelle du type de colonne :<br /><br /> NOT_APPLICABLE<br /><br /> AS_ROW_START<br /><br /> AS_ROW_END|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.system_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)  
  
  
