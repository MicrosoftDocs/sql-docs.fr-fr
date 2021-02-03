---
description: STCurveToLine (type de données geography)
title: STCurveToLine (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STCurveToLine_TSQL
- STCurveToLine
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveToLine method (geography)
ms.assetid: 2f863a85-6168-465a-b32f-bb5e3de58dee
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d73859f9036fc22935e5cb59632d5ba189e417f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204669"
---
# <a name="stcurvetoline-geography-data-type"></a>STCurveToLine (type de données geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une approximation polygonale d’une instance **geography** contenant des segments d’arc de cercle.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STCurveToLine()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
## <a name="remarks"></a>Notes  
 Retourne une instance **LineString** pour une instance **CircularString** ou **CompoundCurve**.  
  
 Retourne une instance **Polygon** pour une instance **CurvePolygon**.  
  
 Retourne une copie des instances **geography** qui ne contiennent pas d’instances **CircularString**, **CompoundCurve** ou **CurvePolygon**.  
  
 Contrairement à la spécification SQL MM, cette méthode n’utilise pas de valeurs de coordonnées z pour calculer l’approximation polygonale. Les valeurs de coordonnées z présentes dans l’instance **geography** appelante sont ignorées.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne une instance `LineString` qui est une approximation polygonale d'une instance `CircularString` :  
  
```
 DECLARE @g1 geography = 'CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 DECLARE @g2 geography;  
 SET @g2 = @g1.STCurveToLine();  
 SELECT @g1.STNumPoints() AS G1, @g2.STNumPoints() AS G2;
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [STLength &#40;type de données geography&#41;](../../t-sql/spatial-geography/stlength-geography-data-type.md)   
 [STNumPoints &#40;type de données geography&#41;](../../t-sql/spatial-geography/stnumpoints-geography-data-type.md)   
 [Présentation des types de données spatiales](../../relational-databases/spatial/spatial-data-types-overview.md)  
  
  
