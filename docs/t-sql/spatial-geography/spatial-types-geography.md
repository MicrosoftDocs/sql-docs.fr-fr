---
description: Types spatiaux - geography
title: geography (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- geography
dev_langs:
- TSQL
helpviewer_keywords:
- geography data type [SQL Server], Transact-SQL
- spatial data types [SQL Server]
ms.assetid: d9e4952a-1841-4465-a64b-11e9288dba1d
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: ec79674754644805404f1cebf144a9c4ec376a07
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210131"
---
# <a name="spatial-types---geography"></a>Types spatiaux - geography
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Le type de données spatiales géographique, **geography**, est implémenté en tant que type de données CLR (Common Language Runtime) .NET dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ce type représente des données dans un système de coordonnées de monde sphérique. Le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **geography** stocke des données ellipsoïdes, telles que des coordonnées de latitude et de longitude GPS.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge un ensemble de méthodes pour le type de données spatiales **geography**. Cela inclut des méthodes sur **geography**, définies par la norme OGC (Open Geospatial Consortium), et un ensemble d’extensions [!INCLUDE[msCoName](../../includes/msconame-md.md)] de cette norme.  
 
 La tolérance d’erreur des méthodes **geography** peut aller jusqu’à 1e-7 * étendues. Les étendues font référence à la distance maximale approximative entre les points de l’objet **geography**.
  

## <a name="registering-the-geography-type"></a>Inscription du type geography  
 Le type **geography** est prédéfini et disponible dans chaque base de données. Vous pouvez créer des colonnes de table de type **geography** et opérer sur les données **geography** comme vous le feriez avec d’autres types fournis par le système. Peut être utilisé dans les colonnes calculées persistantes et non persistantes.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-showing-how-to-add-and-query-geography-data"></a>R. Illustration de l'ajout et de l'interrogation de données géographiques  
 Les exemples suivants montrent comment ajouter et interroger des données géographiques. Le premier exemple crée une table avec une colonne d’identité et une colonne `geography`, `GeogCol1`. Une troisième colonne restitue la colonne `geography` dans sa représentation OGC (Open Geospatial Consortium) WKT (Well-Known Text) et utilise la méthode `STAsText()` . Deux lignes sont ensuite insérées : une ligne contient une instance `LineString` de `geography`et une ligne contient une instance `Polygon` .  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
    DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable   
    ( id int IDENTITY (1,1),  
    GeogCol1 geography,   
    GeogCol2 AS GeogCol1.STAsText() );  
GO  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656 )', 4326));  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('POLYGON((-122.358 47.653 , -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326));  
GO  
```  
  
### <a name="b-returning-the-intersection-of-two-geography-instances"></a>B. Retour de l'intersection de deux instances géographiques  
 L'exemple suivant utilise la méthode `STIntersection()` pour retourner les points où les deux instances `geography` précédemment insérées se croisent.  
  
```sql  
DECLARE @geog1 geography;  
DECLARE @geog2 geography;  
DECLARE @result geography;  
  
SELECT @geog1 = GeogCol1 FROM SpatialTable WHERE id = 1;  
SELECT @geog2 = GeogCol1 FROM SpatialTable WHERE id = 2;  
SELECT @result = @geog1.STIntersection(@geog2);  
SELECT @result.STAsText();  
```  
  
### <a name="c-using-geography-in-a-computed-column"></a>C. Utilisation du type géographique dans une colonne calculée  
 L’exemple suivant crée une table avec une colonne calculée persistante à l’aide d’un type **geography**.  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
    DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable  
(  
    locationId int IDENTITY(1,1),  
    location geography,  
    deliveryArea as location.STBuffer(10) persisted  
);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)   

  
  
