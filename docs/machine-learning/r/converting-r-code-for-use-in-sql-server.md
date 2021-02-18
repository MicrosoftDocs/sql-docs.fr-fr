---
title: Convertir le code R pour SQL
description: Migrez du code R vers une procédure stockée SQL Server pour le déploiement de solutions et l’accès aux données relationnelles sur SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 0b0d3e451c3b95bfa2401f80176dbdb659f1cb86
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336283"
---
# <a name="convert-r-code-for-execution-in-sql-server-in-database-instances"></a>Convertir le code R pour l’exécuter dans les instances SQL Server (dans la base de données)
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Cet article fournit quelques conseils clés pour modifier le code R de façon à ce qu’il fonctionne dans SQL Server. 

Lorsque vous déplacez du code R à partir de R Studio ou d’un autre environnement vers SQL Server, le plus souvent, le code fonctionne sans modification supplémentaire : par exemple, si le code est simple, tel qu’une fonction qui extrait des entrées et renvoie une valeur. Il est également plus facile de copier des solutions qui utilisent les packages **RevoScaleR** ou **MicrosoftML**, qui prennent en charge l’exécution dans différents contextes d’exécution avec des modifications minimes.

Toutefois, votre code peut nécessiter des modifications substantielles si l’une des conditions suivantes s’applique :

+ Vous utilisez des bibliothèques R qui accèdent au réseau ou qui ne peuvent pas être installées sur SQL Server.
+ Le code effectue des appels distincts à des sources de données en dehors de SQL Server, telles que des feuilles de calcul Excel, des fichiers sur des partages et d’autres bases de données. 
+ Vous souhaitez exécuter le code dans le paramètre de *\@script* de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) et paramétrer la procédure stockée.
+ Votre solution d’origine comprend plusieurs étapes qui peuvent être plus efficaces dans un environnement de production si elles sont exécutées indépendamment, par exemple la préparation des données ou l’ingénierie des fonctionnalités, ainsi que la formation du modèle, le scoring ou la création de rapports.
+ Vous souhaitez optimiser les performances en modifiant les bibliothèques, grâce à une exécution parallèle ou en déléguant une partie du traitement à SQL Server. 

## <a name="step-1-plan-requirements-and-resources"></a>Étape 1. Planifiez les spécifications et les ressources

### <a name="packages"></a>.

+ Déterminez les packages nécessaires et assurez-vous qu’ils fonctionnent sur SQL Server.
 
+ Installez les packages à l’avance, dans la bibliothèque de packages par défaut utilisée par Machine Learning Services. Les bibliothèques utilisateur ne sont pas prises en charge.

### <a name="data-sources"></a>Sources de données

+ Si vous envisagez d’incorporer votre code R dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), identifiez les sources de données principales et secondaires. 

  + Les sources de données **principales** sont des jeux de données volumineux, tels que des données d’apprentissage de modèle ou des données d’entrée pour les prédictions. Prévoyez de mapper votre plus grand jeu de données au paramètre d’entrée de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

  + Les sources de données **secondaires** sont généralement des jeux de données plus petits, tels que des listes de facteurs ou des variables de regroupement supplémentaires. 
  
  Actuellement, sp_execute_external_script ne prend en charge qu’un seul jeu de données comme entrée de la procédure stockée. Toutefois, vous pouvez ajouter plusieurs entrées scalaires ou binaires.

  Vous ne pouvez pas utiliser des appels de procédure stockée précédés de l’instruction EXECUTE comme entrée de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Vous pouvez utiliser des requêtes, des affichages ou toute autre instruction SELECT valide.

+ Déterminez les sorties dont vous avez besoin. Si vous exécutez du code R à l’aide de sp_execute_external_script, la procédure stockée peut générer une seule trame de données comme résultat. Toutefois, vous pouvez également générer plusieurs sorties scalaires, y compris des tracés et des modèles au format binaire, ainsi que d’autres valeurs scalaires dérivées du code R ou des paramètres SQL.

### <a name="data-types"></a>Types de données

+ Dressez une liste des problèmes possibles liés aux types des données.

  Tous les types de données R sont pris en charge par SQL Server Machine Learning Services. Du fait que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge davantage de types de données que R, des conversions de types de données sont parfois effectuées de façon implicite quand vous transférez des données entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et R. Vous devrez peut-être effectuer explicitement des opérations de cast ou de conversion sur certaines données. 

  Les valeurs NULL sont prises en charge. Néanmoins, R utilise la construction de données `na` pour représenter une valeur manquante, comme pour une valeur NULL.

+ Envisagez d’éliminer la dépendance vis-à-vis des données qui ne peuvent pas être utilisées par R : par exemple, les types de données rowid et GUID de SQL Server ne peuvent pas être consommés par R et génèrent des erreurs.

  Pour plus d’informations, consultez [Types de données et bibliothèques R](../r/r-libraries-and-data-types.md).

## <a name="step-2-convert-or-repackage-code"></a>Étape 2. Convertir ou reconditionner du code

L’étendue de la modification de votre code varie selon que vous envisagez de soumettre le code R à partir d’un client distant pour qu’il s’exécute dans le contexte de calcul SQL Server, ou de déployer le code dans le cadre d’une procédure stockée, ce qui peut améliorer les performances et la sécurité des données. L’inclusion de votre code dans un wrapper dans une procédure stockée impose des exigences supplémentaires. 

+ Dans la mesure du possible, définissez vos données d’entrée principales en tant que requête SQL, afin d’éviter le déplacement des données.

+ Lors de l’exécution de R dans une procédure stockée, vous pouvez passer par plusieurs entrées **scalaires**. Pour tous les paramètres que vous souhaitez utiliser dans la sortie, ajoutez le mot clé **OUTPUT**. 

  Par exemple, l’entrée scalaire suivante `@model_name` contient le nom du modèle, qui est également généré dans sa propre colonne dans les résultats :

  ```sql
  EXEC sp_execute_external_script @model_name="DefaultModel" OUTPUT, @language=N'R', @script=N'R code here'
  ```

+ Toutes les variables que vous passez comme paramètres de la procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) doivent être mappées à des variables dans le code R. Par défaut, les variables sont mappées par nom.

  Toutes les colonnes dans le jeu de données d’entrée doivent également être mappées à des variables dans le script R.  Par exemple, supposez que votre script R contient une formule comme celle-ci :

  ```R
  formula <- ArrDelay ~ CRSDepTime + DayOfWeek + CRSDepHour:DayOfWeek
  ```
  
  Une erreur se produit si le jeu de données d’entrée ne contient pas de colonnes avec les noms correspondants ArrDelay, CRSDepTime, DayOfWeek, CRSDepHour et DayOfWeek.

+ Dans certains cas, un schéma de sortie doit être défini à l’avance pour les résultats.

  Par exemple, pour insérer les données dans une table, vous devez utiliser la clause **WITH RESULT SET** pour spécifier le schéma.

  Le schéma de sortie est également requis si le script R utilise l’argument `@parallel=1`. Cela s’explique par le fait que plusieurs processus peuvent être créés par SQL Server pour exécuter la requête en parallèle et collecter les résultats à la fin. Le schéma de sortie doit donc être préparé avant la création des processus parallèles.
  
  Dans d’autres cas, vous pouvez omettre le schéma de résultat en utilisant l’option **WITH RESULT SETS UNDEFINED**. Cette instruction renvoie le jeu de données à partir du script R sans nommer les colonnes ni spécifier les types de données SQL.

+ Envisagez de générer des données de minutage ou de suivi à l’aide de T-SQL plutôt que de R.

  Par exemple, vous pouvez transmettre l’heure système ou d’autres informations utilisées pour l’audit et le stockage en ajoutant un appel T-SQL qui est transmis aux résultats, plutôt que de générer des données similaires dans le script R. 

### <a name="improve-performance-and-security"></a>Améliorer les performances et la sécurité

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ Évitez d’écrire des prédictions ou des résultats intermédiaires dans un fichier. Écrivez plutôt les prédictions dans une table, afin d’éviter le déplacement des données.
::: moniker-end

+ Exécutez toutes les requêtes à l’avance, et passez en revue les plans de requête SQL Server pour identifier les tâches qui peuvent être effectuées en parallèle.

  Si la requête d’entrée peut être parallélisée, définissez `@parallel=1` dans les arguments de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). 

  Le traitement parallèle avec cet indicateur est généralement possible quand SQL Server peut utiliser des tables partitionnées ou distribuer une requête entre plusieurs processus et agréger les résultats à la fin. En revanche, il n’est généralement pas possible si vous effectuez l’apprentissage de modèles à l’aide d’algorithmes qui nécessitent la lecture de toutes les données, ou si vous avez besoin de créer des agrégats.

+ Examinez votre code R pour déterminer si certaines étapes peuvent être effectuées séparément, ou de manière plus efficace, à l’aide d’un appel de procédure stockée distinct. Par exemple, vous pouvez atteindre de meilleures performances en effectuant l’ingénierie ou l’extraction des fonctionnalités séparément et en enregistrant les valeurs dans une table.

+ Recherchez des moyens d’utiliser T-SQL plutôt que du code R pour les calculs basés sur un jeu.

  ::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
  Par exemple, cette solution R montre comment les fonctions T-SQL définies par l’utilisateur et R peuvent effectuer la même tâche d’ingénierie des fonctionnalités : [Procédure pas à pas pour une solution complète de science des données](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md).
  ::: moniker-end

+ Si possible, remplacez les fonctions R standard par des fonctions **ScaleR** qui prennent en charge l’exécution distribuée. Pour plus d’informations, consultez [Comparaison des fonctions dans Base R et Scale R](/machine-learning-server/r-reference/revoscaler/revoscaler-compared-to-base-r).

+ Consultez un développeur de base de données pour déterminer les moyens d’améliorer les performances à l’aide de fonctionnalités SQL Server telles que les [tables à mémoire optimisée](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md) ou, si vous disposez de l’édition Enterprise, [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).

## <a name="step-3-prepare-for-deployment"></a>Étape 3. Préparer le déploiement

+ Prévenez l’administrateur afin que vous puissiez installer et tester les packages avant le déploiement de votre code. 

  Dans un environnement de développement, vous pouvez éventuellement installer les packages dans votre code, mais ce n’est pas une bonne pratique dans un environnement de production. 

  Les bibliothèques utilisateur ne sont pas prises en charge, que vous utilisiez une procédure stockée ou que vous exécutiez du code R dans le contexte de calcul SQL Server.

### <a name="package-your-r-code-in-a-stored-procedure"></a>Créer un package pour votre code R dans une procédure stockée

+ Si votre code est relativement simple, vous pouvez l’incorporer dans une fonction T-SQL définie par l’utilisateur sans le modifier, comme décrit dans les exemples suivants :

  + [Ingénierie des fonctionnalités à l’aide de T-SQL et R](../tutorials/r-taxi-classification-create-features.md)

+ Si le code est plus complexe, utilisez le package R **sqlrutils** pour le convertir. Ce package est conçu pour aider les utilisateurs R expérimentés à écrire du code de procédure stockée efficace. 

  La première étape consiste à réécrire votre code R sous la forme d’une fonction unique avec des entrées et des sorties clairement définies.

  Utilisez ensuite le package **sqlrutils** pour générer l’entrée et les sorties dans le format approprié. Le package **sqlrutils** génère l’ensemble du code de la procédure stockée et peut également inscrire la procédure stockée dans la base de données. 

  Pour plus d’informations et d’exemples, consultez [sqlrutils (SQL)](ref-r-sqlrutils.md).

### <a name="integrate-with-other-workflows"></a>Intégrer à d’autres workflows

+ Tirez parti des outils T-SQL et des processus ETL. Procédez à l’ingénierie des fonctionnalités, à l’extraction des fonctionnalités et au nettoyage des données à l’avance dans le cadre des workflows de données.

  Dans un environnement de développement R dédié tel que [!INCLUDE[rsql_rtvs_md](../../includes/rsql-rtvs-md.md)] ou RStudio, vous extrayez peut-être des données sur votre ordinateur, les analysez de manière itérative, puis écrivez ou affichez les résultats. 
  
  Quand vous migrez du code R autonome vers SQL Server, vous pouvez en grande partie simplifier ce processus ou le déléguer à d’autres outils SQL Server. 

+ Utilisez des stratégies de visualisation asynchrones et sécurisées.

  Souvent, les utilisateurs de SQL Server ne peuvent pas accéder aux fichiers sur le serveur, et les outils clients SQL ne prennent généralement pas en charge le périphérique graphique R. Si vous générez des tracés ou d’autres graphiques dans le cadre de la solution, envisagez d’exporter les tracés sous forme de données binaires et de les enregistrer dans une table, ou de les écrire.

+ Incluez les fonctions de prédiction et de scoring dans un wrapper dans des procédures stockées pour un accès direct par les applications.

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir des exemples de déploiement d’une solution R dans SQL Server, consultez les exemples suivants :

+ [Tutoriel : Développement d’un modèle prédictif en R avec le Machine Learning SQL](../tutorials/r-predictive-model-introduction.md)

+ [Didacticiel R : Prédire les tarifs des taxis de New York avec classification binaire](../tutorials/r-taxi-classification-introduction.md)

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ [Solution complète de science des données](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md) - Présente une comparaison de l’ingénierie des caractéristiques dans R et dans T-SQL
::: moniker-end
