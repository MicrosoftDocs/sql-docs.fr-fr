---
description: STGeomCollFromWKB (type de données geometry)
title: STGeomCollFromWKB (type de données geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STGeomCollFromWKB (geometry Data Type)
- STGeomCollFromWKB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STGeomCollFromWKB (geometry Data Type)
ms.assetid: 6c55032c-7f5e-4181-8e67-c0265032db63
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: f780e521ba80822eac4de95e91b8495521e07f60
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88454313"
---
# <a name="stgeomcollfromwkb-geometry-data-type"></a>STGeomCollFromWKB (type de données geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne une instance **geometrycollection** à partir d’une représentation OGC (Open Geospatial Consortium) WKB (Well-Known Binary).
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
STGeomCollFromWKB ( 'WKB_geometrycollection' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *WKB_geometrycollection*  
 Représentation WKB de l’instance **geometrycollection** à retourner. *WKB_geometrycollection* est une expression **varbinary(max)**.  
  
 *SRID*  
 Expression **int** qui représente le SRID (ID de référence spatiale) de l’instance **geometry** à retourner.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="remarks"></a>Remarques  
 Le type OGC de l’instance **geometry** retournée par `STGeomCollFromWKB()` a la valeur **GeomCollection**, **MultiPolygon**, **MultiLineString** ou **MultiPoint**, en fonction de l’entrée WKB correspondante.  
  
 Cette méthode lève une exception FormatException si l'entrée n'est pas correctement mise en forme.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise `STGeomCollFromWKB()` pour créer une instance **geometry**.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomCollFromWKB(0x0107000000020000000103000000010000000400000000000000000014400000000000001440000000000000244000000000000014400000000000002440000000000000244000000000000014400000000000001440010100000000000000000024400000000000002440, 0);  
SELECT @g.STAsText();  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Méthodes géométriques statiques OGC](../../t-sql/spatial-geometry/ogc-static-geometry-methods.md)  
  
