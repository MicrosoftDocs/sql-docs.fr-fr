---
description: MultiPolygon
title: MultiPolygon | Microsoft Docs
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- MultiPolygon geometry subtype [SQL Server]
- geometry subtypes [SQL Server]
ms.assetid: 2c5db358-2a16-49d9-aac5-a74e86813932
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1bcba3499332ca0c41dfd6229bfdb1deca2e7979
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475360"
---
# <a name="multipolygon"></a>MultiPolygon
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  Une instance **MultiPolygon** est une collection de zéro ou plusieurs instances **Polygon** .  
  
## <a name="polygon-instances"></a>Instances Polygon  
 L’illustration suivante montre des exemples d’instances **MultiPolygon** .  
  
 ![Exemples d'instances MultiPolygon géométriques](../../relational-databases/spatial/media/multipolygon.gif "Exemples d'instances MultiPolygon géométriques")  
  
 Comme indiqué par l'illustration :  
  
-   La figure 1 est une instance **MultiPolygon** avec deux éléments **Polygon** . La limite est définie par les deux anneaux extérieurs et les trois anneaux intérieurs.  
  
-   La figure 2 est une instance **MultiPolygon** avec deux éléments **Polygon** . La limite est définie par les deux anneaux extérieurs et les trois anneaux intérieurs. Les deux éléments **Polygon** se croisent à un point tangent.  
  
### <a name="accepted-instances"></a>Instances acceptées  
 Les instances **MultiPolygon** sont acceptées si l’une des conditions suivantes est remplie.  
  
-   Il s’agit d’une instance **MultiPolygon** vide.  
  
-   Toutes les instances comprenant l’instance **MultiPolygon** sont des instances **Polygon** acceptées. Pour plus d’informations sur les instances **Polygon** acceptées, consultez [Polygon](../../relational-databases/spatial/polygon.md).  
  
Les exemples suivants illustrent des instances **MultiPolygon** acceptées.  
  
```sql  
DECLARE @g1 geometry = 'MULTIPOLYGON EMPTY';  
DECLARE @g2 geometry = 'MULTIPOLYGON(((1 1, 1 -1, -1 -1, -1 1, 1 1)),((1 1, 3 1, 3 3, 1 3, 1 1)))';  
DECLARE @g3 geometry = 'MULTIPOLYGON(((2 2, 2 -2, -2 -2, -2 2, 2 2)),((1 1, 3 1, 3 3, 1 3, 1 1)))';  
```  
  
L'exemple suivant illustre une instance MultiPolygon qui lève une exception `System.FormatException`.  
  
```sql  
DECLARE @g geometry = 'MULTIPOLYGON(((1 1, 1 -1, -1 -1, -1 1, 1 1)),((1 1, 3 1, 3 3)))';  
```  
  
La deuxième instance du MultiPolygon est une instance LineString et non une instance Polygon acceptée.  
  
### <a name="valid-instances"></a>Instances valides  
 Les instances **MultiPolygon** sont valides s’il s’agit d’instances **MultiPolygon** vides ou si elles respectent les critères suivants.  
  
1.  Toutes les instances comprenant l’instance **MultiPolygon** sont des instances **Polygon** valides. Pour obtenir des instances **Polygon** valides, consultez [Polygon](../../relational-databases/spatial/polygon.md).  
  
2.  Aucune des instances **Polygon** comprenant l’instance **MultiPolygon** n’en chevauche une autre.  

L’exemple suivant illustre deux instances **MultiPolygon** valides et une instance **MultiPolygon** non valide.  
  
```sql  
DECLARE @g1 geometry = 'MULTIPOLYGON EMPTY';  
DECLARE @g2 geometry = 'MULTIPOLYGON(((1 1, 1 -1, -1 -1, -1 1, 1 1)),((1 1, 3 1, 3 3, 1 3, 1 1)))';  
DECLARE @g3 geometry = 'MULTIPOLYGON(((2 2, 2 -2, -2 -2, -2 2, 2 2)),((1 1, 3 1, 3 3, 1 3, 1 1)))';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid();  
```  
  
`@g2` est valide, car les deux instances **Polygon** se touchent uniquement à un point tangent. `@g3` n’est pas valide, car les intérieurs des deux instances **Polygon** se chevauchent.  
  
## <a name="examples"></a>Exemples  
### <a name="example-a"></a>Exemple A.
L’exemple suivant illustre la création d’une instance `geometry``MultiPolygon` et retourne l’entrée WKT (well-known text) du deuxième composant.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('MULTIPOLYGON(((0 0, 0 3, 3 3, 3 0, 0 0), (1 1, 1 2, 2 1, 1 1)), ((9 9, 9 10, 10 9, 9 9)))');  
SELECT @g.STGeometryN(2).STAsText();  
```  
  
## <a name="example-b"></a>Exemple B.
Cet exemple instancie une instance `MultiPolygon` vide.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('MULTIPOLYGON EMPTY');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Polygon](../../relational-databases/spatial/polygon.md)   
 [STArea &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/starea-geometry-data-type.md)   
 [STCentroid &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stcentroid-geometry-data-type.md)   
 [STPointOnSurface &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)   
 [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
