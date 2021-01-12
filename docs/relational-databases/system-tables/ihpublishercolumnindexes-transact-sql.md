---
description: IHpublishercolumnindexes (Transact-SQL)
title: IHpublishercolumnindexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHpublishercolumnindexes
- IHpublishercolumnindexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublishercolumnindexes system table
ms.assetid: 95b95a1d-b502-4838-825f-82a456487e25
author: cawrites
ms.author: chadam
ms.openlocfilehash: ab3d300d9dc76dd1d4922e6d62b4a6e0f8e1e492
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092489"
---
# <a name="ihpublishercolumnindexes-transact-sql"></a>IHpublishercolumnindexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table système **IHpublishercolumnindexes** mappe les colonnes d’une publication non SQL Server dans la table système [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) à des index dans la table système [IHpublisherindexes](../../relational-databases/system-tables/ihpublisherindexes-transact-sql.md) . Cette table est stockée dans la base de données de distribution.  
  
## <a name="definition"></a>Définition  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publishercolumn_id**|**int**|Identifie la colonne de [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) avec un index associé.|  
|**publisherindex_id**|**int**|Identifie un index à partir de la table [IHpublisherindexes](../../relational-databases/system-tables/ihpublisherindexes-transact-sql.md) associée à la colonne.|  
|**indid**|**int**|Indique la position de la colonne dans la table publiée.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
