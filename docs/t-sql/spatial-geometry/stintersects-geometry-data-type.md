---
description: STIntersects (type de données geometry)
title: STIntersects (type de données geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STIntersects (geometry Data Type)
- STIntersects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STIntersects (geometry Data Type)
ms.assetid: 7c18f5be-5a29-422e-8ca7-d8a5f38e03f5
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: af349e3d79ea8b35854e5c1654d794511b082329
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88445073"
---
# <a name="stintersects-geometry-data-type"></a>STIntersects (type de données geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne 1 si une instance **geometry** entre en intersection avec une autre instance **geometry**. Retourne 0 dans le cas contraire.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STIntersects ( other_geometry )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *other_geometry*  
 Autre instance **geometry** à comparer à l’instance sur laquelle `STIntersects()` est appelé.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **bit**  
  
 Type de retour CLR : **SqlBoolean**  
  
## <a name="remarks"></a>Remarques  
 Cette méthode retourne toujours une valeur Null si les SRID (ID de référence spatiale) des instances **geometry** ne correspondent pas.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise `STIntersects()` pour déterminer si deux instances `geometry` entrent en intersection.  
  
```  
DECLARE @g geometry;  
DECLARE @h geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0, 4 2)', 0);  
SET @h = geometry::STGeomFromText('POINT(1 1)', 0);  
SELECT @g.STIntersects(@h);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Méthodes OGC sur des instances geography](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

