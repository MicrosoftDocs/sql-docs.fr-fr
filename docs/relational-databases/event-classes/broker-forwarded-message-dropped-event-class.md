---
description: Broker:Forwarded Message Dropped, classe d'événements
title: Broker:Forwarded Message Dropped, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Broker:Forwarded Message Dropped event class
ms.assetid: ec242d0b-77b0-45f5-8b12-186a14b173a8
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 14a917d6a6faec85cbc712f612e593fdfe19bc06
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197857"
---
# <a name="brokerforwarded-message-dropped-event-class"></a>Broker:Forwarded Message Dropped, classe d'événements

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un événement Broker:Forwarded Message Dropped si Service Broker supprime un message prévu pour être transféré.  
  
## <a name="brokerforwarded-message-dropped-event-class-data-columns"></a>Colonnes de données de la classe d'événements Broker:Forwarded Message Dropped  
  
|Colonne de données|Type|Description|Numéro de colonne|Filtrable|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|BigintData1|**bigint**|Numéro de séquence du message.|52|Non|  
|ClientProcessID|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. Cette colonne de données est remplie si l'ID du processus du client est fourni par le client.|9|Oui|  
|DatabaseID|**int**|ID de la base de données spécifiée par l’instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n’a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données ServerName est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|nom_base_de_données|**nvarchar**|Nom de la base de données dans laquelle l'instruction de l'utilisateur est exécutée.|35|Oui|  
|DBUserName|**nvarchar**|Identifiant de l'instance de Service Broker d'où provient le message.|40|Non|  
|Error|**int**|ID du message dans sys.messages destiné au texte de l'événement.|31|Non|  
|EventClass|**int**|Type de classe d'événements capturée. Renvoie toujours 191 pour Broker:Forwarded Message Dropped.|27|Non|  
|EventSequence|**int**|Numéro de séquence de cet événement.|51|Non|  
|FileName|**nvarchar**|Nom du service auquel le message est destiné.|36|Non|  
|GUID|**uniqueidentifier**|ID de conversation du dialogue. Cet identifiant est transmis en tant que partie intégrante du message et est partagé par les deux intervenants de la conversation.|54|Non|  
|HostName|**nvarchar**|Nom de l'ordinateur sur lequel s'exécute le client. Cette colonne de données est remplie si le nom de l'hôte est fourni par le client. Pour déterminer le nom de l'hôte, utilisez la fonction HOST_NAME.|8|Oui|  
|IndexID|**int**|Nombre de sauts restants au message transféré.|24|Non|  
|IntegerData|**int**|Nombre de fragments du message transféré.|25|Non|  
|LoginSid|**image**|Numéro d'identification de sécurité (SID) de l'utilisateur connecté. Chaque connexion possède un SID unique au niveau du serveur.|41|Oui|  
|NTDomainName|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|NTUserName|**nvarchar**|Nom de l'utilisateur propriétaire de la connexion ayant généré l'événement.|6|Oui|  
|ObjectId|**int**|Valeur de durée de vie du message transféré.|22|Non|  
|ObjectName|**nvarchar**|ID du message transféré.|34|Non|  
|OwnerName|**nvarchar**|Identificateur de l'instance de Service Broker correspondant à la destination du message.|37|Non|  
|RoleName|**nvarchar**|Rôle du descripteur de conversation. Valeurs possibles :<br /><br /> -Initiateur. Cette instance de Service Broker a lancé la conversation.<br /><br /> -Cible. Cette instance de Service Broker correspond à la cible de la conversation.|38|Non|  
|ServerName|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|severity|**int**|Numéro de gravité du texte de l'événement.|29|Non|  
|SPID|**int**|ID du processus serveur affecté par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au processus associé au client.|12|Oui|  
|StartTime|**datetime**|Heure de début de l'événement, le cas échéant.|14|Oui|  
|State|**int**|Indique l'emplacement dans le code source [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a produit l'événement. Chaque emplacement susceptible de générer cet événement possède un code d'état spécifique. Un spécialiste de l'assistance technique Microsoft peut se servir de ce code d'état afin de déterminer où l'événement s'est produit.|30|Non|  
|Succès|**int**|Durée pendant laquelle le message est resté actif. Si cette valeur est supérieure ou égale à la durée de vie prévue, le message est supprimé.|23|Non|  
|TargetLoginName|**nvarchar**|Adresse réseau à laquelle le message devait être transféré.|42|Non|  
|TargetUserName|**nvarchar**|Nom du service initiateur du message.|39|Non|  
|TextData|**ntext**|Description de la raison pour laquelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a supprimé le message.|1|Oui|  
|ID de transaction|**bigint**|ID affecté à la transaction par le système.|4|Non|  
  
 La colonne TextData de cet événement contient une description de la raison pour laquelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a supprimé le message.  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
