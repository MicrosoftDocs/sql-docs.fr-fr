---
title: Contrôle RBAC Kubernetes
titleSuffix: SQL Server big data clusters
description: Cet article explique comment Clusters Big Data SQL Server utilise le contrôle RBAC avec Kubernetes.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a379b2bd1bb0e1992d70e1c86ae93163f6c02201
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046439"
---
# <a name="kubernetes-rbac-model--impact-on-users-and-service-accounts-managing-bdc"></a>Modèle RBAC Kubernetes et impact sur les utilisateurs et comptes de service qui gèrent des clusters Big Data

Cet article décrit les autorisations requises pour les utilisateurs qui gèrent des clusters Big Data et la sémantique associée au compte de service par défaut et à l’accès Kubernetes à partir du cluster Big Data.

> [!NOTE]
> Pour des ressources supplémentaires sur le modèle RBAC Kubernetes, consultez [Utilisation de l’autorisation RBAC – Kubernetes](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) et [Utilisation du contrôle RBAC pour définir et appliquer des autorisations – OpenShift](https://docs.openshift.com/container-platform/4.4/authentication/using-rbac.html).

## <a name="role-required-for-deployment"></a>Rôle nécessaire pour le déploiement

Clusters Big Data utilise des comptes de service (par exemple, `sa-mssql-controller` ou `master`) pour orchestrer le provisionnement des pods, des services, de la haute disponibilité, de la surveillance, etc. des clusters. Lorsque le déploiement Clusters Big Data démarre (par exemple, `azdata bdc create`), [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] effectue les opérations suivantes :

1. Il vérifie si l’espace de noms fourni existe.
2. S’il n’existe pas, il en crée un et applique l’étiquette `MSSQL_CLUSTER`.
3. Il crée le compte de service `sa-mssql-controller`.
4. Il crée un rôle de `<namespaced>-admin` possédant des autorisations complètes sur l’espace de noms ou le projet, mais pas d’autorisations au niveau du cluster.
5. Il crée une attribution au rôle pour ce compte de service.

Une fois ces étapes effectuées, les pods du plan de contrôle sont provisionnés et le compte de service déploie le reste du cluster Big Data.  

Par conséquent, l’utilisateur qui effectue le déploiement doit disposer des autorisations suivantes :

- Lister les espaces de noms dans le cluster (1)
- Corriger l’ancien ou le nouvel espace de noms avec l’étiquette (2)
- Créer le compte de service `sa-mssql-controller`, le rôle `<namespaced>-admin` et la liaison de rôle (3-5)

Dans la mesure où le rôle `admin` par défaut ne dispose pas de ces autorisations, l’utilisateur qui déploie le cluster Big Data doit posséder au moins des autorisations d’administrateur au niveau de l’espace de noms.

## <a name="cluster-role-required-for-pods-and-nodes-metrics-collection"></a>Rôle de cluster nécessaire pour la collecte de métriques sur les pods et les nœuds

À compter de SQL Server 2019 CU5, Telegraf exige un compte de service disposant d’autorisations de rôle à l’échelle du cluster pour collecter les métriques sur les pods et les nœuds. Lors du déploiement (ou de la mise à niveau de déploiements existants), nous tentons de créer le compte de service et le rôle de cluster nécessaires. Cependant, même si l’utilisateur qui déploie le cluster ou effectue la mise à niveau ne dispose pas d’autorisations suffisantes, le déploiement ou la mise à niveau continuera avec un avertissement et aboutira. Dans ce cas, les métriques sur les pods et les nœuds ne seront pas collectées. L’utilisateur qui déploie le cluster devra demander à un administrateur de cluster de créer le rôle et le compte de service (avant ou après le déploiement ou la mise à niveau). Clusters Big Data pourra alors les utiliser. 

Voici les étapes pour créer les artefacts requis :

1. Créez un fichier *metrics-role.yaml* avec le contenu ci-dessous. Veillez à remplacer les espaces réservés *<clusterName>* par le nom de votre cluster Big Data.

   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRole
   metadata:
     name: <clusterName>:cr-mssql-metricsdc-reader
   rules:
   - apiGroups:
     - '*'
     resources:
     - pods
     - nodes/stats
     verbs:
     - get
   ---
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: <clusterName>:crb-mssql-metricsdc-reader
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: <clusterName>:cr-mssql-metricsdc-reader
   subjects:
   - kind: ServiceAccount
     name: sa-mssql-metricsdc-reader
     namespace: <clusterName>
   ```

2. Créez le rôle de cluster et la liaison de rôle de cluster :

   ```bash
   kubectl create -f metrics-role.yaml
   ```

Le compte de service, le rôle de cluster et la liaison de rôle de cluster peuvent être créés avant ou après le déploiement Clusters Big Data. Kubernetes met automatiquement à jour l’autorisation du compte de service Telegraf. En cas de création dans le cadre d’un déploiement de pod, un délai de quelques minutes se produit dans la collecte des métriques sur les pods et les nœuds.

> [!NOTE]
> SQL Server 2019 CU5 introduit deux commutateurs de fonctionnalités pour contrôler la collecte de métriques sur les pods et les nœuds. Par défaut, ces paramètres ont la valeur true dans toutes les environnements cibles, à l’exception d’OpenShift où la valeur par défaut est remplacée. 

Vous pouvez personnaliser ces paramètres dans la section security du fichier de configuration de déploiement `control.json` :

```json
  "security": {
    …
    "allowNodeMetricsCollection": false,
    "allowPodMetricsCollection": false
  }
```

Si ces paramètres sont définis sur `false`, le flux de travail de déploiement Clusters Big Data ne tente pas de créer le compte de service, le rôle de cluster ni la liaison pour Telegraf.

## <a name="default-service-account-usage-from-within-a-bdc-pod"></a>Utilisation du compte de service par défaut à partir d’un pod BDC

Pour un modèle de sécurité plus strict, SQL Server 2019 CU5 a désactivé le montage par défaut pour le compte de service Kubernetes par défaut dans les pods de BDC. Cela s’applique aux déploiements nouveaux et mis à niveau dans la CU5 ou versions ultérieures.
Le jeton d’informations d’identification à l’intérieur des pods peut être utilisé pour accéder au serveur d’API Kubernetes, et le niveau d’autorisations dépend des paramètres de stratégie d’autorisation Kubernetes. Si vous avez des cas d’usage spécifiques qui requièrent le rétablissement du comportement CU5 précédent, dans la CU6, nous introduisons un nouveau commutateur de fonctionnalité qui vous permet d’activer le montage automatique uniquement au moment du déploiement. Pour ce faire, vous pouvez utiliser le fichier de déploiement de configuration control.json et définir *automountServiceAccountToken* sur *true*. Exécutez cette commande pour mettre à jour ce paramètre dans votre fichier config *control.json* personnalisé à l’aide de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] : 

``` bash
azdata bdc config replace -c custom-bdc/control.json -j "$.security.automountServiceAccountToken=true"
```
