---
title: Instances de cluster de basculement - SQL Server sur Linux
description: Les concepts liés aux instances de cluster de basculement SQL Server sur Linux incluent la couche de clustering, le nombre d’instances, le nom et l’adresse IP, et le stockage partagé.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 1839442f20e2a48ed612e42e3c654d9e3255a477
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346398"
---
# <a name="failover-cluster-instances---sql-server-on-linux"></a>Instances de cluster de basculement - SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique les concepts relatifs aux instances de cluster de basculement SQL Server sur Linux. 

Pour créer une instance de cluster de basculement SQL Server sur Linux, consultez [Configurer une instance de cluster de basculement SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure.md)

## <a name="the-clustering-layer"></a>La couche de clustering

* Dans RHEL, la couche de clustering est basée sur le [module complémentaire haute disponibilité](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) Red Hat Enterprise Linux (RHEL). 

    > [!NOTE] 
    > L’accès à la documentation et au module complémentaire HA de Red Hat requiert un abonnement. 

* Dans SLES, la couche de clustering est basée sur l’[Extension haute disponibilité (HAE)](https://www.suse.com/products/highavailability) de SUSE Linux Enterprise.

    Pour plus d’informations sur la configuration du cluster, les options de l’agent de ressources, la gestion, les meilleures pratiques et les suggestions, consultez [Extension haute disponibilité SUSE Linux Enterprise 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

Les modules complémentaires RHEL HA et SUSE HAE s’appuient sur [Pacemaker](https://clusterlabs.org/).

Comme le montre le diagramme suivant, le stockage est présenté à deux serveurs. Les composants de clustering, Corosync et Pacemaker, coordonnent les communications et la gestion des ressources. L’un des serveurs a la connexion active aux ressources de stockage et au SQL Server. Lorsque Pacemaker détecte une défaillance, les composants de clustering gèrent le déplacement des ressources vers l’autre nœud.  

![Cluster SQL de 7 disques partagés Red Hat Enterprise Linux](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 


> [!NOTE]
> À ce stade, l’intégration de SQL Server avec Pacemaker sur Linux n’est pas aussi couplée qu’avec WSFC sur Windows. À partir de SQL, il n’y a aucune connaissance de la présence du cluster, l’ensemble de l’orchestration est en extérieur-intérieur et le service est contrôlé comme une instance autonome par Pacemaker. En outre, le nom du réseau virtuel est spécifique à WSFC, il n’existe aucun équivalent dans le cas de Pacemaker. Il est prévu que @@servername et sys.servers retournent le nom du nœud, tandis que le cluster dmvs sys.dm_os_cluster_nodes et sys.dm_os_cluster_properties sys. ne conserveront aucun enregistrement. Pour utiliser une chaîne de connexion qui pointe vers un nom de serveur de chaîne et ne pas utiliser l’adresse IP, ils doivent inscrire sur leur serveur DNS l’adresse IP utilisée pour créer la ressource d’adresse IP virtuelle (comme expliqué dans les sections suivantes) avec le nom de serveur choisi.

## <a name="number-of-instances-and-nodes"></a>Nombrer d'instances et de nœuds

L’une des principales différences avec SQL Server sur Linux est qu’il ne peut y avoir qu’une seule installation de SQL Server par serveur Linux. Cette installation est appelée une instance. Cela signifie que contrairement à Windows Server qui prend en charge jusqu’à 25 instances de cluster de basculement par cluster de basculement Windows Server (WSFC), une instance de cluster de basculement basé sur Linux n’a qu’une seule instance. Cette instance est également une instance par défaut ; Il n’existe pas de concept d’instance nommée sur Linux. 

Un cluster Pacemaker peut comporter jusqu’à 16 nœuds lorsque Corosync est impliqué, donc une instance de cluster de basculement unique peut s’étendre sur 16 serveurs. Une instance de cluster de basculement implémentée avec l’Édition Standard de SQL Server prend en charge jusqu’à deux nœuds d’un cluster, même si le cluster Pacemaker a le maximum de 16 nœuds.

Dans une instance de cluster de basculement SQL Server, l’instance est active sur un nœud ou sur l’autre.

## <a name="ip-address-and-name"></a>Adresse IP et nom
Sur un cluster Pacemaker Linux, chaque instance de cluster de basculement SQL Server a besoin d’une adresse IP et d’un nom uniques. Si la configuration de l’instance de cluster de basculement s’étend sur plusieurs sous-réseaux, une adresse IP est requise par sous-réseau. Le nom unique et la ou les adresses IP sont utilisés pour accéder à l’instance de cluster de basculement afin que les applications et les utilisateurs finaux n’aient pas besoin de connaître le serveur sous-jacent du cluster Pacemaker.

Le nom de l’instance de cluster de basculement dans DNS doit être le même que le nom de la ressource d’instance de cluster de basculement créée dans le cluster Pacemaker.
Le nom et l’adresse IP doivent être inscrits dans DNS.

## <a name="shared-storage"></a>Stockage partagé
Toutes les instances de cluster de basculement, qu’elles se trouvent sur Linux ou Windows Server, requièrent une certaine forme de stockage partagé. Ce stockage est présenté à tous les serveurs qui peuvent héberger une instance de cluster de basculement, mais un seul serveur peut utiliser le stockage pour l’instance de cluster de basculement à un moment donné. Les options disponibles pour le stockage partagé sous Linux sont les suivantes :

- iSCSI
- NFS (Network File System)
- SMB (Server Message Block) sous Windows Server, il existe des options légèrement différentes. Une option actuellement non prise en charge pour les instances de cluster de basculement basés sur Linux est la possibilité d’utiliser un disque local sur le nœud pour TempDB, qui est l’espace de travail temporaire de SQL Server.

Dans une configuration qui s’étend sur plusieurs emplacements, ce qui est stocké dans un centre de données doit être synchronisé avec l’autre. En cas de basculement, l’instance de cluster de basculement peut être mis en ligne et le stockage est considéré comme identique. Pour cela, vous devez disposer d’une méthode externe pour la réplication du stockage, qu’elle soit effectuée via le matériel de stockage sous-jacent ou un utilitaire basé sur un logiciel. 

>[!NOTE]
>Pour SQL Server, les déploiements basés sur Linux à l’aide de disques présentés directement à un serveur doivent être formatés avec XFS ou EXT4. D’autres systèmes de fichiers ne sont actuellement pas pris en charge. Toutes les modifications seront reflétées ici.

Le processus de présentation du stockage partagé est le même pour les différentes méthodes prises en charge :

- Configurer le stockage partagé
- Monter le stockage en tant que dossier sur les serveurs qui serviront de nœuds du cluster Pacemaker pour l’interface de cluster de basculement
- Si nécessaire, déplacer les bases de données système SQL Server vers un stockage partagé
- Tester le bon fonctionnement de SQL Server à partir de chaque serveur connecté au stockage partagé

L’une des principales différences avec SQL Server sur Linux est que même si vous pouvez configurer l’emplacement des fichiers journaux et des données utilisateur par défaut, les bases de données système doivent toujours exister à l’adresse `/var/opt/mssql/data`. Sur Windows Server, il est possible de déplacer les bases de données système, y compris TempDB. Ce fait joue un rôle sur la configuration du stockage partagé pour une instance de cluster de basculement.

Les chemins d’accès par défaut pour les bases de données non-système peuvent être modifiés à l’aide de l’utilitaire `mssql-conf`. Pour plus d’informations sur la modification des valeurs par défaut, [Modifiez l’emplacement du répertoire de données ou de journaux par défaut](sql-server-linux-configure-mssql-conf.md#datadir). Vous pouvez également stocker des données de SQL Server et des transactions dans d’autres emplacements, à condition qu’elles aient la sécurité appropriée, même si ce n’est pas un emplacement par défaut ; l’emplacement doit être indiqué.

Les rubriques suivantes expliquent comment configurer des types de stockage pris en charge pour une instance de cluster de basculement SQL Server basé sur Linux :

- [Configurer l’instance de cluster de basculement - iSCSI - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure-iscsi.md)
- [Configurer l’instance de cluster de basculement - NFS - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure-nfs.md)
- [Configurer l’instance de cluster de basculement - SMB - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure-smb.md)
