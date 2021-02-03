---
description: BufferWithTolerance (type de données geography)
title: BufferWithTolerance (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- BefferWithTolerance method
ms.assetid: f1783e6b-0f17-464f-b1c7-1c3f7d8aa042
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 51be64effcbbb75abe7830949d8bdf83b5637307
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205047"
---
# <a name="bufferwithtolerance-geography-data-type"></a>BufferWithTolerance (type de données geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne un objet géométrique qui représente l’union de toutes les valeurs de points dont la distance par rapport à une instance **geography** est inférieure ou égale à une valeur spécifique, en tenant compte d’une tolérance spécifique.  
  
Cette méthode de type de données geography prend en charge les instances **FullGlobe** ou les instances spatiales qui sont plus grandes qu’un hémisphère.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.BufferWithTolerance ( distance, tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
_distance_  
Expression **float** spécifiant la distance de l’instance **geography** autour de laquelle calculer la mémoire tampon.  
  
La distance maximale du tampon ne peut pas dépasser 0,999 \* _π_ * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 de la circonférence de la Terre) ou le globe complet.  
  
_tolerance_  
Expression **float** spécifiant la tolérance de la distance de mémoire tampon.  
  
La variation maximale dans la distance de mémoire tampon idéale pour l’approximation linéaire retournée est la valeur de _tolérance_.  
  
Par exemple, la distance de mémoire tampon idéale d'un point est un cercle, mais elle doit être exprimée de façon à se rapprocher d'un polygone. Plus la tolérance est faible, plus le polygone a de points. Ceci augmente la complexité du résultat, mais réduit l’erreur au minimum.  
  
La limite minimum est de 0,1 pour cent de la distance, plus toute tolérance inférieure à la valeur qui sera arrondie à la limite minimale.  
  
_relative_  
**bit** spécifiant si la valeur de _tolerance_ est relative ou absolue. Si la valeur est « TRUE » ou 1, la tolérance est relative. Cette valeur est le produit du paramètre _tolérance_ et du rayon équatorial de l'ellipsoïde de l'étendue angulaire \*. La tolérance est absolue si la valeur est « FALSE » ou 0. Cette valeur _tolerance_ est la variation maximale dans la distance de mémoire tampon idéale pour l’approximation linéaire retournée.  
  
## <a name="return-types"></a>Types de retour  
Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
Type de retour CLR : **SqlGeography**  
  
## <a name="remarks"></a>Notes  
Cette méthode lève **ArgumentException** si _distance_ est une valeur NAN (n’est pas un nombre), ou si _distance_ est un infini positif ou négatif.  Cette méthode lève également **ArgumentException** si _tolerance_ est égal à zéro (0), est une valeur NAN (n’est pas un nombre), est une valeur négative, ou est un infini positif ou négatif.  
  
`STBuffer()` retourne une instance **FullGlobe** dans certains cas ; par exemple `STBuffer()` retourne une instance **FullGlobe** sur deux pôles quand la distance de mémoire tampon est supérieure à la distance entre l’équateur et les pôles.  
  
Cette méthode lève **ArgumentException** dans les instances **FullGlobe** où la distance de mémoire tampon dépasse la limite suivante :  
  
0,999 \* _π_ * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 de la circonférence de la Terre)  
  
L’erreur entre la mémoire tampon théorique et la mémoire tampon calculée correspond à max(tolérance, étendues \* 1E-7), où tolérance représente la valeur du paramètre _tolerance_. Pour plus d’informations sur les étendues, consultez [Référence de méthodes de type de données geography](./stequals-geography-data-type.md).  
  
Cette méthode n'est pas précise.  
  
## <a name="examples"></a>Exemples  
L'exemple suivant crée une instance `Point` et utilise `BufferWithTolerance()` pour obtenir une estimation grossière de la mémoire tampon autour d'elle.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POINT(-122.34900 47.65100)', 4326);  
SELECT @g.BufferWithTolerance(1, .5, 0).ToString();  
```  
  
## <a name="see-also"></a>Voir aussi  
[STBuffer &#40;type de données geography&#41;](../../t-sql/spatial-geography/stbuffer-geography-data-type.md)   
[Méthodes étendues sur des instances geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
