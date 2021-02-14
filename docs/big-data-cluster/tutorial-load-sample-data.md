---
title: Charger un exemple de données
titleSuffix: SQL Server big data clusters
description: Ce tutoriel montre comment charger un exemple de données dans un cluster Big Data SQL Server. L’exemple de données inclut des données relationnelles dans l’instance maître SQL Server. Il comprend également des données HDFS dans le pool de stockage. Ces données prennent en charge d’autres tutoriels dans cette section.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9b4e7667a3f109a0b5cca2f0af60af38613c5ce8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045650"
---
# <a name="tutorial-load-sample-data-into-a-sql-server-big-data-cluster"></a>Tutoriel : Charger un exemple de données dans votre cluster Big Data

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Ce tutoriel explique comment utiliser un script pour charger un exemple de données dans un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. La plupart des autres tutoriels de la documentation utilisent cet exemple de données.

> [!TIP]
> Vous trouverez d’autres exemples pour [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] dans le dépôt GitHub [sql-server-samples](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster). Le chemin de ce dépôt est le suivant : **sql-server-samples/samples/features/sql-big-data-cluster/** .

## <a name="prerequisites"></a>Prérequis

- [Cluster Big Data déployé](deployment-guidance.md)
- [Outils Big Data](deploy-big-data-tools.md)
   - **azdata**
   - **kubectl**
   - **sqlcmd**
   - **curl**
 
## <a name="load-sample-data"></a><a id="sampledata"></a> Charger un exemple de données

Les étapes suivantes utilisent un script d’amorçage pour télécharger une sauvegarde de base de données SQL Server et charger les données dans votre cluster Big Data. Pour faciliter les choses, ces étapes sont réparties en deux sections : [Windows](#windows) et [Linux](#linux). Si vous souhaitez utiliser le nom d’utilisateur/mot de passe de base comme mécanisme d’authentification, définissez les variables d’environnement AZDATA_USERNAME et AZDATA_PASSWORD avant d’exécuter le script. Dans le cas contraire, le script utilisera l’authentification intégrée pour se connecter à l’instance maître SQL Server et à la passerelle Knox. En outre, le nom DNS doit être spécifié pour les points de terminaison afin d’utiliser l’authentification intégrée.

## <a name="windows"></a><a id="windows"></a> Windows

Les étapes suivantes décrivent comment utiliser un client Windows pour charger l’exemple de données dans votre cluster Big Data.

1. Ouvrez une nouvelle invite de commandes Windows.

   > [!IMPORTANT]
   > N’utilisez pas Windows PowerShell pour ces étapes. Dans PowerShell, le script échoue car il utilise la version PowerShell de **curl**.

1. Utilisez **curl** pour télécharger le script d’amorçage pour l’exemple de données.

   ```cmd
   curl -o bootstrap-sample-db.cmd "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.cmd"
   ```

1. Téléchargez le script Transact-SQL **bootstrap-sample-db.sql**. Ce script est appelé par le script d’amorçage.

   ```cmd
   curl -o bootstrap-sample-db.sql "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sql"
   ```

1. Le script d’amorçage nécessite les paramètres positionnels suivants pour votre cluster Big Data :

   | Paramètre | Description |
   |---|---|
   | <CLUSTER_NAMESPACE> | Nom que vous avez donné à votre cluster Big Data. |
   | <SQL_MASTER_ENDPOINT> | Nom DNS ou adresse IP de votre instance maître. |
   | <KNOX_ENDPOINT> | Le nom DNS ou l’adresse IP de la passerelle HDFS/Spark. |
   
   > [!TIP]
   > Utilisez [kubectl](cluster-troubleshooting-commands.md) pour rechercher les adresses IP de l’instance maître de SQL Server et de Knox. Exécutez `kubectl get svc -n <your-big-data-cluster-name>` et examinez les adresses EXTERNAL-IP de l’instance maître (**master-svc-external**) et de Knox (**gateway-svc-external**). Le nom par défaut d’un cluster est **mssql-cluster**.

1. Exécutez le script d’amorçage.

   ```cmd
   .\bootstrap-sample-db.cmd <CLUSTER_NAMESPACE> <SQL_MASTER_ENDPOINT> <KNOX_ENDPOINT>
   ```

## <a name="linux"></a><a id="linux"></a> Linux

Les étapes suivantes décrivent comment utiliser un client Linux pour charger l’exemple de données dans votre cluster Big Data.

1. Téléchargez le script d’amorçage et affectez-lui les autorisations d’un exécutable.

   ```bash
   curl -o bootstrap-sample-db.sh "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sh"
   chmod +x bootstrap-sample-db.sh
   ```

1. Téléchargez le script Transact-SQL **bootstrap-sample-db.sql**. Ce script est appelé par le script d’amorçage.

   ```bash
   curl -o bootstrap-sample-db.sql "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sql"
   ```

1. Le script d’amorçage nécessite les paramètres positionnels suivants pour votre cluster Big Data :

   | Paramètre | Description |
   |---|---|
   | <CLUSTER_NAMESPACE> | Nom que vous avez donné à votre cluster Big Data. |
   | <SQL_MASTER_ENDPOINT> | Nom DNS ou adresse IP de votre instance maître. |
   | <KNOX_ENDPOINT> | Le nom DNS ou l’adresse IP de la passerelle HDFS/Spark. |

   > [!TIP]
   > Utilisez [kubectl](cluster-troubleshooting-commands.md) pour rechercher les adresses IP de l’instance maître de SQL Server et de Knox. Exécutez `kubectl get svc -n <your-big-data-cluster-name>` et examinez les adresses EXTERNAL-IP de l’instance maître (**master-svc-external**) et de Knox (**gateway-svc-external**). Le nom par défaut d’un cluster est **mssql-cluster**.

1. Exécutez le script d’amorçage.

   ```bash
   ./bootstrap-sample-db.sh <CLUSTER_NAMESPACE> <SQL_MASTER_ENDPOINT> <KNOX_ENDPOINT>
   ```

## <a name="next-steps"></a>Étapes suivantes

Après l’exécution du script d’amorçage., votre cluster Big Data contient les exemples de bases de données et de données HDFS. Les tutoriels suivants utilisent l’exemple de données pour illustrer les fonctionnalités du cluster Big Data :

Virtualisation de données :

- [Tutoriel : Interroger HDFS dans un cluster Big Data SQL Server](tutorial-query-hdfs-storage-pool.md)
- [Tutoriel : Interroger Oracle à partir d’un cluster Big Data SQL Server](tutorial-query-oracle.md)

Ingestion des données :

- [Tutoriel : Ingérer des données dans un pool de données SQL Server avec Transact-SQL](tutorial-data-pool-ingest-sql.md)
- [Tutoriel : Ingérer des données dans un pool de données SQL Server avec des travaux Spark](tutorial-data-pool-ingest-spark.md)

Notebooks :

- [Tutoriel : Exécuter un exemple de notebook sur un cluster Big Data SQL Server 2019](notebooks-tutorial-spark.md)
