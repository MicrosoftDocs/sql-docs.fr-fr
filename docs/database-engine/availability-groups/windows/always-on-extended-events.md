---
title: Configurer les événements étendus pour les groupes de disponibilité
description: SQL Server définit des événements étendus qui sont spécifiques aux groupes de disponibilité Always On. Vous pouvez monitorer ces événements étendus dans une session pour mieux diagnostiquer la cause racine d’un problème lié à un groupe de disponibilité.
ms.custom: ag-guide, seodec18
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 5950f98a-3950-473d-95fd-cde3557b8fc2
author: rothja
ms.author: jroth
ms.openlocfilehash: 28eab9dae04e0268c6dd7827aab8be71220e4218
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643734"
---
# <a name="configure-extended-events-for-always-on-availability-groups"></a>Configurer les événements étendus pour les groupes de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  SQL Server définit des événements étendus qui sont spécifiques aux groupes de disponibilité Always On. Vous pouvez monitorer ces événements étendus dans une session pour mieux diagnostiquer la cause racine d’un problème lié à un groupe de disponibilité. Vous pouvez afficher les événements étendus des groupes de disponibilité à l’aide de la requête suivante :  
  
```sql  
SELECT * FROM sys.dm_xe_objects WHERE name LIKE '%hadr%'  
```  
   
##  <a name="alwayson_health-session"></a><a name="BKMK_alwayson_health"></a> Session Alwayson_health  
 La session d’événements étendus alwayson_health est créée automatiquement quand vous créez le groupe de disponibilité. Elle capture un sous-ensemble des événements liés à un groupe de disponibilité. Cette session préconfigurée constitue un outil à la fois utile et pratique pour démarrer rapidement la résolution des problèmes d’un groupe de disponibilité. L’Assistant Création d’un groupe de disponibilité démarre automatiquement la session sur chaque réplica de disponibilité participant configuré dans l’Assistant.  
  
> [!IMPORTANT]  
>  Si vous n’avez pas créé le groupe de disponibilité à l’aide de **l’Assistant Nouveau groupe de disponibilité**, la session alwayson_health peut ne pas démarrer automatiquement. Si elle n’est pas démarrée, la session ne peut pas capturer les données d’événement en cas de problème inattendu. Vous devez démarrer manuellement la session et configurer ses propriétés de manière à ce qu’elle démarre automatiquement.  
  
 Pour afficher la définition de la session alwayson_health :  
  
1.  Dans **l’Explorateur d’objets**, développez **Gestion**, **Événements étendus**, puis **Sessions**.  
  
2.  Cliquez avec le bouton droit sur **Alwayson_health**, pointez sur **Générer un script de la session en tant que**, sur **CREATE To**, puis cliquez sur **Nouvelle fenêtre d’éditeur de requête**.  

Pour plus d’informations sur certains des événements couverts par alwayson_health, consultez les [informations de référence sur les événements étendus](always-on-extended-events.md#BKMK_Reference).  


##  <a name="extended-events-for-debugging"></a><a name="BKMK_Debugging"></a>Événements étendus pour le débogage  
 Outre les événements étendus couverts par la session alwayson_health, SQL Server définit un ensemble complet d’événements de débogage pour les groupes de disponibilité. Pour exploiter ces événements étendus supplémentaires dans une session, effectuez les procédures suivantes :  
  
1.  Dans **l’Explorateur d’objets**, développez **Gestion**, **Événements étendus**, puis **Sessions**.  
  
2.  Cliquez avec le bouton droit sur **Sessions** et sélectionnez **Nouvelle session**. Vous pouvez aussi cliquer avec le bouton droit sur **Alwayson_health** et sélectionner **Propriétés**.  
  
3.  Dans le volet **Sélectionner une page**, cliquez sur **Événements**.  
  
4.  Dans la bibliothèque d’événements, dans la colonne **Catégorie**, sélectionnez **alwayson** et désactivez toutes les autres catégories.  
  
5.  Dans la colonne **Canal**, sélectionnez **Debug**. Tous les événements liés au groupe de disponibilité qui ne sont pas déjà sélectionnés sont désormais affichés dans la bibliothèque d’événements.  
  
6.  Mettez en surbrillance un événement dans la bibliothèque d’événements, puis cliquez sur le bouton **>** pour le sélectionner pour la session.  
  
7.  Quand vous avez terminé avec la session, cliquez sur **OK** pour la fermer. Vérifiez que la session est démarrée pour qu’elle puisse capturer les événements sélectionnés.  
  
##  <a name="always-on-availability-groups-extended-events-reference"></a><a name="BKMK_Reference"></a> Informations de référence sur les événements étendus des groupes de disponibilité Always On  
 Cette section décrit certains des événements étendus utilisés pour monitorer les groupes de disponibilité.  
  
 [availability_replica_state_change](#BKMK_availability_replica_state_change)  
  
 [availability_group_lease_expired](#BKMK_availability_group_lease_expired)  
  
 [availability_replica_automatic_failover_validation](#BKMK_availability_replica_automatic_failover_validation)  
  
 [error_reported (plusieurs numéros d’erreur) : pour les problèmes de connexion ou de transport](#BKMK_error_reported)  
  
 [data_movement_suspend_resume](#BKMK_data_movement_suspend_resume)  
  
 [alwayson_ddl_executed](#BKMK_alwayson_ddl_executed)  
  
 [availability_replica_manager_state](#BKMK_availability_replica_manager_state)  
  
 [error_reported (1480) : Le rôle de réplica de base de données a changé](#BKMK_error_reported_1480)  
  
###  <a name="availability_replica_state_change"></a><a name="BKMK_availability_replica_state_change"></a> availability_replica_state_change  
 Se produit en cas de changement de l’état d’un réplica de disponibilité. La création d’un groupe de disponibilité ou la participation à un réplica de disponibilité peut déclencher cet événement. Il est utile pour diagnostiquer les échecs de basculement automatique. Il peut également être utilisé pour suivre les étapes de basculement.  
  
#### <a name="event-information"></a>Informations sur l’événement  
  
|Colonne|Description|  
|------------|-----------------|  
|Nom|availability_replica_state_change|  
|Category|always on|  
|Channel|En fonctionnement|  
  
#### <a name="event-fields"></a>Champs de l’événement  
  
|Nom|Type_name|Description|  
|----------|----------------|-----------------|  
|availability_group_id|guid|ID du groupe de disponibilité.|  
|availability_group_name|unicode_string|Nom du groupe de disponibilité.|  
|availability_replica_id|guid|ID du réplica de disponibilité.|  
|previous_state|availability_replica_state|Rôle du réplica avant le changement.<br /><br /> **Valeurs possibles :**<br /><br /> Primary_Normal<br /><br /> Secondary_Normal<br /><br /> Resolving_Pending_Failover<br /><br /> Resolving_Normal<br /><br /> Primary_Pending<br /><br /> Not_Available|  
|current_state|availability_replica_state|Rôle du réplica après le changement.<br /><br /> **Valeurs possibles :**<br /><br /> Primary_Normal<br /><br /> Secondary_Normal<br /><br /> Resolving_Pending_Failover<br /><br /> Resolving_Normal<br /><br /> Primary_Pending<br /><br /> Not_Available|  
  
#### <a name="alwayson_health-session-definition"></a>Définition de la session alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT availability_replica_state_change  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_group_lease_expired"></a><a name="BKMK_availability_group_lease_expired"></a> availability_group_lease_expired  
 Se produit après l’expiration du bail en cas de problème de connectivité entre le cluster et le groupe de disponibilité. Cet événement indique que la connectivité entre le groupe de disponibilité et le cluster WSFC sous-jacent est rompue. Si le problème de connectivité se produit sur le réplica principal, l’événement peut entraîner un basculement automatique ou provoquer la mise hors connexion du groupe de disponibilité.  
  
#### <a name="event-information"></a>Informations sur l’événement  
  
|Colonne|Description|  
|------------|-----------------|  
|Nom|availability_group_lease_expired|  
|Category|always on|  
|Channel|En fonctionnement|  
  
#### <a name="event-fields"></a>Champs de l’événement  
  
|Nom|Type_name|Description|  
|----------|----------------|-----------------|  
|availability_group_id|guid|ID du groupe de disponibilité.|  
|availability_group_name|unicode_string|Nom du groupe de disponibilité.|  
  
#### <a name="alwayson_health-session-definition"></a>Définition de la session alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT availability_group_lease_expired  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_replica_automatic_failover_validation"></a><a name="BKMK_availability_replica_automatic_failover_validation"></a> availability_replica_automatic_failover_validation  
 Se produit quand le basculement automatique valide l’état de préparation d’un réplica de disponibilité comme réplica principal, et indique si le réplica de disponibilité cible est prêt à devenir le nouveau réplica principal. Par exemple, la validation du basculement retourne false quand les bases de données ne sont pas toutes synchronisées ou jointes. Cet événement est conçu pour fournir un point de défaillance pendant les basculements. Ces informations sont intéressantes pour l’administrateur de base de données, en particulier au niveau des basculements automatiques, ceux-ci constituant une opération sans assistance. L’administrateur de base de données peut passer en revue l’événement pour déterminer la raison de l’échec d’un basculement automatique.  
  
#### <a name="event-information"></a>Informations sur l’événement  
  
|Nom|Description|  
|----------|-----------------|  
|availability_replica_automatic _failover_validation||  
|Category|always on|  
|Channel|Analytiques|  
  
#### <a name="event-fields"></a>Champs de l’événement  
  
|Nom|Type_name|Description|  
|----------|----------------|-----------------|  
|availability_group_id|guid|ID du groupe de disponibilité.|  
|availability_group_name|unicode_string|Nom du groupe de disponibilité.|  
|availability_replica_id|guid|ID du réplica de disponibilité.|  
|forced_quorum|validation_result_type|Si la valeur est TRUE, le basculement automatique est invalidé sur ce réplica de disponibilité.<br /><br /> TRUE<br /><br /> FALSE|  
|joined_and_synchronized|validation_result_type|Si la valeur est FALSE, le basculement automatique est invalidé sur ce réplica de disponibilité.<br /><br /> TRUE<br /><br /> FALSE|  
|previous_primary_or_automatic_failover_target|validation_result_type|Si la valeur est FALSE, le basculement automatique est invalidé sur ce réplica de disponibilité.<br /><br /> TRUE<br /><br /> FALSE|  
  
#### <a name="alwayson_health-session-definition"></a>Définition de la session alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT availability_replica_automatic_failover_validation (  
WHERE (  
[forced_quorum]=(TRUE) OR [joined_and_synchronized]=(FALSE) OR [previous_primary_or_automatic_failover_target]=(TRUE)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
  
```  
  
###  <a name="error_reported-multiple-error-numbers-for-transport-or-connection-issues"></a><a name="BKMK_error_reported"></a> error_reported (plusieurs numéros d’erreur) : pour les problèmes de connexion ou de transport  
 Chaque événement filtré indique qu’un problème de connectivité s’est produit dans le transport ou le point de terminaison de mise en miroir de bases de données dont dépend ce groupe de disponibilité.  
  
|Colonne|Description|  
|------------|-----------------|  
|Nom|error_reported<br /><br /> numéros à filtrer : 35201, 35202, 35206, 35204, 35207, 9642, 9666, 9691, 9692, 9693, 28034, 28036, 28080, 28091, 33309|  
|Category|erreurs|  
|Channel|Admin|  
  
#### <a name="error-numbers-to-filter"></a>Numéros d’erreur à filtrer  
  
|Numéro d’erreur|Description|  
|------------------|-----------------|  
|35201|Le délai imparti pour l’établissement de la connexion au réplica de disponibilité « %ls » a été dépassé.|  
|35202|Une connexion a été établie pour le groupe de disponibilité « %ls » du réplica de disponibilité « %ls » avec l’ID [%ls] à « %ls » avec l’ID [%ls].  Ce message est fourni uniquement à titre d'information. Aucune action de l'utilisateur n'est requise.|  
|35206|Un délai de connexion s’est produit sur une connexion préalablement établie au réplica de disponibilité « %ls ».|  
|35204|La connexion entre l’instance « %ls » et « %ls » a été désactivée en raison de l’arrêt du point de terminaison.|  
|Délai d’expiration dépassé + connexion|  
|35207|Échec de la tentative de connexion pour l’ID de groupe de disponibilité « %ls » de l’ID de réplica « %ls » à l’ID de réplica « %ls » en raison de l’erreur %d. Gravité : %d, état : %d.  Gravité : %d, état : %d. (ceci peut avoir un intérêt limité pour un administrateur de base de données. Vérifier et supprimer le cas échéant.)|  
|9642|Une erreur s’est produite dans un point de terminaison de la connexion de transport Service Broker/Mise en miroir de bases de données. Erreur : %i, état : %i. (rôle de point de terminaison à proximité : %S_MSG, adresse du point de terminaison lointain : « %.*hs »). Erreur : %i, état : %i. (rôle de point de terminaison à proximité : %S_MSG, adresse du point de terminaison lointain : « %.\*hs »)|  
|9666|Le point de terminaison %S_MSG est désactivé ou arrêté.|  
|9691|Le point de terminaison %S_MSG a cessé d’écouter les connexions.|  
|9692|Le point de terminaison %S_MSG ne peut pas écouter le port %d, car il est utilisé par un autre processus.|  
|9693|Le point de terminaison %S_MSG ne peut pas écouter les connexions en raison de l’erreur suivante : « %.*ls ».|  
|28034|Échec de la négociation de connexion. L'utilisateur '%.*ls' ne dispose pas de l'autorisation CONNECT au point de terminaison. État %d.|  
|28036|Échec de la négociation de connexion. Le certificat utilisé par ce point de terminaison est introuvable : %S_MSG. Utilisez DBCC CHECKDB dans la base de données master pour vérifier l'intégrité des métadonnées du point de terminaison. État %d.|  
|28080|Échec de la négociation de connexion. Le point de terminaison %S_MSG n'est pas configuré. État %d.|  
|28091|Le démarrage du point de terminaison pour %S_MSG sans authentification n’est pas pris en charge.|  
|33309|Impossible de démarrer le point de terminaison de cluster, car la configuration du point de terminaison %S_MSG par défaut n’a pas encore été chargée.|  
  
#### <a name="alwayson_health-session-definition"></a>Définition de la session alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT sqlserver.error_reported(  
    WHERE   
(  
--Connectivity Error Messages  
[error_number]=(35201)   
OR [error_number]=(35202)   
OR [error_number]=(35204)   
OR [error_number]=(35206)   
OR [error_number]=(35207)   
OR [error_number]=(9642)   
--OR [error_number]=(9666)   
OR [error_number]=(9691)   
OR [error_number]=(9692)   
OR [error_number]=(9693)   
OR [error_number]=(28034)   
OR [error_number]=(28036)   
OR [error_number]=(28080)   
OR [error_number]=(28091)   
OR [error_number]=(33309)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="data_movement_suspend_resume"></a><a name="BKMK_data_movement_suspend_resume"></a> data_movement_suspend_resume  
 Se produit quand le déplacement de base de données d’un réplica de base de données est suspendu ou repris.  
  
#### <a name="event-information"></a>Informations sur l’événement  
  
|Colonne|Description|  
|------------|-----------------|  
|Nom|data_movement_suspend_resume|  
|Category|Toujours active|  
|Channel|En fonctionnement|  
  
#### <a name="event-fields"></a>Champs de l’événement  
  
|Nom|Type_name|Description|  
|-|-|-|  
|availability_group_id|guid|ID du groupe de disponibilité.|  
|availability_group_name|unicode_string|Nom du groupe de disponibilité (le cas échéant).|  
|availability_replica_id|guid|ID du réplica de disponibilité.|  
|database_replica_id|guid|ID de la base de données de disponibilité.|  
|database_replica_name|unicode_string|Nom de la base de données de disponibilité.|  
|database_id|uint32|ID de la base de données de disponibilité.|  
|suspend_status|suspend_status_type|Valeurs d’état de suspension.<br /><br /> SUSPEND_NULL<br /><br /> RESUMED<br /><br /> SUSPENDED<br /><br /> SUSPENDED_INVALID|  
|suspend_source|suspend_source_type|Source de l’action de suspension ou de reprise.|  
|suspend_reason|unicode_string|Raison de la suspension capturée dans le gestionnaire de réplicas de base de données.|  
  
#### <a name="alwayson_health-session-definition"></a>Définition de la session alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT data_movement_suspend_resume (  
WHERE (  
[suspend_status]=(SUSPENDED)or [suspend_status]=(SUSPENDED_INVALID) or   
[suspend_status]=(SUSPEND_NULL)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="alwayson_ddl_executed"></a><a name="BKMK_alwayson_ddl_executed"></a> alwayson_ddl_executed  
 Se produit quand une instruction DDL (Data Definition Language) de groupe de disponibilité est en cours d’exécution, notamment CREATE, ALTER ou DROP. L’objectif principal de l’événement est d’indiquer un problème avec une action de l’utilisateur sur un réplica de disponibilité ou d’indiquer le point de départ d’une action opérationnelle, celle-ci étant suivie d’un problème de runtime (basculement manuel, basculement forcé, déplacement de données suspendu, déplacement de données repris, etc.).  
  
#### <a name="event-information"></a>Informations sur l’événement  
  
|Colonne|Description|  
|------------|-----------------|  
|Nom|alwayson_ddl_execution|  
|Category|always on|  
|Channel|Analytiques|  
  
#### <a name="event-fields"></a>Champs de l’événement  
  
|Nom|Type_name|Description|  
|----------|----------------|-----------------|  
|availability_group_id|Guid|ID du groupe de disponibilité.|  
|availability_group_name|unicode_string|Nom du groupe de disponibilité.|  
|ddl_action|alwayson_ddl_action|Indique le type d’action DDL : CREATE, ALTER ou DROP.|  
|ddl_phase|ddl_opcode|Indique la phase de l’opération DDL : BEGIN, COMMIT ou ROLLBACK.|  
|.|unicode_string|Texte de l’instruction exécutée.|  
  
#### <a name="alwayson_health-session-definition"></a>Définition de la session alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT alwayson_ddl_executed  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_replica_manager_state"></a><a name="BKMK_availability_replica_manager_state"></a> availability_replica_manager_state  
 Se produit en cas de changement de l’état du gestionnaire de réplicas de disponibilité. Cet événement indique la pulsation du gestionnaire de réplicas de disponibilité. Quand le gestionnaire de réplicas de disponibilité n’est pas dans un état sain, tous les réplicas de disponibilité dans l’instance SQL Server sont hors service.  
  
#### <a name="event-information"></a>Informations sur l’événement  
  
|Colonne|Description|  
|------------|-----------------|  
|Nom|availability_replica_manager_state_change|  
|Category|always on|  
|Channel|En fonctionnement|  
  
#### <a name="event-fields"></a>Champs de l’événement  
  
|Nom|Type_name|Description|  
|----------|----------------|-----------------|  
|current_state|manager_state|État actuel du gestionnaire de réplicas de disponibilité.<br /><br /> En ligne<br /><br /> Hors connexion<br /><br /> WaitingForClusterCommunication|  
  
#### <a name="alwayson_health-session-definition"></a>Définition de la session alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT availability_replica_manager_state (  
WHERE ([current_state]=(OFFLINE))  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="error_reported-1480-database-replica-role-change"></a><a name="BKMK_error_reported_1480"></a> error_reported (1480) : Le rôle de réplica de base de données a changé  
 Cet événement error_reported filtré se produit de façon asynchrone après un changement de rôle de réplica de disponibilité. Il indique la base de données de disponibilité qui ne parvient pas à changer son rôle attendu durant le processus de basculement.  
  
#### <a name="event-information"></a>Informations sur l’événement  
  
|Colonne|Description|  
|------------|-----------------|  
|Nom|error_reported<br /><br /> Numéro d’erreur 1480 : La base de données REPLICATION_TYPE_MSG « DATABASE_NAME » change les rôles de « OLD_ROLE » en « NEW_ROLE » en raison de REASON_MSG|  
|Category|erreurs|  
|Channel|Admin|  
  
#### <a name="alwayson_health-session-definition"></a>Définition de la session alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT sqlserver.error_reported(  
    WHERE   
(  
--database replica role change message  
OR [error_number] = (1480)  
  
--database replica runtime error messages  
OR [error_number]=(823)   
OR [error_number]=(824)   
OR [error_number]=(829)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Afficher des données de session d’événements](https://msdn.microsoft.com/library/hh710068(v=sql.110).aspx)   
