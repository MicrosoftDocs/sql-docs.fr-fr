---
description: CREATE EVENT NOTIFICATION (Transact-SQL)
title: CREATE EVENT NOTIFICATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_EVENT_NOTIFICATION_TSQL
- NOTIFICATION_TSQL
- EVENT
- NOTIFICATION
- CREATE EVENT NOTIFICATION
- EVENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EVENT NOTIFICATION statement
- events [SQL Server], notifications
- event notifications [SQL Server], creating
ms.assetid: dbbff0e8-9e25-4f12-a1ba-e12221d16ac2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6f1e3aa71a31dfff7bd7ec83d367efdaa8a01967
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100860"
---
# <a name="create-event-notification-transact-sql"></a>CREATE EVENT NOTIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée un objet qui envoie des informations sur un événement de base de données ou de serveur à un service Service Broker. Les notifications d'événements sont créées uniquement au moyen d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE EVENT NOTIFICATION event_notification_name   
ON { SERVER | DATABASE | QUEUE queue_name }   
[ WITH FAN_IN ]  
FOR { event_type | event_group } [ ,...n ]  
TO SERVICE 'broker_service' , { 'broker_instance_specifier' | 'current database' }  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *event_notification_name*  
 Nom de la notification d’événement. Un nom de notification d’événements doit être conforme aux règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md) et être unique dans l’étendue où il est créé : SERVER, DATABASE ou *object_name*.  
  
 SERVER  
 Applique l'étendue de la notification d'événement à l'instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si SERVER est spécifié, la notification se déclenche lorsque l'événement spécifié dans la clause FOR se produit n'importe où dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
 DATABASE  
 Applique l'étendue de la notification d'événement à la base de données actuelle. Si DATABASE est spécifié, la notification se déclenche lorsque l'événement spécifié dans la clause FOR se produit n'importe où dans la base de données actuelle.  
  
 QUEUE  
 Applique l'étendue de la notification à une file d'attente spécifique dans la base de données actuelle. QUEUE peut être spécifié seulement si FOR QUEUE_ACTIVATION ou FOR BROKER_QUEUE_DISABLED est également spécifié.  
  
 *queue_name*  
 Nom de la file d'attente à laquelle la notification d'événement s'applique. *queue_name* peut être spécifié seulement si QUEUE est défini.  
  
 WITH FAN_IN  
 Indique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'envoyer un seul message par événement au service spécifié pour toutes les notifications d'événement qui :  
  
-   sont créées pour le même événement ;  
  
-   sont créées par le même principal (identifié par le même SID).  
  
-   Spécifient le même service et *broker_instance_specifier*.  
  
-   spécifient WITH FAN_IN.  
  
 Par exemple, trois notifications d'événements sont créées. Toutes les notifications d'événements spécifient FOR ALTER_TABLE, WITH FAN_IN, la même clause TO SERVICE, et sont créées par le même identificateur de sécurité (SID). Lorsqu'une instruction ALTER TABLE est exécutée, les messages créés par ces trois notifications d'événements sont fusionnés en un message. Par conséquent, le service cible reçoit uniquement un message de l'événement.  
  
 *event_type*  
 Nom d'un type d'événement qui provoque l'exécution de la notification d'événement. *event_type* peut être un événement de type DDL [!INCLUDE[tsql](../../includes/tsql-md.md)], Trace SQL ou [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Pour obtenir une liste des types d’événements [!INCLUDE[tsql](../../includes/tsql-md.md)] DDL éligibles, consultez [Événements DDL](../../relational-databases/triggers/ddl-events.md). Les types d'événements [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont QUEUE_ACTIVATION et BROKER_QUEUE_DISABLED. Pour plus d'informations, voir [Event Notifications](../../relational-databases/service-broker/event-notifications.md).  
  
 *event_group*  
 Nom d'un groupe prédéfini d'événements de type [!INCLUDE[tsql](../../includes/tsql-md.md)] ou SQL Trace. Une notification d'événement peut se déclencher après l'exécution d'un événement qui appartient à un groupe d'événements. Pour obtenir une liste des groupes d’événements DDL, des événements [!INCLUDE[tsql](../../includes/tsql-md.md)] qu’ils couvrent et l’étendue à laquelle ils peuvent être définis, consultez [Groupes d’événements DDL](../../relational-databases/triggers/ddl-event-groups.md).  
  
 *event_group* agit également comme une macro, quand l’exécution de l’instruction CREATE EVENT NOTIFICATION se termine, en ajoutant les types d’événements qu’il couvre à la vue de catalogue **sys.events**.  
  
 **'** *broker_service* **'**  
 Spécifie le service cible qui reçoit les données de l'instance d'événement. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ouvre une ou plusieurs conversations avec le service cible de la notification d'événement. Ce service doit respecter le type de message d'événement et de contrat [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé pour envoyer le message.  
  
 Les conversations restent ouvertes jusqu'à ce que la notification d'événement soit supprimée. Certaines erreurs peuvent mettre fin aux conversations de manière anticipée. Le fait de mettre fin explicitement à certaines conversations ou à toutes les conversations peut empêcher le service cible de recevoir davantage de messages.  
  
 { **'** _broker\_instance\_specifier_ **'**  |  **'current database'** }  
 Spécifie une instance de Service Broker par rapport à laquelle *broker_service* est résolu. La valeur d’un Service Broker spécifique peut être obtenue en interrogeant la colonne **service_broker_guid** de la vue de catalogue **sys.databases**. Utilisez **'current database'** pour définir l’instance de Service Broker dans la base de données actuelle. **'current database'** est un littéral de chaîne qui tient compte de la casse.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
## <a name="remarks"></a>Notes  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] intègre un type de message et un contrat spécialement conçus pour les notifications d'événements. Par conséquent, il n'est pas nécessaire de créer un service d'initialisation Service Broker car il en existe déjà un qui spécifie le nom de contrat suivant :`https://schemas.microsoft.com/SQL/Notifications/PostEventNotification`  
  
 Le service cible qui reçoit les notifications d'événements doit respecter ce contrat préexistant.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssSB](../../includes/sssb-md.md)] doit être configurée pour les notifications d'événements qui envoient des messages à un Service Broker résidant sur un serveur distant. La sécurité du dialogue doit être configurée manuellement conformément au modèle de sécurité totale. Pour plus d’informations, consultez [Configurer la sécurité du dialogue pour les notifications d’événements](../../relational-databases/service-broker/configure-dialog-security-for-event-notifications.md).  
  
 Si une transaction d'événement qui active une notification est annulée, l'envoi de la notification d'événement est également annulé. Les notifications d'événements ne se déclenchent pas par une action définie dans un déclencheur lorsque la transaction est validée ou annulée à l'intérieur du déclencheur. Les événements de trace n'étant pas limités par les transactions, les notifications d'événements basées sur des événements de trace sont envoyées, que la transaction qui les active soit annulée ou non.  
  
 Si la conversation entre le serveur et le service cible est interrompue après le déclenchement d'une notification d'événement, une erreur est signalée et la notification d'événement est supprimée.  
  
 La transaction d'événement qui a démarré la notification n'est pas affectée par le succès ou l'échec de l'envoi de la notification d'événement.  
  
 Les échecs d'envoi de notification d'événement sont consignés.  
  
## <a name="permissions"></a>Autorisations  
 Pour créer une notification d'événement dont l'étendue correspond à la base de données (ON DATABASE), vous devez disposer de l'autorisation CREATE DATABASE DDL EVENT NOTIFICATION sur la base de données.  
  
 Pour créer une notification d'événement sur une instruction DDL dont l'étendue correspond au serveur (ON SERVER), vous devez disposer de l'autorisation CREATE DDL EVENT NOTIFICATION sur le serveur.  
  
 Pour créer une notification d'événement sur un événement de trace, vous devez disposer de l'autorisation CREATE TRACE EVENT NOTIFICATION sur le serveur.  
  
 Pour créer une notification d'événement dont l'étendue correspond à une file d'attente, vous devez disposer de l'autorisation ALTER sur la file d'attente.  
  
## <a name="examples"></a>Exemples  
  
> [!NOTE]  
>  Dans les exemples A et B ci-dessous, le GUID dans la clause `TO SERVICE 'NotifyService'` ('8140a771-3c4b-4479-8ac0-81008ab17984') est spécifique à l'ordinateur sur lequel l'exemple a été installé. Pour cette instance, il s'agit du GUID de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
>   
>  Pour copier et exécuter ces exemples, vous devez remplacer ce GUID par un GUID de votre ordinateur et de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Comme expliqué dans la section Arguments ci-dessus, vous pouvez acquérir **'** _broker\_instance\_specifier_ **'** en interrogeant la colonne service_broker_guid de la vue de catalogue sys.databases.  
  
### <a name="a-creating-an-event-notification-that-is-server-scoped"></a>R. Création d'une notification d'événement dont l'étendue correspond au serveur  
 L'exemple suivant crée les objets nécessaires pour configurer un service cible à l'aide de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Le service cible fait référence au type de message et de contrat du service à l'origine de l'initialisation spécifique aux notifications d'événements La notification d'événement est ensuite créée sur le service cible qui envoie une notification chaque fois qu'un événement de trace `Object_Created` se produit sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
--Create a queue to receive messages.  
CREATE QUEUE NotifyQueue ;  
GO  

--Create a service on the queue that references  
--the event notifications contract.  
CREATE SERVICE NotifyService  
ON QUEUE NotifyQueue  
([https://schemas.microsoft.com/SQL/Notifications/PostEventNotification]);  
GO  

--Create a route on the service to define the address   
--to which Service Broker sends messages for the service.  
CREATE ROUTE NotifyRoute  
WITH SERVICE_NAME = 'NotifyService',  
ADDRESS = 'LOCAL';  
GO 

--Create the event notification.  
CREATE EVENT NOTIFICATION log_ddl1   
ON SERVER   
FOR Object_Created   
TO SERVICE 'NotifyService',  
    '8140a771-3c4b-4479-8ac0-81008ab17984' ;  
```  
  
### <a name="b-creating-an-event-notification-that-is-database-scoped"></a>B. Création d'une notification d'événement dont l'étendue correspond à une base de données  
 L'exemple suivant crée une notification d'événement sur le même service cible que l'exemple précédent. La notification d'événement se déclenche après l'occurrence d'un événement `ALTER_TABLE` dans l'exemple de base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
CREATE EVENT NOTIFICATION Notify_ALTER_T1  
ON DATABASE  
FOR ALTER_TABLE  
TO SERVICE 'NotifyService',  
    '8140a771-3c4b-4479-8ac0-81008ab17984';  
```  
  
### <a name="c-getting-information-about-an-event-notification-that-is-server-scoped"></a>C. Obtention d'informations sur une notification d'événement dont l'étendue correspond à un serveur  
 L'exemple suivant interroge l'affichage catalogue `sys.server_event_notifications` pour obtenir des métadonnées sur la notification d'événement `log_ddl1` créée avec une étendue de serveur.  
  
```sql  
SELECT * FROM sys.server_event_notifications  
WHERE name = 'log_ddl1';  
```  
  
### <a name="d-getting-information-about-an-event-notification-that-is-database-scoped"></a>D. Obtention d'informations sur une notification d'événement dont l'étendue correspond à une base de données  
 L'exemple suivant interroge l'affichage catalogue `sys.event_notifications` pour obtenir des métadonnées sur la notification d'événement `Notify_ALTER_T1` créée avec une étendue de base de données.  
  
```sql  
SELECT * FROM sys.event_notifications  
WHERE name = 'Notify_ALTER_T1';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Notifications d'événements](../../relational-databases/service-broker/event-notifications.md)   
 [DROP EVENT NOTIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-notification-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.event_notifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-event-notifications-transact-sql.md)   
 [sys.server_event_notifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-notifications-transact-sql.md)   
 [sys.events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-events-transact-sql.md)   
 [sys.server_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-events-transact-sql.md)  
  
  
