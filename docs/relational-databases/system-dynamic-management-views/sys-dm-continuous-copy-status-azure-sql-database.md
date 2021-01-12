---
description: sys.dm_continuous_copy_status (Azure SQL Database)
title: sys.dm_continuous_copy_status
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_continuous_copy_status_TSQL
- dm_continuous_copy_status_TSQL
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
dev_langs:
- TSQL
helpviewer_keywords:
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
ms.assetid: 411b2e71-4421-4ef5-900d-5af068750899
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 94370b562160315e53feb46a83cbb68a848e064f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099962"
---
# <a name="sysdm_continuous_copy_status-azure-sql-database"></a>sys.dm_continuous_copy_status (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Retourne une ligne pour chaque base de données utilisateur (v11) qui est actuellement engagée dans une relation de copie continue de géo-réplication. Si plusieurs relations de copie continue sont initialisées pour une base de données primaire donnée, cette table contient une ligne pour chaque base de données secondaire active.  
  
Si vous utilisez SQL Database V12, vous devez utiliser [sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) (car *sys.dm_continuous_copy_status* s’applique uniquement à v11).

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**copy_guid**|**uniqueidentifier**|ID unique de la base de données réplica.|  
|**partner_server**|**sysname**|Nom du serveur SQL Database lié.|  
|**partner_database**|**sysname**|Nom de la base de données liée sur le serveur SQL Database lié.|  
|**last_replication**|**datetimeoffset**|Horodateur de la dernière transaction dupliquée appliquée.|  
|**replication_lag_sec**|**int**|Décalage horaire en secondes entre l'heure actuelle et l'horodateur de la dernière transaction validée dans la base de données primaire qui n'a pas été acceptée par la base de données secondaire active.|  
|**replication_state**|**tinyint**|État de la réplication de copie continue pour cette base de données. Voici les valeurs possibles et leurs descriptions.<br /><br /> 1 : amorçage. La cible de réplication est amorcée et est dans un état incohérent d'un point de vue transactionnel. Tant que l'amorçage n'est pas terminé, vous ne pouvez pas vous connecter à la base de données secondaire active. <br />2 : rattrapage. La base de données secondaire active rattrape la base de données primaire et est dans un état cohérent d'un point de vue transactionnel.<br />3 : réamorçage. La base de données secondaire active est automatiquement réamorcée, en raison d'une erreur irrécupérable de réplication.<br />4 : suspendu. Il ne s'agit pas d'une relation de copie continue active. Cet état indique généralement que la bande passante disponible pour l'interlien est insuffisante pour le niveau d'activité de transaction dans la base de données primaire. Toutefois, la relation de copie continue est toujours intacte.|  
|**replication_state_desc**|**nvarchar (256)**|Description de replication_state :<br /><br /> SEEDING<br /><br /> CATCH_UP<br /><br /> RE_SEEDING<br /><br /> SUSPENDED|  
|**is_rpo_limit_reached**|**bit**|A toujours pour valeur 0.|  
|**is_target_role**|**bit**|0 = Source de la relation de copie<br /><br /> 1 = Cible de la relation de copie|  
|**is_interlink_connected**|**bit**|1 = L'interlien est connecté.<br /><br /> 0 = L'interlien est déconnecté.|  
  
## <a name="permissions"></a>Autorisations  
 Pour récupérer des données, nécessite l’appartenance au rôle de base de données **db_owner** . L’utilisateur dbo, les membres du rôle de base de données **dbmanager** et la connexion sa peuvent également interroger cette vue.  
  
## <a name="remarks"></a>Notes  
 La vue **sys.dm_continuous_copy_status** est créée dans la base de données **Resource** et est visible dans toutes les bases de données, y compris la base de données Master logique. Cependant, l'interrogation de cette vue dans la base de données master logique retourne un ensemble vide.  
  
 Si la relation de copie continue se termine sur une base de données, la ligne de cette base de données dans la vue **sys.dm_continuous_copy_status** disparaît.  
  
 À l’instar de la vue **sys.dm_database_copies** , **sys.dm_continuous_copy_status** reflète l’état de la relation de copie continue dans laquelle la base de données est une base de données primaire ou secondaire active. Contrairement à **sys.dm_database_copies**, **sys.dm_continuous_copy_status** contient plusieurs colonnes qui fournissent des détails sur les opérations et les performances. Ces colonnes incluent **last_replication** et **replication_lag_sec**.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_database_copies &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)   
 [Procédures stockées active Geo-Replication &#40;Transact-SQL&#41;](../system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
