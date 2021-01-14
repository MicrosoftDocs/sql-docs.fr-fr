---
title: sys.dm_db_rda_migration_status (Transact-SQL) | Microsoft Docs
description: Découvrez comment sys.dm_db_rda_migration_status contient une ligne pour chaque lot de données migrées à partir de chaque table prenant en charge Stretch sur l’instance locale de SQL Server.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: language-reference
f1_keywords:
- sys.dm_db_rda_migration_status
- sys.dm_db_rda_migration_status_TSQL
- dm_db_rda_migration_status
- dm_db_rda_migration_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_rda_migration_status dynamic management view
ms.assetid: faf3901c-a0e0-4e0c-8b1b-86d9f15f34dd
author: pmasl
ms.author: pelopes
ms.openlocfilehash: eac014075edd5663b490791f6c49fc0b865afd2a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171631"
---
# <a name="stretch-database---sysdm_db_rda_migration_status"></a>Stretch Database-sys.dm_db_rda_migration_status
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Contient une ligne pour chaque lot de données migrées à partir de chaque table prenant en charge Stretch sur l’instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les lots sont identifiés par l’heure de début et l’heure de fin.  
  
 **sys.dm_db_rda_migration_status** est limitée au contexte de la base de données actuelle. Assurez-vous que vous êtes dans le contexte de la base de données des tables d’activation Stretch pour lesquelles vous souhaitez afficher l’état de la migration.  
  
 Dans [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] , la sortie de **sys.dm_db_rda_migration_status** est limitée à 200 lignes.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**table_id**|**int**|ID de la table à partir de laquelle les lignes ont été migrées.|  
|**database_id**|**int**|ID de la base de données à partir de laquelle les lignes ont été migrées.|  
|**migrated_rows**|**bigint**|Nombre de lignes migrées dans ce lot.|  
|**start_time_utc**|**datetime**|Heure UTC à laquelle le lot a démarré.|  
|**end_time_utc**|**datetime**|Heure UTC à laquelle le lot s’est terminé.|  
|**error_number**|**int**|Si le lot échoue, numéro d’erreur de l’erreur qui s’est produite ; Sinon, null.|  
|**error_severity**|**int**|Si le lot échoue, gravité de l’erreur qui s’est produite ; Sinon, null.|  
|**error_state**|**int**|Si le lot échoue, état de l’erreur qui s’est produite ; Sinon, null.<br /><br /> Le **ERROR_STATE** indique la condition ou l’emplacement où l’erreur s’est produite.|  
  
## <a name="see-also"></a>Voir aussi  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
