---
description: sys.parameters (Transact-SQL)
title: sys. Parameters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.parameters_TSQL
- sys.parameters
- parameters
- parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.parameters catalog view
- table-valued parameters,sys.parameters
ms.assetid: 24e2764b-c8e5-4322-97a4-7407d8b8a92b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba1df113674be0d20570e9cab7781f28dc2d5de2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191258"
---
# <a name="sysparameters-transact-sql"></a>sys.parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne pour chaque paramètre d'un objet acceptant les paramètres. Si l'objet est une fonction scalaire, il existe également une ligne unique décrivant la valeur renvoyée. Cette ligne aura une valeur **parameter_id** de 0.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de l’objet auquel appartient ce paramètre.|  
|**name**|**sysname**|Nom du paramètre. Unique dans l'objet.<br /><br /> Si l'objet est une fonction scalaire, le nom du paramètre est une chaîne de caractères vide dans la ligne qui représente la valeur renvoyée.|  
|**parameter_id**|**int**|ID du paramètre. Unique dans l'objet.<br /><br /> Si l’objet est une fonction scalaire, **parameter_id** = 0 représente la valeur de retour.|  
|**system_type_id**|**tinyint**|Identificateur du type système du paramètre.|  
|**user_type_id**|**int**|Identificateur du type du paramètre tel qu'il est défini par l'utilisateur.<br /><br /> Pour retourner le nom du type, Joignez-vous à l’affichage catalogue [sys. types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) sur cette colonne.|  
|**max_length**|**smallint**|Longueur maximale du paramètre en octets.<br /><br /> Valeur =-1 lorsque le type de données de la colonne est **varchar (max)**, **nvarchar (max)**, **varbinary (max)** ou **XML**.|  
|**precision**|**tinyint**|Précision du paramètre s'il est de type numérique ; sinon, 0.|  
|**scale**|**tinyint**|Échelle du paramètre s'il est de type numérique ; sinon, 0.|  
|**is_output**|**bit**|1 = le paramètre est OUTPUT ou RETURN ; sinon, 0.|  
|**is_cursor_ref**|**bit**|1 = le paramètre est un paramètre de référence de curseur.|  
|**has_default_value**|**bit**|1 = Paramètre par défaut.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve seulement les valeurs par défaut des objets CLR dans cet affichage catalogue ; par conséquent, cette colonne a une valeur nulle (0) pour les objets [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour afficher la valeur par défaut d’un paramètre dans un [!INCLUDE[tsql](../../includes/tsql-md.md)] objet, interrogez la colonne de **définition** de l’affichage catalogue [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) ou utilisez la fonction système [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) .|  
|**is_xml_document**|**bit**|1 = Le contenu est un document XML complet.<br /><br /> 0 = le contenu est un fragment de document ou le type de données de la colonne n’est pas **XML**.|  
|**default_value**|**sql_variant**|Si **has_default_value** a la valeur 1, la valeur de cette colonne est la valeur par défaut du paramètre ; Sinon, NULL.|  
|**xml_collection_id**|**int**|Valeur différente de zéro si le type de données du paramètre est **XML** et que le XML est typé. La valeur est l'identificateur de la collection contenant l'espace de noms du schéma XML de validation du paramètre.<br /><br /> 0 = Aucune collection de schéma XML.|  
|**is_readonly**|**bit**|1 = Le paramètre est READONLY ; sinon, 0.|  
|**is_nullable**|**bit**|1 = Le paramètre accepte la valeur Null. (valeur par défaut).<br /><br /> 0 = Le paramètre n'accepte pas la valeur null, pour une exécution plus efficace des procédures stockées compilées en mode natif.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.all_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-parameters-transact-sql.md)   
 [sys.system_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-parameters-transact-sql.md)  
  
  
