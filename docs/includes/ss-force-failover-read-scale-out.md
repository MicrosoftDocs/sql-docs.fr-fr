---
title: Forcer le basculement SQL Server pour des groupes de disponibilité
description: Forcer le basculement pour des groupes de disponibilité avec le type de cluster AUCUN
services: ''
author: MikeRayMSFT
ms.topic: include
ms.date: 02/05/2018
ms.author: mikeray
ms.custom: include file
ms.openlocfilehash: fa97b4a86fd4275b12f784ce40d6d03139e3d52e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99214934"
---
Chaque groupe de disponibilité contient un seul réplica principal. Le réplica principal autorise les opérations de lecture et d’écriture. Pour changer de réplica principal, vous pouvez effectuer un basculement. Dans un groupe de disponibilité standard, le gestionnaire de cluster automatise le processus de basculement. Dans un groupe de disponibilité avec le type de cluster AUCUN, le processus de basculement est manuel.

Il existe deux façons de basculer le réplica principal dans un groupe de disponibilité avec le type de cluster AUCUN :

- Basculement manuel sans perte de données
- Basculement manuel forcé avec perte de données


### <a name="manual-failover-without-data-loss"></a>Basculement manuel sans perte de données

Utilisez cette méthode quand le réplica principal est disponible, mais que vous devez temporairement ou définitivement changer l’instance qui héberge le réplica principal.
Pour éviter toute perte de données, avant d’effectuer le basculement manuel, vérifiez que le réplica secondaire cible est à jour.

Pour effectuer un basculement manuel sans perte de données :

1. Faites en sorte que le réplica principal actuel et le réplica secondaire cible soient `SYNCHRONOUS_COMMIT`.

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        MODIFY REPLICA ON N'<node2>' 
        WITH (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
   ```

1. Pour vérifier que les transactions actives sont validées sur le réplica principal et sur au moins un réplica secondaire synchrone, exécutez la requête suivante :

   ```SQL
   SELECT ag.name, 
      drs.database_id, 
      drs.group_id, 
      drs.replica_id, 
      drs.synchronization_state_desc, 
      ag.sequence_number
   FROM sys.dm_hadr_database_replica_states drs, sys.availability_groups ag
   WHERE drs.group_id = ag.group_id; 
   ```

   Le réplica secondaire est synchronisé quand `synchronization_state_desc` a pour valeur `SYNCHRONIZED`.

1. Affectez la valeur 1 à `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT`.

   Le script suivant définit `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` sur 1 sur un groupe de disponibilité nommé `ag1`. Avant d’exécuter le script suivant, remplacez `ag1` par le nom de votre groupe de disponibilité :

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        SET (REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 1);
   ```

   Ce paramétrage garantit que chaque transaction active est validée sur le réplica principal et sur au moins un réplica secondaire synchrone.
   >[!NOTE]
   >Ce paramètre n’est pas propre au basculement et doit être défini en fonction des exigences de l’environnement.

1. Définissez le réplica principal hors connexion pour préparer le changement de rôle : 

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] OFFLINE
   ```

1. Promouvez le réplica secondaire en réplica principal.

   ```SQL
   ALTER AVAILABILITY GROUP AGRScale FORCE_FAILOVER_ALLOW_DATA_LOSS; 
   ```

1. Changez le rôle de l’ancien réplica principal en `SECONDARY` et exécutez la commande suivante sur l’instance de SQL Server qui héberge l’ancien réplica principal :

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        SET (ROLE = SECONDARY); 
   ```

   > [!NOTE]
   > Pour supprimer un groupe de disponibilité, utilisez [DROP AVAILABILITY GROUP](../t-sql/statements/drop-availability-group-transact-sql.md). Pour un groupe de disponibilité créé avec le type de cluster NONE ou EXTERNAL, exécutez la commande sur tous les réplicas faisant partie du groupe de disponibilité.

1. Pour reprendre le déplacement des données, exécutez la commande suivante pour chaque base de données du groupe de disponibilité sur l’instance de SQL Server qui héberge le réplica principal :

   ```SQL
   ALTER DATABASE [db1]
        SET HADR RESUME
   ```

1. Recréez l’écouteur que vous avez créé à des fins d’échelle lecture et qui n’est pas géré par un gestionnaire de cluster. Si l’écouteur d’origine pointe vers l’ancien réplica principal, supprimez-le et recréez-le pour qu’il pointe vers le nouveau réplica principal.

### <a name="forced-manual-failover-with-data-loss"></a>Basculement manuel forcé avec perte de données

Si le réplica principal n’est pas disponible et ne peut pas être récupéré immédiatement, vous devez forcer un basculement vers le réplica secondaire avec perte de données. Cependant, si le réplica principal d’origine récupère après le basculement, il va assumer le rôle principal. Pour éviter que chaque réplica soit dans un état différent, supprimez le réplica principal d’origine du groupe de disponibilité après un basculement forcé avec perte de données. Une fois que le serveur principal d’origine revient en ligne, supprimez-y entièrement le groupe de disponibilité. 

Pour forcer un basculement manuel avec perte de données du réplica principal N1 vers le réplica secondaire N2, effectuez les étapes suivantes : 

1. Sur le réplica secondaire (N2), lancez le basculement forcé : 

    ```SQL
    ALTER AVAILABILITY GROUP [AGRScale] FORCE_FAILOVER_ALLOW_DATA_LOSS;
    ```
    
1. Sur le nouveau réplica principal (N2), supprimez le réplica principal d’origine (N1) : 

    ```SQL
    ALTER AVAILABILITY GROUP [AGRScale]
    REMOVE REPLICA ON N'N1';
    ```
    
1. Vérifiez que tout le trafic d’application pointe vers l’écouteur et/ou le nouveau réplica principal. 
1. Si le réplica principal d’origine (N1) est mis en ligne, placez immédiatement le groupe de disponibilité AGRScale hors connexion sur le réplica principal d’origine (N1) :

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] OFFLINE
   ```
1. S’il existe des données ou des modifications non synchronisées, conservez ces données via des sauvegardes ou d’autres options de réplication des données qui répondent aux besoins de votre entreprise.     
1. Ensuite, supprimez le groupe de disponibilité du réplica principal d’origine (N1) :

    ```SQL
    DROP AVAILABILITY GROUP [AGRScale];
    ```
1. Supprimez la base de données du groupe de disponibilité sur le réplica principal d’origine (N1) : 

    ```SQL
    USE [master]
    GO
    DROP DATABASE [AGDBRScale]
    GO
    ```
    
 1. (Facultatif) Si vous le souhaitez, vous pouvez maintenant ajouter N1 comme nouveau réplica secondaire au groupe de disponibilité AGRScale.
