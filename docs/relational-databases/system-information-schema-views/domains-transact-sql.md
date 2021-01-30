---
description: DOMAINS (Transact-SQL)
title: DOMAINES (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DOMAINS_TSQL
- DOMAINS
dev_langs:
- TSQL
helpviewer_keywords:
- DOMAINS view
- INFORMATION_SCHEMA.DOMAINS view
ms.assetid: f0b734d5-816f-4b10-a60c-615931b515c2
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 19c906eb2c32884b02e9c7835c88186823516aa2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190095"
---
# <a name="domains-transact-sql"></a>DOMAINS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie une ligne pour chaque type de données alias accessible par l'utilisateur actuel dans la base de données active.  
  
 Pour récupérer des informations de ces vues, spécifiez le nom complet de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|Base de données dans laquelle réside le type de données alias.|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient le type de données de l’alias.<br /><br /> **&#42;&#42;  &#42;&#42;importante** N’utilisez pas de vues de INFORMATION_SCHEMA pour déterminer le schéma d’un type de données. La seule méthode fiable pour rechercher le schéma d'un type est d'utiliser la fonction TYPEPROPERTY.|  
|**DOMAIN_NAME**|**sysname**|Type de données alias.|  
|**DATA_TYPE**|**sysname**|Type de données fourni par le système.|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|Longueur maximale (en caractères) des données de type binaire, caractère, texte et image.<br /><br /> -1 pour les données de type **XML** et de valeur élevée. Renvoie NULL dans les autres cas. Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
|**CHARACTER_OCTET_LENGTH**|**int**|Longueur maximale, en octets, des données de type binaire, caractère, texte et image.<br /><br /> -1 pour les données de type **XML** et de valeur élevée. Renvoie NULL dans les autres cas.|  
|**COLLATION_CATALOG**|**varchar (** 6 **)**|Retourne toujours la valeur Null.|  
|**COLLATION_SCHEMA**|**varchar (** 3 **)**|Retourne toujours la valeur Null.|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|Retourne le nom unique de l’ordre de tri si la colonne est de type de données caractère ou **texte** . Renvoie NULL dans les autres cas.|  
|**CHARACTER_SET_CATALOG**|**varchar (** 6 **)**|Retourne **Master**. Il s’agit de la base de données dans laquelle se trouve le jeu de caractères, si la colonne est de type de données caractère ou **texte** . Renvoie NULL dans les autres cas.|  
|**CHARACTER_SET_SCHEMA**|**varchar (** 3 **)**|Retourne toujours la valeur Null.|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|Retourne le nom unique du jeu de caractères si cette colonne est une donnée de type caractère ou **texte** . Renvoie NULL dans les autres cas.|  
|**NUMERIC_PRECISION**|**tinyint**|Précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|Base de précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|**NUMERIC_SCALE**|**tinyint**|Échelle des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|  
|**DATETIME_PRECISION**|**smallint**|Code de sous-type pour les types de données **DateTime** et ISO **Interval** . Pour les autres types de données, cette colonne retourne une valeur NULL.|  
|**DOMAIN_DEFAULT**|**nvarchar (** 4000 **)**|Texte réel de l’instruction de définition [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [ Jeux desys.sys&#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)  
  
