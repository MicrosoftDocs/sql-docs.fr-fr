---
description: sys.all_parameters (Transact-SQL)
title: sys.all_parameters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- all_parameters_TSQL
- sys.all_parameters
- all_parameters
- sys.all_parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.all_parameters catalog view
ms.assetid: eecbb68e-9b4c-4243-94e2-8096a9cc7892
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 17c1c62c582a4e8937e82b7ba911955a6805ab26
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205543"
---
# <a name="sysall_parameters-transact-sql"></a>sys.all_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Affiche l'union de tous les paramètres qui appartiennent aux objets système ou définis par l'utilisateur.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de l’objet auquel appartient ce paramètre.|  
|**name**|**sysname**|Nom du paramètre. Unique dans l'objet. Si l'objet est une fonction scalaire, le nom du paramètre est une chaîne de caractères vide dans la ligne qui représente la valeur renvoyée.|  
|**parameter_id**|**int**|Identificateur du paramètre. Unique dans l'objet. Si l’objet est une fonction scalaire, **parameter_id** = 0 représente la valeur de retour.|  
|**system_type_id**|**tinyint**|Identificateur du type système du paramètre.|  
|**user_type_id**|**int**|Identificateur du type du paramètre tel qu'il est défini par l'utilisateur.<br /><br /> Pour retourner le nom du type, Joignez-vous à l’affichage catalogue [sys. types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) sur cette colonne.|  
|**max_length**|**smallint**|Longueur maximale du paramètre en octets.<br /><br /> -1 = le type de données de la colonne est **varchar (max)**, **nvarchar (max)**, **varbinary (max)** ou **XML**.|  
|**precision**|**tinyint**|Précision du paramètre s'il est numérique ; sinon, 0.|  
|**scale**|**tinyint**|Échelle du paramètre s'il est numérique ; sinon, 0.|  
|**is_output**|**bit**|1 = le paramètre est renvoyé ; sinon, 0.|  
|**is_cursor_ref**|**bit**|1 = le paramètre est un paramètre de référence de curseur.|  
|**has_default_value**|**bit**|1 = le paramètre a une valeur par défaut.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve seulement les valeurs par défaut des objets CLR dans cet affichage catalogue ; par conséquent, cette colonne a toujours une valeur nulle (0) pour les objets [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour afficher la valeur par défaut d’un paramètre dans un [!INCLUDE[tsql](../../includes/tsql-md.md)] objet, interrogez la colonne de **définition** de l’affichage catalogue [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) ou utilisez la fonction système [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) .|  
|**is_xml_document**|**bit**|1 = Le contenu est un document XML complet.<br /><br /> 0 = le contenu est un fragment de document ou le type de données de la colonne n’est pas **XML**.|  
|**default_value**|**sql_variant**|Si **has_default_value** a la valeur 1, la valeur de cette colonne est la valeur par défaut du paramètre ; Sinon, NULL.|  
|**xml_collection_id**|**int**|Identificateur de la collection du schéma XML utilisé pour valider le paramètre.<br /><br /> Différent de zéro si le type de données du paramètre est **XML** et que le XML est typé.<br /><br /> 0 = il n'existe pas de collection de schéma XML ou le paramètre n'est pas de type XML.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys. Parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)   
 [sys.system_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-parameters-transact-sql.md)  
  
  
