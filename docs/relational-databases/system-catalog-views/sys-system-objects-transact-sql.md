---
description: sys.system_objects (Transact-SQL)
title: sys.system_objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.system_objects
- system_objects
- system_objects_TSQL
- sys.system_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_objects catalog view
ms.assetid: 069e9045-97f2-4463-8e8f-c73855f3ea0a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c3023fdedcac48278f34fc7759cdcc013c0a6dea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189340"
---
# <a name="syssystem_objects-transact-sql"></a>sys.system_objects (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne pour tous les objets système dont l’étendue est le schéma et qui sont inclus dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Tous les objets système sont contenus dans les schémas nommés sys ou INFORMATION_SCHEMA.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nom d’objet|  
|object_id|**int**|Numéro d'identification de l'objet. Unique dans une base de données.|  
|principal_id|**int**|ID du propriétaire individuel, s’il est différent du propriétaire du schéma. Par défaut, le propriétaire du schéma détient les objets contenus dans le schéma. Il est toutefois possible de spécifier un autre propriétaire à l'aide de l'instruction ALTER AUTHORIZATION.<br /><br /> La valeur est NULL s'il n'y a pas d'autre propriétaire individuel.<br /><br /> Prend la valeur NULL si le type de l'objet est un des types suivants :<br /><br /> C = contrainte CHECK<br /><br /> D = DEFAULT (contrainte ou autonome)<br /><br /> F = Contrainte FOREIGN KEY<br /><br /> PK = Contrainte PRIMARY KEY<br /><br /> R = Règle (ancienne, autonome)<br /><br /> TA = Déclencheur assembly (CLR)<br /><br /> TR = Déclencheur SQL<br /><br /> UQ = Contrainte UNIQUE|  
|schema_id|**int**|ID du schéma dans lequel se trouve l'objet.<br /><br /> Pour tous les objets système compris dans l'étendue du schéma qui sont inclus dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette valeur sera toujours dans (schema_id('sys'), schema_id('INFORMATION_SCHEMA')).|  
|parent_object_id|**int**|Identificateur de l'objet auquel appartient cet objet.<br /><br /> 0 = Il ne s'agit pas d'un objet enfant.|  
|type|**char(2)**|Type d’objet :<br /><br /> AF = Fonction d'agrégation (CLR)<br /><br /> C = contrainte CHECK<br /><br /> D = DEFAULT (contrainte ou autonome)<br /><br /> F = Contrainte FOREIGN KEY<br /><br /> FN = Fonction scalaire SQL<br /><br /> FS = Fonction scalaire d'assembly (CLR)<br /><br /> FT = Fonction table d'assembly (CLR)<br /><br /> IF = Fonction table en ligne SQL<br /><br /> IT = table interne<br /><br /> P = Procédure stockée SQL<br /><br /> PC = procédure stockée d’assembly (CLR)<br /><br /> PG = Repère de plan<br /><br /> PK = Contrainte PRIMARY KEY<br /><br /> R = Règle (ancienne, autonome)<br /><br /> RF = Procédure de filtre de réplication<br /><br /> S = Table de base système<br /><br /> SN = Synonyme<br /><br /> SQ = File d'attente du service<br /><br /> TA = Déclencheur d'assembly DML (CLR)<br /><br /> TF = fonction table SQL<br /><br /> TR = Déclencheur DML SQL<br /><br /> TT = Type de table<br /><br /> U = Table (définie par l'utilisateur)<br /><br /> UQ = Contrainte UNIQUE<br /><br /> V = Vue<br /><br /> X = Procédure stockée étendue|  
|type_desc|**nvarchar(60)**|Description du type d'objet. AGGREGATE_FUNCTION<br /><br /> CHECK_CONSTRAINT<br /><br /> DEFAULT_CONSTRAINT<br /><br /> FOREIGN_KEY_CONSTRAINT<br /><br /> SQL_SCALAR_FUNCTION<br /><br /> CLR_SCALAR_FUNCTION<br /><br /> CLR_TABLE_VALUED_FUNCTION<br /><br /> SQL_INLINE_TABLE_VALUED_FUNCTION<br /><br /> INTERNAL_TABLE<br /><br /> SQL_STORED_PROCEDURE<br /><br /> CLR_STORED_PROCEDURE<br /><br /> PLAN_GUIDE<br /><br /> PRIMARY_KEY_CONSTRAINT<br /><br /> RULE<br /><br /> REPLICATION_FILTER_PROCEDURE<br /><br /> SYSTEM_TABLE<br /><br /> SYNONYM<br /><br /> SERVICE_QUEUE<br /><br /> CLR_TRIGGER<br /><br /> SQL_TABLE_VALUED_FUNCTION<br /><br /> SQL_TRIGGER<br /><br /> TABLE_TYPE<br /><br /> USER_TABLE<br /><br /> UNIQUE_CONSTRAINT<br /><br /> VIEW<br /><br /> EXTENDED_STORED_PROCEDURE|  
|create_date|**datetime**|Date de création de l'objet.|  
|modify_date|**datetime**|Date de la dernière modification de l'objet avec l'instruction ALTER. Si l'objet est une table ou une vue, modify_date change également lorsqu'un index cluster sur la table ou la vue est créé ou modifié.|  
|is_ms_shipped|**bit**|Un composant [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interne a créé l'objet.|  
|is_published|**bit**|L'objet est publié.|  
|is_schema_published|**bit**|Seul le schéma de l'objet est publié.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
