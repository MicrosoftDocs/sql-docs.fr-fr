---
description: COLUMN_DOMAIN_USAGE (Transact-SQL)
title: COLUMN_DOMAIN_USAGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- COLUMN_DOMAIN_USAGE_TSQL
- COLUMN_DOMAIN_USAGE
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.COLUMN_DOMAIN_USAGE view
- COLUMN_DOMAIN_USAGE view
ms.assetid: deb20037-6a51-47ae-9f49-7601698fafaf
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cb4864ff61282f74f2f8c1aa3d609bb2183059d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187345"
---
# <a name="column_domain_usage-transact-sql"></a>COLUMN_DOMAIN_USAGE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie une ligne pour chaque colonne de la base actuelle ayant des données de type alias. Cette vue de schémas d'informations renvoie des informations sur les objets pour lesquels l'utilisateur actuel dispose d'autorisations.  
  
 Pour récupérer des informations de ces vues, spécifiez le nom complet de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|Base de données dans laquelle réside le type de données alias.|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient le type de données alias.<br /><br /> **&#42;&#42;  &#42;&#42;importante** N’utilisez pas de vues de INFORMATION_SCHEMA pour déterminer le schéma d’un type de données. La seule méthode fiable pour rechercher le schéma d'un type est d'utiliser la fonction TYPEPROPERTY.|  
|**DOMAIN_NAME**|**sysname**|Type de données alias.|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Qualificateur de la table.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Propriétaire de la table.<br /><br /> **&#42;&#42;  &#42;&#42;importante** N’utilisez pas de vues de INFORMATION_SCHEMA pour déterminer le schéma d’un objet. Les vues de INFORMATION_SCHEMA représentent seulement un sous-ensemble des métadonnées d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet consiste à interroger l’affichage catalogue sys. Objects.|  
|**TABLE_NAME**|**sysname**|Base de données où le type de données alias est utilisé.|  
|**COLUMN_NAME**|**sysname**|Colonne utilisant le type de données alias.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)  
  
