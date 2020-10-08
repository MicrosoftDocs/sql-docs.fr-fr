---
title: sp_rxPredict | Microsoft Docs
ms.date: 03/31/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: language-reference
f1_keywords:
- sp_rxPredict
- sp_rxPredict_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_rxPredict procedure
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 625157885fa4494f4d8c70da5bea8ac70472d3b5
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91809480"
---
# <a name="sp_rxpredict"></a>sp_rxPredict  
[!INCLUDE [SQL Server 2016 Windows only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]

Génère une valeur prédite pour une entrée donnée composée d’un modèle de Machine Learning stocké dans un format binaire dans une base de données SQL Server.

Fournit des notations sur les modèles de Machine Learning R et Python en temps quasi-réel. `sp_rxPredict` est une procédure stockée fournie en tant que wrapper pour la `rxPredict` fonction R dans [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler) et [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package), et la fonction python [rx_predict](/machine-learning-server/python-reference/revoscalepy/rx-predict) dans [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) et [MicrosoftML](/machine-learning-server/python-reference/microsoftml/microsoftml-package). Il est écrit en C++ et est optimisé spécifiquement pour les opérations de notation.

Bien que le modèle doive être créé à l’aide de R ou python, une fois qu’il est sérialisé et stocké dans un format binaire sur une instance du moteur de base de données cible, il peut être consommé à partir de cette instance du moteur de base de données même si l’intégration de R ou python n’est pas installée. Pour plus d’informations, consultez [notation en temps réel avec sp_rxPredict](../../machine-learning/predictions/real-time-scoring.md).

## <a name="syntax"></a>Syntaxe

```
sp_rxPredict  ( @model, @input )
```

### <a name="arguments"></a>Arguments

**model**

Modèle préformé dans un format pris en charge. 

**input**

Une requête SQL valide

### <a name="return-values"></a>Valeurs retournées

Une colonne score est retournée, ainsi que toutes les colonnes directes de la source de données d’entrée.
Des colonnes de score supplémentaires, telles que l’intervalle de confiance, peuvent être retournées si l’algorithme prend en charge la génération de telles valeurs.

## <a name="remarks"></a>Notes

Pour permettre l’utilisation de la procédure stockée, SQLCLR doit être activé sur l’instance.

> [!NOTE]
> Il existe des implications en matière de sécurité pour enabing cette option. Utilisez une autre implémentation, telle que la fonction [Transact-SQL Predict](../../t-sql/queries/predict-transact-sql.md?view=sql-server-2017) , si SQLCLR ne peut pas être activé sur votre serveur.

L’utilisateur a besoin `EXECUTE` d’une autorisation sur la base de données.

### <a name="supported-algorithms"></a>Algorithmes pris en charge

Pour créer et former un modèle, utilisez l’un des algorithmes pris en charge pour R ou python, fournis par [SQL Server machine learning services (r ou python)](../../machine-learning/sql-server-machine-learning-services.md), [SQL Server 2016 R Services](../../machine-learning/r/sql-server-r-services.md), [SQL Server machine learning Server (autonome) (r ou python)](../../machine-learning/r/r-server-standalone.md)ou [SQL Server 2016 R Server (autonome)](../../machine-learning/r/r-server-standalone.md?view=sql-server-2016).

#### <a name="r-revoscaler-models"></a>R : modèles RevoScaleR

  + [rxLinMod](/machine-learning-server/r-reference/revoscaler/rxlinmod)
  + [rxLogit](/machine-learning-server/r-reference/revoscaler/rxlogit)
  + [rxBTrees](/machine-learning-server/r-reference/revoscaler/rxbtrees)
  + [rxDtree](/machine-learning-server/r-reference/revoscaler/rxdtree)
  + [rxdForest](/machine-learning-server/r-reference/revoscaler/rxdforest)

#### <a name="r-microsoftml-models"></a>R : modèles MicrosoftML

  + [rxFastTrees](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [rxFastForest](/machine-learning-server/r-reference/microsoftml/rxfastforest)
  + [rxLogisticRegression](/machine-learning-server/r-reference/microsoftml/rxlogisticregression)
  + [rxOneClassSvm](/machine-learning-server/r-reference/microsoftml/rxoneclasssvm)
  + [rxNeuralNet](/machine-learning-server/r-reference/microsoftml/rxneuralnet)
  + [rxFastLinear](/machine-learning-server/r-reference/microsoftml/rxfastlinear)

#### <a name="r-transformations-supplied-by-microsoftml"></a>R : transformations fournies par MicrosoftML

  + [featurizeText](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [concat](/machine-learning-server/r-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/r-reference/microsoftml/categorical)
  + [categoricalHash](/machine-learning-server/r-reference/microsoftml/categoricalHash)
  + [selectFeatures](/machine-learning-server/r-reference/microsoftml/selectFeatures)

#### <a name="python-revoscalepy-models"></a>Python : modèles revoscalepy

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod)
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit)
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees)
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree)
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest)


#### <a name="python-microsoftml-models"></a>Python : modèles microsoftml

  + [rx_fast_trees](/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [rx_fast_forest](/machine-learning-server/python-reference/microsoftml/rx-fast-forest)
  + [rx_logistic_regression](/machine-learning-server/python-reference/microsoftml/rx-logistic-regression)
  + [rx_oneclass_svm](/machine-learning-server/python-reference/microsoftml/rx-oneclass-svm)
  + [rx_neural_network](/machine-learning-server/python-reference/microsoftml/rx-neural-network)
  + [rx_fast_linear](/machine-learning-server/python-reference/microsoftml/rx-fast-linear)

#### <a name="python-transformations-supplied-by-microsoftml"></a>Python : transformations fournies par microsoftml

  + [featurize_text](/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [concat](/machine-learning-server/python-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/python-reference/microsoftml/categorical)
  + [categorical_hash](/machine-learning-server/python-reference/microsoftml/categorical-hash)
  
### <a name="unsupported-model-types"></a>Types de modèles non pris en charge

Les types de modèles suivants ne sont pas pris en charge :

+ Modèles utilisant les `rxGlm` `rxNaiveBayes` algorithmes ou dans RevoScaleR
+ Modèles PMML dans R
+ Modèles créés à l’aide d’autres bibliothèques tierces 

## <a name="examples"></a>Exemples

```sql
DECLARE @model = SELECT @model 
FROM model_table 
WHERE model_name = 'rxLogit trained';

EXEC sp_rxPredict @model = @model,
@inputData = N'SELECT * FROM data';
```

En plus d’être une requête SQL valide, les données d’entrée dans * \@ inputData* doivent inclure des colonnes compatibles avec les colonnes du modèle stocké.

`sp_rxPredict` prend en charge uniquement les types de colonnes .NET suivants : double, float, Short, ushort, long, ULONG et String. Vous devrez peut-être filtrer les types non pris en charge dans vos données d’entrée avant de les utiliser pour une notation en temps réel. 

  Pour plus d’informations sur les types SQL correspondants, consultez [Mappage de type SQL-CLR](/dotnet/framework/data/adonet/sql/linq/sql-clr-type-mapping) ou [Mappage des données de paramètres CLR](../clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).