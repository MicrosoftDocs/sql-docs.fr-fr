---
description: CurveToLineWithTolerance (type de données geometry)
title: CurveToLineWithTolerance (type de données geometry) | Microsoft Docs
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
- CurveToLineWithTolerance method (geometry)
ms.assetid: 96871075-1998-4cd9-86b1-3fc55577aee4
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d1851b51e9068c3bd0fcbe7ac559e3d770fc1b48
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198263"
---
# <a name="curvetolinewithtolerance-geometry-data-type"></a>CurveToLineWithTolerance (type de données geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Retourne une approximation polygonale d’une instance **geometry** contenant des segments d’arc de cercle.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.CurveToLineWithTolerance ( tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *tolerance*  
 Expression **double** qui définit l’erreur maximale entre le segment d’arc de cercle d’origine et son approximation linéaire.  
  
 *relative*  
 Expression **bool** qui indique s’il est nécessaire d’utiliser une valeur maximale relative pour l’écart. Lorsque la valeur relative est définie sur False (0), une valeur maximale absolue est définie pour l'écart d'une approximation linéaire. Si la valeur relative est True (1), la tolérance est calculée sous la forme d'un produit du paramètre de tolérance et du diamètre du rectangle englobant pour l'objet spatial.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="exceptions"></a>Exceptions  
 La définition d’une tolérance <= 0 lève une exception `ArgumentOutOfRange`.  
  
## <a name="remarks"></a>Notes  
 Cette méthode peut spécifier une valeur de tolérance d’erreur pour le **LineString** résultant.  
  
 Le tableau affiche le type d'instance retourné par `CurveToLineWithTolerance()` pour différents types.  
  
|Appel du type d'instance|Type spatial retourné|  
|----------------------------|---------------------------|  
|Instance géométrique vide|Instance **GeometryCollection** vide|  
|**Point** et **MultiPoint**|Instance **Point**|  
|**MultiPoint**|Instance **Point** ou **MultiPoint**|  
|**CircularString**, **CompoundCurve** ou **LineString**|Instance **LineString**|  
|**MultiLineString**|Instance **LineString** ou **MultiLineString**|  
|**CurvePolygon** et **Polygon**|Instance **Polygon**|  
|**MultiPolygon**|Instance **Polygon** ou **MultiPolygon**|  
|**GeometryCollection** avec une instance unique qui ne contient pas de segment d’arc de cercle|L’instance contenue dans **GeometryCollection** détermine le type d’instance retourné.|  
|**GeometryCollection** avec une seule instance de segment d’arc de cercle unidimensionnelle (**CircularString**, **CompoundCurve**)|Instance **LineString**|  
|**GeometryCollection** avec une seule instance de segment d’arc de cercle bidimensionnelle (**CurvePolygon**)|Instance **Polygon**|  
|**GeometryCollection** avec plusieurs instances unidimensionnelles|Instance **MultiLineString**|  
|**GeometryCollection** avec plusieurs instances bidimensionnelles|Instance **MultiPolygon**|  
|**GeometryCollection** avec plusieurs instances de différentes dimensions|Instance **GeometryCollection**|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-different-tolerance-values-on-a-circularstring-instance"></a>R. Utilisation de valeurs de tolérance différentes sur une instance CircularString  
 L’exemple suivant montre comment la définition de la tolérance affecte l’instance `LineString` retournée par une instance `CircularString` :  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).STNumPoints(), @g.CurveToLineWithTolerance(0.01, 0).STNumPoints();
 ```  
  
### <a name="b-using-the-method-on-a-multilinestring-instance-containing-one-linestring"></a>B. Utilisation de la méthode sur une instance MultiLineString qui contient un LineString  
 L'exemple suivant montre ce qui est retourné d'une instance `MultiLineString` qui contient uniquement une instance `LineString` :  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('MULTILINESTRING((1 3, 4 8, 6 9))'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
 ```  
  
### <a name="c-using-the-method-on-a-multilinestring-instance-containing-multiple-linestrings"></a>C. Utilisation de la méthode sur une instance MultiLineString qui contient plusieurs LineStrings  
 L'exemple suivant montre ce qui est retourné d'une instance `MultiLineString` qui contient plusieurs instances `LineString` :  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('MULTILINESTRING((1 3, 4 8, 6 9),(4 4, 9 18))'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
 ```  
  
### <a name="d-setting-relative-to-true-for-an-invoking-curvepolygon-instance"></a>D. Définition d'une valeur relative sur True pour appeler une instance CurvePolygon  
 L’exemple suivant utilise une instance `CurvePolygon` pour appeler `CurveToLineWithTolerance()` avec *relative* qui a la valeur true :  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.CurveToLineWithTolerance(.5,1).ToString();
 ```  
  
### <a name="e-using-the-method-on-a-geometrycollection-instance"></a>E. Utilisation de la méthode sur une instance GeometryCollection  
 L'exemple suivant appelle `CurveToLineWithTolerance()` sur une `GeometryCollection` qui contient une instance `CurvePolygon` bidimensionnelle et une instance `CircularString` unidimensionnelle. `CurveToLineWithTolerance()` convertit des types de segment d'arc de cercle en types de segment linéaire et les retourne dans un type `GeometryCollection`.  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('GEOMETRYCOLLECTION(CURVEPOLYGON( COMPOUNDCURVE(CIRCULARSTRING(0 2, 2 0, 4 2), (4 2, 0 2))), CIRCULARSTRING(4 4, 8 6, 9 5))'); 
 SELECT @g.CurveToLineWithTolerance(0.1,0).STNumPoints(), @g.CurveToLineWithTolerance(0.1, 0).ToString();
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [CurveToLineWithTolerance &#40;type de données geography&#41;](../../t-sql/spatial-geography/curvetolinewithtolerance-geography-data-type.md)   
 [STCurveToLine &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stcurvetoline-geometry-data-type.md)  
  
  

