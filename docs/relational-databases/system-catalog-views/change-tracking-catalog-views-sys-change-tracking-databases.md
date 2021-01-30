---
description: Affichages catalogue Change Tracking-sys.change_tracking_databases
title: sys.change_tracking_databases (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- change_tracking_databases
- sys.change_tracking_databases_TSQL
- sys.change_tracking_databases
- change_tracking_databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.change_tracking_databases
- change tracking [SQL Server], sys.change_tracking_databases
ms.assetid: bb233baa-2991-4904-a0eb-3772b81121a4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a0f7402689e083ecb1a6eac133a095c3b90d4bd4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193089"
---
# <a name="change-tracking-catalog-views---syschange_tracking_databases"></a>Affichages catalogue Change Tracking-sys.change_tracking_databases
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une ligne pour chaque base de données pour laquelle le suivi des modifications est activé.  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID de la base de données. Cet ID est unique dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|is_auto_cleanup_on|**bit**|Indique si les données de suivi des modifications sont nettoyées automatiquement à l'issue de la période de rétention configurée :<br /><br /> 0 = Off<br /><br /> 1 = Activé|  
|retention_period|**int**|Si le nettoyage automatique est utilisé, la période de rétention spécifie la durée pendant laquelle les données de suivi des modifications sont conservées dans la base de données.|  
|retention_period_units_desc|**nvarchar(60)**|Spécifie la description de la période de rétention :<br /><br /> Minutes<br /><br /> Heures<br /><br /> Jours|  
|retention_period_units|**tinyint**|Unité de temps de la période de rétention :<br /><br /> 1 = Minutes<br /><br /> 2 = Heures<br /><br /> 3 = Jours|  
  
## <a name="permissions"></a>Autorisations  
 Les vérifications d'autorisation effectuées pour sys.change_tracking_databases sont les mêmes que celles effectuées pour sys.databases. Si l'appelant de sys.change_tracking_databases n'est pas le propriétaire de la base de données, les autorisations minimales requises pour consulter la ligne correspondante sont des autorisations ALTER ANY DATABASE ou VIEW ANY DATABASE au niveau du serveur, ou encore l'autorisation CREATE DATABASE dans la base de données master ou la base de données active.  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue Change Tracking &#40;Transact-SQL&#41;](./catalog-views-transact-sql.md)   
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
