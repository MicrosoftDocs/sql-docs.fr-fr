---
description: MSmerge_sessions (Transact-SQL)
title: MSmerge_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_sessions
- MSmerge_sessions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_sessions system table
ms.assetid: 09ada8fc-c148-4379-9524-7826b1b0216c
author: cawrites
ms.author: chadam
ms.openlocfilehash: af57f9ae760c09fa14bffe42ceba987734b7c5ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200285"
---
# <a name="msmerge_sessions-transact-sql"></a>MSmerge_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSmerge_sessions** contient des lignes d’historique avec les résultats des sessions de travail précédentes agent de fusion. Chaque fois que l'Agent de fusion est exécuté, une nouvelle ligne est ajoutée à cette table. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID de la session de travail de l'Agent de fusion|  
|**agent_id**|**int**|ID de l'Agent de fusion|  
|**heure-début**|**datetime**|Heure de début de l'exécution du travail|  
|**heure-fin**|**datetime**|Heure de fin de l'exécution du travail|  
|**duration**|**int**|Durée cumulée (en secondes) de cette session de travail|  
|**delivery_time**|**int**|Durée (en secondes) de l'application d'un traitement de modifications|  
|**upload_time**|**int**|Durée (en secondes) du téléchargement des modifications sur le serveur de publication|  
|**download_time**|**int**|Durée (en secondes) du téléchargement des modifications sur l'Abonné|  
|**delivery_rate**|**float**|Nombre moyen des commandes transmises par seconde|  
|**time_remaining**|**int**|Estimation du nombre de secondes restant dans une session active|  
|**percent_complete**|**decimal**|Estimation du nombre total de modifications ayant déjà été transmises dans une session active|  
|**upload_inserts**|**int**|Nombre d'insertions appliquées sur le serveur de publication|  
|**upload_updates**|**int**|Nombre de mises à jour appliquées sur le serveur de publication|  
|**upload_deletes**|**int**|Nombre de suppressions appliquées sur le serveur de publication|  
|**upload_conflicts**|**int**|Nombre de conflits s'étant produits lors de l'application des modifications sur le serveur de publication|  
|**upload_conflicts_resolved**|**int**|Nombre de conflits s'étant produits lors de l'application des modifications sur le serveur de publication qui ont été résolus.|  
|**upload_rows_retried**|**int**|Nombre de lignes en cours de téléchargement sur le serveur de publication ayant fait l'objet d'une nouvelle tentative|  
|**download_inserts**|**int**|Nombre d'insertions appliquées sur l'Abonné|  
|**download_updates**|**int**|Nombre de mises à jour appliquées sur l'Abonné|  
|**download_deletes**|**int**|Nombre de suppressions appliquées sur l'Abonné|  
|**download_conflicts**|**int**|Nombre de conflits s'étant produits lors de l'application des modifications sur l'Abonné|  
|**download_conflicts_resolved**|**int**|Nombre de conflits s'étant produits lors de l'application des modifications sur l'Abonné qui ont été résolus.|  
|**download_rows_retried**|**int**|Nombre de lignes en cours de téléchargement sur l'Abonné ayant fait l'objet d'une nouvelle tentative|  
|**schema_changes**|**int**|Nombre de modifications de schéma appliquées au cours de la session|  
|**metadata_rows_cleanedup**|**int**|Nombre de lignes de métadonnées nettoyées au cours de la session|  
|**runstatus**|**int**|État d'exécution :<br /><br /> **1** = début.<br /><br /> **2** = opération réussie.<br /><br /> **3** = en cours.<br /><br /> **4** = inactif.<br /><br /> **5** = nouvelle tentative.<br /><br /> **6** = échec.|  
|**estimated_upload_changes**|**int**|Estimation du nombre de modifications devant être appliquées sur le serveur de publication|  
|**estimated_download_changes**|**int**|Estimation du nombre de modifications devant être appliquées sur l'Abonné|  
|**connection_type**|**int**|Connexion utilisée au cours du téléchargement :<br /><br /> **1** = réseau local (LAN).<br /><br /> **2** = connexion d’accès réseau à distance.<br /><br /> **3** = synchronisation Web.|  
|**timestamp**|**timestamp**|Colonne timestamp de cette table|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
