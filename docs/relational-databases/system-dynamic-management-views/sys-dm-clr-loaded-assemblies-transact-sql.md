---
description: sys.dm_clr_loaded_assemblies (Transact-SQL)
title: sys.dm_clr_loaded_assemblies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_clr_loaded_assemblies
- sys.dm_clr_loaded_assemblies_TSQL
- dm_clr_loaded_assemblies_TSQL
- sys.dm_clr_loaded_assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_loaded_assemblies dynamic management view
ms.assetid: 8523d8db-d8a0-4b1f-ae19-6705d633e0a6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d132a0389ca636dbc24afe016b98389021d8d848
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099952"
---
# <a name="sysdm_clr_loaded_assemblies-transact-sql"></a>sys.dm_clr_loaded_assemblies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque assembly d'utilisateur géré chargé dans l'espace d'adressage du serveur. Utilisez cette vue pour comprendre et dépanner les objets de base de données managés d’intégration du CLR qui s’exécutent dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Les assemblys sont des fichiers DLL de code managé qui sont utilisés pour définir et déployer des objets de base de données managés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dès lors qu'un utilisateur exécute un de ces objets de base de données managés, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le CLR chargent l'assembly (et ses références) dans lequel l'objet de base de données managé est défini. L'assembly reste chargé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour augmenter les performances, de sorte que les objets de base de données managés contenus dans l'assembly puissent être appelés ultérieurement, sans avoir à recharger l'assembly. L'assembly n'est pas déchargé tant que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne présente pas une mémoire insuffisante. Pour plus d’informations sur les assemblys et l’intégration du CLR, consultez [environnement hébergé dans CLR](../../relational-databases/clr-integration/clr-integration-architecture-clr-hosted-environment.md). Pour plus d’informations sur les objets de base de données managés, consultez [génération d’objets de base de données avec Common Language Runtime &#40;l’intégration du CLR&#41;](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md).  

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|ID de l'assembly chargé. La **assembly_id** peut être utilisée pour rechercher plus d’informations sur l’assembly dans la vue de catalogue [sys. Assemblies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md) . Notez que le [!INCLUDE[tsql](../../includes/tsql-md.md)] catalogue [sys. Assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md) affiche uniquement les assemblys dans la base de données active. La vue **SQS.dm_clr_loaded_assemblies** affiche tous les assemblys chargés sur le serveur.|  
|**appdomain_address**|**int**|Adresse du domaine d’application (**AppDomain**) dans lequel l’assembly est chargé. Tous les assemblys appartenant à un seul utilisateur sont toujours chargés dans le même **AppDomain**. La **appdomain_address** peut être utilisée pour rechercher plus d’informations sur le domaine d’application **AppDomain** dans la vue de [sys.dm_clr_appdomains](../../relational-databases/system-dynamic-management-views/sys-dm-clr-appdomains-transact-sql.md) .|  
|**load_time**|**datetime**|Heure à laquelle l'assembly a été chargé. Notez que l’assembly reste chargé jusqu’à ce que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soit soumis à une sollicitation de la mémoire et décharge l' **AppDomain**. Vous pouvez surveiller **load_time** pour comprendre la fréquence de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la sollicitation de la mémoire et décharger l' **AppDomain**.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="remarks"></a>Notes  
 La vue **dm_clr_loaded_assemblies. appdomain_address** a une relation plusieurs-à-un avec  **dm_clr_appdomains. appdomain_address**. La vue **dm_clr_loaded_assemblies. assembly_id** a une relation un-à-plusieurs avec **sys. Assemblies. assembly_id**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre comment afficher les détails de tous les assemblys de la base de données active qui sont chargés.  
  
```  
 SELECT a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
FROM sys.dm_clr_loaded_assemblies AS l   
INNER JOIN sys.assemblies AS a  
ON l.assembly_id = a.assembly_id;  
```  
  
 L’exemple suivant montre comment afficher les détails de l' **AppDomain** dans lequel un assembly donné est chargé.  
  
```  
SELECT appdomain_id, creation_time, db_id, user_id, state  
FROM sys.dm_clr_appdomains AS a  
WHERE appdomain_address =   
(SELECT appdomain_address   
 FROM sys.dm_clr_loaded_assemblies  
 WHERE assembly_id = 555);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique liées au Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
