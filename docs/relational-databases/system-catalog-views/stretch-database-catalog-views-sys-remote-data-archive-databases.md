---
title: sys.remote_data_archive_databases (Transact-SQL) | Microsoft Docs
description: Découvrez comment sys.remote_data_archive_databases contient une ligne pour chaque base de données distante qui stocke des données à partir d’une base de données locale Stretch.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.remote_data_archive_databases
- sys.remote_data_archive_databases_TSQL
- remote_data_archive_databases
- remote_data_archive_databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.remote_data_archive_databases catalog view
ms.assetid: 25bffb0c-9821-40b4-88cf-75f854891a09
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 8ab3672838849626ea5f72f392ae102d8565b749
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185015"
---
# <a name="stretch-database-catalog-views---sysremote_data_archive_databases"></a>Affichages catalogue Stretch Database-sys.remote_data_archive_databases
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Contient une ligne pour chaque base de données distante qui stocke les données d’une base de données locale Stretch.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**remote_database_id**|**int**|Identificateur local généré automatiquement de la base de données distante.|  
|**remote_database_name**|**sysname**|Nom de la base de données distante.|  
|**data_source_id**|**int**|Source de données utilisée pour se connecter au serveur distant|  
  
## <a name="see-also"></a>Voir aussi  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
