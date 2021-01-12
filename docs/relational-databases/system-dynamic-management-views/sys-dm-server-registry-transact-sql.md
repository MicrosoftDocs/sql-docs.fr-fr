---
description: sys.dm_server_registry (Transact-SQL)
title: sys.dm_server_registry (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_server_registry_TSQL
- sys.dm_server_registry
- dm_server_registry
- sys.dm_server_registry_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_server_registry dynamic management view
ms.assetid: 9b3e0c74-2e99-4996-a383-104d51831e97
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 909aaa820947285da692e43a178e7580a76adf58
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096445"
---
# <a name="sysdm_server_registry-transact-sql"></a>sys.dm_server_registry (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne les informations relatives à la configuration et à l'installation qui sont stockées dans le Registre de Windows pour l'instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Retourne une ligne par clé de Registre. Utilisez cette vue de gestion dynamique pour retourner des informations telles que les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponibles sur l'ordinateur hôte ou les valeurs de configuration réseau pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|registry_key|**nvarchar (256)**|Nom de la clé de Registre. Autorise la valeur NULL.|  
|value_name|**nvarchar (256)**|Nom de la valeur de la clé. Il s’agit de l’élément affiché dans la colonne **nom** de l’éditeur du Registre. Autorise la valeur NULL.|  
|value_data|**sql_variant**|Valeur des données de la clé. Il s’agit de la valeur affichée dans la colonne de **données** de l’éditeur du Registre pour une entrée donnée. Autorise la valeur NULL.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-display-the-sql-server-services"></a>R. Affichage des services SQL Server  
 L'exemple suivant retourne les valeurs de clé de Registre pour les services SQL Server et SQL Server Agent pour l'instance actuelle de SQL Server.  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%ControlSet%';  
```  
  
### <a name="b-display-the-sql-server-agent-registry-key-values"></a>B. Affichage des valeurs de clés de Registre de Le SQL Server Agent  
 L'exemple suivant retourne les valeurs de clés de Registre de Le SQL Server Agent pour l'instance actuelle de SQL Server.  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%SQLAgent%';  
```  
  
### <a name="c-display-the-current-version-of-the-instance-of-sql-server"></a>C. Affichage de la version actuelle de l'instance de SQL Server  
 L'exemple suivant retourne la version de l'instance actuelle de SQL Server.  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE value_name = N'CurrentVersion';  
```  
  
### <a name="d-display-the-parameters-passed-to-the-instance-of-sql-server-during-startup"></a>D. Affichage des paramètres passés à l'instance de SQL Server pendant le démarrage  
 L'exemple suivant retourne les paramètres passés à l'instance de SQL Server pendant le démarrage.  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%Parameters';  
```  
  
### <a name="e-return-network-configuration-information-for-the-instance-of-sql-server"></a>E. Retour des informations de configuration réseau pour l'instance de SQL Server  
 L'exemple suivant retourne les valeurs de configuration réseau pour l'instance actuelle de SQL Server.  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%SuperSocketNetLib%';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_server_services &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-services-transact-sql.md)  
  
  
