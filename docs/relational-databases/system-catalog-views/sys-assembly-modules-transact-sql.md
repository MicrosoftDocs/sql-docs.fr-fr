---
description: sys.assembly_modules (Transact-SQL)
title: sys.assembly_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.assembly_modules
- sys.assembly_modules_TSQL
- assembly_modules
- assembly_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assembly_modules catalog view
ms.assetid: 5f9e644e-8065-49a2-b53d-db7df98f70d8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5d46e4eca31f16a9f244046f05abe6dda44df3fb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099294"
---
# <a name="sysassembly_modules-transact-sql"></a>sys.assembly_modules (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Renvoie une ligne pour chaque fonction, procédure ou déclencheur défini pour un assembly CLR (Common Language Runtime). Cet affichage catalogue mappe des procédures stockées CLR, des déclencheurs CLR ou des fonctions CLR avec leur implémentation sous-jacente. Les objets de type TA, AF, PC, FS et FT ont un module d'assembly associé. Pour trouver l'association entre l'objet et l'assembly, vous pouvez joindre cet affichage catalogue à d'autres. Par exemple, lorsque vous créez une procédure stockée CLR, elle est représentée par une ligne dans **sys. Objects**, une ligne dans **sys. procedures** (qui hérite de **sys. Objects**) et une ligne dans **sys.assembly_modules**. La procédure stockée elle-même est représentée par les métadonnées dans **sys. Objects** et **sys. procedures**. Les références à l’implémentation CLR sous-jacente de la procédure se trouvent dans **sys.assembly_modules**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Numéro d'identification de l'objet SQL. Unique dans une base de données.|  
|**assembly_id**|**int**|ID de l'assembly à partir duquel ce module a été créé.|  
|**assembly_class**|**sysname**|Nom de la classe dans l'assembly qui définit ce module.|  
|**assembly_method**|**sysname**|Nom de la méthode dans la **assembly_class** qui définit ce module.<br /><br /> Les fonctions d'agrégation (AF) ont la valeur NULL.|  
|**null_on_null_input**|**bit**|Le module a été déclaré pour produire une sortie NULL pour toute entrée NULL.|  
|**execute_as_principal_id**|**int**|ID de la base de données principale dans laquelle le contexte est exécuté, comme spécifié par la clause EXECUTE AS de la fonction, la procédure stockée ou le déclencheur CLR.<br /><br /> NULL = EXECUTE AS CALLER. Il s’agit de la valeur par défaut.<br /><br /> ID du principal de base de données spécifié = EXECUTe AS SELF, EXECUTe AS *user_name* ou Execute As *login_name*.<br /><br /> -2 = EXECUTE AS OWNER.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
