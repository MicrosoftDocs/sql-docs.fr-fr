---
description: MSlogreader_history (Transact-SQL)
title: MSlogreader_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSlogreader_history_TSQL
- MSlogreader_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSlogreader_history system table
ms.assetid: 2e399fa1-3591-4c1c-96b7-7964fe82c7c4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ae2abdcaf4014df405ebf6dcefff8e2207530a93
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89545730"
---
# <a name="mslogreader_history-transact-sql"></a>MSlogreader_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSlogreader_history** contient des lignes d’historique pour les agents de lecture du journal associés au serveur de distribution local. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|ID de l'Agent de lecture du journal.|  
|**runstatus**|**int**|État d'exécution :<br /><br /> 1 = Démarrage.<br /><br /> 2 = Succès.<br /><br /> 3 = En cours.<br /><br /> 4 = Inactif.<br /><br /> 5 = Nouvel essai.<br /><br /> 6 = Échec.|  
|**heure-début**|**datetime**|Heure de démarrage de l'exécution de la tâche|  
|**time**|**datetime**|Heure de consignation du message dans le journal|  
|**duration**|**int**|Durée, en secondes, de la session de message.|  
|**commentaires**|**nvarchar(255)**|Texte du message.|  
|**xact_seqno**|**varbinary(16)**|Numéro de séquence de la dernière transaction réalisée.|  
|**delivery_time**|**int**|Heure à laquelle la première transaction est remise.|  
|**delivered_transactions**|**int**|Nombre total des transactions transmises dans la session.|  
|**delivered_commands**|**int**|Nombre total des commandes transmises dans la session.|  
|**average_commands**|**int**|Nombre moyen des commandes transmises dans la session.|  
|**delivery_rate**|**float**|Moyenne des commandes transmises par seconde.|  
|**delivery_latency**|**int**|Temps de latence entre l'entrée de la commande dans la base de données publiée et son entrée dans la base de données de distribution. En millisecondes.|  
|**error_id**|**int**|ID de l’erreur dans la table système **Msrepl_error** .|  
|**timestamp**|**timestamp**|Colonne timestamp de cette table|  
|**updateable_row**|**bit**|Défini sur **1** si la ligne de l’historique peut être remplacée.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
