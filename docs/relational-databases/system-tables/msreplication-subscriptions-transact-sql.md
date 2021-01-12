---
description: MSreplication_subscriptions (Transact-SQL)
title: MSreplication_subscriptions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSreplication_subscriptions
- MSreplication_subscriptions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_subscriptions system table
ms.assetid: fd0c5843-4e9b-4448-8bfb-0a4067d1d8d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8d5d0b8b450102483c844ec53472ff509b75cd0d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092375"
---
# <a name="msreplication_subscriptions-transact-sql"></a>MSreplication_subscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSreplication_subscriptions** contient une ligne d’informations de réplication pour chaque agent de distribution desservant la base de données de l’abonné local. Cette table est stockée dans la base de données d’abonnement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Nom du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**édition**|**sysname**|Nom de la publication.|  
|**independent_agent**|**bit**|Indique s'il existe un Agent de distribution autonome pour cette publication.|  
|**subscription_type**|**int**|Type d’abonnement :<br /><br /> 0 = Par envoi de données (push).<br /><br /> 1 = Par extraction de données (pull).<br /><br /> 2 = Anonyme.|  
|**distribution_agent**|**sysname**|Nom de l'Agent de distribution.|  
|**Time**|**smalldatetime**|Heure de la dernière mise à jour effectuée par l'Agent de distribution.|  
|**description**|**nvarchar(255)**|Description de l'abonnement.|  
|**transaction_timestamp**|**varbinary(16)**|À usage interne uniquement.|  
|**update_mode**|**tinyint**|Type de mise à jour.|  
|**agent_id**|**Binary(16**|ID de l'Agent.|  
|**subscription_guid**|**Binary(16**|Identificateur global de la version de l'abonnement sur la publication.|  
|**subid**|**Binary(16**|Identificateur global d'un abonnement anonyme.|  
|**immediate_sync**|**bit**|Indique si des fichiers de synchronisation sont créés ou recréés lors de chaque exécution de l'Agent d'instantané.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
