---
title: Utiliser une instance de cluster de basculement RHEL pour SQL Server sur Linux
description: Découvrez comment utiliser une instance de cluster de basculement de disque partagé RHEL (Red Hat Enterprise Linux) pour SQL Server pour la haute disponibilité (basculement manuel de l’instance, par exemple) et comment ajouter des nœuds au cluster et en supprimer.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 075ab7d8-8b68-43f3-9303-bbdf00b54db1
ms.openlocfilehash: 272bc33012e37c58f6a3f8ea0ed51f1c216d716f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346269"
---
# <a name="operate-rhel-failover-cluster-instance-fci-for-sql-server"></a>Utiliser une instance de cluster de basculement RHEL pour SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Ce document explique comment effectuer les tâches suivantes pour SQL Server sur un cluster de basculement de disques partagés avec Red Hat Enterprise Linux.

- Basculer manuellement le cluster
- Surveiller un service SQL Server de cluster de basculement
- Ajouter un nœud de cluster
- Supprimer un nœud de cluster
- Modifier la fréquence d’analyse des ressources SQL Server

## <a name="architecture-description"></a>Description de l’architecture

La couche de clustering est basée sur le [module complémentaire haute disponibilité](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) Red Hat Enterprise Linux (RHEL) basé sur [Pacemaker](https://clusterlabs.org/). Corosync et Pacemaker coordonnent les communications et la gestion des ressources du cluster. L’instance est active sur un nœud ou sur l’autre.

Le diagramme suivant illustre les composants d’un cluster Linux avec SQL Server. 

![Cluster SQL de 7 disques partagés Red Hat Enterprise Linux](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 

Pour plus d’informations sur la configuration du cluster, les options des agents de ressources et la gestion, consultez la [documentation de référence de RHEL](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).

## <a name="failover-cluster-manually"></a><a name = "failManual"></a>Cluster de basculement manuel

La commande `resource move` crée une contrainte forçant la ressource à démarrer sur le nœud cible.    Après l’exécution de la commande `move`, l’exécution de la ressource `clear` supprime la contrainte, de sorte qu’il est possible de déplacer à nouveau la ressource ou d’effectuer le basculement automatique de la ressource. 

```bash
sudo pcs resource move <sqlResourceName> <targetNodeName>  
sudo pcs resource clear <sqlResourceName> 
```

L’exemple suivant déplace la ressource **mssqlha** vers un nœud nommé **sqlfcivm2**, puis supprime la contrainte afin que la ressource puisse se déplacer ultérieurement vers un autre nœud.  

```bash
sudo pcs resource move mssqlha sqlfcivm2 
sudo pcs resource clear mssqlha 
```

## <a name="monitor-a-failover-cluster-sql-server-service"></a>Surveiller un service SQL Server de cluster de basculement

Afficher l’état actuel du cluster :

```bash
sudo pcs status  
```

Afficher l’état actif du cluster et des ressources :

```bash
sudo crm_mon 
```

Afficher les journaux de l’agent de ressources sur `/var/log/cluster/corosync.log`

## <a name="add-a-node-to-a-cluster"></a>Ajouter un nœud à un cluster

1. Vérifiez l’adresse IP de chaque nœud. Le script suivant affiche l’adresse IP de votre nœud actuel. 

   ```bash
   ip addr show
   ```

3. Le nouveau nœud doit recevoir un nom unique de 15 caractères ou moins. Par défaut, dans Red Hat Linux, le nom de l’ordinateur est `localhost.localdomain`. Ce nom par défaut ne peut pas être unique et est trop long. Définissez le nom de l’ordinateur comme étant le nouveau nœud. Définissez le nom de l’ordinateur en l'ajoutant à `/etc/hosts`. Le script suivant vous permet de modifier `/etc/hosts` avec `vi`. 

   ```bash
   sudo vi /etc/hosts
   ```

   L’exemple suivant présente `/etc/hosts` avec des ajouts pour trois nœuds nommés `sqlfcivm1`, `sqlfcivm2` et `sqlfcivm3`.

   ```
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1         localhost localhost6 localhost6.localdomain6
   10.128.18.128 fcivm1
   10.128.16.77 fcivm2
   10.128.14.26 fcivm3
    ```
    
   Le fichier doit être le même sur chaque nœud. 

1. Arrêtez le service SQL Server sur le nouveau nœud.

1. Suivez les instructions pour monter le répertoire du fichier de base de données dans l’emplacement partagé :

   À partir du serveur NFS, installez `nfs-utils`

   ```bash
   sudo yum -y install nfs-utils 
   ``` 

   Ouvrir le pare-feu sur les clients et le serveur NFS 

   ```bash
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

   Modifiez le fichier/etc/fstab pour inclure la commande Monter : 

   ```bash
   <IP OF NFS SERVER>:<shared_storage_path> <database_files_directory_path> nfs timeo=14,intr
   ```

   Exécutez `mount -a` pour que les modifications soient prises en compte.
   
1. Sur le nouveau nœud, créez un fichier pour stocker le nom d’utilisateur et le mot de passe SQL Server pour la connexion à Pacemaker. La commande suivante a pour effet de créer et remplir ce fichier :

   ```bash
   sudo touch /var/opt/mssql/passwd
   sudo echo "<loginName>" >> /var/opt/mssql/secrets/passwd
   sudo echo "<loginPassword>" >> /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/passwd
   sudo chmod 600 /var/opt/mssql/passwd
   ```

3. Sur le nouveau nœud, ouvrez les ports de pare-feu de Pacemaker. Pour ouvrir ces ports avec `firewalld`, exécutez la commande suivante :

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > [!NOTE]
   > Si vous utilisez un autre pare-feu qui n’intègre pas de configuration à haute disponibilité intégrée, les ports suivants doivent être ouverts pour permettre à Pacemaker de communiquer avec les autres nœuds du cluster
   >
   > * TCP : Ports 2224, 3121, 21064
   > * UDP : Port 5405

1. Installez les packages Pacemaker sur le nouveau nœud.

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```
 
2. Définissez le mot de passe pour l’utilisateur par défaut qui est créé pendant l’installation des packages Pacemaker et Corosync. Utilisez le même mot de passe que les nœuds existants. 

   ```bash
   sudo passwd hacluster
   ```
 
3. Activez et démarrez le service `pcsd` et Pacemaker. Cela permettra au nouveau nœud de rejoindre le cluster après le redémarrage. Exécutez la commande suivante sur le nouveau nœud.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

4. Installez l’agent de ressources FCI pour SQL Server. Exécutez les commandes suivantes sur le nouveau nœud. 

   ```bash
   sudo yum install mssql-server-ha
   ```

1. Sur un nœud existant à partir du cluster, authentifiez le nouveau nœud et ajoutez-le au cluster:

    ```bash
    sudo pcs    cluster auth <nodeName3> -u hacluster 
    sudo pcs    cluster node add <nodeName3> 
    ```

    L’exemple suivant ajoute un nœud nommé **vm3** au cluster.

    ```bash
    sudo pcs    cluster auth  
    sudo pcs    cluster start 
    ```

## <a name="remove-nodes-from-a-cluster"></a>Supprimer des nœuds d’un cluster

Pour supprimer un nœud d’un cluster, exécutez la commande suivante :

```bash
sudo pcs    cluster node remove <nodeName>  
```

## <a name="change-the-frequency-of-sqlservr-resource-monitoring-interval"></a>Modifier la fréquence de l’intervalle d’analyse des ressources sqlservr

```bash
sudo pcs    resource op monitor interval=<interval>s <sqlResourceName> 
```

L’exemple suivant définit l’intervalle d’analyse à 2 secondes pour la ressource mssql :

```bash
sudo pcs    resource op monitor interval=2s mssqlha 
```
## <a name="troubleshoot-red-hat-enterprise-linux-shared-disk-cluster-for-sql-server"></a>Détecter un problème sur le cluster de disques partagés Red Hat Enterprise Linux pour SQL Server

Lors de la résolution des problèmes du cluster, il peut être utile de comprendre comment les trois démons fonctionnent ensemble pour gérer les ressources de cluster. 

| Daemon | Description 
| ----- | -----
| Corosync | Fournit l’appartenance au quorum et la messagerie entre les nœuds du cluster.
| Pacemaker | Se trouve au-dessus de Corosync et fournit des machines d’état pour les ressources. 
| PCSD | Gère à la fois Pacemaker et Corosync via les outils `pcs`

PCSD doit être en cours d’exécution pour pouvoir utiliser les outils `pcs`. 

### <a name="current-cluster-status"></a>État actuel du cluster 

`sudo pcs status` renvoie des informations de base sur le cluster, le quorum, les nœuds, les ressources et l’état du démon pour chaque nœud. 

Voici un exemple de sortie de quorum Pacemaker sain :

```
Cluster name: MyAppSQL 
Last updated: Wed Oct 31 12:00:00 2016  Last change: Wed Oct 31 11:00:00 2016 by root via crm_resource on sqlvmnode1 
Stack: corosync 
Current DC: sqlvmnode1  (version 1.1.13-10.el7_2.4-44eb2dd) - partition with quorum 
3 nodes and 1 resource configured 

Online: [ sqlvmnode1 sqlvmnode2 sqlvmnode3] 

Full list of resources: 

mssqlha (ocf::sql:fci): Started sqlvmnode1 

PCSD Status: 
sqlvmnode1: Online 
sqlvmnode2: Online 
sqlvmnode3: Online 

Daemon Status: 
corosync: active/disabled 
pacemaker: active/enabled 
```

Dans l’exemple, `partition with quorum` signifie qu’un quorum majoritaire de nœuds est en ligne. Si le cluster perd un quorum de nœuds majoritaire, `pcs status` retourne `partition WITHOUT quorum` et toutes les ressources sont arrêtées. 

`online: [sqlvmnode1 sqlvmnode2 sqlvmnode3]` retourne le nom de tous les nœuds participant actuellement au cluster. Si des nœuds ne sont pas participants, `pcs status` retourne `OFFLINE: [<nodename>]`.

`PCSD Status` affiche l’état du cluster pour chaque nœud.

### <a name="reasons-why-a-node-may-be-offline"></a>Raisons pour lesquelles un nœud peut être hors connexion

Vérifiez les éléments suivants lorsqu’un nœud est hors connexion.

- **Pare-feu**

    Les ports suivants doivent être ouverts sur tous les nœuds pour que Pacemaker puisse communiquer.
    
    - **TCP : 2224, 3121, 21064

- **Pacemaker ou services Corosync en cours d’exécution**

- **Communication entre nœuds**

- **Mappages de noms de nœuds**

## <a name="additional-resources"></a>Ressources supplémentaires

* Guide de Pacemaker [Cluster à partir de zéro](https://clusterlabs.org/doc/Cluster_from_Scratch.pdf)

## <a name="next-steps"></a>Étapes suivantes

[Configurer le cluster de disques partagés Red Hat Enterprise Linux pour SQL Server](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)

