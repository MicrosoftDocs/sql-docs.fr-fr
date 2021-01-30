---
description: sys.user_token (Transact-SQL)
title: sys.user_token (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.user_token
- user_token
- sys.user_token_TSQL
- user_token_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- logins [SQL Server], security tokens
- sys.user_token catalog view
- user tokens [SQL Server]
- tokens [SQL Server]
- user_token catalog view
ms.assetid: be018103-5e57-43a4-9160-9bf420892aa7
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017|| = azure-sqldw-latest
ms.openlocfilehash: 05dcc6f926834170ce5541ae146f2f24e66a6333
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206812"
---
# <a name="sysuser_token-transact-sql"></a>sys.user_token (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2008-asdb-asdw-xxx-md.md)]

  Renvoie une ligne pour chaque principal de base de données faisant partie du jeton de l'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**principal_id**|**int**|ID du principal. Sa valeur est unique dans une base de données.|  
|**sid**|**varbinary(85)**|Identificateur de sécurité du principal, si le principal est défini comme externe à la base de données. Par exemple, ce peut être une information de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Windows, Windows Group, ou une information de connexion mappée sur un certificat ; sinon, cette valeur est NULL.|  
|**name**|**nvarchar (128)**|Nom du principal. Sa valeur est unique dans une base de données.|  
|**type**|**nvarchar (128)**|Description du type de principal. Tous les types sont mappés à **sid**. Il peut s'agir de l'une des valeurs suivantes :<br /><br /> SQL USER<br /><br /> WINDOWS LOGIN<br /><br /> WINDOWS GROUP<br /><br /> ROLE<br /><br /> APPLICATION ROLE<br /><br /> DATABASE ROLE<br /><br /> USER MAPPED TO CERTIFICATE<br /><br /> USER MAPPED TO ASYMMETRIC KEY<br /><br /> CERTIFICATE<br /><br /> ASYMMETRIC KEY|  
|**syntaxe**|**nvarchar (128)**|Indique que le principal prend part à l'évaluation des autorisations GRANT ou DENY, ou sert d'authentificateur.<br /><br /> Cette valeur peut être l'une des suivantes :<br /><br /> GRANT OR DENY<br /><br /> DENY ONLY<br /><br /> AUTHENTICATOR|  
  
## <a name="see-also"></a>Voir aussi  
 [sys.login_token &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-login-token-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
