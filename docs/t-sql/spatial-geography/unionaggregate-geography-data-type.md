---
description: UnionAggregate (type de données geography)
title: UnionAggregate (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- UnionAggregate
- UnionAggregate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UnionAggregate method (geography)
ms.assetid: 1a3aeef1-5b0e-4ae8-aeb7-c4aab22f42ab
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 6a31d700c51a4d6bc5836de7d904fdbc3e4644c4
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88488226"
---
# <a name="unionaggregate-geography-data-type"></a>UnionAggregate (type de données geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Effectue une opération d'union sur un jeu d'objets de type geography.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
UnionAggregate ( geography_operand )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *geography_operand*  
 Colonne de table de type **geography** qui contient l’ensemble d’objets **geography** sur lequel effectuer une opération d’union.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
## <a name="remarks"></a>Notes  
 La méthode retourne **null** si l’entrée a des SRID différents. Consultez [Identificateurs de référence spatiale &#40;SRID&#41;](../../relational-databases/spatial/spatial-reference-identifiers-srids.md).  
  
 La méthode ignore les entrées **null**.  
  
> [!NOTE]  
>  La méthode retourne **null** si toutes les valeurs entrées sont **null**.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant effectue un `UnionAggregate` sur un ensemble de points d’emplacement **geography** dans une ville.  
  
 ```
 USE AdventureWorks2012  
 GO  
 SELECT City,  
 geography::UnionAggregate(SpatialLocation) AS SpatialLocation  
 FROM Person.Address  
 WHERE PostalCode LIKE('981%')  
 GROUP BY City;
 ```  
  
## <a name="see-also"></a> Voir aussi  
 [Méthodes geography statiques étendues](../../t-sql/spatial-geography/extended-static-geography-methods.md)  
  
  
