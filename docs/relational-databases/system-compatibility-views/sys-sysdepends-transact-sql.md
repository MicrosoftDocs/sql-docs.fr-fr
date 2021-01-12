---
description: sys.sysdepends (Transact-SQL)
title: sys.sysdépend de (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysdepends_TSQL
- sysdepends
- sysdepends_TSQL
- sys.sysdepends
dev_langs:
- TSQL
helpviewer_keywords:
- sysdepends system table
- sys.sysdepends compatibility view
ms.assetid: f9c182cb-386f-4e72-859f-9f1115b389f9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 18abac997911fcd78de49d83d12144821c7764ca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099085"
---
# <a name="syssysdepends-transact-sql"></a>sys.sysdepends (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient des informations de dépendance entre les objets (vues, procédures et déclencheurs) de la base de données et les objets contenus dans leur définition (tables, vues et procédures).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID d'objet.|  
|**depid**|**int**|ID d'objet dépendant.|  
|**number**|**smallint**|Numéro de la procédure.|  
|**depnumber**|**smallint**|Numéro de procédure dépendante.|  
|**statut**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**deptype**|**tinyint**|Identifie le type d'objet dépendant :<br /><br /> 0 = Objet ou colonne (uniquement les références non liées au schéma)<br /><br /> 1 = Objet ou colonne (uniquement les références liées au schéma)|  
|**depdbid**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**depsiteid**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**selall**|**bit**|1 = L'objet est utilisé dans une instruction SELECT *.<br /><br /> 0 = Non.|  
|**resultobj**|**bit**|1 = L'objet est en cours de mise à jour.<br /><br /> 0 = Non.|  
|**readobj**|**bit**|1 = L'objet est en cours de lecture.<br /><br /> 0 = Non.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vues de compatibilité &#40;&#41;Transact-SQL ](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [sp_depends &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-depends-transact-sql.md)   
 [sys.sql_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md)  
  
  
