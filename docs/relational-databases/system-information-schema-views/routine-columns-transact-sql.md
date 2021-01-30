---
description: ROUTINE_COLUMNS (Transact-SQL)
title: ROUTINE_COLUMNS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- ROUTINE_COLUMNS_TSQL
- ROUTINE_COLUMNS
dev_langs:
- TSQL
helpviewer_keywords:
- ROUTINE_COLUMNS view
- INFORMATION_SCHEMA.ROUTINE_COLUMNS view
ms.assetid: 91dbc61b-e4c0-4826-976c-b2fce88b7793
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f2a60f579dd5769d4c555e79de1b67cfd75660ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189599"
---
# <a name="routine_columns-transact-sql"></a>ROUTINE_COLUMNS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une ligne pour chaque colonne retournée par les fonctions table, accessibles à l'utilisateur actuel de la base de données active.  
  
 Pour récupérer des informations à partir de cette vue, spécifiez le nom complet de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Nom du catalogue ou de la base de données de la fonction table|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient la fonction table.<br /><br /> Important n’utilisez pas les vues de INFORMATION_SCHEMA pour déterminer le schéma d’un objet. <strong> \* \* \* \* </strong> Les vues de INFORMATION_SCHEMA représentent seulement un sous-ensemble des métadonnées d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet consiste à interroger l’affichage catalogue sys. Objects.|  
|**TABLE_NAME**|**nvarchar (** 128 **)**|Nom de la fonction table|  
|**COLUMN_NAME**|**nvarchar (** 128 **)**|Nom de la colonne.|  
|**ORDINAL_POSITION**|**int**|Numéro d'identification de colonne.|  
|**COLUMN_DEFAULT**|**nvarchar (** 4000 **)**|Valeur par défaut de la colonne.|  
|**IS_NULLABLE**|**varchar (** 3 **)**|Si cette colonne accepte la valeur NULL, elle retourne la valeur YES. Dans le cas contraire, la valeur retournée est NO.|  
|**DATA_TYPE**|**nvarchar (** 128 **)**|Type de données fourni par le système.|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|Longueur maximale (en caractères) des données de type binaire, caractère, texte et image.<br /><br /> -1 pour les données de type **XML** et de valeur élevée. Dans le cas contraire, la valeur NULL est retournée. Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
|**CHARACTER_OCTET_LENGTH**|**int**|Longueur maximale, en octets, des données de type binaire, caractère, texte et image.<br /><br /> -1 pour les données de type **XML** et de valeur élevée. Dans le cas contraire, la valeur NULL est retournée.|  
|**NUMERIC_PRECISION**|**tinyint**|Précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Dans le cas contraire, la valeur NULL est retournée.|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|Base de précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Dans le cas contraire, la valeur NULL est retournée.|  
|**NUMERIC_SCALE**|**tinyint**|Échelle des données numériques approchées ou exactes, des données de type entier ou monétaire. Dans le cas contraire, la valeur NULL est retournée.|  
|**DATETIME_PRECISION**|**smallint**|Code de sous-type pour les types de données **Integer** **DateTime** et ISO. Retourne la valeur NULL pour les autres types de données.|  
|**CHARACTER_SET_CATALOG**|**varchar (** 6 **)**|Retourne **Master**. Il s’agit de la base de données dans laquelle se trouve le jeu de caractères si la colonne est de type de données caractère ou **texte** . Dans le cas contraire, la valeur NULL est retournée.|  
|**CHARACTER_SET_SCHEMA**|**varchar (** 3 **)**|Retourne toujours la valeur Null.|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|Retourne le nom unique du jeu de caractères si cette colonne est une donnée de type caractère ou **texte** . Dans le cas contraire, la valeur NULL est retournée.|  
|**COLLATION_CATALOG**|**varchar (** 6 **)**|Retourne toujours la valeur Null.|  
|**COLLATION_SCHEMA**|**varchar (** 3 **)**|Retourne toujours la valeur Null.|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|Retourne le nom unique de l’ordre de tri si la colonne est de type de données caractère ou **texte** . Dans le cas contraire, la valeur NULL est retournée.|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|Si la colonne est un type de données alias, elle correspond au nom de la base de données dans laquelle le type de données défini par l'utilisateur a été créé. Dans le cas contraire, la valeur NULL est retournée.|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|Si la colonne est un type de données défini par l'utilisateur, elle représente le nom du schéma qui contient le type de données défini par l'utilisateur. Dans le cas contraire, la valeur NULL est retournée.<br /><br /> Important n’utilisez pas les vues de INFORMATION_SCHEMA pour déterminer le schéma d’un objet. <strong> \* \* \* \* </strong> Les vues de INFORMATION_SCHEMA représentent seulement un sous-ensemble des métadonnées d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet consiste à interroger l’affichage catalogue sys. Objects.|  
|**DOMAIN_NAME**|**nvarchar (** 128 **)**|Si la colonne est un type de données défini par l'utilisateur, elle représente le nom du type de données défini par l'utilisateur. Dans le cas contraire, la valeur NULL est retournée.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
