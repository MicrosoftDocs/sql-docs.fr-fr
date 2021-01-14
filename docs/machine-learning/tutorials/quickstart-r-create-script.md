---
title: 'Démarrage rapide : Exécuter des scripts R'
titleSuffix: SQL machine learning
description: Exécutez un ensemble de scripts R simples avec le Machine Learning SQL. Découvrez comment utiliser la procédure stockée sp_execute_external_script pour exécuter le script.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 540804d2a35758627b24586ec8c29ca2dbab6e5f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101922"
---
# <a name="quickstart-run-simple-r-scripts-with-sql-machine-learning"></a>Démarrage rapide : Exécuter des scripts R simples avec le Machine Learning SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans ce démarrage rapide, vous allez exécuter un ensemble de scripts R simples en utilisant [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) ou sur des [clusters Big Data](../../big-data-cluster/machine-learning-services.md). Vous allez découvrir comment utiliser la procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) pour exécuter ce script dans une instance SQL Server.
::: moniker-end
::: moniker range="=sql-server-2017"
Dans ce démarrage rapide, vous allez exécuter un ensemble de scripts R simples en utilisant [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Vous allez découvrir comment utiliser la procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) pour exécuter ce script dans une instance SQL Server.
::: moniker-end
::: moniker range="=sql-server-2016"
Dans ce démarrage rapide, vous allez exécuter un ensemble de scripts R simples en utilisant [SQL Server R Services](../r/sql-server-r-services.md). Vous allez découvrir comment utiliser la procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) pour exécuter ce script dans une instance SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans ce démarrage rapide, vous allez exécuter un ensemble de scripts R simples avec [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview). Vous découvrirez comment utiliser la procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) pour exécuter le script dans votre base de données.
::: moniker-end

## <a name="prerequisites"></a>Prérequis

Pour effectuer ce démarrage rapide, vous avez besoin de ce qui suit.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
- SQL Server Machine Learning Services. Pour installer Machine Learning Services, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md) ou le [Guide d’installation Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). Vous pouvez également [activer Machine Learning Services sur des clusters Big Data SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
- SQL Server Machine Learning Services. Pour installer Machine Learning Services, consultez le [Guide d’installation Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
- SQL Server 2016 R Services. Pour installer R Services, consultez le [Guide d’installation Windows](../install/sql-r-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
- Azure SQL Managed Instance Machine Learning Services. Pour plus d’informations, consultez [Présentation de Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

- Un outil pour exécuter les requêtes SQL qui contiennent des scripts R. Ce guide de démarrage rapide utilise [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="run-a-simple-script"></a>Exécuter un script simple

Pour exécuter un script R, vous allez le transmettre en tant qu’argument de la procédure stockée système [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Cette procédure stockée sur le système démarre le runtime R, transmet les données à R, gère les sessions utilisateur R de manière sécurisée et retourne les résultats au client.

Dans les étapes suivantes, vous allez exécuter cet exemple de script R :

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. Ouvrez **Azure Data Studio** et connectez-vous à votre serveur.

1. Transmettez le script R complet à la procédure stockée `sp_execute_external_script`.

   Le script est transmis via l’argument `@script`. Tout ce qui se trouve dans l’argument `@script` doit être du code R valide.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. Le résultat correct est calculé et la fonction R `print` le retourne dans la fenêtre **Messages**.

   Voici comment il se présente.

    **Résultats**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Exécuter un script Hello World

Un exemple type de script est celui qui génère simplement la chaîne « Hello World ». Exécutez la commande suivante :

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Les entrées de la procédure stockée `sp_execute_external_script` sont les suivantes :

| Entrée | Description |
|-|-|
| @language | définit l’extension de langage à appeler, dans le cas présent, R |
| @script | définit les commandes transmises au runtime R. Tout votre script R doit être placé dans cet argument sous forme de texte Unicode. Vous pouvez aussi ajouter le texte à une variable de type **nvarchar**, puis appeler cette variable |
| @input_data_1 | données retournées par la requête, transmises au runtime R, qui retourne les données sous forme de trame de données |
|WITH RESULT SETS | la clause définit le schéma de la table de données retournée, ajoutant « Hello World » comme nom de colonne, **int** pour le type de données |

La commande génère le texte suivant :

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Utiliser des entrées et des sorties

Par défaut, `sp_execute_external_script` accepte un seul jeu de données en entrée, que vous fournissez généralement sous forme de requête SQL valide. Il retourne ensuite une seule trame de données R en sortie.

Pour le moment, utilisons les variables d’entrée et de sortie par défaut de `sp_execute_external_script` : **InputDataSet** et **OutputDataSet**.

1. Créez une petite table de données de test.

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    
    INSERT INTO RTestData
    VALUES (1);
    
    INSERT INTO RTestData
    VALUES (10);
    
    INSERT INTO RTestData
    VALUES (100);
    GO
    ```

1. Utilisez l’instruction `SELECT` pour interroger la table.
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **Résultats**

    ![Contenu de la table RTestData](./media/select-rtestdata.png)

1. Exécutez le script R suivant. Il récupère les données de la table en utilisant l’instruction `SELECT`, les transmet via le runtime R et retourne les données sous forme de trame de données. La clause `WITH RESULT SETS` définit le schéma de la table de données retournée pour SQL Server, ajoutant le nom de colonne *NewColName*.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Résultats**

    ![Sortie du script R qui retourne des données d’une table](./media/r-output-rtestdata.png)

1. À présent, changeons le nom des variables d’entrée et de sortie. Les variables d’entrée et de sortie se nomment par défaut **InputDataSet** et **OutputDataSet** ; ce script remplace ces noms par **SQL_in** et **SQL_out** :

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Notez que R respecte la casse. Les variables d’entrée et de sortie utilisées dans le script R (**SQL_out**, **SQL_in**) doivent correspondre aux noms définis avec `@input_data_1_name` et `@output_data_1_name`, y compris la casse.

   > [!TIP]
   > Un seul jeu de données d’entrée peut être passé en tant que paramètre, et un seul jeu de données peut être renvoyé. Toutefois, vous pouvez appeler d’autres jeux de données dans votre code R et des sorties d’autres types peuvent être renvoyées en plus du jeu de données. Vous pouvez également ajouter le mot clé OUTPUT à n’importe quel paramètre pour qu’il soit retourné avec les résultats.

1. Vous pouvez aussi générer des valeurs en utilisant simplement le script R sans données d’entrée (`@input_data_1` est vide).

   Le script suivant génère le texte « hello » et « world ».

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Résultats**

    ![Résultats de la requête en utilisant @script comme entrée](./media/r-data-generated-output.png)

## <a name="check-r-version"></a>Vérifier la version de R

Si vous souhaitez savoir quelle version de R est installée, exécutez le script suivant.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

La fonction R `print` retourne la version dans la fenêtre **Messages**. Dans l’exemple de sortie ci-dessous, vous pouvez voir que la version installée de R est dans ce cas la version 3.4.4.

**Résultats**

```text
STDOUT message(s) from external script:
                   _
platform       x86_64-w64-mingw32
arch           x86_64
os             mingw32
system         x86_64, mingw32
status
major          3
minor          4.4
year           2018
month          03
day            15
svn rev        74408
language       R
version.string R version 3.4.4 (2018-03-15)
nickname       Someone to Lean On
```

## <a name="list-r-packages"></a>Lister les packages R
::: moniker range=">=sql-server-2017"
Microsoft fournit un certain nombre de packages R préinstallés avec Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
Microsoft fournit un certain nombre de packages R préinstallés avec R Services.
::: moniker-end

Pour afficher la liste des packages R installés, dont la version, les dépendances, la licence et le chemin de la bibliothèque, exécutez le script suivant.

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

La sortie provient de `installed.packages()` dans R et est retournée sous forme de jeu de résultats.

**Résultats**

![Packages installés dans R](./media/rsql-installed-packages.png) 

## <a name="next-steps"></a>Étapes suivantes

Pour savoir comment utiliser des structures de données quand R est utilisé avec le Machine Learning SQL, suivez ce guide de démarrage rapide :

> [!div class="nextstepaction"]
> [Gérer les objets et types de données en utilisant R avec le Machine Learning SQL](quickstart-r-data-types-and-objects.md)