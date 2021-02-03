---
description: Point (type de données geography)
title: Point (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 10/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Point
- Point_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Point method
- Point (geography Data Type)
ms.assetid: 0dc6f422-7aae-4016-b7f4-3289fa8f989c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 207b4c8fb9c9d171e7caad003e9b398c97dc4152
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210147"
---
# <a name="point-geography-data-type"></a>Point (type de données geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Construit une instance **geography** qui représente une instance **Point** à partir de ses valeurs de latitude et de longitude, et d’un SRID (ID de référence spatiale).
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Point ( Lat, Long, SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *Lat*  
 Expression **float** qui représente la coordonnée y du **Point** généré.  
  
 *Long*  
 Expression **float** qui représente la coordonnée x du **Point** généré. Pour plus d’informations sur les valeurs de latitude et de longitude valides, consultez [Point](../../relational-databases/spatial/point.md).  
  
 *SRID*  
 Expression **int** qui représente le [SRID (spatial reference identifier)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md) de l’instance **géographique** à retourner.  
  
> [!NOTE]  
>  Les arguments de la méthode Point (geography Data Type) ont des coordonnées inversées, comparées à WKT.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise la méthode `Point()` pour créer une instance `geography`.  
  
```  
DECLARE @g geography;   
SET @g = geography::Point(47.65100, -122.34900, 4326)  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Méthodes geography statiques étendues](../../t-sql/spatial-geography/extended-static-geography-methods.md)