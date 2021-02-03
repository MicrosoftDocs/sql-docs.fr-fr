---
description: Broker:Conversation, classe d'événements
title: Broker:Conversation, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Broker:Conversation event class
ms.assetid: 784707b5-cc67-46a3-8ae6-8f8ecf4b27c0
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 32e74cf95915559a1d8d5e751b79f096d4bb59ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187459"
---
# <a name="brokerconversation-event-class"></a>Broker:Conversation, classe d'événements

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement **Broker:Conversation** pour indiquer la progression d'une conversation Service Broker.  
  
## <a name="brokerconversation-event-class-data-columns"></a>Colonnes de données de la classe d'événements Broker:Conversation  
  
|Colonne de données|Type|Description|Numéro de colonne|Filtrable|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne contient des valeurs transmises par l'application à la place du nom affiché du programme.|10|Oui|  
|**ClientProcessID**|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. Cette colonne de données est remplie si l'ID du processus du client est fourni par le client.|9|Oui|  
|**DatabaseID**|**int**|ID de la base de données spécifiée par l'instruction USE *base de données* . Si aucune instruction USE *base de données* n’a été spécifiée, il s’agit de l’ID de la base de données par défaut. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données **ServerName** du serveur est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l’aide de la fonction **DB_ID** .|3|Oui|  
|**EventClass**|**int**|Type de classe d'événements capturée. La valeur est toujours **124** pour **Broker:Conversation**.|27|Non|  
|**EventSequence**|**int**|Numéro de séquence de cet événement.|51|Non|  
|**EventSubClass**|**nvarchar**|Type de sous-classe d'événements. Cela fournit plus d'informations sur chaque classe d'événements.|21|Oui|  
|**GUID**|**uniqueidentifier**|ID de conversation du dialogue. Cet identifiant est transmis en tant que partie intégrante du message et est partagé par les deux intervenants de la conversation.|54|Non|  
|**HostName**|**nvarchar**|Nom de l'ordinateur sur lequel s'exécute le client. Cette colonne de données est remplie si le nom de l'hôte est fourni par le client. Pour déterminer le nom de l’hôte, utilisez la fonction **HOST_NAME** .|8|Oui|  
|**IsSystem**|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur.<br /><br /> 0 = utilisateur<br /><br /> 1 = système|60|Non|  
|**LoginSid**|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|**MethodName**|**nvarchar**|Groupe de conversations auquel la conversation appartient.|47|Non|  
|**NTDomainName**|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|**NTUserName**|**nvarchar**|Nom de l'utilisateur propriétaire de la connexion ayant généré l'événement.|6|Oui|  
|**ObjectName**|**nvarchar**|Descripteur de conversation du dialogue.|34|Non|  
|**Priorité**|**int**|Niveau de priorité de la conversation.|5|Oui|  
|**RoleName**|**nvarchar**|Rôle du descripteur de conversation. Il peut prendre la valeur **initiator** ou la valeur **target**.|38|Non|  
|**ServerName**|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|**Niveau de gravité**|**int**|Gravité de l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] éventuellement indiquée par cet événement.|29|Non|  
|**SPID**|**int**|ID de processus serveur qui est affecté par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au processus associé au client.|12|Oui|  
|**StartTime**|**datetime**|Heure de début de l'événement, si disponible.|14|Oui|  
|**TextData**|**ntext**|État actuel de la conversation. Peut avoir l'une des valeurs suivantes :|1|Oui|  
|||**SO**. Démarrée en sortie. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a traité une instruction BEGIN CONVERSATION pour cette conversation, mais aucun message n'a été envoyé.|||  
|||**SI**. Démarré en entrée. Une autre instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] a démarré une nouvelle conversation avec l’instance actuelle, mais cette dernière n’a pas fini de recevoir le premier message. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut créer la conversation dans cet état si le premier message est fragmenté ou si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reçoit les messages dans le désordre. Toutefois, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut créer la conversation dans l'état CO si la première transmission reçue pour cette conversation contient la totalité du premier message.|||  
|||**CO**. Conversation en cours. La conversation est établie et ses deux parties peuvent envoyer des messages. L'essentiel de la communication pour un service classique a lieu lorsque la conversation se trouve dans cet état.|||  
|||**DI**. Déconnecté en entrée. La partie distante de la conversation a émis une instruction END CONVERSATION. La conversation demeure dans cet état jusqu'à ce que la partie locale de la conversation émette une instruction END CONVERSATION. Une application peut encore recevoir des messages pour la conversation. Dans la mesure où la partie distante de la conversation a terminé celle-ci, une application ne peut pas envoyer de messages sur cette conversation. Lorsqu'une application émet une instruction END CONVERSATION, la conversation passe à l'état CD (fermée).|||  
|||**DÉPLOYER**. Déconnecté en sortie. La partie locale de la conversation a émis une instruction END CONVERSATION. La conversation reste dans cet état jusqu'à ce que le côté distant accuse réception de la commande END CONVERSATION. Une application ne peut pas envoyer ou recevoir des messages pour la conversation. Lorsque la partie distante de la conversation accuse réception de l'instruction END CONVERSATION, la conversation passe en état CD (fermée).|||  
|||**ER**. Erreur. Une erreur s'est produite sur ce point de terminaison. Les colonnes Error, Severity et State contiennent des informations sur l'erreur spécifique qui s'est produite.|||  
|||**CD**. Fermé. Le point de terminaison de la conversation n'est plus en cours d'utilisation.|||  
|**ID de transaction**|**bigint**|ID affecté à la transaction par le système.|4|Non|  
  
 Le tableau ci-dessous répertorie les valeurs des sous-classes pour cette classe d'événements.  
  
|id|Sous-classe|Description|  
|--------|--------------|-----------------|  
|1|SEND Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement **SEND Message** lorsque le [!INCLUDE[ssDE](../../includes/ssde-md.md)] exécute une instruction SEND.|  
|2|END CONVERSATION|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement **END CONVERSATION** lorsque le [!INCLUDE[ssDE](../../includes/ssde-md.md)] exécute une instruction END CONVERSATION qui ne comporte pas la clause WITH ERROR.|  
|3|END CONVERSATION WITH ERROR|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement **END CONVERSATION WITH ERROR** lorsque le [!INCLUDE[ssDE](../../includes/ssde-md.md)] exécute une instruction END CONVERSATION qui comporte la clause WITH ERROR.|  
|4|Broker Initiated Error|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement **Broker Initiated Error** chaque fois que [!INCLUDE[ssSB](../../includes/sssb-md.md)] crée un message d'erreur. Par exemple, lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] ne parvient pas à acheminer un message pour un dialogue, il crée un message d'erreur pour celui-ci et génère cet événement. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne génère pas cet événement lorsqu'une application termine une conversation par une erreur.|  
|5|Terminate Dialog|[!INCLUDE[ssSB](../../includes/sssb-md.md)] a mis fin au dialogue. [!INCLUDE[ssSB](../../includes/sssb-md.md)] agit ainsi en réponse à des conditions qui empêchent la poursuite du dialogue mais qui ne correspondent ni à des erreurs ni à la fin normale d'une conversation. Par exemple, la suppression d'un service amène [!INCLUDE[ssSB](../../includes/sssb-md.md)] à mettre fin à tous les dialogues associés à ce service.|  
|6|Received Sequenced Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une classe d’événements **Received Sequenced Message** lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reçoit un message qui contient un numéro de séquence de message. Tous les types de messages définis par l'utilisateur sont des messages séquencés. [!INCLUDE[ssSB](../../includes/sssb-md.md)] génère un message non séquencé dans deux cas :<br /><br /> Les messages d'erreur générés par [!INCLUDE[ssSB](../../includes/sssb-md.md)] ne sont pas séquencés.<br /><br /> Les accusés de réception des messages peuvent être non séquencés. Par souci d’efficacité, [!INCLUDE[ssSB](../../includes/sssb-md.md)] inclut tout accusé de réception disponible d’un message dans le cadre d’un message séquencé. Toutefois, si une application n'envoie pas un message séquencé au point de terminaison distant pendant un certain temps, [!INCLUDE[ssSB](../../includes/sssb-md.md)] crée un message non séquencé pour l'accusé de réception du message.|  
|7|Received END CONVERSATION|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement Received END CONVERSATION lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reçoit un message End Dialog de l’autre partie de la conversation.|  
|8|Received END CONVERSATION WITH ERROR|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement **Received END CONVERSATION WITH ERROR** lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reçoit une erreur définie par l’utilisateur provenant de l’autre côté de la conversation. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne génère pas cet événement lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reçoit une erreur définie par le Broker.|  
|9|Received Broker Error Message|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement **Received Broker Error Message** lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] reçoit un message d’erreur défini par le Broker provenant de l’autre côté de la conversation. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne génère pas cet événement lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] reçoit un message d'erreur généré par une application.<br /><br /> Par exemple, si la base de données actuelle contient un itinéraire par défaut vers une base de données de transfert, [!INCLUDE[ssSB](../../includes/sssb-md.md)] achemine un message avec un nom de service inconnu vers la base de données de transfert. Si celle-ci ne peut pas acheminer le message, le Broker qu'elle contient crée un message d'erreur et le renvoie à la base de données active. Lorsque la base de données active reçoit l’erreur générée par le Broker en provenance de la base de données de transfert, elle génère un événement **Received Broker Error Message** .|  
|10|Received END CONVERSATION Ack|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une classe d'événements **Received END CONVERSATION Ack** lorsque l'autre partie d'une conversation accuse réception d'un message End Dialog ou Error envoyé par cette partie de la conversation.|  
|11|BEGIN DIALOG|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement **BEGIN DIALOG** lorsque le moteur de base de données exécute une commande BEGIN DIALOG.|  
|12|Dialog Created|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement **Dialog Created** lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] crée un point de terminaison pour un dialogue. [!INCLUDE[ssSB](../../includes/sssb-md.md)] crée un point de terminaison toutes les fois qu'un nouveau dialogue est établi, que la base de données actuelle soit l'initiateur ou la cible du dialogue.|  
|13|END CONVERSATION WITH CLEANUP|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement END CONVERSATION WITH CLEANUP lorsque le [!INCLUDE[ssDE](../../includes/ssde-md.md)] exécute une instruction END CONVERSATION qui comporte la clause WITH CLEANUP.|  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
