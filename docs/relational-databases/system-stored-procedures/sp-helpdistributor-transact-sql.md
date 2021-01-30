---
description: sp_helpdistributor (Transact-SQL)
title: sp_helpdistributor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdistributor_TSQL
- sp_helpdistributor
helpviewer_keywords:
- sp_helpdistributor
ms.assetid: 37b0983e-3b69-4f0f-977e-20efce0a0b97
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c23613a7b79ea7bfa418425a32b6d2ee9bf62699
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186979"
---
# <a name="sp_helpdistributor-transact-sql"></a>sp_helpdistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Répertorie des informations sur le serveur de distribution, la base de données de distribution, le répertoire de travail et le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte d’utilisateur de l’agent. Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication ou sur n'importe quelle base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpdistributor [ [ @distributor= ] 'distributor' OUTPUT ]  
    [ , [ @distribdb= ] 'distribdb' OUTPUT ]  
    [ , [ @directory= ] 'directory' OUTPUT ]  
    [ , [ @account= ] 'account' OUTPUT ]  
    [ , [ @min_distretention= ] min_distretention OUTPUT ]  
    [ , [ @max_distretention= ] max_distretention OUTPUT ]  
    [ , [ @history_retention= ] history_retention OUTPUT ]  
    [ , [ @history_cleanupagent= ] 'history_cleanupagent' OUTPUT ]  
    [ , [ @distrib_cleanupagent = ] 'distrib_cleanupagent' OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @local = ] 'local' ]  
    [ , [ @rpcsrvname= ] 'rpcsrvname' OUTPUT ]  
    [ , [ @publisher_type = ] 'publisher_type' OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @distributor = ] 'distributor' OUTPUT` Nom du serveur de distribution. Distributor est de **type sysname**, avec **%** la valeur par défaut, qui est la seule valeur qui retourne un jeu de résultats.  
  
`[ @distribdb = ] 'distribdb' OUTPUT` Nom de la base de données de distribution. *distribdb* est de **type sysname**, avec **%** la valeur par défaut, qui est la seule valeur qui retourne un jeu de résultats.  
  
`[ @directory = ] 'directory' OUTPUT` Est le répertoire de travail. *Directory* est de type **nvarchar (255)**, avec **%** la valeur par défaut, qui est la seule valeur qui retourne un jeu de résultats.  
  
`[ @account = ] 'account' OUTPUT` Est le [!INCLUDE[msCoName](../../includes/msconame-md.md)] compte d’utilisateur Windows. *Account* est de type **nvarchar (255)**, avec **%** la valeur par défaut, qui est la seule valeur qui retourne un jeu de résultats.  
  
`[ @min_distretention = ] _min_distretentionOUTPUT` Période de rétention minimale de la distribution, en heures. *min_distretention* est de **type int**, avec **-1** comme valeur par défaut.  
  
`[ @max_distretention = ] _max_distretentionOUTPUT` Période de rétention maximale de la distribution, en heures. *max_distretention* est de **type int**, avec **-1** comme valeur par défaut.  
  
`[ @history_retention = ] _history_retentionOUTPUT` Période de rétention de l’historique, en heures. *history_retention* est de **type int**, avec **-1** comme valeur par défaut.  
  
`[ @history_cleanupagent = ] 'history_cleanupagent' OUTPUT` Nom de l’agent de nettoyage de l’historique. *history_cleanupagent* est de type **nvarchar (100)**, avec **%** la valeur par défaut, qui est la seule valeur qui retourne un jeu de résultats.  
  
`[ @distrib_cleanupagent = ] 'distrib_cleanupagent' OUTPUT` Nom de l’agent de nettoyage de distribution. *distrib_cleanupagent* est de type **nvarchar (100)**, avec **%** la valeur par défaut, qui est la seule valeur qui retourne un jeu de résultats.  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @local = ] 'local'` Indique si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit récupérer les valeurs du serveur local. *local* est de type **nvarchar (5)**, avec NULL comme valeur par défaut.  
  
`[ @rpcsrvname = ] 'rpcsrvname' OUTPUT` Nom du serveur qui émet des appels de procédure distante. *rpcsrvname* est de **type sysname**, avec **%** la valeur par défaut, qui est la seule valeur qui retourne un jeu de résultats.  
  
`[ @publisher_type = ] 'publisher_type' OUTPUT` Type de serveur de publication du serveur de publication. *publisher_type* est de **type sysname**, avec **%** la valeur par défaut, qui est la seule valeur qui retourne un jeu de résultats.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**conseiller**|**sysname**|Nom du serveur de distribution.|  
|**base de données de distribution**|**sysname**|Nom de la base de données de distribution.|  
|**Directory**|**nvarchar(255)**|Nom du répertoire de travail.|  
|**account**|**nvarchar(255)**|Nom du compte d'utilisateur Windows.|  
|**min distrib retention**|**int**|Période de rétention de distribution minimale.|  
|**max distrib retention**|**int**|Période maximale de rétention de distribution.|  
|**history retention**|**int**|Période de rétention de l'historique.|  
|**history cleanup agent**|**nvarchar(100**|Nom de l'Agent de nettoyage de l'historique|  
|**distribution cleanup agent**|**nvarchar(100**|Nom de l'Agent de nettoyage de distribution.|  
|**rpc server name**|**sysname**|Nom du serveur de distribution local ou distant.|  
|**rpc login name**|**sysname**|Connexion utilisée pour les appels de procédure à distance au serveur de distribution distant.|  
|**type de serveur de publication**|**sysname**|Type de serveur de publication ; il peut s'agir d'une des valeurs suivantes :<br /><br /> **MSSQLSERVER**<br /><br /> **SOLUTION**<br /><br /> **ORACLE GATEWAY**|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpdistributor** est utilisé dans tous les types de réplications.  
  
 Si un ou plusieurs paramètres de sortie sont spécifiés lors de l’exécution de **sp_helpdistributor**, tous les paramètres de sortie définis sur null reçoivent des valeurs à la sortie et aucun jeu de résultats n’est retourné. Si aucun paramètre de sortie n'est spécifié, un ensemble de résultats est retourné.  
  
## <a name="permissions"></a>Autorisations  
 Les colonnes de l’ensemble de résultats suivantes ou les paramètres de sortie sont retournés aux membres du rôle serveur fixe **sysadmin** sur le serveur de publication et le rôle de base de données fixe **db_owner** sur la base de données de publication :  
  
|Colonne de l'ensemble de résultats|Paramètre de sortie|  
|-----------------------|----------------------|  
|account|**\@compte**|  
|min distrib retention|**\@min_distretention**|  
|max distrib retention|**\@max_distretention**|  
|history retention|**\@history_retention**|  
|history cleanup agent|**\@history_cleanupagent**|  
|distribution cleanup agent|**\@distrib_cleanupagent**|  
|rpc login name|aucun|  
  
 La colonne de l'ensemble de résultats suivante est retournée aux utilisateurs dans la liste d'accès aux publications sur le serveur de distribution :  
  
-   directory  
  
 Les colonnes de l'ensemble de résultats suivantes sont retournées à tous les utilisateurs.  
  
|Colonne de l'ensemble de résultats|Paramètre de sortie|  
|-----------------------|----------------------|  
|distributor|**\@conseiller**|  
|distribution database|**\@distribdb**|  
|rpc server name|**\@rpcsrvname**|  
|publisher type|**\@publisher_type**|  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un serveur de distribution et d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)  
  
  
