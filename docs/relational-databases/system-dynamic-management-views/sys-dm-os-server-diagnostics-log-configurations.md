---
description: sys.dm_os_server_diagnostics_log_configurations
title: sys.dm_os_server_diagnostics_log_configurations | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_server_diagnostics_log_configurations
- sys.dm_os_server_diagnostics_log_configurations_TSQL
- dm_os_server_diagnostics_log_configurations
- dm_os_server_diagnostics_log_configurations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_os_server_diagnostics_log_configurations
- sys.dm_os_server_diagnostics_log_configurations
ms.assetid: c09ea433-d283-4f83-af69-d458aad59217
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2df0c1c899990aa123d8e46d3a8b5c6994379d26
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190134"
---
# <a name="sysdm_os_server_diagnostics_log_configurations"></a>sys.dm_os_server_diagnostics_log_configurations
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une ligne avec la configuration actuelle pour le journal de diagnostics du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ces paramètres de propriété déterminent si la journalisation des diagnostics est activée ou désactivée, et indiquent l'emplacement, le nombre et la taille des fichiers journaux.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|is_enabled|**bit**|Indique si la journalisation est activée ou désactivée.<br /><br /> 1 = La journalisation des diagnostics est activée<br /><br /> 0 = La journalisation des diagnostics est désactivée|  
|max_size|**int**|Taille maximale, en mégaoctets, que peut atteindre chacun des journaux de diagnostics. La valeur par défaut est 100 MB.|  
|max_files|**int**|Nombre maximal de fichiers journaux de diagnostics qui peuvent être stockés sur l'ordinateur avant qu'ils ne soient recyclés pour créer de nouveaux journaux de diagnostics.|  
|path|**nvarchar(260)**|Chemin d'accès qui indique l'emplacement des journaux de diagnostics. L’emplacement par défaut est \<\MSSQL\Log> dans le dossier d’installation de l’instance du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="permissions"></a>Autorisations  
 Requiert les autorisations VIEW SERVER STATE sur l'instance de cluster de basculement SQL Server.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise sys.dm_os_server_diagnostics_log_configurations pour retourner les paramètres de propriété pour les journaux de diagnostics du basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT <list of columns>  
FROM sys.dm_os_server_diagnostics_log_configurations;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|IS_ENABLED|PATH|MAX_SIZE|MAX_FILES|  
|-----------------|----------|---------------|----------------|  
|1|\<C:\Program Files\Microsoft SQL Server\MSSQL13\MSSQL\Log>|10|10|  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et lire le journal de diagnostic de l’instance de cluster de basculement](../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md)  
  
  
