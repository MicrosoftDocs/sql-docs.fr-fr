---
title: Déployer un conteneur SQL Server avec Azure Kubernetes Services (AKS)
description: Ce didacticiel explique comment déployer une solution de haute disponibilité SQL Server avec Kubernetes sur Azure Kubernetes Service.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/01/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 53d63320bc72b0c5f0e1891b67fd41b81d46458e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337298"
---
# <a name="deploy-a-sql-server-container-in-kubernetes-with-azure-kubernetes-services-aks"></a>Déployer un conteneur SQL Server dans Kubernetes avec Azure Kubernetes Services (AKS)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Découvrez comment configurer une instance sur Kubernetes dans Azure Kubernetes Service (AKS), avec un stockage persistant pour la haute disponibilité (HA). La solution offre une résilience. En cas d’échec de l’instance, Kubernetes la recrée automatiquement dans un nouveau pod. Kubernetes fournit également une résilience contre les défaillances de nœud.

Ce didacticiel explique comment configurer une instance à haut niveau de disponibilité dans un conteneur sur AKS.

> [!div class="checklist"]
> * Créer un mot de passe AS
> * Créer un stockage
> * Créer le déploiement
> * Se connecter à SQL Server Management Studio (SSMS)
> * Vérifier la défaillance et la récupération

## <a name="ha-solution-on-kubernetes-running-in-azure-kubernetes-service"></a>Solution de haute disponibilité sur Kubernetes s’exécutant dans Azure Kubernetes Service

Kubernetes 1.6 et versions ultérieures prend en charge les [classes de stockage](https://kubernetes.io/docs/concepts/storage/storage-classes/), [les revendications de volume persistant](https://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims) et le [type de volume de disque Azure](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk). Vous pouvez créer et gérer vos instances en mode natif dans Kubernetes. L’exemple de cet article indique comment créer un [déploiement](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) pour obtenir une configuration de haute disponibilité similaire à une instance de cluster de basculement de disque partagé. Dans cette configuration, Kubernetes joue le rôle de l’orchestrateur de cluster. En cas d’échec d’une instance dans un conteneur, l’orchestrateur amorce une autre instance du conteneur qui se joint au même stockage persistant.

![Conteneur SQL Server sur cluster Kubernetes](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql.png)

Dans le diagramme précédent, `mssql-server` est un conteneur dans un [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/). Kubernetes orchestre les ressources dans le cluster. Un [jeu de réplicas](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) garantit que le pod est automatiquement récupéré après une défaillance de nœud. Les applications se connectent au service. Dans ce cas, le service représente un équilibreur de charge qui héberge une adresse IP restant la même après la défaillance de `mssql-server`.

Dans le diagramme suivant, le conteneur `mssql-server` a échoué. En tant qu’orchestrateur, Kubernetes garantit le nombre correct d’instances saines dans le jeu de réplicas et démarre un nouveau conteneur en fonction de la configuration. L’orchestrateur démarre un nouveau pod sur le même nœud et `mssql-server` se reconnecte au même stockage persistant. Le service se connecte à `mssql-server` recréé.

![Échec du pod SQL Server sur cluster Kubernetes](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql-after-pod-fail.png)

Dans le diagramme suivant, le nœud hébergeant le conteneur `mssql-server` a échoué. L’orchestrateur démarre un nouveau pod sur un nœud différent et `mssql-server` se reconnecte au même stockage persistant. Le service se connecte à `mssql-server` recréé.

![Récupération de pod SQL Server sur cluster Kubernetes](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql-after-node-fail.png)

## <a name="prerequisites"></a>Prérequis

* **Cluster Kubernetes**
   - Ce didacticiel requiert un cluster Kubernetes. Les étapes utilisent [kubectl](https://kubernetes.io/docs/user-guide/kubectl/) pour gérer le cluster. 

   - Consultez [Déployer un cluster Azure Kubernetes Service (AKS)](/azure/aks/tutorial-kubernetes-deploy-cluster) pour créer et se connecter à un cluster Kubernetes à nœud unique dans AKS avec `kubectl`. 

   >[!NOTE]
   >Pour une protection contre les défaillances de nœuds, un cluster Kubernetes requiert plusieurs nœuds.

* **Azure CLI 2.0.23**
   - Les instructions de ce didacticiel ont été validées par rapport à Azure CLI 2.0.23.

## <a name="create-an-sa-password"></a>Créer un mot de passe AS

Créez un mot de passe AS dans le cluster Kubernetes. Kubernetes peut gérer des informations de configuration sensibles, comme des mots de passe en tant que [secrets](https://kubernetes.io/docs/concepts/configuration/secret/).

La commande suivante crée un mot de passe pour le compte AS :

   ```console
   kubectl create secret generic mssql --from-literal=SA_PASSWORD="MyC0m9l&xP@ssw0rd"
   ```  

   Remplacez `MyC0m9l&xP@ssw0rd` par un mot de passe complexe.

   Pour créer un secret dans Kubernetes nommé `mssql` qui contient la valeur `MyC0m9l&xP@ssw0rd` pour le `SA_PASSWORD`, exécutez la commande.


## <a name="create-storage"></a>Créer un stockage

Configurez un [volume persistant](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) et une [revendication de volume persistant](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volume-claim-protection) dans le cluster Kubernetes. Suivez les étapes ci-dessous : 

1. Créez un manifeste pour définir la classe de stockage et la revendication de volume persistant.  Le manifeste spécifie le fournisseur de stockage, les paramètres et la [stratégie de récupération](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#reclaiming). Le cluster Kubernetes utilise ce manifeste pour créer le stockage persistant. 

   L’exemple de YAML suivant définit une classe de stockage et une revendication de volume persistant. Le fournisseur de classe de stockage est `azure-disk`, car ce cluster Kubernetes est dans Azure. Le type de compte de stockage est `Standard_LRS`. La revendication de volume persistant est nommée `mssql-data`. Les métadonnées de revendication de volume persistant incluent une annotation qui les reconnecte à la classe de stockage. 

   ```yaml
   kind: StorageClass
   apiVersion: storage.k8s.io/v1
   metadata:
        name: azure-disk
   provisioner: kubernetes.io/azure-disk
   parameters:
     storageaccounttype: Standard_LRS
     kind: Managed
   ---
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: mssql-data
     annotations:
       volume.beta.kubernetes.io/storage-class: azure-disk
   spec:
     accessModes:
     - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi
   ```

   Enregistrez le fichier (par exemple, **pvc.yaml**).

1. Créez la revendication de volume persistant dans Kubernetes.

   ```console
   kubectl apply -f <Path to pvc.yaml file>
   ```

   `<Path to pvc.yaml file>` est l’emplacement où vous avez enregistré le fichier.

   Le volume persistant est automatiquement créé en tant que compte de stockage Azure et lié à la revendication de volume persistant. 

    ![Capture d’écran de la commande de revendication de volume persistant](media/tutorial-sql-server-containers-kubernetes/02_pvc_cmd.png)

1. Vérifiez la revendication de volume persistant.

   ```console
   kubectl describe pvc <PersistentVolumeClaim>
   ```

   `<PersistentVolumeClaim>` est le nom de la revendication de volume persistant.

   Dans l’étape précédente, la revendication de volume persistant est nommée `mssql-data`. Pour afficher les métadonnées relatives à la revendication de volume persistant, exécutez la commande suivante :

   ```console
   kubectl describe pvc mssql-data
   ```

   Les métadonnées retournées incluent une valeur appelée `Volume`. Cette valeur correspond au nom du blob.

   ![Capture d’écran des métadonnées retournées, y compris le volume](media/tutorial-sql-server-containers-kubernetes/describe-volume.png)

   La valeur de volume correspond à une partie du nom du blob dans l’image suivante du Portail Azure : 

   ![Capture d’écran du nom du blob du Portail Azure](media/tutorial-sql-server-containers-kubernetes/describe-volume-portal.png)

1. Vérifiez le volume persistant.

   ```console
   kubectl describe pv
   ```

   `kubectl` retourne les métadonnées relatives au volume persistant automatiquement créé et lié à la revendication de volume persistant. 

## <a name="create-the-deployment"></a>Créer le déploiement

Dans cet exemple, le conteneur qui héberge l’instance est décrit comme un objet de déploiement Kubernetes. Le déploiement crée un jeu de réplicas. Le jeu de réplicas crée le pod. 

Dans cette étape, créez un manifeste pour décrire le conteneur en fonction de l'image de Docker [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server) de SQL Server. Le manifeste fait référence à la revendication de volume persistant `mssql-server` et au secret `mssql` que vous avez déjà appliqué au cluster Kubernetes. Le manifeste décrit également un [service](https://kubernetes.io/docs/concepts/services-networking/service/). Ce service est un équilibreur de charge. L’équilibreur de charge garantit que l’adresse IP persiste après la récupération de l’instance. 

1. Créez un manifeste (fichier YAML) pour décrire le déploiement. L’exemple suivant décrit un déploiement, y compris un conteneur basé sur l’image de conteneur SQL Server.

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: mssql-deployment
   spec:
     replicas: 1
     selector:
        matchLabels:
          app: mssql
     template:
       metadata:
         labels:
           app: mssql
       spec:
         terminationGracePeriodSeconds: 30
         hostname: mssqlinst
         securityContext:
           fsGroup: 10001
         containers:
         - name: mssql
           image: mcr.microsoft.com/mssql/server:2019-latest
           ports:
           - containerPort: 1433
           env:
           - name: MSSQL_PID
             value: "Developer"
           - name: ACCEPT_EULA
             value: "Y"
           - name: SA_PASSWORD
             valueFrom:
               secretKeyRef:
                 name: mssql
                 key: SA_PASSWORD 
           volumeMounts:
           - name: mssqldb
             mountPath: /var/opt/mssql
         volumes:
         - name: mssqldb
           persistentVolumeClaim:
             claimName: mssql-data
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: mssql-deployment
   spec:
     selector:
       app: mssql
     ports:
       - protocol: TCP
         port: 1433
         targetPort: 1433
     type: LoadBalancer
   ```

   Copiez le code précédent dans un nouveau fichier, nommé `sqldeployment.yaml`. Utilisez les valeurs suivantes : 

   * MSSQL_PID `value: "Developer"` : Définit le conteneur pour qu’il exécute l’édition SQL Server Développeur. L’édition Développeur n’est pas concédée sous licence pour les données de production. Si le déploiement est destiné à une utilisation en production, définissez l'édition appropriée (`Enterprise`, `Standard` ou `Express`). 

      >[!NOTE]
      >Pour plus d'informations, consultez [Comment installer SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

   * `persistentVolumeClaim`: Cette valeur requiert une entrée pour `claimName:` qui correspond au nom utilisé pour la revendication de volume persistant. Ce didacticiel utilise `mssql-data`. 

   * `name: SA_PASSWORD`: Configure l’image de conteneur pour définir le mot de passe AS, comme défini dans cette section.

     ```yaml
     valueFrom:
       secretKeyRef:
         name: mssql
         key: SA_PASSWORD
     ```

    Quand Kubernetes déploie le conteneur, il fait référence au secret nommé `mssql` pour obtenir la valeur du mot de passe.

   * `securityContext` : Un contexte de sécurité définit des paramètres de privilège et de contrôle d’accès pour un pod ou un conteneur. Dans ce cas, il est spécifié au niveau du pod, de sorte que tous les conteneurs (un seul ici) adhèrent à ce contexte de sécurité. Dans le contexte de sécurité, nous définissons le fsGroup avec la valeur 10001 (qui est le GID du groupe mssql). Tous les processus du conteneur font également partie de l’ID de groupe supplémentaire 10001 (mssql). Le propriétaire du volume /var/opt/mssql et tous les fichiers créés dans ce volume vont correspondre à l’ID de groupe 10001 (groupe mssql).

   >[!NOTE]
   >En utilisant le type de service `LoadBalancer`, l’instance =est accessible à distance (via Internet) sur le port 1433.

   Enregistrez le fichier (par exemple, **sqldeployment.yaml**).

1. Créez le déploiement.

   ```console
   kubectl apply -f <Path to sqldeployment.yaml file>
   ```

   `<Path to sqldeployment.yaml file>` est l’emplacement où vous avez enregistré le fichier.

   ![Capture d’écran de la commande de déploiement](media/tutorial-sql-server-containers-kubernetes/04_deploy_cmd.png)

   Le déploiement et le service sont créés. L’instance se trouve dans un conteneur, connecté au stockage persistant.

   Pour afficher l'état du pod, saisissez `kubectl get pod`.

   ![Capture d’écran de la commande Obtenir le pod](media/tutorial-sql-server-containers-kubernetes/05_get_pod_cmd.png)

   Dans l’image précédente, le pod présente l’état `Running`. Cet état indique que le conteneur est prêt. Cette opération peut prendre plusieurs minutes.

   >[!NOTE]
   >Une fois le déploiement créé, plusieurs minutes peuvent être nécessaires avant que le pod ne soit visible. Le délai est dû au fait que le cluster extrait l’image [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server) du Hub Docker. Une fois l’image extraite pour la première fois, les déploiements ultérieurs peuvent être plus rapides si le déploiement se fait sur un nœud sur lequel l’image est déjà mise en cache. 

1. Vérifiez que les services sont en cours d'exécution. Exécutez la commande suivante :

   ```console
   kubectl get services 
   ```

   Cette commande retourne des services en cours d’exécution, ainsi que les adresses IP internes et externes des services. Notez l’adresse IP externe pour le service `mssql-deployment`. Utilisez cette adresse IP pour vous connecter à SQL Server. 

   ![Capture d’écran de la commande Obtenir le service](media/tutorial-sql-server-containers-kubernetes/06_get_service_cmd.png)

   Pour plus d’informations sur l’état des objets dans le cluster Kubernetes, exécutez :

   ```azurecli
   az aks browse --resource-group <MyResourceGroup> --name <MyKubernetesClustername>
   ```

1. Vous pouvez également vérifier que le conteneur s’exécute en tant que non-racine en exécutant la commande suivante :

    ```console
    kubectl.exe exec <name of SQL POD> -it -- /bin/bash 
    ```

    Ensuite, exécutez « whoami ». Vous devez obtenir le nom d’utilisateur mssql. Ce nom correspond à un utilisateur non-racine.

    ```console
    whoami
    ```

## <a name="connect-to-the-sql-server-instance"></a>Se connecter à l'instance

Si vous avez configuré le conteneur comme décrit, vous pouvez vous connecter à une application à partir de l’extérieur du réseau virtuel Azure. Utilisez le compte `sa` et l’adresse IP externe pour le service. Utilisez le mot de passe que vous avez configuré en tant que secret Kubernetes. 

Vous pouvez utiliser les applications suivantes pour vous connecter à l’instance. 

* [SSMS](./sql-server-linux-manage-ssms.md)

* [SSDT](./sql-server-linux-develop-use-ssdt.md)

* sqlcmd

   Pour se connecter avec `sqlcmd`, exécutez la commande suivante :

   ```cmd
   sqlcmd -S <External IP Address> -U sa -P "MyC0m9l&xP@ssw0rd"
   ```

   Remplacez les valeurs suivantes :

  * `<External IP Address>` par l’adresse IP pour le service `mssql-deployment` 
  * `MyC0m9l&xP@ssw0rd` par votre mot de passe

## <a name="verify-failure-and-recovery"></a>Vérifier la défaillance et la récupération

Pour vérifier l’échec et la récupération, vous pouvez supprimer le pod. Procédez comme suit :

1. Répertoriez le pod exécutant SQL Server.

   ```console
   kubectl get pods
   ```

   Notez le nom du pod exécutant SQL Server.

1. Supprimer le pod.

   ```console
   kubectl delete pod mssql-deployment-0
   ```

   `mssql-deployment-0` est la valeur retournée de l’étape précédente pour le nom du pod. 

Kubernetes recrée automatiquement le pod pour récupérer une instance et se connecter au stockage persistant. Utilisez `kubectl get pods` pour vérifier qu’un nouveau pod est déployé. Utilisez `kubectl get services` pour vérifier que l’adresse IP du nouveau conteneur est la même. 

## <a name="summary"></a>Résumé

Dans ce didacticiel, vous avez appris à déployer es conteneurs SQL Server sur un cluster Kubernetes pour la haute disponibilité. 

> [!div class="checklist"]
> * Créer un mot de passe AS
> * Créer un stockage
> * Créer le déploiement
> * Se connecter à SQL Server Management Studio (SSMS)
> * Vérifier la défaillance et la récupération

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
>[Présentation de Kubernetes](/azure/aks/intro-kubernetes)
