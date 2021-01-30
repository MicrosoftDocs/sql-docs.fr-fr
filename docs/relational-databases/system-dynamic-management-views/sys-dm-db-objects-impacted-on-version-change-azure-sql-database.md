---
description: sys.dm_db_objects_impacted_on_version_change (Azure SQL Database)
title: sys.dm_db_objects_impacted_on_version_change
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.dm_db_objects_impacted_on_version_change_TSQL
- dm_db_objects_impacted_on_version_change
- dm_db_objects_impacted_on_version_change_TSQL
- sys.dm_db_objects_impacted_on_version_change
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_objects_impacted_on_version_change
- sys.dm_db_objects_impacted_on_version_change
ms.assetid: b94af834-c4f6-4a27-80a6-e8e71fa8793a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 6e04a46841fe9e1df60133c9f3ff9495e4375abc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206264"
---
# <a name="sysdm_db_objects_impacted_on_version_change-azure-sql-database"></a>sys.dm_db_objects_impacted_on_version_change (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Cette vue système dont l'étendue est la base de données est conçue pour fournir un système d'avertissement anticipé pour déterminer les objets qui seront affectés par une mise à niveau de version majeure dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Utilisez cette vue avant ou après la mise à niveau pour obtenir une énumération complète des objets affectés. Vous devez interroger cette vue dans chaque base de données pour obtenir le nombre total sur le serveur.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|class|**entier** NON NULL|Classe de l'objet qui sera affecté :<br /><br /> **1** = contrainte<br /><br /> **7** = index et segments de mémoire|  
|class_desc|**nvarchar (60)** NON NULL|Description de la classe :<br /><br /> **OBJECT_OR_COLUMN**<br /><br /> **INDEX**|  
|major_id|**entier** NON NULL|ID d'objet de la contrainte, ou ID d'objet de la table contenant l'index ou le segment de mémoire.|  
|minor_id|**entier** NUL|**NULL** pour les contraintes<br /><br /> Index_id pour les index et les segments|  
|dependency|**nvarchar (60)** NON NULL|Description de la dépendance qui provoque l'impact sur une contrainte ou un index. La valeur est également utilisée pour les avertissements générés pendant la mise à niveau.<br /><br /> Exemples :<br /><br /> **espace** (pour intrinsèque)<br /><br /> **geometry** (pour UDT système)<br /><br /> **geography::Parse** (pour méthode UDT système)|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW DATABASE STATE.  
  
## <a name="example"></a>Exemple  
 L'exemple suivant illustre une requête portant sur **sys.dm_db_objects_impacted_on_version_change** qui vise à détecter les objets affectés par une mise à niveau vers la prochaine version majeure du serveur.  
  
```  
SELECT * FROM sys.dm_db_objects_disabled_on_version_change;  
GO  
```  
  
```  
class  class_desc        major_id    minor_id    dependency                       
------ ----------------- ----------- ----------- ----------   
1      OBJECT_OR_COLUMN  181575685   NULL        geometry                        
7      INDEX             37575172    1           geometry                        
7      INDEX             2121058592  1           geometry                        
1      OBJECT_OR_COLUMN  101575400   NULL        geometry     
```  
  
## <a name="remarks"></a>Notes  
  
### <a name="how-to-update-impacted-objects"></a>Procédure : mettre à jour les objets affectés  
 Les étapes ordonnées suivantes décrivent l'action corrective à entreprendre après la mise à niveau Service Release de juin.  
  
|Commande|Objet affecté|Action corrective|  
|-----------|---------------------|-----------------------|  
|1|**Index**|Reconstruisez tout index identifié par **sys.dm_db_objects_impacted_on_version_change** par exemple :  `ALTER INDEX ALL ON <table> REBUILD`<br />ou<br />`ALTER TABLE <table> REBUILD`|  
|2|**Object**|Toutes les contraintes identifiées par **sys.dm_db_objects_impacted_on_version_change** doivent être revalidées lorsque les données de géométrie et de géographie de la table sous-jacente ont été recalculées. Pour les contraintes, revalidez l'aide de ALTER TABLE. <br />Par exemple : <br />`ALTER TABLE <tab> WITH CHECK CHECK CONSTRAINT <constraint name>`<br />or<br />`ALTER TABLE <tab> WITH CHECK CONSTRAINT ALL`|  
  
  
