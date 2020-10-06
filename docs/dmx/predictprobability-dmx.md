---
description: PredictProbability (DMX)
title: PredictProbability (DMX) | Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: c7bbdc73d217de895b84ae8d6aeadf7adbfe1796
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91727689"
---
# <a name="predictprobability-dmx"></a>PredictProbability (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Retourne la probabilité pour un état spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
PredictProbability(<scalar column reference>, [<predicted state>])  
```  
  
## <a name="applies-to"></a>S'applique à  
 Colonne scalaire  
  
## <a name="return-type"></a>Type de retour  
 Valeur scalaire.  
  
## <a name="remarks"></a>Notes  
 Si l'état prédit est omis, l'état ayant la probabilité la plus élevée est utilisé, à l'exception du compartiment des états manquants. Pour inclure le compartiment des États manquants, affectez à la valeur \<predicted state> **INCLUDE_NULL**. Pour retourner la probabilité pour les États manquants, affectez la valeur \<predicted state> null.  
  
> [!NOTE]  
>  Certains modèles d'exploration de données ne fournissent pas de valeurs de probabilité et ne peuvent donc pas utiliser cette fonction. De plus, les valeurs de probabilité des valeurs cibles spécifiques sont calculées différemment ou peuvent avoir une interprétation différente selon le type de modèle que vous interrogez. Pour plus d’informations sur la façon dont la probabilité est calculée pour un type de modèle particulier, consultez la rubrique relative à l’algorithme dans [Mining Model Content &#40;Analysis Services-Data Mining&#41;](/analysis-services/data-mining/mining-model-content-analysis-services-data-mining).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise une prédiction de jointure naturelle pour déterminer si un individu peut se révéler un acheteur potentiel de bicyclette selon le modèle d'exploration de données Arbre de décision TM et pour définir la probabilité de cette prévision. Cet exemple inclut deux fonctions PredictProbability, à raison d'une pour chaque valeur possible. Si vous omettez cet argument, la fonction retourne la probabilité pour la valeur la plus probable.  
  
```  
SELECT  
  [Bike Buyer],  
  PredictProbability([Bike Buyer], 1) AS [Bike Buyer = Yes],  
  PredictProbability([Bike Buyer], 0) AS [Bike Buyer = No]  
FROM [TM Decision Tree]  
NATURAL PREDICTION JOIN  
(SELECT 28 AS [Age],  
  '2-5 Miles' AS [Commute Distance],  
  'Graduate Degree' AS [Education],  
  0 AS [Number Cars Owned],  
  0 AS [Number Children At Home]) AS t  
```  
  
 Résultats de l'exemple :  
  
|Bike Buyer|Bike Buyer = Yes|Bike Buyer = No|  
|----------------|-----------------------|----------------------|  
|1|0.867074195848097|0.132755556974282|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur les fonctions DMX&#41; Data Mining Extensions &#40;](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Fonctions &#40;&#41;DMX ](../dmx/functions-dmx.md)   
 [Fonctions de prédiction générales &#40;&#41;DMX ](../dmx/general-prediction-functions-dmx.md)  
  
