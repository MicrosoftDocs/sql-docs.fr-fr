---
description: BufferWithTolerance (type de données geometry)
title: BufferWithTolerance (type de données geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BufferWithTolerance_TSQL
- BufferWithTolerance
dev_langs:
- TSQL
helpviewer_keywords:
- BufferWithTolerance (geometry Data Type)
ms.assetid: 7049d37a-3e72-4e93-87a1-c96a6f0e2b99
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b84197d386d92da8593449142ae3e13b54b8a605
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205799"
---
# <a name="bufferwithtolerance-geometry-data-type"></a>BufferWithTolerance (type de données geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne un objet géométrique qui représente l’union de toutes les valeurs de points dont la distance par rapport à une instance **geometry** est inférieure ou égale à une valeur spécifique, en tenant compte d’une tolérance spécifique.
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.BufferWithTolerance ( distance, tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *distance*  
 Expression **float** spécifiant la distance de l’instance **geometry** autour de laquelle calculer la mémoire tampon.  
  
 *tolerance*  
 Expression **float** spécifiant la tolérance de la distance de mémoire tampon.  
  
 *Tolerance* fait référence à la variation maximale dans la distance de mémoire tampon idéale pour l’approximation linéaire retournée.  
  
 Par exemple, la distance de mémoire tampon idéale d'un point est un cercle, mais elle doit être exprimée de façon à se rapprocher d'un polygone. Plus la tolérance est petite, plus le polygone aura de points, ce qui augmente la complexité du résultat mais décroît l'erreur.  
  
 *relative*  
 **bit** spécifiant si la valeur de *tolerance* est relative ou absolue. Si la valeur est « TRUE » ou 1, la tolérance est relative et calculée sous la forme du produit du paramètre *tolerance* et du diamètre du rectangle englobant de l’instance. Si la valeur est « FALSE » ou 0, la tolérance est absolue et la valeur de *tolerance* représente la variation maximale absolue dans la distance de mémoire tampon idéale pour l’approximation linéaire retournée.  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geometry**  
  
 Type de retour CLR : **SqlGeometry**  
  
## <a name="exceptions"></a>Exceptions  
 Le paramètre *tolerance* doit être supérieur à zéro. Si *tolerance* <= 0, `System.ArgumentOutOfRangeException` est levé.  
  
> [!NOTE]  
>  Dans la mesure où *tolerance* est de type **float**, `System.Runtime.InteropServices.COMException` peut être levé si la valeur de tolérance est très petite en raison de problèmes d’arrondi avec les types à virgule flottante.  
  
## <a name="remarks"></a>Remarques  
 Quand *distance* > 0, une instance **Polygon** ou **MultiPolygon** est retournée.  
  
> [!NOTE]  
>  Dans la mesure où distance est de type **float**, une valeur extrêmement petite peut être équivalente à zéro dans les calculs. Quand cela se produit, une copie de l’instance **geometry** appelante est retournée. Consultez [float et real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md).  
  
 Quand *distance* = 0, une copie de l’instance **geometry** appelante est retournée.  
  
 Quand *distance* < 0 alors  
  
-   Une instance **GeometryCollection** vide est retournée quand les dimensions de l’instance sont 0 ou 1.  
  
-   Une mémoire tampon négative est retournée lorsque les dimensions de l'instance sont de 2 ou plus.  
  
    > [!NOTE]  
    >  Une mémoire tampon négative peut également créer une instance **GeometryCollection** vide.  
  
 Une mémoire tampon négative supprime tous les points dans la distance donnée de la limite de l’instance **geometry**.  
  
 L’erreur entre la mémoire tampon théorique et la mémoire tampon calculée correspond à max(tolérance, étendues \* 1E-7), où tolérance représente la valeur du paramètre *tolerance*. Pour plus d’informations sur les étendues, consultez [Référence de méthodes de type de données geometry](./spatial-types-geometry-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une instance `Point` et utilise `BufferWithTolerance()` pour obtenir une estimation grossière de la mémoire tampon autour d'elle.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POINT(3 3)', 0);  
SELECT @g.BufferWithTolerance(1, .5, 0).ToString();  
```  
  
## <a name="see-also"></a>Voir aussi  
 [STBuffer &#40;type de données geometry&#41;](../../t-sql/spatial-geometry/stbuffer-geometry-data-type.md)   
 [Méthodes étendues sur les instances géométriques](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
