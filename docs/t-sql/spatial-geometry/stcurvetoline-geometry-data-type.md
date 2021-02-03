---
description: STCurveToLine (type de données geometry)
title: STCurveToLine (type de données geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveToLine method (geometry)
ms.assetid: abc80b32-4152-4e10-b816-798b901e0ac5
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 81b1a222937a19a8fe18e3cb2261581011206d1b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189495"
---
# <a name="stcurvetoline-geometry-data-type"></a>STCurveToLine (type de données geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Retourne une approximation polygonale d’une instance **geometry** contenant des segments d’arc de cercle.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STCurveToLine ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="remarks"></a>Remarques  
 Retourne une instance **GeometryCollection** vide pour les variables d’instance **geometry** vides, et retourne **NULL** pour les variables **geometry** non initialisées.  
  
 L’approximation polygonale que la méthode retourne dépend de l’instance **geometry** que vous utilisez pour appeler la méthode :  
  
-   Retourne une instance **LineString** pour une instance **CircularString** ou **CompoundCurve**.  
  
-   Retourne une instance **Polygon** pour une instance **CurvePolygon**.  
  
-   Retourne une copie de l’instance **geometry** si cette instance n’est pas une instance **CircularString**, **CompoundCurve** ou **CurvePolygon**. Par exemple, la méthode `STCurveToLine` retourne une instance **Point** pour une instance **geometry** qui est une instance **Point**.  
  
 Contrairement à la spécification SQL/MM, la méthode `STCurveToLine` n’utilise pas de valeurs de coordonnées z pour calculer l’approximation polygonale. La méthode ignore les valeurs de coordonnées z présentes dans l’instance **geometry** appelante.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-an-uninitialized-geometry-variable-and-empty-instance"></a>R. Utilisation d'une variable Geometry non initialisée et d'une instance vide  
 Dans l’exemple suivant, la première instruction **SELECT** utilise une instance non initialisée de **geometry** pour appeler la méthode `STCurveToLine`, alors que la seconde instruction **SELECT** utilise une instance **geometry** vide. Ainsi, la méthode retourne **NULL** pour la première instruction et une collection **GeometryCollection** pour la seconde instruction.  
  
```
 DECLARE @g geometry; 
 SET @g = @g.STCurveToLine(); 
 SELECT @g.STGeometryType(); 
 SET @g = geometry::Parse('LINESTRING EMPTY'); 
 SELECT @g.STGeometryType();
 ```  
  
### <a name="b-using-a-linestring-instance"></a>B. Utilisation d'une instance LineString  
 L’instruction **SELECT** de l’exemple suivant utilise une instance **LineString** pour appeler la méthode STCurveToLine. Ainsi, la méthode retourne une instance **LineString**.  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('LINESTRING(1 3, 5 5, 4 3, 1 3)'); 
 SET @g = @g.STCurveToLine(); 
 SELECT @g.STGeometryType();
 ```  
  
### <a name="c-using-a-circularstring-instance"></a>C. Utilisation d'une instance CircularString  
 La première instruction **SELECT** de l’exemple suivant utilise une instance **CircularString** pour appeler la méthode STCurveToLine. Ainsi, la méthode retourne une instance **LineString**. Cette instruction **SELECT** compare également les longueurs des deux instances, qui sont approximativement les mêmes.  Enfin, la seconde instruction **SELECT** retourne le nombre de points pour chaque instance.  Elle retourne seulement 5 points pour l’instance **CircularString**, mais 65 points pour l’instance **LineString**.  
  
```
 DECLARE @g1 geometry, @g2 geometry; 
 SET @g1 = geometry::Parse('CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0)'); 
 SET @g2 = @g1.STCurveToLine(); 
 SELECT @g1.STGeometryType() AS [G1 Type], @g2.STGeometryType() AS [G2 Type], @g1.STLength() AS [G1 Perimeter], @g2.STLength() AS [G2 Perimeter], @g2.ToString() AS [G2 Def]; 
 SELECT @g1.STNumPoints(), @g2.STNumPoints();
 ```  
  
### <a name="d-using-a-curvepolygon-instance"></a>D. Utilisation d'une instance CurvePolygon  
 L’instruction **SELECT** de l’exemple suivant utilise une instance **CurvePolygon** pour appeler la méthode STCurveToLine. Ainsi, la méthode retourne une instance **Polygon**.  
  
```
 DECLARE @g1 geometry, @g2 geometry; 
 SET @g1 = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0))'); 
 SET @g2 = @g1.STCurveToLine(); 
 SELECT @g1.STGeometryType() AS [G1 Type], @g2.STGeometryType() AS [G2 Type];
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation des types de données spatiales](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [STLength &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)   
 [STNumPoints &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stnumpoints-geometry-data-type.md)   
 [STGeometryType &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)  
  
  

