---
description: dbo.sysalerts (Transact-SQL)
title: Alertes de dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysalerts
- sysalerts_TSQL
- dbo.sysalerts_TSQL
- sysalerts
dev_langs:
- TSQL
helpviewer_keywords:
- sysalerts system table
ms.assetid: a2c2f50d-61f3-4951-996a-add5ad092cc2
author: cawrites
ms.author: chadam
ms.openlocfilehash: ff1e70701882c1740ae91212c8d33ae5e7e3040b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094869"
---
# <a name="dbosysalerts-transact-sql"></a>dbo.sysalerts (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque alerte. Une alerte est un message envoyé en réponse à un événement. Elle peut transmettre des messages au-delà de l'environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sous la forme d'un message électronique ou d'un radiomessage. Une alerte peut également générer une tâche.  Cette table est stockée dans la base de données **msdb** .
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identification de l'alerte|  
|**name**|**sysname**|Nom de l’alerte.|  
|**event_source**|**nvarchar(100**|Source de l'événement : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**event_category_id**|**int**|Réservé à un usage ultérieur.|  
|**event_id**|**int**|Réservé à un usage ultérieur.|  
|**message_id**|**int**|ID de message défini par l’utilisateur ou référence à un message de **sysmessages** qui déclenche cette alerte.|  
|**severity**|**int**|Gravité de l'erreur qui déclenche cette alerte.|  
|**activé**|**tinyint**|État de l'alerte :<br /><br /> **0** = désactivé.<br /><br /> **1** = activé.|  
|**delay_between_responses**|**int**|Délai d'attente, en secondes, entre les notifications de cette alerte.|  
|**last_occurrence_date**|**int**|Dernière occurrence (date) de l'alerte.|  
|**last_occurrence_time**|**int**|Dernière occurrence (heure) de l'alerte.|  
|**last_response_date**|**int**|Dernière notification (date) de l'alerte.|  
|**last_response_time**|**int**|Dernière notification (heure) de l'alerte.|  
|**notification_message**|**nvarchar(512)**|Informations complémentaires envoyées avec l'alerte.|  
|**include_event_description**|**tinyint**|Masque binaire indiquant si la description de l’événement est envoyée par courrier électronique, radiomessagerie ou net send. Consultez le graphique ci-dessous pour connaître les valeurs.|  
|**database_name**|**nvarchar(512)**|Base de données dans laquelle cette alerte doit se produire pour déclencher cette alerte.|  
|**event_description_keyword**|**nvarchar(100**|Modèle auquel doit correspondre l'erreur pour déclencher l'alerte.|  
|**occurrence_count**|**int**|Nombre d'occurrences de cette alerte.|  
|**count_reset_date**|**int**|Le nombre de jours (date) sera réinitialisé à **0**.|  
|**count_reset_time**|**int**|L’heure du nombre de jours sera réinitialisée à **0**.|  
|**job_id**|**uniqueidentifier**|Identificateur du travail exécuté lorsque l'alerte se produit.|  
|**has_notification**|**int**|Nombre d'opérateurs avertis par courrier électronique lorsque l'alerte a lieu.|  
|**flags**|**int**|Réservé.|  
|**performance_condition**|**nvarchar(512)**|Réservé.|  
|**category_id**|**int**|Réservé.|  
  
 ## <a name="remarks"></a>Notes

Le tableau suivant présente les valeurs du masque de include_event_description. La valeur décimale est retournée par les alertes dbo.sys. 

|Décimal | binary | Signification |
|------|------|------|
|0 |0000 |aucun message |
|1 |0,001 |email |
|2 |0010 |pager |
|3 |0011 |radiomessagerie et e-mail |
|4 |0100 |Envoi réseau |
|5 |0101 |Envoi réseau et messagerie |
|6 |0110 |Envoi réseau et radiomessagerie |
|7 |0111 |Net Send, radiomessagerie et e-mail |
  
