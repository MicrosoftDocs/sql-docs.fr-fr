---
description: IHpublishercolumnconstraints (Transact-SQL)
title: IHpublishercolumnconstraints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHpublishercolumnconstraints
- IHpublishercolumnconstraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublishercolumnconstraints system table
ms.assetid: d7a41da6-e067-430a-8da2-3f6745b8a4f3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8f82d650cc09c7379c9ad9256413229143be4cdf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092479"
---
# <a name="ihpublishercolumnconstraints-transact-sql"></a>IHpublishercolumnconstraints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table système **IHpublishercolumnconstraints** mappe les colonnes d’une publication non SQL Server dans la table système [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) aux contraintes de la table système [IHpublisherconstraints](../../relational-databases/system-tables/ihpublisherconstraints-transact-sql.md) . Cette table est stockée dans la base de données de distribution.  
  
## <a name="definition"></a>Définition  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publishercolumn_id**|**int**|Identifie la colonne de [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) avec une contrainte associée.|  
|**publisherconstraint_id**|**int**|Identifie une contrainte de [IHpublisherconstraints](../../relational-databases/system-tables/ihpublisherconstraints-transact-sql.md) associée à la colonne.|  
|**indid**|**int**|Indique la position de la colonne dans la table publiée.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
