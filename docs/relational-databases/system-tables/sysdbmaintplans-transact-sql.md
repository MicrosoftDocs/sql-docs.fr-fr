---
description: sysdbmaintplans (Transact-SQL)
title: sysdbmaintplans (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysdbmaintplans_TSQL
- sysdbmaintplans
dev_langs:
- TSQL
helpviewer_keywords:
- sysdbmaintplans system table
ms.assetid: 0363296a-3082-48a9-9eb5-a1020b2f541a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2aec8e8b94dc341225a5e34e35f2f168fa83e56e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098073"
---
# <a name="sysdbmaintplans-transact-sql"></a>sysdbmaintplans (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette table existe dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin de conserver des informations existantes pour les instances mises à niveau à partir d'une précédente version de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne modifie pas le contenu de cette table. Cette table est stockée dans la base de données **msdb** .  
  
 [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)]  

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**plan_id**|**uniqueidentifier**|Identificateur du plan de maintenance de la base de données.|  
|**plan_name**|**sysname**|Nom du plan de maintenance de base de données.|  
|**date_created**|**datetime**|Date de création du plan de maintenance de base de données.|  
|**propriétaire**|**sysname**|Propriétaire du plan de maintenance de base de données.|  
|**max_history_rows**|**int**|Nombre maximal de lignes allouées à l'enregistrement de l'historique du plan de maintenance de base de données dans la table système.|  
|**remote_history_server**|**sysname**|Nom du serveur distant sur lequel le rapport de l'historique peut être écrit.|  
|**max_remote_history_rows**|**int**|Nombre maximal de lignes allouées dans la table système d'un serveur distant dans laquelle le rapport de l'historique peut être écrit.|  
|**user_defined_1**|**int**|La valeur par défaut est NULL.|  
|**user_defined_2**|**nvarchar(100**|La valeur par défaut est NULL.|  
|**user_defined_3**|**datetime**|La valeur par défaut est NULL.|  
|**user_defined_4**|**uniqueidentifier**|La valeur par défaut est NULL.|  
|**log_shipping**|**bit**|État de l'envoi de journaux :<br /><br /> **0** = désactivé **1** = activé|  
  
  
