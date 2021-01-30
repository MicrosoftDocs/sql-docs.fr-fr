---
description: MSdatatype_mappings (Transact-SQL)
title: MSdatatype_mappings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdatatype_mappings
- MSdatatype_mappings_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdatatype_mappings view
ms.assetid: 13cdabb3-6e07-4e8d-ae80-4235022ccc7f
author: stevestein
ms.author: sstein
ms.openlocfilehash: bbafdd41f1c1c3c10425a3d5f63af103a23dc9a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160411"
---
# <a name="msdatatype_mappings-transact-sql"></a>MSdatatype_mappings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vue **MSdatatype_mappings** mappe SQL Server types de données aux types de données utilisés par les systèmes de gestion de base de données non SQL Server (SGBD). Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**dbms_name**|**nvarchar(128)**|Nom du SGBD. Vous trouverez ci-dessous les valeurs possibles et leurs descriptions.<br /><br /> **MSSQLSERVER**: la destination est une base de données SQL Server.<br />**Oracle**: la destination est une base de données Oracle.<br />**DB2**: la destination est une base de données IBM DB2.<br />**Sybase**: la destination est une base de données Sybase.|  
|**sql_type**|**nvarchar(128)**|Type de données SQL Server.|  
|**dest_type**|**nvarchar(128)**|Nom du type de données non SQL Server.|  
|**dest_prec**|**bigint**|Précision du type de données non SQL Server.|  
|**dest_create_params**|**int**|À usage interne uniquement|  
|**dest_nullable**|**bit**|Indique si le type de données non SQL Server prend en charge une valeur NULL.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Spécifier des mappages de types de données pour un serveur de publication Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
