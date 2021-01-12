---
description: Affichages catalogue Change Tracking-sys.change_tracking_tables
title: sys.change_tracking_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- change_tracking_tables_TSQL
- sys.change_tracking_tables
- change_tracking_tables
- sys.change_tracking_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- change tracking [SQL Server], sys.change_tracking_tables
- sys.change_tracking_tables
ms.assetid: 97ec69b6-0d49-4d98-82f0-d3e77ba1ad2b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a4f0a4ef53b1225efee4ed1c382a3cf012bac02
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093235"
---
# <a name="change-tracking-catalog-views---syschange_tracking_tables"></a>Affichages catalogue Change Tracking-sys.change_tracking_tables
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une seule ligne pour chaque table incluse dans la base de données active pour laquelle le suivi des modifications est activé.  
   
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID d'une table ayant un journal des modifications. La table peut avoir un journal des modifications même si le suivi des modifications est actuellement désactivé.<br /><br /> L'ID de la table est unique dans la base de données.|  
|is_track_columns_updated_on|**bit**|État actuel du suivi des modifications sur la table :<br /><br /> 0 = désactivé<br /><br /> 1 = activé|  
|begin_version|**bigint**|Version de la base de données au moment où le suivi des modifications a commencé pour la table. Cette version indique habituellement le moment de l'activation du suivi des modifications, mais cette valeur est réinitialisée si la table est tronquée.|  
|cleanup_version|**bigint**|Version jusqu'à laquelle le nettoyage peut avoir supprimé des informations de suivi des modifications.|  
|min_valid_version|**bigint**|Version valide minimale des informations de suivi des modifications disponible pour la table.<br /><br /> Lors de l'obtention des modifications de la table associée à cette ligne, la valeur de last_sync_version doit être supérieure ou égale à la version indiquée par cette colonne. Pour plus d’informations, consultez [CHANGE_TRACKING_MIN_VALID_VERSION &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md).|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [CHANGE_TRACKING_MIN_VALID_VERSION &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md)   
 [Affichages catalogue Change Tracking &#40;Transact-SQL&#41;](./catalog-views-transact-sql.md)   
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
