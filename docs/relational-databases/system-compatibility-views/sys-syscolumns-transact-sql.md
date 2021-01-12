---
description: sys.syscolumns (Transact-SQL)
title: Colonnes sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-enginel, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.syscolumns
- sys.syscolumns_TSQL
- syscolumns_TSQL
- syscolumns
dev_langs:
- TSQL
helpviewer_keywords:
- syscolumns system table
- sys.syscolumns compatibility view
ms.assetid: 863fd87b-ff33-4ac5-9aa9-df21140681da
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9ecd4982135135fbf23c700a30318d86026d266
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099194"
---
# <a name="syssyscolumns-transact-sql"></a>sys.syscolumns (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Retourne une ligne pour chaque colonne des tables et des vues, et une ligne pour chaque paramètre des procédures stockées de la base de données.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de la colonne ou du paramètre de la procédure|  
|**id**|**int**|Identificateur d'objet de la table à laquelle cette colonne appartient, ou ID de la procédure stockée à laquelle ce paramètre est associé|  
|**xtype**|**tinyint**|Type de stockage physique de **sys. types**.|  
|**typestat**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**smallint**|Identificateur de type de données étendu défini par l'utilisateur Déborde ou retourne la valeur NULL si le nombre de types de données dépasse 32 767.|  
|**length**|**smallint**|Longueur maximale de stockage physique de **sys**. **types**.|  
|**xprec**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xscale**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colid**|**smallint**|Identificateur de colonne ou de paramètre|  
|**xoffset**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**bitpos**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**réservé**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colstat**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**cdefault**|**int**|Identificateur de la valeur par défaut pour cette colonne|  
|**Domain**|**int**|Identificateur de la règle ou de la contrainte CHECK pour cette colonne|  
|**number**|**smallint**|Numéro de sous-procédure pour les procédures groupées.<br /><br /> 0 = entrées qui ne décrivent pas une procédure|  
|**colorder**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**autoval**|**varbinary(8000)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offset**|**smallint**|Décalage dans la ligne où apparaît cette colonne.|  
|**CollationID**|**int**|ID du classement de la colonne. NULL pour les colonnes de type non caractère.|  
|**statut**|**tinyint**|Bitmap servant à décrire une propriété de la colonne ou du paramètre :<br /><br /> 0x08 = La colonne autorise les valeurs NULL.<br /><br /> 0x10 = le remplissage ANSI était en vigueur lorsque des colonnes **varchar** ou **varbinary** étaient ajoutées. Les espaces à droite sont conservés pour les colonnes **varchar** et les zéros à droite sont conservés pour les colonnes **varbinary** .<br /><br /> 0x40 = Le paramètre est un paramètre de sortie (OUTPUT).<br /><br /> 0x80 = La colonne est une colonne d'identité.|  
|**type**|**tinyint**|Type de stockage physique de **sys**. **types**.|  
|**UserType**|**smallint**|ID du type de données défini par l’utilisateur à partir de **sys. types**. Déborde ou retourne la valeur NULL si le nombre de types de données dépasse 32 767.|  
|**printfmt**|**varchar(255)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**smallint**|Niveau de précision de cette colonne<br /><br /> -1 = **XML** ou type de valeur élevée.|  
|**scale**|**int**|Échelle de cette colonne<br /><br /> NULL = le type de données est non numérique.|  
|**IsComputed**|**int**|Indicateur signalant si la colonne est calculée :<br /><br /> 0 = Non calculée<br /><br /> 1 = Calculée|  
|**isoutparam**|**int**|Indique si le paramètre de la procédure est un paramètre de sortie ou non :<br /><br /> 1 = Vrai<br /><br /> 0 = Faux|  
|**IsNullable**|**int**|Indique si les colonnes autorisent les valeurs NULL :<br /><br /> 1 = Vrai<br /><br /> 0 = Faux|  
|**classement**|**sysname**|Nom du classement de la colonne. NULL s'il ne s'agit pas d'une colonne de type caractère.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
