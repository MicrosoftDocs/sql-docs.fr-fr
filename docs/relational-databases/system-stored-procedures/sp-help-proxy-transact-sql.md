---
description: sp_help_proxy (Transact-SQL)
title: sp_help_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_proxy
- sp_help_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_proxy
ms.assetid: a2fce164-2b64-40c2-8f35-6eeb7844abf1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a598d3fef67af2e2d9f4874b9e8f63770b762bb7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199618"
---
# <a name="sp_help_proxy-transact-sql"></a>sp_help_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Répertorie les informations d'un ou plusieurs serveurs proxy.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_proxy   
    [ @proxy_id = ] id,  
    [ @proxy_name = ] 'proxy_name' ,  
    [ @subsystem_name = ] 'subsystem_name' ,  
    [ @name = ] 'name'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @proxy_id = ] id` Numéro d’identification du proxy pour lequel répertorier les informations. *Proxy_id* est de **type int**, avec NULL comme valeur par défaut. L' *ID* ou le *proxy_name* peuvent être spécifiés.  
  
`[ @proxy_name = ] 'proxy_name'` Nom du proxy pour lequel répertorier les informations. *Proxy_name* est de **type sysname**, avec NULL comme valeur par défaut. L' *ID* ou le *proxy_name* peuvent être spécifiés.  
  
`[ @subsystem_name = ] 'subsystem_name'` Nom du sous-système pour lequel répertorier les proxies. *Subsystem_name* est de **type sysname**, avec NULL comme valeur par défaut. Lorsque *subsystem_name* est spécifié, le *nom* doit également être spécifié.  
  
 Le tableau suivant répertorie les valeurs possibles pour chaque sous-système.  
  
|Valeur|Description|  
|-----------|-----------------|  
|ActiveScripting|Script ActiveX|  
|CmdExec|Système d'exploitation (CmdExec)|  
|Instantané|Agent d'instantané de réplication|  
|LogReader|Agent de lecture du journal des réplications|  
|Distribution|Agent de distribution de réplication|  
|Fusionner|Replication Merge Agent|  
|QueueReader|Agent de lecture de la file d'attente de réplication|  
|ANALYSISQUERY|Commandes Analysis Services|  
|ANALYSISCOMMAND|Requête Analysis Services|  
|Dts|Exécution de package SSIS|  
|PowerShell|script PowerShell|  
  
`[ @name = ] 'name'` Nom d’une connexion pour laquelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] répertorier les proxies. Le nom est de type **nvarchar (256)**, avec NULL comme valeur par défaut. Quand *Name* est spécifié, *subsystem_name* doit également être spécifié.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|Numéro d'identification du proxy.|  
|**name**|**sysname**|Nom du proxy.|  
|**credential_identity**|**sysname**|Nom du domaine Microsoft Windows et nom d'utilisateur pour les informations d'identification associées au serveur proxy.|  
|**activé**|**tinyint**|Indique si ce serveur proxy est activé. { **0** = non activé, **1** = activé}|  
|**description**|**nvarchar(1024)**|Description de ce serveur proxy.|  
|**user_sid**|**varbinary(85)**|Numéro d'identification de sécurité (SID) Windows de l'utilisateur Windows pour ce serveur proxy.|  
|**credential_id**|**int**|Identifiant des informations d'identification associées à ce serveur proxy.|  
|**credential_identity_exists**|**int**|Indique si l'identifiant des informations d'identification existe. { 0 = inexistant, 1 = existant }|  
  
## <a name="remarks"></a>Notes  
 Quand aucun paramètre n’est fourni, **sp_help_proxy** répertorie les informations de tous les proxies de l’instance.  
  
 Pour déterminer les proxys qu’une connexion peut utiliser pour un sous-système donné, spécifiez *Name* et *subsystem_name*. Lorsque ces arguments sont fournis, **sp_help_proxy** répertorie les proxies auxquels la connexion spécifiée peut accéder et qui peut être utilisée pour le sous-système spécifié.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer du rôle de base de données fixe **SQLAgentOperatorRole** dans la base de données **msdb** .  
  
 Pour plus d’informations sur les **SQLAgentOperatorRole**, consultez [SQL Server Agent des rôles de base de données fixes](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
> [!NOTE]  
>  Les colonnes **credential_identity** et **user_sid** ne sont retournées dans le jeu de résultats que lorsque les membres de **sysadmin** exécutent cette procédure stockée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-information-for-all-proxies"></a>R. Affichage des informations pour tous les serveurs proxy  
 L'exemple ci-dessous répertorie les informations pour tous les serveurs proxy de l'instance.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_proxy ;  
GO  
```  
  
### <a name="b-listing-information-for-a-specific-proxy"></a>B. Affichage des informations pour un serveur proxy spécifique  
 L'exemple ci-dessous répertorie les informations pour le serveur proxy nommé `Catalog application proxy`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_proxy  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Agent des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_delete_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-proxy-transact-sql.md)  
  
  
