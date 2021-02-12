---
title: 'Exécuter des travaux : Outils de Spark & Hive pour VS Code'
titleSuffix: SQL Server Big Data Clusters
description: Envoyez un travail Spark avec l’extension Spark & Hive Tools for Visual Studio Code sur un cluster Big Data SQL Server.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f89450293597c28afb8db519f1c06d9d0b566046
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043479"
---
# <a name="submit-spark-jobs-on-sql-server-big-data-cluster-in-visual-studio-code"></a>Envoyer des travaux Spark sur un cluster Big Data SQL Server dans Visual Studio Code

Découvrez comment utiliser l’extension Spark & Hive Tools for Visual Studio Code afin de créer et d’envoyer des scripts PySpark pour Apache Spark. Nous allons tout d’abord expliquer comment installer l’extension Spark & Hive Tools dans Visual Studio Code, puis nous montrerons comment envoyer des travaux à Spark.  

L’extension Spark & Hive Tools peut être installée sur les plateformes prises en charge par Visual Studio Code, notamment Windows, Linux et macOS. Vous trouverez ci-dessous les prérequis pour les différentes plateformes.


## <a name="prerequisites"></a>Prérequis

Les éléments suivants sont requis pour effectuer les étapes décrites dans cet article :

- Cluster Big Data SQL Server. Voir [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://docs.microsoft.com/sql/big-data-cluster/big-data-cluster-overview).
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono est uniquement requis pour Linux et macOS.
- [Configurez l’environnement interactif PySpark pour Visual Studio Code](/azure/hdinsight/set-up-pyspark-interactive-environment).
- Un répertoire local nommé **SQLBDCexample**.  Cet article utilise **C:\SQLBDC\SQLBDCexample**.

## <a name="install-spark--hive-tools"></a>Installer l’extension Spark & Hive Tools

Une fois que vous satisfaites aux prérequis, vous pouvez installer l’extension Spark & Hive Tools for Visual Studio Code.  Effectuez les étapes suivantes pour installer l’extension Spark & Hive Tools :

1. Ouvrez Visual Studio Code.

2. Dans la barre de menus, accédez à **Afficher** > **Extensions**.

3. Dans la zone de recherche, entrez **Spark & Hive**.

4. Sélectionnez **Spark & Hive Tools** dans les résultats de la recherche, puis sélectionnez **Installer**.  

   ![Installer l’extension](./media/spark-hive-tools-vscode/extension.png)

5. Rechargez quand cela est nécessaire.

## <a name="open-work-folder"></a>Ouvrir le dossier de travail

Pour ouvrir un dossier de travail et créer un fichier dans Visual Studio Code, effectuez les étapes suivantes :

1. Dans la barre de menus, accédez à **Fichier** > **Ouvrir le dossier...**  > **C:\SQLBDC\SQLBDCexample**, puis sélectionnez le bouton **Sélectionner un dossier**. Le dossier s’affiche dans l’affichage **Explorateur** sur la gauche.

2. Dans la vue **Explorateur**, sélectionnez le dossier **SQLBDCexample**, puis l’icône **Nouveau fichier** en regard du dossier de travail.

   ![Nouveau fichier](./media/spark-hive-tools-vscode/new-file.png)

3. Nommez le nouveau fichier avec l’extension de fichier `.py` (script Spark).  Cet exemple utilise **HelloWorld.py**.
4. Copiez le code suivant et collez-le dans le fichier de script :
   ```python
    import sys
    from operator import add
    from pyspark.sql import SparkSession, Row
    
    spark = SparkSession\
        .builder\
        .appName("PythonWordCount")\
        .getOrCreate()
    
    data = [Row(col1='pyspark and spark', col2=1), Row(col1='pyspark', col2=2), Row(col1='spark vs hadoop', col2=2), Row(col1='spark', col2=2), Row(col1='hadoop', col2=2)]
    df = spark.createDataFrame(data)
    lines = df.rdd.map(lambda r: r[0])

    counters = lines.flatMap(lambda x: x.split(' ')) \
        .map(lambda x: (x, 1)) \
        .reduceByKey(add)
    
    output = counters.collect()
    sortedCollection = sorted(output, key = lambda r: r[1], reverse = True)
    
    for (word, count) in sortedCollection:
        print("%s: %i" % (word, count))
   ```

## <a name="link-a-sql-server-big-data-cluster"></a>Lier un cluster Big Data SQL Server

Avant de pouvoir envoyer des scripts à vos clusters à partir de Visual Studio Code, vous devez lier un cluster Big Data SQL Server.

1. Dans la barre de menus, accédez à **Afficher** > **Palette de commandes...** , puis entrez **Spark / Hive: Link a Cluster** (Spark / Hive : lier un cluster).

   ![Commande de liaison de cluster](./media/spark-hive-tools-vscode/link-cluster-command.png)

2. Sélectionnez le type de cluster lié **SQL Server Big Data** (Big Data SQL Server).

3. Entrez le point de terminaison Big Data SQL Server.

4. Entrez le nom d’utilisateur du cluster Big Data SQL Server.

5. Entrez le mot de passe de l’administrateur d’utilisateurs.

6. Définissez le nom d’affichage du cluster (facultatif).

7. Listez les clusters, puis examinez l’affichage **OUTPUT** (SORTIE).

## <a name="list-clusters"></a>Lister les clusters

1. Dans la barre de menus, accédez à **Afficher** > **Palette de commandes...** , puis entrez **Spark / Hive: List Cluster**.

2. Examinez l’affichage **OUTPUT** (SORTIE).  L’affichage montre vos clusters liés.

    ![Définir une configuration de cluster par défaut](./media/spark-hive-tools-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Définir le cluster par défaut

1. Réouvrez le dossier **SQLBDCexample** créé [précédemment](#open-work-folder) s’il est fermé.  

2. Sélectionnez le fichier **HelloWorld.py** créé [précédemment](#open-work-folder) ; il s’ouvre alors dans l’éditeur de script.

3. Si ce n’est déjà fait, liez un cluster.

4. Cliquez avec le bouton droit sur l’éditeur de script, puis sélectionnez **Spark / Hive: Set Default Cluster** (Spark / Hive : définir le cluster par défaut).   

5. Sélectionnez un cluster comme cluster par défaut pour le fichier de script actuel. Les outils mettent à jour automatiquement le fichier de configuration **.VSCode\settings.json**. 

   ![Définir une configuration de cluster par défaut](./media/spark-hive-tools-vscode/set-default-cluster-configuration.png)

## <a name="submit-interactive-pyspark-queries"></a>Envoyer des requêtes PySpark interactives

Suivez les étapes ci-dessous pour envoyer des requêtes PySpark interactives :

1. Réouvrez le dossier **SQLBDCexample** créé [précédemment](#open-work-folder) s’il est fermé.  

2. Sélectionnez le fichier **HelloWorld.py** créé [précédemment](#open-work-folder) ; il s’ouvre alors dans l’éditeur de script.

3. Si ce n’est déjà fait, liez un cluster.

4. Choisissez tout le code, cliquez avec le bouton droit sur l’éditeur de script, sélectionnez **Spark: PySpark Interactive** pour envoyer la requête, ou utilisez le raccourci **Ctrl + Alt +I**.

   ![Menu contextuel pyspark interactive](./media/spark-hive-tools-vscode/pyspark-interactive-right-click.png)

5. Sélectionnez le cluster si vous n’avez pas spécifié de cluster par défaut. Après quelques instants, les résultats **Python Interactive** s’affichent sous un nouvel onglet. Les outils vous permettent également d’envoyer un bloc de code à la place de l’intégralité du fichier de script à l’aide du menu contextuel. 

   ![Fenêtre Python interactive dans PySpark Interactive](./media/spark-hive-tools-vscode/pyspark-interactive-python-interactive-window.png) 

6. Saisissez **« %%info »** , puis appuyez sur **Maj + Entrée** pour afficher les informations sur la tâche. (facultatif)

   ![Affichage des informations sur la tâche](./media/spark-hive-tools-vscode/pyspark-interactive-view-job-information.png)

   > [!NOTE] 
   >
   > Si la case **Extension Python activée** n’est pas cochée dans les paramètres (le paramètre est activé par défaut), les résultats d’interaction PySpark envoyés utilisent l’ancienne fenêtre.
   >
   > ![Extension Python dans Pyspark Interactive désactivée](./media/spark-hive-tools-vscode/pyspark-interactive-python-extension-disabled.png)


## <a name="submit-pyspark-batch-job"></a>Envoyer le travail de traitement par lots PySpark

1. Réouvrez le dossier **SQLBDCexample** créé [précédemment](#open-work-folder) s’il est fermé.  

2. Sélectionnez le fichier **HelloWorld.py** créé [précédemment](#open-work-folder) ; il s’ouvre alors dans l’éditeur de script.

3. Si ce n’est déjà fait, liez un cluster.

4. Cliquez avec le bouton droit sur l’éditeur de script, puis sélectionnez **Spark: PySpark Batch** (Spark : traitement par lots PySpark) ou utilisez le raccourci **Ctrl + Alt + H**. 

5. Sélectionnez le cluster si vous n’avez pas spécifié de cluster par défaut. Une fois que vous avez envoyé un travail Python, les journaux d’envoi s’affichent dans la fenêtre **OUTPUT** (SORTIE) de Visual Studio Code. L’**URL de l’interface utilisateur Spark** et l’**URL de l’interface utilisateur Yarn** sont également affichées. Vous pouvez ouvrir l’URL dans un navigateur web pour suivre l’état du travail.

   ![Envoyer le résultat du travail Python](./media/spark-hive-tools-vscode/submit-pythonjob-result.png) 

## <a name="apache-livy-configuration"></a>Configuration d’Apache Livy

La configuration d’[Apache Livy](https://livy.incubator.apache.org/) est prise en charge. Elle peut être définie dans le fichier **.VSCode\settings.json** dans le dossier de l’espace de travail. Actuellement, la configuration Livy prend uniquement en charge le script Python. Pour plus d’informations, consultez le fichier [Lisez-moi de Livy](https://github.com/cloudera/livy/blob/master/README.rst ).

### <a name="how-to-trigger-livy-configuration"></a><a id="triggerlivyconf"></a>**Comment déclencher une configuration Livy**

#### <a name="method-1"></a>Méthode 1

1. Dans la barre de menus, accédez à **Fichier** > **Préférences** > **Paramètres**.  
2. Dans la zone de texte **Paramètres de recherche**, entrez **HDInsight Job Sumission: Livy Conf** (Envoi de travail HDInsight : configuration de Livy).  
3. Sélectionnez **Modifier dans settings.json** pour le résultat de recherche approprié.

#### <a name="method-2"></a>Méthode 2

Envoyez un fichier ; notez que le dossier `.vscode` est ajouté automatiquement au dossier de travail. Vous pouvez trouver la configuration de Livy en cliquant sur `.vscode\settings.json`.

+ Paramètres du projet :

    ![Configuration de Livy](./media/spark-hive-tools-vscode/hdi-livyconfig.png)

>[!NOTE]
>Pour les paramètres **driverMomory** et **executorMomry**, définissez la valeur avec l’unité, par exemple 1g ou 1024m. 

### <a name="supported-livy-configurations"></a>Configurations de Livy prises en charge

#### <a name="post-batches"></a>POST /batches

**Corps de la demande**

| name | description | type |
| :- | :- | :- |
| fichier | Fichier contenant l’application à exécuter | Chemin (obligatoire) |
| proxyUser | Utilisateur dont l’identité doit être empruntée lors de l’exécution du travail | string |
| ClassName | Classe principale Java/Spark de l’application | string |
| args | Arguments de ligne de commande pour l’application | Liste de chaînes |
| jars | Fichiers jar à utiliser dans cette session | Liste de chaînes |
| pyFiles | Fichiers Python à utiliser dans cette session | Liste de chaînes |
| files | Fichiers à utiliser dans cette session | Liste de chaînes |
| driverMemory | Quantité de mémoire à utiliser pour le processus du pilote | string |
| driverCores | Nombre de cœurs à utiliser pour le processus du pilote | int |
| executorMemory | Quantité de mémoire à utiliser par processus d’exécuteur | string |
| executorCores | Nombre de cœurs à utiliser pour chaque exécuteur | int |
| numExecutors | Nombre d’exécuteurs à lancer pour cette session | int |
| archives | Archives à utiliser dans cette session | Liste de chaînes |
| queue | Nom de la file d’attente YARN destinataire de l’envoi | string |
| name | Nom de cette session | string |
| conf | Propriétés de configuration de Spark | Mappage clé=valeur |

#### <a name="response-body"></a>Corps de la réponse

Objet de traitement par lots créé.

| name | description | type |
| :- | :- | :- |
| id | ID de session | int |
| appId | ID d’application de cette session | String |
| appInfo | Informations détaillées sur l’application | Mappage clé=valeur |
| log | Lignes de journal | Liste de chaînes |
| state | État du traitement par lots | string |

>[!NOTE]
>La configuration de Livy assignée s’affiche dans le volet de sortie lors de l’envoi du script.

## <a name="additional-features"></a>Fonctionnalités supplémentaires

L’extension Spark & Hive Tools for Visual Studio Code prend en charge les fonctionnalités suivantes :

- **Saisie semi-automatique IntelliSense**. Des suggestions s’affichent pour les mots clés, les méthodes, les variables, etc. Différentes icônes représentent différents types d’objets.

    ![Types d’objets IntelliSense dans l’extension Spark & Hive Tools for Visual Studio Code](./media/spark-hive-tools-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **Marqueur d’erreur IntelliSense**. Le service de langage souligne les erreurs de modification du script Hive.     
- **Coloration syntaxique**. Le service de langage utilise des couleurs différentes pour différencier les variables, les mots clés, les types de données, les fonctions, etc. 

    ![Coloration syntaxique dans l’extension Spark & Hive Tools for Visual Studio Code](./media/spark-hive-tools-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="unlink-cluster"></a>Dissocier le cluster

1. Dans la barre de menus, accédez à **Afficher** > **Palette de commandes...** , puis entrez **Spark / Hive: Unlink a Cluster** (Dissocier un cluster).  

2. Sélectionnez le cluster à dissocier.  

3. Examinez l’affichage **OUTPUT** (SORTIE).  

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur le cluster Big Data SQL Server et les scénarios associés, consultez [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://docs.microsoft.com/sql/big-data-cluster/big-data-cluster-overview).
