---
description: Point
title: Point | Microsoft Docs
ms.date: 02/02/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- Point geometry subtype [SQL Server]
- geometry data type [SQL Server], spatial data
ms.assetid: 2a596ec4-8b2f-4962-bcb4-e5c8f77edad5
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bcbdda427a587e06bf1ad678939b8164178112dd
ms.sourcegitcommit: 05fc736e6b6b3a08f503ab124c3151f615e6faab
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99478574"
---
# <a name="point"></a>Point
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  Dans les données spatiales [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , un **Point** est un objet à zéro dimension qui représente un emplacement unique et peut contenir des valeurs Z (élévation) et M (mesure).  
  
## <a name="geography-data-type"></a>Type de données geography  
 Le type Point pour le type de données geography représente un emplacement unique où *Lat* et *Long* désignent respectivement la latitude et la longitude. Les valeurs de latitude et de longitude sont mesurées en degrés. Les valeurs de latitude sont toujours comprises dans l’intervalle [-90, 90]. Si vous entrez une valeur non comprise dans cette plage, une exception est levée. Les valeurs de longitude sont toujours comprises dans l’intervalle [-180, 180]. Si vous entrez une valeur non comprise dans cette plage, elle est renvoyée pour y être contenue. Par exemple, si vous entrez la valeur 190 pour la longitude, elle est renvoyée à la valeur -170. Le *SRID* représente l’ID de référence spatiale de l’instance **geography** à retourner.  
  
## <a name="geometry-data-type"></a>Type de données geometry  
 Le type Point pour le type de données geometry représente un emplacement unique où *X* et *Y* désignent respectivement les coordonnées X et Y du point généré. Le *SRID* représente l’ID de référence spatiale de l’instance **geometry** à retourner.  
  
## <a name="examples"></a>Exemples  
### <a name="example-a"></a>Exemple A.
L’exemple suivant crée une instance géométrie Point qui représente le point (3, 4) avec un SRID de 0.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POINT (3 4)', 0);  
```  
  
### <a name="example-b"></a>Exemple B.
L’exemple suivant crée une instance géométrie Point qui représente le point (3, 4) avec une valeur Z (élévation) de 7, une valeur M (mesure) de 2,5 et le SRID par défaut de 0.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::Parse('POINT(3 4 7 2.5)');  
```  
  
### <a name="example-c"></a>Exemple C.
L’exemple suivant retourne les valeurs X, Y, Z et M pour l’instance géométrie Point.  
  
```  
SELECT @g.STX;  
SELECT @g.STY;  
SELECT @g.Z;  
SELECT @g.M;  
```  
  
### <a name="example-d"></a>Exemple D.
Les valeurs Z et M peuvent être spécifiées explicitement comme NULL, comme illustré dans l'exemple suivant.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::Parse('POINT(3 4 NULL NULL)');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [MultiPoint](../../relational-databases/spatial/multipoint.md)   
 [STX &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stx-geometry-data-type.md)   
 [STY &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/sty-geometry-data-type.md)   
 [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
