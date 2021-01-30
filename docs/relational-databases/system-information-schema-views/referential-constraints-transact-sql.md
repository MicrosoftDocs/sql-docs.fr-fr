---
description: REFERENTIAL_CONSTRAINTS (Transact-SQL)
title: REFERENTIAL_CONSTRAINTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- REFERENTIAL_CONSTRAINTS
- REFERENTIAL_CONSTRAINTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.REFERENTIAL_CONSTRAINTS view
- REFERENTIAL_CONSTRAINTS view
ms.assetid: 5d358f18-0a85-4b55-af4b-98d5f4cd1020
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e3ee577dbd1aef1da2da8d22e253bfb90bb9616a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189627"
---
# <a name="referential_constraints-transact-sql"></a>REFERENTIAL_CONSTRAINTS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie une ligne pour chaque contrainte FOREIGN KEY dans la base de données active. Cette vue de schémas d'informations renvoie des informations sur les objets pour lesquels l'utilisateur actuel dispose d'autorisations.  
  
 Pour récupérer des informations de ces vues, spécifiez le nom complet de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**CONSTRAINT_CATALOG**|**nvarchar (** 128 **)**|Qualificateur de la contrainte.|  
|**CONSTRAINT_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient la contrainte.<br /><br /> **&#42;&#42;  &#42;&#42;importante** N’utilisez pas de vues de INFORMATION_SCHEMA pour déterminer le schéma d’un objet. Les vues de INFORMATION_SCHEMA représentent seulement un sous-ensemble des métadonnées d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet consiste à interroger l’affichage catalogue sys. Objects.|  
|**CONSTRAINT_NAME**|**sysname**|Nom de la contrainte.|  
|**UNIQUE_CONSTRAINT_CATALOG**|**nvarchar (** 128 **)**|Identificateur de la contrainte unique.|  
|**UNIQUE_CONSTRAINT_SCHEMA**|**nvarchar (** 128 **)**|Nom du schéma qui contient la contrainte UNIQUE.<br /><br /> **&#42;&#42;  &#42;&#42;importante** N’utilisez pas de vues de INFORMATION_SCHEMA pour déterminer le schéma d’un objet. Les vues de INFORMATION_SCHEMA représentent seulement un sous-ensemble des métadonnées d’un objet. La seule méthode fiable pour rechercher le schéma d’un objet consiste à interroger l’affichage catalogue sys. Objects.|  
|**UNIQUE_CONSTRAINT_NAME**|**sysname**|Contrainte UNIQUE.|  
|**MATCH_OPTION**|**varchar (** 7 **)**|Conditions référentielles de correspondance de contraintes. Renvoie toujours la valeur SIMPLE. Signifie qu'aucune correspondance n'est définie. La condition est considérée comme correspondante si l'une des conditions suivantes est réalisée :<br /><br /> au moins une valeur de la colonne clé étrangère est NULL ;<br /><br /> toutes les valeurs de la colonne clé étrangère ne comportent pas la valeur NULL, et une ligne de la table de clé primaire contient la même clé.|  
|**UPDATE_RULE**|**varchar (** 11 **)**|Action entreprise lorsqu’une [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction enfreint l’intégrité référentielle définie par cette contrainte. Retourne l'une des valeurs suivantes : <br />NO ACTION<br />CASCADE<br />SET NULL<br />SET DEFAULT<br /><br /> Si NO ACTION est spécifié sur une instruction ON UPDATE de cette contrainte, la mise à jour de la clé primaire référencée dans la contrainte n'est pas propagée vers la clé étrangère. Si une telle mise à jour enfreint l'intégrité référentielle car au moins une clé étrangère contient la même valeur, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'apporte aucune modification aux tables parentes et de référence. Par ailleurs, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur.<br /><br /> Si CASCADE est spécifié sur une instruction ON UPDATE de cette contrainte, toute modification de valeur de la clé primaire est automatiquement propagée vers la valeur de la clé étrangère.|  
|**DELETE_RULE**|**varchar (** 11 **)**|Action entreprise lorsqu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] viole l'intégrité référentielle définie par cette contrainte. Retourne l'une des valeurs suivantes : <br />NO ACTION<br />CASCADE<br />SET NULL<br />SET DEFAULT<br /><br /> Si NO ACTION est spécifié sur une instruction ON DELETE de cette contrainte, la suppression de la clé primaire référencée dans la contrainte n'est pas propagée vers la clé étrangère. Si la suppression d'une clé primaire enfreint l'intégrité référentielle car au moins une clé étrangère contient la même valeur, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'apporte aucune modification aux tables parentes et de référence. Par ailleurs, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur.<br /><br /> Si CASCADE est spécifié sur une instruction ON DELETE de cette contrainte, toute modification de valeur de la clé primaire est automatiquement propagée vers la valeur de la clé étrangère.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.foreign_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)  
  
