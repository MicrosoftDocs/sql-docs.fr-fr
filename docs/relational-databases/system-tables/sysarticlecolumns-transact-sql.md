---
description: sysarticlecolumns (Transact-SQL)
title: sysarticlecolumns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysarticlecolumns
- sysarticlecolumns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticlecolumns system table
ms.assetid: 55918592-e05d-43b6-843b-7e4d82fa6275
author: cawrites
ms.author: chadam
ms.openlocfilehash: a7acd26639a87c509b97643d4876b43eaab13f06
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191118"
---
# <a name="sysarticlecolumns-transact-sql"></a>sysarticlecolumns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **sysarticlecolumns** contient une ligne pour chaque colonne de table publiée dans une publication transactionnelle ou d’instantané, et mappe chaque colonne à son article. Cette table est stockée dans la base de données de publication.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Identifie un article.|  
|**colid**|**smallint**|Identifie une colonne dans un article.|  
|**is_udt**|**bit**|Indique si la colonne correspond à une colonne de type de données défini par l'utilisateur (UDT, User-defined Data Type). La valeur **1** indique une colonne UDT.|  
|**is_xml**|**bit**|Indique si la colonne est une colonne **XML** . La valeur **1** indique une colonne XML.|  
|**is_max**|**bit**|Indique si la colonne est une colonne de type de données de valeur élevée, **varchar (max)**, **nvarchar (max)** et **varbinary (max)**. La valeur **1** indique une colonne de valeur élevée.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
