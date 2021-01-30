---
description: IHextendedSubscriptionView (Transact-SQL)
title: IHextendedSubscriptionView (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHextendedSubscriptionView_TSQL
- IHextendedSubscriptionView
dev_langs:
- TSQL
helpviewer_keywords:
- IHextendedSubscriptionView view
ms.assetid: 124756a4-463a-4a81-bf5b-de7e8ffc7a62
author: stevestein
ms.author: sstein
ms.openlocfilehash: 75498189c413aa89a81d2c0e8086c0f5cc37e1a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208887"
---
# <a name="ihextendedsubscriptionview-transact-sql"></a>IHextendedSubscriptionView (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vue **IHextendedSubscriptionView** expose des informations sur l’abonnement à une publication non SQL Server. Cette vue est stockée dans la base de données de **distribution** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**article_id**|**int**|Identificateur unique d'un article|  
|**dest_db**|**sysname**|Nom de la base de données de destination|  
|**srvid**|**smallint**|Identificateur unique pour un Abonné|  
|**login_name**|**sysname**|Nom de connexion utilisé pour se connecter à un Abonné.|  
|**distribution_jobid**|**binary**|Identifie le travail de l'Agent de distribution.|  
|**publisher_database_id**|**int**|Identifie la base de données de publication.|  
|**subscription_type**|**int**|Type d’abonnement :<br /><br /> **0** = Push : l’agent de distribution s’exécute sur l’abonné.<br /><br /> **1** = pull-l’agent de distribution s’exécute sur le serveur de distribution.|  
|**sync_type**|**tinyint**|Type de synchronisation initiale :<br /><br /> **1** = automatique<br /><br /> **2** = aucun|  
|**statut**|**tinyint**|État de l’abonnement :<br /><br /> **0** = inactif<br /><br /> **1** = abonné<br /><br /> **2** = actif|  
|**snapshot_seqno_flag**|**bit**|Indique si un numéro de séquence d'instantané est utilisé.|  
|**independent_agent**|**bit**|Spécifie s’il existe un Agent de distribution autonome pour cette publication.<br /><br /> **0** = la publication utilise un agent de distribution partagé, et chaque paire base de données du serveur de publication/base de données de l’abonné possède un seul agent partagé.<br /><br /> **1** = il existe un agent de distribution autonome pour cette publication.|  
|**subscription_time**|**datetime**|À usage interne uniquement|  
|**loopback_detection**|**bit**|S'applique aux abonnements qui font partie d'une topologie de réplication transactionnelle bidirectionnelle. La détection de boucle détermine si l'Agent de distribution renvoie à l'Abonné les transactions émanant de ce dernier :<br /><br /> **1** = n’est pas renvoyé.<br /><br /> **0** = renvoie.|  
|**agent_id**|**int**|Identificateur unique de l'Agent de distribution.|  
|**update_mode**|**tinyint**|Indique le type du mode de mise à jour, qui peut être l'un des suivants :<br /><br /> **0** = lecture seule.<br /><br /> **1** = mise à jour immédiate.<br /><br /> **2** = mise à jour en file d’attente à l’aide de Message Queuing.<br /><br /> **3** = mise à jour immédiate avec mise à jour en file d’attente comme basculement à l’aide de Message Queuing.<br /><br /> **4** = mise à jour en file d’attente à l’aide de SQL Server file d’attente.<br /><br /> **5** = mise à jour immédiate avec basculement de mise à jour en file d’attente à l’aide de SQL Server file d’attente.|  
|**publisher_seqno**|**varbinary(16)**|Numéro de séquence de la transaction au niveau du serveur de publication pour cet abonnement.|  
|**ss_cplt_seqno**|**varbinary(16)**|Numéro de séquence utilisé pour indiquer la fin du traitement de l'instantané concurrent.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
