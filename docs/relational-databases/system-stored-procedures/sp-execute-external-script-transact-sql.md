---
description: sp_execute_external_script (Transact-SQL)
title: sp_execute_external_script (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- sp_execute_external_script_TSQL
- sys.sp_execute_external_script
- sys.sp_execute_external_script_TSQL
- sp_execute_external_script
dev_langs:
- TSQL
helpviewer_keywords:
- sp_execute_external_script
ms.assetid: de4e1fcd-0e1a-4af3-97ee-d1becc7f04df
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: ef90d43021349b0327ff96ebdec85b6a8483e43c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188001"
---
# <a name="sp_execute_external_script-transact-sql"></a>sp_execute_external_script (Transact-SQL)
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
La procédure stockée **sp_execute_external_script** exécute un script fourni en tant qu’argument d’entrée pour la procédure et est utilisé avec les [extensions de langage](../../language-extensions/language-extensions-overview.md)et de [machine learning services](../../machine-learning/sql-server-machine-learning-services.md) . 

Pour Machine Learning Services, [python](../../machine-learning/concepts/extension-python.md) et [R](../../machine-learning/concepts/extension-r.md) sont des langages pris en charge. Pour les extensions de langage, Java est pris en charge, mais doit être défini avec [Create External Language](../../t-sql/statements/create-external-language-transact-sql.md).

Pour exécuter **sp_execute_external_script**, vous devez d’abord installer les extensions de langage ou de machine learning services. Pour plus d’informations, consultez [installer SQL Server machine learning services (Python et R) sur Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md) et [Linux](../../linux/sql-server-linux-setup-machine-learning.md), ou [installer les extensions de langage SQL Server sur Windows](../../language-extensions/install/windows-java.md) et [Linux](../../linux/sql-server-linux-setup-language-extensions-java.md).
::: moniker-end

::: moniker range="=sql-server-2017"
La procédure stockée **sp_execute_external_script** exécute un script fourni en tant qu’argument d’entrée pour la procédure et est utilisé avec [Machine Learning Services](../../machine-learning/sql-server-machine-learning-services.md) sur SQL Server 2017.

Pour Machine Learning Services, [python](../../machine-learning/concepts/extension-python.md) et [R](../../machine-learning/concepts/extension-r.md) sont des langages pris en charge.

Pour exécuter **sp_execute_external_script**, vous devez d’abord installer machine learning services. Pour plus d’informations, consultez [installer SQL Server machine learning services (Python et R) sur Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md).
::: moniker-end

::: moniker range="=sql-server-2016"
La procédure stockée **sp_execute_external_script** exécute un script fourni en tant qu’argument d’entrée pour la procédure et est utilisé avec [R Services](../../machine-learning/r/sql-server-r-services.md) sur SQL Server 2016.

Pour R services,  [r](../../machine-learning/concepts/extension-r.md) est le langage pris en charge.

Pour exécuter **sp_execute_external_script**, vous devez d’abord installer R services. Pour plus d’informations, consultez [installer SQL Server machine learning services (Python et R) sur Windows](../../machine-learning/install/sql-r-services-windows-install.md).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
La procédure stockée **sp_execute_external_script** exécute un script fourni en tant qu’argument d’entrée pour la procédure et est utilisé avec [machine learning services dans Azure SQL Managed instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).

Pour Machine Learning Services, [python](../../machine-learning/concepts/extension-python.md) et [R](../../machine-learning/concepts/extension-r.md) sont des langages pris en charge.

Pour exécuter **sp_execute_external_script**, vous devez d’abord activer machine learning services. Pour plus d’informations, consultez la [machine learning services dans la documentation Azure SQL Managed instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
## <a name="syntax"></a>Syntaxe

```
sp_execute_external_script
    @language = N'language',
    @script = N'script'  
    [ , @input_data_1 = N'input_data_1' ]
    [ , @input_data_1_name = N'input_data_1_name' ]  
    [ , @input_data_1_order_by_columns = N'input_data_1_order_by_columns' ]
    [ , @input_data_1_partition_by_columns = N'input_data_1_partition_by_columns' ]  
    [ , @output_data_1_name = N'output_data_1_name' ]  
    [ , @parallel = 0 | 1 ]  
    [ , @params = N'@parameter_name data_type [ OUT | OUTPUT ] [ ,...n ]' ] 
    [ , @parameter1 = 'value1' [ OUT | OUTPUT ] [ ,...n ] ]
```
::: moniker-end
::: moniker range="=sql-server-2016"
## <a name="syntax-for-sql-server-2017-and-earlier"></a>Syntaxe pour SQL Server 2017 et versions antérieures

```
sp_execute_external_script   
    @language = N'language',   
    @script = N'script'  
    [ , @input_data_1 = N'input_data_1' ]   
    [ , @input_data_1_name = N'input_data_1_name' ]  
    [ , @output_data_1_name = N'output_data_1_name' ]  
    [ , @parallel = 0 | 1 ]  
    [ , @params = N'@parameter_name data_type [ OUT | OUTPUT ] [ ,...n ]' ] 
    [ , @parameter1 = 'value1' [ OUT | OUTPUT ] [ ,...n ] ]
```
::: moniker-end

## <a name="arguments"></a>Arguments
 **\@ Language** = N'*Language*'  
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
 Indique le langage de script. *Language* est de **type sysname**. Les valeurs valides sont **R**, **python** et n’importe quel langage défini avec [Create External Language](../../t-sql/statements/create-external-language-transact-sql.md) (par exemple, Java).
::: moniker-end
::: moniker range="=sql-server-2017"
 Indique le langage de script. *Language* est de **type sysname**. Dans SQL Server 2017, les valeurs valides sont **R** et **python**.
::: moniker-end
::: moniker range="=sql-server-2016"
 Indique le langage de script. *Language* est de **type sysname**. Dans SQL Server 2016, la seule valeur valide est **R**.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
 Indique le langage de script. *Language* est de **type sysname**. Dans Azure SQL Managed Instance, les valeurs valides sont **R** et **python**.
::: moniker-end

 **\@ script = N** script du langage externe de *script* spécifié en tant qu’entrée littérale ou variable. le *script* est **de type nvarchar (max)**.  

`[ @input_data_1 =  N'input_data_1' ]` Spécifie les données d’entrée utilisées par le script externe sous la forme d’une [!INCLUDE[tsql](../../includes/tsql-md.md)] requête. Le type de données de *input_data_1* est **nvarchar (max)**.

`[ @input_data_1_name = N'input_data_1_name' ]` Spécifie le nom de la variable utilisée pour représenter la requête définie par @input_data_1 . Le type de données de la variable dans le script externe dépend de la langue. Dans le cas de R, la variable d’entrée est une trame de données. Dans le cas de Python, l’entrée doit être tabulaire. *input_data_1_name* est de **type sysname**.  La valeur par défaut est *InputDataSet*.  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
`[ @input_data_1_order_by_columns = N'input_data_1_order_by_columns' ]` Utilisé pour générer des modèles par partition. Spécifie le nom de la colonne utilisée pour trier le jeu de résultats, par exemple par nom de produit. Le type de données de la variable dans le script externe dépend de la langue. Dans le cas de R, la variable d’entrée est une trame de données. Dans le cas de Python, l’entrée doit être tabulaire.

`[ @input_data_1_partition_by_columns = N'input_data_1_partition_by_columns' ]` Utilisé pour générer des modèles par partition. Spécifie le nom de la colonne utilisée pour segmenter les données, telles que la région géographique ou la date. Le type de données de la variable dans le script externe dépend de la langue. Dans le cas de R, la variable d’entrée est une trame de données. Dans le cas de Python, l’entrée doit être tabulaire. 
::: moniker-end

`[ @output_data_1_name =  N'output_data_1_name' ]` Spécifie le nom de la variable dans le script externe qui contient les données à retourner à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’achèvement de l’appel de la procédure stockée. Le type de données de la variable dans le script externe dépend de la langue. Pour R, la sortie doit être une trame de données. Pour Python, la sortie doit être une trame de données pandas. *output_data_1_name* est de **type sysname**.  La valeur par défaut est *OutputDataSet*.  

`[ @parallel = 0 | 1 ]` Activez l’exécution parallèle des scripts R en affectant la valeur `@parallel` 1 au paramètre. La valeur par défaut de ce paramètre est 0 (aucun parallélisme). Si `@parallel = 1` et que la sortie est diffusée directement sur l’ordinateur client, la `WITH RESULT SETS` clause est obligatoire et un schéma de sortie doit être spécifié.  

 + Pour les scripts R qui n’utilisent pas de fonctions RevoScaleR, l’utilisation du  `@parallel` paramètre peut être bénéfique pour le traitement de jeux de données volumineux, en supposant que le script peut être parallélisée de façon triviale. Par exemple, lors de l’utilisation de la `predict` fonction R avec un modèle pour générer de nouvelles prédictions, définissez `@parallel = 1` en tant qu’indicateur pour le moteur de requête. Si la requête peut être parallélisée, les lignes sont distribuées selon le paramètre **MAXDOP** .  
  
 + Pour les scripts R qui utilisent des fonctions RevoScaleR, le traitement parallèle est géré automatiquement et vous ne devez pas spécifier `@parallel = 1` à l’appel **sp_execute_external_script** .  
  
`[ @params = N'@parameter_name data_type [ OUT | OUTPUT ] [ ,...n ]' ]` Liste des déclarations de paramètre d’entrée utilisées dans le script externe.  
  
`[ @parameter1 = 'value1' [ OUT | OUTPUT ] [ ,...n ] ]` Liste de valeurs pour les paramètres d’entrée utilisés par le script externe.  

## <a name="remarks"></a>Notes

> [!IMPORTANT]
> L’arborescence de requêtes est contrôlée par SQL Machine Learning et les utilisateurs ne peuvent pas effectuer d’opérations arbitraires sur la requête.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Utilisez **sp_execute_external_script** pour exécuter des scripts écrits dans un langage pris en charge. Les langues prises en charge sont **python** et **R** utilisées avec machine learning services, ainsi que tous les langages définis avec [Create External Language](../../t-sql/statements/create-external-language-transact-sql.md) (par exemple, Java) utilisés avec les extensions de langage.
::: moniker-end
::: moniker range="=sql-server-2017"
Utilisez **sp_execute_external_script** pour exécuter des scripts écrits dans un langage pris en charge. Les langues prises en charge sont **python** et **R** dans SQL Server 2017 machine learning services.
::: moniker-end
::: moniker range="=sql-server-2016"
Utilisez **sp_execute_external_script** pour exécuter des scripts écrits dans un langage pris en charge. La seule langue prise en charge est **r** dans SQL Server les Services r 2016.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Utilisez **sp_execute_external_script** pour exécuter des scripts écrits dans un langage pris en charge. Les langues prises en charge sont **python** et **R** dans Azure SQL Managed instance machine learning services.
::: moniker-end

Par défaut, les jeux de résultats retournés par cette procédure stockée sont générés avec des colonnes sans nom. Les noms de colonne utilisés dans un script sont locaux dans l’environnement de script et ne sont pas reflétés dans le jeu de résultats sorti. Pour nommer les colonnes du jeu de résultats, utilisez la `WITH RESULT SET` clause de [`EXECUTE`](../../t-sql/language-elements/execute-transact-sql.md) .

En plus de retourner un jeu de résultats, vous pouvez retourner des valeurs scalaires à à l’aide de paramètres de sortie.

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
Vous pouvez contrôler les ressources utilisées par les scripts externes en configurant un pool de ressources externes. Pour plus d’informations, consultez [Create External RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md). Les informations sur la charge de travail peuvent être obtenues à partir des affichages catalogue du gouverneur de ressources, des DMV et des compteurs. Pour plus d’informations, consultez [Resource Governor les affichages catalogue &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md), [Resource Governor vues de gestion dynamique associées &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)et [SQL Server, objet scripts externes](../../relational-databases/performance-monitor/sql-server-external-scripts-object.md).  
::: moniker-end

### <a name="monitor-script-execution"></a>Surveiller l’exécution du script

Surveiller l’exécution du script à l’aide de [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md) et [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
### <a name="parameters-for-partition-modeling"></a>Paramètres pour la modélisation de partition

Vous pouvez définir deux paramètres supplémentaires qui activent la modélisation sur des données partitionnées, où les partitions sont basées sur une ou plusieurs colonnes que vous fournissez, qui segmentent naturellement un jeu de données en partitions logiques créées et utilisées uniquement pendant l’exécution du script. Les colonnes contenant des valeurs répétitives pour l’âge, le sexe, la région géographique, la date ou l’heure, sont quelques exemples qui se prêtent à des jeux de données partitionnés.

Les deux paramètres sont **input_data_1_partition_by_columns** et **input_data_1_order_by_columns**, où le deuxième paramètre est utilisé pour trier le jeu de résultats. Les paramètres sont passés comme entrées à `sp_execute_external_script` avec le script externe qui s’exécute une fois pour chaque partition. Pour plus d’informations et d’exemples, consultez [Didacticiel : créer des modèles basés sur une partition](../../machine-learning/tutorials/r-tutorial-create-models-per-partition.md).

Vous pouvez exécuter le script en parallèle en spécifiant `@parallel=1` . Si la requête d’entrée peut être parallélisée, vous devez définir dans le `@parallel=1` cadre de vos arguments sur `sp_execute_external_script` . Par défaut, l’optimiseur de requête fonctionne sous `@parallel=1` sur des tables contenant plus de 256 lignes, mais si vous souhaitez gérer cela explicitement, ce script comprend le paramètre comme une démonstration.

> [!Tip]
> Pour les charges de travail d’entraînement, vous pouvez utiliser `@parallel` avec n’importe quel script d’entraînement arbitraire, même ceux qui utilisent des algorithmes non-Microsoft-rx. En règle générale, seuls les algorithmes RevoScaleR (avec le préfixe rx) offrent un parallélisme dans les scénarios d’entraînement dans SQL Server. Toutefois, avec les nouveaux paramètres de SQL Server 2019 et versions ultérieures, vous pouvez paralléliser un script qui appelle des fonctions qui ne sont pas spécifiquement conçues avec cette fonctionnalité.
::: moniker-end

### <a name="streaming-execution-for-python-and-r-scripts"></a>Exécution de la diffusion en continu pour les scripts Python et R  

La diffusion en continu permet au script Python ou R de travailler avec plus de données que la mémoire ne peut en contenir. Pour contrôler le nombre de lignes transmises pendant la diffusion en continu, spécifiez une valeur entière pour le paramètre `@r_rowsPerRead` dans la `@params` collection.  Par exemple, si vous effectuez l’apprentissage d’un modèle qui utilise des données très larges, vous pouvez ajuster la valeur pour lire moins de lignes, afin de vous assurer que toutes les lignes peuvent être envoyées dans un segment de données. Vous pouvez également utiliser ce paramètre pour gérer le nombre de lignes lues et traitées en même temps, afin d’atténuer les problèmes de performances du serveur. 
  
Le `@r_rowsPerRead` paramètre pour la diffusion en continu et l' `@parallel` argument doivent être considérés comme des indicateurs. Pour que l’indicateur soit appliqué, il doit être possible de générer un plan de requête SQL qui comprend un traitement parallèle. Si ce n’est pas possible, le traitement parallèle ne peut pas être activé.  
  
> [!NOTE]  
> La diffusion en continu et le traitement parallèle sont pris en charge uniquement dans Enterprise Edition. Vous pouvez inclure les paramètres dans vos requêtes dans l’édition standard sans générer d’erreur, mais les paramètres n’ont aucun effet et les scripts R s’exécutent dans un processus unique.  
  
## <a name="restrictions"></a>Restrictions  

### <a name="data-types"></a>Types de données

Les types de données suivants ne sont pas pris en charge lorsqu’ils sont utilisés dans la requête ou les paramètres d’entrée de **sp_execute_external_script** procédure, et renvoient une erreur de type non pris en charge.  

En guise de solution de contournement, **effectuez un cast** de la colonne ou de la valeur en un type pris en charge dans [!INCLUDE[tsql](../../includes/tsql-md.md)] avant de l’envoyer au script externe.  
  
+ **cursor**  
  
+ **timestamp**  
  
+ **datetime2**, **DateTimeOffset**, **Time**  
  
+ **sql_variant**  
  
+ **texte**, **image**  
  
+ **xml**  
  
+ **hierarchyid**, **Geometry**, **Geography**  
  
+ types CLR définis par l'utilisateur

En général, tout jeu de résultats qui ne peut pas être mappé à un [!INCLUDE[tsql](../../includes/tsql-md.md)] type de données est généré comme valeur null.  

### <a name="restrictions-specific-to-r"></a>Restrictions spécifiques à R

Si l’entrée comprend des valeurs **DateTime** qui ne correspondent pas à la plage de valeurs autorisée dans R, les valeurs sont converties en **na**. Cela est nécessaire, car SQL Machine Learning autorise une plus grande plage de valeurs que ce qui est pris en charge dans le langage R.

Les valeurs float (par exemple,,, `+Inf` `-Inf` `NaN` ) ne sont pas prises en charge dans SQL machine learning même si les deux langages utilisent IEEE 754. Le comportement actuel envoie simplement les valeurs à SQL directement. par conséquent, le client SQL génère une erreur. Par conséquent, ces valeurs sont converties en valeurs **null**.

## <a name="permissions"></a>Autorisations

Requiert **l’autorisation exécuter une** base de données de script externe.  

## <a name="examples"></a>Exemples

Cette section contient des exemples de la façon dont cette procédure stockée peut être utilisée pour exécuter des scripts R ou python à l’aide de [!INCLUDE[tsql](../../includes/tsql-md.md)] .

### <a name="a-return-an-r-data-set-to-sql-server"></a>R. Retourne un jeu de données R SQL Server  

L’exemple suivant crée une procédure stockée qui utilise **sp_execute_external_script** pour retourner le jeu de données Iris inclus avec R.  

```sql
DROP PROC IF EXISTS get_iris_dataset;  
go  
CREATE PROC get_iris_dataset
AS  
BEGIN  
 EXEC   sp_execute_external_script  
       @language = N'R'  
     , @script = N'iris_data <- iris;'
     , @input_data_1 = N''  
     , @output_data_1_name = N'iris_data'
     WITH RESULT SETS (("Sepal.Length" float not null,
           "Sepal.Width" float not null,  
        "Petal.Length" float not null,
        "Petal.Width" float not null, "Species" varchar(100)));  
END;
GO
```

### <a name="b-create-a-python-model-and-generate-scores-from-it"></a>B. Créer un modèle Python et générer des scores à partir de ce modèle

Cet exemple montre comment utiliser `sp_execute_external_script` pour générer des scores sur un modèle python simple.

```sql
CREATE PROCEDURE [dbo].[py_generate_customer_scores]
AS
BEGIN

-- Input query to generate the customer data
DECLARE @input_query NVARCHAR(MAX) = N'SELECT customer, orders, items, cost FROM dbo.Sales.Orders'

EXEC sp_execute_external_script @language = N'Python', @script = N'
import pandas as pd
from sklearn.cluster import KMeans

# Get data from input query
customer_data = my_input_data

# Define the model
n_clusters = 4
est = KMeans(n_clusters=n_clusters, random_state=111).fit(customer_data[["orders","items","cost"]])
clusters = est.labels_
customer_data["cluster"] = clusters

OutputDataSet = customer_data
'
, @input_data_1 = @input_query
, @input_data_1_name = N'my_input_data'
WITH RESULT SETS (("CustomerID" int, "Orders" float,"Items" float,"Cost" float,"ClusterResult" float));
END;
GO
```

Les en-têtes de colonne utilisés dans le code python ne sont pas générés dans SQL Server ; par conséquent, utilisez l’instruction WITH RESULT pour spécifier les noms de colonnes et les types de données que SQL doit utiliser.

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
### <a name="c-generate-an-r-model-based-on-data-from-sql-server"></a>C. Générer un modèle R basé sur les données de SQL Server  

L’exemple suivant crée une procédure stockée qui utilise **sp_execute_external_script** pour générer un modèle d’IRIS et retourner le modèle.  

> [!NOTE]
> Cet exemple nécessite une installation avancée du package e1071. Pour plus d’informations, consultez [installer des packages R supplémentaires sur SQL Server](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md).

```sql
DROP PROC IF EXISTS generate_iris_model;
GO
CREATE PROC generate_iris_model
AS
BEGIN
 EXEC sp_execute_external_script  
      @language = N'R'  
     , @script = N'  
          library(e1071);  
          irismodel <-naiveBayes(iris_data[,1:4], iris_data[,5]);  
          trained_model <- data.frame(payload = as.raw(serialize(irismodel, connection=NULL)));  
'  
     , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species" from iris_data'  
     , @input_data_1_name = N'iris_data'  
     , @output_data_1_name = N'trained_model'  
    WITH RESULT SETS ((model varbinary(max)));  
END;
GO
```

Pour générer un modèle semblable à l’aide de Python, vous devez remplacer l’identificateur de langage `@language=N'R'` par `@language = N'Python'` et apporter les modifications nécessaires à l’argument `@script`. Autrement, tous les paramètres fonctionnent de la même manière que pour R.
::: moniker-end

Pour calculer les scores, vous pouvez également utiliser la fonction [PREDICT](../../t-sql/queries/predict-transact-sql.md) native, qui est généralement plus rapide car elle évite d’appeler le runtime Python ou R.

## <a name="see-also"></a>Voir aussi

+ [Machine Learning SQL](../../machine-learning/index.yml)
+ [Extensions de langage SQL Server](../../language-extensions/language-extensions-overview.md). 
+ [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
+ [CRÉER une bibliothèque externe &#40;&#41;Transact-SQL ](../../t-sql/statements/create-external-library-transact-sql.md)  
+ [sp_prepare &#40;Transact SQL&#41;](../../relational-databases/system-stored-procedures/sp-prepare-transact-sql.md)   
+ [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
+ [external scripts enabled (option de configuration de serveur)](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)   
+ [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
+ [SQL Server, objet External Scripts](../../relational-databases/performance-monitor/sql-server-external-scripts-object.md)  
+ [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md)  
+ [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md)