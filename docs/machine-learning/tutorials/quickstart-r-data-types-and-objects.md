---
title: 'Démarrage rapide : Structures de données, types de données et objets R'
titleSuffix: SQL machine learning
description: Suivez ce démarrage rapide pour savoir comment utiliser des structures de données, des types de données et des objets quand R est utilisé pour l’apprentissage automatique SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e005d979e1c2b2de531979d18d2a7fc882f2b734
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339628"
---
# <a name="quickstart-data-structures-data-types-and-objects-using-r-with-sql-machine-learning"></a>Démarrage rapide : Structure de données, types de données et objets en utilisant R avec l’apprentissage automatique SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Suivez ce démarrage rapide pour savoir comment utiliser des structures de données quand R est utilisé dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) ou sur des [clusters Big Data](../../big-data-cluster/machine-learning-services.md). Vous en apprendrez plus sur le transfert de données entre R et SQL Server, ainsi que les problèmes courants qui peuvent se produire.
::: moniker-end
::: moniker range="=sql-server-2017"
Suivez ce démarrage rapide pour savoir comment utiliser des structures de données quand R est utilisé dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Vous en apprendrez plus sur le transfert de données entre R et SQL Server, ainsi que les problèmes courants qui peuvent se produire.
::: moniker-end
::: moniker range="=sql-server-2016"
Suivez ce démarrage rapide pour savoir comment utiliser des structures de données quand R est utilisé dans [SQL Server R Services](../r/sql-server-r-services.md). Vous en apprendrez plus sur le transfert de données entre R et SQL Server, ainsi que les problèmes courants qui peuvent se produire.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Dans ce démarrage rapide, vous allez apprendre à utiliser des structures de données et des types de données en R dans [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview). Vous découvrirez comment déplacer des données entre R et SQL Managed Instance, ainsi que les problèmes courants qui peuvent se produire.
::: moniker-end

Voici les problèmes courants à connaître :

- Les types de données ne correspondent pas toujours
- Des conversions implicites peuvent avoir lieu
- Des opérations de transtypage et de conversion sont parfois nécessaires
- R et SQL utilisent différents objets de données

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

## <a name="always-return-a-data-frame"></a>Toujours retourner une trame de données

Quand votre script retourne les résultats de R vers SQL Server, il doit retourner les données sous forme de trame de données (objet **data.frame**). Tout autre type d’objet que vous générez dans votre script (liste, facteur, vecteur ou données binaires) doit être converti en trame de données si vous souhaitez retourner la sortie de l’objet dans les résultats de la procédure stockée. Heureusement, plusieurs fonctions R prennent en charge la transformation d’autres objets en data frame. Vous pouvez même sérialiser un modèle binaire et le retourner dans une trame de données, ce que vous ferez plus tard dans ce guide de démarrage rapide.

Tout d’abord, vous allez expérimenter certains objets R de base, notamment les vecteurs, matrices et listes, pour voir de quelle manière leur conversion en trame de données change la sortie passée à SQL Server.

Comparez ces deux scripts « Hello World » dans R. Les scripts semblent presque identiques, mais le premier retourne une seule colonne de trois valeurs, tandis que le second retourne trois colonnes avec une seule valeur chacune.

**Exemple 1**

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
     , @script = N' mytextvariable <- c("hello", " ", "world");
       OutputDataSet <- as.data.frame(mytextvariable);'
     , @input_data_1 = N' ';
```

**Exemple 2**

```sql
EXECUTE sp_execute_external_script
        @language = N'R'
      , @script = N' OutputDataSet<- data.frame(c("hello"), " ", c("world"));'
      , @input_data_1 = N'  ';
```

## <a name="identify-schema-and-data-types"></a>Identifier les types de schéma et de données

Pourquoi les résultats sont-ils si différents ?

La réponse peut généralement être trouvée à l’aide de la commande R `str()`. Ajoutez la fonction `str(object_name)` n’importe où dans votre script R pour que le schéma de données de l’objet R spécifié soit retourné en tant que message d’information.

Afin de déterminer pourquoi les résultats des exemples 1 et 2 sont si différents, insérez la ligne `str(OutputDataSet)` à la fin de la définition de la variable `@script` dans chaque instruction, comme ceci :

**Exemple 1 avec la fonction str ajoutée**

```sql
EXECUTE sp_execute_external_script
        @language = N'R'
      , @script = N' mytextvariable <- c("hello", " ", "world");
      OutputDataSet <- as.data.frame(mytextvariable);
      str(OutputDataSet);'
      , @input_data_1 = N'  '
;
```

**Exemple 2 avec la fonction str ajoutée**

```sql
EXECUTE sp_execute_external_script
  @language = N'R', 
  @script = N' OutputDataSet <- data.frame(c("hello"), " ", c("world"));
    str(OutputDataSet);' , 
  @input_data_1 = N'  ';
```

Maintenant, passez en revue le texte dans **Messages** afin de voir pourquoi la sortie est différente.

**Résultats de l’exemple 1**

```sql
STDOUT message(s) from external script:
'data.frame':   3 obs. of  1 variable:
$ mytextvariable: Factor w/ 3 levels " ","hello","world": 2 1 3
```

**Résultats de l’exemple 2**

```sql
STDOUT message(s) from external script:
'data.frame':   1 obs. of  3 variables:
$ c..hello..: Factor w/ 1 level "hello": 1
$ X...      : Factor w/ 1 level " ": 1
$ c..world..: Factor w/ 1 level "world": 1
```

Comme vous pouvez le voir, une légère modification de la syntaxe de R a un effet considérable sur le schéma des résultats. Nous n’allons pas en expliquer la raison ici, mais les différences entre les types de données R sont décrites en détail dans la section *Data Structures* dans [Advanced R](http://adv-r.had.co.nz), par Hadley Wickham.

Pour le moment, ayez seulement à l’esprit que vous devez vérifier les résultats attendus quand vous convertissez de force des objets R en data frames.

> [!TIP]
> Vous pouvez également utiliser des fonctions d’identité R, telles que `is.matrix`, `is.vector`, pour retourner des informations sur la structure de données interne.

## <a name="implicit-conversion-of-data-objects"></a>Conversion implicite d’objets de données

Chaque objet de données R a ses propres règles de gestion des valeurs lorsqu’elles sont combinées avec d’autres objets de données si les deux objets de données ont le même nombre de dimensions, ou si un objet de données contient des types de données hétérogènes.

Créez une petite table de données de test.

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

Par exemple, supposez que vous exécutez l’instruction suivante pour effectuer une multiplication de matrice à l’aide de R. Vous multipliez une matrice à une seule colonne de trois valeurs par un tableau de quatre valeurs, et attendez comme résultat une matrice contenant quatre colonnes de trois valeurs.

```sql
EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'
        x <- as.matrix(InputDataSet);
        y <- array(12:15);
    OutputDataSet <- as.data.frame(x %*% y);'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
    WITH RESULT SETS (([Col1] int, [Col2] int, [Col3] int, Col4 int));
```

En coulisses, la colonne de trois valeurs est convertie en matrice à colonne unique. Comme une matrice n’est qu’un cas de tableau spécial dans R, le tableau `y` fait implicitement l’objet d’une conversion forcée en matrice à colonne unique pour rendre les deux arguments conformes.

**Résultats**

|Col1|Col2|Col3|Col4|
|---|---|---|---|
|12|13|14|15|
|120|130|140|150|
|1200|1 300|1400|1500|

Toutefois, observez ce qui se passe quand vous modifiez la taille du tableau `y`.

```sql
execute sp_execute_external_script
   @language = N'R'
   , @script = N'
        x <- as.matrix(InputDataSet);
        y <- array(12:14);
   OutputDataSet <- as.data.frame(y %*% x);'
   , @input_data_1 = N' SELECT [Col1]  from RTestData;'
   WITH RESULT SETS (([Col1] int ));
```

Maintenant, R retourne une valeur unique comme résultat.

**Résultats**

|Col1|
|---|
|1 542|

Pourquoi ? Dans ce cas, les deux arguments peuvent être traités comme des vecteurs de même longueur, ce qui permet à R de retourner le produit interne sous forme de matrice.  C’est le comportement attendu selon les règles de l’algèbre linéaire, mais cela peut poser problème si votre application en aval s’attend à ce que le schéma de sortie reste toujours le même !

> [!TIP]
>
> Vous obtenez des erreurs ? Vérifiez que vous exécutez la procédure stockée dans le contexte de la base de données qui contient la table, et non dans **maître** ou une autre base de données.
>
> Nous vous suggérons également d’éviter d’utiliser des tables temporaires pour ces exemples. Certains clients R mettent fin à une connexion entre les lots, ce qui supprime les tables temporaires.

## <a name="merge-or-multiply-columns-of-different-length"></a>Fusionner ou multiplier des colonnes de longueur différente

R offre une grande flexibilité dans l’utilisation de vecteurs de tailles différentes et la combinaison des structures en colonnes en trames de données. Les listes de vecteurs peuvent ressembler à un tableau, mais elles ne suivent pas toutes les règles qui régissent les tables de base de données.

Par exemple, le script suivant définit un tableau numérique de longueur 6 et le stocke dans la variable R `df1`. Le tableau numérique est ensuite combiné aux entiers de la table RTestData, qui contient trois valeurs, pour créer la trame de données `df2`.

```sql
EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'
               df1 <- as.data.frame( array(1:6) );
               df2 <- as.data.frame( c( InputDataSet , df1 ));
               OutputDataSet <- df2'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
    WITH RESULT SETS (( [Col2] int not null, [Col3] int not null ));
```

Pour remplir le data frame, R répète les éléments récupérés dans RTestData autant de fois que nécessaire pour que cela corresponde au nombre d’éléments dans le tableau `df1`.

**Résultats**

|*Col2*|*Col3*|
|----|----|
|1|1|
|10|2|
|100|3|
|1|4|
|10|5|
|100|6|

N’oubliez pas qu’une trame de données est une liste de vecteurs, même si elle a l’apparence d’une table.

## <a name="cast-or-convert-data"></a>Conversion ou forçage de type

R et SQL Server n’utilisent pas les mêmes types de données. Quand vous exécutez une requête dans SQL Server pour obtenir des données que vous voulez ensuite passer au runtime R, un type de conversion implicite a généralement lieu. D’autres conversions sont effectuées quand vous retournez les données de R vers SQL Server.

- SQL Server transfère les données de la requête au processus R géré par le service Launchpad, et les convertit en représentation interne pour une plus grande efficacité.
- Le runtime R charge les données dans une variable data.frame et effectue ses propres opérations sur ces données.
- Le moteur de base de données retourne les données à SQL Server par le biais d’une connexion sécurisée interne et présente les données en termes de types de données SQL Server.
- Vous obtenez les données en vous connectant à SQL Server à l’aide d’une bibliothèque cliente ou réseau qui peut envoyer des requêtes SQL et gérer des jeux de données tabulaires. Cette application cliente peut potentiellement affecter les données d’une autre façon.

Pour voir comment cela fonctionne, exécutez une requête telle que celle-ci sur l’entrepôt de données [AdventureWorksDW](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks). Cette vue retourne des données de ventes utilisées pour créer des prévisions.

```sql
USE AdventureWorksDW
GO

SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = 'M200 Europe'
           ORDER BY ReportingDate ASC
```

> [!NOTE]
>
> Vous pouvez utiliser n’importe quelle version d’AdventureWorks ou créer une requête différente à l’aide d’une de vos bases de données. Le but est de manipuler des données contenant du texte, des valeurs date/heure et des nombres.

À présent, essayez de coller cette requête comme entrée de la procédure stockée.

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
      , @script = N' str(InputDataSet);
      OutputDataSet <- InputDataSet;'
      , @input_data_1 = N'
           SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = ''M200 Europe''
           ORDER BY ReportingDate ASC ;'
WITH RESULT SETS undefined;
```

Si vous obtenez une erreur, vous devrez probablement apporter certaines modifications au texte de la requête. Par exemple, le prédicat de chaîne dans la clause WHERE doit être placé entre deux ensembles de guillemets simples.

Une fois la requête réussie, passez en revue les résultats de la fonction `str` pour voir comment R traite les données d’entrée.

**Résultats**

```text
STDOUT message(s) from external script: 'data.frame':    37 obs. of  3 variables:
STDOUT message(s) from external script: $ ReportingDate: POSIXct, format: "2010-12-24 23:00:00" "2010-12-24 23:00:00"
STDOUT message(s) from external script: $ ProductSeries: Factor w/ 1 levels "M200 Europe",..: 1 1 1 1 1 1 1 1 1 1
STDOUT message(s) from external script: $ Amount       : num  3400 16925 20350 16950 16950
```

- La colonne DateHeure a été traitée à l’aide du type de données R, **POSIXct**.
- La colonne texte « ProductSeries » a été identifiée comme **facteur**, autrement dit une variable de catégorie. Par défaut, les valeurs de chaîne sont gérées comme des facteurs. Si vous passez une chaîne à R, elle est convertie en entier pour un usage interne, puis mappée à la chaîne à la sortie.

### <a name="summary"></a>Résumé

À partir de ces exemples courts, vous pouvez voir la nécessité de vérifier les effets de la conversion de données lors du passage de requêtes SQL en tant qu’entrée. Certains types de données SQL Server ne sont pas pris en charge dans R. Pour éviter les erreurs d’exécution, suivez ces conseils :

- Testez vos données au préalable. Vérifiez les colonnes ou valeurs dans votre schéma qui peuvent poser problème quand elles sont passées au code R.
- Spécifiez individuellement des colonnes dans votre source de données d’entrée plutôt que d’utiliser `SELECT *`, et sachez comment chaque colonne sera traitée.
- Effectuez des conversions forcées explicites en fonction des besoins lors de la préparation de vos données d’entrée afin d’éviter les mauvaises surprises.
- Évitez de passer des colonnes de données (telles que des GUID ou des rowguid) qui provoquent des erreurs et qui ne sont pas utiles pour la modélisation.

Pour plus d’informations sur les types de données pris en charge et non pris en charge, consultez [Types de données et bibliothèques R](../r/r-libraries-and-data-types.md).

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur l’écriture de fonctions R avancées avec l’apprentissage automatique SQL, suivez ce démarrage rapide :

> [!div class="nextstepaction"]
> [Écrire des fonctions R avancées avec l’apprentissage automatique SQL](quickstart-r-functions.md)