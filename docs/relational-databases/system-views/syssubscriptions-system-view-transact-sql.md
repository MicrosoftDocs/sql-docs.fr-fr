---
description: syssubscriptions (System View) (Transact-SQL)
title: SYSSUBSCRIPTIONS (vue système) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- syssubscriptions_TSQL
- syssubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- syssubscriptions view
ms.assetid: c9613858-9512-43a9-aa53-7ee8064f064c
author: stevestein
ms.author: sstein
ms.openlocfilehash: 052d7d4c3fa302fbb5962f89f69f1446bd6e7295
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181549"
---
# <a name="syssubscriptions-system-view-transact-sql"></a>syssubscriptions (System View) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vue **SYSSUBSCRIPTIONS** expose les informations d’abonnement. Cette vue est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|ID unique d'un article ayant fait l'objet de l'abonnement.|  
|**srvid**|**smallint**|ID de serveur de l’abonné.|  
|**dest_db**|**sysname**|Nom de la base de données d’abonnement.|  
|**statut**|**tinyint**|État de l’abonnement :<br /><br /> **0** = inactif.<br /><br /> **1** = abonné.<br /><br /> **2** = actif.|  
|**sync_type**|**tinyint**|Type de synchronisation initiale :<br /><br /> **1** = automatique.<br /><br /> **2** = aucune.|  
|**login_name**|**sysname**|Spécifie le nom de connexion utilisé lors de la connexion au serveur de publication pour ajouter l'abonnement.|  
|**subscription_type**|**int**|Type d’abonnement :<br /><br /> **0** = Push : l’agent de distribution s’exécute sur le serveur de distribution.<br /><br /> **1** = pull-l’agent de distribution s’exécute sur l’abonné.|  
|**distribution_jobid**|**Binary(16**|Identifie la tâche de l'Agent de distribution utilisée pour synchroniser l'abonnement.|  
|**timestmap**|**timestamp**|Date et heure de création de l'abonnement.|  
|**update_mode**|**tinyint**|Mode de mise à jour :<br /><br /> **0** = lecture seule.<br /><br /> **1** = mise à jour immédiate.|  
|**loopback_detection**|**bit**|S'applique aux abonnements qui font partie d'une topologie de réplication transactionnelle bidirectionnelle. La détection de boucle détermine si l'Agent de distribution renvoie à l'Abonné les transactions émanant de ce dernier :<br /><br /> **0** = renvoie.<br /><br /> **1** = n’est pas renvoyé.|  
|**queued_reinit**|**bit**|Indique si l'article est marqué pour l'initialisation ou la réinitialisation. La valeur **1** indique que l’article souscrit est marqué pour l’initialisation ou la réinitialisation.|  
|**nosync_type**|**tinyint**|Type d'initialisation de l'abonnement :<br /><br /> **0** = automatique (instantané)<br /><br /> **1** = prise en charge de la réplication uniquement<br /><br /> **2** = initialiser avec la sauvegarde<br /><br /> **3** = initialiser à partir du numéro séquentiel dans le journal (LSN)<br /><br /> Pour plus d’informations, consultez le paramètre **\@ sync_type** de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md).<br /><br /> **1,3** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**srvname**|**sysname**|Nom de l'Abonné.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [syssubscriptions &#40;Transact-SQL&#41;](../../relational-databases/system-tables/syssubscriptions-transact-sql.md)  
  
  
