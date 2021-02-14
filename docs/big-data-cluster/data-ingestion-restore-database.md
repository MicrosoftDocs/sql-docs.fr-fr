---
title: Restaurer une base de données
titleSuffix: SQL Server big data clusters
description: Cet article explique comment restaurer une base de données dans l’instance principale d’un cluster Big Data SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3082c970c648fddf67b635ba7cbae5b19bfc0170
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049019"
---
# <a name="restore-a-database-into-the-sql-server-big-data-cluster-master-instance"></a>Restaurer une base de données dans l’instance principale du cluster Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article explique comment restaurer une base de données existante dans l’instance principale d’un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. La méthode recommandée consiste à utiliser une approche de sauvegarde, de copie et de restauration.

## <a name="backup-your-existing-database"></a>Sauvegarder votre base de données existante

Sauvegardez d’abord votre base de données SQL Server existante à partir de SQL Server sur Windows ou Linux. Utilisez des techniques de sauvegarde standard avec Transact-SQL ou un outil comme SQL Server Management Studio (SSMS).

Cet article explique comment restaurer la base de données AdventureWorks, mais vous pouvez utiliser n’importe quelle sauvegarde de base de données. 

> [!TIP]
> Téléchargez la [sauvegarde d’AdventureWorks](../samples/adventureworks-install-configure.md).

## <a name="copy-the-backup-file"></a>Copier le fichier de sauvegarde

Copiez le fichier de sauvegarde vers le conteneur SQL Server dans le pod de l’instance principale du cluster Kubernetes.

```bash
kubectl cp <path to .bak file> master-0:/var/tmp/<.bak filename> -c mssql-server -n <name of your big data cluster>
```

Exemple :

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak master-0:/var/tmp/AdventureWorks2016CTP3.bak -c mssql-server -n clustertest
```

Ensuite, vérifiez que le fichier de sauvegarde a été copié dans le conteneur du pod.

```bash
kubectl exec -it master-0 -n <name of your big data cluster> -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

Exemple :

```bash
kubectl exec -it master-0 -n clustertest -c mssql-server -- bin/bash
cd /var/
ls /tmp
exit
```

## <a name="restore-the-backup-file"></a>Restaurer le fichier de sauvegarde

Ensuite, restaurez la sauvegarde de la base de données sur l’instance principale SQL Server.  Si vous restaurez une sauvegarde de base de données qui a été créée sur Windows, vous devrez obtenir les noms des fichiers.  Dans Azure Data Studio, connectez-vous à l’instance principale et exécutez ce script SQL :

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/<db file name>.bak'
```

Exemple :

```sql
RESTORE FILELISTONLY FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
```

![Liste des fichiers de la sauvegarde](media/restore-database/database-restore-file-list.png)

Maintenant, restaurez la base de données. Le script suivant en est un exemple. Remplacez les noms/chemins selon les besoins en fonction de la sauvegarde de votre base de données.

```sql
RESTORE DATABASE AdventureWorks2016CTP3
FROM DISK='/tmp/AdventureWorks2016CTP3.bak'
WITH MOVE 'AdventureWorks2016CTP3_Data' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Data.mdf',
        MOVE 'AdventureWorks2016CTP3_Log' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_Log.ldf',
        MOVE 'AdventureWorks2016CTP3_mod' TO '/var/opt/mssql/data/AdventureWorks2016CTP3_mod'
```

## <a name="configure-data-pool-and-hdfs-access"></a>Configurer l’accès aux pools de données et à HDFS

À présent, pour que l’instance principale SQL Server accède aux pools de données et à HDFS, exécutez les procédures stockées du pool de données et du pool de stockage. Exécutez les scripts Transact-SQL suivants sur votre base de données nouvellement restaurée :

```sql
USE AdventureWorks2016CTP3
GO
-- Create the SqlDataPool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlDataPool')
  CREATE EXTERNAL DATA SOURCE SqlDataPool
  WITH (LOCATION = 'sqldatapool://controller-svc/default');

-- Create the SqlStoragePool data source:
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
   CREATE EXTERNAL DATA SOURCE SqlStoragePool
   WITH (LOCATION = 'sqlhdfs://controller-svc/default');
GO
```

> [!NOTE]
> Vous devez exécuter ces scripts de configuration seulement pour les bases de données restaurées à partir de versions antérieures de SQL Server. Si vous créez une base de données dans l’instance principale SQL Server, les procédures stockées du pool de données et du pool de stockage sont déjà configurées pour vous.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez la vue d’ensemble suivante :

- [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)
