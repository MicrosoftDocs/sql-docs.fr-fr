---
title: Réglage des performances des données
description: Cet article traite des optimisations de performances pour les scripts R ou Python qui s’exécutent dans SQL Server. Elle décrit également les méthodes que vous pouvez utiliser pour mettre à jour votre code R afin d’améliorer les performances et d’éviter les problèmes connus.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 71c8852efd538ef0a702c8f96435facf4882b7d2
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596657"
---
# <a name="performance-tuning-and-data-optimization-for-r"></a>Réglage du niveau de performance et optimisation des données pour R
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Cet article traite des optimisations de performances pour les scripts R ou Python qui s’exécutent dans SQL Server. Vous pouvez appliquer ces méthodes pour mettre à jour votre code R afin d’améliorer le niveau de performance tout en évitant les problèmes connus.

## <a name="choosing-a-compute-context"></a>Choisir un contexte de calcul

Dans SQL Server, vous pouvez utiliser le contexte de calcul **local** ou **SQL** lors de l’exécution d’un script R ou Python.

Lorsque vous utilisez le contexte de calcul **local**, l’analyse est effectuée sur votre ordinateur et non sur le serveur. Par conséquent, si vous obtenez des données de SQL Server à utiliser dans votre code, les données doivent être récupérées sur le réseau. L’impact de ce transfert réseau sur les performances est plus ou moins important selon le volume de données transférées, la vitesse du réseau et les autres transferts réseau simultanés.

Lorsque vous utilisez le **contexte de calcul SQL Server**, le code est exécuté sur le serveur. Si vous obtenez des données à partir de SQL Server, les données doivent se trouver en local sur le serveur qui exécute l’analyse pour éviter toute surcharge du réseau. Si vous devez importer des données à partir d’autres sources, envisagez d’organiser ETL au préalable.

Si vous manipulez des jeux de données volumineux, utilisez toujours le contexte de calcul SQL.

## <a name="factors"></a>Facteurs

Le langage R utilise le concept de *facteurs*, qui sont des variables spéciales pour les données de catégorie. Les scientifiques des données utilisent souvent des variables de facteur dans leurs formules, car la gestion des variables de catégorie sous forme de facteurs permet de s’assurer que les données sont traitées correctement par les fonctions de Machine Learning.

Par défaut, les variables de facteur peuvent être converties de chaînes en entiers et inversement pour le stockage ou le traitement. La fonction R `data.frame` gère toutes les chaînes sous forme de variables de facteur, à moins que l’argument *stringsAsFactors* ait la valeur **False**. Cela signifie que les chaînes sont automatiquement converties en un entier à traiter, puis de nouveau mappées à la chaîne d’origine.

Si les données sources des facteurs sont stockées sous la forme d’un entier, les performances peuvent en pâtir, car R convertit les entiers de facteur en chaînes au moment de l’exécution, puis effectue sa propre conversion de chaîne en entier en interne.

Pour éviter de telles conversions au moment de l’exécution, envisagez de stocker les valeurs sous forme d’entiers dans la table SQL Server et d’utiliser l’argument _colInfo_ pour spécifier les niveaux de la colonne utilisée comme facteur. La plupart des objets de source de données dans RevoScaleR prennent le paramètre _colInfo_. Ce paramètre permet de nommer les variables utilisées par la source de données, de spécifier leur type et de définir les niveaux de variables ou les transformations sur les valeurs de colonne.

Par exemple, l’appel de fonction R suivant obtient les entiers 1, 2 et 3 d’une table, mais mappe les valeurs à un facteur avec les niveaux « Apple », « orange » et « Banana ».

```R
c("fruit" = c(type = "factor", levels=as.character(c(1:3)), newLevels=c("apple", "orange", "banana")))
```

Lorsque la colonne source contient des chaînes, il est toujours plus efficace de spécifier les niveaux à l’avance à l’aide du paramètre _colInfo_. Par exemple, le code R suivant traite les chaînes comme des facteurs tels qu’ils sont lus.

```R
c("fruit" = c(type = "factor", levels= c("apple", "orange", "banana")))
```

S’il n’y a pas de différence sémantique dans la génération du modèle, cette approche peut améliorer les performances.

## <a name="data-transformations"></a>Transformations de données

Les « scientifiques des données » utilisent souvent des fonctions de transformation écrites en langage R pour leurs analyses. La fonction de transformation doit être appliquée à chaque ligne récupérée de la table. Dans SQL Server, ces transformations sont appliquées à toutes les lignes récupérées dans un lot, ce qui nécessite une communication entre l’interpréteur R et le moteur d’analyse. Les données sont transférées de SQL vers le moteur analytique, puis vers le processus de l’interpréteur R, avant d’être renvoyées.

Pour cette raison, selon le volume de données transférées, l’utilisation de transformations dans le cadre de votre code R peut donc diminuer les performances de l’algorithme de manière significative.

Vous avez plutôt intérêt à ajouter toutes les colonnes nécessaires à la table ou vue avant d’effectuer des analyses, et à éviter les transformations pendant le calcul. Si vous ne pouvez pas ajouter de colonnes aux tables existantes, créez une autre table ou vue contenant les colonnes transformées, puis utilisez une requête appropriée pour récupérer les données.

## <a name="batch-row-reads"></a>Lectures de lignes par lot

Si vous utilisez une source de données SQL Server (`RxSqlServerData`) dans votre code, nous vous recommandons d’essayer d’utiliser le paramètre _rowsPerRead_ pour spécifier la taille du lot. Ce paramètre définit le nombre de lignes interrogées, puis envoyées au script externe pour traitement. Au moment de l’exécution, l’algorithme lit uniquement le nombre de lignes spécifié dans chaque lot.

La possibilité de contrôler la quantité de données traitées à la fois peut vous aider à résoudre ou à éviter les problèmes. Par exemple, si votre jeu de données d’entrée est très large (comporte de nombreuses colonnes), ou si le jeu de données comporte quelques colonnes de grande taille (par exemple, du texte libre), vous pouvez réduire la taille du lot afin d’éviter la pagination des données en mémoire.

Par défaut, la valeur de ce paramètre est définie sur 50000 pour garantir des performances correctes, y compris sur les machines avec une mémoire insuffisante. Si le serveur a suffisamment de mémoire disponible, vous pouvez augmenter cette valeur à 500 000, voire à un million, pour améliorer les performances, notamment pour les tables volumineuses.

Les avantages de l’amélioration de la taille des lots deviennent évidents sur un jeu de données volumineux et dans une tâche qui peut s’exécuter sur plusieurs processus. Toutefois, l’augmentation de cette valeur ne produit pas toujours les meilleurs résultats. Nous vous recommandons d’expérimenter vos données et algorithmes pour déterminer la valeur optimale.

## <a name="parallel-processing"></a>Traitement parallèle

Pour améliorer les performances de fonctions analytiques **RX**, vous pouvez tirer parti de SQL Server pour exécuter des tâches en parallèle à l’aide de cœurs disponibles sur l’ordinateur serveur.

Il existe deux méthodes de parallélisation avec R dans SQL Server :

+ **Use \@parallel.** Si vous utilisez la procédure stockée `sp_execute_external_script` pour exécuter un script R, définissez le paramètre `@parallel` à `1`. Il s’agit de la meilleure méthode si votre script R n’**utilise** pas les fonctions RevoScaleR, qui ont d’autres mécanismes de traitement. Si votre script utilise des fonctions RevoScaleR (portant généralement le préfixe « RX »), le traitement parallèle est effectué automatiquement et vous n’avez pas besoin de définir explicitement `@parallel` sur `1`.

  Si le script R et la requête SQL peuvent être tous deux parallélisés, le moteur de base de données crée plusieurs processus parallèles. Le nombre maximal de processus qui peuvent être créés est égal au paramètre **maximum degree of parallelism** (MAXDOP) de l’instance. Tous les processus exécutent ensuite le même script, mais ne reçoivent qu’une partie des données.
  
  Par conséquent, cette méthode n’est pas appropriée pour les scripts qui ont besoin de voir toutes les données, par exemple, lors de l’apprentissage d’un modèle. Au contraire, cette méthode est utile pour effectuer certaines tâches, comme la prédiction par lot en parallèle. Pour plus d’informations sur l’utilisation du parallélisme avec `sp_execute_external_script`, consultez la section **Advanced tips: parallel processing (Conseils avancés : traitement parallèle)** dans [Using R Code in Transact-SQL (Utilisation du code R dans Transact-SQL)](../tutorials/quickstart-r-create-script.md).

+ **Use numTasks =1.** Lorsque vous utilisez les fonctions **RX** dans un contexte de calcul SQL Server, définissez la valeur du paramètre _numTasks_ sur le nombre de processus que vous souhaitez créer. Le nombre de processus créés ne peut jamais être supérieur à **MAXDOP**. Toutefois, le nombre réel de processus créés est déterminé par le moteur de base de données et peut être inférieur à celui que vous avez demandé.

  Si le script R et la requête SQL peuvent être tous deux parallélisés, le serveur SQL crée plusieurs processus parallèles au moment de l’exécution des fonctions rx. Le nombre réel de processus créés dépend de plusieurs facteurs, notamment la gouvernance des ressources, l’utilisation actuelle des ressources, l’existence d’autres sessions et le plan d’exécution de la requête utilisée avec le script R.

## <a name="query-parallelization"></a>Parallélisation de la requête

Dans Microsoft R, vous pouvez travailler avec des sources de données SQL Server en définissant vos données en tant qu’objet de source de données RxSqlServerData.

Crée une source de données basée sur une table ou une vue entière :

```R
RxSqlServerData(table= "airline", connectionString = sqlConnString)
```

Crée une source de données basée sur une requête SQL :

```R
RxSqlServerData(sqlQuery= "SELECT [ArrDelay],[CRSDepTime],[DayOfWeek] FROM  airlineWithIndex WHERE rowNum <= 100000", connectionString = sqlConnString)
```

> [!NOTE]
> Si une table est spécifiée dans la source de données à la place d’une requête, R Services utilise des données heuristiques internes pour déterminer les colonnes à récupérer de la table. Toutefois, cette approche est peu compatible avec l’exécution parallèle.

Pour permettre l’analyse parallèle des données, la requête utilisée pour récupérer les données doit être formée de manière à rendre possible la création d’une requête parallèle par le moteur de base de données. Si le code ou l’algorithme utilise de grands volumes de données, assurez-vous que la requête donnée à `RxSqlServerData` est optimisée pour une exécution en parallèle. Une requête qui ne retourne pas de plan d’exécution parallèle peut générer un processus unique pour le calcul.

Si vous avez besoin de travailler avec des jeux de données volumineux, utilisez Management Studio ou un autre analyseur de requêtes SQL avant d’exécuter votre code R pour analyser le plan d’exécution. Ensuite, prenez les mesures recommandées pour améliorer les performances de la requête. Par exemple, l’absence d’un index sur une table peut faire augmenter le temps d’exécution d’une requête. Pour plus d’informations, consultez [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md).

Une autre erreur courante pouvant entraîner une baisse des performances est la récupération d’un nombre de colonnes plus important que nécessaire par la requête. Par exemple, si une formule est basée sur trois colonnes seulement, mais que votre table source comporte 30 colonnes, vous déplacez des données inutilement.

+ Évitez d’utiliser `SELECT *` !
+ Prenez le temps de passer en revue les colonnes du jeu de données et d’identifier uniquement celles qui sont nécessaires à l’analyse.
+ Supprimez de vos requêtes toutes les colonnes qui contiennent des types de données qui ne sont pas compatibles avec le code R, comme les GUID et rowguids
+ Vérifier les formats de date et d’heure non pris en charge
+ Au lieu de charger une table, créez une vue qui sélectionne certaines valeurs ou convertit des colonnes pour éviter les erreurs de conversion

## <a name="optimizing-the-machine-learning-algorithm"></a>Optimisation de l’algorithme de Machine Learning

Cette section fournit des conseils et des ressources divers propres à RevoScaleR et à d’autres options de Microsoft R.

> [!TIP]
> L’optimisation de R de manière générale n’est pas abordée dans cet article. Toutefois, si vous avez besoin d’accélérer votre code, nous vous recommandons l’article le plus populaire intitulé [The R Inferno](https://www.burns-stat.com/pages/Tutor/R_inferno.pdf). Il aborde les constructions de programmation en R et les pièges courants de manière concrète, et fournit de nombreux exemples spécifiques de techniques de programmation en R.

### <a name="optimizations-for-revoscaler"></a>Optimisations pour RevoScaleR

De nombreux algorithmes d’apprentissage RevoScaleR prennent en charge l’utilisation de paramètres pour déterminer la façon dont le modèle d’apprentissage est généré. Les performances de l’algorithme constituent un critère tout aussi important que la précision et l’exactitude du modèle. Pour trouver le bon équilibre entre l’exactitude et la durée d’apprentissage, vous pouvez modifier les paramètres d’apprentissage du modèle pour augmenter la vitesse de calcul et, dans la plupart des cas, améliorer les performances sans nuire à la précision et l’exactitude du modèle.

+ [rxDTree](/r-server/r-reference/revoscaler/rxdtree)

  `rxDTree` prend en charge le paramètre `maxDepth` qui détermine la profondeur de l’arbre de décision. L’augmentation de la valeur de `maxDepth` peut entraîner une baisse des performances. Il est donc important d’évaluer l’effet d’une augmentation de la profondeur sur les performances.

  Vous pouvez également régler certains paramètres tels que `maxNumBins`, `maxDepth`, `maxComplete` et `maxSurrogate` pour parvenir à un équilibre entre la complexité en temps et la précision de la prédiction. L’augmentation de la profondeur au-delà de 10 ou 15 peut rendre le calcul très coûteux.

+ [rxLinMod](/r-server/r-reference/revoscaler/rxlinmod)

  L’argument `cube` peut être utilisé si la première variable dépendante de la formule est une variable de facteur.
  
  Si `cube` est défini sur `TRUE`, la régression s’effectue sur le modèle de régression inverse partitionnée, qui peut être plus rapide et consommer moins de mémoire que le calcul de régression standard. Si la formule contient un grand nombre de variables, le gain de performance peut être significatif.

+ [rxLogit](/r-server/r-reference/revoscaler/rxlogit)

  L’argument `cube` peut être utilisé si la première variable dépendante est une variable de facteur.
  
  Lorsque `cube` est défini sur `TRUE`, l’algorithme utilise un inverse partitionné, ce qui peut être plus rapide et consommer moins de mémoire. Si la formule contient un grand nombre de variables, le gain de performance peut être significatif.

Pour plus d’informations sur l’optimisation de RevoScaleR, consultez les articles suivants :

+ Article de support : [Options de réglage des performances pour rxDForest et rxDTree](https://support.microsoft.com/kb/3104235)

+ Méthodes de contrôle de l’ajustement du modèle dans un modèle d’arborescence augmentée : [Estimation des modèles à l’aide de l’accélération de gradient stochastique](/r-server/r/how-to-revoscaler-boosting)

+ Vue d’ensemble de la façon dont RevoScaleR déplace et traite les données : [Écrire des algorithmes de segmentation personnalisés dans ScaleR](/r-server/r/how-to-developer-write-chunking-algorithms)

+ Modèle de programmation pour RevoScaleR : [Gestion des threads dans RevoScaleR](/r-server/r/how-to-developer-manage-threads)

+ Référence de fonction pour [rxDForest](/r-server/r-reference/revoscaler/rxdforest)

+ Référence de fonction pour [rxDForest](/r-server/r-reference/revoscaler/rxbtrees)

### <a name="use-microsoftml"></a>Utiliser MicrosoftML

Nous vous recommandons également d’examiner le nouveau package **MicrosoftML**, qui fournit des algorithmes de Machine Learning évolutifs qui peuvent utiliser les contextes de calcul et les transformations fournis par RevoScaleR.

+ [Prise en main de MicrosoftML](/r-server/r/concept-what-is-the-microsoftml-package)

+ [Comment choisir un algorithme MicrosoftML](/r-server/r/how-to-choose-microsoftml-algorithms-cheatsheet)

## <a name="next-steps"></a>Étapes suivantes

+ Pour connaître les fonctions R que vous pouvez utiliser pour améliorer le niveau de performance de votre code R, consultez [Amélioration du niveau de performance avec les fonctions de profilage du code R](using-r-code-profiling-functions.md).

+ Pour plus d’informations sur le réglage du niveau de performance dans SQL Server, consultez [Centre de performances pour le Moteur de base de données SQL Server et Azure SQL Database](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md).