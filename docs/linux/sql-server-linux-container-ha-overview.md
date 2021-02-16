---
title: Haute disponibilité des conteneurs SQL Server
description: Découvrez en détail la haute disponibilité des conteneurs SQL Server. Découvrez également le déploiement d’un conteneur avec SQL Server sur Kubernetes.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/09/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: 168b1e11896ac29cf1d34037b1a28d7421fcfcc3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061454"
---
# <a name="high-availability-for-sql-server-containers"></a>Haute disponibilité des conteneurs SQL Server

Créer et gérer vos instances SQL Server en mode natif dans Kubernetes.

Déployez SQL Server sur des conteneurs Docker managés par [Kubernetes](https://kubernetes.io/). Dans Kubernetes, un conteneur avec une instance de SQL Server peut être récupéré automatiquement en cas de défaillance d’un nœud de cluster.

SQL Server 2017 fournit une image Docker qui peut être déployée sur Kubernetes. Vous pouvez configurer l’image avec une revendication de volume persistant Kubernetes (PVC). Kubernetes surveille le processus de SQL Server dans le conteneur. En cas d’échec du processus, du pod, du conteneur ou du nœud, Kubernetes démarre automatiquement une autre instance et se reconnecte au stockage.

## <a name="container-with-sql-server-instance-on-kubernetes"></a>Conteneur avec instance SQL Server sur Kubernetes

Kubernetes 1.6 et versions ultérieures prend en charge les [*classes de stockage*](https://kubernetes.io/docs/concepts/storage/storage-classes/), les [*revendications de volume persistant*](https://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims) et le [*type de volume de disque Azure*](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk). 

Dans cette configuration, Kubernetes joue le rôle de l’orchestrateur de conteneur. 

![Diagramme de la batterie de serveurs SQL Server Kubernetes](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql.png)

Dans le diagramme précédent, `mssql-server` est une instance SQL Server (conteneur) dans un [*pod*](https://kubernetes.io/docs/concepts/workloads/pods/pod/). Un [jeu de réplicas](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) garantit que le pod est automatiquement récupéré après une défaillance de nœud. Les applications se connectent au service. Dans ce cas, le service représente un équilibreur de charge qui héberge une adresse IP restant la même après la défaillance de `mssql-server`.

Kubernetes orchestre les ressources dans le cluster. Lorsqu’un nœud hébergeant un conteneur d’instance SQL Server échoue, il fait démarrer un nouveau conteneur avec une instance SQL Server et l’attache au même stockage persistant.

SQL Server 2017 et versions ultérieures prennent en charge les conteneurs sur Kubernetes.

Pour créer un conteneur dans Kubernetes, consultez [Déployer un conteneur SQL Server dans Kubernetes](tutorial-sql-server-containers-kubernetes.md)

## <a name="next-steps"></a>Étapes suivantes

Pour déployer des conteneurs SQL Server dans Azure Kubernetes service (AKS), consultez les exemples suivants :
* [Déployer SQL Server dans le conteneur Docker](./sql-server-linux-docker-container-deployment.md)
* [Déployer un conteneur SQL Server dans Kubernetes](tutorial-sql-server-containers-kubernetes.md)