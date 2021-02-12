---
title: Résolution des problèmes liés au notebook `pyspark`
titleSuffix: SQL Server Big Data Cluster
description: Résolvez les problèmes liés au notebook `pyspark`.
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mikeray
ms.date: 06/01/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 02ac9f72b7cd688cd38bdc5e785732e8b97bc2dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043399"
---
# <a name="troubleshoot-pyspark-notebook"></a>Résolution des problèmes liés au notebook `pyspark`

Cet article explique comment résoudre les problèmes provoquant l’échec d’un notebook `pyspark`.

## <a name="architecture-of-a-pyspark-job-under-azure-data-studio"></a>Architecture d’un travail PySpark sous Azure Data Studio

Azure Data Studio communique avec le point de terminaison `livy` sur Clusters Big Data SQL Server. 

Le point de terminaison `livy` émet des commandes `spark-submit` au sein du cluster Big Data. Chaque commande `spark-submit` possède un paramètre qui spécifie YARN comme Gestionnaire des ressources clusters.

Pour résoudre efficacement les problèmes de votre session PySpark, vous devez collecter et examiner les journaux de chaque couche : Livy, YARN et Spark.

Cette procédure de dépannage comporte plusieurs prérequis :

1. Avoir installé [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] et défini correctement la configuration sur le cluster.
2. Savoir exécuter des commandes Linux et posséder quelques compétences en résolution des problèmes liés aux journaux.

## <a name="troubleshooting-steps"></a>Étapes de dépannage

1. Passez en revue la pile et les messages d’erreur dans `pyspark`.

   Récupérez l’ID de l’application dans la première cellule du notebook. Servez-vous-en pour étudier les journaux `livy`, YARN et Spark. `SparkContext` utilise cet ID d’application YARN.

   :::image type="content" source="../big-data-cluster/media/troubleshoot-pyspark-notebook/1-failed-cell.png" alt-text="Cellule en échec":::

1. Récupérez les journaux.

   Utilisez `azdata bdc debug copy-logs` pour l’analyse.

   L’exemple suivant connecte un point de terminaison de cluster Big Data pour copier les journaux. Mettez à jour les valeurs suivantes dans l’exemple avant de l’exécuter.
   - `<ip_address>`: point de terminaison du cluster Big Data.
   - `<username>`: nom d’utilisateur du cluster Big Data.
   - `<namespace>`: espace de noms Kubernetes du cluster.
   - `<folder_to_copy_logs>`: chemin du dossier local où seront copiés les journaux.

   ```console
   azdata login --auth basic --username <username> --endpoint https://<ip_address>:30080
   azdata bdc debug copy-logs -n <namespace> -d <folder_to_copy_logs>
   ```

   Exemple de sortie

   ```output
   <user>@<server>:~$ azdata bdc debug copy-logs -n <namespace> -d copy_logs
   Collecting the logs for cluster '<namespace>'.
   Collecting logs for containers...
   Creating an archive from logs-tmp/<namespace>.
   Log files are archived in /home/<user>/copy_logs/debuglogs-<namespace>-YYYYMMDD-HHMMSS.tar.gz.
   Creating an archive from logs-tmp/dumps.
   Log files are archived in /home/<user>/copy_logs/debuglogs-<namespace>-YYYYMMDD-HHMMSS-dumps.tar.gz.
   Collecting the logs for cluster 'kube-system'.
   Collecting logs for containers...
   Creating an archive from logs-tmp/kube-system.
   Log files are archived in /home/<user>/copy_logs/debuglogs-kube-system-YYYYMMDD-HHMMSS.tar.gz.
   Creating an archive from logs-tmp/dumps.
   Log files are archived in /home/<user>/copy_logs/debuglogs-kube-system-YYYYMMDD-HHMMSS-dumps.tar.gz.
   ```

1. Examinez les journaux Livy. Ils se trouvent à l’emplacement suivant : `<namespace>\sparkhead-0\hadoop-livy-sparkhistory\supervisor\log`.

   - Recherchez l’ID d’application YARN dans la première cellule du notebook PySpark.
   - Recherchez le statut `ERR`.
   
   Voici un exemple de journal Livy dont l’état est `YARN ACCEPTED`. Livy a soumis l’application YARN.

   ```output
   HH:MM:SS INFO utils.LineBufferedStream: YYY-MM-DD HH:MM:SS INFO impl.YarnClientImpl: Submitted application application_<application_id>
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream: YYY-MM-DD HH:MM:SS INFO yarn.Client: Application report for application_<application_id> (state: ACCEPTED)
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream: YYY-MM-DD HH:MM:SS INFO yarn.Client: 
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      client token: N/A
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      diagnostics: N/A
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      ApplicationMaster host: N/A
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      ApplicationMaster RPC port: -1
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      queue: default
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      start time: ############
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      final status: UNDEFINED
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      tracking URL: https://sparkhead-1.fnbm.corp:8090/proxy/application_<application_id>/
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      user: <account>
   ```

1. Examinez l’interface utilisateur YARN.

   Récupérez l’URL du point de terminaison YARN sur le tableau de bord de gestion Clusters Big Data Azure Data Studio ou exécutez `azdata bdc endpoint list –o table`.

   Par exemple :

   ```console
   azdata bdc endpoint list -o table
   ```

   Retours

   ```output
   Description                                             Endpoint                                                          Name                        Protocol
   ------------------------------------------------------  ----------------------------------------------------------------  --------------------------  ----------
   Gateway to access HDFS files, Spark                     https://knox.<namespace-value>.local:30443                               gateway                     https
   Spark Jobs Management and Monitoring Dashboard          https://knox.<namespace-value>.local:30443/gateway/default/sparkhistory  spark-history               https
   Spark Diagnostics and Monitoring Dashboard              https://knox.<namespace-value>.local:30443/gateway/default/yarn          yarn-ui                     https
   Application Proxy                                       https://proxy.<namespace-value>.local:30778                              app-proxy                   https
   Management Proxy                                        https://bdcmon.<namespace-value>.local:30777                             mgmtproxy                   https
   Log Search Dashboard                                    https://bdcmon.<namespace-value>.local:30777/kibana                      logsui                      https
   Metrics Dashboard                                       https://bdcmon.<namespace-value>.local:30777/grafana                     metricsui                   https
   Cluster Management Service                              https://bdcctl.<namespace-value>.local:30080                             controller                  https
   SQL Server Master Instance Front-End                    sqlmaster.<namespace-value>.local,31433                                  sql-server-master           tds
   SQL Server Master Readable Secondary Replicas           sqlsecondary.<namespace-value>.local,31436                               sql-server-master-readonly  tds
   HDFS File System Proxy                                  https://knox.<namespace-value>.local:30443/gateway/default/webhdfs/v1    webhdfs                     https
   Proxy for running Spark statements, jobs, applications  https://knox.<namespace-value>.local:30443/gateway/default/livy/v1       livy                        https
   ```

1. Vérifiez l’ID de l’application et les différents journaux application_master et container.

   :::image type="content" source="media/troubleshoot-pyspark-notebook/15-hadoop-dashboard.png" alt-text="Examen de l’ID de l’application":::

1. Examinez les journaux des applications YARN.

   Récupérez le journal de l’application. Utilisez `kubectl` pour vous connecter au pod `sparkhead-0`, par exemple :
   
   ```console
   kubectl exec -it sparkhead-0 -- /bin/bash
   ```
      
   Maintenant, exécutez cette commande dans ce shell en utilisant le bon `application_id` :

   ```console
   yarn logs -applicationId application_<application_id>
   ```

1. Recherchez des erreurs ou des piles.

   Voici un exemple d’erreur d’autorisation vis-à-vis de HDFS. Dans la pile Java, recherchez `Caused by:` :

   ```output
   YYYY-MM-DD HH:MM:SS,MMM ERROR spark.SparkContext: Error initializing SparkContext.
   org.apache.hadoop.security.AccessControlException: Permission denied: user=<account>, access=WRITE, inode="/system/spark-events":sph:<bdc-admin>:drwxr-xr-x
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:399)
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:255)
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:193)
        at org.apache.hadoop.hdfs.server.namenode.FSDirectory.checkPermission(FSDirectory.java:1852)
        at org.apache.hadoop.hdfs.server.namenode.FSDirectory.checkPermission(FSDirectory.java:1836)
        at org.apache.hadoop.hdfs.server.namenode.FSDirectory.checkAncestorAccess(FSDirectory.java:1795)
        at org.apache.hadoop.hdfs.server.namenode.FSDirWriteFileOp.resolvePathForStartFile(FSDirWriteFileOp.java:324)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.startFileInt(FSNamesystem.java:2504)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.startFileChecked(FSNamesystem.java:2448)
   
   Caused by: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.security.AccessControlException): Permission denied: user=<account>, access=WRITE, inode="/system/spark-events":sph:<bdc-admin>:drwxr-xr-x
   ```

1. Examinez l’interface utilisateur Spark.

   :::image type="content" source="media/troubleshoot-pyspark-notebook/30-spark-ui.png" alt-text="Interface utilisateur Spark":::

   Descendez dans la hiérarchie des étapes pour trouver des erreurs dans les tâches.

## <a name="next-steps"></a>Étapes suivantes

[Résolution des problèmes d’intégration Active Directory Clusters Big Data SQL Server](troubleshoot-active-directory.md)