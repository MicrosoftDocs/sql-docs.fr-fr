---
description: sp_enum_proxy_for_subsystem (Transact-SQL)
title: sp_enum_proxy_for_subsystem (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_enum_proxy_for_subsystem_TSQL
- sp_enum_proxy_for_subsystem
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_proxy_for_subsystems
ms.assetid: 580cc3be-1068-4a96-8d15-78ca3a5bb719
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: bac523afb9f802e3f6feb8a23c47835c82bac807
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157075"
---
# <a name="sp_enum_proxy_for_subsystem-transact-sql"></a>sp_enum_proxy_for_subsystem (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Répertorie les autorisations pour les proxys de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent afin d'accéder aux sous-systèmes.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_enum_proxy_for_subsystem  
    [ @proxy_id = ] proxy_id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @subsystem_id = ] subsystem_id,  
    [ @subsystem_name = ] 'subsystem_name'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @proxy_id = ] proxy_id` Numéro d’identification du proxy pour lequel répertorier les informations. *Proxy_id* est de **type int**, avec NULL comme valeur par défaut. L' *ID* ou le *proxy_name* peuvent être spécifiés.  
  
`[ @proxy_name = ] 'proxy_name'` Nom du proxy pour lequel répertorier les informations. *Proxy_name* est de **type sysname**, avec NULL comme valeur par défaut. L' *ID* ou le *proxy_name* peuvent être spécifiés.  
  
`[ @subsystem_id = ] subsystem_id` Numéro d’identification du sous-système pour lequel répertorier les informations. *Subsystem_id* est de **type int**, avec NULL comme valeur par défaut. La *subsystem_id* ou la *subsystem_name* peut être spécifiée.  
  
`[ @subsystem_name = ] 'subsystem_name'` Nom du sous-système pour lequel répertorier les informations. *Subsystem_name* est de **type sysname**, avec NULL comme valeur par défaut. La *subsystem_id* ou la *subsystem_name* peut être spécifiée.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|Numéro d'identification du sous-système|  
|**subsystem_name**|**sysname**|Nom du sous-système.|  
|**proxy_id**|**int**|Numéro d'identification du proxy.|  
|**proxy_name**|**sysname**|Nom du proxy.|  
| &nbsp; | &nbsp; | &nbsp; |
  
## <a name="remarks"></a>Notes  
 Quand aucun paramètre n’est fourni, **sp_enum_proxy_for_subsystem** répertorie des informations sur tous les proxies de l’instance pour chaque sous-système.  
  
 Lorsqu’un ID de proxy ou un nom de proxy est fourni, **sp_enum_proxy_for_subsystem** répertorie les sous-systèmes auxquels le proxy a accès. Lorsqu’un ID de sous-système ou un nom de sous-système est fourni, **sp_enum_proxy_for_subsystem** répertorie les proxies qui ont accès à ce sous-système.  
  
 Lorsque des informations de proxy et de sous-systèmes sont fournies, le jeu de résultats renvoie une ligne si le proxy spécifié dispose d'un accès au sous-système spécifié.  
  
 Cette procédure stockée se trouve dans **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-associations"></a>R. Création de la liste de toutes les associations  
 L'exemple suivant répertorie toutes les autorisations établies entre des proxys et des sous-systèmes dans l'instance en cours.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_proxy_for_subsystem ;  
GO  
```  
  
### <a name="b-determining-if-a-proxy-has-access-to-a-specific-subsystem"></a>B. Détermination de si un proxy dispose d'un accès à un sous-système spécifique  
 L'exemple suivant renvoie une ligne si le proxy `Catalog application proxy` peut accéder au sous-système `ActiveScripting`. Dans les autres cas, l'exemple renvoie un jeu de résultats vide.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_proxy_for_subsystem  
    @subsystem_name = 'ActiveScripting',  
    @proxy_name = 'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_grant_proxy_to_subsystem &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
  
  
