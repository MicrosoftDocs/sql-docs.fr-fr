---
description: DOMAIN_CONSTRAINTS (Transact-SQL)
title: DOMAIN_CONSTRAINTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DOMAIN_CONSTRAINTS
- DOMAIN_CONSTRAINTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.DOMAIN_CONSTRAINTS view
- DOMAIN_CONSTRAINTS view
ms.assetid: 436c4480-f1e3-403f-b2bd-de04539afe3c
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0382fd441570730c2e5781fc90027855d0730ff7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189291"
---
# <a name="domain_constraints-transact-sql"></a>DOMAIN_CONSTRAINTS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une ligne pour chaque type d'alias dans la base de données actuelle, qui a une règle lui étant associée et à laquelle l'utilisateur actuel peut accéder.  
  
 Pour récupérer des informations de ces vues, spécifiez le nom complet de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**CONSTRAINT_CATALOG**|**nvarchar (** 128 **)**|Base de données dans laquelle se trouve la règle.|  
|**CONSTRAINT_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient la contrainte.<br /><br /> Important n’utilisez pas les vues de INFORMATION_SCHEMA pour déterminer le schéma d’un objet. <strong> \* \* \* \* </strong> Les vues de INFORMATION_SCHEMA représentent seulement un sous-ensemble des métadonnées d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet consiste à interroger l’affichage catalogue sys. Objects.|  
|**CONSTRAINT_NAME**|**sysname**|Nom de la règle.|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|Base de données dans laquelle réside le type de données alias.|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient le type de données alias.<br /><br /> Important n’utilisez pas les vues de INFORMATION_SCHEMA pour déterminer le schéma d’un type de données. <strong> \* \* \* \* </strong> La seule méthode fiable pour rechercher le schéma d'un type est d'utiliser la fonction TYPEPROPERTY.|  
|**DOMAIN_NAME**|**sysname**|Type de données alias.|  
|**IS_DEFERRABLE**|**varchar (** 2 **)**|Spécifie si la vérification de contrainte peut être différée. Renvoie toujours NO.|  
|**INITIALLY_DEFERRED**|**varchar (** 2 **)**|Spécifie si la vérification des contraintes est différée au départ. Renvoie toujours NO.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)  
  
