---
description: sysmergepartitioninfo (Transact-SQL)
title: sysmergepartitioninfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergepartitioninfo_TSQL
- sysmergepartitioninfo
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepartitioninfo system table
ms.assetid: 7429ad2c-dd33-4f7d-89cc-700e083af518
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8a94f14212f2603639c1d272da4d35e9bea9fb29
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100491"
---
# <a name="sysmergepartitioninfo-transact-sql"></a>sysmergepartitioninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit des informations sur les partitions de chaque article. Contient une ligne pour chaque article de fusion défini dans la base de données locale. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**artid**|**uniqueidentifier**|Numéro d'identification unique de l'article donné.|  
|**pubid**|**uniqueidentifier**|Numéro d'identification unique de la publication, généré lors de l'ajout de la publication|  
|**partition_view_id**|**int**|ID de la vue de partition sur cette table. La vue montre un mappage de chaque ligne de l'article avec les différents ID de partition auxquels elle appartient.|  
|**repl_view_id**|**int**|À ajouter.|  
|**partition_deleted_view_rule**|**nvarchar(4000)**|Instruction SQL utilisée dans un déclencheur de réplication de fusion pour extraire l'ID de partition de chaque ligne supprimée ou mise à jour en fonction de ses anciennes valeurs de colonne.|  
|**partition_inserted_view_rule**|**nvarchar(4000)**|Instruction SQL utilisée dans un déclencheur de réplication de fusion pour extraire l'ID de partition de chaque ligne insérée ou mise à jour en fonction de ses nouvelles valeurs de colonne.|  
|**membership_eval_proc_name**|**sysname**|Nom de la procédure qui évalue les ID de partition actuels des lignes dans **MSmerge_contents**.|  
|**column_list**|**nvarchar(4000)**|Liste séparée par des virgules des colonnes répliquées dans un article.|  
|**column_list_blob**|**nvarchar(4000)**|Liste séparée par des virgules des colonnes répliquées dans un article, y compris les colonnes BLOB (Binary Large Object).|  
|**expand_proc**|**sysname**|Nom de la procédure qui réévalue les ID de partition de toutes les lignes enfants d'une ligne parente nouvellement insérée et des lignes parentes supprimées ou soumises à une modification de partition.|  
|**logical_record_parent_nickname**|**int**|Surnom du parent de niveau supérieur d'un article donné dans un enregistrement logique.|  
|**logical_record_view**|**int**|Vue qui génère la colonne rowguid d'article de parent de niveau supérieur correspondant à chaque colonne rowguid enfant.|  
|**logical_record_deleted_view_rule**|**nvarchar(4000)**|Semblable à **logical_record_view**, sauf qu’elle affiche des lignes enfants dans la table « Deleted » dans les déclencheurs Update et DELETE.|  
|**logical_record_level_conflict_detection**|**bit**|Indique si les conflits doivent être détectés au niveau des enregistrements logiques ou au niveau des lignes ou des colonnes.<br /><br /> **0** = la détection de conflit au niveau des lignes ou des colonnes est utilisée.<br /><br /> **1** = la détection de conflit d’enregistrement logique est utilisée, où une modification d’une ligne sur le serveur de publication et la modification d’une ligne distincte le même enregistrement logique sur l’abonné sont gérées en tant que conflit.<br /><br /> Lorsque cette valeur est **1**, seule la résolution des conflits au niveau des enregistrements logiques peut être utilisée.|  
|**logical_record_level_conflict_resolution**|**bit**|Indique si les conflits doivent être résolus au niveau des enregistrements logiques ou au niveau des lignes ou des colonnes.<br /><br /> **0** = la résolution au niveau des lignes ou des colonnes est utilisée.<br /><br /> **1** = en cas de conflit, la totalité de l’enregistrement logique du gagnant remplace l’ensemble de l’enregistrement logique du côté perdant.<br /><br /> La valeur **1** peut être utilisée avec la détection au niveau des enregistrements logiques et avec la détection au niveau des lignes ou des colonnes.|  
|**partition_options**|**tinyint**|Définit le mode de partitionnement des données de l'article, ce qui permet l'optimisation des performances lorsque toutes les lignes appartiennent à une seule partition ou à un seul abonnement. *partition_options* peut prendre l’une des valeurs suivantes.<br /><br /> **0** = le filtrage de l’article est statique ou ne génère pas un sous-ensemble unique de données pour chaque partition, c’est-à-dire une partition « qui se chevauche ».<br /><br /> **1** = les partitions se chevauchent, et les mises à jour DML effectuées sur l’abonné ne peuvent pas modifier la partition à laquelle une ligne appartient.<br /><br /> **2** = le filtrage de l’article génère des partitions qui ne se chevauchent pas, mais plusieurs abonnés peuvent recevoir la même partition.<br /><br /> **3** = le filtrage de l’article génère des partitions qui ne se chevauchent pas et qui sont uniques pour chaque abonnement.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
