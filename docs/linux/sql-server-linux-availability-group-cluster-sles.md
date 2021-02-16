---
title: 'SUSE : Configurer un groupe de disponibilité pour SQL Server sur Linux'
titleSuffix: SQL Server
description: Découvrez comment créer des clusters de groupes de disponibilité pour SQL Server sur SLES (SUSE Linux Enterprise Server).
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 85180155-6726-4f42-ba57-200bf1e15f4d
ms.openlocfilehash: 232235e1e7b8718b83c4826b4a019cb6c52e437c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338414"
---
# <a name="configure-sles-cluster-for-sql-server-availability-group"></a>Configurer un cluster SLES pour le groupe de disponibilité SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Ce guide fournit des instructions pour créer un cluster à trois nœuds pour SQL Server sur SUSE Linux Enterprise Server (SLES) 12 SP2. Pour une haute disponibilité, un groupe de disponibilité sur Linux nécessite trois nœuds, consultez [Haute disponibilité et protection des données pour les configurations de groupes de disponibilité](sql-server-linux-availability-group-ha.md). La couche de clustering est basée sur l’[Extension haute disponibilité (HAE)](https://www.suse.com/products/highavailability) de SUSE placée au-dessus de [Pacemaker](https://clusterlabs.org/). 

Pour plus d’informations sur la configuration du cluster, les options de l’agent de ressources, la gestion, les meilleures pratiques et les suggestions, consultez [Extension haute disponibilité SUSE Linux Enterprise 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

>[!NOTE]
>À ce stade, l’intégration de SQL Server avec Pacemaker sur Linux n’est pas aussi couplée qu’avec WSFC sur Windows. Le service SQL Server sur Linux ne prend pas en charge les clusters. Pacemaker contrôle la totalité de l’orchestration des ressources de cluster, y compris la ressource du groupe de disponibilité. Sur Linux, vous ne devez pas compter sur les vues de gestion dynamique (DMV) du groupe de disponibilité Always On qui fournissent des informations sur le cluster telles que sys.dm_hadr_cluster. En outre, le nom du réseau virtuel est spécifique à WSFC, il n’existe aucun équivalent dans le cas de Pacemaker. Vous pouvez toujours créer un écouteur et l’utiliser pour la reconnexion transparente après le basculement, mais vous devrez inscrire manuellement le nom de l’écouteur sur le serveur DNS avec l’adresse IP utilisée pour créer la ressource IP virtuelle (comme expliqué dans les sections suivantes).

[!INCLUDE [bias-sensitive-term-t](../includes/bias-sensitive-term-t.md)]

## <a name="roadmap"></a>Feuille de route

La procédure de création d’un groupe de disponibilité pour la haute disponibilité varie entre les serveurs Linux et un cluster de basculement Windows Server. La liste suivante décrit les différentes étapes de haut niveau : 

1. [Configurez SQL Server sur les nœuds du cluster](sql-server-linux-setup.md).

2. [Créez le groupe de disponibilité](sql-server-linux-availability-group-failover-ha.md). 

3. Configurez un gestionnaire de ressources de cluster, comme Pacemaker. Ces instructions se trouvent dans ce document.
   
   La façon de configurer un gestionnaire de ressources de cluster dépend de la distribution Linux spécifique. 

   >[!IMPORTANT]
   >Les environnements de production nécessitent un agent d’isolation, comme STONITH pour la haute disponibilité. Les exemples de cet article n’utilisent pas les agents d’isolation. Ils sont destinés uniquement à des fins de test et de validation. 
   
   >Un cluster Pacemaker utilise l’isolation pour ramener le cluster à un état connu. La façon de configurer l’isolation dépend de la distribution et de l’environnement. À ce stade, l’isolation n’est pas disponible dans certains environnements cloud. Consultez [Extension de haute disponibilité SUSE Linux Enterprise](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing).

5. [Ajoutez le groupe de disponibilité en tant que ressource dans le cluster](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server). 

## <a name="prerequisites"></a>Prérequis

Pour effectuer le scénario de bout en bout suivant, vous avez besoin de trois machines pour déployer le cluster à trois nœuds. Les étapes suivantes décrivent la configuration de ces serveurs.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Installer et configurer le système d’exploitation sur chaque nœud du cluster 

La première étape consiste à configurer le système d'exploitation sur les nœuds de cluster. Pour ce guide, utilisez SLES 12 SP2 avec un abonnement valide pour le module complémentaire de haute disponibilité.

### <a name="install-and-configure-sql-server-service-on-each-cluster-node"></a>Installer et configurer le service SQL Server sur chaque nœud du cluster

1. Installez et configurez le service SQL Server sur tous les nœuds. Pour obtenir des instructions détaillées, consultez [Installer SQL Server sur Linux](sql-server-linux-setup.md).

1. Désignez un nœud en tant que principal et d’autres nœuds en tant que secondaires. Utilisez ces termes dans ce guide.

1. Assurez-vous que les nœuds qui vont faire partie du cluster peuvent communiquer entre eux.

   L’exemple suivant présente `/etc/hosts` avec des ajouts pour trois nœuds nommés SLES1, SLES2 et SLES3.

   ```
   127.0.0.1   localhost
   10.128.16.33 SLES1
   10.128.16.77 SLES2
   10.128.16.22 SLES3
   ```

   Tous les nœuds de cluster doivent pouvoir accéder aux uns et aux autres via SSH. Certains outils, comme `hb_report` ou `crm_report` (pour la résolution des problèmes) et l’Explorateur d’historique de Hawk, exigent un accès SSH sans mot de passe entre les nœuds, sinon ils ne peuvent collecter que les données du nœud actif. Si vous utilisez un port SSH non standard, utilisez l’option -X (consultez la page `man`). Par exemple, si votre port SSH est le 3479, appelez un `crm_report` avec :

   ```bash
   sudo crm_report -X "-p 3479" [...]
   ```

   Pour plus d’informations, consultez la section [Guide d’administration SLES - Divers](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc).


## <a name="create-a-sql-server-login-for-pacemaker"></a>Créer une connexion SQL Server pour Pacemaker

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="configure-an-always-on-availability-group"></a>Configurer un groupe de disponibilité Always On

Sur les serveurs Linux, configurez le groupe de disponibilité, puis configurez les ressources de cluster. Pour configurer le groupe de disponibilité, consultez [Configurer le groupe de disponibilité Always On pour SQL Server sur Linux ](sql-server-linux-availability-group-configure-ha.md)

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Installer et configurer Pacemaker sur chaque nœud de cluster

1. Installer l’extension de haute disponibilité

   Pour référence, consultez [Installation de SUSE Linux Enterprise Server et de l’extension de haute disponibilité](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.installation)

1. Installez le package d’agents de ressources SQL Server sur les deux nœuds.

   ```bash
   sudo zypper install mssql-server-ha
   ```

## <a name="set-up-the-first-node"></a>Configurer le premier nœud

   Référez-vous aux [instructions d'installation SLES](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node)

1. Connectez-vous en tant que `root` à la machine physique ou virtuelle que vous souhaitez utiliser en tant que nœud de cluster.
2. Démarrez le script de démarrage en exécutant :
   ```bash
   sudo ha-cluster-init
   ```

   Si NTP n’a pas été configuré pour démarrer au moment du démarrage, un message s’affiche. 

   Si vous décidez de continuer, le script génère automatiquement des clés pour l’accès SSH et pour l’outil de synchronisation Csync2 et démarre les services nécessaires pour les deux. 

3. Pour configurer la couche de communication du cluster (Corosync) : 

   a. Entrez une adresse réseau à laquelle effectuer la liaison. Par défaut, le script propose l’adresse réseau d’eth0. Vous pouvez également entrer une autre adresse réseau, par exemple l’adresse de bond0. 

   b. Entrez une adresse de multidiffusion. Le script propose une adresse aléatoire que vous pouvez utiliser par défaut. 

   c. Entrez un port de multidiffusion. Le script propose 5405 comme valeur par défaut. 

   d. Pour configurer `SBD ()`, entrez un chemin d’accès permanent à la partition de votre appareil de bloc que vous souhaitez utiliser pour SBD. Le chemin d’accès doit être cohérent sur tous les nœuds du cluster. 
   Enfin, le script démarre le service Pacemaker pour mettre le cluster à un nœud en ligne et activer l’interface de gestion Web Hawk2. L’URL à utiliser pour Hawk2 s’affiche à l’écran. 

4. Pour obtenir des détails sur le processus d’installation, cochez `/var/log/sleha-bootstrap.log`. Vous disposez maintenant d’un cluster à un nœud en cours d’exécution. Vérifiez l’état du cluster à l’aide de l’état crm :

   ```bash
   sudo crm status
   ```

   Vous pouvez également consulter la configuration du cluster avec `crm configure show xml` ou `crm configure show`.

5. La procédure de démarrage crée un utilisateur Linux nommé hacluster avec le mot de passe linux. Remplacez le mot de passe par défaut par un mot de passe sécurisé dès que possible : 

   ```bash
   sudo passwd hacluster
   ```

## <a name="add-nodes-to-the-existing-cluster"></a>Ajouter des nœuds à un cluster existant

Si vous avez un cluster en cours d’exécution avec un ou plusieurs nœuds, ajoutez des nœuds de cluster supplémentaires avec le script de démarrage de la jonction de clusters à haute disponibilité. Le script a uniquement besoin d’accéder à un nœud de cluster existant et effectue automatiquement le programme d’installation de base sur la machine actuelle. Utiliser les étapes suivantes :

Si vous avez configuré les nœuds de cluster existants à l’aide du module de cluster `YaST`, assurez-vous que les conditions préalables suivantes sont remplies avant d’exécuter `ha-cluster-join` :
- L’utilisateur racine sur les nœuds existants a des clés SSH en place pour la connexion sans mot de passe. 
- `Csync2` est configuré sur les nœuds existants. Pour plus d'informations, consultez Configuration de Csync2 avec YaST. 

1. Connectez-vous en tant qu’utilisateur racine à la machine physique ou virtuelle censée joindre le cluster. 
2. Démarrez le script de démarrage en exécutant : 

   ```bash
   sudo ha-cluster-join
   ```

   Si NTP n’a pas été configuré pour démarrer au moment du démarrage, un message s’affiche. 

3. Si vous décidez de continuer, vous serez invité à entrer l’adresse IP d’un nœud existant. Entrez l'adresse IP. 

4. Si vous n’avez pas encore configuré un accès SSH sans mot de passe entre les deux machines, vous serez également invité à entrer le mot de passe racine du nœud existant. 

   Une fois connecté au nœud spécifié, le script copie la configuration Corosync, configure SSH et `Csync2`, puis met la machine actuelle en ligne en tant que nouveau nœud de cluster. En outre, il démarre le service nécessaire à Hawk. Si vous avez configuré le stockage partagé avec `OCFS2`, il crée également automatiquement le répertoire de montage pour le système de fichiers `OCFS2`. 

5. Répétez les étapes précédentes pour toutes les machines que vous souhaitez ajouter au cluster. 

6. Pour plus d’informations sur le processus, vérifiez `/var/log/ha-cluster-bootstrap.log`. 

1. Vérifiez l’état du cluster à l’aide de `sudo crm status`. Si vous avez correctement ajouté le deuxième nœud, voici ce que vous obtenez en sortie :

   ```bash
   sudo crm status
   
   3 nodes configured
   1 resource configured
   Online: [ SLES1 SLES2 SLES3]
   Full list of resources:   
   admin_addr     (ocf::heartbeat:IPaddr2):       Started node1
   ```

   >[!NOTE]
   >`admin_addr` est la ressource de cluster IP virtuelle qui est configurée pendant l’installation initiale du cluster à un nœud.

Après avoir ajouté tous les nœuds, vérifiez si vous devez ajuster la stratégie de non-quorum dans les options de cluster global. Cela est particulièrement important pour les clusters à deux nœuds. Pour plus d’informations, consultez la section 4.1.2, Option de stratégie de non quorum. 

## <a name="set-cluster-property-cluster-recheck-interval"></a>Définir l’intervalle de revérification du cluster de la propriété du cluster

`cluster-recheck-interval` indique l’intervalle d’interrogation selon lequel le cluster vérifie les modifications dans les paramètres de ressource, les contraintes ou d’autres options de cluster. Si un réplica tombe en panne, le cluster tente de redémarrer le réplica à un intervalle qui est lié par la valeur `failure-timeout` et la valeur `cluster-recheck-interval`. Par exemple, si `failure-timeout` a la valeur de 60 secondes et `cluster-recheck-interval` est défini sur 120 secondes, le redémarrage est tenté à un intervalle supérieur à 60 secondes, mais inférieur à 120 secondes. Nous vous recommandons de définir le délai d’expiration des défaillances sur 60s et l’intervalle de revérification du cluster sur une valeur supérieure à 60 secondes. Il n’est pas recommandé de définir l’option d’intervalle de revérification du cluster sur une valeur faible.

Pour mettre à jour la valeur de la propriété sur une exécution de `2 minutes` :

```bash
crm configure property cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Si vous disposez déjà d’une ressource de groupe de disponibilité gérée par un cluster Pacemaker, notez que toutes les distributions qui utilisent le dernier package Pacemaker 1.1.18-11.el7 introduisent une modification de comportement pour le paramètre du cluster défaillance du démarrage fatale lorsque sa valeur est false. Cette modification affecte le workflow de basculement. Si un réplica principal subit une interruption, le cluster est censé basculer vers l’un des réplicas secondaires disponibles. Au lieu de cela, les utilisateurs remarqueront que le cluster continue d’essayer de démarrer le réplica principal qui a échoué. Si ce principal ne passe jamais en ligne (en raison d’une interruption permanente), le cluster ne bascule jamais vers un autre réplica secondaire disponible. En raison de cette modification, une configuration précédemment recommandée pour définir Défaillance de démarrage fatale n’est plus valide et le paramètre doit être rétabli à sa valeur par défaut de `true`. En outre, la ressource du groupe de disponibilité doit être mise à jour pour inclure la propriété `failover-timeout`. 
>
>Pour mettre à jour la valeur de la propriété sur une exécution de `true` :
>
>```bash
>crm configure property start-failure-is-fatal=true
>```
>
>Mettez à jour la propriété de ressource de votre groupe de disponibilité existant `failure-timeout` pour une exécution de `60s` (remplacez `ag1` par le nom de votre ressource de groupe de disponibilité) : 
>
>```bash
>crm configure edit ag1
># In the text editor, add `meta failure-timeout=60s` after any `param`s and before any `op`s
>```

Pour plus d’informations sur les propriétés de cluster Pacemaker, consultez [Configuration des ressources de cluster](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_config_crm_resources.html).

## <a name="configure-fencing-stonith"></a>Configurer l’isolation (STONITH)
Les fournisseurs de cluster Pacemaker nécessitent l’activation de STONITH et d’un appareil d’isolation configuré pour une configuration de cluster prise en charge. Lorsque le gestionnaire des ressources de cluster ne peut pas déterminer l’état d’un nœud ou d’une ressource sur un nœud, l’isolation est utilisée pour ramener le cluster à un état connu.

L’isolation au niveau des ressources garantit principalement qu’il n’y a pas d’altération des données pendant une interruption en configurant une ressource. Vous pouvez utiliser l’isolation au niveau des ressources, par exemple, avec DRBD (périphérique de bloc répliqué distribué) pour marquer le disque d’un nœud comme obsolète lorsque la liaison de communication tombe en panne.

L’isolation au niveau du nœud garantit qu’un nœud n’exécute aucune ressource. Pour ce faire, réinitialisez le nœud et l’implémentation de son Pacemaker est appelée STONITH (qui signifie « prendre l’autre nœud dans la tête »). Pacemaker prend en charge un grand nombre de périphériques d’isolation, tels qu’une alimentation sans coupure ou des cartes d’interface de gestion pour des serveurs.

Pour plus d'informations, consultez les pages suivantes :

- [Clusters de Pacemaker à partir de zéro](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/)
- [Isolation et STONITH](https://clusterlabs.org/doc/crm_fencing.html)
- [Documentation haute disponibilité SUSE : isolation et STONITH](https://www.suse.com/documentation/sle_ha/book_sleha/data/cha_ha_fencing.html)

Au moment de l’initialisation du cluster, STONITH est désactivé si aucune configuration n’est détectée. Vous pouvez l’activer ultérieurement en exécutant la commande suivante :

```bash
sudo crm configure property stonith-enabled=true
```
  
>[!IMPORTANT]
>La désactivation de STONITH est uniquement à des fins de test. Si vous envisagez d’utiliser Pacemaker dans un environnement de production, vous devez planifier une implémentation de STONITH en fonction de votre environnement et la garder activée. SUSE ne fournit pas d’agents d’isolation pour les environnements cloud (y compris Azure) ou Hyper-V. En fait, le fournisseur de cluster n’offre pas de prise en charge pour l’exécution de clusters de production dans ces environnements. Nous travaillons sur une solution pour cet intervalle qui sera disponible dans les mises en production ultérieures.

## <a name="configure-the-cluster-resources-for-sql-server"></a>Configurer les ressources de cluster pour SQL Server

Reportez-vous au [Guid d'administration SLES](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config)

## <a name="enable-pacemaker"></a>Activer Pacemaker

Activez Pacemaker afin qu’il démarre automatiquement.

Exécutez la commande suivante sur chaque nœud du cluster.

```bash
systemctl enable pacemaker
```

### <a name="create-availability-group-resource"></a>Créer une ressource de groupe de disponibilité

La commande suivante crée et configure la ressource de groupe de disponibilité pour trois réplicas du groupe de disponibilité [AG1]. Les opérations et les délais d’expiration de l’analyse doivent être spécifiés explicitement dans SLES en fonction du fait que les délais d’expiration dépendent fortement de la charge de travail et doivent être ajustés avec soin pour chaque déploiement.
Exécutez la commande sur l’un des nœuds du cluster :

1. Exécutez `crm configure` pour ouvrir l’invite crm :

   ```bash
   sudo crm configure 
   ```

1. Dans l’invite crm, exécutez la commande suivante pour configurer les propriétés de la ressource.

   ```bash
   primitive ag_cluster \
      ocf:mssql:ag \
      params ag_name="ag1" \
      meta failure-timeout=60s \
      op start timeout=60s \
      op stop timeout=60s \
      op promote timeout=60s \
      op demote timeout=10s \
      op monitor timeout=60s interval=10s \
      op monitor timeout=60s interval=11s role="Master" \
      op monitor timeout=60s interval=12s role="Slave" \
      op notify timeout=60s
   ms ms-ag_cluster ag_cluster \
      meta master-max="1" master-node-max="1" clone-max="3" \
     clone-node-max="1" notify="true" \
   commit
      ```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

### <a name="create-virtual-ip-resource"></a>Créer une ressource d’adresse IP virtuelle

Si vous n’avez pas créé la ressource d’adresse IP virtuelle au moment de l’exécution de `ha-cluster-init`, vous pouvez créer cette ressource maintenant. La commande suivante crée une ressource d’adresse IP virtuelle. Remplacez `<**0.0.0.0**>` par une adresse disponible de votre réseau et `<**24**>` par le nombre de bits dans le masque de sous-réseau CIDR. Exécutez sur un nœud.

```bash
crm configure \
primitive admin_addr \
   ocf:heartbeat:IPaddr2 \
   params ip=<**0.0.0.0**> \
      cidr_netmask=<**24**>
```

### <a name="add-colocation-constraint"></a>Ajouter une contrainte de colocalisation
Presque toutes les décisions d’un cluster Pacemaker, comme le choix de l’emplacement dans lequel une ressource doit s’exécuter, s’effectuent en comparant les scores. Les scores sont calculés par ressource et le gestionnaire de ressources de cluster choisit le nœud avec le score le plus élevé pour une ressource particulière. (Si un nœud a un score négatif pour une ressource, la ressource ne peut pas être exécutée sur ce nœud.) Nous pouvons manipuler les décisions du cluster avec les contraintes. Les contraintes ont un score. Si une contrainte a un score inférieur à INFINITY, il ne s’agit que d’une suggestion. Un score INFINITY signifie qu’il s’agit d’une obligation. Nous voulons nous assurer que le principal du groupe de disponibilité et la ressource d’adresse IP virtuelle sont exécutés sur le même hôte. Nous définissons donc une contrainte de colocalisation avec un score INFINITY. 

Pour définir une contrainte de colocalisation pour que l’adresse IP virtuelle s’exécute sur le même nœud que le Master, exécutez la commande suivante sur un nœud :

```bash
crm configure
colocation vip_on_master inf: \
    admin_addr ms-ag_cluster:Master
commit
```

### <a name="add-ordering-constraint"></a>Ajouter une contrainte de classement
La contrainte de colocalisation a une contrainte de classement implicite. Elle déplace la ressource IP virtuelle avant de déplacer la ressource de groupe de disponibilité. La séquence des événements par défaut est la suivante : 

1. La ressource des problèmes de l’utilisateur migre vers le master du groupe de disponibilité du nœud1 au nœud2.
2. La ressource d’adresse IP virtuelle s’arrête sur le nœud 1.
3. La ressource d’adresse IP virtuelle démarre sur le nœud 2. À ce stade, l’adresse IP pointe temporairement vers le nœud 2, tandis que le nœud 2 est toujours un secondaire de pré-basculement. 
4. Le master du groupe de disponibilité sur le nœud 1 est rétrogradé.
5. Le groupe de disponibilité sur le nœud 2 est promu master. 

Pour empêcher l’adresse IP de pointer temporairement vers le nœud avec le secondaire antérieur au basculement, ajoutez une contrainte de classement. Pour ajouter une contrainte de classement, exécutez la commande suivante sur un nœud : 

```bash
sudo crm configure \
   order ag_first inf: ms-ag_cluster:promote admin_addr:start
```


>[!IMPORTANT]
>Après avoir configuré le cluster et ajouté le groupe de disponibilité en tant que ressource de cluster, vous ne pouvez pas utiliser Transact-SQL pour basculer les ressources du groupe de disponibilité. Les ressources de cluster SQL Server sur Linux ne sont pas couplées aussi étroitement au système d’exploitation, car elles se trouvent sur un cluster de basculement Windows Server (WSFC). Le service SQL Server n’est pas informé de la présence du cluster. Toutes les orchestrations sont effectuées via les outils d’administration de cluster. Dans SLES utilisez `crm`. 

Basculez manuellement le groupe de disponibilité avec `crm`. N’initialisez pas le basculement avec Transact-SQL. Pour plus d'informations, consultez [Basculement](sql-server-linux-availability-group-failover-ha.md#failover).


Pour plus d'informations, consultez les pages suivantes :
- [Gestion des ressources de cluster](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm).   
- [Concepts de haute disponibilité](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.concepts)
- [Référence rapide à Pacemaker](https://github.com/ClusterLabs/pacemaker/blob/master/doc/pcs-crmsh-quick-ref.md) 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>Étapes suivantes

[Opérer le groupe de disponibilité de haute disponibilité](sql-server-linux-availability-group-failover-ha.md)
