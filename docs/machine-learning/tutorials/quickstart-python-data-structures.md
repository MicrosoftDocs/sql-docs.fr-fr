---
title: 'Démarrage rapide : Structures de données Python'
titleSuffix: SQL machine learning
description: Dans ce démarrage rapide, vous allez apprendre à utiliser des structures de données et des objets de données en Python avec le Machine Learning SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: cawrites
ms.author: chadam
ms.reviewer: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: ed35820d38ea31ea0b7f8bae9b0a440398d55674
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85784097"
---
# <a name="quickstart-data-structures-and-objects-using-python-with-sql-machine-learning"></a>Démarrage rapide : Structure de données et objets en Python avec le Machine Learning SQL
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
Suivez ce démarrage rapide pour savoir comment utiliser des structures de données et des types de données quand Python est utilisé dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) ou sur des [clusters Big Data](../../big-data-cluster/machine-learning-services.md). Vous en apprendrez plus sur le transfert de données entre Python et SQL Server, ainsi que les problèmes courants qui peuvent se produire.
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
Suivez ce démarrage rapide pour savoir comment utiliser des structures de données et des types de données quand Python est utilisé dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Vous en apprendrez plus sur le transfert de données entre Python et SQL Server, ainsi que les problèmes courants qui peuvent se produire.
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
Dans ce démarrage rapide, vous allez apprendre à utiliser des structures de données et des types de données en Python dans [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview). Vous découvrirez comment déplacer des données entre Python et Azure SQL Managed Instance, ainsi que les problèmes courants qui peuvent se produire.
::: moniker-end

Le Machine Learning SQL s’appuie sur le package Python **Pandas**, qui est idéal pour travailler avec des données tabulaires. Toutefois, vous ne pouvez pas vous contenter de transmettre un scalaire de Python à votre base de données pour que *tout fonctionne*. Dans ce démarrage rapide, vous allez passer en revue la définition de certaines structures de données de base pour vous préparer à d’autres problèmes que vous pourriez rencontrer en transmettant des données tabulaires entre Python et la base de données.

Voici les concepts à connaître :

- Une trame de données est une table avec _plusieurs_ colonnes.
- Une colonne d’une trame de données est un objet de type liste appelé « série ».
- Une valeur d’une trame de données est appelée une cellule et est accessible par index.

Comment exposeriez-vous le résultat d’un calcul en tant que trame de données, si un élément data.frame requiert une structure tabulaire ? Une solution pourrait être de représenter la valeur scalaire en tant que série, qui est facilement convertible en trame de données.

> [!NOTE]
> Lorsque les dates sont renvoyées, Python dans SQL utilise DATETIME qui a une plage de dates limitée de 1753-01-01(-53690) à 9999-12-31(2958463).

## <a name="prerequisites"></a>Prérequis

Pour effectuer ce démarrage rapide, vous avez besoin de ce qui suit.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
- SQL Server Machine Learning Services. Pour savoir comment installer Machine Learning Services, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md) ou le [Guide d’installation Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). Vous pouvez également [activer Machine Learning Services sur des clusters Big Data SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
- SQL Server Machine Learning Services. Pour savoir comment installer Machine Learning Services, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
- Azure SQL Managed Instance Machine Learning Services. Pour savoir comment vous inscrire, consultez [Vue d’ensemble d’Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

- Un outil permettant d’exécuter des requêtes SQL qui contiennent des scripts Python. Ce guide de démarrage rapide utilise [Azure Data Studio](../../azure-data-studio/what-is.md).

## <a name="scalar-value-as-a-series"></a>Valeur scalaire en tant que série

Cet exemple effectue une simple opération mathématique et convertit un scalaire en une série.

1. Une série requiert un index, que vous pouvez assigner manuellement, comme illustré ici, ou par programmation.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   a = 1
   b = 2
   c = a/b
   print(c)
   s = pandas.Series(c, index =["simple math example 1"])
   print(s)
   '
   ```

   Étant donné que la série n’a pas été convertie en élément data.frame, les valeurs sont renvoyées dans la fenêtre Messages, mais vous pouvez voir que les résultats sont dans un format tabulaire plus grand.

   **Résultats**

   ```text
   STDOUT message(s) from external script: 
   0.5
   simple math example 1    0.5
   dtype: float64
   ```

1. Pour augmenter la longueur de la série, vous pouvez ajouter de nouvelles valeurs à l’aide d’un tableau.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   a = 1
   b = 2
   c = a/b
   d = a*b
   s = pandas.Series([c,d])
   print(s)
   '
   ```

   Si vous ne spécifiez pas d’index, un index avec des valeurs comprises entre 0 et la longueur du tableau est généré.

   **Résultats**

   ```text
   STDOUT message(s) from external script:
   0    0.5
   1    2.0
   dtype: float64
   ```

1. Si vous augmentez le nombre de valeurs de **l’index**, mais que vous n’ajoutez pas de nouvelles valeurs de **données**, les valeurs de données sont répétées pour remplir la série.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   a = 1
   b = 2
   c = a/b
   s = pandas.Series(c, index =["simple math example 1", "simple math example 2"])
   print(s)
   '
   ```

   **Résultats**

   ```text
   STDOUT message(s) from external script:
   0.5
   simple math example 1    0.5
   simple math example 2    0.5
   dtype: float64
   ```

## <a name="convert-series-to-data-frame"></a>Convertir une série en trame de données

La conversion mathématique du scalaire permet d’obtenir une structure tabulaire. Il reste néanmoins à la convertir dans un format géré par le Machine Learning SQL.

1. Pour convertir une série en élément data.frame, appelez la méthode [DataFrame](https://pandas.pydata.org/pandas-docs/stable/dsintro.html#dataframe) pandas.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   import pandas as pd
   a = 1
   b = 2
   c = a/b
   d = a*b
   s = pandas.Series([c,d])
   print(s)
   df = pd.DataFrame(s)
   OutputDataSet = df
   '
   WITH RESULT SETS((ResultValue FLOAT))
   ```

   Le résultat est illustré ci-dessous. Même si vous utilisez l’index pour récupérer des valeurs spécifiques à partir de l’élément data.frame, les valeurs d’index ne font pas partie de la sortie.

   **Résultats**

   |ResultValue|
   |------|
   |0.5|
   |2|

## <a name="output-values-into-dataframe"></a>Valeurs de sortie dans l’élément data.frame

À présent, vous allez générer des valeurs spécifiques à partir de deux séries de résultats mathématiques dans un élément data.frame. La première a un index de valeurs séquentielles générées par Python. La seconde utilise un index arbitraire de valeurs de chaîne.

1. L’exemple suivant obtient une valeur de la série à l’aide d’un index d’entiers.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   import pandas as pd
   a = 1
   b = 2
   c = a/b
   d = a*b
   s = pandas.Series([c,d])
   print(s)
   df = pd.DataFrame(s, index=[1])
   OutputDataSet = df
   '
   WITH RESULT SETS((ResultValue FLOAT))
   ```

   **Résultats**

   |ResultValue|
   |------|
   |2|

   N’oubliez pas que l’index généré automatiquement commence à 0. Essayez d’utiliser une valeur d’index hors de la plage pour voir ce qui se passe.

1. À présent, récupérez une valeur de l’autre trame de données à l’aide d’un index de chaîne.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   import pandas as pd
   a = 1
   b = 2
   c = a/b
   s = pandas.Series(c, index =["simple math example 1", "simple math example 2"])
   print(s)
   df = pd.DataFrame(s, index=["simple math example 1"])
   OutputDataSet = df
   '
   WITH RESULT SETS((ResultValue FLOAT))
   ```

   **Résultats**

   |ResultValue|
   |------|
   |0.5|

   Si vous essayez d’utiliser un index numérique pour obtenir une valeur de cette série, vous recevez une erreur.

## <a name="next-steps"></a>Étapes suivantes

Pour savoir comment écrire des fonctions Python avancées avec le Machine Learning SQL, suivez ce démarrage rapide :

> [!div class="nextstepaction"]
> [Écriture de fonctions Python avancées](quickstart-python-functions.md)
