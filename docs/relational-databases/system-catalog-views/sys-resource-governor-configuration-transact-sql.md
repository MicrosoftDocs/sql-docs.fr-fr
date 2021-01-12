---
description: sys.resource_governor_configuration (Transact-SQL)
title: sys.resource_governor_configuration (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.resource_governor_configuration_TSQL
- sys.resource_governor_configuration
- resource_governor_configuration_TSQL
- resource_governor_configuration
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_configuration catalog view
ms.assetid: 89099668-1dc6-4b07-9d8b-49bc95c7bfc0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 34e8a269a2c8e7fafd0ba0e6645e2363eefa5d77
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097968"
---
# <a name="sysresource_governor_configuration-transact-sql"></a>sys.resource_governor_configuration (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne l'état du gouverneur de ressources stocké.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|classifier_function_id|**int**|ID de la fonction classifieur tel qu'il est stocké dans les métadonnées. N'accepte pas la valeur NULL.<br /><br /> **Remarque** Cette fonction est utilisée pour classer de nouvelles sessions et utilise des règles pour acheminer la charge de travail vers le groupe de charge de travail approprié. Pour plus d’informations, consultez [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).|  
|is_enabled|**bit**|Indique l'état actuel du gouverneur de ressources :<br /><br /> 0 = Resource Governor n’est pas activé.<br /><br /> 1 = Resource Governor est activé.<br /><br /> N'accepte pas la valeur NULL.|  
|max_outstanding_io_per_volume|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Nombre maximal d'E/S en attente par volume.|  
  
## <a name="remarks"></a>Notes  
 L'affichage catalogue affiche la configuration du gouverneur de ressources telle qu'elle est stockée dans les métadonnées. Pour consulter la configuration en mémoire, utilisez la vue de gestion dynamique correspondante.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW ANY DEFINITION pour consulter le contenu, requiert l'autorisation CONTROL SERVER pour modifier le contenu.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant indique comment obtenir et comparer les valeurs de métadonnées stockées et les valeurs en mémoire de la configuration du gouverneur de ressources.  
  
```  
USE master;  
GO  
-- Get the stored metadata.  
SELECT   
object_schema_name(classifier_function_id) AS 'Classifier UDF schema in metadata',   
object_name(classifier_function_id) AS 'Classifier UDF name in metadata'  
FROM   
sys.resource_governor_configuration;  
GO  
-- Get the in-memory configuration.  
SELECT   
object_schema_name(classifier_function_id) AS 'Active classifier UDF schema',   
object_name(classifier_function_id) AS 'Active classifier UDF name'  
FROM   
sys.dm_resource_governor_configuration;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue Resource Governor &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.dm_resource_governor_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-configuration-transact-sql.md)   
 [gouverneur de ressources](../../relational-databases/resource-governor/resource-governor.md)  
  
  
