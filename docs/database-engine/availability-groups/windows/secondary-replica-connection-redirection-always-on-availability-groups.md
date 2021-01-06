---
title: Rediriger les connexions en lecture/écriture vers le réplica principal
description: Découvrez comment rediriger les connexions en lecture/écriture vers le réplica principal d’un groupe de disponibilité Always On, quel que soit le serveur spécifié dans la chaîne de connexion.
ms.custom: seo-lt-2019
ms.date: 01/09/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: article
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- readable secondary replicas
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 8fb07363cd52de514c646cff3d482b38f76493b3
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643310"
---
# <a name="secondary-to-primary-replica-readwrite-connection-redirection-always-on-availability-groups"></a>Redirection de connexion en lecture/écriture depuis un réplica secondaire vers le réplica principal (groupes de disponibilité Always On)

[!INCLUDE[appliesto](../../../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE[sssqlv15-md](../../../includes/sssqlv15-md.md)] CTP 2.0 permet désormais la *redirection de connexion en lecture/écriture depuis un réplica secondaire vers le réplica principal* pour les groupes de disponibilité AlwaysOn. La redirection de connexion en lecture/écriture est disponible pour toutes les plateformes de système d’exploitation. Elle permet de rediriger les connexions d’applications clientes vers le réplica principal, quel que soit le serveur cible spécifié dans la chaîne de connexion. 

Par exemple, la chaîne de connexion peut cibler un réplica secondaire. Selon la configuration du réplica de groupe de disponibilité et les paramètres de la chaîne de connexion, la connexion peut être automatiquement redirigée vers le réplica principal. 

## <a name="use-cases"></a>Cas d'utilisation

Dans les versions antérieures à la [!INCLUDE[sssqlv15-md](../../../includes/sssqlv15-md.md)], l’écouteur du groupe de disponibilité et la ressource de cluster correspondante redirigent le trafic utilisateur vers le réplica principal afin de garantir la reconnexion après un basculement. La [!INCLUDE[sssqlv15-md](../../../includes/sssqlv15-md.md)] continue de prendre en charge la fonctionnalité d’écouteur du groupe de disponibilité, et permet de rediriger la connexion de réplica pour les scénarios qui ne peuvent pas inclure d’écouteur. Par exemple :

* La technologie de cluster qui est intégrée aux groupes de disponibilité SQL Server ne propose pas d’écouteur. 
* Une configuration à plusieurs sous-réseaux comme dans le cloud, ou une adresse IP flottante à plusieurs sous-réseaux avec Pacemaker où les configurations sont complexes, sujettes aux erreurs et difficiles à corriger en raison des nombreux composants impliqués
* L’échelle horizontale en lecture, la récupération d’urgence ou le type de cluster ont la valeur `NONE`, car il n’existe aucun mécanisme simple permettant de garantir la reconnexion transparente après un basculement manuel

## <a name="requirement"></a>Condition requise

Pour qu’un réplica secondaire redirige les requêtes de connexion en lecture/écriture :
* Le réplica secondaire doit être en ligne. 
* Les spécifications `PRIMARY_ROLE` du réplica doivent inclure `READ_WRITE_ROUTING_URL`.
* La chaîne de connexion doit être `ReadWrite`. Pour cela, `ApplicationIntent` doit être défini sur `ReadWrite` ou `ApplicationIntent` ne doit pas être défini et la valeur par défaut (`ReadWrite`) doit s’appliquer.

## <a name="set-read_write_routing_url-option"></a>Définir l’option READ_WRITE_ROUTING_URL

Pour configurer la redirection des connexions en lecture/écriture, définissez `READ_WRITE_ROUTING_URL` pour le réplica principal lorsque vous créez le groupe de disponibilité. 

Dans [!INCLUDE[sssqlv15-md](../../../includes/sssqlv15-md.md)], `READ_WRITE_ROUTING_URL` a été ajouté aux spécifications `<add_replica_option>`. Consultez les rubriques suivantes : 

* [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)
* [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)


### <a name="primary_roleread_write_routing_url-not-set-default"></a>PRIMARY_ROLE(READ_WRITE_ROUTING_URL) non défini (par défaut) 

Par défaut, la redirection des connexions de réplica en lecture/écriture n’est pas définie pour un réplica. La façon dont un réplica secondaire gère les requêtes de connexion varie selon qu’il est configuré pour autoriser les connexions, et selon la valeur du paramètre `ApplicationIntent` de la chaîne de connexion. Le tableau suivant montre comment un réplica secondaire gère les connexions en fonction de `SECONDARY_ROLE (ALLOW CONNECTIONS = )` et de `ApplicationIntent`.

|Valeur <code>ApplicationIntent</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = NO)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = READ_ONLY)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)</code>|
|-----|-----|-----|-----|
|`ApplicationIntent=ReadWrite`<br/> Default|Échec des connexions|Échec des connexions|Réussite des connexions<br/>Réussite des lectures<br/>Échec des écritures|
|`ApplicationIntent=ReadOnly`|Échec des connexions|Réussite des connexions|Réussite des connexions

Le tableau précédant montre le comportement par défaut, qui est identique à celui des versions de SQL Server antérieures à la [!INCLUDE[sssqlv15-md](../../../includes/sssqlv15-md.md)]. 

### <a name="primary_roleread_write_routing_url-set"></a>PRIMARY_ROLE(READ_WRITE_ROUTING_URL) défini 

Une fois la redirection des connexions en lecture/écriture définie, le réplica gère les requêtes de connexion différemment. Le comportement de connexion dépend toujours de `SECONDARY_ROLE (ALLOW CONNECTIONS = )` et de `ApplicationIntent`. Le tableau suivant montre comment un réplica secondaire, où `READ_WRITE_ROUTING` est défini, gère les connexions en fonction de `SECONDARY_ROLE (ALLOW CONNECTIONS = )` et de `ApplicationIntent`.

|Valeur <code>ApplicationIntent</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = NO)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = READ_ONLY)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)</code>|
|-----|-----|-----|-----|
|`ApplicationIntent=ReadWrite`<br/>Default|Échec des connexions|Échec des connexions|Connexions acheminées vers le réplica principal|
|`ApplicationIntent=ReadOnly`|Échec des connexions|Réussite des connexions|Réussite des connexions

Le tableau précédent montre que lorsque `READ_WRITE_ROUTING_URL` est défini dans le réplica principal, le réplica secondaire redirige les connexions vers le réplica principal lorsque `SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)`, et que la connexion spécifie `ReadWrite`.

## <a name="example"></a>Exemple 

Dans cet exemple, un groupe de disponibilité a trois réplicas :
* Un réplica principal sur COMPUTER01
* Un réplica secondaire synchrone sur COMPUTER02
* Un réplica secondaire asynchrone sur COMPUTER03

L’illustration suivante représente le groupe de disponibilité.

![Groupe de disponibilité avec réplicas principal, secondaire et secondaire asynchrone](media/replica-connection-redirection-always-on-availability-groups/01_originalAG.png)

Le script Transact-SQL suivant crée ce groupe de disponibilité. Dans cet exemple, chaque réplica spécifie `READ_WRITE_ROUTING_URL`.
```sql
CREATE AVAILABILITY GROUP MyAg   
     WITH ( CLUSTER_TYPE =  NONE )  
   FOR   
     DATABASE  <Database1>   
   REPLICA ON   
      'COMPUTER01' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER01.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL,  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER01.<domain>.<tld>:1433' ),
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER02, COMPUTER03),
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER01.<domain>.<tld>:1433' )   
         SESSION_TIMEOUT = 10  
         ),   
      'COMPUTER02' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER02.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL, 
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER02.<domain>.<tld>:1433' ),  
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER01, COMPUTER03),  
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER02.<domain>.<tld>:1433' )   
         SESSION_TIMEOUT = 10  
         ),   
      'COMPUTER03' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER03.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL,  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER03.<domain>.<tld>:1433' ),  
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER01, COMPUTER02),  
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER03.<domain>.<tld>:1433' )  
         SESSION_TIMEOUT = 10  
         );
GO  
```
   - `<domain>.<tld>`
      - Domaine et domaine de niveau supérieur du nom de domaine complet. Par exemple : `corporation.com`.


### <a name="connection-behaviors"></a>Comportements de connexion

Dans le diagramme suivant, une application cliente se connecte à COMPUTER02, avec `ApplicationIntent=ReadWrite`. La connexion est redirigée vers le réplica principal. 

![La connexion à l’ordinateur 2 est redirigée vers le réplica principal](media/replica-connection-redirection-always-on-availability-groups/02_redirectionAG.png)

Le réplica secondaire redirige les appels de lecture/écriture vers le réplica principal. Une connexion en lecture/écriture vers l’un des réplicas sera redirigée vers le réplica principal. 

Dans le diagramme suivant, le réplica principal a fait l’objet d’un basculement manuel vers COMPUTER02. Une application cliente se connecte à COMPUTER01, avec `ApplicationIntent=ReadWrite`. La connexion est redirigée vers le réplica principal. 

![Connexion redirigée vers le nouveau réplica principal sur l’ordinateur 2](media/replica-connection-redirection-always-on-availability-groups/03_redirectionAG.png)

## <a name="see-also"></a>Voir aussi

[Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 
[À propos de l’accès de la connexion client aux réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   

[Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)
