---
description: STIntersects (type de données geography)
title: STIntersects (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STIntersects (geography Data Type)
- STIntersects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STIntersects method
ms.assetid: c9db8b42-83c7-48c6-8963-fce54eb34c05
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2377ab8eae271c59ba89694ae97a67be3151e285
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195391"
---
# <a name="stintersects-geography-data-type"></a>STIntersects (type de données geography)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Retourne 1 si une instance **geography** entre en intersection avec une autre instance **geography**. Retourne 0 dans le cas contraire.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
.STIntersects ( other_geography )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]
  
## <a name="arguments"></a>Arguments

*other_geography*  
Autre instance **geography** à comparer à l’instance sur laquelle `STIntersects()` est appelé.  
  
## <a name="return-types"></a>Types de retour

Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **bit**  
  
 Type de retour CLR : **SqlBoolean**  
  
## <a name="remarks"></a>Remarques  
 Cette méthode retourne toujours **NULL** si les SRID (ID de référence spatiale) des instances **geography** ne correspondent pas.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise `STIntersects()` pour déterminer si deux instances `geography` se croisent.  
  
```  
 DECLARE @g geography;  
 DECLARE @h geography;  
 SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
 SET @h = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
```  
  
 ```
 SELECT CASE @g.STIntersects(@h) 
 WHEN 1 THEN '@g intersects @h'  
 ELSE '@g does not intersect @h'  
 END;
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes OGC sur des instances Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
