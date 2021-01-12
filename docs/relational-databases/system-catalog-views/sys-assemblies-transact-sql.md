---
description: sys.assemblies (Transact-SQL)
title: sys. Assemblies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.assemblies
- assemblies_TSQL
- sys.assemblies_TSQL
- assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assemblies catalog view
ms.assetid: e321753f-293f-42ab-b225-d118713df40b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6860b76d61730a91140ddf502f1bedf57f607b4a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102886"
---
# <a name="sysassemblies-transact-sql"></a>sys.assemblies (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Retourne une ligne pour chaque assembly.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de l’assembly. Unique dans la base de données.|  
|**principal_id**|**int**|Identificateur du principal qui est propriétaire de cet assembly.|  
|**assembly_id**|**int**|Numéro d'identification de l'assembly Unique dans une base de données.|  
|**clr_name**|**nvarchar(4000)**|Chaîne canonique qui encode le nom simple, le numéro de version, les paramètres régionaux, la clé publique, et l'architecture de l'assembly. Cette valeur identifie de façon univoque l'assembly du côté CLR (Common Language Runtime).|  
|**permission_set**|**tinyint**|Jeu d'autorisations/niveau de sécurité de l'assembly.<br /><br /> 1 = accès sécurisé<br /><br /> 2 = accès externe<br /><br /> 3 = accès non sécurisé|  
|**permission_set_desc**|**nvarchar(60)**|Description du jeu d'autorisations/niveau de sécurité de l'assembly.<br /><br /> SAFE_ACCESS<br /><br /> EXTERNAL_ACCESS<br /><br /> UNSAFE_ACCESS|  
|**is_visible**|**bit**|1 = l'assembly est visible pour inscrire des points d'entrée [!INCLUDE[tsql](../../includes/tsql-md.md)].<br /><br /> 0 = l'assembly est destiné uniquement aux appelants gérés : L'assembly fournit une implémentation interne pour d'autres assemblys de la base de données.|  
|**create_date**|**datetime**|Date de création ou d'inscription de l'assembly.|  
|**modify_date**|**datetime**|Date de modification de l'assembly.|  
|**is_user_defined**|**bit**|Indique la source de l'assembly.<br /><br /> 0 = assemblys définis par le système (tels que Microsoft. SqlServer. types pour le type de données **hierarchyid** )<br /><br /> 1 = assemblys définis par l'utilisateur|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de l’assembly CLR &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/clr-assembly-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ASSEMBLYPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/assemblyproperty-transact-sql.md)  
  
  
