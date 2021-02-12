---
title: 'Exécuter les travaux Spark : Kit de ressources Azure pour IntelliJ'
titleSuffix: SQL Server Big Data Clusters
description: Découvrez comment envoyer des travaux Spark sur des clusters Big Data SQL Server dans Azure Toolkit for IntelliJ en envoyant un fichier jar ou py local.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.topic: conceptual
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 36718ef7ea9def99a2b2629119cbd7dfee63dac3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043629"
---
# <a name="submit-spark-jobs-on-big-data-clusters-2019-in-intellij"></a>Soumettre des travaux Spark sur [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] dans IntelliJ

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Un des principaux scénarios pour [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] est la possibilité de soumettre des travaux Spark. La fonctionnalité de soumission de travaux Spark vous permet d’envoyer un fichier Jar ou Py local avec des références à [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]. Elle vous permet également d’exécuter des fichiers jar ou py, qui se trouvent déjà sur le système de fichiers HDFS. 

## <a name="prerequisites"></a>Prérequis

- Un cluster Big Data SQL Server
- Un kit de développement Oracle Java. Vous pouvez l’installer à partir du [site web Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
- IntelliJ IDEA. Vous pouvez l’installer à partir du [site web JetBrains](https://www.jetbrains.com/idea/download/).
- L’extension Azure Toolkit for IntelliJ. Pour obtenir des instructions d’installation, consultez [Installer Azure Toolkit for IntelliJ](/azure/azure-toolkit-for-intellij-installation).

## <a name="link-sql-server-big-data-cluster"></a>Lier un cluster Big Data SQL Server
1. Ouvrez l’outil IntelliJ IDEA.

2. Si vous utilisez un certificat autosigné, vous devez désactiver la validation des certificats TLS/SSL. Pour cela, accédez au menu **Tools**, sélectionnez **Azure**, **Validate Spark Cluster SSL Certificate** (Valider les certificats SSL des clusters Spark), puis **Disable**.

    ![Lier un cluster Big Data SQL Server - Désactiver TLS/SSL](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-disableSSL.png)

3. Pour ouvrir l’explorateur Azure, dans le menu **View**, sélectionnez **Tool Windows**, puis **Azure Explorer**.
4. Cliquez avec le bouton droit sur **SQL Server big data cluster** (Cluster Big Data SQL Server), puis sélectionnez **Link SQL Server big data cluster** (Lier un cluster Big Data SQL Server). Dans les champs **Server**, **User Name** et **Password**, entrez respectivement un nom de serveur, un nom d’utilisateur et un mot de passe, puis cliquez sur **OK**.

    ![Lier un cluster Big Data - Boîte de dialogue](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-dialog.png)

5. Lorsque la boîte de dialogue du certificat du serveur non approuvé s’affiche, cliquez sur **Accept**. Vous pourrez gérer le certificat ultérieurement (voir [Certificats de serveur](https://www.jetbrains.com/help/idea/settings-tools-server-certificates.html)).

6. Le cluster lié s’affiche sous **SQL Server big data cluster**. Vous pouvez superviser un travail Spark à partir de l’historique Spark ou de l’interface utilisateur Yarn. Vous pouvez également supprimer la liaison en cliquant avec le bouton droit sur le cluster.

    ![Lier un cluster Big Data - Menu contextuel](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-contextmenu.png)

## <a name="create-a-spark-scala-application-from-spark-template"></a>Créer une application Spark Scala à partir d’un modèle Spark

1. Démarrez IntelliJ IDEA, puis créez un projet. Dans la boîte de dialogue **New Project**, effectuez les étapes suivantes : 

   a. Sélectionnez **Azure Spark/HDInsight** > **Spark Project with Samples (Scala)** .

   b. Dans la liste **Build tool** (Outil de build), sélectionnez l’une des options ci-dessous, en fonction de vos besoins :

      * **Maven**, pour la prise en charge de l’Assistant Création de projet Scala
      * **SBT**, pour la gestion des dépendances et la création du projet Scala

    ![Boîte de dialogue New Project](./media/spark-submit-job-intellij-tool-plugin/create-hdi-scala-app.png)

2. Sélectionnez **Suivant**.

3. L’Assistant Création de projet Scala détecte automatiquement si vous avez installé le plug-in Scala. Sélectionnez **Installer**.

   ![Vérification du plug-in Scala](./media/spark-submit-job-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. Pour télécharger le plug-in Scala, sélectionnez **OK**. Suivez les instructions pour redémarrer IntelliJ. 

   ![Boîte de dialogue d’installation du plug-in Scala](./media/spark-submit-job-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. Dans la boîte de dialogue **New Project** (Nouveau projet), effectuez les étapes suivantes :  

    ![Sélection du SDK Spark](./media/spark-submit-job-intellij-tool-plugin/hdi-new-project.png)

   a. Entrez un nom et un emplacement pour le projet.

   b. Dans la liste déroulante **Project SDK** (SDK du projet), sélectionnez **Java 1.8** pour le cluster Spark 2.x, ou sélectionnez **Java 1.7** pour le cluster Spark 1.x.

   c. Dans la liste déroulante **Spark version** (Version Spark), l’Assistant Création de projets Scala affiche la version du SDK Spark et la version du SDK Scala correspondantes. Si la version du cluster Spark est antérieure à la version 2.0, sélectionnez **Spark 1.x**. Sinon, sélectionnez **Spark 2.x**. Cet exemple utilise **Spark 2.0.2 (Scala 2.11.8)** .

6. Sélectionnez **Terminer**.

7. Le projet Spark crée automatiquement un artefact. Pour afficher l’artefact, effectuez les étapes suivantes :

   a. Dans le menu **File** (Fichier), sélectionnez **Project Structure** (Structure du projet).

   b. Dans la boîte de dialogue **Project Structure**, sélectionnez **Artifacts** pour voir l’artefact par défaut qui a été créé. Vous pouvez également créer votre propre artefact en sélectionnant le signe plus ( **+** ).

      ![Informations sur l’artefact dans la boîte de dialogue](./media/spark-submit-job-intellij-tool-plugin/default-artifact.png)
      

## <a name="submit-application-to-sql-server-big-data-cluster"></a>Envoyer l’application vers un cluster Big Data SQL Server
Après avoir lié un cluster Big Data SQL Server, vous pouvez envoyer une application vers celui-ci.

1. Dans la fenêtre **Run/Debug Configurations** (Exécuter/Déboguer les configurations),procédez à la configuration, cliquez sur +->**Apache Spark on SQL Server**, sélectionnez l’onglet **Remotely Run in Cluster** (Exécuter à distance dans le cluster), configurez les paramètres comme ci-dessous, puis cliquez sur OK.

    ![Ajout d’une entrée de configuration dans la console interactive](./media/spark-submit-job-intellij-tool-plugin/interactive-console-add-config-entry.png)

    ![Lier un cluster Big Data - Configuration](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-config.png)

    * Dans **Spark clusters (Linux only)** (Clusters Spark (Linux uniquement)), sélectionnez le cluster sur lequel vous souhaitez exécuter votre application.

    * Sélectionnez un artefact dans le projet IntelliJ ou sélectionnez-en un à partir du disque dur.

    * Champ **Main class name** (Nom de la classe principale) : La valeur par défaut est la classe principale du fichier sélectionné. Vous pouvez changer la classe en sélectionnant les points de suspension ( **...** ), puis en choisissant une autre classe.   

    * Champ **Job Configurations** :  les valeurs par défaut sont définies comme dans l’image ci-dessus. Vous pouvez changer la valeur ou ajouter une nouvelle clé/valeur pour l’envoi de votre travail. Pour plus d'informations : [API REST Apache Livy](http://livy.incubator.apache.org./docs/latest/rest-api.html)

      ![Boîte de dialogue d’envoi Spark - Signification des configurations de travaux](./media/spark-submit-job-intellij-tool-plugin/submit-job-configurations.png)

    * Champ **Command line arguments** : Pour la classe principale, vous pouvez entrer les valeurs des arguments en les séparant par un espace, si nécessaire.

    * Champs **Referenced Jars** et **Referenced Files** : vous pouvez entrer les chemins des fichiers jar et des fichiers référencés, si vous en avez. Pour plus d'informations : [Configuration Apache Spark](https://spark.apache.org/docs/latest/configuration.html#runtime-environment) 

      ![Boîte de dialogue d’envoi Spark - Signification des fichiers jar](./media/spark-submit-job-intellij-tool-plugin/jar-files-meaning.png)

       > [!NOTE]  
       > Pour charger vos fichiers jar et vos fichiers référencés, consultez : [Comment charger des ressources dans un cluster](/azure/storage/blobs/storage-quickstart-blobs-storage-explorer)
                         
    * Champ **Upload Path** : vous pouvez indiquer l’emplacement de stockage pour l’envoi du fichier jar ou des ressources du projet Scala. Plusieurs types de stockage sont pris en charge : **Use Spark interactive session to upload** (Utiliser une session interactive Spark pour le chargement) et **Use WebHDFS to upload** (Utiliser WebHDFS pour le chargement)
    
2. Cliquez sur **SparkJobRun** pour envoyer votre projet vers le cluster sélectionné. L’onglet **Remote Spark Job in Cluster** (Travail Spark distant dans le cluster) affiche la progression de l’exécution du travail au bas de la page. Vous pouvez arrêter l’application en cliquant sur le bouton rouge.  

    ![Lier un cluster Big Data - Exécution](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-run.png)

## <a name="spark-console"></a>Console Spark
Vous pouvez exécuter la console locale Spark (Scala) ou exécuter la console de sessions interactives Spark Livy (Scala).

### <a name="spark-local-consolescala"></a>Console locale Spark (Scala)
Veillez à respecter les prérequis WINUTILS.EXE.

1. Dans la barre de menus, accédez à **Run** > **Edit Configurations...** .

2. Dans la fenêtre **Run/Debug Configurations**, dans le volet de gauche, accédez à **Apache Spark on SQL Server big data cluster** >  **[Spark on SQL] myApp**.

3. Dans la fenêtre principale, sélectionnez l’onglet **Locally Run**.

4. Entrez les valeurs suivantes, puis sélectionnez **OK** :

    |Propriété |Valeur |
    |----|----|
    |Job main class (Classe principale du travail)|La valeur par défaut est la classe principale du fichier sélectionné. Vous pouvez changer la classe en sélectionnant les points de suspension ( **...** ), puis en choisissant une autre classe.|
    |Variables d'environnement|Vérifiez que la valeur de HADOOP_HOME est correcte.|
    |WINUTILS.exe location|Vérifiez que le chemin est correct.|

    ![Configuration de la console locale](./media/spark-submit-job-intellij-tool-plugin/console-set-configuration.png)

5. Dans le projet, accédez à **myApp** > **src** > **main** > **scala** > **myApp**.  

6. Dans la barre de menus, accédez à **Tools** > **Spark Console** > **Run Spark Local Console(Scala)** .

7. Deux boîtes de dialogue peuvent s’afficher pour vous demander si vous souhaitez corriger automatiquement les dépendances. Si c’est le cas, sélectionnez **Correction automatique**.

    ![Correction automatique Spark 1](./media/spark-submit-job-intellij-tool-plugin/console-auto-fix1.png)

    ![Correction automatique Spark 2](./media/spark-submit-job-intellij-tool-plugin/console-auto-fix2.png)

8. La console doit ressembler à l’image ci-dessous. Dans la fenêtre de la console, tapez `sc.appName`, puis appuyez sur Ctrl + Entrée.  Le résultat s’affiche. Vous pouvez arrêter l’exécution de la console locale en cliquant sur le bouton rouge.

    ![Résultat dans la console locale](./media/spark-submit-job-intellij-tool-plugin/local-console-result.png)


### <a name="spark-livy-interactive-session-consolescala"></a>Console de sessions interactives Spark Livy (Scala)
La console de sessions interactives Spark Livy (Scala) est uniquement prise en charge sur IntelliJ 2018.2 et 2018.3.

1. Dans la barre de menus, accédez à **Run** > **Edit Configurations...** .

2. Dans la fenêtre **Run/Debug Configurations**, dans le volet de gauche, accédez à **Apache Spark on SQL Server big data cluster** >  **[Spark on SQL] myApp**.

3. Dans la fenêtre principale, sélectionnez l’onglet **Remotely Run in Cluster**.

4. Entrez les valeurs suivantes, puis sélectionnez **OK** :

    |Propriété |Valeur |
    |----|----|
    |Spark clusters (Linux only)|Sélectionnez le cluster Big Data SQL Server sur lequel vous souhaitez exécuter votre application.|
    |Main class name|La valeur par défaut est la classe principale du fichier sélectionné. Vous pouvez changer la classe en sélectionnant les points de suspension ( **...** ), puis en choisissant une autre classe.|

    ![Configuration de la console interactive](./media/spark-submit-job-intellij-tool-plugin/interactive-console-configuration.png)

5. Dans le projet, accédez à **myApp** > **src** > **main** > **scala** > **myApp**.  

6. Dans la barre de menus, accédez à **Tools** > **Spark Console** > **Run Spark Livy Interactive Session Console(Scala)** .

7. La console doit ressembler à l’image ci-dessous. Dans la fenêtre de la console, tapez `sc.appName`, puis appuyez sur Ctrl + Entrée.  Le résultat s’affiche. Vous pouvez arrêter l’exécution de la console locale en cliquant sur le bouton rouge.

    ![Résultat dans la console interactive](./media/spark-submit-job-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Envoyer la sélection vers la console Spark

Pour des raisons pratiques, vous pouvez voir le résultat du script en envoyant du code vers la console locale ou la console de sessions interactives Livy (Scala). Vous pouvez mettre en surbrillance du code dans le fichier Scala, puis cliquer avec le bouton droit sur **Send Selection To Spark Console** (Envoyer la sélection vers la console Spark). Le code sélectionné est envoyé vers la console pour être exécuté. Le résultat s’affiche après le code dans la console. La console vérifie les erreurs, le cas échant.  

   ![Envoyer la sélection vers la console Spark](./media/spark-submit-job-intellij-tool-plugin/send-selection-to-console.png)

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur le cluster Big Data SQL Server et les scénarios associés, consultez [Présentation des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md).