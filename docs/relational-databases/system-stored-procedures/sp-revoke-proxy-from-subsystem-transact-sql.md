---
description: sp_revoke_proxy_from_subsystem (Transact-SQL)
title: sp_revoke_proxy_from_subsystem (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_revoke_login_from_subsystem
- sp_revoke_login_from_subsystem_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_revoke_proxy_from_subsystem
ms.assetid: b87bc8ba-3ea8-4aed-b54b-32c3d82d9d2a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 9457a1d9dd0d9fd01a7418cd56e772902d593b6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211854"
---
# <a name="sp_revoke_proxy_from_subsystem-transact-sql"></a>sp_revoke_proxy_from_subsystem (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Interdit à un proxy d'accéder à un sous-système.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_revoke_proxy_from_subsystem   
    [ @proxy_id = ] proxy_id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @subsystem_id = ] subsystem_id,  
    [ @subsystem_name = ] 'subsystem_name'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @proxy_id = ] id` Numéro d’identification du proxy à partir duquel révoquer l’accès. *Proxy_id* est de **type int**, avec NULL comme valeur par défaut. *Proxy_id* ou *proxy_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @proxy_name = ] 'proxy_name'` Nom du proxy à partir duquel révoquer l’accès. *Proxy_name* est de **type sysname**, avec NULL comme valeur par défaut. *Proxy_id* ou *proxy_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @subsystem_id = ] id` Numéro d’identification du sous-système auquel révoquer l’accès. *Subsystem_id* est de **type int**, avec NULL comme valeur par défaut. *Subsystem_id* ou *subsystem_name* doivent être spécifiés, mais ne peuvent pas être spécifiés. Le tableau suivant répertorie les valeurs possibles pour chaque sous-système.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**2**|Script ActiveX<br /><br /> Important le sous-système de script ActiveX sera supprimé de l’agent dans une version future de **\* . \* \* \*** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.|  
|**3**|Système d'exploitation (CmdExec)|  
|**4**|Agent d'instantané de réplication|  
|**5**|Agent de lecture du journal des réplications|  
|**6**|Agent de distribution de réplication|  
|**7**|Replication Merge Agent|  
|**8**|Agent de lecture de la file d'attente de réplication|  
|**9**|Commandes Analysis Services|  
|**10**|Requête Analysis Services|  
|**11**|[!INCLUDE[ssIS](../../includes/ssis-md.md)] Exécution du package|  
|**12**|script PowerShell|  
  
`[ @subsystem_name = ] 'subsystem_name'` Nom du sous-système auquel révoquer l’accès. *Subsystem_name* est de **type sysname**, avec NULL comme valeur par défaut. *Subsystem_id* ou *subsystem_name* doivent être spécifiés, mais ne peuvent pas être spécifiés. Le tableau suivant répertorie les valeurs possibles pour chaque sous-système.  
  
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
|Dts|[!INCLUDE[ssIS](../../includes/ssis-md.md)] Exécution du package|  
|PowerShell|script PowerShell|  
  
## <a name="remarks"></a>Notes  
 Refuser l'accès à un sous-système ne change en rien les autorisations accordées au principal spécifié dans le proxy.  
  
> [!NOTE]  
>  Pour déterminer les étapes de travail qui référencent un proxy, cliquez avec le bouton droit sur le nœud **proxies** sous **SQL Server Agent** dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , puis cliquez sur **Propriétés**. Dans la boîte de dialogue **Propriétés du compte proxy** , sélectionnez la page **références** pour afficher toutes les étapes de travail qui font référence à ce proxy.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_revoke_proxy_from_subsystem**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant refuse l'accès au sous-système [!INCLUDE[ssIS](../../includes/ssis-md.md)] au proxy `Catalog application proxy`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_revoke_proxy_from_subsystem  
    @proxy_name = 'Catalog application proxy',  
    @subsystem_name = N'Dts';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Agent des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [Implémenter la sécurité SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md)   
 [sp_grant_proxy_to_subsystem &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
  
  
