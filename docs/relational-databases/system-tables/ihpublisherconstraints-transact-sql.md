---
description: IHpublisherconstraints (Transact-SQL)
title: IHpublisherconstraints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHpublisherconstraints
- IHpublisherconstraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublisherconstraints system table
ms.assetid: 537b1e1a-7228-4680-aa27-5ad7072ea01e
author: cawrites
ms.author: chadam
ms.openlocfilehash: d53c7bdee115715b8513356f02a3afddc75ec118
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092446"
---
# <a name="ihpublisherconstraints-transact-sql"></a>IHpublisherconstraints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table système **IHpublisherconstraints** contient une ligne pour chaque contrainte répliquée à partir d’éditeurs non-SQL Server à l’aide du serveur de distribution actuel. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisherconstraint_id**|**int**|Identifie une contrainte publiée.|  
|**table_id**|**int**|Identifie la table de [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md) à laquelle la contrainte appartient.|  
|**publisher_id**|**smallint**|Identifie le serveur de publication non SQL Server à partir duquel la colonne est publiée.|  
|**Nom**|**Sa**|Nom de la contrainte publiée.|  
|**Type**|**nvarchar(255)**|Type de contrainte pris en charge à partir de la table système [IHconstrainttypes](../../relational-databases/system-tables/ihconstrainttypes-transact-sql.md) .|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
