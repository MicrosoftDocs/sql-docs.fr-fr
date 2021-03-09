---
description: sys.columns (Transact-SQL)
title: sys. Columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.columns_TSQL
- sys.columns
- columns_TSQL
- columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.columns catalog view
ms.assetid: 323ac9ea-fc52-4b8c-8a7e-e0e44f8ed86c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: babedc8a8f4711776e3bc014ee83b22130bc6554
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464747"
---
# <a name="syscolumns-transact-sql"></a>sys.columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une ligne pour chaque colonne d'un objet comportant des colonnes, telles que des vues ou des tables. Voici une liste de types d'objet comportant des colonnes :  
  
-   Fonctions table d'assembly (FT)  
  
-   Fonctions table inline SQL (IF)  
  
-   Tables internes (IT)  
  
-   Tables système (S)  
  
-   Fonctions table SQL (TF)  
  
-   Tables utilisateur  
  
-   Vues (V)  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Identificateur de l'objet auquel appartient cette colonne.|  
|name|**sysname**|Nom de la colonne. Unique dans l'objet.|  
|column_id|**int**|Identificateur de la colonne. Unique dans l'objet.<br /><br /> Les ID de colonnes peuvent ne pas être séquentiels.|  
|system_type_id|**tinyint**|ID du type de système de la colonne.|  
|user_type_id|**int**|ID du type de colonne tel que défini par l'utilisateur.<br /><br /> Pour retourner le nom du type, Joignez-vous à l’affichage catalogue [sys. types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) sur cette colonne.|  
|max_length|**smallint**|Longueur maximale (en octets) de la colonne.<br /><br /> -1 = le type de données de la colonne est **varchar (max)**, **nvarchar (max)**, **varbinary (max)** ou **XML**.<br /><br /> Pour les colonnes de **texte** , la valeur max_length sera 16 ou la valeur définie par sp_tableoption’texte dans la ligne'.|  
|précision|**tinyint**|Précision de la colonne si elle est numérique ; sinon, 0.|  
|scale|**tinyint**|Échelle de la colonne si elle est numérique ; sinon, la valeur est 0.|  
|collation_name|**sysname**|Nom du classement de la colonne si elle est basée sur des caractères ; sinon, NULL.|  
|is_nullable|**bit**|1 = La colonne accepte les valeurs NULL.|  
|is_ansi_padded|**bit**|1 = La colonne utilise le comportement ANSI_PADDING ON si elle est de type caractère, binaire ou variant.<br /><br /> 0 = La colonne n'est pas de type caractère, binaire ou variant.|  
|is_rowguidcol|**bit**|1 = La colonne est un ROWGUIDCOL déclaré.|  
|is_identity|**bit**|1 = La colonne a des valeurs d'identité.|  
|is_computed|**bit**|1 = La colonne est calculée.|  
|is_filestream|**bit**|1 = La colonne est une colonne FILESTREAM.|  
|is_replicated|**bit**|1 = La colonne est répliquée.|  
|is_non_sql_subscribed|**bit**|1 = La colonne a un abonné autre que SQL Server.|  
|is_merge_published|**bit**|1 = La colonne est associée à une publication fusionnée.|  
|is_dts_replicated|**bit**|1 = La colonne est répliquée à l'aide de [!INCLUDE[ssIS](../../includes/ssis-md.md)].|  
|is_xml_document|**bit**|1 = Le contenu est un document XML complet.<br /><br /> 0 = le contenu est un fragment de document ou le type de données de la colonne n’est pas **XML**.|  
|xml_collection_id|**int**|Différent de zéro si le type de données de la colonne est **XML** et que le XML est typé. La valeur correspondra à l'ID de la collection qui contient l'espace de noms de schéma XML de validation de la colonne.<br /><br /> 0 = Aucune collection de schéma XML.|  
|default_object_id|**int**|ID de l’objet par défaut, qu’il s’agisse d’un objet autonome [sys.sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md), ou d’une contrainte par défaut en ligne de niveau colonne. La colonne parent_object_id d'un objet inline par défaut de niveau colonne est une référence à la table elle-même.<br /><br /> 0 = Aucune valeur par défaut.|  
|rule_object_id|**int**|ID de la règle autonome liée à la colonne à l'aide de sys.sp_bindrule.<br /><br /> 0 = Aucune règle autonome. Pour les contraintes de vérification au niveau des colonnes, consultez [sys.check_constraints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md).|  
|is_sparse|**bit**|1 = La colonne est éparse. Pour plus d’informations, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).|  
|is_column_set|**bit**|1 = La colonne est un jeu de colonnes. Pour plus d’informations, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).|  
|generated_always_type|**tinyint**|**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Identifie le moment où la valeur de colonne est générée (est toujours 0 pour les colonnes dans les tables système) :<br /><br /> 0 = NOT_APPLICABLE<br /><br /> 1 = AS_ROW_START<br /><br /> 2 = AS_ROW_END<br /><br /> Pour plus d’informations, consultez [tables temporelles &#40;bases de données relationnelles&#41;](../../relational-databases/tables/temporal-tables.md).|  
|generated_always_type_desc|**nvarchar(60)**|**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Description textuelle de `generated_always_type` la valeur de (toujours NOT_APPLICABLE pour les colonnes des tables système) <br /><br /> NOT_APPLICABLE<br /><br /> AS_ROW_START<br /><br /> AS_ROW_END|  
|encryption_type|**int**|**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Type de chiffrement :<br /><br /> 1 = chiffrement déterministe<br /><br /> 2 = chiffrement aléatoire|  
|encryption_type_desc|**nvarchar (64)**|**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Description du type de chiffrement :<br /><br /> ALÉATOIRE<br /><br /> DETERMINISTIC|  
|encryption_algorithm_name|**sysname**|**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Nom de l’algorithme de chiffrement.<br /><br /> Seul AEAD_AES_256_CBC_HMAC_SHA_512 est pris en charge.|  
|column_encryption_key_id|**int**|**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> ID du clé CEK.|  
|column_encryption_key_database_name|**sysname**|**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[ssSDW_md](../../includes/sssds-md.md)].<br /><br /> Nom de la base de données dans laquelle la clé de chiffrement de colonne existe si elle est différente de la base de données de la colonne. NULL si la clé existe dans la même base de données que la colonne.|  
|is_hidden|**bit**|**S’applique à** : [!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] et versions ultérieures, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Indique si la colonne est masquée :<br /><br /> 0 = colonne normale, non masquée, visible<br /><br /> 1 = colonne masquée|  
|is_masked|**bit**|**S’applique à** : [!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] et versions ultérieures, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Indique si la colonne est masquée par un masquage dynamique des données :<br /><br /> 0 = colonne normale, non masquée<br /><br /> 1 = la colonne est masquée|  


 
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.system_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-columns-transact-sql.md)  
  
