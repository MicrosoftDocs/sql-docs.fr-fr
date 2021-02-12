---
title: 'Envoyer des travaux Spark : Azure Data Studio'
titleSuffix: SQL Server Big Data Clusters
description: Envoyez des travaux Spark sur un cluster Big Data SQL Server dans Azure Data Studio.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: dadcdd9e4c50ad1944957f601a54dd1bb9687f22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045789"
---
# <a name="submit-spark-jobs-on-big-data-clusters-2019-in-azure-data-studio"></a>Envoyer des travaux Spark sur [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] dans Azure Data Studio

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Un des principaux scénarios impliquant les clusters Big Data est l’envoi de travaux Spark pour SQL Server. La fonctionnalité d’envoi de travaux Spark vous permet d’envoyer un fichier jar ou py local avec des références au cluster Big Data SQL Server 2019. Elle vous permet également d’exécuter des fichiers jar ou py, qui se trouvent déjà sur le système de fichiers HDFS. 

## <a name="prerequisites"></a>Prérequis

- [Outils de Big Data SQL Server 2019](deploy-big-data-tools.md) :
   - **Azure Data Studio**
   - **Extension SQL Server 2019**
   - **kubectl**

- [Connectez Azure Data Studio à la passerelle HDFS/Spark de votre cluster Big Data](connect-to-big-data-cluster.md).

## <a name="open-spark-job-submission-dialog"></a>Ouvrir la boîte de dialogue d’envoi d’un travail Spark

Vous pouvez ouvrir la boîte de dialogue d’envoi d’un travail Spark de plusieurs façons. Vous avez ainsi le choix entre le tableau de bord, le menu contextuel dans l’Explorateur d’objets et la palette de commandes.

- Pour ouvrir la boîte de dialogue d’envoi d’un travail Spark, cliquez sur **New Spark Job** (Nouveau travail Spark) dans le tableau de bord.

    ![Accès au menu Submit (Envoyer) en cliquant sur le tableau de bord](./media/submit-spark-job/new-spark-job.png)

- Vous pouvez également cliquer avec le bouton droit sur le cluster dans l’Explorateur d’objets et sélectionner **Submit Spark Job** dans le menu contextuel.

    ![Accès au menu Submit en cliquant avec le bouton droit sur le fichier](./media/submit-spark-job/submit-spark-job-1.png)


- Pour ouvrir la boîte de dialogue d’envoi d’un travail Spark avec les champs Jar/Py préremplis, cliquez avec le bouton droit sur un fichier jar/py dans l’Explorateur d’objets et sélectionnez **Submit Spark Job** (Envoyer un travail Spark) dans le menu contextuel.  

    ![Accès au menu Submit (Envoyer) en cliquant avec le bouton droit sur le cluster](./media/submit-spark-job/submit-spark-job.png)

- Pour utiliser **Submit Spark Job** à partir de la palette de commandes, appuyez sur les touches **Ctrl+Maj+P** (sur Windows) ou **Cmd+Maj+P** (sur Mac).

    ![Accès au menu Submit à partir de la palette de commandes dans Windows](./media/submit-spark-job/submit-spark-job-3.png)

    ![Accès au menu Submit à partir de la palette de commandes sur Mac](./media/submit-spark-job/submit-spark-job-4.png)
  
 
## <a name="submit-spark-job"></a>Envoyer un travail Spark 

La boîte de dialogue d’envoi d’un travail Spark s’affiche comme suit. Renseignez le nom du travail, le chemin d’accès du fichier Jar/Py, la classe principale et d’autres champs. Le fichier jar/py peut provenir d’une source locale ou de HDFS. Si le travail Spark contient des fichiers jar de référence, des fichiers py ou des fichiers supplémentaires, cliquez sur l’onglet **ADVANCED** et entrez les chemins correspondants. Cliquez sur **Submit** pour envoyer le travail Spark.

![Boîte de dialogue New Job](./media/submit-spark-job/submit-spark-job-section.png)

![Boîte de dialogue Advanced](./media/submit-spark-job/submit-spark-job-section-1.png)

## <a name="monitor-spark-job-submission"></a>Superviser l’envoi d’un travail Spark

Une fois le travail Spark envoyé, les informations sur l’envoi et l’état d’exécution du travail Spark s’affichent dans Task History (Historique des travaux) à gauche. Des détails sur la progression et les journaux sont également affichés dans la fenêtre **OUTPUT** en bas.

- Quand le travail Spark s’exécute, le volet **Task History** et la fenêtre **OUTPUT** sont actualisés avec la progression.

    ![Superviser un travail Spark en cours d’exécution](./media/submit-spark-job/monitor-spark-job-submission.png)

- Une fois le travail Spark terminé, les liens vers l’interface utilisateur de Spark et celle de Yarn apparaissent dans la fenêtre **OUTPUT**. Cliquez sur les liens pour obtenir plus d’informations.

    ![Lien du travail Spark dans la sortie](./media/submit-spark-job/monitor-spark-job-submission-2.png)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le cluster Big Data SQL Server et les scénarios associés, consultez [Présentation des clusters Big Data SQL Server](big-data-cluster-overview.md).
