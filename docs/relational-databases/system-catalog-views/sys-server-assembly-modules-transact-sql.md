---
description: sys.server_assembly_modules (Transact-SQL)
title: sys.server_assembly_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_assembly_modules_TSQL
- sys.server_assembly_modules
- server_assembly_modules
- sys.server_assembly_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_assembly_modules catalog view
ms.assetid: af799e38-2d16-49b2-bcf5-6f9199af899e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: dead9bfa36010e7ffc2835595bcfa4710d582206
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182441"
---
# <a name="sysserver_assembly_modules-transact-sql"></a>sys.server_assembly_modules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient pour chaque module d'assembly une ligne destinée aux déclencheurs de niveau serveur de type TA. Cette vue mappe des déclencheurs d'assembly à leur implémentation CLR sous-jacente. Vous pouvez joindre cette relation à **sys.server_triggers**. L’assembly doit être chargé dans la base de données **Master** . C'est le tuple (object_id) qui correspond à la clé de cette relation.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Contre référence de FOREIGN KEY à l'objet sur lequel ce module d'assembly est défini.|  
|**assembly_id**|**int**|ID de l'assembly à partir duquel ce module a été créé. L'assembly doit être chargé dans la base de données master.|  
|**assembly_class**|**sysname**|Nom de la classe dans l'assembly définissant ce module.|  
|**assembly_method**|**sysname**|Nom de la méthode dans la classe définissant ce module. Correspond à la valeur NULL dans le cas de fonctions d'agrégation (AF, aggregate functions).|  
|**execute_as_principal_id**|**int**|ID de l'instruction d'exécution en tant que principal de serveur (EXECUTE AS).<br /><br /> Valeur NULL par défaut ou dans le cas de l'instruction EXECUTE AS CALLER.<br /><br /> ID du principal spécifié si EXECUTe AS SELF EXECUTe AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
