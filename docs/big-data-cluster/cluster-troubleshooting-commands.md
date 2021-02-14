---
title: Résoudre les problèmes liés à Kubernetes
titleSuffix: SQL Server big data clusters
description: Cet article fournit des commandes utiles pour la supervision et la résolution des problèmes d’un cluster Big Data SQL Server 2019.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3f0d8ba02c56ca625094829770771c053e31802e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039639"
---
# <a name="troubleshoot-big-data-clusters-2019-kubernetes"></a>Résoudre les problèmes liés à [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article décrit plusieurs commandes Kubernetes utiles que vous pouvez utiliser pour superviser et dépanner un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Il montre comment visualiser les informations détaillées d’un pod ou d’autres artefacts Kubernetes situés dans le cluster Big Data. Cet article traite également de tâches courantes, comme la copie de fichiers vers ou depuis un conteneur exécutant un des services du cluster Big Data SQL Server.

> [!TIP]
> Pour la supervision des composants des clusters Big Data, vous pouvez utiliser les commandes [**azdata bdc status**](deployment-guidance.md#status) ou les [notebooks de résolution des problèmes intégrés](notebooks-manage-bdc.md) fournis avec Azure Data Studio.

> [!TIP]
> Exécutez les commandes **kubectl** suivantes sur une machine cliente Windows (cmd ou PS) ou Linux (bash). Elles nécessitent une authentification antérieure dans le cluster et un contexte de cluster pour s’exécuter. Par exemple, pour un cluster AKS créé précédemment, vous pouvez exécuter `az aks get-credentials --name <aks_cluster_name> --resource-group <azure_resource_group_name>` pour télécharger le fichier de configuration de cluster Kubernetes et définir le contexte de cluster.

## <a name="get-status-of-pods"></a>Obtenir l’état des pods

Vous pouvez utiliser la commande `kubectl get pods` pour obtenir l’état des pods dans le cluster pour tous les espaces de noms ou pour l’espace de noms du cluster Big Data. Les sections suivantes montrent des exemples pour les deux.

### <a name="show-status-of-all-pods-in-the-kubernetes-cluster"></a>Voir l’état de tous les pods dans le cluster Kubernetes

Exécutez la commande suivante pour obtenir tous les pods et leur état, y compris les pods qui font partie de l’espace de noms où les pods du cluster Big Data SQL Server sont créés :

```bash
kubectl get pods --all-namespaces
```

### <a name="show-status-of-all-pods-in-the-sql-server-big-data-cluster"></a>Voir l’état de toutes les pods dans le cluster Big Data SQL Server

Utilisez le paramètre `-n` pour spécifier un espace de noms spécifique. Notez que les pods de cluster Big Data SQL Server sont créés dans un nouvel espace de noms créé au moment de l’amorçage du cluster, d’après le nom du cluster spécifié dans le fichier de configuration de déploiement. Le nom par défaut, `mssql-cluster`, est utilisé ici.

```bash
kubectl get pods -n mssql-cluster
```

La sortie doit se présenter comme la liste suivante pour un cluster Big Data en cours d’exécution :

```output
PS C:\> kubectl get pods -n mssql-cluster
NAME              READY   STATUS    RESTARTS   AGE
appproxy-f2qqt    2/2     Running   0          110m
compute-0-0       3/3     Running   0          110m
control-zlncl     4/4     Running   0          118m
data-0-0          3/3     Running   0          110m
data-0-1          3/3     Running   0          110m
gateway-0         2/2     Running   0          109m
logsdb-0          1/1     Running   0          112m
logsui-jtdnv      1/1     Running   0          112m
master-0          7/7     Running   0          110m
metricsdb-0       1/1     Running   0          112m
metricsdc-shv2f   1/1     Running   0          112m
metricsui-9bcj7   1/1     Running   0          112m
mgmtproxy-x6gcs   2/2     Running   0          112m
nmnode-0-0        1/1     Running   0          110m
storage-0-0       7/7     Running   0          110m
storage-0-1       7/7     Running   0          110m
```

> [!NOTE]
> Pendant un déploiement, les pods avec un **ÉTAT** **ContainerCreating** sont toujours en cours de création. Si le déploiement se bloque pour une raison quelconque, cela peut vous donner une idée de l’origine du problème. Examinez également la colonne **PRÊT**. Ceci vous indique le nombre de conteneurs démarrés dans le pod. Notez que les déploiements peuvent prendre 30 minutes ou plus, selon votre configuration et votre réseau. La majeure partie de ce temps est consacrée au téléchargement des images de conteneur pour différents composants.

## <a name="get-pod-details"></a>Obtenir les détails d’un pod

Exécutez la commande suivante pour obtenir une description détaillée d’un pod spécifique au format JSON. Elle comprend des informations détaillées, comme le nœud Kubernetes actuel sur lequel le pod est placé, les conteneurs s’exécutant dans le pod et l’image utilisée pour démarrer les conteneurs. Elle montre également d’autres détails, comme les étiquettes, l’état et les revendications de volumes persistants qui sont associés au pod.

```bash
kubectl describe pod  <pod_name> -n <namespace_name>
```

L’exemple suivant montre les détails pour le pod `master-0` dans un cluster Big Data nommé `mssql-cluster` :

```bash
kubectl describe pod  master-0 -n mssql-cluster
```

Si des erreurs se sont produites, vous pouvez parfois voir l’erreur dans les événements récents pour le pod.

## <a name="get-pod-logs"></a>Obtenir les journaux d’un pod

Vous pouvez récupérer les journaux pour les conteneurs qui s’exécutent dans un pod. La commande suivante récupère les journaux pour tous les conteneurs s’exécutant dans le pod nommé `master-0` et les place dans un fichier nommé `master-0-pod-logs.txt` :

```bash
kubectl logs master-0 --all-containers=true -n mssql-cluster > master-0-pod-logs.txt
```

## <a name="get-status-of-services"></a><a id="services"></a> Obtenir l’état des services

Exécutez la commande suivante pour obtenir des détails sur les services du cluster Big Data. Ces détails incluent leur type et les adresses IP associées aux services et aux ports respectifs. Notez que les services de cluster Big Data SQL Server sont créés dans un nouvel espace de noms créé au moment de l’amorçage du cluster, d’après le nom du cluster spécifié dans le fichier de configuration de déploiement.

```bash
kubectl get svc -n <namespace_name>
```

L’exemple suivant montre l’état des services dans un cluster Big Data nommé `mssql-cluster` :

```bash
kubectl get svc -n mssql-cluster
```

Les services suivants prennent en charge les connexions externes au cluster Big Data :

| Service | Description |
|---|---|
| **master-svc-external** | Fournit l’accès à l’instance principale.<br/>(**EXTERNAL-IP,31433** et l’utilisateur **SA**) |
| **controller-svc-external** | Prend en charge les outils et les clients qui gèrent le cluster. |
| **gateway-svc-external** | Fournit l’accès à la passerelle HDFS/Spark.<br/>(**EXTERNAL-IP** et l’utilisateur `<AZDATA_USERNAME>`)<sup>1</sup>|
| **appproxy-svc-external** | Prend en charge les scénarios de déploiement d’applications. |

<sup>1</sup> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

> [!TIP]
> Il s’agit d’un moyen de voir les services avec **kubectl**, mais il est également possible d’utiliser la commande `azdata bdc endpoint list` pour voir ces points de terminaison. Pour plus d’informations, consultez [Obtenir les points de terminaison d’un cluster Big Data](deployment-guidance.md#endpoints).

## <a name="get-service-details"></a>Obtenir les détails d’un service

Exécutez cette commande pour obtenir une description détaillée d’un service au format JSON. Il inclut des détails comme les étiquettes, le sélecteur, l’adresse IP, l’adresse IP externe (si le service est de type LoadBalancer), le port, etc.

```bash
kubectl describe service <service_name> -n <namespace_name>
```

L’exemple suivant récupère les détails du service **master-svc-external** :

```bash
kubectl describe service master-svc-external -n mssql-cluster
```

## <a name="copy-files"></a><a id="copy"></a> Copier des fichiers

Si vous devez copier des fichiers depuis le conteneur sur votre machine locale, utilisez la commande `kubectl cp` avec la syntaxe suivante :

```bash
kubectl cp <pod_name>:<source_file_path> -c <container_name> -n <namespace_name> <target_local_file_path>
```

De même, vous pouvez utiliser `kubectl cp` pour copier des fichiers depuis la machine locale dans un conteneur spécifique.

```bash
kubectl cp <source_local_file_path> <pod_name>:<target_container_path> -c <container_name>  -n <namespace_name>
```

### <a name="copy-files-from-a-container"></a><a id="copyfrom"></a> Copier des fichiers à partir d’un conteneur

L’exemple suivant copie les fichiers journaux de SQL Server depuis le conteneur dans le chemin `~/temp/sqlserverlogs` sur la machine locale (dans cet exemple, la machine locale est un client Linux) :

```bash
kubectl cp master-0:/var/opt/mssql/log -c mssql-server -n mssql-cluster ~/tmp/sqlserverlogs
```

### <a name="copy-files-into-container"></a><a id="copyinto"></a> Copier des fichiers dans un conteneur

L’exemple suivant copie le fichier **AdventureWorks2016CTP3.bak** depuis la machine locale dans le conteneur de l’instance principale SQL Server (`mssql-server`) dans le pod `master-0`. Le fichier est copié dans le répertoire `/tmp` dans le conteneur. 

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak master-0:/tmp -c mssql-server -n mssql-cluster
```

## <a name="force-delete-a-pod"></a><a id="forcedelete"></a> Forcer la suppression d’un pod
 
Il n’est pas recommandé de forcer la suppression d’un pod. Cependant, pour tester la disponibilité, la résilience ou la persistance des données, vous pouvez supprimer un pod pour simuler la défaillance d’un pod avec la commande `kubectl delete pods`.

```bash
kubectl delete pods <pod_name> -n <namespace_name> --grace-period=0 --force
```

L’exemple suivant supprime le pod du pool de stockage, `storage-0-0` :

```bash
kubectl delete pods storage-0-0 -n mssql-cluster --grace-period=0 --force
```

## <a name="get-pod-ip"></a><a id="getip"></a> Obtenir l’adresse IP d’un pod
 
À des fins de dépannage, il peut être nécessaire d’obtenir l’adresse IP du nœud sur lequel un pod est en cours d’exécution. Pour obtenir l’adresse IP, utilisez la commande `kubectl get pods` avec la syntaxe suivante :

```bash
kubectl get pods <pod_name> -o yaml -n <namespace_name> | grep hostIP
```

L’exemple suivant obtient l’adresse IP du nœud sur lequel le pod `master-0` s’exécute :

```bash
kubectl get pods master-0 -o yaml -n mssql-cluster | grep hostIP
```

## <a name="kubernetes-dashboard"></a>Tableau de bord Kubernetes

Vous pouvez lancer le tableau de bord Kubernetes pour obtenir des informations supplémentaires sur le cluster. Les sections suivantes expliquent comment lancer le tableau de bord pour Kubernetes sur AKS et pour Kubernetes amorcé avec kubeadm.
 
### <a name="start-dashboard-when-cluster-is-running-in-aks"></a>Démarrer le tableau de bord quand le cluster s’exécute dans AKS

Pour lancer l’exécution du tableau de bord Kubernetes :

```bash
az aks browse --resource-group <azure_resource_group> --name <aks_cluster_name>
```

> [!Note]
> Si vous obtenez l’erreur suivante : *Impossible d’écouter sur le port 8001 : Échec de la création de tous les écouteurs avec les erreurs suivantes : Impossible de créer l’écouteur : Erreur d’écoute tcp4 127.0.0.1:8001 : >liaison : Une seule utilisation de chaque adresse de socket (protocole/adresse réseau/port) est habituellement autorisée. Impossible de créer l’écouteur : Erreur d’écoute tcp6: adresse [[:: 1]]: 8001 : port manquant dans >erreur d’adresse : Impossible d’écouter sur les ports demandés : [{8001 9090}]* , vérifiez que vous n’avez pas déjà démarré le tableau de bord à partir d’une autre fenêtre.

Quand vous lancez le tableau de bord sur votre navigateur, vous pouvez obtenir des avertissements liés aux autorisations en raison de l’activation par défaut de RBAC dans les clusters AKS et du fait que le compte de service utilisé par le tableau de bord ne dispose pas d’autorisations suffisantes pour accéder à toutes les ressources (par exemple, *pods est interdit : L’utilisateur « system:serviceaccount:kube-system:kubernetes-dashboard » ne peut pas lister les pods de l’espace de noms « default »* ). Exécutez la commande suivante pour accorder les autorisations nécessaires à `kubernetes-dashboard`, puis redémarrez le tableau de bord :

```bash
kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

### <a name="start-dashboard-when-kubernetes-cluster-is-bootstrapped--using-kubeadm"></a>Démarrer le tableau de bord quand le cluster Kubernetes est amorcé avec kubeadm

Pour obtenir des instructions détaillées sur le déploiement et la configuration du tableau de bord dans votre cluster Kubernetes, consultez [la documentation de Kubernetes](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/). Pour lancer le tableau de bord Kubernetes, exécutez cette commande :

```bash
kubectl proxy
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les clusters Big Data, consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] ?](big-data-cluster-overview.md).
