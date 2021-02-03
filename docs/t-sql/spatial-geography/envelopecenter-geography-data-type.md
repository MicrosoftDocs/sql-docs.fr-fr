---
description: EnvelopeCenter (type de données geography)
title: EnvelopeCenter (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EnvelopeCenter
- EnvelopeCenter_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- EnvelopeCenter method
ms.assetid: dee9d807-faad-45b8-b3f3-7e8aa7d07147
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 59428c1d87ab1a4c855e4890a113f96318c8c164
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211426"
---
# <a name="envelopecenter-geography-data-type"></a>EnvelopeCenter (type de données geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne un point que vous pouvez utiliser comme centre du cercle englobant pour l’instance **geography**.  
  
Chaque point dans l’instance est décrit comme un vecteur. Pour déterminer le cercle englobant, le vecteur s’étend à partir du centre de la terre au point sur la surface de la terre. Le point central du cercle englobant est calculé en faisant la moyenne de tous les vecteurs. Pour les boucles fermées, que ce soit dans une instance **Polygon** ou une instance **LineString**, le premier et dernier point est utilisé une seule fois.  
  
Cette méthode de type de données **geography** prend en charge les instances **FullGlobe** ou les instances spatiales qui sont plus grandes qu’un hémisphère.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
EnvelopeCenter( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
Type de retour CLR : **SqlGeography**  
  
## <a name="remarks"></a>Notes  
Cette méthode retourne un **point**. En cas d’utilisation avec `EnvelopeAngle()`, `EnvelopeCenter()` retourne un cercle englobant d’une instance **geography**.  
  
> [!NOTE]  
>  `EnvelopeCenter()` retourne un cercle englobant pour une instance **geography**, mais il n’est pas garanti que les résultats produisent le cercle englobant minimal. En revanche, la méthode de type de données **geometry**`STEnvelope()` retourne de manière certaine le rectangle englobant minimal quand elle est appliquée à une instance **geometry**.  
  
Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et les versions ultérieures, retourne le centre du cercle qui représente l’enveloppe de cette instance en tant que **point**. Pour tous les objets volumineux, tels qu'ils sont définis par `EnvelopeAngle()` = 180, `EnvelopeCenter()` retourne (90,0).  
  
Cette méthode n'est pas précise.  
  
## <a name="examples"></a>Exemples  
  
```  
DECLARE @g geography = 'LINESTRING(-120 45, -120 0, -90 0)';  
SELECT @g.EnvelopeCenter().ToString();  
```  
  
## <a name="see-also"></a>Voir aussi  
[Méthodes étendues sur des instances Geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
[EnvelopeAngle &#40;type de données geography&#41;](../../t-sql/spatial-geography/envelopeangle-geography-data-type.md)  
  
  
