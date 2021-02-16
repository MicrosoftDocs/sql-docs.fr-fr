---
title: Configurer un cluster Ubuntu pour un groupe de disponibilité
titleSuffix: SQL Server on Linux
description: Découvrez comment créer un cluster à trois nœuds sur Ubuntu et ajouter une ressource de groupe de disponibilité précédemment créée au cluster.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: dd0d6fb9-df0a-41b9-9f22-9b558b2b2233
ms.openlocfilehash: 13b719848e9cbced670e58ae3a747967a8901ada
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338299"
---
# <a name="configure-ubuntu-cluster-and-availability-group-resource"></a>Configurer un cluster Ubuntu et la ressource du groupe de disponibilité

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Ce document explique comment créer un cluster à trois nœuds sur Ubuntu et ajouter un groupe de disponibilité précédemment créé en tant que ressource dans le cluster. Pour une haute disponibilité, un groupe de disponibilité sur Linux nécessite trois nœuds, consultez [Haute disponibilité et protection des données pour les configurations de groupes de disponibilité](sql-server-linux-availability-group-ha.md).

> [!NOTE] 
> À ce stade, l’intégration de SQL Server avec Pacemaker sur Linux n’est pas aussi couplée qu’avec WSFC sur Windows. À partir de SQL, il n’y a aucune connaissance de la présence du cluster, l’ensemble de l’orchestration est en extérieur-intérieur et le service est contrôlé comme une instance autonome par Pacemaker. En outre, le nom du réseau virtuel est spécifique à WSFC, il n’existe aucun équivalent dans le cas de Pacemaker. Les vues de gestion dynamique Always On qui interrogent les informations du cluster renvoient des lignes vides. Vous pouvez toujours créer un écouteur et l’utiliser pour une reconnexion transparente après le basculement, mais vous devrez inscrire manuellement le nom de l’écouteur sur le serveur DNS avec l’adresse IP utilisée pour créer la ressource IP virtuelle (comme expliqué dans les sections suivantes).

Les sections suivantes décrivent les étapes de configuration d’une solution de cluster de basculement. 

## <a name="roadmap"></a>Feuille de route

Les étapes de création d’un groupe de disponibilité sur des serveurs Linux pour la haute disponibilité diffèrent de celles d’un cluster de basculement Windows Server. La liste suivante décrit les différentes étapes de haut niveau : 

1. [Configurez SQL Server sur les nœuds du cluster](sql-server-linux-setup.md).

2. [Créez le groupe de disponibilité](sql-server-linux-availability-group-configure-ha.md). 

3. Configurez un gestionnaire de ressources de cluster, comme Pacemaker. Ces instructions se trouvent dans ce document.
   
   La façon de configurer un gestionnaire de ressources de cluster dépend de la distribution Linux spécifique. 

   >[!IMPORTANT]
   >Les environnements de production nécessitent un agent d’isolation, comme STONITH pour la haute disponibilité. Les démonstrations de cette documentation n’utilisent pas les agents d’isolation. Elles sont destinées uniquement à des fins de test et de validation. 
   >
   >Un cluster Linux utilise l’isolation pour ramener le cluster à un état connu. La façon de configurer l’isolation dépend de la distribution et de l’environnement. À ce stade, l’isolation n’est pas disponible dans certains environnements cloud. Pour plus d’informations, consultez [Stratégies de support pour les clusters à haute disponibilité RHEL - Plateformes de virtualisation](https://access.redhat.com/articles/29440).
   >
   >L’isolation est généralement implémentée au niveau du système d’exploitation et dépend de l’environnement. Pour plus d’informations sur l’isolation, consultez la documentation du fournisseur du système d’exploitation.

5.  [Ajoutez le groupe de disponibilité en tant que ressource dans le cluster](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource). 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installer et configurer Pacemaker sur chaque nœud de cluster

1. Sur tous les nœuds, ouvrez les ports du pare-feu. Ouvrez le port pour le service de haute disponibilité de Pacemaker, l’instance et le point de terminaison du groupe de disponibilité. Le port TCP par défaut pour le serveur exécutant SQL Server est 1433.  

   ```bash
   sudo ufw allow 2224/tcp
   sudo ufw allow 3121/tcp
   sudo ufw allow 21064/tcp
   sudo ufw allow 5405/udp
        
   sudo ufw allow 1433/tcp # Replace with TDS endpoint
   sudo ufw allow 5022/tcp # Replace with DATA_MIRRORING endpoint
        
   sudo ufw reload
   ```
   
   Vous pouvez également désactiver simplement le pare-feu :
        
   ```bash
   sudo ufw disable
   ```

1. Installez les packages Pacemaker. Exécutez les commandes suivantes sur tous les nœuds :

   ```bash
   sudo apt-get install pacemaker pcs fence-agents resource-agents
   ```

2. Définissez le mot de passe pour l’utilisateur par défaut qui est créé pendant l’installation des packages Pacemaker et Corosync. Utilisez le même mot de passe sur tous les nœuds. 

   ```bash
   sudo passwd hacluster
   ```

## <a name="enable-and-start-pcsd-service-and-pacemaker"></a>Activer et démarrer le service pcsd et Pacemaker

La commande suivante active et démarre le service pcsd et Pacemaker. Exécutez sur tous les nœuds. Cela permet aux nœuds de rejoindre le cluster après le redémarrage. 

```bash
sudo systemctl enable pcsd
sudo systemctl start pcsd
sudo systemctl enable pacemaker
```
>[!NOTE]
>L’activation de la commande Pacemaker peut se terminer avec l’erreur « le démarrage par défaut Pacemaker ne contient aucun niveaux d’exécution, abandon. » Cela est sans conséquence, la configuration du cluster peut continuer. 

## <a name="create-the-cluster"></a>Créer le cluster

1. Supprimez toutes les configurations de cluster existantes de tous les nœuds. 

   L’exécution « sudo apt-get install pcs » installe Pacemaker, Corosync et les PC en même temps et commence à exécuter les 3 services.  Le démarrage de Corosync génère un fichier de modèle « /etc/cluster/corosync.conf ».  Pour que les étapes suivantes aboutissent, ce fichier ne doit pas exister. Par conséquent, la solution de contournement consiste à arrêter Pacemaker/Corosync et à supprimer « /etc/cluster/corosync.conf », puis les étapes suivantes se terminent avec succès. « pcs cluster destroy » fait la même chose et vous pouvez l’utiliser en tant qu’étape de configuration initiale du cluster en une fois.
   
   La commande suivante supprime tous les fichiers de configuration de cluster existants et arrête tous les services de cluster. Cela détruit définitivement le cluster. Exécutez-la en tant que première étape dans un environnement de pré-production. Notez que « pcs cluster destroy » a désactivé le service Pacemaker et doit être réactivé. Exécutez la commande suivante sur tous les nœuds.
   
   >[!WARNING]
   >La commande détruit toutes les ressources de cluster existantes.

   ```bash
   sudo pcs cluster destroy 
   sudo systemctl enable pacemaker
   ```

1. Créez le cluster. 

   >[!WARNING]
   >En raison d’un problème connu que le fournisseur de clusters examine, le démarrage du cluster (« pcs cluster destroy ») échoue avec l’erreur suivante. Cela est dû au fait que le fichier journal configuré dans /etc/corosync/corosync.conf, qui est créé lors de l’exécution de la commande de configuration du cluster, est incorrect. Pour contourner ce problème, remplacez le fichier journal par : /var/log/corosync/corosync.log. Vous pouvez également créer le fichier /var/log/cluster/corosync.log.
 
   ```Error
   Job for corosync.service failed because the control process exited with error code. 
   See "systemctl status corosync.service" and "journalctl -xe" for details.
   ```
  
La commande suivante crée un cluster à trois nœuds. Avant d’exécuter le script, remplacez les valeurs entre `< ... >`. Exécutez la commande suivante sur le réplica principal. 

   ```bash
   sudo pcs cluster auth <node1> <node2> <node3> -u hacluster -p <password for hacluster>
   sudo pcs cluster setup --name <clusterName> <node1> <node2...> <node3>
   sudo pcs cluster start --all
   sudo pcs cluster enable --all
   ```
   
   >[!NOTE]
   >Si vous avez configuré un cluster sur les mêmes nœuds, vous devez utiliser l’option « --force » pendant l’exécution de « pcs cluster setup ». Notez que cela revient à exécuter « pcs cluster destroy » et que le service Pacemaker doit être réactivé à l’aide de « sudo systemctl enable pacemaker ».


## <a name="configure-fencing-stonith"></a>Configurer l’isolation (STONITH)

Les fournisseurs de cluster Pacemaker nécessitent l’activation de STONITH et d’un appareil d’isolation configuré pour une configuration de cluster prise en charge. Lorsque le gestionnaire des ressources de cluster ne peut pas déterminer l’état d’un nœud ou d’une ressource sur un nœud, l’isolation est utilisée pour ramener le cluster à un état connu. L’isolation au niveau des ressources garantit principalement qu’il n’y a pas d’altération des données pendant une interruption en configurant une ressource. Vous pouvez utiliser l’isolation au niveau des ressources, par exemple, avec DRBD (périphérique de bloc répliqué distribué) pour marquer le disque d’un nœud comme obsolète lorsque la liaison de communication tombe en panne. L’isolation au niveau du nœud garantit qu’un nœud n’exécute aucune ressource. Pour ce faire, réinitialisez le nœud et l’implémentation de son Pacemaker est appelée STONITH (qui signifie « prendre l’autre nœud dans la tête »). Pacemaker prend en charge un grand nombre de périphériques d’isolation, tels qu’une alimentation sans coupure ou des cartes d’interface de gestion pour des serveurs. Pour plus d’informations, consultez [Clusters Pacemaker à partir de zéro](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/), [Isolation et stonith](https://clusterlabs.org/doc/crm_fencing.html) 

Étant donné que la configuration d’isolation au niveau du nœud dépend fortement de votre environnement, nous la désactivons pour ce didacticiel (vous pouvez la configurer ultérieurement). Exécutez le script suivant sur le réplica principal : 

```bash
sudo pcs property set stonith-enabled=false
```

>[!IMPORTANT]
>La désactivation de STONITH est uniquement à des fins de test. Si vous envisagez d’utiliser Pacemaker dans un environnement de production, vous devez planifier une implémentation de STONITH en fonction de votre environnement et la garder activée. Contactez le fournisseur du système d’exploitation pour obtenir des informations sur les agents d’isolation pour toute distribution spécifique. 

## <a name="set-cluster-property-cluster-recheck-interval"></a>Définir l’intervalle de revérification du cluster de la propriété du cluster

`cluster-recheck-interval` indique l’intervalle d’interrogation selon lequel le cluster vérifie les modifications dans les paramètres de ressource, les contraintes ou d’autres options de cluster. Si un réplica tombe en panne, le cluster tente de redémarrer le réplica à un intervalle qui est lié par la valeur `failure-timeout` et la valeur `cluster-recheck-interval`. Par exemple, si `failure-timeout` a la valeur de 60 secondes et `cluster-recheck-interval` est défini sur 120 secondes, le redémarrage est tenté à un intervalle supérieur à 60 secondes, mais inférieur à 120 secondes. Nous vous recommandons de définir le délai d’expiration des défaillances sur 60s et l’intervalle de revérification du cluster sur une valeur supérieure à 60 secondes. Il n’est pas recommandé de définir l’option d’intervalle de revérification du cluster sur une valeur faible.

Pour mettre à jour la valeur de la propriété sur une exécution de `2 minutes` :

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Si vous disposez déjà d’une ressource de groupe de disponibilité gérée par un cluster Pacemaker, notez que toutes les distributions qui utilisent le dernier package Pacemaker 1.1.18-11.el7 introduisent une modification de comportement pour le paramètre du cluster défaillance du démarrage fatale lorsque sa valeur est false. Cette modification affecte le workflow de basculement. Si un réplica principal subit une interruption, le cluster est censé basculer vers l’un des réplicas secondaires disponibles. Au lieu de cela, les utilisateurs remarqueront que le cluster continue d’essayer de démarrer le réplica principal qui a échoué. Si ce principal ne passe jamais en ligne (en raison d’une interruption permanente), le cluster ne bascule jamais vers un autre réplica secondaire disponible. En raison de cette modification, une configuration précédemment recommandée pour définir Défaillance de démarrage fatale n’est plus valide et le paramètre doit être rétabli à sa valeur par défaut de `true`. En outre, la ressource du groupe de disponibilité doit être mise à jour pour inclure la propriété `failover-timeout`. 
>
>Pour mettre à jour la valeur de la propriété sur une exécution de `true` :
>
>```bash
>sudo pcs property set start-failure-is-fatal=true
>```
>
>Mettez à jour la propriété de ressource de votre groupe de disponibilité existant `failure-timeout` pour une exécution de `60s` (remplacez `ag1` par le nom de votre ressource de groupe de disponibilité) :
>
>```bash
>pcs resource update ag1 meta failure-timeout=60s
>```

## <a name="install-sql-server-resource-agent-for-integration-with-pacemaker"></a>Installer l’agent de ressource SQL Server pour l’intégration avec Pacemaker

Exécutez les commandes suivantes sur tous les nœuds. 

```bash
sudo apt-get install mssql-server-ha
```

## <a name="create-a-sql-server-login-for-pacemaker"></a>Créer une connexion SQL Server pour Pacemaker

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>Créer une ressource de groupe de disponibilité

Pour créer la ressource de groupe de disponibilité, utilisez la commande `pcs resource create` et définissez les propriétés de la ressource. La commande ci-dessous crée une ressource de type maître/subordonné `ocf:mssql:ag` pour le groupe de disponibilité nommé `ag1`. 

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=30s master meta notify=true

```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

## <a name="create-virtual-ip-resource"></a>Créer une ressource d’adresse IP virtuelle

Pour créer la ressource d’adresse IP virtuelle, exécutez la commande suivante sur un nœud. Utilisez une adresse IP statique disponible à partir du réseau. Avant d’exécuter le script, remplacez les valeurs entre `< ... >` par une adresse IP virtuelle.

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

Il n’y a aucun nom de serveur virtuel équivalent dans Pacemaker. Pour utiliser une chaîne de connexion qui pointe vers un nom de serveur de chaîne et ne pas utiliser l’adresse IP, enregistrez l’adresse de ressource IP et le nom de serveur virtuel souhaité dans DNS. Pour les configurations de récupération d’urgence, enregistrez le nom de serveur virtuel et l’adresse IP souhaités auprès des serveurs DNS sur le site principal et le site de récupération d’urgence.

## <a name="add-colocation-constraint"></a>Ajouter une contrainte de colocalisation

Presque toutes les décisions d’un cluster Pacemaker, comme le choix de l’emplacement dans lequel une ressource doit s’exécuter, s’effectuent en comparant les scores. Les scores sont calculés par ressource et le gestionnaire de ressources de cluster choisit le nœud avec le score le plus élevé pour une ressource particulière. (Si un nœud a un score négatif pour une ressource, la ressource ne peut pas être exécutée sur ce nœud.) Utilisez des contraintes pour configurer les décisions du cluster. Les contraintes ont un score. Si une contrainte a un score inférieur à INFINITY, il ne s’agit que d’une suggestion. Un score INFINITY signifie qu’il s’agit d’une obligation. Pour vous assurer que le réplica principal et la ressource d’adresse IP virtuelle sont exécutés sur le même hôte, définissez une contrainte de colocalisation avec un score INFINITY. Pour ajouter une contrainte de colocalisation, exécutez la commande suivante sur un nœud. 

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

## <a name="add-ordering-constraint"></a>Ajouter une contrainte de classement

La contrainte de colocalisation a une contrainte de classement implicite. Elle déplace la ressource IP virtuelle avant de déplacer la ressource de groupe de disponibilité. La séquence des événements par défaut est la suivante :

1. L’utilisateur émet `pcs resource move` sur le groupe de disponibilité principal, de nœud1 à nœud2.
1. La ressource d’adresse IP virtuelle s’arrête sur le nœud1.
1. La ressource d’adresse IP virtuelle démarre sur le nœud2.

   >[!NOTE]
   >À ce stade, l’adresse IP pointe temporairement vers le nœud2, tandis que le nœud2 est toujours un secondaire de pré-basculement. 
   
1. Le groupe de disponibilité principal sur nœud1 est rétrogradé en secondaire.
1. Le groupe de disponibilité secondaire sur nœud2 est promu en principal. 

Pour empêcher l’adresse IP de pointer temporairement vers le nœud avec le secondaire antérieur au basculement, ajoutez une contrainte de classement. 

Pour ajouter une contrainte de classement, exécutez la commande suivante sur un nœud :

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

>[!IMPORTANT]
>Après avoir configuré le cluster et ajouté le groupe de disponibilité en tant que ressource de cluster, vous ne pouvez pas utiliser Transact-SQL pour basculer les ressources du groupe de disponibilité. Les ressources de cluster SQL Server sur Linux ne sont pas couplées aussi étroitement au système d’exploitation, car elles se trouvent sur un cluster de basculement Windows Server (WSFC). Le service SQL Server n’est pas informé de la présence du cluster. Toutes les orchestrations sont effectuées via les outils d’administration de cluster. Dans RHEL ou Ubuntu, utilisez `pcs`. 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>Étapes suivantes

[Opérer le groupe de disponibilité de haute disponibilité](sql-server-linux-availability-group-failover-ha.md)

