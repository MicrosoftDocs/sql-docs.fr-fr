---
description: sys.dm_database_copies (Azure SQL Database)
title: sys.dm_database_copies (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- dm_database_copies_TSQL
- sys.dm_database_copies
- dm_database_copies
- sys.dm_database_copies_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_database_copies
- sys.dm_database_copies
ms.assetid: d03d4657-86d1-4496-97e6-cc3bc292e0b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: a9a6e570b6c535620152f49947b4d4e289a6ff2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195099"
---
# <a name="sysdm_database_copies-azure-sql-database"></a>sys.dm_database_copies (Azure SQL Database)
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Retourne des informations sur une copie de base de données.  
  
Pour renvoyer des informations sur les liens de géo-réplication, utilisez les vues [sys.geo_replication_links](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md) ou [sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) (disponibles dans SQL Database V12).
  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de la base de données active dans la vue `sys.databases`.|  
|**start_date**|**datetimeoffset**|Heure UTC dans un centre de données [!INCLUDE[ssSDS](../../includes/sssds-md.md)] local lorsque la copie de la base de données a été initialisée.|  
|**modify_date**|**datetimeoffset**|Heure UTC dans un centre de données [!INCLUDE[ssSDS](../../includes/sssds-md.md)] local lorsque la copie de la base de données s'est terminée. La nouvelle base de données est cohérente d'un point de vue transactionnel avec la base de données primaire à compter de ce moment. Les informations de saisie semi-automatique sont mises à jour toutes les minutes.<br /><br />Heure UTC reflétant la dernière mise à jour du champ percent_complete.|  
|**percent_complete**|**real**|Pourcentage des octets copiés. Les valeurs valides sont comprises entre 0 et 100. [!INCLUDE[ssSDS](../../includes/sssds-md.md)] résout automatiquement certaines erreurs, telles qu'un basculement, et redémarre la copie de base de données. Dans ce cas, percent_complete redémarre à 0.|  
|**error_code**|**int**|Lorsque la valeur est supérieure à 0, le code indique une erreur qui s'est produite lors de la copie. Si la valeur est égale à 0, aucune erreur ne s'est produite.|  
|**error_desc**|**nvarchar (4096)**|Description de l'erreur qui s'est produite lors de la copie.|  
|**error_severity**|**int**|Retourne 16, si la copie de la base de données a échoué.|  
|**error_state**|**int**|Retourne 1, si la copie a échoué.|  
|**copy_guid**|**uniqueidentifier**|ID unique de l’opération de copie.|  
|**partner_server**|**sysname**|Nom du serveur SQL Database sur lequel la copie est créée.|  
|**partner_database**|**sysname**|Nom de la copie de la base de données sur le serveur partenaire.|  
|**replication_state**|**tinyint**|État de la réplication de copie continue pour cette base de données. Les valeurs sont les suivantes :<br /><br /> 0 = en attente. La création de la copie de la base de données est planifiée, mais les étapes de préparation nécessaires ne sont pas encore terminées ou sont temporairement bloquées par le quota d’amorçage.<br /><br /> 1 = amorçage. La base de données de copie en cours d’amorçage n’est pas encore entièrement synchronisée avec la base de données source. Dans cet État, vous ne pouvez pas vous connecter à la copie. Pour annuler l’opération d’amorçage en cours, vous devez supprimer la base de données de copie.|  
|**replication_state_desc**|**nvarchar (256)**|Description de replication_state :<br /><br /> PENDING<br /><br /> SEEDING<br />|  
|**maximum_lag**|**int**|Champ réservé.|  
|**is_continuous_copy**|**bit**|0 = retourne 0|  
|**is_target_role**|**bit**|0 = base de données source<br /><br /> 1 = copier la base de données|  
|**is_interlink_connected**|bit|Champ réservé.|  
|**is_offline_secondary**|bit|Champ réservé.|  
  
## <a name="permissions"></a>Autorisations  
 Cette vue est disponible uniquement dans la base de données **Master** à la connexion du principal au niveau du serveur.  
  
## <a name="remarks"></a>Notes  
 Vous pouvez utiliser la vue **sys.dm_database_copies** dans la base de données **Master** du serveur source ou cible [!INCLUDE[ssSDS](../../includes/sssds-md.md)] . Lorsque la copie de base de données s'achève avec succès et que la nouvelle base de données devient ONLINE, la ligne dans la vue **sys.dm_database_copies** est supprimée automatiquement.  
  
  
