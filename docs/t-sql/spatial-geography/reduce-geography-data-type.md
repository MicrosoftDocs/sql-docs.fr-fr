---
description: Reduce (type de données geography)
title: Reduce (type de données geography)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Reduce_TSQL
- Reduce
dev_langs:
- TSQL
helpviewer_keywords:
- Reduce method
ms.assetid: c5dfa8c1-6764-41d8-9150-f3cb30633d3e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 6667a6f3681a3c3c058673cbcac997c2b91c33c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208735"
---
# <a name="reduce-geography-data-type-"></a>Reduce (type de données geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une approximation de l’instance **geography** donnée en exécutant l’algorithme de Douglas-Peucker sur l’instance, avec la tolérance donnée.  
  
 Cette méthode de type de données **geography** prend en charge les instances **FullGlobe** ou les instances spatiales qui sont plus grandes qu’un hémisphère.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.Reduce ( tolerance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

|Terme|Définition|
|----|----------|
|*tolerance*|Valeur de type **float**. *tolerance* est la tolérance à entrer pour l’algorithme Douglas-Peucker. *tolerance* doit être un nombre positif.|  
  
## <a name="return-types"></a>Types de retour  
 Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
 Type de retour CLR : **SqlGeography**  
  
## <a name="remarks"></a>Notes  
 Pour les types de collection, cet algorithme fonctionne indépendamment sur chaque **geography** contenu dans l’instance. Cet algorithme ne modifie pas les instances **Point**.  
  
 Cette méthode tente de conserver les points de terminaison des instances **LineString**, mais elle peut ne pas y parvenir si elle a pour but de conserver un résultat valide.  
  
 Si `Reduce()` est appelé avec une valeur négative, cette méthode lève **ArgumentException**. Les tolérances utilisées dans `Reduce()` doivent être des nombres positifs.  
  
 L’algorithme Douglas-Peucker fonctionne sur chaque courbe ou anneau de l’instance **geography** en supprimant tous les points, à l’exception du point de départ et du point de terminaison. Chaque point supprimé est ensuite rajouté, en commençant par le point excentré le plus lointain, jusqu’à ce qu’aucun point ne soit à plus de la valeur *tolerance* du résultat. Le résultat est ensuite rendu valide si nécessaire, dans la mesure où un résultat valide est garanti.  
  
 Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], cette méthode a été étendue aux instances **FullGlobe**.  
  
 Cette méthode n'est pas précise.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une instance `LineString` et utilise `Reduce()` afin de simplifier l'instance.  
  
```  
DECLARE @g geography = 'LineString(120 45, 120.1 45.1, 199.9 45.2, 120 46)'  
SELECT @g.Reduce(10000).ToString()  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes étendues sur des instances geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
