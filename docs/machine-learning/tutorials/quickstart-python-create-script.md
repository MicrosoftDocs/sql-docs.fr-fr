---
title: 'Démarrage rapide : Exécuter des scripts Python'
titleSuffix: SQL machine learning
description: Exécuter un ensemble de scripts Python simples en utilisant SQL Server Machine Learning Services, des clusters Big Data ou des instances gérées Azure SQL Managed Instance. Découvrez comment utiliser la procédure stockée sp_execute_external_script pour exécuter le script.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q1
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: ddee800cdba3a90414b0ee9d3eb9638b9a343e67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351533"
---
# <a name="quickstart-run-simple-python-scripts-with-sql-machine-learning"></a>Démarrage rapide : Exécution de scripts Python simples avec le Machine Learning SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Dans ce guide de démarrage rapide, vous allez exécuter un ensemble de scripts Python simples en utilisant [Machine Learning Services de SQL Server](../sql-server-machine-learning-services.md), [Machine Learning Services d’Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) ou des [clusters Big Data SQL Server](../../big-data-cluster/machine-learning-services.md). Vous allez découvrir comment utiliser la procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) pour exécuter ce script dans une instance SQL Server.

## <a name="prerequisites"></a>Prérequis

Pour effectuer ce démarrage rapide, vous avez besoin de ce qui suit.

- Une base de données SQL sur l’une de ces plateformes :
  - [Machine Learning Services SQL Server](../sql-server-machine-learning-services.md). Pour l’installer, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md) ou le [Guide d’installation Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Clusters Big Data SQL Server. Voir comment [Activer Machine Learning Services sur des clusters Big Data SQL Server](../../big-data-cluster/machine-learning-services.md).
  - Azure SQL Managed Instance Machine Learning Services. Pour plus d’informations, consultez [Présentation de Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).

- Un outil permettant d’exécuter des requêtes SQL qui contiennent des scripts Python. Ce guide de démarrage rapide utilise [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="run-a-simple-script"></a>Exécuter un script simple

Pour exécuter un script Python, vous devez le transmettre sous forme d’argument à la procédure stockée système, [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Cette procédure stockée système démarre le runtime Python dans le contexte du Machine Learning SQL, transmet les données à Python, gère les sessions utilisateur Python de manière sécurisée et retourne les résultats au client.

Au cours des étapes suivantes, vous allez exécuter cet exemple de script Python dans votre base de données :

```python
a = 1
b = 2
c = a/b
d = a*b
print(c, d)
```

1. Ouvrez une nouvelle fenêtre Requête connectée à votre instance SQL dans **Azure Data Studio**.

1. Transmettez le script Python complet à la procédure stockée `sp_execute_external_script`.

   Le script est transmis via l’argument `@script`. Tout ce qui se trouve dans l’argument `@script` doit être du code Python valide.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

1. Le résultat correct est calculé et la fonction Python `print` retourne le résultat dans la fenêtre **Messages**.

   Voici comment il se présente.

    **Résultats**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Exécuter un script Hello World

Un exemple type de script est celui qui génère simplement la chaîne « Hello World ». Exécutez la commande suivante :

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'OutputDataSet = InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Les entrées de la procédure stockée `sp_execute_external_script` sont les suivantes :

| Entrée | Description |
|-|-|
| @language | définit l’extension de langage à appeler (dans le cas présent, Python) |
| @script | Cette entrée définit les commandes transmises au runtime Python. L’intégralité de votre script Python doit être placé dans cet argument en tant que texte Unicode. Vous pouvez aussi ajouter le texte à une variable de type **nvarchar**, puis appeler cette variable |
| @input_data_1 | données retournées par la requête, transmises au runtime Python, qui retourne les données sous forme de trame de données |
| WITH RESULT SETS | Cette clause définit le schéma de la table de données retournée pour le Machine Learning SQL, en ajoutant « Hello World » comme nom de colonne et **int** comme type de données. |

La commande génère le texte suivant :

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Utiliser des entrées et des sorties

Par défaut, `sp_execute_external_script` accepte un seul jeu de données en entrée, que vous fournissez généralement sous forme de requête SQL valide. Il retourne ensuite une seule trame de données Python en sortie.

Pour le moment, utilisons les variables d’entrée et de sortie par défaut de `sp_execute_external_script` : **InputDataSet** et **OutputDataSet**.

1. Créez une petite table de données de test.

    ```sql
    CREATE TABLE PythonTestData (col1 INT NOT NULL)
    
    INSERT INTO PythonTestData
    VALUES (1);
    
    INSERT INTO PythonTestData
    VALUES (10);
    
    INSERT INTO PythonTestData
    VALUES (100);
    GO
    ```

1. Utilisez l’instruction `SELECT` pour interroger la table.
  
    ```sql
    SELECT *
    FROM PythonTestData
    ```

    **Résultats**

    ![Contenu de la table PythonTestData](./media/select-pythontestdata.png)

1. Exécutez le script Python suivant. Il récupère les données de la table en utilisant l’instruction `SELECT`, les transmet via le runtime Python et retourne les données sous forme de trame de données. La clause `WITH RESULT SETS` définit le schéma de la table de données retournée pour SQL Server, ajoutant le nom de colonne *NewColName*.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'OutputDataSet = InputDataSet;'
        , @input_data_1 = N'SELECT * FROM PythonTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Résultats**

    ![Sortie du script Python qui retourne des données d’une table](./media/python-output-pythontestdata.png)

1. À présent, renommez les variables d’entrée et de sortie. Les variables d’entrée et de sortie se nomment par défaut **InputDataSet** et **OutputDataSet**. Le script suivant remplace ces noms par **SQL_in** et **SQL_out** :

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'SQL_out = SQL_in;'
        , @input_data_1 = N'SELECT 12 as Col;'
        , @input_data_1_name  = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Notez que Python respecte la casse. Les variables d’entrée et de sortie utilisées dans le script Python (**SQL_out**, **SQL_in**) doivent correspondre aux noms définis avec `@input_data_1_name` et `@output_data_1_name`, y compris la casse.

    > [!TIP]
    > Un seul jeu de données d’entrée peut être passé en tant que paramètre, et un seul jeu de données peut être renvoyé. Toutefois, vous pouvez appeler d’autres jeux de données à partir de votre code Python et retourner des sorties d’autres types en plus du jeu de données. Vous pouvez également ajouter le mot clé OUTPUT à n’importe quel paramètre pour qu’il soit retourné avec les résultats.

1. Vous pouvez aussi générer des valeurs en utilisant simplement le script Python sans données d’entrée (`@input_data_1` est vide).

    Le script suivant génère le texte « hello » et « world ».

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    import pandas as pd
    mytextvariable = pandas.Series(["hello", " ", "world"]);
    OutputDataSet = pd.DataFrame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Résultats**

    ![Résultats de la requête en utilisant @script comme entrée](./media/python-data-generated-output.png)

> [!NOTE]
> Python utilise des espaces à gauche pour regrouper les instructions. Ainsi, lorsque le script Python incorporé s’étend sur plusieurs lignes (comme dans le script précédent), n’essayez pas de mettre en retrait les commandes Python afin de les aligner sur les commandes SQL. Par exemple, ce script génère une erreur :
>
> ```sql
> EXECUTE sp_execute_external_script @language = N'Python'
>       , @script = N'
>       import pandas as pd
>       mytextvariable = pandas.Series(["hello", " ", "world"]);
>       OutputDataSet = pd.DataFrame(mytextvariable);
>       '
>       , @input_data_1 = N''
> WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
> ```

## <a name="check-python-version"></a>Vérifier la version de Python

Si vous voulez savoir quelle version de Python est installée sur votre serveur, exécutez le script suivant.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import sys
print(sys.version)
'
GO
```

La fonction Python `print` retourne la version dans la fenêtre **Messages**. Dans l’exemple de sortie ci-dessous, vous pouvez voir que la version 3.5.2 de Python est installée, dans le cas présent.

**Résultats**

```text
STDOUT message(s) from external script:
3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
```

## <a name="list-python-packages"></a>Répertorier les packages Python

Microsoft fournit un certain nombre de packages Python préinstallés avec Machine Learning Services.

Pour afficher la liste des packages Python installés avec leur version, exécutez le script suivant.

```SQL
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import pkg_resources
import pandas
dists = [str(d) for d in pkg_resources.working_set]
OutputDataSet = pandas.DataFrame(dists)
'
WITH RESULT SETS(([Package] NVARCHAR(max)))
GO
```

La liste provient de `pkg_resources.working_set` dans Python et est renvoyée à SQL en tant que trame de données.

**Résultats**

:::image type="content" source="media/python-package-list.png" alt-text="Répertorier tous les packages Python ayant été installés":::

## <a name="next-steps"></a>Étapes suivantes

Pour savoir comment utiliser des structures de données quand Python est utilisé pour l’apprentissage automatique SQL, suivez ce guide de démarrage rapide :

> [!div class="nextstepaction"]
> [Démarrage rapide : Structures de données et objets en Python](quickstart-python-data-structures.md)