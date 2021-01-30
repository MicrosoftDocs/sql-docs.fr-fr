---
description: sys.types (Transact-SQL)
title: sys. types (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- types
- types_TSQL
- sys.types_TSQL
- sys.types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.types catalog view
- table-valued parameters,sys.types
ms.assetid: a5dbc842-71a0-4f62-b5e0-f560a99b7f8c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0c12c42ca73f32936b33ffd1b66408698288c957
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205370"
---
# <a name="systypes-transact-sql"></a>sys.types (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne par type système et par type défini par l'utilisateur.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom du type. Est unique dans le schéma.|  
|**system_type_id**|**tinyint**|ID du type de système interne du type.|  
|**user_type_id**|**int**|ID du type. Unique dans la base de données. Pour les types de données système, **user_type_id**  =  **system_type_id**.|  
|**schema_id**|**int**|Identificateur du schéma auquel appartient le type.|  
|**principal_id**|**int**|ID du propriétaire spécifique s'il diffère du propriétaire du schéma. Par défaut, le propriétaire du schéma détient les objets contenus dans le schéma. Cependant, il est possible de spécifier un autre propriétaire à l'aide de l'instruction ALTER AUTHORIZATION qui permet de changer de propriétaire.<br /><br /> La valeur est NULL en l'absence de propriétaire de remplacement spécifique.|  
|**max_length**|**smallint**|Longueur maximale (en octets) du type.<br /><br /> -1 = le type de données de la colonne est **varchar (max)**, **nvarchar (max)**, **varbinary (max)** ou **XML**.<br /><br /> Pour les colonnes de **texte** , la valeur **max_length** sera 16.|  
|**precision**|**tinyint**|Précision maximale du type s'il est basé sur un nombre ; sinon, 0.|  
|**scale**|**tinyint**|Échelle maximale du type s'il est de type numérique ; sinon, 0.|  
|**collation_name**|**sysname**|Nom du classement du type s’il est basé sur des caractères ; autre Wise, NULL.|  
|**is_nullable**|**bit**|Le type accepte les valeurs NULL.|  
|**is_user_defined**|**bit**|1 = type défini par l'utilisateur.<br /><br /> 0 = type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**is_assembly_type**|**bit**|1 = l'implémentation du type est définie dans un assembly CLR.<br /><br /> 0 = le type est basé sur un type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**default_object_id**|**int**|ID de la valeur par défaut autonome qui est liée au type à l’aide de [sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md).<br /><br /> 0 = aucune valeur par défaut n'existe.|  
|**rule_object_id**|**int**|ID de la règle autonome liée au type à l’aide de [sp_bindrule](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md).<br /><br /> 0 = aucune règle n'existe.|  
|**is_table_type**|**bit**|Indique que le type est une table.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue types scalaires &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
