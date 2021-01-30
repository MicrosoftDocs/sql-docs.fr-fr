---
description: sys.xml_schema_wildcards (Transact-SQL)
title: sys.xml_schema_wildcards (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.xml_schema_wildcards
- sys.xml_schema_wildcards_TSQL
- xml_schema_wildcards
- xml_schema_wildcards_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_wildcards catalog view
ms.assetid: 7cedfe9a-e99e-4777-8a28-98674b6e5cff
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1648798a92b545ffcb5500fe7cc90f7d28e76987
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190824"
---
# <a name="sysxml_schema_wildcards-transact-sql"></a>sys.xml_schema_wildcards (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne par composant de schéma XML qui est un Attribute-Wildcard (**genre** de **V**) ou Element-Wildcard **(genre** de **W**), avec **symbol_space** de **N**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Hérite des colonnes de [sys.xml_schema_components](../../relational-databases/system-catalog-views/sys-xml-schema-components-transact-sql.md).|  
|**process_content**|**char(1)**|Indique comment les contenus sont traités.<br /><br /> S = validation stricte (validation obligatoire)<br /><br /> L = validation libre (validation si possible)<br /><br /> P = validation ignorée|  
|**process_content_desc**|**nvarchar(60)**|Description de la manière dont les contenus sont traités :<br /><br /> **STRICT_VALIDATION**<br /><br /> **LAX_VALIDATION**<br /><br /> **SKIP_VALIDATION**|  
|**disallow_namespaces**|**bit**|0 = les espaces de noms énumérés dans [sys.xml_schema_wildcard_namespaces](../../relational-databases/system-catalog-views/sys-xml-schema-wildcard-namespaces-transact-sql.md) sont les seuls autorisés.<br /><br /> 1 = les espaces de noms sont les seuls non autorisés.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Schémas XML &#40;les affichages catalogue du système de type XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
