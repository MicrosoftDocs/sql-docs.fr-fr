---
description: sys.database_event_session_fields (Azure SQL Database)
title: sys. database_event_session_fields (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
ms.assetid: 9b5c94d6-612c-4e0f-976d-ac6ba55da3ac
author: CarlRabeler
ms.author: carlrab
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7c0116d1a1c308172a32e37c2b8ee1b59630af22
ms.sourcegitcommit: 331b8495e4ab37266945c81ff5b93d250bdaa6da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88646345"
---
# <a name="sysdatabase_event_session_fields-azure-sql-database"></a>sys.database_event_session_fields (Azure SQL Database)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Retourne une ligne pour chaque colonne personnalisable définie explicitement sur les événements et les cibles.  
  
||  
|-|  
|**S’applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 et à toutes les versions ultérieures.|  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|ID de la session d'événements. N'accepte pas la valeur NULL.|  
|object_id|**int**|ID de l'objet auquel est associé ce champ. N'accepte pas la valeur NULL.|  
|name|**sysname**|Nom du champ. N'accepte pas la valeur NULL.|  
|value|**sql_variant**|Valeur du champ. N'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DATABASE STATE sur le serveur.  
  
## <a name="remarks"></a>Notes  
 Cette vue a les cardinalités de relation suivantes.  
  
| Du | À | Relation |
| ---- | -- | ------------ |
|sys. database_event_session_actions. event_session_id|sys. database_event_sessions. event_session_id|Plusieurs-à-un|  
|sys. database_event_session_actions. event_id<br /><br /> sys. database_event_session_actions. object_id<br /><br /> sys. database_event_session_actions. event_session_id|sys. database_event_session_events. event_session_id<br /><br /> sys. database_event_session_events. event_id|Plusieurs-à-un|  
|sys. database_event_session_actions. event_session_id<br /><br /> sys. database_event_session_actions. object_id|sys. database_event_session_targets. event_session_id<br /><br /> sys. database_event_session_targets. target_id|Plusieurs-à-un|  
  
## <a name="see-also"></a>Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  
  
