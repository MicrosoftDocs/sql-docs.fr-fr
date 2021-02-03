---
description: Classe d'événement Audit Broker Conversation
title: Audit Broker Conversation, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Audit Broker Conversation event class
ms.assetid: d58e3577-e297-42e5-b8fe-206665a75d13
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 41813ebefcdf014045b343719249009a2908584f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179797"
---
# <a name="audit-broker-conversation-event-class"></a>Classe d'événement Audit Broker Conversation
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée un événement **Audit Broker Conversation** pour renvoyer des messages d’audit relatifs à la sécurité du dialogue de Service Broker.  
  
## <a name="audit-broker-conversation-event-class-data-columns"></a>Colonnes de données de la classe d'événements Audit Broker Conversation  
  
|Colonne de données|Type|Description|Numéro de colonne|Filtrable|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|10|Oui|  
|**BigintData1**|**bigint**|Numéro de séquence du message.|52|Non|  
|**ClientProcessID**|**int**|ID affecté par l'ordinateur hôte au processus dans lequel s'exécute l'application cliente. Cette colonne de données est remplie si l'ID du processus du client est fourni par le client.|9|Oui|  
|**DatabaseID**|**int**|ID de la base de données spécifiée par l'instruction USE *database* ou celui de la base de données par défaut si aucune instruction USE *database* n'a été spécifiée pour une instance donnée. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] affiche le nom de la base de données si la colonne de données **ServerName** du serveur est capturée dans la trace et que le serveur est disponible. Déterminez la valeur pour une base de données à l'aide de la fonction DB_ID.|3|Oui|  
|**Error**|**int**|Numéro de l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si cet événement signale une erreur.|31|Non|  
|**EventClass**|**int**|Type de classe d'événements capturée. Toujours **158** pour **Audit Broker Conversation**.|27|Non|  
|**EventSubClass**|**int**|Type de sous-classe d’événements, qui fournit des informations complémentaires concernant chaque classe d’événements. Le tableau suivant répertorie les valeurs des sous-classes d'événements pour cet événement.|21|Oui|  
|**FileName**|**nvarchar**|Raison de l'échec de la connexion. Si la connexion réussit, cette colonne est vide.|36|Non|  
|**GUID**|**uniqueidentifier**|ID de conversation du dialogue. Cet identifiant est transmis en tant que partie intégrante du message et est partagé par les deux intervenants de la conversation.|54|Non|  
|**HostName**|**nvarchar**|Nom de l'ordinateur sur lequel s'exécute le client. Cette colonne de données est remplie si le nom de l'hôte est fourni par le client. Pour déterminer le nom de l’hôte, utilisez la fonction **HOST_NAME** .|8|Oui|  
|**IntegerData**|**int**|Numéro de fragment du message.|25|Non|  
|**NTDomainName**|**nvarchar**|Domaine Windows auquel appartient l'utilisateur.|7|Oui|  
|**NTUserName**|**nvarchar**|Nom de l'utilisateur propriétaire de la connexion ayant généré l'événement.|6|Oui|  
|**ObjectId**|**int**|ID utilisateur du service cible.|22|Non|  
|**RoleName**|**nvarchar**|Rôle du descripteur de conversation. Il peut prendre la valeur **initiator** ou la valeur **target**.|38|Non|  
|**ServerName**|**nvarchar**|Nom de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tracée.|26|Non|  
|**Niveau de gravité**|**int**|Gravité de l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] éventuellement indiquée par cet événement.|29|Non|  
|**SPID**|**int**|ID du processus serveur affecté par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au processus associé au client.|12|Oui|  
|**StartTime**|**datetime**|Heure de début de l'événement, le cas échéant.|14|Oui|  
|**State**|**int**|Indique l'emplacement dans le code source [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a produit l'événement. Chaque emplacement susceptible de générer cet événement possède un code d'état spécifique. Un spécialiste de l'assistance technique Microsoft peut se servir de ce code d'état afin de déterminer où l'événement s'est produit.|30|Non|  
|**TextData**|**ntext**|En cas d'erreurs, contient un message qui décrit la raison de l'échec. Une des valeurs suivantes :<br /><br /> <br /><br /> **Le certificat est introuvable**. L'utilisateur spécifié pour la sécurité du protocole du dialogue n'a pas de certificat.<br /><br /> **Période non valide**. L'utilisateur spécifié pour la sécurité du protocole de dialogue a un certificat, mais ce dernier est arrivé à expiration.<br /><br /> **Le certificat est trop volumineux pour une allocation de mémoire**. L'utilisateur spécifié pour la sécurité du protocole de dialogue a un certificat, mais ce dernier est trop volumineux. La taille maximale que Service Broker prend en charge pour les certificats est de 32 768 octets.<br /><br /> **Clé privée introuvable**. L'utilisateur spécifié pour la sécurité du protocole de dialogue a un certificat, mais ce dernier n'est associé à aucune clé privée.<br /><br /> **La taille de la clé privée du certificat est incompatible avec le fournisseur de services de chiffrement**. Du fait de sa taille, la clé privée qui accompagne le certificat rend le traitement impossible. La taille d'une clé privée doit être un multiple de 64 octets.<br /><br /> **La taille de la clé publique du certificat est incompatible avec le fournisseur de services de chiffrement**. Du fait de sa taille, la clé publique qui accompagne le certificat rend le traitement impossible. La taille d'une clé publique doit être un multiple de 64 octets.<br /><br /> **La taille de la clé privée du certificat est incompatible avec la clé d’échange de clés chiffrées**. La taille de la clé spécifiée dans la clé d'échange de clés ne correspond pas à la taille de la clé privée définie pour le certificat. Cela indique généralement que le certificat sur l'ordinateur distant ne correspond pas au certificat figurant dans la base de données.<br /><br /> **La taille de la clé publique du certificat est incompatible avec la signature de l’en-tête de sécurité**. L'en-tête de sécurité contient une signature qu'il est impossible de valider par rapport à la clé publique du certificat. Cela indique généralement que le certificat sur l'ordinateur distant ne correspond pas au certificat figurant dans la base de données.|1|Oui|  
  
 Le tableau suivant répertorie les valeurs des sous-classes pour cette classe d'événements.  
  
|id|Sous-classe|Description|  
|--------|--------------|-----------------|  
|1|Aucun en-tête de sécurité|Au cours d'une conversation sécurisée, Service Broker a reçu un message sans clé de session. Une fois la conversation sécurisée établie, le protocole de dialogue requiert que tous les messages de la conversation comportent une clé de session.|  
|2|Aucun certificat|Service Broker n'a pas pu trouver un certificat valide pour l'un des participants à la conversation. Pour sécuriser une conversation, la base de données doit contenir un certificat pour l'émetteur et le destinataire de la conversation.|  
|3|Signature incorrecte|Service Broker n'a pas pu vérifier la signature du message fournie par l'émetteur à l'aide de la clé publique figurant dans son certificat. Cela peut indiquer que le message est corrompu ou endommagé, que les services distants et locaux ne sont pas configurés avec le même certificat d'utilisateur ou que le certificat est arrivé à expiration.|  
|4|Échec de l'exécution en tant que cible|L'utilisateur de destination ne dispose pas des autorisations de réception voulues sur la file d'attente de destination. Pour empêcher les utilisateurs non habilités de recevoir des messages, Service Broker n'empile pas les messages à destination d'un utilisateur ne disposant pas des autorisations appropriées sur la file d'attente, que l'initiateur ait ou non le droit d'empiler les messages.|  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
