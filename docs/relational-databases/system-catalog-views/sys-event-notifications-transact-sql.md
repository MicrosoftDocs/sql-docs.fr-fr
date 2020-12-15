---
description: sys.event_notifications (Transact-SQL)
title: sys.event_notifications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- event_notifications_TSQL
- event_notifications
- sys.event_notifications_TSQL
- sys.event_notifications
dev_langs:
- TSQL
helpviewer_keywords:
- sys.event_notifications catalog view
ms.assetid: 136a76ee-2b35-4418-ab46-fda2d51f7d99
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fa6f06df580b7f76309660a87e24ae458bf1c2d5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467040"
---
# <a name="sysevent_notifications-transact-sql"></a>sys.event_notifications (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une ligne pour chaque objet qui est une notification d’événement, avec **sys. Objects. type** = en.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de la notification d'événement|  
|**object_id**|**int**|Numéro d'identification de l'objet. Unique dans une base de données.|  
|**parent_class**|**tinyint**|Classe du parent.<br /><br /> 0 = Base de données<br /><br /> 1 = objet ou colonne|  
|**parent_class_desc**|**nvarchar(60)**|DATABASE<br /><br /> OBJECT_OR_COLUMN|  
|**parent_id**|**int**|ID non NULL de l'objet parent<br /><br /> 0 = La classe parent est la base de données.|  
|**create_date**|**datetime**|Date de création.|  
|**modify_date**|**datetime**|Est toujours égal à **create_date**.|  
|**service_name**|**nvarchar (256)**|Nom du service cible auquel la notification est envoyée.|  
|**broker_instance**|**nvarchar(128)**|Instance du broker auquel la notification est envoyée.|  
|**principal_id**|**int**|ID du principal de base de données propriétaire de cette notification d'événement|  
|**creator_sid**|**varbinary(85)**|SID de la connexion qui a créé la notification d'événement.<br /><br /> A la valeur NULL si l'option FAN_IN n'est pas spécifiée.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
