---
description: STPointN (type de données geography)
title: STPointN (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STPointN_TSQL
- STPointN (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STPointN method
ms.assetid: 47670feb-b9e0-4b4b-af83-b9bba7da66ac
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b871061f6e2d90471ee006184fed2bf438fb8fa2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190543"
---
# <a name="stpointn-geography-data-type"></a>STPointN (type de données geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne le point spécifié dans une instance **geography**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STPointN ( expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression*  
 Expression **int** comprise entre 1 et le nombre de points de l’instance **geography**.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
 Type OGC (Open Geospatial Consortium) : **Point**  
  
## <a name="remarks"></a>Remarques  
 Si une instance **geography** est créée par l’utilisateur, STPointN() retourne le point spécifié par *expression* en classant les points dans l’ordre dans lequel ils ont été entrés à l’origine.  
  
 Si une instance **geography** est construite par le système, STPointN() retourne le point spécifié par *expression* en classant tous les points dans le même ordre que celui de leur sortie : d’abord par instance **geography**, puis par anneau dans l’instance (le cas échéant), puis par point dans l’anneau. Cet ordre est déterministe.  
  
 Si cette méthode est appelée avec une valeur inférieure à 1, elle lève **ArgumentOutOfRangeException**.  
  
 Si cette méthode est appelée avec une valeur supérieure au nombre de points dans l'instance, elle retourne Null.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une instance `LineString` et utilise `STPointN()` pour extraire le deuxième point dans la description de l'instance.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STPointN(2).ToString();  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes OGC sur des instances Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
