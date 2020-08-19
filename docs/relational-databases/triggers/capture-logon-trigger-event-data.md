---
description: Capturer des données d'événements de déclencheurs de connexion
title: Capturer des données d’événement de déclencheur de connexion | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: e05b1ab4-c10b-402a-9591-f6ec1e3db8c0
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8511e1021383921d52a190cb9df80b3d5939060a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88418903"
---
# <a name="capture-logon-trigger-event-data"></a>Capturer des données d'événements de déclencheurs de connexion
[!INCLUDE[tsql-appliesto-ss2008-appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)]
   Pour capturer des données XML relatives à des événements LOGON à utiliser dans des déclencheurs de connexion, utilisez la fonction [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md). L'événement LOGON retourne le schéma des données d'événement suivant :  
  
 `<EVENT_INSTANCE>`  
  
 `<EventType>event_type</EventType>`  
  
 `<PostTime>post_time</PostTime>`  
  
 `<SPID>spid</SPID>`  
  
 `<ServerName>server_name</ServerName>`  
  
 `<LoginName>login_name</LoginName>`  
  
 `<LoginType>login_type</LoginType>`  
  
 `<SID>sid</SID>`  
  
 `<ClientHost>client_host</ClientHost>`  
  
 `<IsPooled>is_pooled</IsPooled>`  
  
 `</EVENT_INSTANCE>`  
  
 `<EventType>`  
 Contient `LOGON`.  
  
 `<PostTime>`  
 Contient l'heure de demande d'établissement d'une session.  
  
 `<SID>`  
 Contient le flux binaire encodé en base 64 du numéro d'identification de sécurité (SID) correspondant au nom de connexion spécifié.  
  
 `<ClientHost>`  
 Contient le nom d'hôte du client à partir duquel a été établie la connexion. La valeur est '`<local_machine>`' si le nom du serveur et du client sont identiques. Sinon, la valeur est l'adresse IP du client.  
  
 `<IsPooled>`  
 Valeur égale à `1` si la connexion est réutilisée à l'aide du groupement de connexions. Sinon, la valeur est `0`.  
  
  
