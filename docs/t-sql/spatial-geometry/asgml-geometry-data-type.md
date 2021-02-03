---
description: AsGml (type de données geometry)
title: AsGml (type de données geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AsGml(geometry_Data_Type)_TSQL
- AsGml
- AsGml(geometry Data Type)
- AsGml_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- AsGml (geometry Data Type)
ms.assetid: f6c2e130-05f3-4ef3-921b-d78b51437d48
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: c932617b3f2bea916105242e7195c5302d9ca130
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208718"
---
# <a name="asgml-geometry-data-type"></a>AsGml (type de données geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne la représentation GML (Geography Markup Language) d’une instance **geometry**.
  
Pour plus d’informations sur le langage GML, consultez la spécification OGC (Open Geospatial Consortium) suivante : [OGC Specifications, Geography Markup Language.](https://go.microsoft.com/fwlink/?LinkId=93629)
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.AsGml ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **xml**  
  
 Type de retour CLR : **SqlXml**  
  
## <a name="remarks"></a>Notes  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une instance `LineString` et utilise `AsGML()` pour retourner la description GML de l'instance.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 0 1, 1 0)', 0)  
SELECT @g.AsGml();  
```  
  
 Cette méthode retourne la description en tant qu'instance `LineString`.  
  
```  
<LineString xmlns="https://www.opengis.net/gml">  
<posList>0 0 0 1 1 0</posList></LineString>  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes étendues sur les instances géométriques](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
  

