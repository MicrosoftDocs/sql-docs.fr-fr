---
title: Déboguer/diagnostiquer des applications Spark
titleSuffix: SQL Server Big Data Clusters
description: Utilisez le serveur d’historique Spark pour déboguer et diagnostiquer les applications Spark s’exécutant sur des clusters Big Data SQL Server 2019.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 43a450b21ab7d025f61f77b1391f9484e1cb2991
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045919"
---
# <a name="debug-and-diagnose-spark-applications-on-big-data-clusters-2019-in-spark-history-server"></a>Déboguer et diagnostiquer les applications Spark sur [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] dans le serveur d’historique Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article fournit des conseils sur l’utilisation du serveur d’historique Spark étendu pour déboguer et diagnostiquer les applications Spark dans un cluster Big Data SQL Server. Ces fonctionnalités de débogage et de diagnostic sont intégrées au serveur d’historique Spark et reposent sur la technologie Microsoft. L’extension comprend les onglets Data (Données), Graph (Graphique) et Diagnosis (Diagnostic). Sous l’onglet Data, les utilisateurs peuvent vérifier les données d’entrée et de sortie du travail Spark. Sous l’onglet Graph, les utilisateurs peuvent vérifier le dataflow et relire le graphique du travail. Sous l’onglet Diagnosis, l’utilisateur peut consulter l’asymétrie des données, l’asymétrie temporelle et l’analyse de l’utilisation des exécuteurs.

## <a name="get-access-to-spark-history-server"></a>Accéder au serveur d’historique Spark

L’expérience utilisateur du serveur d’historique Spark, à l’origine open source, est enrichie d’informations, notamment les données spécifiques au travail et la visualisation interactive du graphique du travail et des dataflows pour le cluster Big Data. 

### <a name="open-the-spark-history-server-web-ui-by-url"></a>Ouvrir l’interface utilisateur web du serveur d’historique Spark au moyen d’une URL
Ouvrez le serveur d’historique Spark en accédant à l’URL suivante, en remplaçant `<Ipaddress>` et `<Port>` par les informations spécifiques du cluster Big Data. Sur des clusters déployés avant SQL Server 2019 CU 5, avec une configuration de cluster Big Data d’authentification de base (nom d’utilisateur/mot de passe), vous devez spécifier l’utilisateur **racine** quand vous êtes invité à vous connecter aux points de terminaison de la passerelle (Knox). Consultez [Déployer un cluster Big Data SQL Server](quickstart-big-data-cluster-deploy.md). [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

```
https://<Ipaddress>:<Port>/gateway/default/sparkhistory
```

L’interface utilisateur web du serveur d’historique Spark ressemble à ceci :

![Serveur d’historique Spark](./media/apache-azure-spark-history-server/spark-history-server.png)


## <a name="data-tab-in-spark-history-server"></a>Onglet Data du serveur d’historique Spark
Sélectionnez l’ID du travail, puis cliquez sur **Data** dans le menu des outils pour obtenir l’affichage des données.

+ Vérifiez les entrées (**Inputs**), sorties (**Outputs**) et opérations de table (**Table Operations**) en sélectionnant les onglets correspondants.

    ![Onglets des données du serveur d’historique Spark](./media/apache-azure-spark-history-server/sparkui-data-tabs.png)

+ Copiez toutes les lignes en cliquant sur le bouton **Copy**.

    ![Copier toutes les lignes](./media/apache-azure-spark-history-server/sparkui-data-copy.png)

+ Enregistrez toutes les données dans un fichier CSV en cliquant sur le bouton **csv**.

    ![Enregistrer les données dans des fichiers CSV](./media/apache-azure-spark-history-server/sparkui-data-save.png)

+ Effectuez une recherche en entrant des mots clés dans le champ **Search** (Rechercher) ; le résultat de la recherche s’affiche immédiatement.

    ![Effectuer une recherche avec des mots clés](./media/apache-azure-spark-history-server/sparkui-data-search.png)

+ Cliquez sur l’en-tête de colonne pour trier la table, cliquez sur le signe plus pour développer une ligne afin d’afficher plus de détails ou cliquez sur le signe moins pour réduire une ligne.

    ![Fonctionnalité de la table de données](./media/apache-azure-spark-history-server/sparkui-data-table.png)

+ Téléchargez un fichier spécifique en cliquant sur le bouton **Partial Download** (Téléchargement partiel) situé à droite ; le fichier sélectionné est alors téléchargé à l’emplacement local. Si le fichier n’existe plus, un nouvel onglet s’ouvre pour afficher les messages d’erreur.

    ![Télécharger une ligne de données](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Copiez le chemin complet ou relatif en sélectionnant l’option **Copy Full Path** (Copier le chemin complet) ou **Copy Relative Path** (Copier le chemin relatif) dans le menu de téléchargement. Pour les fichiers de stockage Azure Data Lake, l’option **Open in Azure Storage Explorer** (Ouvrir dans l’Explorateur Stockage Azure) lance l’Explorateur Stockage Azure. En outre, recherchez le dossier exact lors de la connexion.

    ![Copier un chemin complet ou relatif](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Si toutes les lignes ne contiennent pas dans une seule page, vous pouvez cliquer sur un nombre sous la table pour naviguer entre les pages. 

    ![Page des données](./media/apache-azure-spark-history-server/sparkui-data-page.png)

+ Pointez sur le point d’interrogation en regard de Data pour afficher l’info-bulle, ou cliquez sur le point d’interrogation pour obtenir plus d’informations.

    ![Informations supplémentaires depuis l’onglet Data](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+ Si vous rencontrez des problèmes, envoyez vos commentaires en cliquant sur **Provide us feedback** (Envoyer des commentaires).

    ![Commentaires sur le graphique](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="graph-tab-in-spark-history-server"></a>Onglet Graph du serveur d’historique Spark

Sélectionnez l’ID du travail, puis cliquez sur **Graph** dans le menu des outils pour obtenir l’affichage du graphique du travail.

+ Vérifiez la vue d’ensemble de votre travail à l’aide du graphique généré correspondant. 

+ Par défaut, la vue affiche tous les travaux, mais elle peut être filtrée par **Job ID** (ID de travail).

    ![ID de travail dans le graphique](./media/apache-azure-spark-history-server/sparkui-graph-jobid.png)

+ Nous conservons **Progress** (Avancement) comme valeur par défaut. L’utilisateur peut vérifier le dataflow en sélectionnant **Read** (Données lues) ou **Written** (Données écrites) dans la liste déroulante **Display** (Affichage).

    ![Affichage du graphique](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

    La carte thermique détermine la couleur des nœuds du graphique.

    ![Carte thermique du graphique](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)

+ Lisez le travail en cliquant sur le bouton **Playback** (Lire) et arrêtez à tout moment en cliquant sur le bouton d’arrêt. Lors de la lecture, l’affichage de la tâche utilise des couleurs pou refléter les différents états :

    + Vert (réussite) : le travail s’est correctement effectué.
    + Orange (nouvelle tentative) : instances de tâches qui ont échoué, mais qui n’affectent pas le résultat final du travail. Ces tâches comportaient des instances dupliquées ou renouvelées qui peuvent réussir ultérieurement.
    + Bleu (en cours d’exécution) : la tâche est en cours d’exécution.
    + Blanc (en attente ou ignorée) : la tâche est en attente d’exécution, ou la phase a été ignorée.
    + Rouge (échec) : la tâche a échoué.

    ![Exemple de couleur dans le graphique : en cours d’exécution](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)
 
    La phase ignorée s’affiche en blanc.
    ![Exemple de couleur dans le graphique : ignorer](./media/apache-azure-spark-history-server/sparkui-graph-color-skip.png)

    ![Exemple de couleur dans le graphique : échec](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)
 
    > [!NOTE]
    > La lecture est autorisée pour chaque travail. Pour un travail incomplet, la lecture n’est pas prise en charge.


+ Utilisez la roulette de la souris pour effectuer un zoom avant/arrière sur le graphique du travail ou cliquez sur **Zoom to fit** (Zoomer pour ajuster) pour l’ajuster à l’écran.
 
    ![Zoom sur le graphique pour l’ajuster](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Placez le curseur sur un nœud du graphique pour afficher l’info-bulle en cas d’échec de tâches, puis cliquez sur la phase pour ouvrir la page correspondante.

    ![Info-bulle du graphique](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ Dans l’onglet du graphique du travail, les phases sont accompagnées d’une info-bulle et d’une petite icône si elles ont des tâches qui sont conformes aux conditions suivantes :
    + Asymétrie des données : taille de lecture des données > taille moyenne de lecture des données de toutes les tâches à l’intérieur de cette phase * 2 et taille de lecture des données > 10 Mo
    + Asymétrie temporelle : durée d’exécution > durée d’exécution moyenne de toutes les tâches à l’intérieur de cette phase * 2 et durée d’exécution > 2 minutes

    ![Icône d’asymétrie dans le graphique](./media/apache-azure-spark-history-server/sparkui-graph-skew-icon.png)

+ Le nœud de graphique du travail affiche les informations suivantes de chaque phase :
    + ID.
    + Nom ou description.
    + Nombre total de tâches.
    + Lecture de données : somme de la taille d’entrée et de la taille de lecture aléatoire.
    + Écriture de données : somme de la taille de sortie et de la taille d’écriture aléatoire.
    + Durée d’exécution : durée entre l’heure de début de la première tentative et l’heure de fin de la dernière tentative.
    + Nombre de lignes : somme des enregistrements d’entrée, des enregistrements de sortie, des enregistrements de lecture aléatoire et des enregistrements d’écriture aléatoire.
    + Avancement.

    > [!NOTE]
    > Par défaut, le nœud de graphique du travail affiche les informations de la dernière tentative de chaque phase (à l’exception de la durée d’exécution de la phase), mais pendant la lecture, il affiche les informations de chaque tentative.

    > [!NOTE]
    > Pour la taille des données de lecture et d’écriture, nous utilisons 1 Mo = 1000 Ko = 1000 *1000 octets.

+ Si vous rencontrez des problèmes, envoyez vos commentaires en cliquant sur **Provide us feedback** (Envoyer des commentaires).

    ![Commentaires sur le graphique](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="diagnosis-tab-in-spark-history-server"></a>Onglet Diagnosis du serveur d’historique Spark
Sélectionnez l’ID du travail, puis cliquez sur **Diagnosis** dans le menu des outils pour obtenir l’affichage du diagnostic du travail. L’onglet du diagnostic comprend **Data Skew** (Asymétrie des données), **Time Skew** (Asymétrie temporelle) et **Executor Usage Analysis** (Analyse de l’utilisation des exécuteurs).
    
+ Vérifiez l’**asymétrie des données**, l’**asymétrie temporelle** et l’**analyse de l’utilisation des exécuteurs** en sélectionnant l’onglet correspondant.

    ![Onglets de diagnostic](./media/apache-azure-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Asymétrie des données
Cliquez sur l’onglet **Data Skew** ; les tâches asymétriques correspondantes s’affichent en fonction des paramètres spécifiés. 

+ **Spécifier les paramètres** : la première section affiche les paramètres qui sont utilisés pour détecter l’asymétrie des données. La règle intégrée est la suivante : Les données des tâches lues sont trois fois supérieures aux données des tâches lues en moyenne, et les données des tâches lues sont supérieures à 10 Mo. Si vous souhaitez définir votre propre règle pour les tâches asymétriques, vous pouvez choisir vos paramètres ; les sections **Skewed Stage** (Phase asymétrique) et **Skew Chart** (Graphique des asymétries) sont actualisées en conséquence. 

+ **Phase asymétrique** : la seconde section affiche les phases comportant des tâches asymétriques qui répondent aux critères spécifiés ci-dessus. S’il y a plusieurs tâches asymétriques dans une phase, la table des phases asymétriques affiche uniquement la tâche la plus asymétrique (par exemple, les données les plus volumineuses pour l’asymétrie des données). 

    ![Asymétrie des données, section 2](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

+ **Skew Chart** : quand une ligne de la table des phases asymétriques est sélectionnée, le graphique des asymétries affiche plus de détails sur les distributions des tâches en fonction de la lecture des données et de la durée d’exécution. Les tâches asymétriques sont marquées en rouge et les tâches normales sont marquées en bleu. Pour des considérations liées aux performances, le graphique affiche au maximum 100 exemples de tâches. Les détails de la tâche s’affichent dans le volet inférieur droit.

    ![Asymétrie des données, section 3](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Asymétrie temporelle
L’onglet **Time Skew** affiche les tâches asymétriques en fonction de la durée d’exécution des tâches. 

+ **Spécifier les paramètres** : la première section affiche les paramètres qui sont utilisés pour détecter l’asymétrie temporelle. Par défaut, les critères de détection de l’asymétrie temporelle sont les suivants : la durée d’exécution de la tâche est plus de trois fois supérieure à la durée d’exécution moyenne et supérieure à 30 secondes. Vous pouvez changer les paramètres en fonction de vos besoins. Les sections **Skewed Stage** et **Skew Chart** affichent les informations sur les phases et les tâches correspondantes, à l’image de l’onglet **Data Skew** ci-dessus.

+ Cliquez **Time Skew** ; le résultat filtré s’affiche dans la section **Skewed Stage** en fonction des paramètres définis dans la section **Specify Parameters**. Cliquez sur un élément dans la section **Skewed Stage** ; le graphique correspondant est élaboré dans la section 3 et les détails de la tâche s’affichent dans le volet inférieur droit.

    ![Asymétrie temporelle, section 2](./media/apache-azure-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis"></a>Analyse de l’utilisation des exécuteurs
Le graphique de l’utilisation des exécuteurs permet de visualiser l’allocation réelle des exécuteurs du travail Spark et l’état de l’exécution.  

+ Cliquez **Executor Usage Analysis** ; ensuite, nous ébauchons quatre courbes sur l’utilisation des exécuteurs. Ce sont **Allocated Executors** (Exécuteurs alloués), **Running Executors** (Exécuteurs en cours d’exécution), **Idle Executors** (exécuteurs inactifs) et **Max Executor Instances** (Nombre maximal d’instances d’exécuteur). En ce qui concerne les exécuteurs alloués, chaque événement d’ajout d’exécuteur ou de suppression d’exécuteur augmente ou diminue les exécuteurs alloués. Vous pouvez cocher la case « Event Timeline » (Chronologie des événements) sous l’onglet « Jobs » (Travaux) pour effectuer d’autres comparaisons.

    ![Onglet des exécuteurs](./media/apache-azure-spark-history-server/sparkui-diagnosis-executors.png)

+ Cliquez sur une icône de couleur pour sélectionner ou désélectionner le contenu correspondant dans toutes les ébauches.

    ![Sélectionner un graphique](./media/apache-azure-spark-history-server/sparkui-diagnosis-select-chart.png)
    
## <a name="spark--yarn-logs"></a>Journaux Spark/Yarn
En plus du serveur d’historique Spark, vous pouvez trouver les journaux pour Spark et Yarn, respectivement :
* Journaux des événements Spark : hdfs:///system/spark-events
* Journaux Yarn : hdfs:///tmp/logs/root/logs-tfile

Remarque : Ces deux journaux ont une période de rétention par défaut de 7 jours. Si vous souhaitez modifier la période de rétention, consultez la page [Configurer Apache Spark et Apache Hadoop](configure-spark-hdfs.md). Il est **impossible** de modifier l’emplacement.

## <a name="known-issues"></a>Problèmes connus
Le serveur d’historique Spark présente les problèmes connus suivants :

+ Il fonctionne uniquement pour le cluster Spark 2.3.

+ Les données d’entrée/sortie utilisant un jeu de données distribué résilient ne sont pas affichées dans l’onglet des données.

## <a name="next-steps"></a>Étapes suivantes

* [Bien démarrer avec les clusters Big Data SQL Server](./deploy-get-started.md)
* Configurer les paramètres Spark
* [Configurer les paramètres Spark](/azure/hdinsight/spark/apache-spark-settings/)