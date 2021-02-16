---
title: Configurer un groupe de disponibilité pour SQL Server sur Linux
description: Découvrez comment créer un groupe de disponibilité Always On SQL Server pour la haute disponibilité sur Linux.
author: VanMSFT
ms.custom: seo-lt-2019
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/26/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 7313fbc789b37cf326d55566c1f087f80b4929df
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351929"
---
# <a name="configure-sql-server-always-on-availability-group-for-high-availability-on-linux"></a>Configurer le groupe de disponibilité Always On SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article décrit comment créer un groupe de disponibilité Always On SQL Server pour la haute disponibilité sur Linux. Il existe deux types de configuration pour les groupes de disponibilité. Une configuration de *haute disponibilité* utilise un gestionnaire de clusters pour fournir la continuité des activités. Cette configuration peut également inclure des réplicas en échelle lecture. Ce document explique comment créer le groupe de disponibilité pour la haute disponibilité.

Vous pouvez également créer un groupe de disponibilité sans gestionnaire de clusters pour une *échelle lecture*. Le groupe de disponibilité pour l’échelle lecture fournit uniquement des réplicas en lecture seule pour le scale-out des performances. Il n’assure pas la haute disponibilité. Pour créer un groupe de disponibilité en échelle lecture, consultez [Configurer un groupe de disponibilité SQL Server pour l’échelle lecture sur Linux](sql-server-linux-availability-group-configure-rs.md).

Les configurations qui garantissent une haute disponibilité et la protection des données requièrent deux ou trois réplicas de validation synchrone. Avec trois réplicas synchrones, le groupe de disponibilité peut effectuer une récupération automatique même si un serveur n’est pas disponible. Pour plus d’informations, consultez [Haute disponibilité et protection des données pour les configurations des groupes de disponibilité](sql-server-linux-availability-group-ha.md). 

Tous les serveurs doivent être physiques ou virtuels et les serveurs virtuels doivent se trouver sur la même plateforme de virtualisation. Cette exigence est due au fait que les agents d’isolation sont spécifiques à la plateforme. Consultez [Stratégies pour les clusters invités](https://access.redhat.com/articles/29440#guest_policies).

## <a name="roadmap"></a>Feuille de route

Les étapes de création d’un groupe de disponibilité sur des serveurs Linux pour la haute disponibilité diffèrent de celles d’un cluster de basculement Windows Server. La liste suivante décrit les différentes étapes de haut niveau : 

1. [Configurez SQL Server sur trois serveurs de cluster](sql-server-linux-setup.md).

   >[!IMPORTANT]
   >Les trois serveurs du groupe de disponibilité doivent se trouver sur la même plateforme, physique ou virtuelle, car la haute disponibilité Linux utilise des agents d’isolation pour isoler les ressources sur les serveurs. Les agents d’isolation sont spécifiques à chaque plateforme.

2. Créez le groupe de disponibilité. Cette étape est traitée dans cet article en cours. 

3. Configurez un gestionnaire de ressources de cluster, comme Pacemaker.
   
   La façon de configurer un gestionnaire de ressources de cluster dépend de la distribution Linux spécifique. Pour obtenir des instructions spécifiques à la distribution, consultez les liens suivants : 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md)
   * [SUSE](sql-server-linux-availability-group-cluster-sles.md)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md)

   >[!IMPORTANT]
   >Les environnements de production nécessitent un agent d’isolation, comme STONITH pour la haute disponibilité. Les démonstrations de cette documentation n’utilisent pas les agents d’isolation. Elles sont destinées uniquement à des fins de test et de validation. 
   
   >Un cluster Linux utilise l’isolation pour ramener le cluster à un état connu. La façon de configurer l’isolation dépend de la distribution et de l’environnement. À ce stade, l’isolation n’est pas disponible dans certains environnements cloud. Pour plus d’informations, consultez [Stratégies de support pour les clusters à haute disponibilité RHEL - Plateformes de virtualisation](https://access.redhat.com/articles/29440).
   
   >Pour SLES, consultez [Extension de haute disponibilité SUSE Linux Enterprise](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing).

5. Ajoutez le groupe de disponibilité en tant que ressource dans le cluster.  

   La façon d’ajouter le groupe de disponibilité en tant que ressource dans le cluster dépend de la distribution Linux. Pour obtenir des instructions spécifiques à la distribution, consultez les liens suivants : 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource)
   * [SLES](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource)

[!INCLUDE [Create Prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>Créer le groupe de disponibilité

Les exemples de cette section expliquent comment créer le groupe de disponibilité à l’aide de Transact-SQL. Vous pouvez également utiliser l’Assistant Groupe de disponibilité SQL Server Management Studio. Lorsque vous créez un groupe de disponibilité à l’aide de l’Assistant, une erreur est renvoyée lorsque vous joignez les réplicas au groupe de disponibilité. Pour résoudre ce problème, accordez `ALTER`, `CONTROL` et `VIEW DEFINITIONS` à Pacemaker sur le groupe de disponibilité à tous les réplicas. Une fois les autorisations octroyées sur le réplica principal, joignez les nœuds au groupe de disponibilité à l’aide de l’Assistant, mais pour que la haute disponibilité fonctionne correctement, octroyez l’autorisation à tous les réplicas.

Pour une configuration de haute disponibilité qui garantit le basculement automatique, le groupe de disponibilité requiert au moins trois réplicas. L’une des configurations suivantes peut prendre en charge la haute disponibilité :

- [Trois réplicas synchrones](sql-server-linux-availability-group-ha.md#threeSynch)

- [Deux réplicas synchrones et un réplica de configuration](sql-server-linux-availability-group-ha.md#twoSynch)

Pour plus d’informations, consultez [Haute disponibilité et protection des données pour les configurations des groupes de disponibilité](sql-server-linux-availability-group-ha.md).

>[!NOTE]
>Les groupes de disponibilité peuvent inclure des réplicas synchrones ou asynchrones supplémentaires. 

Créez le groupe de disponibilité pour la haute disponibilité sur Linux. Utilisez [CREATE AVAILABILITY GROUP](../t-sql/statements/create-availability-group-transact-sql.md) avec `CLUSTER_TYPE = EXTERNAL`. 

* Groupe de disponibilité : `CLUSTER_TYPE = EXTERNAL` spécifie qu’une entité de cluster externe gère le groupe de disponibilité. Pacemaker est un exemple d’entité de cluster externe. Lorsque le type de cluster du groupe de disponibilité est externe, 

* Définissez les réplicas principaux et secondaires `FAILOVER_MODE = EXTERNAL`. 
   Spécifie que le réplica interagit avec un gestionnaire de clusters externe, comme Pacemaker. 

Les scripts Transact-SQL suivants créent un groupe de disponibilité pour la haute disponibilité nommé `ag1`. Le script configure les réplicas de groupe de disponibilité avec `SEEDING_MODE = AUTOMATIC`. Ce paramètre permet à SQL Server de créer automatiquement la base de données sur chaque serveur secondaire. Mettez à jour le script suivant en fonction de votre environnement. Remplacez les valeurs `<node1>`, `<node2>` et `<node3>` par les noms des instances de SQL Server qui hébergent les réplicas. Remplacez la valeur `<5022>` par le port défini pour le point de terminaison de mise en miroir des données. Pour créer le groupe de disponibilité, exécutez Transact-SQL suivant sur l’instance hébergeant le réplica principal.

Exécutez **seulement un** des scripts suivants : 

- [Créez un groupe de disponibilité avec trois réplicas synchrones](#threeSynch).
- [Créer un groupe de disponibilité avec deux réplicas synchrones et un réplica de configuration](#configOnly)
- [Créez un groupe de disponibilité avec deux réplicas synchrones](#readScale).

<a name="threeSynch"></a>

- Créer un groupe de disponibilité avec trois réplicas synchrones

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
       WITH (DB_FAILOVER = ON, CLUSTER_TYPE = EXTERNAL)
       FOR REPLICA ON
           N'<node1>' 
            WITH (
               ENDPOINT_URL = N'tcp://<node1>:<5022>',
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node2>' 
            WITH ( 
               ENDPOINT_URL = N'tcp://<node2>:<5022>', 
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node3>'
           WITH( 
              ENDPOINT_URL = N'tcp://<node3>:<5022>', 
              AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
              FAILOVER_MODE = EXTERNAL,
              SEEDING_MODE = AUTOMATIC
              );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```

   >[!IMPORTANT]
   >Après avoir exécuté le script précédent pour créer un groupe de disponibilité avec trois réplicas synchrones, n’exécutez pas le script suivant :

<a name="configOnly"></a>
- Créez un groupe de disponibilité avec deux réplicas synchrones et un réplica de configuration :

   >[!IMPORTANT]
   >Cette architecture permet à n’importe quelle édition de SQL Server d’héberger le troisième réplica. Par exemple, le troisième réplica peut être hébergé sur SQL Server Express Edition. Sur Express Edition, le seul type de point de terminaison valide est `WITNESS`. 

   ```SQL
   CREATE AVAILABILITY GROUP [ag1] 
      WITH (CLUSTER_TYPE = EXTERNAL) 
      FOR REPLICA ON 
       N'<node1>' WITH ( 
          ENDPOINT_URL = N'tcp://<node1>:<5022>', 
          AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
          FAILOVER_MODE = EXTERNAL, 
          SEEDING_MODE = AUTOMATIC 
          ), 
       N'<node2>' WITH (  
          ENDPOINT_URL = N'tcp://<node2>:<5022>',  
          AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
          FAILOVER_MODE = EXTERNAL, 
          SEEDING_MODE = AUTOMATIC 
          ), 
       N'<node3>' WITH ( 
          ENDPOINT_URL = N'tcp://<node3>:<5022>', 
          AVAILABILITY_MODE = CONFIGURATION_ONLY  
          );
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```
<a name="readScale"></a>

- Créer un groupe de disponibilité avec deux réplicas synchrones

   Incluez deux réplicas avec le mode de disponibilité synchrone. Par exemple, le script suivant crée un groupe de disponibilité appelé `ag1`. `node1` et `node2` hébergent des réplicas en mode synchrone, avec l’amorçage automatique et le basculement automatique.

   >[!IMPORTANT]
   >Exécutez uniquement le script suivant pour créer un groupe de disponibilité avec deux réplicas synchrones. N’exécutez pas le script suivant si vous avez exécuté le script précédent. 

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
      WITH (CLUSTER_TYPE = EXTERNAL)
      FOR REPLICA ON
      N'node1' WITH (
         ENDPOINT_URL = N'tcp://node1:5022',
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      ),
      N'node2' WITH ( 
         ENDPOINT_URL = N'tcp://node2:5022', 
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```


Vous pouvez également configurer un groupe de disponibilité avec `CLUSTER_TYPE=EXTERNAL` à l’aide de SQL Server Management Studio ou de PowerShell. 

### <a name="join-secondary-replicas-to-the-ag"></a>Joindre les réplicas secondaires au groupe de disponibilité

L’utilisateur Pacemaker requiert des autorisations `ALTER`, `CONTROL` et `VIEW DEFINITION` sur le groupe de disponibilité de tous les réplicas. Pour octroyer des autorisations, exécutez le script Transact-SQL suivant une fois le groupe de disponibilité créé sur le réplica principal et chaque réplica secondaire immédiatement après qu’ils aient été ajoutés au groupe de disponibilité. Avant d’exécuter le script suivant, remplacez `<pacemakerLogin>` par le nom du compte d'utilisateur Pacemaker. Si vous n’avez pas de compte de connexion pour Pacemaker, [créez un compte de connexion SQL Server pour Pacemaker](sql-server-linux-availability-group-cluster-ubuntu.md#create-a-sql-server-login-for-pacemaker).

```sql
GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::ag1 TO <pacemakerLogin>
GRANT VIEW SERVER STATE TO <pacemakerLogin>
```

Le script Transact-SQL suivant joint une instance à un groupe de disponibilité nommé `ag1`. Mettez à jour le script en fonction de votre environnement. Sur chaque instance qui héberge un réplica secondaire, exécutez le script Transact-SQL suivant pour joindre le groupe de disponibilité.

```sql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create Post](../includes/ss-linux-cluster-availability-group-create-post.md)]

>[!IMPORTANT]
>Après avoir créé le groupe de disponibilité, vous devez configurer l’intégration avec une technologie de cluster comme Pacemaker pour la haute disponibilité. Pour une configuration à l’échelle lecture à l’aide de groupes de disponibilité, à partir de [!INCLUDE [SQL Server version](../includes/sssql17-md.md)], la configuration d’un cluster n’est pas nécessaire.

Si vous avez suivi les étapes de ce document, vous disposez d’un groupe de disponibilité qui n’est pas encore en cluster. L’étape suivante consiste à ajouter le cluster. Cette configuration est valide pour les scénarios d’échelle lecture/équilibrage de charge, mais elle n’est pas complète pour la haute disponibilité. Pour une haute disponibilité, vous devez ajouter le groupe de disponibilité en tant que ressource de cluster. Pour obtenir des instructions, consultez [Étapes suivantes](#next-steps). 

## <a name="notes"></a>Notes

>[!IMPORTANT]
>Après avoir configuré le cluster et ajouté le groupe de disponibilité en tant que ressource de cluster, vous ne pouvez pas utiliser Transact-SQL pour basculer les ressources du groupe de disponibilité. Les ressources de cluster SQL Server sur Linux ne sont pas couplées aussi étroitement au système d’exploitation, car elles se trouvent sur un cluster de basculement Windows Server (WSFC). Le service SQL Server n’est pas informé de la présence du cluster. Toutes les orchestrations sont effectuées via les outils d’administration de cluster. Dans RHEL ou Ubuntu, utilisez `pcs`. Dans SLES utilisez `crm`. 

>[!IMPORTANT]
>Si le groupe de disponibilité est une ressource de cluster, il existe un problème connu dans la mise en production actuelle, où le basculement forcé avec perte de données vers un réplica asynchrone ne fonctionne pas. Ce problème sera résolu dans la prochaine mise en production. Le basculement manuel ou automatique vers un réplica synchrone a réussi.


## <a name="next-steps"></a>Étapes suivantes

[Configurer le cluster Red Hat Enterprise Linux pour les ressources de cluster du groupe de disponibilité SQL Server](sql-server-linux-availability-group-cluster-rhel.md)

[Configurer le cluster SUSE Linux Enterprise Server pour les ressources de cluster du groupe de disponibilité SQL Server](sql-server-linux-availability-group-cluster-sles.md)

[Configurer le cluster Ubuntu pour les ressources de cluster du groupe de disponibilité SQL Server](sql-server-linux-availability-group-cluster-ubuntu.md)