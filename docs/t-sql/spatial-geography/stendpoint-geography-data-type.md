---
description: STEndpoint (type de données géographie)
title: STEndpoint (type de données géographie) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STEndPoint (geography Data Type)
- STEndPoint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STEndPoint method
ms.assetid: 8974cd07-8ec4-4126-8fc2-fdcf322ccedd
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 331c20cbde67920b2a2ba25d1d50f59996610626
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183921"
---
# <a name="stendpoint-geography-data-type"></a>STEndpoint (type de données géographie)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne le point de terminaison d’une instance **geography**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STEndPoint ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
 Type OGC (Open Geospatial Consortium) : **Point**  
  
## <a name="remarks"></a>Remarques  
 STEndPoint() équivaut à [STPointN](../../t-sql/spatial-geography/stpointn-geography-data-type.md)`(x.STNumPoints``())`.  
  
 Cette méthode retourne une valeur Null si elle est appelée sur une instance **geography** vide.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une instance `LineString` avec `STGeomFromText()` et utilise `STEndPoint()` pour extraire le point de terminaison du `LineString`.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STEndPoint().ToString();  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes OGC sur les instances geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
