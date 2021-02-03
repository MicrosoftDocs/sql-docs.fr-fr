---
description: STIntersection (type de données geometry)
title: STIntersection (type de données geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STIntersection_TSQL
- STIntersection (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STIntersection (geometry Data Type)
ms.assetid: 354843f5-cc14-478c-974a-04f363f9530f
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 72d740400b22c08d0803ca90ba204fecfd4dcd77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195724"
---
# <a name="stintersection-geometry-data-type"></a>STIntersection (type de données geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne un objet qui représente les points où une instance **geometry** entre en intersection avec une autre instance **geometry**.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STIntersection ( other_geometry )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *other_geometry*  
 Autre instance **geometry** à comparer à l’instance sur laquelle `STIntersection()` est appelé, pour déterminer leur intersection.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="remarks"></a>Remarques  
 `STIntersection()` retourne toujours une valeur Null si les SRID (ID de référence spatiale) des instances **geometry** ne correspondent pas. Le résultat peut contenir des segments d'arc de cercle uniquement si les instances d'entrée les contiennent.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-stintersection-on-polygon-instances"></a>R. Utilisation de STIntersection() sur les instances Polygon  
 L'exemple suivant utilise `STIntersection()` pour calculer l'intersection de deux polygones.  
  
```  
DECLARE @g geometry;  
DECLARE @h geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 0 2, 2 2, 2 0, 0 0))', 0);  
SET @h = geometry::STGeomFromText('POLYGON((1 1, 3 1, 3 3, 1 3, 1 1))', 0);  
SELECT @g.STIntersection(@h).ToString();  
```  
  
### <a name="b-using-stintersection-with-curvepolygon-instance"></a>B. Utilisation de STIntersection() avec une instance CurvePolygon  
 L'exemple suivant retourne une instance qui contient un segment d'arc de cercle.  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON (CIRCULARSTRING (0 -4, 4 0, 0 4, -4 0, 0 -4))';  
 DECLARE @h geometry = 'POLYGON ((1 -1, 5 -1, 5 3, 1 3, 1 -1))';  
 SELECT @h.STIntersection(@g).ToString();
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes OGC sur des instances geography](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

