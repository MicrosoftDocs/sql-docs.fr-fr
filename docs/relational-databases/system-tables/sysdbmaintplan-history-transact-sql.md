---
description: sysdbmaintplan_history (Transact-SQL)
title: sysdbmaintplan_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysdbmaintplan_history_TSQL
- sysdbmaintplan_history
dev_langs:
- TSQL
helpviewer_keywords:
- sysdbmaintplan_history system table
ms.assetid: 02d36f08-ac93-4463-bb59-284c5cd6ed04
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 473e98da465aa9952261ae3c57d77ab9b5d2629e
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89517676"
---
# <a name="sysdbmaintplan_history-transact-sql"></a>sysdbmaintplan_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette table est stockée dans la base de données **msdb** .  
  
 [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)]  
  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**sequence_id**|**int**|Séquence de l'historique effectué par les plans de maintenance de la base de données.|  
|**plan_id**|**uniqueidentifier**|Identificateur du plan de maintenance de la base de données.|  
|**plan_name**|**sysname**|Nom du plan de maintenance de base de données.|  
|**database_name**|**sysname**|Nom de la base de données associée au plan de maintenance de base de données.|  
|**server_name**|**sysname**|Nom système.|  
|**activity**|**nvarchar(128)**|Activité réalisée par le plan de maintenance de la base de données (par exemple, sauvegarde du journal des transactions).|  
|**succeeded**|**bit**|**0** = succès **1** = échec|  
|**heure-fin**|**datetime**|Heure de la fin de l'exécution de l'action.|  
|**duration**|**int**|Durée nécessaire à l'exécution de l'action du plan de maintenance de la base de données.|  
|**heure-début**|**datetime**|Heure de début de l'action.|  
|**error_number**|**int**|Numéro d'erreur indiqué lors d'un échec.|  
|**message**|**nvarchar(512)**|Message généré par **sqlmaint**.|  
  
  
