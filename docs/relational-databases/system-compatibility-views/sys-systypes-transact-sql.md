---
description: sys.systypes (Transact-SQL)
title: Types de sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.systypes_TSQL
- systypes_TSQL
- sys.systypes
- systypes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.systypes compatibility view
- systypes system table
ms.assetid: 1b0b1d0c-5f7b-470b-bd52-8bfa922d7889
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7f00dab371048ed8a1b39cb702897ce5f481c377
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160647"
---
# <a name="syssystypes-transact-sql"></a>sys.systypes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une ligne pour chaque type de données système et chaque type de données défini par l'utilisateur définis dans la base de données.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom du type de données.|  
|**xtype**|**tinyint**|Type de stockage physique|  
|**statut**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**smallint**|Type d'utilisateur étendu Déborde ou retourne la valeur NULL si le nombre de types de données dépasse 32 767.|  
|**length**|**smallint**|Longueur physique du type de données.|  
|**xprec**|**tinyint**|Précision interne, telle qu'utilisée par le serveur. À ne pas utiliser dans les requêtes.|  
|**xscale**|**tinyint**|Échelle interne, telle qu'utilisée par le serveur. À ne pas utiliser dans les requêtes.|  
|**tdefault**|**int**|ID de la procédure stockée qui contient les contrôles d'intégrité de ce type de données.|  
|**Domain**|**int**|ID de la procédure stockée qui contient les contrôles d'intégrité de ce type de données.|  
|**codé**|**smallint**|ID de schéma du propriétaire du type.<br /><br /> Pour les bases de données mises à niveau à partir d'une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'ID de schéma correspond à l'ID d'utilisateur du propriétaire.<br /><br /> Important si vous utilisez l’une des instructions DDL suivantes, vous devez utiliser l’affichage catalogue **\* sys. types au lieu des types desys.sys. \* \* \*** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) <br /><br /> ALTER AUTHORIZATION ON TYPE<br /><br /> CREATE TYPE<br /><br /> Déborde ou retourne la valeur NULL si le nombre d'utilisateurs et de rôles dépasse 32 767.|  
|**réservé**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**CollationID**|**int**|S’il est basé sur un caractère, **CollationID** est l’ID du classement de la base de données active ; dans le cas contraire, la valeur est NULL.|  
|**UserType**|**smallint**|ID de type d'utilisateur. Déborde ou retourne la valeur NULL si le nombre de types de données dépasse 32 767.|  
|**variable**|**bit**|Type de données de longueur variable.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux|  
|**allownulls**|**bit**|Détermine la possibilité de valeur NULL par défaut pour ce type de données. Cette valeur par défaut est remplacée par si la possibilité de valeur null est spécifiée à l’aide de [Create table](../../t-sql/statements/create-table-transact-sql.md) ou [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).|  
|**type**|**tinyint**|Type de données de stockage physique.|  
|**printfmt**|**varchar(255)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**smallint**|Niveau de précision de ce type de données<br /><br /> -1 = **XML** ou types de valeur élevée.|  
|**scale**|**tinyint**|Échelle de ce type de données, basée sur la précision.<br /><br /> NULL = le type de données est non numérique.|  
|**classement**|**sysname**|S’il est basé sur un caractère, **le classement est** le classement de la base de données active ; dans le cas contraire, la valeur est NULL.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de compatibilité &#40;&#41;Transact-SQL ](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
