---
description: MSdbms_datatype (Transact-SQL)
title: MSdbms_datatype (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdbms_datatype
- MSdbms_datatype_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_datatype system table
ms.assetid: 606168cc-79a8-442f-ab43-936f8f884d72
author: cawrites
ms.author: chadam
ms.openlocfilehash: daf45f65ddc0e7ea496a60a3da8b9fc9d270fc73
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098210"
---
# <a name="msdbms_datatype-transact-sql"></a>MSdbms_datatype (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSdbms_datatype** contient la liste complète des types de données natifs au niveau de chaque système de gestion de base de données (SGBD) pris en charge, utilisé comme serveur de publication ou abonné dans la réplication de bases de données hétérogènes. Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**datatype_id**|**int**|Identifie chaque type de données unique.|  
|**dbms_id**|**int**|Identifie le SGBD auquel appartient le type.|  
|**type**|**sysname**|Nom du type de données (natif).|  
|**createparams**|**int**|Bitmap qui décrit la combinaison de longueur, précision et échelle applicable à chaque type de données, à savoir :<br /><br /> **0x1** = précision.<br /><br /> **0X2** = échelle.<br /><br /> **0x4** = longueur.|  
  
## <a name="remarks"></a>Notes  
 Cette table contient les entrées des types de données SQL Server, parce qu'une instance de SQL Server peut à la fois s'abonner à une base de données non-SQL Server et publier sur un abonné non-SQL Server.  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Spécifier des mappages de types de données pour un serveur de publication Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
