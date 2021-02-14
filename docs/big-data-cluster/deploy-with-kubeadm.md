---
title: Configurer Kubernetes avec kubeadm
titleSuffix: SQL Server Big Data Clusters
description: Découvrez comment configurer Kubernetes sur plusieurs machines (physiques ou virtuelles) Ubuntu 16.04 ou 18.04 pour les déploiements sur un cluster Big Data SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8c189f3a77f68dc212ead363b935016931b1dd6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038865"
---
# <a name="configure-kubernetes-on-multiple-machines-for-sql-server-big-data-cluster-deployments"></a>Configurer Kubernetes sur plusieurs machines pour les déploiements de cluster Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article fournit un exemple d’utilisation de **kubeadm** permettant de configurer Kubernetes sur plusieurs machines pour des déploiements [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Dans cet exemple, plusieurs machines Ubuntu 16.04 ou 18.04 LTS (physiques ou virtuelles) représentent la cible. Si vous effectuez le déploiement sur une autre plateforme Linux, vous devez changer certaines commandes pour qu’elles correspondent à votre système.  

> [!TIP] 
> Pour obtenir des exemples de scripts de configuration de Kubernetes, consultez [Créer un cluster Kubernetes à l’aide de Kubeadm sur Ubuntu 16.04 LTS ou 18.04 LTS.](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm)
Consultez également [cette](deployment-script-single-node-kubeadm.md) rubrique pour obtenir un exemple de script qui automatise le déploiement d’un seul nœud kubeadm sur une machine virtuelle, puis déploie une configuration par défaut du cluster Big Data par-dessus.

## <a name="prerequisites"></a>Prérequis

- Au minimum, 3 machines physiques ou machines virtuelles Linux
- Configuration recommandée par machine :
   - 8 CPU
   - 64 Go de mémoire
   - 100 Go de stockage
 
> [!Important] 
> Avant de démarrer le déploiement du cluster Big Data, vérifiez que les horloges sont synchronisées entre tous les nœuds Kubernetes ciblés par le déploiement. Le cluster Big Data intègre des propriétés d’intégrité temporaires pour différents services et des décalages d’horloge peuvent entraîner un état incorrect.

## <a name="prepare-the-machines"></a>Préparer les machines

Sur chaque machine, plusieurs prérequis doivent être remplis. Dans un terminal Bash, exécutez les commandes suivantes sur chaque machine :

1. Ajoutez la machine actuelle au fichier `/etc/hosts` :

   ```bash
   echo $(hostname -i) $(hostname) | sudo tee -a /etc/hosts
   ```

1. Désactivez l’échange de mémoire sur tous les appareils.

   ```bash
   sudo sed -i "/ swap / s/^/#/" /etc/fstab
   sudo swapoff -a
   ```

1. Importez les clés et inscrivez le dépôt pour Kubernetes.

   ```bash
   sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
   echo 'deb http://apt.kubernetes.io/ kubernetes-xenial main' | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
   ```

1. Configurez les prérequis de Docker et Kubernetes sur la machine.

   ```bash
   KUBE_DPKG_VERSION=1.15.0-00 #or your other target K8s version, which should be at least 1.13.
   sudo apt-get update && \
   sudo apt-get install -y ebtables ethtool && \
   sudo apt-get install -y docker.io && \
   sudo apt-get install -y apt-transport-https && \
   sudo apt-get install -y kubelet=$KUBE_DPKG_VERSION kubeadm=$KUBE_DPKG_VERSION kubectl=$KUBE_DPKG_VERSION && \
   curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash
   ```
 
1. Définissez `net.bridge.bridge-nf-call-iptables=1`. Sur Ubuntu 18.04, les commandes suivantes activent d’abord `br_netfilter`.

   ```bash
   . /etc/os-release
   if [ "$VERSION_CODENAME" == "bionic" ]; then sudo modprobe br_netfilter; fi
   sudo sysctl net.bridge.bridge-nf-call-iptables=1
   ```

## <a name="configure-the-kubernetes-master"></a>Configurer le maître Kubernetes

Après avoir exécuté les commandes précédentes sur chaque machine, choisissez l’une des machines à utiliser comme maître Kubernetes. Exécutez ensuite les commandes suivantes sur cette machine.

1. Commencez par créer un fichier rbac.yaml dans votre répertoire actif à l’aide de la commande suivante. 

   ```bash
   cat <<EOF > rbac.yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: default-rbac
   subjects:
   - kind: ServiceAccount
     name: default
     namespace: default
   roleRef:
     kind: ClusterRole
     name: cluster-admin
     apiGroup: rbac.authorization.k8s.io
   EOF
   ```

1. Initialisez le maître Kubernetes sur cette machine. L’exemple de script ci-dessous spécifie Kubernetes version `1.15.0`. La version que vous utilisez dépend de votre cluster Kubernetes.

   ```bash
   KUBE_VERSION=1.15.0
   sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --kubernetes-version=$KUBE_VERSION
   ```

   Vous devez voir une sortie indiquant que le maître Kubernetes a été initialisé correctement.

1. Notez la commande `kubeadm join` à utiliser sur les autres serveurs pour rejoindre le cluster Kubernetes. Copiez ceci pour l’utiliser plus tard.

   ![kubeadm join](./media/deploy-with-kubeadm/kubeadm-join.png)

1. Créez un fichier config Kubernetes dans votre répertoire de base.

   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```

1. Configurez le cluster et le tableau de bord Kubernetes.

   ```bash
   kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
   helm init
   kubectl apply -f rbac.yaml
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
   kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
   ```

## <a name="configure-the-kubernetes-agents"></a>Configurer les agents Kubernetes

Les autres machines servent d’agents Kubernetes dans le cluster. 

Sur chacune des autres machines, exécutez la commande `kubeadm join` que vous avez copiée à la section précédente.

![kubeadm join sur les agents](./media/deploy-with-kubeadm/kubeadm-join-agents.png)

## <a name="view-the-cluster-status"></a>Voir l’état du cluster

Pour vérifier la connexion à votre cluster, utilisez la commande [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands) pour retourner une liste des nœuds du cluster.

```bash
kubectl get nodes
```

## <a name="next-steps"></a>Étapes suivantes

Les étapes décrites dans cet article ont permis de configurer un cluster Kubernetes sur plusieurs machines Ubuntu. L’étape suivante consiste à déployer le cluster Big Data SQL Server 2019. Pour obtenir des instructions, consultez l’article suivant :

[Déployer SQL Server sur Kubernetes](deployment-guidance.md#deploy)
