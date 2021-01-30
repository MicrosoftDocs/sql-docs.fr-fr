---
description: dbo.sysproxylogin (Transact-SQL)
title: dbo.sysProxyLogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysproxylogin_TSQL
- sysproxylogin_TSQL
- dbo.sysproxylogin
- sysproxylogin
dev_langs:
- TSQL
helpviewer_keywords:
- sysproxylogin system table
ms.assetid: 433d33cb-bdf2-47bb-af78-2a40b7c8dfce
author: cawrites
ms.author: chadam
ms.openlocfilehash: b28fb3c6984814ad7e26f82c0d09cf4fede3ac05
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210733"
---
# <a name="dbosysproxylogin-transact-sql"></a>dbo.sysproxylogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enregistre les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sont associées avec chaque compte proxy de SQL Server Agent. Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|Identificateur du compte proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette valeur correspond à la colonne **proxy_id** de la table **sysproxies** .|  
|**sid**|**varbinary(85)**|Microsoft Windows *security_identifier* pour la connexion SQL Server.|  
|**principal_id**|**int**|ID de l'utilisateur ou du groupe qui a l'autorisation d'utiliser le compte proxy pour une étape du sous-système spécifié.|  
|**flags**|**int**|Type de connexion :<br /><br /> **0** = utilisateur ou groupe Windows, et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion.<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rôle de système fixe<br /><br /> **2**  =  rôle de base de données **msdb**|  
  
## <a name="remarks"></a>Notes  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent accéder à cette table.  
  
## <a name="see-also"></a>Voir aussi  
 [ Proxiesdbo.sys&#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)  
  
  
