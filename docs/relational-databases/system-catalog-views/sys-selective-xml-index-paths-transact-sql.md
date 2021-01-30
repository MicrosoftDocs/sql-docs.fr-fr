---
description: sys.selective_xml_index_paths (Transact-SQL)
title: sys.selective_xml_index_paths (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xml_schema_attributes_TSQL
- xml_schema_attributes
- sys.xml_schema_attributes_TSQL
- sys.xml_schema_attributes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_attributes catalog view
ms.assetid: 07a73d71-ec3e-4894-947a-5859ca62c606
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 4a768ded6ef89a54f35e7ce30a64bc3dd6e277ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182474"
---
# <a name="sysselective_xml_index_paths-transact-sql"></a>sys.selective_xml_index_paths (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Disponible à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Service Pack 1, chaque ligne dans sys.selective_xml_index_paths représente un chemin d'accès promu pour un index XML sélectif spécifique.  
  
 Si vous créez un index XML sélectif sur xmlcol de la table T à l'aide de l'instruction suivante,  
  
```  
CREATE SELECTIVE XML INDEX sxi1 ON T(xmlcol)   
FOR ( path1 = '/a/b/c' AS XQUERY 'xs:string',  
      path2 = '/a/b/d' AS XQUERY 'xs:double'  
    )  
```  
  
 Il y aura deux nouvelles lignes dans sys.selective_xml_index_paths correspondant à l'index sxi1.  

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de table avec une colonne XML.|  
|**index_id**|**int**|ID unique de l'index XML sélectif.|  
|**path_id**|**int**|ID du chemin d'accès XML promu.|  
|**path**|**nvarchar(4000)**|Chemin d'accès promu. Par exemple, '/a/b/c/d/e'.|  
|**name**|**sysname**|Chemin d'accès.|  
|**path_type**|**tinyint**|0 = XQUERY<br /><br /> 1 = SQL|  
|**path_type_desc**|**sysname**|Basé sur **path_type** valeur’XQuery’ou’SQL'.|  
|**xml_component_id**|**int**|ID unique du composant de schéma XML dans la base de données.|  
|**xquery_type_description**|**nvarchar(4000)**|Nom du type xsd spécifié.|  
|**is_xquery_type_inferred**|**bit**|1 = le type est déduit.|  
|**xquery_max_length**|**smallint**|Longueur maximale (en caractères du type xsd).|  
|**is_xquery_max_length_inferred**|**bit**|1 = la longueur maximale est déduite.|  
|**is_node**|**bit**|0 = l'indicateur node() n'est pas présent.<br /><br /> 1 = l'indicateur d'optimisation node() est appliqué.|  
|**system_type_id**|**tinyint**|ID du type de système de la colonne.|  
|**user_type_id**|**tinyint**|ID du type d'utilisateur de la colonne.|  
|**max_length**|**smallint**|Longueur maximale (en octets) du type.<br /><br /> -1 = le type de données de colonne est varchar(max), nvarchar(max), varbinary(max) ou xml.|  
|**precision**|**tinyint**|Précision maximale du type s'il est basé sur un nombre. Sinon, 0.|  
|**scale**|**tinyint**|Échelle maximale du type s'il est basé sur un nombre. Sinon, il prend la valeur 0.|  
|**collation_name**|**sysname**|Nom du classement du type s'il est de type caractère. Sinon, NULL.|  
|**is_singleton**|**bit**|0 = l'indicateur SINGLETON n'est pas présent.<br /><br /> 1 = l'indicateur d'optimisation SINGLETON est appliqué.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Schémas XML &#40;les affichages catalogue du système de type XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
