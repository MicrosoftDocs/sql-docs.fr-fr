---
description: ShortestLineTo (type de données geometry)
title: ShortestLineTo (type de données geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- ShortestLineTo method (geometry)
ms.assetid: 39a2d0e4-4f93-4e94-a27e-6ad9537cfe74
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 937a631ca32ab7d63dece2603d5cf1357fdd1567
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200406"
---
# <a name="shortestlineto-geometry-data-type"></a>ShortestLineTo (type de données geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Retourne une instance **LineString** avec deux points qui représentent la distance la plus courte entre les deux instances **geometry**. La longueur de l’instance **LineString** retournée correspond à la distance entre les deux instances **geometry**.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.ShortestLineTo ( geometry_other )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *geometry_other*  
 Deuxième instance **geometry** dont l’instance **geometry** appelante tente de déterminer la distance la plus courte.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="remarks"></a>Remarques  
 La méthode retourne une instance **LineString** avec des points de terminaison situés sur les bordures des deux instances **geometry** sans intersection qui sont comparées. La longueur du **LineString** retourné est égale à la distance la plus courte entre les deux instances **geometry**. Une instance **LineString** vide est retournée quand les deux instances **geometry** se croisent.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-calling-shortestlineto-on-non-intersecting-instances"></a>R. Appel de ShortestLineTo() sur des instances qui ne se croisent pas  
 Cet exemple recherche la distance la plus courte entre une instance `CircularString` et une instance `LineString` et retourne l'instance `LineString` qui connecte les deux points :  
  
```
 DECLARE @g1 geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';  
 DECLARE @g2 geometry = 'LINESTRING(-4 7, 7 10, 3 7)';  
 SELECT @g1.ShortestLineTo(@g2).ToString();
 ```  
  
### <a name="b-calling-shortestlineto-on-intersecting-instances"></a>B. Appel de ShortestLineTo() sur des instances qui se croisent  
 Cet exemple retourne une instance `LineString` vide du fait que l'instance `LineString` croise l'instance `CircularString` :  
  
```
 DECLARE @g1 geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';  
 DECLARE @g2 geometry = 'LINESTRING(0 5, 7 10, 3 7)';  
 SELECT @g1.ShortestLineTo(@g2).ToString();
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [ShortestLineTo &#40;type de données geography&#41;](../../t-sql/spatial-geography/shortestlineto-geography-data-type.md)  
  
  

