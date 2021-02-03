---
description: STCurveN (type de données geography)
title: STCurveN (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STCurveN
- STCurveN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveN method (geography)
ms.assetid: 99ef7100-2c4b-4f07-8d66-b343da94b023
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: fdb5dc0e5bae929b931c219937d89ea2afb9b828
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207527"
---
# <a name="stcurven-geography-data-type"></a>STCurveN (type de données geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne la courbe spécifiée à partir d’une instance **geography** qui est **LineString**, **CircularString** ou **CompoundCurve**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.STCurveN( n )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *n*  
 Expression **int** comprise entre 1 et le nombre de courbes de l’instance **geography**.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
## <a name="exceptions"></a>Exceptions  
 Si n < 1, **ArgumentOutOfRangeException** est levé.  
  
## <a name="remarks"></a>Remarques  
 **NULL** est retourné en présence des critères suivants.  
  
-   L’instance **geography** est déclarée, mais n’est pas instanciée  
  
-   L’instance **geography** est vide  
  
-   n dépasse le nombre de courbes dans l’instance **geography** (consultez [STNumCurves &#40;type de données geography&#41;](../../t-sql/spatial-geography/stnumcurves-geography-data-type.md)  
  
-   La dimension de l’instance **geography** n’est pas la même (consultez [STDimension &#40;type de données geography&#41;](../../t-sql/spatial-geography/stdimension-geography-data-type.md)  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-stcurven-on-a-circularstring"></a>R. Utilisation de STCurveN() sur un CircularString  
 L’exemple suivant retourne la deuxième courbe d’une instance **CircularString** :  
  
```
 DECLARE @g geography = 'CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 L'exemple retourne.  
  
 `CIRCULARSTRING (-122.348 47.658, -122.358 47.658, -122.358 47.653)`  
  
### <a name="b-using-stcurven-on-a-compoundcurve"></a>B. Utilisation de STCurveN() sur un CompoundCurve  
 L’exemple suivant retourne la deuxième courbe d’une instance **CompoundCurve** :  
  
```
 DECLARE @g geography = 'COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 L'exemple retourne.  
  
 `CIRCULARSTRING (-122.348 47.658, -122.358 47.658, -122.358 47.653)`  
  
### <a name="c-using-stcurven-on-a-compoundcurve-containing-three-circularstrings"></a>C. Utilisation de STCurveN() sur un CompoundCurve qui contient trois CircularStrings  
 L’exemple suivant utilise une instance **CompoundCurve** qui combine trois instances **CircularString** distinctes dans la même séquence de courbes que l’exemple précédent :  
  
```
 DECLARE @g geography = 'COMPOUNDCURVE (CIRCULARSTRING (-122.358 47.653, -122.348 47.649, -122.348 47.658), CIRCULARSTRING(-122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 L'exemple retourne.  
  
 `CIRCULARSTRING (-122.348 47.658, -122.358 47.658, -122.358 47.653)`  
  
 `STCurveN()` retourne les mêmes résultats indépendamment du format de texte WKT utilisé.  
  
### <a name="d-testing-for-validity-before-calling-stcurve"></a>D. Test de validité avant d'appeler STCurve()  
 L’exemple suivant montre comment vérifier que *n* est valide avant d’appeler la méthode STCurveN() :  
  
```
 DECLARE @g geography;  
 DECLARE @n int;  
 SET @n = 2;  
 SET @g = geography::Parse('LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)');  
 IF @n >= 1 AND @n <= @g.STNumCurves()  
 BEGIN  
 SELECT @g.STCurveN(@n).ToString();  
 END
  ```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes OGC sur des instances Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
