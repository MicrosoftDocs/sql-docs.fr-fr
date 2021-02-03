---
description: STIntersection (type de données geography)
title: STIntersection (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STIntersection_TSQL
- STIntersection (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STIntersection method
ms.assetid: 7e09468f-499f-4a38-ba4b-bb30b8821e3b
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 5f188ae0b9746271d99c0ce91c8d1d2240a5ca20
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207015"
---
# <a name="stintersection-geography-data-type"></a>STIntersection (type de données geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Retourne un objet qui représente les points où une instance **geography** entre en intersection avec une autre instance **geography**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STIntersection ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *other_geography*  
 Autre instance **geography** à comparer à l’instance sur laquelle STIntersection() est appelé.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
## <a name="remarks"></a>Notes  
 L'intersection de deux instances géographiques est retournée.  
  
 STIntersection() retourne toujours une valeur Null si les SRID (identificateurs de référence spatiale) des instances **geography** ne correspondent pas.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge des instances spatiales qui sont plus grandes qu'un hémisphère. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut inclure des instances **FullGlobe** dans le jeu de résultats possibles retournés sur le serveur.  
  
 Le résultat peut contenir des segments d'arc de cercle uniquement si les instances d'entrée contiennent des segments d'arc de cercle.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-computing-the-intersection-of-a-polygon-and-a-linestring"></a>R. Calcul de l'intersection d'une instance Polygon et LineString  
 L'exemple suivant utilise `STIntersection()` pour calculer l'intersection d'un `Polygon` et d'un `LineString`.  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
SET @h = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STIntersection(@h).ToString();  
```  
  
### <a name="b-computing-the-intersection-of-a-polygon-and-a-curvepolygon"></a>B. Calcul de l'intersection d'une instance Polygon et CurvePolygon  
 L'exemple suivant retourne une instance qui contient un segment d'arc de cercle.  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
SET @h = geography::STGeomFromText('CURVEPOLYGON(CIRCULARSTRING(-122.351 47.656, -122.341 47.656, -122.341 47.661, -122.351 47.661, -122.351 47.656))', 4326);  
SELECT @g.STIntersection(@h).ToString();  
```  
  
### <a name="c-computing-the-symmetric-difference-with-fullglobe"></a>C. Calcul de la différence symétrique avec FullGlobe  
 L'exemple suivant compare la différence symétrique d'un `Polygon` avec `FullGlobe`.  
  
```  
DECLARE @g geography = 'POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
SELECT @g.STIntersection('FULLGLOBE').ToString();  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes OGC sur les instances geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
