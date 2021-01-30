---
description: MSmerge_partition_groups (Transact-SQL)
title: MSmerge_partition_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_partition_groups_TSQL
- MSmerge_partition_groups
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_partition_groups system table
ms.assetid: 5d56d780-ee40-4afc-9c2a-d1723d86e430
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1cd5fb30418a16a03f67b2ef574d8748448212f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205934"
---
# <a name="msmerge_partition_groups-transact-sql"></a>MSmerge_partition_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSmerge_partition_groups** stocke une ligne pour chaque partition précalculée dans une base de données donnée. Outre les colonnes répertoriées, une colonne est ajoutée à cette table pour chaque fonction utilisée dans un filtre de lignes paramétrable. Par exemple, une colonne nommée **HOST_NAME_FN** est ajoutée à la table si un filtre utilise la fonction [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) . Une ligne est stockée pour chaque jeu unique de valeurs de fonction synchronisées avec ce serveur de publication. Au moins deux abonnés qui se synchronisent avec exactement la même valeur pour toutes ces fonctions partagent la même ligne dans cette table et, par conséquent, partagent tous le même ID de partition. Cette table est stockée dans la base de données de publication.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**partition_id**|**int**|Colonne d'identité fournissant un numéro d'identification unique pour la partition précalculée.|  
|**publication_number**|**smallint**|Numéro de publication, qui est stocké dans **sysmergepublications**.|  
|**maxgen_whenadded**|**bigint**|Génération la plus élevée connue sur le serveur de publication au moment de l'insertion de la ligne dans cette table.|  
|**using_partition_groups**|**bit**|Indique si la partition appartient à une publication qui utilise des partitions précalculées. Peut être l'une des valeurs suivantes :<br /><br /> **0** = la publication n’utilise pas de partitions précalculées.<br /><br /> **1** = la publication utilise des partitions précalculées<br /><br /> Pour plus d’informations, consultez [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).|  
|**HOST_NAME_FN**|**nvarchar(128)**|Valeur fournie lors de l'utilisation du filtre de lignes paramétrable en vue de générer des partitions. Pour plus d'informations, voir [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
