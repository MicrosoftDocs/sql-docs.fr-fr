---
title: Restauration des autorisations HDFS
titleSuffix: SQL Server Big Data Cluster
description: Restaurez les droits d’administrateur HDFS.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/21/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d914b2006656162b233351815e238f282d73e913
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045529"
---
# <a name="restore-hdfs-permissions"></a>Restauration des autorisations HDFS

Les modifications apportées aux listes de contrôle d’accès (ACL) HDFS peuvent avoir affecté les dossiers `/system` et `/tmp` dans HDFS. La cause la plus probable de la modification de la liste de contrôle d’accès est la manipulation manuelle des ACL des dossiers par l’utilisateur. Les modifications directes des autorisations dans le dossier /system et le dossier/tmp/logs ne sont pas prises en charge.

## <a name="symptom"></a>Symptôme

Une tâche Spark est envoyée par le biais d’ADS et échoue avec l’erreur d’initialisation de SparkContext et AccessControlException.

```
583 ERROR spark.SparkContext: Error initializing SparkContext.
org.apache.hadoop.security.AccessControlException: Permission denied: user=<UserAccount>, access=WRITE, inode="/system/spark-events":sph:BDCAdmin:drwxr-xr-x
```

L’interface utilisateur de Yarn affiche l’ID de l’application dans l’état KILLED.

Lorsque vous essayez d’écrire dans le dossier en tant qu’utilisateur de domaine, cela échoue également. Vous pouvez tester avec l’exemple suivant :

```bash
kinit <UserAccount>
hdfs dfs -touch /system/spark-events/test
hdfs dfs -rm /system/spark-events/test
```

## <a name="cause"></a>Cause

Les listes de contrôle d’accès HDFS ont été modifiées pour le groupe de sécurité de domaine utilisateur BDC. Les modifications possibles incluaient les ACL pour les dossiers /system et /tmp. Les modifications de ces dossiers ne sont pas prises en charge.

Vérifiez l’effet dans les journaux Livy :

```
INFO utils.LineBufferedStream: YYYY-MM-DD-HH:MM:SS,858 INFO yarn.Client: Application report for application_1580771254352_0041 (state: ACCEPTED)
...
WARN rsc.RSCClient: Client RPC channel closed unexpectedly.
INFO interactive.InteractiveSession: Failed to ping RSC driver for session <ID>. Killing application
```

L’interface utilisateur de YARN affiche les applications en état KILLED pour l’ID de l’application.

Pour obtenir la cause racine de la fermeture de la connexion RPC, recherchez dans le journal des applications YARN l’application correspondant à l’application. Dans l’exemple précédent, cela se rapporte à `application_1580771254352_0041`. Utilisez `kubectl` pour vous connecter au pod sparkhead-0, puis exécutez la commande suivante :

La commande suivante interroge le journal YARN de l’application.

```bash
yarn logs -applicationId application_1580771254352_0041
```

Dans les résultats ci-dessous, l’autorisation est refusée pour l’utilisateur. 

```
YYYY-MM-DD-HH:MM:SS,583 ERROR spark.SparkContext: Error initializing SparkContext.
org.apache.hadoop.security.AccessControlException: Permission denied: user=user1, access=WRITE, inode="/system/spark-events":sph:BDCAdmin:drwxr-xr-x
```

La cause peut être que l’utilisateur BDC a été ajouté de manière récursive au dossier racine HDFS. Cela peut avoir affecté les autorisations par défaut.

## <a name="resolution"></a>Résolution

Restaurez les autorisations avec le script suivant : Utilisez `kinit` avec l’administrateur :

```bash
hdfs dfs -chmod 733 /system/spark-events
hdfs dfs -setfacl --set default:user:sph:rwx,default:other::--- /system/spark-events
hdfs dfs -setfacl --set default:user:app-setup:r-x,default:other::--- /system/appdeploy
hadoop fs -chmod 733 /tmp/logs
hdfs dfs -setfacl --set default:user:yarn:rwx,default:other::--- /tmp/logs
```
