---
description: syssubscriptions (Transact-SQL)
title: SYSSUBSCRIPTIONS (Transact-SQL) | Microsoft Docs
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
- syssubscriptions system table
ms.assetid: 106c1707-e0e0-49b4-ba50-25380c40fab2
author: cawrites
ms.author: chadam
ms.openlocfilehash: 724627c394fa495eaed2a5d42ea8b16de4f491fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197599"
---
# <a name="syssubscriptions-transact-sql"></a>syssubscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque abonnement de la base de données. Cette table est stockée dans la base de données de publication.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|ID unique d'un article|  
|**srvid**|**smallint**|ID de serveur de l’abonné.|  
|**dest_db**|**sysname**|Nom de la base de données de destination|  
|**statut**|**tinyint**|État de l’abonnement :<br /><br /> **0** = inactif.<br /><br /> **1** = abonné.<br /><br /> **2** = actif.|  
|**sync_type**|**tinyint**|Type de synchronisation initiale :<br /><br /> **1** = automatique.<br /><br /> **2** = aucun|  
|**login_name**|**sysname**|Nom d’accès utilisé lors de l’ajout de l’abonnement|  
|**subscription_type**|**int**|Type d’abonnement :<br /><br /> 0 = Par envoi de données (push) - l'agent de distribution s'exécute sur le serveur de distribution.<br /><br /> 1 = Par extraction de données (pull) - l'agent de distribution s'exécute sur l'Abonné.|  
|**distribution_jobid**|**Binary(16**|ID du travail de l'Agent de distribution|  
|**timestamp**|**timestamp**|horodateur.|  
|**update_mode**|**tinyint**|Mode de mise à jour :<br /><br /> **0** = lecture seule.<br /><br /> **1** = mise à jour immédiate.|  
|**loopback_detection**|**bit**|S'applique aux abonnements qui font partie d'une topologie de réplication transactionnelle bidirectionnelle. La détection de boucle détermine si l'Agent de distribution renvoie à l'Abonné les transactions émanant de ce dernier :<br /><br /> **0** = renvoie.<br /><br /> **1** = n’est pas renvoyé.|  
|**queued_reinit**|**bit**|Indique si l'article est marqué pour l'initialisation ou la réinitialisation. La valeur **1** indique que l’article souscrit est marqué pour l’initialisation ou la réinitialisation.|  
|**nosync_type**|**tinyint**|Type d'initialisation de l'abonnement :<br /><br /> **0** = automatique (instantané)<br /><br /> **1** = prise en charge de la réplication uniquement<br /><br /> **2** = initialiser avec la sauvegarde<br /><br /> **3** = initialiser à partir du numéro séquentiel dans le journal (LSN)<br /><br /> Pour plus d’informations, consultez le paramètre **\@ sync_type** de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md).|  
|**srvname**|**sysname**|Nom de l'Abonné.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
