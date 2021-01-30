---
description: sysmergesubscriptions (Transact-SQL)
title: sysmergesubscriptions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergesubscriptions_TSQL
- sysmergesubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergesubscriptions system table
ms.assetid: 6adc78da-991d-4c08-98c3-ecb4762e0e99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 30b7f652f0bb85ab0c2bdf56ebf50090699c476e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184692"
---
# <a name="sysmergesubscriptions-transact-sql"></a>sysmergesubscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque Abonné connu et constitue une table locale sur le serveur de publication. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|subscriber_server|**sysname**|ID du serveur. Permet de mapper le champ srvid avec la valeur propre au serveur lors de la migration d'une copie de la base de données d'abonnement vers un autre serveur.|  
|db_name|**sysname**|Nom de la base de données d’abonnement.|  
|pubid|**uniqueidentifier**|ID de la publication à partir de laquelle l'abonnement actuel a été créé.|  
|datasource_type|**int**|Type de source de données :<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> **2** = OLE DB jet.|  
|subid|**uniqueidentifier**|Numéro d'identification unique de l'abonnement.|  
|replnickname|**binary**|Surnom compressé du réplica.|  
|replicastate|**uniqueidentifier**|Identificateur unique utilisé pour déterminer si la synchronisation précédente a réussi en comparant la valeur sur l'Éditeur à celle sur l'Abonné.|  
|status|**tinyint**|État de l’abonnement :<br /><br /> **0** = inactif.<br /><br /> **1** = actif.<br /><br /> **2** = supprimé.|  
|subscriber_type|**int**|Type d’abonné :<br /><br /> **1** = global.<br /><br /> **2** = local.<br /><br /> **3** = anonyme.|  
|subscription_type|**int**|Type d’abonnement :<br /><br /> **0** = push.<br /><br /> **1** = pull.<br /><br /> **2** = anonyme.|  
|sync_type|**tinyint**|Type de synchronisation :<br /><br /> **1** = automatique.<br /><br /> **2** = aucune synchronisation.|  
|description|**nvarchar(255)**|Brève description de l'abonnement.|  
|priority|**real**|Indique la priorité de l'abonnement et permet l'implémentation d'utilitaires de résolution de conflits basée sur les priorités. Est égal à **0,00** pour tous les abonnements locaux ou anonymes.|  
|recgen|**bigint**|Numéro de la dernière génération reçue.|  
|recguid|**uniqueidentifier**|ID unique de la dernière génération reçue.|  
|sentgen|**bigint**|Numéro de la dernière génération envoyée.|  
|sentguid|**uniqueidentifier**|ID unique de la dernière génération envoyée.|  
|schemaversion|**int**|Numéro du dernier schéma reçu.|  
|schemaguid|**uniqueidentifier**|ID unique du dernier schéma reçu.|  
|last_validated|**datetime**|**Date et heure** de la dernière validation réussie des données de l’abonné.|  
|attempted_validate|**datetime**|Dernière **date/heure** à laquelle la validation a été tentée sur l’abonnement.|  
|last_sync_date|**datetime**|**Date et heure** de la synchronisation.|  
|last_sync_status|**int**|État de l'abonnement :<br /><br /> **0** = tous les travaux sont en attente de démarrage.<br /><br /> **1** = un ou plusieurs travaux sont en cours de démarrage.<br /><br /> **2** = toutes les tâches ont été exécutées avec succès.<br /><br /> **3** = au moins un travail est en cours d’exécution.<br /><br /> **4** = tous les travaux sont planifiés et inactifs.<br /><br /> **5** = au moins un travail tente de s’exécuter après un échec précédent.<br /><br /> **6** = au moins un travail n’a pas réussi à s’exécuter correctement.|  
|last_sync_summary|**sysname**|Description des résultats de la dernière synchronisation.|  
|metadatacleanuptime|**datetime**|Le dernier **DateTime** qui a expiré les métadonnées a été supprimé des tables système de réplication de fusion.|  
|partition_id|**int**|Identifie la partition précalculée à laquelle appartient l'abonnement.|  
|cleanedup_unsent_changes|**bit**|Indique que les métadonnées relatives aux modifications non envoyées ont été nettoyées sur l'Abonné.|  
|replica_version|**int**|Identifie la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de l'abonné auquel appartient l'abonnement. Les valeurs possibles sont :<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|supportability_mode|**int**|À usage interne uniquement|  
|application_name|**nvarchar(128)**|À usage interne uniquement|  
|subscriber_number|**int**|À usage interne uniquement|  
|last_makegeneration_datetime|**datetime**|Dernière **date/heure** d’exécution du processus makegeneration pour le serveur de publication. Pour plus d’informations, consultez le paramètre-MakeGenerationInterval dans [agent de fusion de réplication](../../relational-databases/replication/agents/replication-merge-agent.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
