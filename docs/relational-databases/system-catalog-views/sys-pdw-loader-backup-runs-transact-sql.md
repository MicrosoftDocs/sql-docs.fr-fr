---
description: sys.pdw_loader_backup_runs (Transact-SQL)
title: sys.pdw_loader_backup_runs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 2b72034c-6a11-46b9-a76c-7a88b2bea360
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 12fcde5ded873393a6273a43772b86376f5df607
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208369"
---
# <a name="syspdw_loader_backup_runs-transact-sql"></a>sys.pdw_loader_backup_runs (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient des informations sur les opérations de sauvegarde et de restauration en cours et terminées dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , ainsi que sur les opérations de sauvegarde, de restauration et de chargement en cours et terminées dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] . Ces informations sont conservées après le redémarrage du système.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|run_id|**int**|Identificateur unique d’une sauvegarde, d’une restauration ou d’une exécution de charge spécifique.<br /><br /> Clé pour cette vue.||  
|name|**nvarchar(255)**|NULL pour le chargement. Nom facultatif pour la sauvegarde ou la restauration.||  
|submit_time|**datetime**|Heure à laquelle la demande a été soumise.||  
|start_time|**datetime**|Heure à laquelle l'opération a commencé.||  
|end_time|**datetime**|Heure à laquelle l’opération s’est terminée, a échoué ou a été annulée.||  
|total_elapsed_time|**int**|Temps total écoulé entre start_time et l’heure actuelle, ou entre start_time et end_time pour les exécutions terminées, annulées ou ayant échoué.|Si total_elapsed_time dépasse la valeur maximale d’un entier (24,8 jours en millisecondes), cela entraînera un échec de matérialisation en raison d’un dépassement de capacité.<br /><br /> La valeur maximale en millisecondes est équivalente à 24,8 jours.|  
|operation_type|**nvarchar (16)**|Type de charge.|« BACKUP », « LOAD », « RESTORE »|  
|mode|**nvarchar (16)**|Mode dans le type d’exécution.|Pour operation_type = **sauvegarde**<br />**DIFFERENTIAL**<br />**FULL**<br /><br /> Pour operation_type = **Load**<br />**PARENTHÈSE**<br />**RECHARGER**<br />**UPSERT**<br /><br /> Pour operation_type = **Restore**<br />**DATABASE**<br />**HEADER_ONLY**|  
|database_name|**nvarchar(255)**|Nom de la base de données qui est le contexte de cette opération||  
|table_name|**nvarchar(255)**|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]||  
|Principal_id|**int**|ID de l’utilisateur qui demande l’opération.||  
|session_id|**nvarchar(32)**|ID de la session effectuant l’opération.|Consultez session_id dans [sys.dm_pdw_exec_sessions &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|request_id|**nvarchar(32)**|ID de la demande effectuant l’opération. Pour les chargements, il s’agit de la requête actuelle ou de la dernière demande associée à cette charge..|Consultez request_id dans [sys.dm_pdw_exec_requests &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|status|**nvarchar (16)**|État de l’exécution.|« CANCELLED », « COMPLETED », « FAILED », « QUEUED », « RUNNING »|  
|progress|**int**|Pourcentage effectué.|0 à 100|  
|.|**nvarchar(4000)**|Texte complet de la commande envoyée par l’utilisateur.|Sera tronqué si plus de 4000 caractères (compter les espaces).|  
|rows_processed|**bigint**|Nombre de lignes traitées dans le cadre de cette opération.||  
|rows_rejected|**bigint**|Nombre de lignes rejetées dans le cadre de cette opération.||  
|rows_inserted|**bigint**|Nombre de lignes insérées dans la ou les tables de base de données dans le cadre de cette opération.||  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue Azure Synapse Analytics et Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
