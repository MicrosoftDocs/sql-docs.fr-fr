---
description: sys.sql_logins (Transact-SQL)
title: sys.sql_logins (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sql_logins_TSQL
- sql_logins_TSQL
- sys.sql_logins
- sql_logins
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_logins catalog view
ms.assetid: 0d9c5b09-86fe-40ff-baab-00b7c051402f
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da35c043189954e8e4534ff640c3a9cdc9fc8078
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210276"
---
# <a name="syssql_logins-transact-sql"></a>sys.sql_logins (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]

  Retourne une ligne pour chaque connexion d'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|--|Hérite de **sys.server_principals**.|  
|**is_policy_checked**|**bit**|La stratégie de mot de passe est vérifiée.|  
|**is_expiration_checked**|**bit**|Vérification de l'expiration du mot de passe.|  
|**password_hash**|**varbinary(256)**|Hachage du mot de passe SQL. Depuis [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les informations de mot de passe stockées sont calculées à l’aide de la valeur salt SHA-512 du mot de passe.|  
  
 Pour obtenir la liste des colonnes héritées par cette vue, consultez [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md). Les colonnes `owning_principal_id` et ne `is_fixed_role` sont pas héritées de sys.server_principals.
  
## <a name="remarks"></a>Notes  
 Pour afficher à la fois les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’authentification et les connexions d’authentification Windows, consultez [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Quand les utilisateurs de base de données à relation contenant-contenu sont activés, les connexions peuvent être établies sans connexions. Pour identifier ces comptes, consultez  [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Toute connexion d'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut voir son propre nom de connexion et la connexion d'administrateur système (sa). Pour voir d'autres connexions, vous devez disposer de l'autorisation ALTER ANY LOGIN, ou d'une autorisation sur la connexion.  
 Pour afficher le contenu de la colonne password_hash, l’autorisation CONTROL SERVER est requise.
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Stratégie de mot de passe](../../relational-databases/security/password-policy.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
