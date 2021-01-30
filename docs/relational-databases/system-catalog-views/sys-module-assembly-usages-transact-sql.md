---
description: sys.module_assembly_usages (Transact-SQL)
title: sys.module_assembly_usages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- module_assembly_usages_TSQL
- module_assembly_usages
- sys.module_assembly_usages_TSQL
- sys.module_assembly_usages
dev_langs:
- TSQL
helpviewer_keywords:
- sys.module_assembly_usages catalog view
ms.assetid: b0f9ffab-6ac7-49d5-8369-477fa6b1c02b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f27e8d337cd7a732031405fbc3184748fbceefc0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191294"
---
# <a name="sysmodule_assembly_usages-transact-sql"></a>sys.module_assembly_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque référence module/assembly.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Numéro d'identification de l'objet SQL. Unique dans une base de données.|  
|**assembly_id**|**int**|ID de l'assembly à partir duquel ce module a été créé.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
