---
description: dbo.sysproxies (Transact-SQL)
title: Proxies dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysproxies_TSQL
- sysproxies_TSQL
- dbo.sysproxies
- sysproxies
dev_langs:
- TSQL
helpviewer_keywords:
- sysproxies system table
ms.assetid: a73da875-be22-45fc-b5e2-ea7ebd48e2d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: fb3f9c479086650752b550588bf8abe6c7f0e220
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184675"
---
# <a name="dbosysproxies-transact-sql"></a>dbo.sysproxies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit les attributs d'un compte proxy de l'agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|ID du compte proxy.|  
|**name**|**sysname**|Nom du compte proxy.|  
|**credential_id**|**int**|ID des informations d'identification utilisées par le compte proxy.|  
|**activé**|**tinyint**|État du compte proxy :<br /><br /> **0** = désactivé. **1** = activé.|  
|**description**|**nvarchar(512)**|Description entrée par l'utilisateur lors de la création du compte proxy.|  
|**user_sid**|**varbinary(85)**|Microsoft Windows *security_identifier* de l’utilisateur ou du groupe associé aux informations d’identification du proxy.|  
|**credential_date_created**|**datetime**|Date et heure de création des informations d'identification.|  
  
## <a name="remarks"></a>Notes  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent accéder à la table **sysproxies** .  
  
## <a name="see-also"></a>Voir aussi  
 [dbo.sysProxyLogin &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxylogin-transact-sql.md)   
 [dbo.sysproxysubsystem &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)   
 [dbo.syssous-systèmes &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-syssubsystems-transact-sql.md)  
  
  
