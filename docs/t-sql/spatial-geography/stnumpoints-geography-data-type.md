---
description: STNumPoints (type de données geography)
title: STNumPoints (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STNumPoints (geography Data Type)
- STNumPoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STNumPoints method
ms.assetid: 25ff7ad1-ba5f-4cfb-816a-59255ac1591d
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 220e2e9149c3e153080fd044996d01a08c22db89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206503"
---
# <a name="stnumpoints-geography-data-type"></a>STNumPoints (type de données geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne le nombre total de points de chacune des figures d’une instance **geography**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STNumPoints ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **int**  
  
 Type de retour CLR : **SqlInt32**  
  
## <a name="remarks"></a>Remarques  
 Cette méthode compte les points dans la description d’une instance **geography**. Les points en double sont comptés ; toutefois, les points de connexion entre les segments ne sont comptés qu'une seule fois. Si cette instance est une collection, cette méthode retourne le nombre total de points dans la collection.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-retrieving-the-total-number-of-points-in-a-linestring"></a>R. Récupération du nombre total de points dans un LineString  
 L'exemple suivant crée une instance `LineString` et utilise `STNumPoints()` pour déterminer combien de points ont été utilisés dans la description de l'instance.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STNumPoints();  
```  
  
### <a name="b-retrieving-the-total-number-of-points-in-a-geometrycollection"></a>B. Récupération du nombre total de points dans un GeometryCollection  
 L'exemple suivant retourne une somme des points pour tous les éléments dans `GeometryCollection`.  
  
```  
DECLARE @g geography = 'GEOMETRYCOLLECTION(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)  
    ,CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)))';  
SELECT @g.STNumPoints();  
```  
  
### <a name="c-returning-the-number-of-points-in-a-compoundcurve"></a>C. Retour du nombre de points dans un CompoundCurve  
 L'exemple suivant retourne le nombre de points dans une instance CompoundCurve. La requête retourne 5 au lieu de 6 car STNumPoints() compte le point de connexion entre les segments une fois seulement.  
  
```
 DECLARE @g geography = 'COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658),( -122.348 47.658, -121.56 48.12, -122.358 47.653))'  
 SELECT @g.STNumPoints();
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes OGC sur des instances Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
