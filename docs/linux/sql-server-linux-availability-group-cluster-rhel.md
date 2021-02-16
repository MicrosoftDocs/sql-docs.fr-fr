---
title: 'RHEL : configurer un groupe de disponibilité pour SQL Server sur Linux'
description: Découvrez comment configurer un groupe de disponibilité si vous exécutez RHEL (Red Hat Enterprise Linux) pour SQL Server.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/23/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: b7102919-878b-4c08-a8c3-8500b7b42397
ms.openlocfilehash: 2790bbe9eb648a2d599d1b0bad8f514ab72eda50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338396"
---
# <a name="configure-rhel-cluster-for-sql-server-availability-group"></a>Configurer un cluster RHEL pour le groupe de disponibilité SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Ce document explique comment créer un cluster de groupe de disponibilité à trois nœuds pour SQL Server sur Red Hat Enterprise Linux. Pour une haute disponibilité, un groupe de disponibilité sur Linux nécessite trois nœuds, consultez [Haute disponibilité et protection des données pour les configurations de groupes de disponibilité](sql-server-linux-availability-group-ha.md). La couche de clustering est basée sur le [module complémentaire haute disponibilité](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) Red Hat Enterprise Linux (RHEL) basé sur [Pacemaker](https://clusterlabs.org/). 

> [!NOTE] 
> L’accès à la documentation complète de Red Hat requiert un abonnement valide. 

Pour plus d’informations sur la configuration du cluster, les options des agents de ressources et la gestion, consultez la [documentation de référence de RHEL](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).

> [!NOTE] 
> SQL Server n’est pas aussi étroitement intégré qu’avec Pacemaker sur Linux, comme c’est le cas avec le clustering de basculement Windows Server. Une instance n’est pas informée du cluster. Pacemaker fournit une orchestration des ressources de cluster. En outre, le nom du réseau virtuel est spécifique au clustering de basculement Windows Server : il n’existe aucun équivalent dans Pacemaker. Les vues de gestion dynamique (DMV) du groupe de disponibilité qui interrogent les informations de cluster renvoient des lignes vides sur les clusters de Pacemaker. Pour créer un écouteur pour une reconnexion transparente après le basculement, enregistrez manuellement le nom de l’écouteur dans DNS avec l’adresse IP utilisée pour créer la ressource d’adresse IP virtuelle. 

Les sections suivantes décrivent les étapes à suivre pour configurer un cluster Pacemaker et ajouter un groupe de disponibilité en tant que ressource dans le cluster pour la haute disponibilité.

## <a name="roadmap"></a>Feuille de route

Les étapes de création d’un groupe de disponibilité sur des serveurs Linux pour la haute disponibilité diffèrent de celles d’un cluster de basculement Windows Server. La liste suivante décrit les différentes étapes de haut niveau : 

1. [Configurez SQL Server sur les nœuds du cluster](sql-server-linux-setup.md).

2. [Créez le groupe de disponibilité](sql-server-linux-availability-group-configure-ha.md). 

3. Configurez un gestionnaire de ressources de cluster, comme Pacemaker. Ces instructions se trouvent dans ce document.
   
   La façon de configurer un gestionnaire de ressources de cluster dépend de la distribution Linux spécifique. 

   >[!IMPORTANT]
   >Les environnements de production nécessitent un agent d’isolation, comme STONITH pour la haute disponibilité. Les démonstrations de cette documentation n’utilisent pas les agents d’isolation. Elles sont destinées uniquement à des fins de test et de validation.
   
   >Un cluster Linux utilise l’isolation pour ramener le cluster à un état connu. La façon de configurer l’isolation dépend de la distribution et de l’environnement. À ce stade, l’isolation n’est pas disponible dans certains environnements cloud. Pour plus d’informations, consultez [Stratégies de support pour les clusters à haute disponibilité RHEL - Plateformes de virtualisation](https://access.redhat.com/articles/29440).

4. [Ajoutez le groupe de disponibilité en tant que ressource dans le cluster](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource).  

## <a name="configure-high-availability-for-rhel"></a>Configurer la haute disponibilité pour RHEL

Pour configurer la haute disponibilité pour RHEL, activez l’abonnement à haute disponibilité, puis configurez Pacemaker.

### <a name="enable-the-high-availability-subscription-for-rhel"></a>Activer l’abonnement haute disponibilité pour RHEL

Chaque nœud du cluster doit avoir un abonnement approprié pour RHEL et le module complémentaire haute disponibilité. Consultez les exigences sur [Comment installer des packages de cluster haute disponibilité dans Red Hat Enterprise Linux](https://access.redhat.com/solutions/45930). Procédez comme suit pour configurer l’abonnement et les référentiels :

1. Enregistrez le système.

   ```bash
   sudo subscription-manager register
   ```

   Fournissez vos nom d’utilisateur et mot de passe.   

1. Répertoriez les pools disponibles pour l’inscription.

   ```bash
   sudo subscription-manager list --available
   ```

   Dans la liste des pools disponibles, notez l’ID de pool pour l’abonnement à haute disponibilité.

1. Mettez à jour le script suivant. Remplacez `<pool id>` par l’ID de pool pour la haute disponibilité de l’étape précédente. Exécutez le script pour joindre l’abonnement.

   ```bash
   sudo subscription-manager attach --pool=<pool id>
   ```

1. Activez le référentiel.

   **RHEL 7**

   ```bash
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
   ```

   **RHEL 8**

   ```bash
   sudo subscription-manager repos --enable=rhel-8-for-x86_64-highavailability-rpms
   ```

Pour plus d’informations, consultez [Pacemaker - Open source, cluster haute disponibilité](https://clusterlabs.org/pacemaker/). 

Après avoir configuré l’abonnement, procédez comme suit pour configurer Pacemaker :

### <a name="configure-pacemaker"></a>Configurer Pacemaker

Après avoir inscrit l’abonnement, procédez comme suit pour configurer Pacemaker :
   
[!INCLUDE [RHEL-Configure-Pacemaker](../includes/ss-linux-cluster-pacemaker-configure-rhel.md)]

Une fois Pacemaker configuré, utilisez `pcs` pour interagir avec le cluster. Exécutez toutes les commandes sur un nœud à partir du cluster. 

## <a name="configure-fencing-stonith"></a>Configurer l’isolation (STONITH)

Les fournisseurs de cluster Pacemaker nécessitent l’activation de STONITH et d’un appareil d’isolation configuré pour une configuration de cluster prise en charge. STONITH signifie « prendre l’autre nœud dans la tête ». Lorsque le gestionnaire des ressources de cluster ne peut pas déterminer l’état d’un nœud ou d’une ressource sur un nœud, l’isolation est utilisée pour ramener le cluster à un état connu.

Un appareil STONITH fournit un agent d’isolation. [Configuration de Pacemaker sur Red Hat Entreprise Linux dans Azure](/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker/#1-create-the-stonith-devices) fournit un exemple de création d’un appareil STONITH pour ce cluster dans Azure. Modifiez les instructions pour votre environnement.

L’isolation au niveau des ressources garantit qu’il n’y a pas d’altération des données pendant une interruption en configurant une ressource. Par exemple, vous pouvez utiliser l’isolation au niveau des ressources pour marquer le disque d’un nœud comme obsolète lorsque la liaison de communication tombe en panne. 

L’isolation au niveau du nœud garantit qu’un nœud n’exécute aucune ressource. Pour ce faire, réinitialisez le nœud. Pacemaker prend en charge un grand nombre d’appareils d’isolation. Les exemples incluent une alimentation sans coupure ou des cartes d’interface de gestion pour les serveurs.

Pour plus d’informations sur STONITH et l’isolation, consultez les articles suivants :

* [Clusters de Pacemaker à partir de zéro](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/index.html)
* [Isolation et STONITH](https://clusterlabs.org/doc/crm_fencing.html)
* [Module complémentaire Red Hat haute disponibilité avec Pacemaker : Isolation](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-fencing-HAAR.html)

>[!NOTE]
>Étant donné que la configuration d’isolation au niveau du nœud dépend fortement de votre environnement, désactivez-la pour ce didacticiel (il peut être configurer ultérieurement). Le script suivant désactive l’isolation au niveau du nœud :
>
>```bash
>sudo pcs property set stonith-enabled=false
>``` 
>
>La désactivation de STONITH est uniquement à des fins de test. Si vous envisagez d’utiliser Pacemaker dans un environnement de production, vous devez planifier une implémentation de STONITH en fonction de votre environnement et la garder activée.

## <a name="set-cluster-property-cluster-recheck-interval"></a>Définir l’intervalle de revérification du cluster de la propriété du cluster

`cluster-recheck-interval` indique l’intervalle d’interrogation selon lequel le cluster vérifie les modifications dans les paramètres de ressource, les contraintes ou d’autres options de cluster. Si un réplica tombe en panne, le cluster tente de redémarrer le réplica à un intervalle qui est lié par la valeur `failure-timeout` et la valeur `cluster-recheck-interval`. Par exemple, si `failure-timeout` a la valeur de 60 secondes et `cluster-recheck-interval` est défini sur 120 secondes, le redémarrage est tenté à un intervalle supérieur à 60 secondes, mais inférieur à 120 secondes. Nous vous recommandons de définir le délai d’expiration des défaillances sur 60s et l’intervalle de revérification du cluster sur une valeur supérieure à 60 secondes. Il n’est pas recommandé de définir l’option d’intervalle de revérification du cluster sur une valeur faible.

Pour mettre à jour la valeur de la propriété sur une exécution de `2 minutes` :

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Toutes les distributions (y compris RHEL 7.3 et 7.4) qui utilisent le dernier package Pacemaker 1.1.18-11.el7 introduisent une modification de comportement pour le paramètre du cluster défaillance du démarrage fatale lorsque sa valeur est false. Cette modification affecte le workflow de basculement. Si un réplica principal subit une interruption, le cluster est censé basculer vers l’un des réplicas secondaires disponibles. Au lieu de cela, les utilisateurs remarqueront que le cluster continue d’essayer de démarrer le réplica principal qui a échoué. Si ce principal ne passe jamais en ligne (en raison d’une interruption permanente), le cluster ne bascule jamais vers un autre réplica secondaire disponible. En raison de cette modification, une configuration précédemment recommandée pour définir Défaillance de démarrage fatale n’est plus valide et le paramètre doit être rétabli à sa valeur par défaut de `true`. En outre, la ressource du groupe de disponibilité doit être mise à jour pour inclure la propriété `failover-timeout`. 

Pour mettre à jour la valeur de la propriété sur une exécution de `true` :

```bash
sudo pcs property set start-failure-is-fatal=true
```

Pour mettre à jour la propriété de ressource `ag_cluster``failure-timeout` pour une exécution de `60s` :

```bash
pcs resource update ag_cluster meta failure-timeout=60s
```


Pour plus d’informations sur les propriétés de cluster de Pacemaker, consultez [Propriétés des clusters de Pacemaker](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/ch-clusteropts-HAAR.html).

## <a name="create-a-sql-server-login-for-pacemaker"></a>Créer une connexion SQL Server pour Pacemaker

[!INCLUDE [SQL-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>Créer une ressource de groupe de disponibilité

Pour créer la ressource de groupe de disponibilité, utilisez la commande `pcs resource create` et définissez les propriétés de la ressource. La commande suivante crée une ressource de type maître/subordonné `ocf:mssql:ag` pour le groupe de disponibilité nommé `ag1`.

**RHEL 7**

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=60s master notify=true
```

**RHEL 8**

Avec la disponibilité de **RHEL 8**, la syntaxe de création a changé. Si vous utilisez **RHEL 8**, la terminologie `master` a été remplacée par `promotable`. Utilisez la commande de création suivante à la place de la commande ci-dessus : 

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=60s promotable notify=true
```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

<a name="createIP"></a>

## <a name="create-virtual-ip-resource"></a>Créer une ressource d’adresse IP virtuelle

Pour créer la ressource d’adresse IP virtuelle, exécutez la commande suivante sur un nœud. Utilisez une adresse IP statique disponible à partir du réseau. Remplacez l’adresse IP entre `<10.128.16.240>` par une adresse IP valide.

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

Il n’y a aucun nom de serveur virtuel équivalent dans Pacemaker. Pour utiliser une chaîne de connexion qui pointe vers un nom de serveur de chaîne au lieu d’une adresse IP, enregistrez l’adresse de ressource IP et le nom de serveur virtuel souhaité dans DNS. Pour les configurations de récupération d’urgence, enregistrez le nom de serveur virtuel et l’adresse IP souhaités auprès des serveurs DNS sur le site principal et le site de récupération d’urgence.

## <a name="add-colocation-constraint"></a>Ajouter une contrainte de colocalisation

Presque toutes les décisions d’un cluster Pacemaker, comme le choix de l’emplacement dans lequel une ressource doit s’exécuter, s’effectuent en comparant les scores. Les scores sont calculés par ressource. Le gestionnaire de ressources de cluster choisit le nœud avec le score le plus élevé pour une ressource particulière. Si un nœud a un score négatif pour une ressource, la ressource ne peut pas être exécutée sur ce nœud.

Sur un cluster Pacemaker, vous pouvez manipuler les décisions du cluster avec les contraintes. Les contraintes ont un score. Si une contrainte a un score inférieur à `INFINITY`, Pacemaker la considère comme une suggestion. Un score de `INFINITY` est obligatoire.

Pour vous assurer que le réplica principal et les ressources d’adresse IP virtuelle sont exécutés sur le même hôte, définissez une contrainte de colocalisation avec un score INFINITY. Pour ajouter une contrainte de colocalisation, exécutez la commande suivante sur un nœud.

### <a name="rhel-7"></a>RHEL 7

Lorsque vous créez la ressource `ag_cluster` dans RHEL 7, la ressource est créée en tant que `ag_cluster-master`. Utilisez la commande suivante pour RHEL 7 :

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

### <a name="rhel-8"></a>RHEL 8

Lorsque vous créez la ressource `ag_cluster` dans RHEL 8, la ressource est créée en tant que `ag_cluster-clone`. Utilisez la commande suivante pour RHEL 8 :

```bash
sudo pcs constraint colocation add virtualip with master ag_cluster-clone INFINITY with-rsc-role=Master
```

## <a name="add-ordering-constraint"></a>Ajouter une contrainte de classement

La contrainte de colocalisation a une contrainte de classement implicite. Elle déplace la ressource IP virtuelle avant de déplacer la ressource de groupe de disponibilité. La séquence des événements par défaut est la suivante :

1. L’utilisateur émet `pcs resource move` sur le groupe de disponibilité principal, de nœud1 à nœud2.
1. La ressource d’adresse IP virtuelle s’arrête sur le nœud 1.
1. La ressource d’adresse IP virtuelle démarre sur le nœud 2.
  
   >[!NOTE]
   >À ce stade, l’adresse IP pointe temporairement vers le nœud 2, tandis que le nœud 2 est toujours un secondaire de pré-basculement. 
   
1. Le groupe de disponibilité principal sur nœud 1 est rétrogradé en secondaire.
1. Le groupe de disponibilité secondaire sur nœud 2 est promu en principal. 

Pour empêcher l’adresse IP de pointer temporairement vers le nœud avec le secondaire antérieur au basculement, ajoutez une contrainte de classement. 

Pour ajouter une contrainte de classement, exécutez la commande suivante sur un nœud :

### <a name="rhel-7"></a>RHEL 7

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

### <a name="rhel-8"></a>RHEL 8

```bash
sudo pcs constraint order promote ag_cluster-clone then start virtualip
```

>[!IMPORTANT]
>Après avoir configuré le cluster et ajouté le groupe de disponibilité en tant que ressource de cluster, vous ne pouvez pas utiliser Transact-SQL pour basculer les ressources du groupe de disponibilité. Les ressources de cluster SQL Server sur Linux ne sont pas couplées aussi étroitement au système d’exploitation, car elles se trouvent sur un cluster de basculement Windows Server (WSFC). Le service SQL Server n’est pas informé de la présence du cluster. Toutes les orchestrations sont effectuées via les outils d’administration de cluster. Dans RHEL ou Ubuntu utilisez `pcs` et dans SLES, utilisez les outils `crm`. 

Basculez manuellement le groupe de disponibilité avec `pcs`. N’initialisez pas le basculement avec Transact-SQL. Pour obtenir des instructions, consultez [Basculement](sql-server-linux-availability-group-failover-ha.md#failover).

## <a name="next-steps"></a>Étapes suivantes

[Opérer le groupe de disponibilité de haute disponibilité](sql-server-linux-availability-group-failover-ha.md)
