---
description: sys.database_event_session_targets (Azure SQL Database)
title: sys.database_event_session_targets (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
ms.assetid: 38d775ee-1fe1-4820-88c6-02b2f875a66b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2b77a6340fe245884c137fb24fbeac653a8a2ff0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97458513"
---
# <a name="sysdatabase_event_session_targets-azure-sql-database"></a>sys.database_event_session_targets (Azure SQL Database)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Retourne une ligne pour chaque cible d'événement d'une session d'événements.  
  
||  
|-|  
|**S’applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 et à toutes les versions ultérieures.|  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|ID de la session d'événements. N'accepte pas la valeur NULL.|  
|target_id|**int**|ID de la cible. Cet ID est unique au sein de l'objet de la session d'événements. N'accepte pas la valeur NULL.|  
|name|**sysname**|Nom de la cible d'événement. N'accepte pas la valeur NULL.|  
|package|**sysname**|Nom du package d'événement qui contient la cible d'événement. N'accepte pas la valeur NULL.|  
|module|**sysname**|Nom du module qui contient la cible d'événement. N'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DATABASE STATE sur le serveur.  
  
## <a name="remarks"></a>Remarks  
 Cette vue a les cardinalités de relation suivantes.  
  
|Du|À|Relationship|  
|-|-|-|  
|sys.database_event_session_targets sys.database_event_session_targets.event_session_id|sys.database_event_sessions sys.database_event_sessions.event_session_id|Plusieurs-à-un|  
  
## <a name="see-also"></a>Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  
  
