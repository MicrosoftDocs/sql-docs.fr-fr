---
description: PREDICT (Transact-SQL)
title: PREDICT (Transact-SQL)
titleSuffix: SQL machine learning
ms.custom: ''
ms.date: 06/26/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- PREDICT
- PREDICT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PREDICT clause
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||>=azure-sqldw-latest'
ms.openlocfilehash: 9ccee4b3502dafac331803ba1e8e56c068666bbc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347538"
---
# <a name="predict-transact-sql"></a>PREDICT (Transact-SQL)

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Génère une valeur prédite ou des scores calculés à partir d’un modèle stocké. Pour plus d’informations, consultez [Notation native à l’aide de la fonction T-SQL PREDICT](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).

## <a name="syntax"></a>Syntaxe

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```syntaxsql
PREDICT  
(  
  MODEL = @model | model_literal,  
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

MODEL = @model | model_literal  
```

::: moniker-end

::: moniker range=">=azure-sqldw-latest"

```syntaxsql
PREDICT  
(  
  MODEL = <model_object>,
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

<model_object> ::=
  {
    model_literal
    | model_variable
    | ( scalar_subquery )
  }
```

::: moniker-end

### <a name="arguments"></a>Arguments

**MODEL**

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
Le paramètre `MODEL` permet de spécifier le modèle utilisé pour calculer les scores ou effectuer une prédiction. Le modèle est spécifié sous forme de variable, de littéral ou d’expression scalaire.

`PREDICT` prend en charge les modèles formés à l’aide des packages [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) et [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
Le paramètre `MODEL` permet de spécifier le modèle utilisé pour calculer les scores ou effectuer une prédiction. Le modèle est spécifié sous forme de variable, de littéral ou d’expression scalaire.

Dans Azure SQL Managed Instance, `PREDICT` prend en charge les modèles au format [ONNX (Open Neural Network Exchange)](https://onnx.ai/get-started.html) ou des modèles formés à l’aide des packages [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) et [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md).
::: moniker-end

::: moniker range=">=azure-sqldw-latest"
Le paramètre `MODEL` permet de spécifier le modèle utilisé pour calculer les scores ou effectuer une prédiction. Le modèle est spécifié sous forme de variable, de littéral, d’expression scalaire ou de sous-requête scalaire.

Dans Azure Synapse Analytics, `PREDICT` prend en charge les modèles au format [ONNX (Open Neuronal Network Exchange)](https://onnx.ai/get-started.html).
::: moniker-end

**DONNÉES**

Le paramètre DATA permet de spécifier les données utilisées pour calculer les scores ou effectuer une prédiction. Les données sont spécifiées sous la forme d’une source de table dans la requête. La source de table peut être une table, un alias de table, un alias d’expression de table commune (CTE), une vue ou une fonction table.

**RUNTIME = ONNX**

> [!IMPORTANT]
> L’argument `RUNTIME = ONNX` n’est disponible que dans [Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview), [Azure SQL Edge](/azure/sql-database-edge/onnx-overview) et [Azure Synapse Analytics](/azure/synapse-analytics/overview-what-is).

Indique le moteur d’apprentissage automatique utilisé pour l’exécution du modèle. La valeur du paramètre `RUNTIME` est toujours `ONNX`. Le paramètre est obligatoire pour Azure SQL Edge et Azure Synapse Analytics. Dans Azure SQL Managed Instance, le paramètre est facultatif et utilisé uniquement lors de l’utilisation de modèles ONNX.

**WITH ( <result_set_definition> )**

La clause WITH permet de spécifier le schéma de la sortie retournée par la fonction `PREDICT`.

En plus des colonnes retournées par la fonction `PREDICT` proprement dite, toutes les colonnes qui font partie des données d’entrée peuvent être utilisées dans la requête.

### <a name="return-values"></a>Valeurs retournées

Aucun schéma prédéfini n’est disponible. Le contenu du modèle n’est pas validé et les valeurs de colonne retournées ne sont pas non plus validées.

- La fonction `PREDICT` passe par les colonnes en tant qu’entrée.
- La fonction `PREDICT` génère également de nouvelles colonnes, dont le nombre et le type de données dépendent du type de modèle qui a été utilisé pour la prédiction.

Les messages d’erreur liés aux données, au modèle ou au format de colonne sont retournés par la fonction de prédiction sous-jacente qui est associée au modèle.

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017"
## <a name="remarks"></a>Notes

La fonction `PREDICT` est prise en charge dans toutes les éditions de SQL Server 2017 et des versions ultérieures, sur Windows et Linux. [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) n’a pas besoin d’être activé pour utiliser `PREDICT`.
::: moniker-end

### <a name="supported-algorithms"></a>Algorithmes pris en charge

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
Le modèle que vous utilisez doit avoir été créé à l’aide de l’un des algorithmes pris en charge fournis dans les packages [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) ou [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md). Pour obtenir la liste des modèles actuellement pris en charge, consultez [Scoring natif à l’aide de la fonction T-SQL PREDICT](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).
::: moniker-end
::: moniker range="=azure-sqldw-latest"
Les algorithmes qui peuvent être convertis au format du modèle [ONNX](https://onnx.ai/) sont pris en charge.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Les algorithmes qui peuvent être convertis au format de modèle [ONNX](https://onnx.ai/) et les modèles que vous avez créés à l’aide d’un des algorithmes pris en charge fournis dans les packages [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) ou [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md) sont pris en charge. Pour obtenir la liste des algorithmes actuellement pris en charge dans RevoScaleR et revoscalepy, consultez [Scoring natif à l’aide de la fonction T-SQL PREDICT](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).
::: moniker-end

### <a name="permissions"></a>Autorisations

Aucune autorisation n’est requise pour `PREDICT`. Cependant, l’utilisateur doit avoir l’autorisation `EXECUTE` sur la base de données, ainsi que l’autorisation d’effectuer des requêtes sur les données utilisées comme entrées. L’utilisateur doit également pouvoir lire le modèle à partir d’une table, si le modèle a été stocké dans une table.

## <a name="examples"></a>Exemples

Les exemples suivants illustrent la syntaxe à utiliser pour appeler `PREDICT`.

### <a name="using-predict-in-a-from-clause"></a>Utilisation de PREDICT dans une clause FROM

Cet exemple référence la fonction `PREDICT` dans la clause `FROM` d’une instruction `SELECT` :

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d) WITH (Score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT test_model FROM scoring_model WHERE model_id = 1);

SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

::: moniker-end

L’alias **d** spécifié pour la source de table dans le paramètre `DATA` est utilisé pour référencer les colonnes appartenant à `dbo.mytable`. L’alias **p** spécifié pour la fonction `PREDICT` est utilisé pour référencer les colonnes retournées par la fonction `PREDICT`.

- Le modèle est stocké en tant que colonne `varbinary(max)` dans l’appel de table **Models**. Des informations supplémentaires telles que **ID** et **description** sont enregistrées dans la table pour identifier le mode.
- L’alias **d** spécifié pour la source de table dans le paramètre `DATA` est utilisé pour référencer les colonnes appartenant à `dbo.mytable`. Les noms des colonnes de données d’entrée doivent correspondre au nom des entrées du modèle.
- L’alias **p** spécifié pour la fonction `PREDICT` est utilisé pour référencer la colonne prédite retournée par la fonction `PREDICT`. Le nom de colonne doit avoir le même nom que le nom de sortie du modèle.
- Toutes les colonnes de données d’entrée et les colonnes prédites peuvent être affichées dans l’instruction SELECT.

::: moniker range=">=azure-sqldw-latest"

L’exemple de requête précédent peut être réécrit pour créer une vue en spécifiant `MODEL` en tant que sous-requête scalaire :

```sql
CREATE VIEW predictions
AS
SELECT d.*, p.Score
FROM PREDICT(MODEL = (SELECT test_model FROM scoring_model WHERE model_id = 1),
             DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

:::moniker-end

### <a name="combining-predict-with-an-insert-statement"></a>Combinaison de PREDICT avec une instruction INSERT

Un cas d’usage courant pour la prédiction consiste à calculer un score pour les données d’entrée, puis à insérer les valeurs prédites dans une table. L’exemple suivant suppose que l’application appelante utilise une procédure stockée pour insérer une ligne contenant la valeur prédite dans une table :

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d) WITH(score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH(score FLOAT) AS p;
```

:::moniker-end

- Les résultats de `PREDICT` sont stockés dans une table appelée PredictionResults. 
- Le modèle est stocké en tant que colonne `varbinary(max)` dans l’appel de table **Models**. Des informations supplémentaires telles que l’ID et la description sont enregistrées dans la table pour identifier le modèle.
- L’alias **d** spécifié pour la table source dans le paramètre `DATA` est utilisé pour référencer les colonnes dans `dbo.mytable`. Les noms des colonnes de données en entrée doivent correspondre au nom des entrées du modèle.
- L’alias **p** spécifié pour la fonction `PREDICT` est utilisé pour référencer la colonne prédite retournée par la fonction `PREDICT`. Le nom de colonne doit avoir le même nom que le nom de sortie du modèle.
- Toutes les colonnes d’entrée et les colonnes prédites peuvent être affichées dans l’instruction SELECT.

## <a name="next-steps"></a>Étapes suivantes

- [Notation native à l’aide de la fonction T-SQL PREDICT](../../machine-learning/predictions/native-scoring-predict-transact-sql.md)
::: moniker range="=azure-sqldw-latest||=azuresqldb-mi-current"
-   [En savoir plus sur les modèles ONNX](/azure/machine-learning/concept-onnx#get-onnx-models)
::: moniker-end
