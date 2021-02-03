---
description: STEquals (type de données geography)
title: STEquals (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STEquals_TSQL
- STEquals (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STEquals method
ms.assetid: 0766ff37-0b9e-49bf-83c0-019f4354fe44
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b05146c00e87e8d57fe9d7d0b642bd0d4c262dc9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207196"
---
# <a name="stequals-geography-data-type"></a>STEquals (type de données geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Retourne 1 si une instance **geography** représente le même ensemble de points qu’une autre instance **geography**. Retourne 0 dans le cas contraire.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STEquals ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *other_geography*  
 Autre instance **geography** à comparer à l’instance sur laquelle `STEquals()` est appelé.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **bit**  
  
 Type de retour CLR : **SqlBoolean**  
  
## <a name="remarks"></a>Remarques  
 Cette méthode retourne toujours une valeur Null si les SRID (ID de référence spatiale) des instances **geography** ne correspondent pas.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée deux instances `geography` avec `STGeomFromText()` qui sont égales mais pas de façon triviale, et utilise `STEquals()` pour tester leur égalité. Les instances sont égales car `LINESTRING` et `POINT` sont contenus dans le `POLYGON`.  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('GEOMETRYCOLLECTION(POLYGON((-122.368 47.658, -122.338 47.649, -122.338 47.658, -122.368 47.658, -122.368 47.658)), LINESTRING(-122.360 47.656, -122.343 47.656), POINT (-122.35 47.656))', 4326);  
SET @h = geography::STGeomFromText('POLYGON((-122.368 47.658, -122.338 47.649, -122.338 47.658, -122.368 47.658, -122.368 47.658))', 4326);  
SELECT @g.STEquals(@h);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes OGC sur des instances Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
