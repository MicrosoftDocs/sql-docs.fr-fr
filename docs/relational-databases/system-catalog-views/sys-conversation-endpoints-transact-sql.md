---
description: sys.conversation_endpoints (Transact-SQL)
title: sys.conversation_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- conversation_endpoints_TSQL
- conversation_endpoints
- sys.conversation_endpoints
- sys.conversation_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.conversation_endpoints catalog view
ms.assetid: 2ed758bc-2a9d-4831-8da2-4b80e218f3ea
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2a06a028bc7ab6bfebb64e6b54f32c7d8d0a991b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200864"
---
# <a name="sysconversation_endpoints-transact-sql"></a>sys.conversation_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Chaque côté d'une conversation [!INCLUDE[ssSB](../../includes/sssb-md.md)] est représenté par un point de terminaison. Cet affichage catalogue contient une ligne pour chaque point de terminaison dans la base de données.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|conversation_handle|**uniqueidentifier**|Identificateur du point de terminaison de la conversation. Cette colonne n'accepte pas la valeur NULL.|  
|conversation_id|**uniqueidentifier**|Identificateur de la conversation. Ce dernier est partagé par les deux participants à la conversation. Comme la colonne is_initiator, il est unique au sein de la base de données. Cette colonne n'accepte pas la valeur NULL.|  
|is_initiator|**tinyint**|Indique si le point de terminaison est l'initiateur ou la cible de la conversation.  Cette colonne n'accepte pas la valeur NULL.<br /><br /> 1 = initiateur<br /><br /> 0 = cible|  
|service_contract_id|**int**|Identificateur du contrat associé à la conversation. Cette colonne n'accepte pas la valeur NULL.|  
|conversation_group_id|**uniqueidentifier**|Identificateur du groupe de conversation auquel cette conversation appartient. Cette colonne n'accepte pas la valeur NULL.|  
|service_id|**int**|Identificateur du service associé à ce côté de la conversation. Cette colonne n'accepte pas la valeur NULL.|  
|lifetime|**datetime**|Date/heure d'expiration de la conversation. Cette colonne n'accepte pas la valeur NULL.|  
|state|**char(2)**|État actuel de la conversation. Cette colonne n'accepte pas la valeur NULL. Valeurs possibles :<br /><br /> DONC démarré en sortie. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a traité une instruction BEGIN CONVERSATION pour cette conversation, mais aucun message n'a été envoyé pour l'instant.<br /><br /> SI a démarré, entrant. Une autre instance a démarré une nouvelle conversation avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'a toujours pas complètement reçu le premier message. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut créer la conversation dans cet état si le premier message est fragmenté ou si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reçoit des messages inexploitables. Toutefois, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut créer la conversation dans l'état CO (conversation en cours) si la première transmission reçue pour cette conversation contient la totalité du premier message.<br /><br /> CO-inversion. La conversation est établie et les deux côtés peuvent envoyer des messages. La majeure partie de la communication associée à un service a généralement lieu lorsque la conversation est dans cet état.<br /><br /> DI déconnecté entrant. La partie distante de la conversation a émis une instruction END CONVERSATION. La conversation demeure dans cet état jusqu'à ce que la partie locale de la conversation émette une instruction END CONVERSATION. Une application peut toujours recevoir des messages pour la conversation. Dans la mesure où la partie distante de la conversation a terminé celle-ci, une application ne peut pas envoyer de messages sur cette conversation. Lorsqu’une application émet une action END CONVERSATION, la conversation passe à l’État CD (fermé).<br /><br /> DO   Déconnectée en sortie. La partie locale de la conversation a émis une instruction END CONVERSATION. La conversation reste dans cet état jusqu'à ce que le côté distant accuse réception de la commande END CONVERSATION. Une application ne peut pas envoyer ou recevoir des messages pour la conversation. Lorsque le côté distant de la conversation accuse réception de la commande END CONVERSATION, la conversation passe en état CD (fermée).<br /><br /> ER   Erreur. Une erreur s'est produite sur ce point de terminaison. Le message d'erreur est placé dans la file d'attente de l'application. Si la file d'attente de l'application est vide, cela signifie que l'application a déjà consommé le message d'erreur.<br /><br /> CD   Fermée. Le point de terminaison de la conversation n'est plus en cours d'utilisation.|  
|state_desc|**nvarchar(60)**|Description de l’état de la conversation du point de terminaison. Cette colonne autorise la valeur Null. Valeurs possibles :<br /><br /> **STARTED_OUTBOUND**<br /><br /> **STARTED_INBOUND**<br /><br /> **CONVERSATION**<br /><br /> **DISCONNECTED_INBOUND**<br /><br /> **DISCONNECTED_OUTBOUND**<br /><br /> **LÉGENDES**<br /><br /> **ERROR**|  
|far_service|**nvarchar (256)**|Nom du service du côté distant de la conversation. Cette colonne n'accepte pas la valeur NULL.|  
|far_broker_instance|**nvarchar(128)**|Instance du Broker associée au côté distant de la conversation. Accepte la valeur NULL.|  
|principal_id|**int**|Identificateur du principal dont le certificat est utilisé par le côté local du dialogue. Cette colonne n'accepte pas la valeur NULL.|  
|far_principal_id|**int**|Identificateur de l'utilisateur dont le certificat est utilisé par le côté distant du dialogue. Cette colonne n'accepte pas la valeur NULL.|  
|outbound_session_key_identifier|**uniqueidentifier**|Identificateur de la clé de chiffrement sortante pour le dialogue. Cette colonne n'accepte pas la valeur NULL.|  
|inbound_session_key_identifier|**uniqueidentifier**|Identificateur de la clé de chiffrement entrante pour le dialogue. Cette colonne n'accepte pas la valeur NULL.|  
|security_timestamp|**datetime**|Heure de création de la clé de session locale. Cette colonne n'accepte pas la valeur NULL.|  
|dialog_timer|**datetime**|Heure à laquelle le minuteur de conversation de ce dialogue envoie un message DialogTimer. Cette colonne n'accepte pas la valeur NULL.|  
|send_sequence|**bigint**|Numéro de message suivant dans la séquence d'envoi. Cette colonne n'accepte pas la valeur NULL.|  
|last_send_tran_id|**binary(6)**|Identificateur interne de la dernière transaction qui a envoyé un message. Cette colonne n'accepte pas la valeur NULL.|  
|end_dialog_sequence|**bigint**|Numéro de séquence du message End Dialog. Cette colonne n'accepte pas la valeur NULL.|  
|receive_sequence|**bigint**|Prochain numéro de message attendu dans la séquence de réception. Cette colonne n'accepte pas la valeur NULL.|  
|receive_sequence_frag|**int**|Prochain numéro de fragment de message attendu dans la séquence de réception. Cette colonne n'accepte pas la valeur NULL.|  
|system_sequence|**bigint**|Numéro de séquence du dernier message système pour le dialogue considéré. Cette colonne n'accepte pas la valeur NULL.|  
|first_out_of_order_sequence|**bigint**|Numéro de séquence du premier des messages désordonnés pour le dialogue considéré. Cette colonne n'accepte pas la valeur NULL.|  
|last_out_of_order_sequence|**bigint**|Numéro de séquence du dernier des messages désordonnés pour le dialogue considéré. Cette colonne n'accepte pas la valeur NULL.|  
|last_out_of_order_frag|**int**|Numéro de séquence du dernier message dans les fragments inexploitables pour cette boîte de dialogue. Cette colonne n'accepte pas la valeur NULL.|  
|is_system|**bit**|1 s'il s'agit d'un dialogue système. Cette colonne n'accepte pas la valeur NULL.|  
|priority|**tinyint**|Priorité de conversation assignée à ce point de terminaison. Cette colonne n'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
