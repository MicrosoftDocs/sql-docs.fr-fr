---
description: TABLE_CONSTRAINTS (Transact-SQL)
title: TABLE_CONSTRAINTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- TABLE_CONSTRAINTS
- TABLE_CONSTRAINTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.TABLE_CONSTRAINTS view
- TABLE_CONSTRAINTS view
ms.assetid: 687f3284-2849-4853-8a5c-fc936deceae0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eddfd8e6aa593cde7cfb384d5e0b287095bb3127
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185467"
---
# <a name="table_constraints-transact-sql"></a>TABLE_CONSTRAINTS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie une ligne pour chaque contrainte de table dans la base de données active. Cette vue de schémas d'informations renvoie des informations sur les objets pour lesquels l'utilisateur actuel dispose d'autorisations.  
  
 Pour récupérer des informations de ces vues, spécifiez le nom complet de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**CONSTRAINT_CATALOG**|**nvarchar (** 128 **)**|Qualificateur de la contrainte.|  
|**CONSTRAINT_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient la contrainte.<br /><br /> Important la seule façon fiable de trouver le schéma d’un objet est d’interroger l’affichage catalogue sys. Objects. <strong> \* \* \* \* </strong>|  
|**CONSTRAINT_NAME**|**sysname**|Nom de la contrainte.|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Qualificateur de la table.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient la table.<br /><br /> Important la seule façon fiable de trouver le schéma d’un objet est d’interroger l’affichage catalogue sys. Objects. <strong> \* \* \* \* </strong>|  
|**TABLE_NAME**|**sysname**|Nom de la table.|  
|**CONSTRAINT_TYPE**|**varchar (** 11 **)**|Type de contrainte :<br /><br /> CHECK<br /><br /> UNIQUE<br /><br /> PRIMARY KEY<br /><br /> FOREIGN KEY|  
|**IS_DEFERRABLE**|**varchar (** 2 **)**|Spécifie si la vérification de contrainte peut être différée. Renvoie toujours NO.|  
|**INITIALLY_DEFERRED**|**varchar (** 2 **)**|Spécifie si la vérification des contraintes est différée au départ. Renvoie toujours NO.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.key_constraints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)   
 [sys.check_constraints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)  
  
