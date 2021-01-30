---
description: sp_addqreader_agent (Transact-SQL)
title: sp_addqreader_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addqreader_agent_TSQL
- sp_addqreader_agent
helpviewer_keywords:
- sp_addqreader_agent
ms.assetid: dc9f591a-e67e-4ba8-bf47-defd5eda0822
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f148297e264a0ca097671234655f9854b7e79aba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206649"
---
# <a name="sp_addqreader_agent-transact-sql"></a>sp_addqreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ajoute un Agent de lecture de la file d'attente à un serveur de distribution. Cette procédure stockée est exécutée sur la base de données de distribution du serveur de distribution ou sur la base de publication du serveur de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addqreader_agent [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
    [ , [ @job_name = ] 'job_name'  
    [ , [ @frompublisher = ] frompublisher   
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_login = ] 'job_login'` Nom de connexion du [!INCLUDE[msCoName](../../includes/msconame-md.md)] compte Windows sous lequel l’agent s’exécute. *job_login* est de type **nvarchar (257)**, sans valeur par défaut. Ce compte Windows est toujours utilisé pour les connexions des agents au serveur de distribution.  
  
`[ @job_password = ] 'job_password'` Mot de passe du compte Windows sous lequel l’agent s’exécute. *job_password* est de **type sysname**, sans valeur par défaut.  
  
> [!IMPORTANT]  
>  Ne stockez pas les informations d'authentification dans des fichiers de script. Pour une sécurité optimale, les noms de connexion et les mots de passe doivent être fournis au moment de l'exécution.  
  
`[ @job_name = ] 'job_name'` Nom d’un travail d’agent existant. *job_name* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre n'est spécifié que lorsque l'Agent est créé avec un travail existant au lieu d'un nouveau travail (valeur par défaut).  
  
`[ @frompublisher = ] frompublisher` Spécifie si la procédure est exécutée sur le serveur de publication. *frompublisher* est de bits, avec **0** comme valeur par défaut. La valeur **1** signifie que la procédure est exécutée à partir du serveur de publication sur la base de données de publication.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addqreader_agent** est utilisé dans la réplication transactionnelle.  
  
 les **sp_addqreader_agent** doivent être exécutées au moins une fois sur un serveur de distribution qui prend en charge la mise à jour en attente après [sp_adddistributiondb](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) mais avant [sp_addpublication](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md).  
  
 La tâche de Agent de lecture de la file d’attente est supprimée lorsque vous exécutez [sp_dropdistributiondb](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_addqreader_agent**.  
  
## <a name="see-also"></a>Voir aussi  
 [Activer les abonnements mis à jour pour les publications transactionnelles](../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md)   
 [Mettre à niveau les scripts de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md)   
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_changeqreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changeqreader-agent-transact-sql.md)   
 [sp_helpqreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpqreader-agent-transact-sql.md)  
  
  
