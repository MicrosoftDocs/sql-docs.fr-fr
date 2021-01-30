---
description: sp_enum_login_for_proxy (Transact-SQL)
title: sp_enum_login_for_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_enum_login_for_proxy_TSQL
- sp_enum_login_for_proxy
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_login_for_proxy
ms.assetid: 62a75019-248a-44c8-a5cc-c79f55ea3acf
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: b51d02a66833df04a82a970319bab4e62f462a33
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186964"
---
# <a name="sp_enum_login_for_proxy-transact-sql"></a>sp_enum_login_for_proxy (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Répertorie des associations entre les principaux de sécurité et les proxys.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_enum_login_for_proxy  
    [ @name = ] 'name'  
    [ @proxy_id = ] id,  
    [ @proxy_name = ] 'proxy_name'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @name = ] 'name'` Nom d’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] principal, d’une connexion, d’un rôle de serveur ou d’un rôle de base de données **msdb** pour lequel répertorier les proxies. Le nom est de type **nvarchar (256)**, avec NULL comme valeur par défaut.  
  
`[ @proxy_id = ] id` Numéro d’identification du proxy pour lequel répertorier les informations. *Proxy_id* est de **type int**, avec NULL comme valeur par défaut. L' *ID* ou le *proxy_name* peuvent être spécifiés.  
  
`[ @proxy_name = ] 'proxy_name'` Nom du proxy pour lequel répertorier les informations. *Proxy_name* est de **type sysname**, avec NULL comme valeur par défaut. L' *ID* ou le *proxy_name* peuvent être spécifiés.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|Numéro d'identification du proxy.|  
|**proxy_name**|**sysname**|Nom du proxy.|  
|**name**|**sysname**|Nom du principal de sécurité pour l'association.|  
|**flags**|**int**|Type du principal de sécurité.<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion<br /><br /> **1** = rôle de système fixe<br /><br /> **2** = rôle de base de données dans **msdb**|  
| &nbsp; | &nbsp; | &nbsp; |
  
## <a name="remarks"></a>Notes  
 Quand aucun paramètre n’est fourni, **sp_enum_login_for_proxy** répertorie des informations sur toutes les connexions dans l’instance pour chaque proxy.  
  
 Lorsqu’un ID de proxy ou un nom de proxy est fourni, **sp_enum_login_for_proxy** répertorie les connexions qui ont accès au proxy. Lorsqu’un nom de connexion est fourni, **sp_enum_login_for_proxy** répertorie les proxys auxquels la connexion a accès.  
  
 Lorsque des informations de proxy et un nom de connexion sont fournis, le jeu de résultats renvoie une ligne si la connexion spécifiée dispose d'un accès au proxy spécifié.  
  
 Cette procédure stockée se trouve dans **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-associations"></a>R. Création de la liste de toutes les associations  
 L'exemple suivant répertorie toutes les autorisations établies entre des connexions et des proxys dans l'instance en cours.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_login_for_proxy ;  
GO  
```  
  
### <a name="b-listing-proxies-for-a-specific-login"></a>B. Création de la liste des proxys pour une connexion spécifique  
 L'exemple suivant répertorie les proxys auxquels la connexion `terrid` peut accéder.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_login_for_proxy  
    @name = 'terrid' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_help_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-proxy-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
