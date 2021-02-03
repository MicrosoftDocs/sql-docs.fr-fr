---
description: STIsValid (type de données geometry)
title: STIsValid (type de données geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STIsValid (geometry Data Type)
- STIsValid_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STIsValid (geometry Data Type)
ms.assetid: 6da39bea-0f67-4660-98fc-d7214f9b2138
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 13abb6eb4e7c28289760eb7a1440c0ae99cca046
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206206"
---
# <a name="stisvalid-geometry-data-type"></a>STIsValid (type de données geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne true si une instance **geometry** est bien formée, en fonction de son type OGC (Open Geospatial Consortium). Retourne false si une instance **geometry** n’est pas bien formée.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STIsValid ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **bit**  
  
 Type de retour CLR : **SqlBoolean**  
  
## <a name="remarks"></a>Notes  
 Vous pouvez déterminer le type OGC d’une instance **geometry** en appelant [STGeometryType()](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] produit uniquement des instances **geometry** valides, mais permet le stockage et la récupération d’instances non valides. Une instance valide qui représente le même ensemble de points de toute instance non valide peut être extraite à l'aide de la méthode `MakeValid()`.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une instance `geometry` et utilise `STIsValid()` pour tester si l'instance est valide.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);  
SELECT @g.STIsValid();  
```  
  
## <a name="see-also"></a>Voir aussi  
 [STGeometryType &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)   
 [MakeValid &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/makevalid-geometry-data-type.md)   
 [Méthodes OGC sur des instances geography](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

