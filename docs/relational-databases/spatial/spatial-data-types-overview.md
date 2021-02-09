---
description: Présentation des types de données spatiales
title: Présentation des types de données spatiales | Microsoft Docs
ms.date: 07/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- geometry data type [SQL Server], understanding
- geography data type [SQL Server], spatial data
- planar spatial data [SQL Server], geometry data type
- spatial data types [SQL Server]
ms.assetid: 1615db50-69de-4778-8be6-4e058c00ccd4
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 87ed2882d8276b3a92add3349b73bf38fe821dc9
ms.sourcegitcommit: 10ae200635b9e8554e6bc6f658125e1a80d4d5ae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99589296"
---
# <a name="spatial-data-types-overview"></a>Présentation des types de données spatiales

[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  
Il existe deux types de données spatiales. Le type de données **geometry** prend en charge les données planaires, ou euclidiennes (monde en deux dimensions). Le type de données **geometry** se conforme à la fois à la *spécification Open Geospatial Consortium (OGC) Simple Features for SQL* version 1.1.0. et à la norme SQL MM (norme ISO).
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend également en charge le type de données **geography**, qui stocke des données ellipsoïdes (monde sphérique), telles que des coordonnées GPS de latitude et de longitude.

## <a name="spatial-data-objects"></a>Objets de données spatiales

Les types de données **geometry** et **geography** prennent en charge 16 types d’objets de données spatiales, ou types d’instances. Toutefois, seuls onze de ces types d’instances sont *instanciables* ; vous pouvez créer et utiliser ces instances (ou les instancier) dans une base de données. Ces instances dérivent certaines propriétés de leurs types de données parents.

La figure ci-dessous montre la hiérarchie geometry sur laquelle les types de données **geometry** et **geography** sont basés. Les types instanciables de **geometry** et **geography** sont indiqués en bleu.  

![geom_hierarchy](../../relational-databases/spatial/media/geom-hierarchy.png)

Il existe un type instanciable supplémentaire pour le type de données geography : **FullGlobe**. Les types **geometry** et **geography** peuvent reconnaître une instance spécifique du moment qu’il s’agit d’une instance bien formée, même si elle n’est pas définie de manière explicite. Par exemple, si vous définissez explicitement une instance **Point** à l’aide de la méthode STPointFromText(), **geometry** et **geography** reconnaissent l’instance comme un **Point**, du moment que l’entrée de méthode est de forme correcte. Si vous définissez la même instance à l’aide de la méthode `STGeomFromText()` , les types de données **geometry** et **geography** reconnaissent l’instance comme un **Point**.  

Les sous-types des types geometry et geography sont divisés en types simples et de collection.  Certaines méthodes, telles que `STNumCurves()` , fonctionnent uniquement avec les types simples.  

Les types simples sont les suivants :

- [Point](../../relational-databases/spatial/point.md)  
- [LineString](../../relational-databases/spatial/linestring.md)  
- [CircularString](../../relational-databases/spatial/circularstring.md)  
- [CompoundCurve](../../relational-databases/spatial/compoundcurve.md)  
- [Polygon](../../relational-databases/spatial/polygon.md)  
- [CurvePolygon](../../relational-databases/spatial/curvepolygon.md)  

Les types de collections sont les suivants :

- [MultiPoint](../../relational-databases/spatial/multipoint.md)  
- [MultiLineString](../../relational-databases/spatial/multilinestring.md)  
- [MultiPolygon](../../relational-databases/spatial/multipolygon.md)  
- [GeometryCollection](../../relational-databases/spatial/geometrycollection.md)  

## <a name="geometry-and-geography-data-type-differences"></a>Différences entre les types de données geometry et geography

Les deux types de données spatiales se comportent souvent de la même façon. Il existe des différences clés dans la façon dont les données sont stockées et manipulées.  

### <a name="how-connecting-edges-are-defined"></a>Mode de définition des arêtes de connexion

Les données de définition concernant les types **LineString** et **Polygon** sont uniquement des sommets. L'arête reliant deux sommets dans un type geometry forme une ligne droite. Toutefois, l'arête reliant deux sommets dans un type geography est un grand arc elliptique court entre les deux sommets. Une grande ellipse correspond à l’intersection de l’ellipsoïde avec un plan passant par son centre. Un grand arc elliptique est un segment d’arc sur la grande ellipse.  

### <a name="how-circular-arc-segments-are-defined"></a>Mode de définition des segments d'arc de cercle

Les segments d'arc de cercle pour les types geography sont définis sur le plan des coordonnées cartésiennes XY (les valeurs Z sont ignorées). Les segments d'arc de cercle pour les types geography sont définis par des segments de courbe sur une sphère de référence. Toute parallèle sur la sphère de référence peut être définie par deux arcs de cercle complémentaires où les points des deux arcs ont un angle de latitude constant.  

### <a name="measurements-in-spatial-data-types"></a>Mesures dans les types de données spatiales

Dans le système planaire (monde en deux dimensions), les mesures de distances et de surfaces sont données dans la même unité de mesure que les coordonnées. Avec le type de données **geometry**, la distance entre (2, 2) et (5, 6) est cinq unités, quelles que soient les unités utilisées.  

Dans un système ellipsoïdal, ou monde sphérique, les coordonnées sont données en degrés de latitude et de longitude. Toutefois, les longueurs et surfaces sont généralement mesurées en mètres et en mètres carrés, bien que la mesure puisse dépendre de l’[identificateur de référence spatiale](./spatial-reference-identifiers-srids.md) de l’instance **geography**. L’unité de mesure la plus courante pour le type de données **geography** est le mètre.  

### <a name="orientation-of-spatial-data"></a>Orientation des données spatiales

L’orientation d’anneau d’un polygone n’est pas un facteur important dans le système planaire. La *spécification OGC Simple Features for SQL* ne stipule pas d’ordonnancement d’anneau et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’applique pas d’ordonnancement d’anneau.  

Dans un système ellipsoïdal, un polygone sans orientation n’a aucune signification ou est ambigu. Par exemple, un anneau autour de l'équateur décrit-il l'hémisphère Nord ou Sud ? Si nous utilisons le type de données **geography** pour stocker l’instance spatiale, nous devons spécifier l’orientation de l’anneau et décrire précisément l’emplacement de l’instance.

L’intérieur du polygone dans un système ellipsoïdal est défini par la « règle de gauche » : si vous vous imaginez en train de longer l’anneau formé par un polygone geography en suivant les points dans l’ordre dans lequel ils sont listés, la zone de gauche est considérée comme l’intérieur du polygone et la zone de droite comme l’extérieur.

Quand le niveau de compatibilité est égal ou inférieur à 100 dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , le type de données **geography** présente les restrictions suivantes :

- Chaque instance **geography** doit être contenue à l’intérieur d’un seul hémisphère. Aucun objet spatial plus grand qu'un hémisphère ne peut être stocké.

- Toute instance **geography** d’une représentation WKB (Well-Known Binary) ou WKT (Well-Known Text) OGC (Open Geospatial Consortium) qui produit un objet plus grand qu’un hémisphère lève une **ArgumentException**.  

- Les méthodes de type de données **geography** qui nécessitent l’entrée de deux instances **geography** , telles que STIntersection(), STUnion(), STDifference() et STSymDifference(), retournent Null si les résultats des méthodes ne s’ajustent pas à l’intérieur d’un seul hémisphère. STBuffer() retourne également null si la sortie dépasse un seul hémisphère.  

Dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], **FullGlobe** est un type spécial de polygone qui couvre le globe entier. Il a une zone, mais aucune bordure ni sommet.  

### <a name="outer-and-inner-rings-in-geography-data-type"></a>Anneaux internes et externes dans le type de données `geography`

La *spécification OGC Simple Features for SQL* traite des anneaux externes et internes, mais cette distinction n’a que peu de sens pour le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **geography**. Tout anneau d’un polygone peut être accepté comme étant l’anneau externe.  

Pour plus d’informations sur les spécifications OGC, reportez-vous aux documents suivants :

- [OGC Specifications, Simple Feature Access Part 1 - Common Architecture](https://go.microsoft.com/fwlink/?LinkId=93627)  
- [OGC Specifications, Simple Feature Access Part 2 - SQL Options](https://go.microsoft.com/fwlink/?LinkId=93628)  

## <a name="circular-arc-segments"></a>Segments d’arc de cercle

Trois types instanciables acceptent des segments d’arc de cercle : **CircularString**, **CompoundCurve** et **CurvePolygon**.  Un segment d’arc de cercle est défini par trois points dans un plan à deux dimensions ; le troisième point doit être différent du premier point. Quelques exemples de segments d’arc de cercle :

![segments d’arc de cercle](../../relational-databases/spatial/media/7e382f76-59da-4b62-80dc-caf93e637c14.gif)

Les deux premiers exemples montrent des segments d’arc de cercle types. Remarquez comment chacun des trois points se situe sur le périmètre d’un cercle.  

Les deux autres exemples montrent comment un segment de ligne peut être défini comme un segment d’arc de cercle. Trois points sont toujours nécessaires pour définir le segment d’arc de cercle contrairement à un segment de ligne ordinaire, qui peut être défini par deux points seulement.

Les méthodes qui fonctionnent sur les types de segment d’arc de cercle utilisent des segments de ligne droite pour se rapprocher de l’arc de cercle. Le nombre de segments de ligne utilisé pour se rapprocher de l'arc dépend de la longueur et de la courbure de l'arc. Les valeurs Z peuvent être stockées pour chacun des types de segment d’arc de cercle, mais elles ne sont pas utilisées dans les calculs.  

> [!NOTE]  
> Si des valeurs Z sont fournies pour les segments d'arc de cercle, elles doivent être identiques pour tous les points dans le segment d'arc de cercle pour que ce dernier soit accepté comme entrée. Par exemple, `CIRCULARSTRING(0 0 1, 2 2 1, 4 0 1)` est autorisé, contrairement à `CIRCULARSTRING(0 0 1, 2 2 2, 4 0 1)` .  

### <a name="linestring-and-circularstring-comparison"></a>Comparaison de LineString et de CircularString

Cet exemple indique comment stocker des triangles isocèles identiques à l’aide d’une instance **LineString** et d’une instance **CircularString** :  

```sql
DECLARE @g1 geometry;
DECLARE @g2 geometry;
SET @g1 = geometry::STGeomFromText('LINESTRING(1 1, 5 1, 3 5, 1 1)', 0);
SET @g2 = geometry::STGeomFromText('CIRCULARSTRING(1 1, 3 1, 5 1, 4 3, 3 5, 2 3, 1 1)', 0);
IF @g1.STIsValid() = 1 AND @g2.STIsValid() = 1
  BEGIN
      SELECT @g1.ToString(), @g2.ToString()
      SELECT @g1.STLength() AS [LS Length], @g2.STLength() AS [CS Length]
  END
```  

Notez qu’une instance **CircularString** nécessite sept points pour définir le triangle. L’instance **LineString** nécessite seulement quatre points pour définir le triangle. En effet, une instance **CircularString** stocke des segments d’arc de cercle et non des segments de ligne. Les côtés du triangle stockés dans l’instance **CircularString** sont ABC, CDE et EFA. Les côtés du triangle stockés dans l’instance **LineString** sont AC, CE et EA.  

Prenons l’exemple suivant :  

```sql
SET @g1 = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 4 0)', 0);
SET @g2 = geometry::STGeomFromText('CIRCULARSTRING(0 0, 2 2, 4 0)', 0);
SELECT @g1.STLength() AS [LS Length], @g2.STLength() AS [CS Length];
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
LS Length    CS Length
5.65685...   6.28318...
```

Les instances **CircularString** utilisent moins de points pour stocker des limites de courbe avec une précision supérieure que les instances **LineString**. Les instances **CircularString** conviennent particulièrement bien au stockage de limites circulaires, comme par exemple un rayon de recherche de 20 kilomètres autour d’un point spécifique. Les instances **LineString** conviennent particulièrement bien au stockage de limites qui sont linéaires, comme un bloc d’agglomération carré.  

### <a name="linestring-and-compoundcurve-comparison"></a>Comparaison de LineString et de CompoundCurve

Les exemples de code suivants montrent comment stocker la même figure à l’aide des instances **LineString** et **CompoundCurve** :

```sql
SET @g = geometry::Parse('LINESTRING(2 2, 4 2, 4 4, 2 4, 2 2)');
SET @g = geometry::Parse('COMPOUNDCURVE((2 2, 4 2), (4 2, 4 4), (4 4, 2 4), (2 4, 2 2))');
SET @g = geometry::Parse('COMPOUNDCURVE((2 2, 4 2, 4 4, 2 4, 2 2))');
```

Dans les exemples ci-dessus, une instance **LineString** ou une instance **CompoundCurve** pourrait stocker la figure.  L’exemple suivant utilise un **CompoundCurve** pour stocker un graphique en secteurs :  

```sql
SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(2 2, 1 3, 0 2),(0 2, 1 0, 2 2))');  
```  

Une instance **CompoundCurve** peut stocker le segment d’arc de cercle (2 2, 1 3, 0 2) directement, mais une instance **LineString** doit convertir la courbe en plusieurs segments de ligne plus petits.  

### <a name="circularstring-and-compoundcurve-comparison"></a>Comparaison de CircularString et de CompoundCurve

L’exemple de code suivant indique comment le graphique en secteurs peut être stocké dans une instance **CircularString** :  

```sql
DECLARE @g geometry;
SET @g = geometry::Parse('CIRCULARSTRING( 0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)');
SELECT @g.ToString(), @g.STLength();
```

Le stockage du graphique en secteurs à l’aide d’une instance **CircularString** nécessite l’utilisation de trois points pour chaque segment de ligne. Si un point intermédiaire n’est pas connu, il doit être calculé, ou le point de terminaison du segment de ligne doit être doublé comme le montre l’extrait de code suivant :  

```sql
SET @g = geometry::Parse('CIRCULARSTRING( 0 0, 3 6.3246, 3 6.3246, 0 7, -3 6.3246, 0 0, 0 0)');
```

Les instances **CompoundCurve** autorisent à la fois les composants **LineString** and **CircularString** . Ainsi, seuls deux points des segments de ligne du graphique en secteurs sont nécessaires.  Cet exemple de code indique comment utiliser un **CompoundCurve** pour stocker la même figure :

```sql
DECLARE @g geometry;
SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING( 3 6.3246, 0 7, -3 6.3246), (-3 6.3246, 0 0, 3 6.3246))');
SELECT @g.ToString(), @g.STLength();
```

### <a name="polygon-and-curvepolygon-comparison"></a>Comparaison de Polygon et de CurvePolygon

Les instances **CurvePolygon** peuvent utiliser des instances **CircularString** et **CompoundCurve** instances when defining their exterior et interior rings. Les instances **Polygon** ne le peuvent pas.

## <a name="see-also"></a>Voir aussi

- [Données spatiales (SQL Server)](./spatial-data-sql-server.md)
- [Référence de méthodes de type de données geometry](../../t-sql/spatial-geometry/spatial-types-geometry-transact-sql.md)
- [Référence de méthodes de type de données geography](../../t-sql/spatial-geography/spatial-types-geography.md)
- [STNumCurves &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stnumcurves-geometry-data-type.md)
- [STNumCurves &#40;type de données geography&#41;](../../t-sql/spatial-geography/stnumcurves-geography-data-type.md)
- [STGeomFromText &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stgeomfromtext-geometry-data-type.md)
- [STGeomFromText &#40;type de données geography&#41;](../../t-sql/spatial-geography/stgeomfromtext-geography-data-type.md)