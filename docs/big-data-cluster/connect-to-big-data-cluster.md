---
title: Se connecter aux clusters Big Data HDFS et principal
description: Découvrez comment vous connecter à l’instance principale SQL Server et à la passerelle HDFS/Spark pour un cluster Big Data SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b7f05551da025f513cd69ddf592002126c6f05c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039051"
---
# <a name="connect-to-a-sql-server-big-data-cluster-with-azure-data-studio"></a>Se connecter à un cluster Big Data SQL Server avec Azure Data Studio

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article explique comment se connecter à un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] à partir d’Azure Data Studio.

## <a name="prerequisites"></a>Prérequis

- Un [cluster Big Data SQL Server 2019](deployment-guidance.md) déployé.
- [Outils de Big Data SQL Server 2019](deploy-big-data-tools.md) :
   - **Azure Data Studio**
   - **Extension SQL Server 2019**
   - **kubectl**
   - **azdata**

## <a name="connect-to-the-cluster"></a><a id="master"></a> Se connecter au cluster

Pour vous connecter à un cluster Big Data avec Azure Data Studio, établissez une nouvelle connexion à l’instance principale SQL Server dans le cluster. Voici comment faire.

1. Recherchez le point de terminaison de l’instance principale SQL Server :

   ```
   azdata bdc endpoint list -e sql-server-master
   ```

   > [!TIP]
   > Pour plus d’informations sur la façon de récupérer des points de terminaison, consultez [Récupérer des points de terminaison](deployment-guidance.md#endpoints).

1. Dans Azure Data Studio, appuyez sur **F1** > **Nouvelle connexion**.

1. Dans **Type de connexion**, sélectionnez **Microsoft SQL Server**.

1. Tapez le nom du point de terminaison que vous avez trouvé pour l’instance principale SQL Server dans la zone de texte **Nom du serveur** (par exemple : **\<IP_Address\>,31433**). 

1. Choisissez votre type d’authentification. Pour une instance principale SQL Server s’exécutant dans un cluster Big Data, seules l’**authentification Windows** et la **connexion SQL** sont prises en charge. 

1. Si vous utilisez une connexion SQL, entrez le **nom d’utilisateur** et le **mot de passe** de votre connexion SQL .

   > [!TIP]
   > Par défaut, le nom d’utilisateur **SA** est désactivé durant le déploiement de clusters Big Data. Un nouvel utilisateur sysadmin est provisionné lors du déploiement avec le nom et le mot de passe correspondant aux variables d’environnement **AZDATA_USERNAME** et **AZDATA_PASSWORD**, qui ont été définies avant ou après le déploiement.

1. Remplacez le **Nom de la base de données** cible par une de vos bases de données relationnelles.

   ![Se connecter à l’instance principale](./media/connect-to-big-data-cluster/connect-to-cluster.png)

1. Appuyez sur **Se connecter** : le **Tableau de bord du serveur** doit apparaître.

Avec la version de février 2019 d’Azure Data Studio, la connexion à l’instance principale SQL Server vous permet également d’interagir avec la passerelle HDFS/Spark. Cela signifie que vous n’avez pas besoin d’utiliser une connexion distincte pour HDFS et Spark décrite dans la section suivante.

- L’Explorateur d’objets contient maintenant un nouveau nœud **Services de données** avec prise en charge du clic droit pour les tâches de cluster Big Data, comme la création de notebooks ou l’envoi de travaux Spark. 
- Le nœud **Data Services** contient également un dossier **HDFS** pour vous permettre d’explorer le contenu du HDFS et d’effectuer des tâches courantes impliquant le HDFS (par exemple la création d’une table externe ou l’ouverture d’un notebook pour analyser le contenu du HDFS).
- Le **Tableau de bord du serveur** pour la connexion contient également des onglets pour **Cluster Big Data SQL Server** et **SQL Server 2019** quand l’extension est installée.

   ![Nœud Services de données d’Azure Data Studio](./media/connect-to-big-data-cluster/connect-data-services-node.png)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).
