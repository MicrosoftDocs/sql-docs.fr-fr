---
description: sys.service_queues (Transact-SQL)
title: sys. service_queues (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.service_queues
- service_queues
- service_queues_TSQL
- sys.service_queues_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_queues catalog view
ms.assetid: 9fd9fa76-6128-410c-896f-741e6050143a
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: ded21f580d0243f6c1343af675497d91308d4a16
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88475259"
---
# <a name="sysservice_queues-transact-sql"></a>sys.service_queues (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque objet de la base de données qui est une file d’attente de service, avec **sys. Objects. type** = sq.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Pour obtenir la liste des colonnes héritées par cette vue, consultez [sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**max_readers**|**smallint**|Nombre maximum de lecteurs simultanés autorisés dans la file d'attente.|  
|**activation_procedure**|**nvarchar(776**|Nom en trois parties de la procédure d'activation.|  
|**execute_as_principal_id**|**int**|ID du principal de base de données EXECUTE AS.<br /><br /> Valeur NULL par défaut ou dans le cas de l'instruction EXECUTE AS CALLER.<br /><br /> ID du principal spécifié si EXECUTe AS SELF EXECUTe AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
|**is_activation_enabled**|**bit**|1 = l'activation est activée.|  
|**is_receive_enabled**|**bit**|1 = la réception est activée.|  
|**is_enqueue_enabled**|**bit**|1 = l'empilement est activé.|  
|**is_retention_enabled**|**bit**|1 = les messages sont conservés jusqu'à la fin du dialogue.|  
|**is_poison_message_handling_enabled**|**bit**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> 1 = la gestion des messages incohérents est activée.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
