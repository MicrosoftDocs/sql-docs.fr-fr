---
description: IsTrainingCase (DMX)
title: IsTrainingCase (DMX) | Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 5eb68d0aaa0d19fb903154b8d5c4d4135b57883e
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726144"
---
# <a name="istrainingcase-dmx"></a>IsTrainingCase (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Indique si un cas est utilisé comme cas d'apprentissage pour le modèle d'exploration de données ou la structure d'exploration de données spécifiés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
IsTrainingCase()  
```  
  
## <a name="result-type"></a>Type de résultat  
 Retourne la **valeur true** si le cas fait partie du jeu de données d’apprentissage ; Sinon, **false**.  
  
## <a name="remarks"></a>Notes  
 Si vous utilisez l'Assistant Exploration de données pour créer une structure d'exploration de données et un modèle d'exploration de données connexe, 30 % des cas sont, par défaut, réservés pour une utilisation en tant que jeu de données de test. Les cas restants de la source de données que vous spécifiez sont utilisés pour l'apprentissage du modèle. Toutefois, si vous utilisez DMX (Data Mining Extensions) pour créer le modèle d'exploration de données, toutes les données sont, par défaut, utilisées pour l'apprentissage du modèle, et aucun jeu de test n'est créé. Pour permettre la création d'un jeu de données de test, vous devez définir les paramètres de la clause WITH HOLDOUT.  
  
 Vous pouvez déterminer si les données d'une structure d'exploration de données particulière ont été partitionnées en jeux de test et d'apprentissage en consultant la valeur des propriétés <xref:Microsoft.AnalysisServices.MiningStructure.HoldoutMaxCases%2A> et <xref:Microsoft.AnalysisServices.MiningStructure.HoldoutMaxPercent%2A>.  
  
> [!NOTE]  
>  L’extraction doit être activée sur le modèle si vous souhaitez utiliser les fonctions IsTrainingCase ou IsTestCase pour retourner des détails sur les cas dans le modèle. Pour plus d’informations, consultez [Activer l’extraction pour un modèle d’exploration de données](/analysis-services/data-mining/enable-drillthrough-for-a-mining-model).  
  
 Pour retourner les cas qui font partie du jeu de données de test, utilisez la fonction [IsTestCase &#40;&#41;DMX ](../dmx/istestcase-dmx.md).  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise le modèle d’exploration de données de clustering du scénario de publipostage ciblé dans le didacticiel sur l' [exploration de données de base](/previous-versions/sql/sql-server-2016/ms167167(v=sql.130)). La requête retourne uniquement les cas qui sont utilisés pour l'apprentissage du modèle d'exploration de données. De plus, les cas d'apprentissage sont limités aux clients âgés de moins de 40 ans.  
  
```  
SELECT *  
FROM [TM Clustering].CASES  
WHERE IsTrainingCase()  
AND [Age] <40  
```  
  
 Pour obtenir d’autres exemples d’interrogation des cas utilisés dans l’exploration de données, consultez [SELECT FROM &#60;model&#62;. CAS &#40;&#41;DMX ](../dmx/select-from-model-cases-dmx.md) et [sélectionner des&#62; de la structure de &#60;. CAS](../dmx/select-from-structure-cases.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Jeux de données d’apprentissage et de test](/analysis-services/data-mining/training-and-testing-data-sets)   
 [Fonctions &#40;&#41;DMX ](../dmx/functions-dmx.md)   
 [Requêtes d’exploration de données](/analysis-services/data-mining/data-mining-queries)  
  
