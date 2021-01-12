---
description: sys.dm_resource_governor_configuration (Transact-SQL)
title: sys.dm_resource_governor_configuration (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_resource_governor_configuration_TSQL
- dm_resource_governor_configuration
- sys.dm_resource_governor_configuration
- sys.dm_resource_governor_configuration_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_configuration dynamic management view
ms.assetid: c89aab6a-0434-4ce6-af8c-f8a1a3284e38
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8e49c5a5c5fde8bcbb75393146420323b6bf1c8d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098818"
---
# <a name="sysdm_resource_governor_configuration-transact-sql"></a>sys.dm_resource_governor_configuration (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne qui contient l'état de configuration en mémoire actuel du gouverneur de ressources.  
  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|classifier_function_id|**int**|ID de la fonction classifieur utilisée actuellement par le gouverneur de ressources. Retourne la valeur 0 si aucune fonction n'est utilisée. N'accepte pas la valeur NULL.<br /><br /> **Remarque :** Cette fonction est utilisée pour classer de nouvelles demandes et utilise des règles pour acheminer ces demandes vers le groupe de charge de travail approprié. Pour plus d’informations, consultez [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).|  
|is_reconfiguration_pending|**bit**|Indique si des modifications ont été apportées ou non à un groupe ou un pool à l'aide de l'instruction ALTER RESOURCE GOVERNOR RECONFIGURE mais qu'elles n'ont pas été appliquées à la configuration en mémoire. L'une des valeurs suivantes est retournée :<br /><br /> 0 - Une instruction de reconfiguration n'est pas requise.<br /><br /> 1 - Une instruction de reconfiguration ou un redémarrage du serveur est requis pour que les modifications de configuration en attente soient appliquées.<br /><br /> **Remarque :** La valeur retournée est toujours 0 lorsque Resource Governor est désactivé.<br /><br /> N'accepte pas la valeur NULL.|  
|max_outstanding_io_per_volume|**int**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Nombre maximal d'E/S en attente par volume.|  
  
## <a name="remarks"></a>Notes  
 Cette vue de gestion dynamique montre la configuration en mémoire. Pour consulter les métadonnées de configuration stockées, utilisez l'affichage catalogue correspondant.  
  
 L'exemple suivant indique comment obtenir et comparer les valeurs de métadonnées stockées et les valeurs en mémoire de la configuration du gouverneur de ressources.  
  
```  
USE master;  
go  
-- Get the stored metadata.  
SELECT   
object_schema_name(classifier_function_id) AS 'Classifier UDF schema in metadata',   
object_name(classifier_function_id) AS 'Classifier UDF name in metadata'  
FROM   
sys.resource_governor_configuration;  
go  
-- Get the in-memory configuration.  
SELECT   
object_schema_name(classifier_function_id) AS 'Active classifier UDF schema',   
object_name(classifier_function_id) AS 'Active classifier UDF name'  
FROM   
sys.dm_resource_governor_configuration;  
go  
```  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW SERVER STATE.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.resource_governor_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-configuration-transact-sql.md)   
 [gouverneur de ressources](../../relational-databases/resource-governor/resource-governor.md)  
  
  

