---
description: CurveToLineWithTolerance (type de données geography)
title: CurveToLineWithTolerance (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CurveToLineWithTolerance_TSQL
- CurveToLineWithTolerance
dev_langs:
- TSQL
helpviewer_keywords:
- CurveToLineWithTolerance method (geography)
ms.assetid: 74369c76-2cf6-42ae-b9cc-e7a051db2767
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: f19dbc100f507d10a3c1c382735608b307a40ffd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210610"
---
# <a name="curvetolinewithtolerance-geography-data-type"></a>CurveToLineWithTolerance (type de données geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Retourne une approximation polygonale d’une instance **geography** contenant des segments d’arc de cercle.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.CurveToLineWithTolerance( tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
_tolerance_  
Expression **double** qui définit l’erreur maximale entre le segment d’arc de cercle d’origine et son approximation linéaire.  
  
_relative_  
Expression **bool** qui indique s’il est nécessaire d’utiliser une valeur maximale relative pour l’écart. Si la valeur relative est False (0), une valeur maximale absolue est définie pour l'écart d'une approximation linéaire. Si la valeur relative est True (1), la tolérance se calcule en faisant le produit du paramètre de tolérance et du diamètre du rectangle englobant de l'objet spatial.  
  
## <a name="return-types"></a>Types de retour  
Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
Type de retour CLR : **SqlGeography**  
  
## <a name="exceptions"></a>Exceptions  
Si tolerance est <= 0, une exception **ArgumentOutOfRange** est levée.  
  
## <a name="remarks"></a>Remarques  
Cette méthode permet de spécifier une valeur de tolérance d’erreur pour le **LineString** résultant.  
  
La méthode **CurveToLineWithTolerance** retourne une instance **LineString** pour une instance **CircularString** ou **CompoundCurve**, et une instance **Polygon** pour une instance **CurvePolygon**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-different-tolerance-values-on-a-circularstring-instance"></a>R. Utilisation de valeurs de tolérance différentes sur une instance CircularString  
L’exemple suivant montre comment la définition de la tolérance affecte l’instance `LineString` retournée par une instance `CircularString` :  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)');  
SELECT @g.CurveToLineWithTolerance(0.1,0).STNumPoints(), @g.CurveToLineWithTolerance(0.01, 0).STNumPoints();
```  
  
### <a name="b-using-the-method-on-a-multilinestring-instance-containing-one-linestring"></a>B. Utilisation de la méthode sur une instance MultiLineString qui contient un LineString  
L'exemple suivant montre ce qui est retourné d'une instance `MultiLineString` qui contient uniquement une instance `LineString` :  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('MULTILINESTRING((-122.358 47.653, -122.348 47.649))');  
SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
```  
  
### <a name="c-using-the-method-on-a-multilinestring-instance-containing-multiple-linestrings"></a>C. Utilisation de la méthode sur une instance MultiLineString qui contient plusieurs LineStrings  
L'exemple suivant montre ce qui est retourné d'une instance `MultiLineString` qui contient plusieurs instances `LineString` :  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('MULTILINESTRING((-122.358 47.653, -122.348 47.649),(-123.358 47.653, -123.348 47.649))');  
SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
```  
  
### <a name="d-setting-relative-to-true-for-an-invoking-curvepolygon-instance"></a>D. Définition d'une valeur relative sur True pour appeler une instance CurvePolygon  
L’exemple suivant utilise une instance `CurvePolygon` pour appeler `CurveToLineWithTolerance()` avec *relative* qui a la valeur true :  
  
```
DECLARE @g geography = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658), (-122.348 47.658, -122.358 47.658, -122.358 47.653)))';  
SELECT @g.CurveToLineWithTolerance(.5,1).ToString();
```  
  
## <a name="see-also"></a>Voir aussi  
[Méthodes étendues sur des instances geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
