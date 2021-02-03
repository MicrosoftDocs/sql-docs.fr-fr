---
description: ShortestLineTo (type de données geography)
title: ShortestLineTo (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ShortestLineTo_TSQL
- ShortestLineTo
dev_langs:
- TSQL
helpviewer_keywords:
- ShortestLineTo method (geography)
ms.assetid: 9d7c9885-5d1b-49ae-af31-5ef9fb8acaba
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 90333e51c46320e2910f05b6f66567f7538c9919
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210118"
---
# <a name="shortestlineto-geography-data-type"></a>ShortestLineTo (type de données geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une instance **LineString** avec deux points qui représentent la distance la plus courte entre les deux instances **geography**. La longueur de l’instance **LineString** retournée correspond à la distance entre les deux instances **geography**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.ShortestLineTo ( geography_other )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *geography_other*  
 Spécifie la deuxième instance **geography** dont l’instance **geography** appelante tente de déterminer la distance la plus courte.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
## <a name="remarks"></a>Notes  
 La méthode retourne une instance **LineString** avec des points de terminaison situés sur les bordures des deux instances **geography** sans intersection qui sont comparées. La longueur du **LineString** retourné est égale à la distance la plus courte entre les deux instances **geography**. Une instance **LineString** vide est retournée quand les deux instances **geography** se croisent.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-calling-shortestlineto-on-non-intersecting-instances"></a>R. Appel de ShortestLineTo() sur des instances qui ne se croisent pas  
 Cet exemple recherche la distance la plus courte entre une instance `CircularString` et une instance `LineString` et retourne l'instance `LineString` qui connecte les deux points :  
  
 ```
 DECLARE @g1 geography = 'CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 DECLARE @g2 geography = 'LINESTRING(-119.119263 46.183634, -119.273071 47.107523, -120.640869 47.569114, -122.200928 47.454094)';  
 SELECT @g1.ShortestLineTo(@g2).ToString();
 ```  
  
### <a name="b-calling-shortestlineto-on-intersecting-instances"></a>B. Appel de ShortestLineTo() sur des instances qui se croisent  
 Cet exemple retourne une instance `LineString` vide du fait que l'instance `LineString` croise l'instance `CircularString` :  
  
 ```
 DECLARE @g1 geography = 'CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 DECLARE @g2 geography = 'LINESTRING(-119.119263 46.183634, -119.273071 47.107523, -120.640869 47.569114, -122.348 47.649, -122.681 47.655)';  
 SELECT @g1.ShortestLineTo(@g2).ToString();
``` 
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes étendues sur des instances geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
 [ShortestLineTo (type de données geometry)](../../t-sql/spatial-geometry/shortestlineto-geometry-data-type.md)  
  
