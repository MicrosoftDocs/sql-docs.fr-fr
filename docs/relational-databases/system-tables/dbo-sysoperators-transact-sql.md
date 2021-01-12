---
description: dbo.sysoperators (Transact-SQL)
title: Opérateurs dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysoperators
- dbo.sysoperators
- dbo.sysoperators_TSQL
- sysoperators_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysoperators system table
ms.assetid: c2afa20c-b15f-46ca-ae74-2eb65909409e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 124ba525abf1bc9e96b065d9ef824ec35ef37b50
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096274"
---
# <a name="dbosysoperators-transact-sql"></a>dbo.sysoperators (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque opérateur de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de l’opérateur.|  
|**name**|**sysname**|nom de l’opérateur.|  
|**activé**|**tinyint**|État des notifications d'alerte (booléen). Si la fonction est **1**, cet opérateur peut recevoir des notifications lorsqu’une alerte se produit.|  
|**email_address**|**nvarchar(100**|Adresse de messagerie de cet opérateur.|  
|**last_email_date**|**int**|Date de la dernière notification d'alerte que cet opérateur a reçue par courrier électronique.|  
|**last_email_time**|**int**|Heure de la dernière notification d'alerte que cet opérateur a reçue par courrier électronique.|  
|**pager_address**|**nvarchar(100**|Adresse de radiomessagerie de cet opérateur.|  
|**last_pager_date**|**int**|Date de la dernière notification d'alerte que cet opérateur a reçue par radiomessagerie.|  
|**last_pager_time**|**int**|Heure de la dernière notification d'alerte que cet opérateur a reçue par radiomessagerie.|  
|**weekday_pager_start_time**|**int**|Première heure d'un jour ouvrable (du lundi au vendredi) à partir de laquelle l'opérateur peut recevoir des notifications d'alerte par radiomessagerie.|  
|**weekday_pager_end_time**|**int**|Heure d'un jour ouvrable (du lundi au vendredi) après laquelle cet opérateur ne peut plus recevoir de notifications d'alerte par radiomessagerie.|  
|**saturday_pager_start_time**|**int**|Première heure, le samedi, à partir de laquelle l'opérateur peut recevoir des notifications d'alerte par radiomessagerie.|  
|**saturday_pager_end_time**|**int**|Heure, le samedi, après laquelle cet opérateur ne peut plus recevoir de notifications d'alerte par radiomessagerie.|  
|**sunday_pager_start_time**|**int**|Heure, le dimanche, à partir de laquelle l'opérateur peut recevoir des notifications d'alerte par radiomessagerie.|  
|**sunday_pager_end_time**|**int**|Heure, le dimanche, après laquelle cet opérateur ne peut plus recevoir de notifications d'alerte par radiomessagerie.|  
|**pager_days**|**tinyint**|Masque binaire représentant les jours de la semaine où l'opérateur peut recevoir des notifications d'alerte par radiomessagerie.|  
|**netsend_address**|**nvarchar(100**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**last_netsend_date**|**int**|Date à laquelle le dernier message réseau a été envoyé à l'ID d'opérateur spécifié.|  
|**last_netsend_time**|**int**|Heure à laquelle le dernier message réseau a été envoyé à l'ID d'opérateur spécifié.|  
|**category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables SQL Server Agent &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)  
  
  
