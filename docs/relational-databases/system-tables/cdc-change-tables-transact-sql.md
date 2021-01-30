---
description: cdc.change_tables (Transact-SQL)
title: cdc.change_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- cdc.change_tables
- cdc.change_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.change_tables
ms.assetid: 3525a5f5-8d8b-46a8-b334-4b7cd9fb7c21
author: cawrites
ms.author: chadam
ms.openlocfilehash: 653e1b9b7fa37e510b4ce2c50d7bc9e2e0fff78a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183353"
---
# <a name="cdcchange_tables-transact-sql"></a>cdc.change_tables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque table de modifications de la base de données. Une table de modifications est créée lorsque la capture de données modifiées est activée sur une table source. Nous vous recommandons de ne pas interroger les tables système directement. Au lieu de cela, exécutez la procédure stockée [sys.sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md) .  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de la table de modifications. Unique dans une base de données.|  
|**version**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> Pour [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], cette colonne retourne toujours 0.|  
|**source_object_id**|**int**|ID de la table source activée pour la capture des données modifiées.|  
|**capture_instance**|**sysname**|Nom de l'instance de capture utilisée pour nommer les objets de suivi spécifiques à l'instance. Par défaut, le nom est dérivé du nom de schéma source plus le nom de la table source au format *schemaname_sourcename*.|  
|**start_lsn**|**binary(10)**|Numéro séquentiel dans le journal qui représente le point de terminaison inférieur lors de la recherche des données modifiées dans la table de modifications.<br /><br /> NULL = le point de terminaison inférieur n'a pas été établi.|  
|**end_lsn**|**binary(10)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> Pour [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], cette colonne retourne toujours NULL.|  
|**supports_net_changes**|**bit**|La prise en charge de la recherche de modifications nettes est activée pour la table de modifications.|  
|**has_drop_pending**|**bit**|Le processus de capture a reçu la notification que la table source a été supprimée.|  
|**role_name**|**sysname**|Nom du rôle de base de données utilisé pour la passerelle pour l’accès aux données modifiées.<br /><br /> NULL = aucun rôle n'est utilisé.|  
|**index_name**|**sysname**|Nom de l'index utilisé pour identifier de façon unique des lignes dans la table source. **index_name** est le nom de l’index de clé primaire de la table source ou le nom d’un index unique spécifié lorsque la capture de données modifiées a été activée sur la table source.<br /><br /> NULL = la table source n'avait pas de clé primaire lorsque la capture des données modifiées a été activée et aucun index unique n'a été spécifié quand la capture des données modifiées a été activée.<br /><br /> Remarque : si la capture de données modifiées est activée sur une table où une clé primaire existe, la fonctionnalité de capture de données modifiées utilise l’index, que les modifications nettes soient activées ou non. Après l'activation de la capture des données modifiées, aucune modification de la clé primaire n'est autorisée. S'il n'y a aucune clé primaire sur la table, vous pouvez tout de même activer la capture des données modifiées, mais uniquement avec les modifications nettes définies à « faux ». Une fois la capture des données modifiées activée, vous pouvez créer une clé primaire. Vous pouvez également modifier la clé primaire car la capture des données modifiées n'utilise pas la clé primaire.|  
|**filegroup_name**|**sysname**|Nom du groupe de fichiers qui contient la table de modifications.<br /><br /> NULL = la table de modifications se trouve dans le groupe de fichiers par défaut de la base de données.|  
|**create_date**|**datetime**|Date d'activation de la table source.|  
|**partition_switch**|**bit**|Indique si la commande **switch partition** d' **ALTER TABLE** peut être exécutée sur une table qui est activée pour la capture de données modifiées. 0 indique que le basculement de partition est bloqué. Les tables non partitionnées retournent toujours la valeur 1.|  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)  
  
  
