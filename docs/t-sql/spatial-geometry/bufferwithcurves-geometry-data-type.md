---
description: BufferWithCurves (type de données geometry)
title: BufferWithCurves (type de données geometry) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- BufferWithCurves method (geometry)
ms.assetid: 8ffaba3f-d2dd-4e57-9f41-3ced9f14b600
author: MladjoA
ms.author: mlandzic
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 18691c1a3e02d93d7c98a6b290672af5c77f15c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347534"
---
# <a name="bufferwithcurves-geometry-data-type"></a>BufferWithCurves (type de données geometry)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Retourne une instance **geometry** qui représente l’ensemble de tous les points dont la distance par rapport à l’instance **geometry** appelante est inférieure ou égale au paramètre *distance*.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
.BufferWithCurves ( distance )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments  
 *distance*  
 Expression **float** indiquant la distance maximale à laquelle les points qui forment la mémoire tampon peuvent se trouver par rapport à l’instance **geometry**.  
  
## <a name="return-types"></a>Types de retour
Type de retour SQL Server : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="exceptions"></a>Exceptions  
 Les critères suivants lèvent **ArgumentException**.  
  
-   Aucun paramètre n’est passé à la méthode, par exemple `@g.BufferWithCurves()`  
  
-   Un paramètre non numérique est passé à la méthode, par exemple `@g.BufferWithCurves('a')`  
  
-   **NULL** est passé à la méthode, par exemple `@g.BufferWithCurves(NULL)`  
  
## <a name="remarks"></a>Notes  
 L'illustration suivante montre un exemple d'une instance géométrique retournée par cette méthode.  
  
 ![Diagramme montrant un exemple d’une instance géométrique retournée par cette méthode.](../../t-sql/spatial-geometry/media/bufferedcurve.gif)
  
 Le tableau suivant affiche les résultats retournés pour différentes valeurs de distance.  
  
|Valeur de distance|Dimensions de type|Type spatial retourné|  
|--------------------|---------------------|---------------------------|  
|distance < 0|Zéro ou une|Instance **GeometryCollection** vide|  
|distance < 0|Deux ou plus|Instance **CurvePolygon** ou **GeometryCollection** avec une mémoire tampon négative. **Remarque :** Une mémoire tampon négative peut créer un **GeometryCollection** vide|  
|distance = 0|Toutes les dimensions|Copie de l’instance **geometry** appelante|  
|distance > 0|Toutes les dimensions|Instance **CurvePolygon** ou **GeometryCollection**|  
  
> [!NOTE]  
>  Dans la mesure où *distance* est de type **float**, une valeur très petite peut être équivalente à zéro dans les calculs. Quand cela se produit, une copie de l’instance **geometry** appelante est retournée. Consultez [float et real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md).  
  
 Une mémoire tampon négative supprime tous les points compris dans la distance donnée de la limite de la géométrie. L'illustration suivante montre une mémoire tampon négative sous la forme d'une zone hachurée plus claire du cercle. Le trait en pointillé représente la limite du polygone d'origine et la ligne continue la limite du polygone obtenu.  
  
 Si un paramètre **string** est passé à la méthode, il est converti en **float** ou lève `ArgumentException`.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-calling-bufferwithcurves-with-a-parameter-value--0-on-one-dimensional-geometry-instance"></a>R. Appel de BufferWithCurves() avec une valeur de paramètre < 0 sur une instance géométrique unidimensionnelle  
 L'exemple suivant retourne une instance `GeometryCollection` vide :  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(-1).ToString(); 
 ```
  
### <a name="b-calling-bufferwithcurves-with-a-parameter-value--0-on-a-two-dimensional-geometry-instance"></a>B. Appel de BufferWithCurves() avec une valeur de paramètre < 0 sur une instance géométrique bidimensionnelle  
 L'exemple suivant retourne une instance `CurvePolygon` avec une mémoire tampon négative :  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.BufferWithCurves(-1).ToString()
 ```  
  
### <a name="c-calling-bufferwithcurves-with-a-parameter-value--0-that-returns-an-empty-geometrycollection"></a>C. Appel de BufferWithCurves() avec une valeur de paramètre < 0 qui retourne un GeometryCollection vide  
 L’exemple suivant montre ce qui se passe quand le paramètre *distance* est égal à -2 :  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.BufferWithCurves(-2).ToString();
 ```  
  
 Cette instruction **SELECT** retourne `GEOMETRYCOLLECTION EMPTY`  
  
### <a name="d-calling-bufferwithcurves-with-a-parameter-value--0"></a>D. Appel de BufferWithCurves() avec une valeur de paramètre = 0  
 L’exemple suivant retourne une copie de l’instance **geometry** appelante :  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(0).ToString();
 ```  
  
### <a name="e-calling-bufferwithcurves-with-a-non-zero-parameter-value-that-is-extremely-small"></a>E. Appel de BufferWithCurves() avec une valeur de paramètre non nulle extrêmement petite  
 L’exemple suivant retourne également une copie de l’instance **geometry** appelante :  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 DECLARE @distance float = 1e-20; 
 SELECT @g.BufferWithCurves(@distance).ToString();
 ```  
  
### <a name="f-calling-bufferwithcurves-with-a-parameter-value--0"></a>F. Appel de BufferWithCurves() avec une valeur de paramètre > 0  
 L'exemple suivant retourne une instance `CurvePolygon` :  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(2).ToString();
 ```  
  
### <a name="g-passing-a-valid-string-parameter"></a>G. Transmission d'un paramètre de chaîne valide  
 L'exemple suivant retourne la même instance `CurvePolygon` qu'indiqué précédemment, mais un paramètre de chaîne est passé à la méthode :  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves('2').ToString();
 ```  
  
### <a name="h-passing-an-invalid-string-parameter"></a>H. Transmission d'un paramètre de chaîne non valide  
 L'exemple suivant génère une erreur :  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)' 
 SELECT @g.BufferWithCurves('a').ToString();
 ```  
  
 Notez que les deux exemples précédents ont passé un littéral de chaîne à la méthode `BufferWithCurves()`. Le premier exemple fonctionne car le littéral de chaîne peut être converti en valeur numérique. Toutefois, le deuxième exemple lève un `ArgumentException`.  
  
### <a name="i-calling-bufferwithcurves-on-multipoint-instance"></a>I. Appel de BufferWithCurves() sur l'instance MultiPoint  
 L'exemple suivant retourne deux instances `GeometryCollection` et une instance `CurvePolygon` :  
  
```
 DECLARE @g geometry = 'MULTIPOINT((1 1),(1 4))'; 
 SELECT @g.BufferWithCurves(1).ToString(); 
 SELECT @g.BufferWithCurves(1.5).ToString(); 
 SELECT @g.BufferWithCurves(1.6).ToString();
 ```  
  
 Les deux premières instructions **SELECT** retournent une instance `GeometryCollection`, car le paramètre *distance* est inférieur ou égal à 1/2 de la distance entre les deux points (1 1) et (1 4). La troisième instruction **SELECT** retourne une instance `CurvePolygon`, car les instances mises en mémoire tampon des deux points (1 1) et (1 4) se chevauchent.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes étendues sur les instances géométriques](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
 
