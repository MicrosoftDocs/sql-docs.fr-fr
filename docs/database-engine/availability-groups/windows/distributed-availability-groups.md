---
title: Que sont les groupes de disponibilité distribués ?
description: Un groupe de disponibilité distribué est un type spécial de groupe de disponibilité qui englobe deux groupes de disponibilité distincts. Les groupes de disponibilité qui participent à un groupe de disponibilité distribué n’ont pas besoin de se trouver au même emplacement.
ms.custom: seodec18
ms.date: 10/15/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], distributed
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: d0b6a1c97a1a5958811111915117793da8c416a0
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643619"
---
# <a name="distributed-availability-groups"></a>Groupes de disponibilité distribués
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Les groupes de disponibilité distribués sont une nouvelle fonctionnalité introduite dans SQL Server 2016, et constituent une variante de la fonctionnalité de groupes de disponibilité Always On existante. Cet article clarifie certains aspects des groupes de disponibilité distribués et complète la [documentation de SQL Server](../../../sql-server/index.yml) existante.

> [!NOTE]
> « DAG » n’est pas l’abréviation officielle de *distributed availability group* (groupe de disponibilité distribué), car elle est déjà utilisée pour la fonctionnalité de groupe de disponibilité de base de données Exchange. Cette fonctionnalité Exchange n’a aucune relation avec les groupes de disponibilité SQL Server ou les groupes de disponibilité distribués.

Pour configurer un groupe de disponibilité distribué, consultez [Configurer des groupes de disponibilité distribués](configure-distributed-availability-groups.md).

## <a name="understand-distributed-availability-groups"></a>Comprendre les groupes de disponibilité distribués

Un groupe de disponibilité distribué est un type spécial de groupe de disponibilité qui englobe deux groupes de disponibilité distincts. Les groupes de disponibilité qui participent à un groupe de disponibilité distribué n’ont pas besoin de se trouver au même emplacement. Ils peuvent être physiques, virtuels, locaux, ou se trouver dans le cloud public ou en tout lieu prenant en charge le déploiement d’un groupe de disponibilité. Ils peuvent notamment être sur plusieurs domaines et même sur plusieurs plateformes, comme entre un groupe de disponibilité hébergé sur Linux et un hébergé sur Windows. Tant que les deux groupes de disponibilité peuvent communiquer, vous pouvez configurer un groupe de disponibilité distribué avec eux.

Un groupe de disponibilité traditionnel dispose de ressources configurées dans un cluster WSFC. Un groupe de disponibilité distribué ne configure rien dans le cluster WSFC. Tout élément le concernant est géré dans SQL Server. Pour savoir comment afficher les informations relatives à un groupe de disponibilité distribué, consultez [Affichage des informations relatives aux groupes de disponibilité distribués](#monitor-distributed-availability-group-health). 

Un groupe de disponibilité distribué requiert que les groupes de disponibilité sous-jacents aient un écouteur. Au lieu de fournir le nom du serveur sous-jacent pour une instance autonome (ou, dans le cas d’une instance de cluster de basculement SQL Server, la valeur associée à la ressource de nom réseau) comme vous le feriez avec un groupe de disponibilité traditionnel, vous spécifiez l’écouteur configuré pour le groupe de disponibilité distribué avec le paramètre ENDPOINT_URL quand vous le créez. Bien que chaque groupe de disponibilité sous-jacent du groupe de disponibilité distribué ait un écouteur, un groupe de disponibilité distribué n’en a pas.

La figure suivante montre une vue générale d’un groupe de disponibilité distribué qui englobe deux groupes de disponibilité (AG 1 et AG 2), chacun configuré sur son propre cluster WSFC. Le groupe de disponibilité distribué a un total de quatre réplicas, à raison de deux par groupe de disponibilité. Chaque groupe de disponibilité peut prendre en charge le nombre maximal de réplicas, donc une disponibilité distribuée peut avoir jusqu’à 18 réplicas au total.


![Vue d’ensemble d’un groupe de disponibilité distribué](./media/distributed-availability-group/dag-01-high-level-view-distributed-ag.png)

Vous pouvez configurer le déplacement des données dans des groupes de disponibilité distribués sous forme synchrone ou asynchrone. Toutefois, le déplacement des données est légèrement différent au sein des groupes de disponibilité distribués par rapport à un groupe de disponibilité traditionnel. Bien que chaque groupe de disponibilité ait un réplica principal, une seule copie des bases de données participant à un groupe de disponibilité distribué accepte les insertions, les mises à jour et les suppressions. Comme indiqué dans la figure suivante, AG 1 est le groupe de disponibilité principal. Son réplica principal envoie les transactions aux réplicas secondaires d’AG 1 et au réplica principal d’AG2. Le réplica principal d’AG 2 est également appelé un *redirecteur*. Un redirecteur est un réplica principal dans un groupe de disponibilité secondaire au sein d’un groupe de disponibilité distribué. Le redirecteur reçoit des transactions du réplica principal dans le groupe de disponibilité principal et les transfère aux réplicas secondaires dans son propre groupe de disponibilité.  Le redirecteur maintient ensuite à jour les réplicas secondaires d’AG 2. 

![Groupe de disponibilité distribué et son déplacement de données](./media/distributed-availability-group/dag-02-distributed-ag-data-movement.png)

Pour que le réplica principal d’AG 2 accepte les insertions, les mises à jour et les suppressions, vous n’avez d’autre choix que de basculer manuellement le groupe de disponibilité distribué à partir d’AG 1. Dans la figure précédente, étant donné qu’AG 1 contient la copie accessible en écriture de la base de données, émettre un basculement fait d’AG 2 le groupe de disponibilité qui peut gérer les insertions, les mises à jour et les suppressions. Pour plus d’informations sur la façon de basculer un groupe de disponibilité distribué vers un autre, consultez [Basculer vers un groupe de disponibilité secondaire](configure-distributed-availability-groups.md#failover).

> [!NOTE]
> Les groupes de disponibilité distribués dans SQL Server 2016 prennent en charge le basculement uniquement depuis un groupe de disponibilité vers un autre à l’aide de l’option FORCE_FAILOVER_ALLOW_DATA_LOSS.

> [!NOTE]
> Quand la réplication transactionnelle est utilisée avec les groupes de disponibilité distribués, le réplica redirecteur ne peut pas être configuré comme serveur de publication.

## <a name="sql-server-version-and-edition-requirements-for-distributed-availability-groups"></a>Exigences des version et éditions de SQL Server pour les groupes de disponibilité distribués

Les groupes de disponibilité distribués dans SQL Server 2017 ou ultérieur peuvent combiner des versions principales de SQL Server dans le même groupe de disponibilité distribué. Le groupe de disponibilité contenant le réplica principal en lecture/écriture peut être d’une version identique ou antérieure aux autres groupes de disponibilité membres du groupe de disponibilité distribué. Les autres groupes de disponibilité peuvent être de la même version ou d’une version ultérieure. Ce scénario s’applique aux opérations de mise à niveau et de migration. Par exemple, si le groupe de disponibilité qui contient le réplica principal en lecture/écriture est SQL Server 2016, mais que vous souhaitez effectuer une mise à niveau ou une migration vers SQL Server 2017 ou une version ultérieure, l’autre groupe de disponibilité membre du groupe de disponibilité distribué peut être configuré avec SQL Server 2017.

Étant donné que la fonctionnalité de groupes de disponibilité distribués n’existe pas dans SQL Server 2012 ou 2014, les groupes de disponibilité qui ont été créés dans ces versions ne peuvent pas participer à des groupes de disponibilité distribués. 

> [!NOTE]
> Les groupes de disponibilité distribués ne peuvent pas être configurés avec l’édition Standard ou une combinaison de l’édition Standard et de l’édition Enterprise.

Étant donné qu’il existe deux groupes de disponibilité distincts, le processus d’installation d’un Service Pack ou d’une mise à jour cumulative sur un réplica qui fait partie d’un groupe de disponibilité distribué est légèrement différent de celui d’un groupe de disponibilité traditionnel :

1. Commencez par mettre à jour les réplicas du deuxième groupe de disponibilité dans le groupe de disponibilité distribué.

2. Appliquez le correctif aux réplicas du groupe de disponibilité principal dans le groupe de disponibilité distribué.

3. Comme dans le cas d’un groupe de disponibilité standard, basculez le groupe de disponibilité principal vers un de ses propres réplicas (pas vers le réplica principal du deuxième groupe de disponibilité) et appliquez-lui le correctif. Si le seul réplica existant est le réplica principal, un basculement manuel vers le deuxième groupe de disponibilité est nécessaire.

> [!NOTE]
> Aucune annonce n’a été faite indiquant si les futures versions de SQL Server autoriseront les versions antérieures à participer au même groupe de disponibilité distribué. Si ce scénario se concrétisait, les groupes de disponibilité distribués pourraient faire partie d’un plan de mise à niveau de version de SQL Server.

### <a name="windows-server-versions-and-distributed-availability-groups"></a>Versions de Windows Server et groupes de disponibilité distribués

Un groupe de disponibilité distribué englobe plusieurs groupes de disponibilité, chacun situé sur son propre cluster WSFC sous-jacent, et est une construction SQL Server uniquement.  Cela signifie que les clusters WSFC qui hébergent les différents groupes de disponibilité peuvent avoir différentes versions principales de Windows Server. Les versions principales de SQL Server doivent être les mêmes, comme indiqué dans la section précédente. Tout comme l’illustration initiale, la figure suivante montre AG 1 et AG 2 participant à un groupe de disponibilité distribué, mais chacun des clusters WSFC correspond à une version différente de Windows Server.


![Groupes de disponibilité distribués avec des clusters WSFC dotés de versions différentes de Windows Server](./media/distributed-availability-group/dag-03-distributed-ags-wsfcs-different-versions-windows-server.png)

Les clusters WSFC individuels et leurs groupes de disponibilité correspondants suivent les règles traditionnelles. En d’autres termes, ils peuvent être joints ou non à un domaine (Windows Server 2016 ou version ultérieure). Quand deux groupes de disponibilité différents sont combinés dans un groupe de disponibilité distribué unique, quatre scénarios sont possibles :

* Les deux clusters WSFC sont joints au même domaine.
* Chaque cluster WSFC est joint à un domaine différent.
* Un seul des clusters WSFC est joint à un domaine.
* Aucun des clusters WSFC n’est joint à un domaine.

Si les deux clusters WSFC sont joints au même domaine (domaines non approuvés), aucune opération particulière n’est nécessaire de votre part quand vous créez le groupe de disponibilité distribué. Pour les groupes de disponibilité et les clusters WSFC qui ne sont pas joints au même domaine, utilisez des certificats pour que le groupe de disponibilité distribué fonctionne, à l’image de la création d’un groupe de disponibilité pour un groupe de disponibilité indépendant du domaine. Pour savoir comment configurer des certificats pour un groupe de disponibilité distribué, suivez les étapes 3 à 13 de la section [Créer un groupe de disponibilité indépendant du domaine](domain-independent-availability-groups.md).

Dans le cas d’un groupe de disponibilité distribué, le réplica principal de chaque groupe de disponibilité sous-jacent doit disposer des certificats des autres réplicas principaux. Si vous avez déjà des points de terminaison qui n’utilisent pas de certificats, reconfigurez ces points de terminaison à l’aide de l’instruction [ALTER ENDPOINT](../../../t-sql/statements/alter-endpoint-transact-sql.md) afin de refléter l’utilisation de certificats.

## <a name="distributed-availability-group-usage-scenarios"></a>Scénarios d’utilisation des groupes de disponibilité distribués

Voici les trois principaux scénarios d’utilisation d’un groupe de disponibilité distribué : 

* [Récupération d’urgence et configurations multisite plus faciles](#disaster-recovery-and-multi-site-scenarios)
* [Migration vers un nouveau matériel ou vers de nouvelles configurations, ce qui peut signifier l’utilisation d’un nouveau matériel ou le changement de système d’exploitation sous-jacent](#migrate-by-using-a-distributed-availability-group)
* [Augmentation au-delà de huit du nombre de réplicas lisibles dans un même groupe de disponibilité en englobant plusieurs groupes de disponibilité](#scale-out-readable-replicas-with-distributed-availability-groups)

### <a name="disaster-recovery-and-multi-site-scenarios"></a>Scénarios de récupération d’urgence et de configuration multisite

Un groupe de disponibilité traditionnel requiert que tous les serveurs fassent partie du même cluster WSFC, ce qui peut compliquer la couverture de plusieurs centres de données. La figure suivante présente l’architecture d’un groupe de disponibilité traditionnel multisite, flux de données compris. Il existe un réplica principal qui envoie les transactions à tous les réplicas secondaires. Cette configuration est moins souple à certains égards qu’un groupe de disponibilité distribué. Par exemple, vous devez implémenter des éléments tels qu’Active Directory (le cas échéant) et le témoin d’un quorum dans le cluster WSFC. Vous pouvez également être amené à prendre en compte d’autres aspects d’un cluster WSFC, tels que la modification des votes de nœud.

![Groupe de disponibilité traditionnel multisite](./media/distributed-availability-group/dag-04-traditional-multi-site-ag.png)

Les groupes de disponibilité distribués offrent un scénario de déploiement plus flexible pour les groupes de disponibilité qui couvrent plusieurs centres de données. Vous pouvez même vous servir de groupes de disponibilité distribués là où des fonctionnalités telles que la [copie des journaux de transaction]( https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) étaient utilisées par le passé dans des situations de type récupération d'urgence. Toutefois, contrairement à la copie des journaux de transaction, les groupes de disponibilité distribués ne sont pas compatibles avec l’application différée des transactions. Cela signifie que les groupes de disponibilité ou les groupes de disponibilité distribués ne peuvent être d’aucun secours si une erreur humaine entraîne une mise à jour ou une suppression de données incorrecte.

Les groupes de disponibilité distribués sont faiblement couplés, ce qui signifie dans ce cas qu’ils ne nécessitent pas de cluster WSFC et qu’ils sont gérés par SQL Server. Étant donné que les clusters WSFC sont gérés de façon individuelle et que la synchronisation est principalement asynchrone entre les deux groupes de disponibilité, il est plus facile de configurer la récupération d’urgence sur un autre site. Les réplicas principaux dans chaque groupe de disponibilité synchronisent leurs propres réplicas secondaires.

* Seul le basculement manuel est pris en charge pour un groupe de disponibilité distribué. Dans une situation de récupération d’urgence où vous permutez les centres de données, vous ne devez pas configurer le basculement automatique (à de rares exceptions près). 
* Dans la majorité des cas, vous n’avez pas besoin de définir certains des éléments ou paramètres traditionnels pour les clusters WSFC multisites ou de sous-réseau, tels que CrossSubnetThreshold, mais vous devez toujours vous occuper de la latence réseau à une couche différente pour le transport des données. La différence est que chaque cluster WSFC gère sa propre disponibilité ; le cluster n’est pas une grande entité de quatre nœuds. Vous avez deux clusters WSFC à deux nœuds, comme indiqué dans la figure précédente.  
* Nous recommandons le déplacement de données asynchrone, qui convient pour la récupération d’urgence.
* Si vous configurez le déplacement de données synchrone entre le réplica principal et au moins un réplica secondaire du deuxième groupe de disponibilité, et que vous configurez le déplacement synchrone sur le groupe de disponibilité distribué, un groupe de disponibilité distribué attend que toutes les copies synchrones reconnaissent qu’elles disposent des données.

### <a name="migrate-by-using-a-distributed-availability-group"></a>Migrer à l’aide d’un groupe de disponibilité distribué

Étant donné que les groupes de disponibilité distribués prennent en charge deux configurations de groupe de disponibilité complètement différentes, ils facilitent non seulement les scénarios multisites et de récupération d’urgence, mais également les scénarios de migration. Que vous effectuiez une migration vers un nouveau matériel ou des machines virtuelles (localement ou de type IaaS dans le cloud public), configurer un groupe de disponibilité distribué permet d’effectuer une migration là où, dans le passé, vous auriez utilisé une sauvegarde, une copie, une restauration ou une copie des journaux de transaction. 

La possibilité de migrer est particulièrement utile dans les scénarios où vous changez ou mettez à niveau le système d’exploitation sous-jacent tout en conservant la même version de SQL Server. Bien que Windows Server 2016 permette une mise à niveau propagée à partir de Windows Server 2012 R2 sur le même matériel, la plupart des utilisateurs choisissent de déployer un nouveau matériel ou des machines virtuelles. 

Pour effectuer la migration vers la nouvelle configuration, à la fin du processus, arrêtez tout le trafic de données vers le groupe de disponibilité d’origine et paramétrez le groupe de disponibilité distribué afin qu’il utilise le déplacement de données synchrone. Cette action garantit que le réplica principal du deuxième groupe de disponibilité est entièrement synchronisé, empêchant toute perte de données. Après avoir vérifié la synchronisation, basculez le groupe de disponibilité distribué vers le deuxième groupe de disponibilité. Pour plus d’informations, consultez [Basculer vers un groupe de disponibilité secondaire](configure-distributed-availability-groups.md#failover).

Après la migration, une fois que le deuxième groupe de disponibilité fait office de nouveau groupe de disponibilité principal, vous pouvez être amené à effectuer l’une des opérations suivantes :

* Renommez l’écouteur sur le groupe de disponibilité secondaire (et éventuellement supprimez ou renommez l’ancien sur le groupe de disponibilité principal d’origine), ou recréez-le avec l’écouteur du groupe de disponibilité principal d’origine, afin que les applications et les utilisateurs puissent accéder à la nouvelle configuration.
* Si un renommage ou une recréation est impossible, faites pointer les applications et les utilisateurs vers l’écouteur sur le deuxième groupe de disponibilité.

### <a name="scale-out-readable-replicas-with-distributed-availability-groups"></a>Effectuer un scale-out des réplicas lisibles avec des groupes de disponibilité distribués

Un même groupe de disponibilité distribué peut avoir jusqu’à 16 réplicas secondaires, en fonction des besoins. Ainsi, il peut avoir 18 copies à des fins de lecture, y compris les deux réplicas principaux des différents groupes de disponibilité. Cette approche signifie que plusieurs sites peuvent avoir accès pratiquement en temps réel à différentes applications pour des opérations de rapport.

Les groupes de disponibilité distribués peuvent vous aider à effectuer un scale-out d’une batterie de serveurs en lecture seule, plus qu’avec simplement un seul groupe de disponibilité. Un groupe de disponibilité distribué peut effectuer un scale-out des réplicas lisibles de deux manières :

* Vous pouvez utiliser le réplica principal du deuxième groupe de disponibilité dans un groupe de disponibilité distribué pour créer un autre groupe de disponibilité distribué, même si la base de données n’est pas dans RECOVERY.
* Vous pouvez également utiliser le réplica principal du premier groupe de disponibilité pour créer un autre groupe de disponibilité distribué.

En d’autres termes, un réplica principal peut participer à deux groupes de disponibilité distribués différents. Dans la figure suivante, AG 1 et AG 2 participent au groupe de disponibilité distribué AG 1, tandis qu’AG 2 et AG 3 participent au groupe de disponibilité distribué AG 2. Le réplica principal (ou redirecteur) d’AG 2 est à la fois un réplica secondaire pour le groupe de disponibilité distribué AG 1 et un réplica principal du groupe de disponibilité distribué AG 2.

![Augmentation de la taille des instances des lectures avec des groupes de disponibilité distribués](./media/distributed-availability-group/dag-05-scaling-out-reads-with-distributed-ags.png)

Dans la figure suivante, AG 1 fait office de réplica principal pour deux groupes de disponibilité distribués : le groupe de disponibilité distribué AG 1 (composé d’AG 1 et AG 2) et le groupe de disponibilité distribué AG 2 (composé d’AG 1 et AG 3).


![Autre exemple d’augmentation de la taille des instances des lectures avec des groupes de disponibilité distribués]( ./media/distributed-availability-group/dag-06-another-scaling-out-reads-using-distributed-ags-example.png)


Dans les deux exemples précédents, les trois groupes de disponibilité peuvent comprendre jusqu’à 27 réplicas en tout, qui peuvent tous être utilisés pour des requêtes en lecture seule. 

Le [routage en lecture seule]( https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server) ne fonctionne pas complètement avec les groupes de disponibilité distribués. Plus précisément :

1. Le routage en lecture seule peut être configuré et fonctionne pour le groupe de disponibilité principal du groupe de disponibilité distribué. 
2. Le routage en lecture seule peut être configuré, mais ne fonctionne pas pour le groupe de disponibilité secondaire du groupe de disponibilité distribué. Toutes les requêtes, si elles utilisent l’écouteur pour se connecter au groupe de disponibilité secondaire, accèdent au réplica principal du groupe de disponibilité secondaire. Sinon, vous devez configurer chaque réplica pour autoriser toutes les connexions en tant que réplica secondaire et y accéder directement. Toutefois, le routage en lecture seule fonctionne si le groupe de disponibilité secondaire devient principal après un basculement. Ce comportement pourrait être modifié dans une mise à jour vers SQL Server 2016 ou dans une future version de SQL Server.


## <a name="initialize-secondary-availability-groups-in-a-distributed-availability-group"></a>Initialiser des groupes de disponibilité secondaires dans un groupe de disponibilité distribué

Bénéficiant de [l’amorçage automatique](./automatically-initialize-always-on-availability-group.md), les groupes de disponibilité distribués constituent la méthode principale pour initialiser le réplica principal sur le deuxième groupe de disponibilité. Une restauration complète de la base de données sur le réplica principal du deuxième groupe de disponibilité est possible si vous effectuez les opérations suivantes :

1. Restaurez la sauvegarde de base de données à l’aide de WITH NORECOVERY.
2. Si nécessaire, restaurez les sauvegardes de fichier journal appropriées à l’aide de WITH NORECOVERY.
3. Créez le deuxième groupe de disponibilité sans spécifier de nom de base de données et en définissant SEEDING_MODE sur AUTOMATIC.
4. Créez le groupe de disponibilité distribué à l’aide de l’amorçage automatique.

Quand vous ajoutez le réplica principal du deuxième groupe de disponibilité au groupe de disponibilité distribué, le réplica est vérifié par rapport aux bases de données primaires du premier groupe de disponibilité, et l’amorçage prend la base de données jusqu’à la source. Il existe quelques inconvénients :

* La sortie dans `sys.dm_hadr_automatic_seeding` sur le réplica principal du deuxième groupe de disponibilité affiche un `current_state` de valeur FAILED avec le motif « Seeding Check Message Timeout » (Expiration du message de vérification de l’amorçage).

* Le journal SQL Server actuel sur le réplica principal du deuxième groupe de disponibilité indique que l’amorçage a fonctionné et que les [LSN](../../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md) ont été synchronisés.

* La sortie dans `sys.dm_hadr_automatic_seeding` sur le réplica principal du premier groupe de disponibilité indique que current_state a pour valeur COMPLETED. 

* L’amorçage a également un comportement différent avec des groupes de disponibilité distribués. Pour que l’amorçage commence sur le deuxième réplica, vous devez y émettre la commande `ALTER AVAILABILITY GROUP [AGName] GRANT CREATE ANY DATABASE`. Bien que cette condition s’applique systématiquement à tout réplica secondaire participant au groupe de disponibilité sous-jacent, le réplica principal du deuxième groupe de disponibilité a déjà les autorisations appropriées pour permettre à l’amorçage de commencer une fois qu’il est ajouté au groupe de disponibilité distribué. 

## <a name="monitor-distributed-availability-group-health"></a>Surveiller l’intégrité du groupe de disponibilité distribué

Un groupe de disponibilité distribué est une construction SQL Server uniquement, et il n’est pas visible dans le cluster WSFC sous-jacent. La figure suivante illustre deux clusters WSFC différents (CLUSTER_A et CLUSTER_B), chacun ayant ses propres groupes de disponibilité. Seuls AG1 dans CLUSTER_A et AG2 dans CLUSTER_B sont présentés ici. 

[Deux clusters WSFC avec plusieurs groupes de disponibilité via la commande PowerShell Get-ClusterGroup](./media/distributed-availability-group/dag-07-two-wsfcs-multiple-ags-through-get-clustergroup-command.png)


```
PS C:\> Get-ClusterGroup -Cluster CLUSTER_A

Name                            OwnerNode             State
----                            ---------             -----
AG1                             DENNIS                Online
Available Storage               GLEN                  Offline
Cluster Group                   JY                    Online
New_RoR                         DENNIS                Online
Old_RoR                         DENNIS                Online
SeedingAG                       DENNIS                Online


PS C:\> Get-ClusterGroup -Cluster CLUSTER_B

Name                            OwnerNode             State
----                            ---------             -----
AG2                             TOMMY                 Online
Available Storage               JC                    Offline
Cluster Group                   JC                    Online
```

Toutes les informations détaillées sur un groupe de disponibilité distribué sont dans SQL Server, plus spécifiquement dans les vues de gestion dynamique du groupe de disponibilité. Actuellement, la seule information indiquée dans SQL Server Management Studio pour un groupe de disponibilité distribué est sur le réplica principal pour les groupes de disponibilité. Comme l’illustre la figure suivante, sous le dossier Groupes de disponibilité, SQL Server Management Studio indique qu’il existe un groupe de disponibilité distribué. La figure montre AG1 comme réplica principal pour un groupe de disponibilité qui est local à cette instance, et non pour un groupe de disponibilité distribué.

![Vue dans SQL Server Management Studio du réplica principal sur le premier cluster WSFC d’un groupe de disponibilité distribué](./media/distributed-availability-group/dag-08-view-smss-primary-replica-first-wsfc-distributed-ag.png)


Toutefois, si vous cliquez sur le groupe de disponibilité distribué, aucune option n’est disponible (voir la figure suivante) et les dossiers Bases de données de disponibilité, Écouteurs de groupe de disponibilité et Réplicas de disponibilité développés sont tous vides. SQL Server Management Studio 16 affiche ce résultat, mais cela peut changer dans une version ultérieure de SQL Server Management Studio.

![Aucune option disponible pour l’action](./media/distributed-availability-group/dag-09-no-options-available-action.png)

Comme le montre la figure ci-dessous, les réplicas secondaires n’affichent rien dans SQL Server Management Studio concernant le groupe de disponibilité distribué. Ces noms de groupes de disponibilité correspondent aux rôles indiqués dans l’image de cluster WSFC CLUSTER_A précédente.

![Vue dans SQL Server Management Studio d’un réplica secondaire](./media/distributed-availability-group/dag-10-view-ssms-secondary-replica.png)

### <a name="dmv-to-list-all-availability-replica-names"></a>Vue de gestion dynamique pour répertorier tous les noms de réplica de disponibilité

Les mêmes concepts sont valables quand vous utilisez les vues de gestion dynamique. En exécutant la requête suivante, vous pouvez voir tous les groupes de disponibilité (réguliers et distribués) et les nœuds qui y participent. Ce résultat s’affiche uniquement si vous interrogez le réplica principal dans l’un des clusters WSFC qui font partie du groupe de disponibilité distribué. Il existe une nouvelle colonne dans la vue de gestion dynamique `sys.availability_groups` nommée `is_distributed`, qui a la valeur 1 quand le groupe de disponibilité est un groupe de disponibilité distribué. Pour afficher cette colonne

```sql
-- shows replicas associated with availability groups
SELECT 
   ag.[name] AS [AG Name], 
   ag.Is_Distributed, 
   ar.replica_server_name AS [Replica Name]
FROM sys.availability_groups AS ag 
INNER JOIN sys.availability_replicas AS ar       
   ON ag.group_id = ar.group_id
GO
```

La figure suivante montre un exemple de sortie du deuxième cluster WSFC qui participe à un groupe de disponibilité distribué. SPAG1 est composé de deux réplicas : DENNIS et JY. Toutefois, le groupe de disponibilité distribué nommé SPDistAG a les noms des deux groupes de disponibilité participants (SPAG1 et SPAG2) plutôt que les noms des instances, comme avec un groupe de disponibilité traditionnel. 

![Exemple de sortie de la requête précédente](./media/distributed-availability-group/dag-11-example-output-of-query-above.png)

### <a name="dmv-to-list-distributed-ag-health"></a>Vue de gestion dynamique pour afficher l’intégrité du groupe de disponibilité distribué

Dans SQL Server Management Studio, tout état indiqué dans le Tableau de bord et d’autres zones concerne uniquement la synchronisation locale dans ce groupe de disponibilité. Pour afficher l’intégrité d’un groupe de disponibilité distribué, interrogez les vues de gestion dynamique. L’exemple de requête suivante étend et affine la requête précédente :

```sql
-- shows sync status of distributed AG
SELECT 
   ag.[name] AS [AG Name], 
   ag.is_distributed, 
   ar.replica_server_name AS [Underlying AG], 
   ars.role_desc AS [Role], 
   ars.synchronization_health_desc AS [Sync Status]
FROM  sys.availability_groups AS ag
INNER JOIN sys.availability_replicas AS ar 
   ON  ag.group_id = ar.group_id        
INNER JOIN sys.dm_hadr_availability_replica_states AS ars       
   ON  ar.replica_id = ars.replica_id
WHERE ag.is_distributed = 1
GO
```
       
       
![État des groupes de disponibilité distribués](./media/distributed-availability-group/dag-12-distributed-ag-status.png)

### <a name="dmv-to-view-underlying-performance"></a>Vue de gestion dynamique pour afficher les performances sous-jacentes

Pour étendre la requête précédente, vous pouvez également afficher les performances sous-jacentes par le biais des vues de gestion dynamique en ajoutant `sys.dm_hadr_database_replicas_states`. Actuellement, la vue de gestion dynamique stocke des informations concernant uniquement le deuxième groupe de disponibilité. L’exemple de requête suivant, exécuté sur le groupe de disponibilité principal, génère l’exemple de sortie ci-dessous :

```sql
-- shows underlying performance of distributed AG
SELECT 
   ag.[name] AS [Distributed AG Name], 
   ar.replica_server_name AS [Underlying AG], 
   dbs.[name] AS [Database],
   ars.role_desc AS [Role],
   drs.synchronization_health_desc AS [Sync Status],
   drs.log_send_queue_size,
   drs.log_send_rate, 
   drs.redo_queue_size, 
   drs.redo_rate
FROM sys.databases AS dbs
INNER JOIN sys.dm_hadr_database_replica_states AS drs
   ON dbs.database_id = drs.database_id
INNER JOIN sys.availability_groups AS ag
   ON drs.group_id = ag.group_id
INNER JOIN sys.dm_hadr_availability_replica_states AS ars
   ON ars.replica_id = drs.replica_id
INNER JOIN sys.availability_replicas AS ar
   ON ar.replica_id = ars.replica_id
WHERE ag.is_distributed = 1
GO
```


![Informations sur les performances d’un groupe de disponibilité distribué](./media/distributed-availability-group/dag-13-performance-information-distributed-ag.png)

### <a name="dmv-to-view-performance-counters-for-distributed-ag"></a>Vue de gestion dynamique pour afficher les compteurs de performances pour le groupe de disponibilité distribué
La requête ci-dessous affiche les compteurs de performances associés spécifiquement au groupe de disponibilité distribué. 


 ```sql
 -- displays OS performance counters related to the distributed ag named 'distributedag'
 SELECT * FROM sys.dm_os_performance_counters WHERE instance_name LIKE '%distributed%'
 ```

 ![Vue de gestion dynamique affichant les compteurs de performances du système d’exploitation pour le groupe de disponibilité distribué](./media/distributed-availability-group/dmv-os-performance-counters.png)


 >[!NOTE]
 >Le filtre `LIKE` doit porter le nom du groupe de disponibilité distribué. Dans cet exemple, le nom du groupe de disponibilité distribué est « distributedag ». Changez le modificateur `LIKE` afin de refléter le nom de votre groupe de disponibilité distribué.  

### <a name="dmv-to-display-health-of-both-ag-and-distributed-ag"></a>Vue de gestion dynamique pour afficher l’intégrité du groupe de disponibilité et du groupe de disponibilité distribué
La requête ci-dessous affiche une mine d’informations sur l’intégrité du groupe de disponibilité et du groupe de disponibilité distribué. [Merci Tracy Boggiano !](https://tracyboggiano.com/archive/2017/11/distributed-availability-groups-setup-and-monitoring/)

 ```sql
 -- displays sync status, send rate, and redo rate of availability groups, including distributed AG
 SELECT 
        ag.name AS 'AG Name', 
        ag.is_distributed, 
        ar.replica_server_name AS 'AG', 
        dbs.name AS 'Database',
    ars.role_desc, 
    drs.synchronization_health_desc, 
    drs.log_send_queue_size, 
    drs.log_send_rate, 
    drs.redo_queue_size, 
    drs.redo_rate,
    drs.suspend_reason_desc,
    drs.last_sent_time,
    drs.last_received_time,
    drs.last_hardened_time,
    drs.last_redone_time,
    drs.last_commit_time,
    drs.secondary_lag_seconds
 FROM sys.databases dbs 
 INNER JOIN sys.dm_hadr_database_replica_states drs 
    ON dbs.database_id = drs.database_id
 INNER JOIN sys.availability_groups ag 
    ON drs.group_id = ag.group_id
 INNER JOIN sys.dm_hadr_availability_replica_states ars 
    ON ars.replica_id = drs.replica_id
 INNER JOIN sys.availability_replicas ar 
    ON ar.replica_id =  ars.replica_id
 --WHERE ag.is_distributed = 1
 GO
 ```

![Intégrité du groupe de disponibilité et du groupe de disponibilité distribué](./media/distributed-availability-group/dmv-sync-status-send-rate.png)

### <a name="dmvs-to-view-metadata-of-distributed-ag"></a>Vues de gestion dynamique pour afficher les métadonnées du groupe de disponibilité distribué
Les requêtes ci-dessous affichent des informations sur les URL de point de terminaison utilisées par les groupes de disponibilité, dont le groupe de disponibilité distribué.  [Merci David Barbarin !](https://blog.dbi-services.com/sql-server-2016-alwayson-distributed-availability-groups/)



 ```sql
 -- shows endpoint url and sync state for ag, and dag
 SELECT
    ag.name AS group_name,
    ag.is_distributed,
    ar.replica_server_name AS replica_name,
    ar.endpoint_url,
    ar.availability_mode_desc,
    ar.failover_mode_desc,
    ar.primary_role_allow_connections_desc AS allow_connections_primary,
    ar.secondary_role_allow_connections_desc AS allow_connections_secondary,
    ar.seeding_mode_desc AS seeding_mode
 FROM sys.availability_replicas AS ar
 JOIN sys.availability_groups AS ag
    ON ar.group_id = ag.group_id
 GO
 ```


![Vue de gestion dynamique des métadonnées pour le groupe de disponibilité distribué](./media/distributed-availability-group/dmv-metadata-dag2.png)



### <a name="dmv-to-show-current-state-of-seeding"></a>Vue de gestion dynamique pour afficher l’état actuel de l’amorçage
La requête ci-dessous affiche des informations sur l’état actuel de l’amorçage. Cela est utile pour le dépannage des erreurs de synchronisation entre les réplicas. [Encore merci David Barbarin !](https://blog.dbi-services.com/sql-server-2016-alwayson-distributed-availability-groups/)

 ```sql
 -- shows current_state of seeding 
 SELECT
    ag.name AS aag_name,
    ar.replica_server_name,
    d.name AS database_name,
    has.current_state,
    has.failure_state_desc AS failure_state,
    has.error_code,
    has.performed_seeding,
    has.start_time,
    has.completion_time,
    has.number_of_attempts
 FROM sys.dm_hadr_automatic_seeding AS has
 JOIN sys.availability_groups AS ag
    ON ag.group_id = has.ag_id
 JOIN sys.availability_replicas AS ar
    ON ar.replica_id = has.ag_remote_replica_id
 JOIN sys.databases AS d
    ON d.group_database_id = has.ag_db_id
 GO
 ```


![État actuel de l’amorçage](./media/distributed-availability-group/dmv-seeding.png)




### <a name="next-steps"></a>Étapes suivantes 

* [Utiliser l’Assistant Groupe de disponibilité (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md)

* [Utiliser la boîte de dialogue Nouveau groupe de disponibilité (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
* [Créer un groupe de disponibilité avec Transact-SQL](create-an-availability-group-transact-sql.md)