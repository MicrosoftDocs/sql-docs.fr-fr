---
description: sys.dm_hadr_auto_page_repair (Transact-SQL)
title: sys.dm_hadr_auto_page_repair (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_auto_page_repair_TSQL
- sys.dm_hadr_auto_page_repair
- sys.dm_hadr_auto_page_repair_TSQL
- dm_hadr_auto_page_repair
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- automatic page repair
- sys.dm_hadr_auto_page_repair dynamic management view
ms.assetid: d7840adf-4a1b-41ac-bc94-102c07ad1c79
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 255350fdb2a2de80950575350b5082c6b7bcc6ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206780"
---
# <a name="sysdm_hadr_auto_page_repair-transact-sql"></a>sys.dm_hadr_auto_page_repair (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque tentative de réparation de page automatique sur une base de données de disponibilité sur un réplica de disponibilité hébergé pour un groupe de disponibilité quelconque par l'instance de serveur. Cette vue contient des lignes pour les tentatives de réparation de page automatique les plus récentes sur une base de données primaire ou secondaire donnée, avec un maximum de 100 lignes par base de données. Dès qu'une base de données atteint le maximum, la ligne pour sa tentative de réparation de page automatique suivante remplace l'une des entrées existantes.
  
  Le tableau suivant définit la signification des différentes colonnes :  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID de la base de données à laquelle cette ligne correspond.|  
|**file_id**|**int**|ID du fichier dans lequel la page est située.|  
|**page_id**|**bigint**|ID de la page dans le fichier.|  
|**error_type**|**int**|Type de l'erreur. Les valeurs peuvent être les suivantes :<br /><br /> **-** 1 = toutes les erreurs matérielles 823<br /><br /> 1 = Erreurs 824 autres qu'une somme de contrôle incorrecte ou une page endommagée (par exemple, ID de page incorrect)<br /><br /> 2 = Somme de contrôle incorrecte<br /><br /> 3 = Page endommagée|  
|**page_status**|**int**|État de la tentative de réparation de page :<br /><br /> 2 = En file d'attente pour demande au serveur partenaire.<br /><br /> 3 = Demande envoyée au serveur partenaire.<br /><br /> 4 = la page a été correctement réparée.<br /><br /> 5 = la page n’a pas pu être réparée lors de la dernière tentative/la réparation de page automatique va tenter de réparer à nouveau la page.|  
|**modification_time**|**datetime**|Heure de la dernière modification de l'état de page.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Réparation de page automatique &#40;groupes de disponibilité : mise en miroir de bases de données&#41;](../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [suspect_pages &#40;Transact-SQL&#41;](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
 [Gérer la table suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  

