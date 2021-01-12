---
description: sys.xml_schema_attributes (Transact-SQL)
title: sys.xml_schema_attributes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- xml_schema_attributes_TSQL
- xml_schema_attributes
- sys.xml_schema_attributes_TSQL
- sys.xml_schema_attributes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_attributes catalog view
ms.assetid: dd0c98aa-5e72-4df6-96d9-482786c8dbb1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cedfdf992f1dbec8eeff1ff7fa74a489b179d943
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095405"
---
# <a name="sysxml_schema_attributes-transact-sql"></a>sys.xml_schema_attributes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne par composant de schéma XML qui est un attribut, **symbol_space** d' **un**.  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|--|Hérite de [sys.xml_schema_components](../../relational-databases/system-catalog-views/sys-xml-schema-components-transact-sql.md).|  
|**is_default_fixed**|**bit**|1 = La valeur par défaut est une valeur fixe. Cette valeur ne peut pas être substituée dans une instance XML.<br /><br /> 0 = La valeur par défaut n'est pas une valeur fixe pour l'attribut. (par défaut)|  
|**must_be_qualified**|**bit**|1 = L'attribut doit être explicitement qualifié par l'espace de noms.<br /><br /> 0 = L'attribut peut être implicitement qualifié par l'espace de noms. (par défaut)|  
|**default_value**|**nvarchar**<br /><br /> **(4000)**|Valeur par défaut de l'attribut. NULL si une valeur par défaut n'est pas fournie.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Schémas XML &#40;les affichages catalogue du système de type XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
