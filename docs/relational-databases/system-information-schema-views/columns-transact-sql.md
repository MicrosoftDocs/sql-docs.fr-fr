---
description: COLUMNS (Transact-SQL)
title: COLONNES (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- COLUMNS
- COLUMNS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- COLUMNS view
- INFORMATION_SCHEMA.COLUMNS view
ms.assetid: bbf7ac4a-7444-4351-a590-a9f71e0bc495
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 510409a417ff35edb932b0689f6cfb17967ce171
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210264"
---
# <a name="columns-transact-sql"></a>COLUMNS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie une ligne pour chaque colonne accessible à l'utilisateur actuel dans la base de données actuelle.  
  
 Pour récupérer des informations de ces vues, spécifiez le nom complet de **INFORMATION_SCHEMA**_.view_name_.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Qualificateur de la table.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient la table.<br /><br /> **&#42;&#42;  &#42;&#42;importante** N’utilisez pas de vues de INFORMATION_SCHEMA pour déterminer le schéma d’un objet. Les vues de INFORMATION_SCHEMA représentent seulement un sous-ensemble des métadonnées d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet consiste à interroger l’affichage catalogue sys. Objects.|  
|**TABLE_NAME**|**nvarchar (** 128 **)**|Nom de la table.|  
|**COLUMN_NAME**|**nvarchar (** 128 **)**|Nom de la colonne.|  
|**ORDINAL_POSITION**|**int**|Numéro d'identification de colonne.|  
|**COLUMN_DEFAULT**|**nvarchar (** 4000 **)**|Valeur par défaut de la colonne.|  
|**IS_NULLABLE**|**varchar (** 3 **)**|Valeur NULL possible dans la colonne. Si cette colonne accepte des valeurs NULL, elle renvoie YES. Dans le cas contraire, elle renvoie NO.|  
|**DATA_TYPE**|**nvarchar (** 128 **)**|Type de données fourni par le système.|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|Longueur maximale (en caractères) des données de type binaire, caractère, texte et image.<br /><br /> -1 pour les données de type **XML** et de valeur élevée. Renvoie NULL dans les autres cas. Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
|**CHARACTER_OCTET_LENGTH**|**int**|Longueur maximale, en octets, des données de type binaire, caractère, texte et image.<br /><br /> -1 pour les données de type **XML** et de valeur élevée. Renvoie NULL dans les autres cas.|  
|**NUMERIC_PRECISION**|**tinyint**|Précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|Base de précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|**NUMERIC_SCALE**|**int**|Échelle des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|**DATETIME_PRECISION**|**smallint**|Code de sous-type pour les types de données **DateTime** et **Interval** ISO. Renvoie NULL pour les autres types de données.|  
|**CHARACTER_SET_CATALOG**|**nvarchar (** 128 **)**|Retourne **Master**. Il s’agit de la base de données dans laquelle se trouve le jeu de caractères, si la colonne est de type de données caractère ou **texte** . Renvoie NULL dans les autres cas.|  
|**CHARACTER_SET_SCHEMA**|**nvarchar (** 128 **)**|Retourne toujours la valeur Null.|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|Retourne le nom unique du jeu de caractères si cette colonne est une donnée de type caractère ou **texte** . Renvoie NULL dans les autres cas.|  
|**COLLATION_CATALOG**|**nvarchar (** 128 **)**|Retourne toujours la valeur Null.|  
|**COLLATION_SCHEMA**|**nvarchar (** 128 **)**|Retourne toujours la valeur Null.|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|Retourne le nom unique du classement si la colonne est de type de données caractère ou **texte** . Renvoie NULL dans les autres cas.|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|Si la colonne est un type de données alias, elle correspond au nom de la base de données dans laquelle le type de données défini par l'utilisateur a été créé. Renvoie NULL dans les autres cas.|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|Si la colonne est un type de données défini par l'utilisateur, elle renvoie le nom du schéma du type de données défini par l'utilisateur. Renvoie NULL dans les autres cas.<br /><br /> **&#42;&#42;  &#42;&#42;importante** N’utilisez pas de vues de INFORMATION_SCHEMA pour déterminer le schéma d’un type de données. La seule méthode fiable pour rechercher le schéma d'un type est d'utiliser la fonction TYPEPROPERTY.|  
|**DOMAIN_NAME**|**nvarchar (** 128 **)**|Si la colonne est un type de données défini par l'utilisateur, elle représente le nom du type de données défini par l'utilisateur. Renvoie NULL dans les autres cas.|  
  
## <a name="remarks"></a>Notes  
 **ORDINAL_POSITION** colonne du **INFORMATION_SCHEMA.** La vue colonnes n’est pas compatible avec le modèle binaire des colonnes retournées par la fonction COLUMNS_UPDATED. Pour obtenir un modèle binaire qui est compatible avec COLUMNS_UPDATED, vous devez référencer la propriété **ColumnID** de la fonction système COLUMNPROPERTY lorsque vous interrogez le **INFORMATION_SCHEMA. Vue colonnes** . Par exemple :  
  
```  
USE AdventureWorks2012;  
GO  
SELECT TABLE_NAME, COLUMN_NAME, COLUMNPROPERTY(OBJECT_ID(TABLE_SCHEMA + '.' + TABLE_NAME), COLUMN_NAME, 'ColumnID') AS COLUMN_ID  
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS  
WHERE TABLE_NAME = 'Person';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [ Jeux desys.sys&#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)   
 [COLUMNS_UPDATED &#40;Transact-SQL&#41;](../../t-sql/functions/columns-updated-transact-sql.md)  
  
