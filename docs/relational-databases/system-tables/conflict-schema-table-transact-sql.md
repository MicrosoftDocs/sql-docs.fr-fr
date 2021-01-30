---
description: '&lt;table conflict_ Schema &gt; _ &lt; &gt; (Transact-SQL)'
title: '&lt;table conflict_ Schema &gt; _ &lt; &gt; (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 01/15/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- conflict_
- conflict_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- conflict_<schema>_<table>
ms.assetid: 15ddd536-db03-454e-b9b5-36efe1f756d7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2b578a5b95dfae78b79216343f32bd7e5ea97d5c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204713"
---
# <a name="conflict_ltschemagt_lttablegt-transact-sql"></a>&lt;table conflict_ Schema &gt; _ &lt; &gt; (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La \<schema> table conflict_ _ \<table> contient des informations sur les lignes en conflit dans la réplication d’égal à égal. Il existe une table de conflits pour chaque table répliquée dans une publication, où le nom de la table de conflits est ajouté au nom du schéma et à celui de l'article. Ces tables de conflits spécifiques aux articles existent dans chaque base de données de publication.  
  
 Pour la réplication d'égal à égal, par défaut, l'Agent de distribution échoue lorsqu'il détecte un conflit. Une erreur de conflit est enregistrée dans le journal des erreurs, mais aucune donnée de conflit n'est enregistrée dans la table de conflits ; par conséquent, la consultation de cette erreur n'est pas possible. Si l'Agent de distribution est autorisé à continuer, un conflit est enregistré localement sur chaque nœud où il a été détecté. Pour plus d'informations, consultez « Gestion des conflits » dans [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|__$originator_id|**int**|ID du nœud d'où provient la modification conflictuelle Pour obtenir la liste des ID, exécutez [sp_help_peerconflictdetection](../../relational-databases/system-stored-procedures/sp-help-peerconflictdetection-transact-sql.md).|  
|__$origin_datasource|**int**|Nœud d'où provient la modification conflictuelle.|  
|__$tranid|**nvarchar (40)**|Numéro séquentiel dans le journal (LSN) de la modification en conflit appliquée à __$origin_datasource.|  
|__$conflict_type|**int**|Type de conflit qui s'est produit, qui peut être l'une des valeurs suivantes :<br /><br /> 1 : Une mise à jour a échoué parce que la ligne locale a été modifiée par une autre mise à jour ou elle a été supprimée puis réinsérée.<br /><br /> 2 : Une mise à jour a échoué parce que la ligne locale a déjà été supprimée.<br /><br /> 3 : Une suppression a échoué parce que la ligne locale a été modifiée par une autre mise à jour ou elle a été supprimée puis réinsérée.<br /><br /> 4 : Une suppression a échoué parce que la ligne locale a déjà été supprimée.<br /><br /> 5 : Une insertion a échoué parce que la ligne locale a déjà été insérée ou a été insérée puis mise à jour.|  
|__$is_winner|**bit**|Indique si la ligne dans cette table était le vainqueur du conflit, ce qui signifie qu'elle a été appliquée au nœud local.|  
|__$pre_version|**varbinary (32)**|Version de la base de données d'où provient la modification conflictuelle.|  
|__$reason_code|**int**|Code de résolution du conflit. Peut avoir l’une des valeurs suivantes :<br /><br /> 0<br /><br /> 1<br /><br /> 2<br /><br /> <br /><br /> Pour plus d’informations, consultez **_ _ $ reason_text**.|  
|__$reason_text|**nvarchar (720)**|Résolution du conflit. Peut avoir l’une des valeurs suivantes :<br /><br /> Résolu (1)<br /><br /> Non résolu (2)<br /><br /> Inconnu (0)|  
|__$update_bitmap|**varbinary (** *n* **)**. La taille varie en fonction du contenu.|Bitmap qui indique quelles colonnes ont été mises à jour en cas de conflit mise à jour-mise à jour.|  
|__$inserted_date|**datetime**|Date et heure d'insertion de la ligne en conflit dans cette table.|  
|__$row_id|**timestamp**|Valeur de version associée à la ligne source du conflit.|  
|__$change_id|**binaire (8)**|Pour une ligne locale, cette valeur est égale au __$row_id de la ligne entrante qui a été en conflit avec la ligne locale. Cette valeur est NULL pour une ligne entrante.|  
|\<base table column names>|\<base table column types>|La table en conflit contient une colonne pour chaque colonne dans la table de base.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
