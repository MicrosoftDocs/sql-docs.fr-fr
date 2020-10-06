---
description: TopPercent (DMX)
title: Pourcentage (DMX) | Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: f1193e43a00fc4c20487a92e8df3a9f705fa4490
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726079"
---
# <a name="toppercent-dmx"></a>TopPercent (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  La fonction Top **percent** retourne, dans l’ordre décroissant, les lignes les plus importantes d’une table dont le total cumulé est au moins un pourcentage spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
TopPercent(<table expression>, <rank expression>, <percent>)  
```  
  
## <a name="applies-to"></a>S'applique à  
 Expression qui retourne une table, telle qu’un \<table column reference> , ou une fonction qui retourne une table.  
  
## <a name="return-type"></a>Type de retour  
 \<table expression>  
  
## <a name="remarks"></a>Notes  
 La fonction Top **percent** retourne les lignes les plus hauts dans l’ordre décroissant de classement en fonction de la valeur évaluée de l' \<rank expression> argument pour chaque ligne, de telle sorte que la somme des \<rank expression> valeurs soit au moins égale au pourcentage donné spécifié par l' \<percent> argument. La propriété de **pourcentage** retourne le plus petit nombre d’éléments possible tout en respectant la valeur de pourcentage spécifiée.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée une requête de prédiction sur le modèle d’association que vous générez à l’aide du didacticiel sur l' [exploration de données de base](/previous-versions/sql/sql-server-2016/ms167167(v=sql.130)).  
  
 Pour comprendre comment fonctionne le pourcentage de fonctionnement, il peut être utile d’exécuter d’abord une requête de prédiction qui retourne uniquement la table imbriquée.  
  
```  
SELECT Predict ([Association].[v Assoc Seq Line Items], INCLUDE_STATISTICS, 10)  
FROM   
     [Association]  
NATURAL PREDICTION JOIN  
SELECT (SELECT 'Women''s Mountain Shorts' as [Model]) AS [v Assoc Seq Line Items]) AS t  
```  
  
> [!NOTE]  
>  Dans cet exemple, la valeur fournie en tant qu'entrée contient un guillemet simple et doit donc être placée dans une séquence d'échappement en la préfaçant avec un autre guillemet simple. Si vous n'êtes pas certain de la syntaxe permettant d'insérer un caractère d'échappement, vous pouvez utiliser le Générateur de requêtes de prédiction pour créer la requête. Lorsque vous sélectionnez la valeur dans la liste déroulante, le caractère d'échappement requis est inséré pour vous. Pour plus d’informations, consultez [créer une requête singleton dans le concepteur d’exploration de données](/analysis-services/data-mining/create-a-singleton-query-in-the-data-mining-designer).  
  
 Résultats de l'exemple :  
  
|Modèle|$SUPPORT|$PROBABILITY|$ADJUSTEDPROBABILITY|  
|-----------|--------------|------------------|--------------------------|  
|Sport-100|4334|0,291283016|0,252695851|  
|Water Bottle|2866|0,192620472|0,175205052|  
|Patch kit|2113|0,142012232|0,132389356|  
|Mountain Tire Tube|1992|0,133879965|0,125304948|  
|Mountain-200|1755|0,117951475|0,111260823|  
|Road Tire Tube|1588|0,106727603|0,101229538|  
|Cycling Cap|1473|0,098998589|0,094256014|  
|Fender Set - Mountain|1415|0,095100477|0,090718432|  
|Mountain Bottle Cage|1367|0,091874454|0,087780332|  
|Road Bottle Cage|1195|0,080314537|0,077173962|  
  
 La fonction Coen-pourcentage prend les résultats de cette requête et retourne les lignes avec les valeurs les plus importantes qui se résument au pourcentage spécifié.  
  
```  
SELECT   
TopPercent  
    (  
    Predict ([Association].[v Assoc Seq Line Items],INCLUDE_STATISTICS,10),  
    $SUPPORT,  
    50)  
FROM   
     [Association]  
NATURAL PREDICTION JOIN  
(SELECT (SELECT 'Women''s Mountain Shorts' as [Model]) AS [v Assoc Seq Line Items]) AS t  
```  
  
 Le premier argument de la fonction de pourcentage est le nom d’une colonne de table. Dans cet exemple, la table imbriquée est retournée en appelant la fonction Predict et en utilisant l’argument INCLUDE_STATISTICS.  
  
 Le deuxième argument de la fonction de pourcentage est la colonne de la table imbriquée que vous utilisez pour classer les résultats. Dans cet exemple, l'option INCLUDE_STATISTICS retourne les colonnes $SUPPORT, $PROBABILTY et $ADJUSTED PROBABILITY. Cet exemple utilise $SUPPORT car les valeurs de support ne sont pas fractionnaires et sont donc plus faciles à vérifier.  
  
 Le troisième argument de la fonction Coen-pourcentage spécifie le pourcentage, en tant que valeur double. Pour obtenir les lignes des produits principaux dont la somme est égale à 50 pour cent du support total, tapez 50.  
  
 Résultats de l'exemple :  
  
|Modèle|$SUPPORT|$PROBABILITY|$ADJUSTEDPROBABILITY|  
|-----------|--------------|------------------|--------------------------|  
|Sport-100|4334|0,29...|0,25...|  
|Water Bottle|2866|0,19...|0,17...|  
|Patch kit|2113|0,14...|0,13...|  
|Mountain Tire Tube|1992|0,133...|0,12...|  
  
 **Remarque** Cet exemple est fourni uniquement pour illustrer l’utilisation de la valeur de « % ». L'exécution de cette requête peut prendre beaucoup de temps, en fonction de la taille de votre jeu de données.  
  
> [!WARNING]  
>  Les fonctions MDX pour TOPPERCENT et BOTTOMPERCENT peuvent générer des résultats inattendus lorsque les valeurs utilisées pour calculer le pourcentage incluent les nombres négatifs. Ce comportement n'affecte pas les fonctions DMX. Pour plus d’informations, consultez [BottomPercent &#40;MDX&#41;](../mdx/bottompercent-mdx.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur les fonctions DMX&#41; Data Mining Extensions &#40;](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Fonctions &#40;&#41;DMX ](../dmx/functions-dmx.md)   
 [Fonctions de prédiction générales &#40;&#41;DMX ](../dmx/general-prediction-functions-dmx.md)  
  
