---
description: MSdbms (Transact-SQL)
title: MSdbms (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdbms_TSQL
- MSdbms
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms system table
ms.assetid: 2be631bf-de09-4e7a-9ccb-d6c37b81c237
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2dcea30947cfe6e2f124854d21d0bc48a8055469
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091461"
---
# <a name="msdbms-transact-sql"></a>MSdbms (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSdbms** contient une liste maître de toutes les versions des systèmes de gestion de base de données (SGBD) prises en charge pour la réplication de bases de données hétérogènes. Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**dbms_id**|**int**|Identifie chaque SGBD et version uniques.|  
|**gèrent**|**sysname**|Nom SGBD.<br /><br /> MSSQLSERVER<br /><br /> DB2<br /><br /> ORACLE<br /><br /> SYBASE|  
|**version**|**varchar(10**|Version SGBD.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
