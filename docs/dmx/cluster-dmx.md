---
title: Cluster (DMX) | Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 0cd7b2e0a78f2d47349de2701572b2f9dc4b0095
ms.sourcegitcommit: 205de8fa4845c491914902432791bddf11002945
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86969920"
---
# <a name="cluster-dmx"></a>Cluster (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Retourne le cluster qui est le plus susceptible de contenir le cas d'entrée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Cluster()  
```  
  
## <a name="applies-to"></a>S'applique à  
 Cette fonction ne peut être utilisée que si le modèle d'exploration de données sous-jacent prend en charge le clustering.  
  
## <a name="return-type"></a>Type de retour  
 La fonction de **cluster** ne requiert pas de paramètres.  
  
 La fonction **cluster** retourne une valeur scalaire d’un nom de cluster. Toutefois, si vous utilisez cette fonction en tant qu’argument d’une autre fonction, vous devez la considérer comme un \<cluster column reference> .  
  
## <a name="remarks"></a>Notes  
 Le **cluster** peut également être utilisé en tant que `<` référence `>` de colonne de cluster pour une fonction **PredictHistogram** .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise une requête singleton avec les fonctions [&#40;DMX&#41;](../dmx/predicthistogram-dmx.md) et cluster pour retourner la distance du cas individuel de chaque cluster du modèle d’exploration de données de clustering TM et la probabilité que le cas individuel existe dans chaque cluster.  
  
```  
SELECT  
  PredictHistogram(Cluster())  
FROM  
  [TM Clustering]  
  NATURAL PREDICTION JOIN  
(SELECT 28 AS [Age],  
  '2-5 Miles' AS [Commute Distance],  
  'Graduate Degree' AS [Education],  
  0 AS [Number Cars Owned],  
  0 AS [Number Children At Home]) AS t  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ClusterProbability&#41;DMX &#40;](../dmx/clusterprobability-dmx.md)   
 [Informations de référence sur les fonctions DMX&#41; Data Mining Extensions &#40;](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Fonctions &#40;&#41;DMX](../dmx/functions-dmx.md)   
 [Fonctions de prédiction générales &#40;&#41;DMX](../dmx/general-prediction-functions-dmx.md)  
  
  
