---
description: BufferWithCurves (type de données geography)
title: BufferWithCurves (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BufferWithCurves
- BufferWithCurves_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- BufferWithCurves method (geography)
ms.assetid: abf0a11c-c99c-4faa-bf80-3ae8e04d7bfb
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: e34c83606e632657ab0e4846a6444ecb0a5050ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200238"
---
# <a name="bufferwithcurves-geography-data-type"></a>BufferWithCurves (type de données geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une instance **geography** qui représente l’ensemble de tous les points dont la distance par rapport à l’instance **geography** appelante est inférieure ou égale au paramètre *distance*.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.BufferWithCurves ( distance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *distance*  
 Expression **float** indiquant la distance maximale à laquelle les points qui forment la mémoire tampon peuvent se trouver par rapport à l’instance geography.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
## <a name="exceptions"></a>Exceptions  
 Les critères suivants lèvent **ArgumentException**.  
  
-   Aucun paramètre n'est passé à la méthode, telle que `@g.BufferWithCurves()`  
  
-   Un paramètre non numérique est passé à la méthode, telle que `@g.BufferWithCurves('a')`  
  
-   **NULL** est passé à la méthode, par exemple `@g.BufferWithCurves(NULL)`  
  
## <a name="remarks"></a>Remarques  
 Le tableau suivant affiche les résultats retournés pour différentes valeurs de distance.  
  
|Valeur de distance|Dimensions de type|Type spatial retourné|  
|--------------------|---------------------|---------------------------|  
|distance < 0|Zéro ou une|Instance **GeometryCollection** vide|  
|distance \< 0|Deux ou plus|Instance **CurvePolygon** ou **GeometryCollection** avec une mémoire tampon négative.<br /><br /> Remarque : Une mémoire tampon négative peut créer un **GeometryCollection** vide|
|distance = 0|Toutes les dimensions|Copie de l’instance **geography** appelante|  
|distance > 0|Toutes les dimensions|Instance **CurvePolygon** ou **GeometryCollection**|  
  
> [!NOTE]  
>  Dans la mesure où *distance* est de type **float**, une valeur très petite peut être équivalente à zéro dans les calculs.  Quand cela se produit, une copie de l’instance **geography** appelante est retournée.  
  
 Si un paramètre **string** est passé à la méthode, il est converti en **float** ou lève `ArgumentException`.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-calling-bufferwithcurves-with-a-parameter-value--0-on-one-dimensional-geography-instance"></a>R. Appel de BufferWithCurves() avec une valeur de paramètre < 0 sur une instance géographique unidimensionnelle  
 L'exemple suivant retourne une instance `GeometryCollection` vide :  
  
 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(-1).ToString();
``` 
  
### <a name="b-calling-bufferwithcurves-with-a-parameter-value--0-on-a-two-dimensional-geography-instance"></a>B. Appel de BufferWithCurves() avec une valeur de paramètre < 0 sur une instance géographique bidimensionnelle  
 L'exemple suivant retourne une instance `CurvePolygon` avec une mémoire tampon négative :  
  
 ```sql
 DECLARE @g geography = 'CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.BufferWithCurves(-1).ToString()
 ```  
  
### <a name="c-calling-bufferwithcurves-with-a-parameter-value--0-that-returns-an-empty-geometrycollection"></a>C. Appel de BufferWithCurves() avec une valeur de paramètre < 0 qui retourne un GeometryCollection vide  
 L’exemple suivant montre ce qui se passe quand le paramètre *distance* est égal à -2 :  
  
 ```sql
 DECLARE @g geography = 'CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.BufferWithCurves(-2).ToString();
 ```  
  
 Cette instruction **SELECT** retourne `GEOMETRYCOLLECTION EMPTY`  
  
### <a name="d-calling-bufferwithcurves-with-a-parameter-value--0"></a>D. Appel de BufferWithCurves() avec une valeur de paramètre = 0  
 L’exemple suivant retourne une copie de l’instance **geography** appelante :  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(0).ToString();
 ```  
  
### <a name="e-calling-bufferwithcurves-with-a-non-zero-parameter-value-that-is-extremely-small"></a>E. Appel de BufferWithCurves() avec une valeur de paramètre non nulle extrêmement petite  
 L’exemple suivant retourne également une copie de l’instance **geography** appelante :  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 DECLARE @distance float = 1e-20;  
 SELECT @g.BufferWithCurves(@distance).ToString();
 ```  
  
### <a name="f-calling-bufferwithcurves-with-a-parameter-value--0"></a>F. Appel de BufferWithCurves() avec une valeur de paramètre > 0  
 L'exemple suivant retourne une instance `CurvePolygon` :  

 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(2).ToString();
 ```  
### <a name="g-passing-a-valid-string-parameter"></a>G. Transmission d'un paramètre de chaîne valide  
 L'exemple suivant retourne la même instance `CurvePolygon` qu'indiqué précédemment, mais un paramètre de chaîne est passé à la méthode :  

 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves('2').ToString();
```  
  
### <a name="h-passing-an-invalid-string-parameter"></a>H. Transmission d'un paramètre de chaîne non valide  
 L'exemple suivant génère une erreur :  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)'  
 SELECT @g.BufferWithCurves('a').ToString();
 ```  
  
 Notez que les deux exemples précédents ont passé un littéral de chaîne à la méthode `BufferWithCurves()`. Le premier exemple fonctionne car le littéral de chaîne peut être converti en valeur numérique. Toutefois, le deuxième exemple lève un `ArgumentException`.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes étendues sur des instances Geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [BufferWithCurves &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/bufferwithcurves-geometry-data-type.md)  
  
  
