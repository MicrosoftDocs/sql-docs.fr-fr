---
description: Broker:Remote Message Ack, classe d'événements
title: Broker:Remote Message Ack, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Broker:Remote Message Ack event class
ms.assetid: 3d67efe1-74b4-4633-b029-c6e05b19f4dc
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ef4d192eac8b973285039bbd0d7573a9aa934bc2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99110551"
---
# <a name="brokerremote-message-ack-event-class"></a>Broker:Remote Message Ack, classe d'événements

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement **Broker:Remote Message Ack** quand [!INCLUDE[ssSB](../../includes/sssb-md.md)] envoie ou reçoit un accusé de réception de message.  
  
## <a name="brokerremote-message-ack-event-class-data-columns"></a>Colonnes de données de la classe d'événements Broker:Remote Message Ack  
  
|Colonne de données|Type|Description|Numéro de colonne|Filtrable|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs transmises par l'application au lieu de l'affichage du nom de programme.|10|Oui|  
|**BigintData1**|**bigint**|Numéro de séquence du message qui contient l'accusé de réception.|52|Non|  
|**BigintData2**|**bigint**|Numéro de séquence du message dont la réception est accusée.|53|Non|  
|**ClientProcessID**|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. Cette colonne de données est remplie si l'ID du processus du client est fourni par le client.|9|Oui|  
|**DatabaseID**|**int**|ID de la base de données spécifiée par l'instruction USE *base de données* . Si aucune instruction USE *base de données* n’a été émise pour une instance donnée, ID de la base de données par défaut. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données **ServerName** du serveur est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|**EventClass**|**int**|Type de classe d'événements capturée. Toujours **149** pour **Broker:Message Ack**.|27|Non|  
|**EventSequence**|**int**|Numéro de séquence de cet événement.|51|Non|  
|**EventSubClass**|**nvarchar**|Type de sous-classe d'événements qui fournit des informations complémentaires sur chaque classe d'événements. Cette colonne peut contenir les valeurs ci-dessous :<br /><br /> **Message With Acknowledgement Sent**:<br />                    [!INCLUDE[ssSB](../../includes/sssb-md.md)] a envoyé un accusé de réception dans le cadre d’un message séquencé normal.<br /><br /> **Acknowledgement Sent**:<br />                    [!INCLUDE[ssSB](../../includes/sssb-md.md)] a envoyé un accusé de réception en dehors d’un message séquencé normal.<br /><br /> **Message With Acknowledgement Received**:<br />                  [!INCLUDE[ssSB](../../includes/sssb-md.md)] a reçu un accusé de réception dans le cadre d’un message séquencé normal.<br /><br /> **Acknowledgement Received**:<br />                  [!INCLUDE[ssSB](../../includes/sssb-md.md)] a reçu un accusé de réception en dehors d'un message séquencé.|21|Oui|  
|**GUID**|**uniqueidentifier**|ID de conversation du dialogue. Cet identifiant est transmis en tant que partie intégrante du message et est partagé par les deux intervenants de la conversation.|54|Non|  
|**HonorBrokerPriority**|**Int**|Valeur actuelle de l'option de base de données HONOR_BROKER_PRIORITY : 0 = OFF, 1 = ON.|32|Oui|  
|**HostName**|**nvarchar**|Nom de l'ordinateur sur lequel s'exécute le client. Cette colonne de données est remplie si le nom de l'hôte est fourni par le client. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|**IntegerData**|**int**|Numéro de fragment du message qui contient l'accusé de réception.|25|Non|  
|**IntegerData2**|**int**|Numéro de fragment du message dont la réception est accusée.|55|Non|  
|**IsSystem**|**int**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur.<br /><br /> 0 = utilisateur<br /><br /> 1 = système|60|Non|  
|**LoginSid**|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|**NTDomainName**|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|**NTUserName**|**nvarchar**|Nom de l'utilisateur propriétaire de la connexion ayant généré l'événement.|6|Oui|  
|**Priorité**|**int**|Niveau de priorité de la conversation.|5|Oui|  
|**RoleName**|**nvarchar**|Rôle de l'instance qui accuse réception du message. Il peut prendre la valeur **initiator** ou la valeur **target**.|38|Non|  
|**ServerName**|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|**SPID**|**int**|ID de processus serveur affecté par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au processus qui est associé au client.|12|Oui|  
|**StartTime**|**datetime**|Heure de début de l'événement, le cas échéant.|14|Oui|  
|**StarvationElevation**|**int**|Le message a été envoyé avec une priorité plus élevée que la priorité configurée pour la conversation : 0 = faux, 1 = vrai.|33|Oui|  
|**TransactionID**|**bigint**|ID affecté à la transaction par le système.|4|Non|  
  
  
