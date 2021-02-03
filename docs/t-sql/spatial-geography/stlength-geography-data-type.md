---
description: STLength (type de données geography)
title: STLength (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STLength (geography Data Type)
- STLength_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STLength method
ms.assetid: 774560ab-4a4a-4058-b043-1e67cf6fb9eb
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 67ac1b9f1c846f33ede5522e079e5652ec7b6f7a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181205"
---
# <a name="stlength-geography-data-type"></a>STLength (type de données geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Retourne la longueur totale des éléments d’une instance **geography** ou des instances **geography** présentes dans **GeometryCollection**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STLength ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **float**  
  
 Type de retour CLR : **SqlDouble**  
  
## <a name="remarks"></a>Remarques  
 Si une instance **geography** est fermée, sa longueur est calculée en tant que longueur totale autour de l’instance ; la longueur d’un polygone correspond à son périmètre et la longueur d’un point est 0. La longueur de **GeometryCollection** est déterminée en calculant la somme des longueurs de toutes les instances **geography** contenues dans la collection.  
  
 STLength () fonctionne sur LineStrings valide et non valide. Généralement, un LineString n'est pas valide à cause du chevauchement des segments, qui peut être provoqué par des anomalies telles que des traces de longitude GPS inexactes. STLength () ne supprime pas les segments chevauchés ou non valides. Il les inclut dans la valeur de longueur retournée. La méthode MakeValid () peut supprimer les segments chevauchés d'un LineString.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une instance `LineString` et utilise `STLength()` pour déterminer la longueur de l'instance.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STLength();  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes OGC sur des instances Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
