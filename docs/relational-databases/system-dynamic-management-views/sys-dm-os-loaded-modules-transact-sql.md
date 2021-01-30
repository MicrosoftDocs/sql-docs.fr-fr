---
description: sys.dm_os_loaded_modules (Transact-SQL)
title: sys.dm_os_loaded_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_loaded_modules
- dm_os_loaded_modules
- sys.dm_os_loaded_modules_TSQL
- dm_os_loaded_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_loaded_modules dynamic management view
ms.assetid: 56c7743a-b568-4943-bd3b-73c57d9d641c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5d807593527ea08586aaaefdbf46c0fa9ee874ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184869"
---
# <a name="sysdm_os_loaded_modules-transact-sql"></a>sys.dm_os_loaded_modules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque module chargé dans l'espace d'adressage du serveur.  
  
> [!NOTE]  
>  Pour appeler ce à partir de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_loaded_modules**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**base_address**|**varbinary (8)**|Adresse du module dans le processus.|  
|**file_version**|**varchar (23)**|Version du fichier. Apparaît sous le format suivant :<br /><br /> x.x:x.x|  
|**product_version**|**varchar (23)**|Version du produit. Apparaît sous le format suivant :<br /><br /> x.x:x.x|  
|**Debug**|**bit**|1 = le module est une version de débogage du module chargé.|  
|**patched**|**bit**|1 = le module a été corrigé.|  
|**version préliminaire**|**bit**|1 = le module est une version préliminaire du module chargé.|  
|**private_build**|**bit**|1 = le module est une version privée du module chargé.|  
|**special_build**|**bit**|1 = le module est une version spéciale du module chargé.|  
|**language**|**int**|Langue des informations de version du module.|  
|**entreprise**|**nvarchar (256)**|Nom de la société qui a créé le module.|  
|**description**|**nvarchar (256)**|Description du module.|  
|**name**|**nvarchar(255)**|Nom du module. Inclut le chemin d'accès complet du module.|  
|**pdw_node_id**|**int**|**S’applique à** : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  
