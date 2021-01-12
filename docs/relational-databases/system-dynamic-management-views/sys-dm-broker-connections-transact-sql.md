---
description: sys.dm_broker_connections (Transact-SQL)
title: sys.dm_broker_connections (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/08/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_broker_connections
- dm_broker_connections
- sys.dm_broker_connections_TSQL
- dm_broker_connections_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_connections dynamic management view
ms.assetid: d9e20433-67fe-4fcc-80e3-b94335b2daef
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e95fce78ca6b416de83f9cf5ec991a131b054839
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099986"
---
# <a name="sysdm_broker_connections-transact-sql"></a>sys.dm_broker_connections (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque connexion réseau [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Le tableau suivant fournit plus d'informations :  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**connection_id**|**uniqueidentifier**|Identificateur de la connexion. Accepte la valeur NULL.|  
|**transport_stream_id**|**uniqueidentifier**|Identificateur de la connexion de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interface réseau (SNI) utilisée par cette connexion pour les communications TCP/IP. Accepte la valeur NULL.|  
|**state**|**smallint**|État actuel de la connexion. Accepte la valeur NULL. Valeurs possibles :<br /><br /> 1 = NEW<br /><br /> 2 = CONNECTING<br /><br /> 3 = CONNECTED<br /><br /> 4 = LOGGED_IN<br /><br /> 5 = FERMÉ|  
|**state_desc**|**nvarchar(60)**|État actuel de la connexion. Accepte la valeur NULL. Valeurs possibles :<br /><br /> NEW<br /><br /> CONNECTING<br /><br /> CONNECTED<br /><br /> LOGGED_IN<br /><br /> CLOSED|  
|**connect_time**|**datetime**|Date et heure d'ouverture de la connexion. Accepte la valeur NULL.|  
|**login_time**|**datetime**|Date et heure à laquelle l'ouverture de session a réussi pour la connexion. Accepte la valeur NULL.|  
|**authentication_method**|**nvarchar(128)**|Nom de la méthode d'Authentification Windows (par exemple NTLM ou KERBEROS). La valeur est fournie par Windows. Accepte la valeur NULL.|  
|**principal_name**|**nvarchar(128)**|Nom de l'ouverture de session validée pour les autorisations de connexion. Pour l'authentification Windows, cette valeur est le nom de l'utilisateur distant. Pour l'authentification par certificat, cette valeur est le propriétaire du certificat. Accepte la valeur NULL.|  
|**remote_user_name**|**nvarchar(128)**|Nom de l'utilisateur homologue provenant de l'autre base de données utilisée par l'authentification Windows. Accepte la valeur NULL.|  
|**last_activity_time**|**datetime**|Date et heure de dernière utilisation de la connexion pour envoyer ou recevoir des informations. Accepte la valeur NULL.|  
|**is_accept**|**bit**|Indique si l'origine de la connexion se trouve du côté distant. Accepte la valeur NULL.<br /><br /> 1 = la connexion est une demande acceptée provenant de l'instance distante.<br /><br /> 0 = la connexion a été démarrée par l'instance locale.|  
|**login_state**|**smallint**|État du processus de cette connexion. Valeurs possibles :<br /><br /> 0 = INITIAL<br /><br /> 1 = WAIT LOGIN NEGOTIATE<br /><br /> 2 = ONE ISC<br /><br /> 3 = ONE ASC<br /><br /> 4 = TWO ISC<br /><br /> 5 = TWO ASC<br /><br /> 6 = WAIT ISC Confirm<br /><br /> 7 = WAIT ASC Confirm<br /><br /> 8 = WAIT REJECT<br /><br /> 9 = WAIT PRE-MASTER SECRET<br /><br /> 10 = WAIT VALIDATION<br /><br /> 11 = WAIT ARBITRATION<br /><br /> 12 = EN LIGNE<br /><br /> 13 = ERROR|  
|**login_state_desc**|**nvarchar(60)**|État actuel de la connexion en provenance de l'ordinateur distant. Valeurs possibles :<br /><br /> La négociation de connexion est initialisée.<br /><br /> La négociation de connexion attend le message de négociation de la connexion.<br /><br /> La négociation de connexion a initialisé et envoyé le contexte de sécurité pour l'authentification.<br /><br /> La négociation de connexion a reçu et accepté le contexte de sécurité pour l'authentification.<br /><br /> La négociation de connexion a initialisé et envoyé le contexte de sécurité pour l'authentification. Il existe un mécanisme facultatif disponible pour l'authentification des homologues.<br /><br /> La négociation de connexion a reçu et envoyé le contexte de sécurité accepté pour l'authentification. Il existe un mécanisme facultatif disponible pour l'authentification des homologues.<br /><br /> La négociation de connexion attend le message de confirmation d'initialisation du contexte de sécurité.<br /><br /> La négociation de connexion attend le message de confirmation d'acceptation du contexte de sécurité.<br /><br /> La négociation de connexion attend le message de rejet SSPI pour l'authentification qui a échoué.<br /><br /> La négociation de connexion attend le message secret pré-master.<br /><br /> La négociation de connexion attend le message de validation.<br /><br /> La négociation de connexion attend le message d'arbitrage.<br /><br /> La négociation de connexion est terminée et en ligne (prêt) pour l'échange de messages.<br /><br /> La connexion présente une erreur.|  
|**peer_certificate_id**|**int**|ID de l'objet local du certificat utilisé par l'instance distante pour l'authentification. Le propriétaire de ce certificat doit avoir l'autorisation CONNECT pour se connecter au point de terminaison [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Accepte la valeur NULL.|  
|**encryption_algorithm**|**smallint**|Algorithme de chiffrement utilisé pour cette connexion. Accepte la valeur NULL. Valeurs possibles :<br /><br /> **Description de la valeur &#124; &#124; l’option DDL correspondante**<br /><br /> 0 &#124; aucune &#124; désactivée<br /><br /> 1 &#124; LA SIGNATURE UNIQUEMENT<br /><br /> 2 &#124; AES, RC4 &#124; requis &#124; l’algorithme RC4 requis}<br /><br /> 3 &#124; AES &#124;algorithme AES obligatoire<br /><br /> **Remarque :** L’algorithme RC4 est uniquement pris en charge pour la compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, le matériel chiffré à l’aide de RC4 ou de RC4_128 peut être déchiffré dans n’importe quel niveau de compatibilité.|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Représentation textuelle de l'algorithme de chiffrement. Accepte la valeur NULL. Valeurs possibles :<br /><br /> **Description &#124; option DDL correspondante**<br /><br /> AUCUN &#124; désactivé<br /><br /> RC4 &#124; {required &#124; algorithme RC4}<br /><br /> Algorithme AES &#124; AES obligatoire<br /><br /> AUCUN, RC4 &#124; {algorithme pris en charge &#124; pris en charge RC4}<br /><br /> AUCUN, AES &#124; algorithme RC4 pris en charge<br /><br /> RC4, AES &#124; algorithme RC4 AES<br /><br /> AES, RC4 &#124; algorithme AES RC4<br /><br /> AUCUN, RC4, AES &#124; algorithme RC4 AES<br /><br /> AUCUN, AES, RC4 &#124; algorithme AES RC4 pris en charge|  
|**receives_posted**|**smallint**|Nombre de réceptions asynchrones sur le réseau qui ne sont pas encore terminées pour cette connexion. Accepte la valeur NULL.|  
|**is_receive_flow_controlled**|**bit**|Indique si les réceptions sur le réseau ont été retardées en raison du contrôle de flux car le réseau est occupé. Accepte la valeur NULL.<br /><br /> 1 = Vrai|  
|**sends_posted**|**smallint**|Nombre d'envois asynchrones sur le réseau qui ne sont pas encore terminés pour cette connexion. Accepte la valeur NULL.|  
|**is_send_flow_controlled**|**bit**|Indique si les envois sur le réseau ont été retardés en raison du contrôle de flux sur le réseau et parce que ce dernier est occupé. Accepte la valeur NULL.<br /><br /> 1 = Vrai|  
|**total_bytes_sent**|**bigint**|Nombre total d’octets qui ont été envoyés par cette connexion. Accepte la valeur NULL.|  
|**total_bytes_received**|**bigint**|Nombre total d'octets reçus par cette connexion. Accepte la valeur NULL.|  
|**total_fragments_sent**|**bigint**|Nombre total de fragments de messages [!INCLUDE[ssSB](../../includes/sssb-md.md)] envoyés par cette connexion. Accepte la valeur NULL.|  
|**total_fragments_received**|**bigint**|Nombre total de fragments de messages [!INCLUDE[ssSB](../../includes/sssb-md.md)] reçus par cette connexion. Accepte la valeur NULL.|  
|**total_sends**|**bigint**|Nombre total de demandes d'envoi sur le réseau émises par cette connexion. Accepte la valeur NULL.|  
|**total_receives**|**bigint**|Nombre total de demandes d'envoi sur le réseau reçues par cette connexion. Accepte la valeur NULL.|  
|**peer_arbitration_id**|**uniqueidentifier**|Identificateur interne du point de terminaison. Accepte la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="physical-joins"></a>Jointures physiques  
 ![Jointures pour sys.dm_broker_connections](../../relational-databases/system-dynamic-management-views/media/join-dm-broker-connections-1.gif "Jointures pour sys.dm_broker_connections")  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Relationship|  
|----------|--------|------------------|  
|**dm_broker_connections.connection_id**|**dm_exec_connections.connection_id**|Un-à-un|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

