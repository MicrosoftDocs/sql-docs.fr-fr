---
description: sp_addsynctriggers (Transact-SQL)
title: sp_addsynctriggers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addsynctriggers_TSQL
- sp_addsynctriggers
helpviewer_keywords:
- sp_addsynctriggers
ms.assetid: e37d0c3b-19bf-4719-9535-96ba361372b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 00782a900c02976c06f2cfcdf6c9184d90e8059d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211951"
---
# <a name="sp_addsynctriggers-transact-sql"></a>sp_addsynctriggers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée des déclencheurs sur l'Abonné, utilisés avec tous les types d'abonnements pouvant être mis à jour (mise à jour immédiate, mise à jour en attente et mise à jour immédiate avec mise à jour en attente sous forme de basculement). Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
> [!IMPORTANT]  
>  La procédure [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) doit être utilisée à la place de **sp_addsynctrigger**. [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) génère un script qui contient les appels de **sp_addsynctrigger** .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addsynctriggers [ @sub_table = ] 'sub_table'  
        , [ @sub_table_owner = ] 'sub_table_owner'  
        , [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @ins_proc = ] 'ins_proc'   
        , [ @upd_proc = ] 'upd_proc'   
        , [ @del_proc = ] 'del_proc'   
        , [ @cftproc = ] 'cftproc'  
        , [ @proc_owner = ] 'proc_owner'  
    [ , [ @identity_col = ] 'identity_col' ]  
    [ , [ @ts_col = ] 'timestamp_col' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]   
        , [ @primary_key_bitmap = ] 'primary_key_bitmap'  
    [ , [ @identity_support = ] identity_support ]  
    [ , [ @independent_agent = ] independent_agent ]  
        , [ @distributor = ] 'distributor'   
    [ , [ @pubversion = ] pubversion  
```  
  
## <a name="arguments"></a>Arguments  
`[ @sub_table = ] 'sub_table'` Nom de la table de l’abonné. *sub_table* est de **type sysname**, sans valeur par défaut.  
  
`[ @sub_table_owner = ] 'sub_table_owner'` Nom du propriétaire de la table de l’abonné. *sub_table_owner* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher = ] 'publisher'` Est le nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, sans valeur par défaut. Si NULL est spécifié, la base de données active est utilisée.  
  
`[ @publication = ] 'publication'` Nom de la publication. *Publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @ins_proc = ] 'ins_proc'` Nom de la procédure stockée qui prend en charge les insertions de transactions synchrones sur le serveur de publication. *ins_proc* est de **type sysname**, sans valeur par défaut.  
  
`[ @upd_proc = ] 'upd_proc'` Nom de la procédure stockée qui prend en charge les mises à jour de transactions synchrones sur le serveur de publication. *ins_proc* est de **type sysname**, sans valeur par défaut.  
  
`[ @del_proc = ] 'del_proc'` Nom de la procédure stockée qui prend en charge les suppressions de transactions synchrones sur le serveur de publication. *ins_proc* est de **type sysname**, sans valeur par défaut.  
  
`[ @cftproc = ] 'cftproc'` Nom de la procédure générée automatiquement utilisée par les publications qui autorisent la mise à jour en attente. *cftproc* est de **type sysname**, sans valeur par défaut. Pour les publications autorisant la mise à jour immédiate, cette valeur est NULL. Ce paramètre s'applique aux publications qui autorisent la mise à jour en attente (mise à jour en attente et mise à jour immédiate avec mise à jour en attente sous forme de basculement).  
  
`[ @proc_owner = ] 'proc_owner'` Spécifie le compte d’utilisateur dans le serveur de publication sous lequel toutes les procédures stockées générées automatiquement pour la publication de mise à jour (en file d’attente et/ou immédiate) ont été créées. *proc_owner* est de **type sysname** et n’a pas de valeur par défaut.  
  
`[ @identity_col = ] 'identity_col'` Nom de la colonne d’identité sur le serveur de publication. *identity_col* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @ts_col = ] 'timestamp_col'` Nom de la colonne **timestamp** sur le serveur de publication. *timestamp_col* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @filter_clause = ] 'filter_clause'` Clause de restriction (WHERE) qui définit un filtre horizontal. Quand vous entrez la clause de restriction, omettez le mot clé WHERE. *filter_clause* est de type **nvarchar (4000)**, avec NULL comme valeur par défaut.  
  
`[ @primary_key_bitmap = ] 'primary_key_bitmap'` Est une table de bits des colonnes clés primaires dans la table. *primary_key_bitmap* est de type **varbinary (4000)**, sans valeur par défaut.  
  
`[ @identity_support = ] identity_support` Active et désactive la gestion automatique des plages d’identité lorsque la mise à jour en file d’attente est utilisée. *identity_support* est un **bit**, avec **0** comme valeur par défaut. **0** signifie qu’il n’existe aucune prise en charge de plage d’identité, **1** active la gestion automatique des plages d’identité.  
  
`[ @independent_agent = ] independent_agent` Indique s’il existe une seule Agent de distribution (un agent indépendant) pour cette publication, ou une Agent de distribution par base de données de publication et par paire de bases de données d’abonnement (un agent partagé). Cette valeur reflète la valeur de la propriété independent_agent de la publication définie sur le serveur de publication. *independent_agent* est un bit avec **0** comme valeur par défaut. Si la **valeur est 0**, l’agent est un agent partagé. Si la condition est **1**, l’agent est un agent indépendant.  
  
`[ @distributor = ] 'distributor'` Nom du serveur de distribution. *Distributor* est de **type sysname**, sans valeur par défaut.  
  
`[ @pubversion = ] pubversion` Indique la version du serveur de publication. *pubversion* est de **type int**, avec 1 comme valeur par défaut. **1** signifie que la version du serveur de publication est [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 2 ou version antérieure ; **2** signifie que le serveur de publication est [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 3 (SP3) ou version ultérieure. *pubversion* doit avoir la valeur **2** explicitement lorsque la version du serveur de publication est [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] SP3 ou ultérieure.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addsynctriggers** est utilisé par le agent de distribution dans le cadre de l’initialisation de l’abonnement. Cette procédure stockée n'est généralement pas exécutée par les utilisateurs mais peut s'avérer utile s'ils doivent configurer manuellement un abonnement sans synchronisation.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addsynctriggers**.  
  
## <a name="see-also"></a>Voir aussi  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_script_synctran_commands &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
