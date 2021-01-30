---
description: dbo.sysjobs (Transact-SQL)
title: Travaux de dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysjobs
- sysjobs_TSQL
- dbo.sysjobs
- dbo.sysjobs_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobs system table
ms.assetid: e244a6a5-54c2-47a6-8039-dd1852b0ae59
author: cawrites
ms.author: chadam
ms.openlocfilehash: 58cf513f91b414e5ad98eb388238e69be151e9a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210426"
---
# <a name="dbosysjobs-transact-sql"></a>dbo.sysjobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Stocke les informations de chaque travail planifié qui doit être exécuté par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|ID unique du travail.|  
|**originating_server_id**|**int**|ID du serveur d'où le travail provient.|  
|**name**|**sysname**|Nom du travail.|  
|**activé**|**tinyint**|Indique si le travail est activé pour être exécuté.|  
|**description**|**nvarchar(512)**|Description du travail.|  
|**start_step_id**|**int**|Identificateur de l'étape du travail à partir de laquelle l'exécution doit débuter.|  
|**category_id**|**int**|ID de la catégorie du travail.|  
|**owner_sid**|**varbinary(85)**|Numéro d'identification de sécurité (SID) du propriétaire du travail.|  
|**notify_level_eventlog**|**int**|**Masque binaire** indiquant les circonstances entraînant la consignation d’un événement de notification dans le journal des applications Microsoft Windows :<br /><br /> **0** = jamais<br /><br /> **1** = lorsque la tâche est réussie<br /><br /> **2** = en cas d’échec du travail<br /><br /> **3** = à chaque achèvement du travail (quel que soit le résultat du travail)|  
|**notify_level_email**|**int**|Masque binaire indiquant les circonstances entraînant l'envoi d'un courrier électronique en fin de travail :<br /><br /> **0** = jamais<br /><br /> **1** = lorsque la tâche est réussie<br /><br /> **2** = en cas d’échec du travail<br /><br /> **3** = à chaque achèvement du travail (quel que soit le résultat du travail)|  
|**notify_level_netsend**|**int**|Masque binaire indiquant les circonstances entraînant l'envoi d'un message sur le réseau en fin de travail :<br /><br /> **0** = jamais<br /><br /> **1** = lorsque la tâche est réussie<br /><br /> **2** = en cas d’échec du travail<br /><br /> **3** = à chaque achèvement du travail (quel que soit le résultat du travail)|  
|**notify_level_page**|**int**|Masque binaire indiquant les circonstances entraînant l'envoi d'un message par radiomessagerie en fin de travail :<br /><br /> **0** = jamais<br /><br /> **1** = lorsque la tâche est réussie<br /><br /> **2** = en cas d’échec du travail<br /><br /> **3** = à chaque achèvement du travail (quel que soit le résultat du travail)|  
|**notify_email_operator_id**|**int**|Nom d'adresse électronique de l'opérateur à avertir.|  
|**notify_netsend_operator_id**|**int**|ID de l'ordinateur ou de l'utilisateur utilisé pour envoyer les messages sur le réseau.|  
|**notify_page_operator_id**|**int**|ID de l'ordinateur ou de l'utilisateur utilisé pour envoyer un radiomessage.|  
|**delete_level**|**int**|**Masque binaire** indiquant les circonstances dans lesquelles le travail doit être supprimé lorsqu’un travail se termine :<br /><br /> **0** = jamais<br /><br /> **1** = lorsque la tâche est réussie<br /><br /> **2** = en cas d’échec du travail<br /><br /> **3** = à chaque achèvement du travail (quel que soit le résultat du travail)|  
|**date_created**|**datetime**|Date de création du travail.|  
|**date_modified**|**datetime**|Date de dernière modification du travail.|  
|**version_number**|**int**|Version du travail.|  
  
  
