---
description: sp_helpsubscription (Transact-SQL)
title: sp_helpsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscription_TSQL
- sp_helpsubscription
helpviewer_keywords:
- sp_helpsubscription
ms.assetid: ff96bcbf-e2b9-4da8-8515-d80d4ce86c16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 73fc43bf20a9306d0224392d984b3f2bff74ec6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192896"
---
# <a name="sp_helpsubscription-transact-sql"></a>sp_helpsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Affiche des informations sur les abonnements associés à une publication, un article, un Abonné ou un ensemble d'abonnements particuliers. Cette procédure stockée est exécutée sur la base de données de publication d'un serveur de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpsubscription [ [ @publication = ] 'publication' ]   
    [ , [ @article = ] 'article' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @destination_db = ] 'destination_db' ]   
    [ , [ @found=] found OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication associée. *publication* est de **type sysname**, avec la valeur par défaut **%** , qui retourne toutes les informations d’abonnement pour ce serveur.  
  
`[ @article = ] 'article'` Nom de l’article. *article* est de **type sysname**, avec la valeur par défaut **%** , qui retourne toutes les informations d’abonnement pour les publications et les abonnés sélectionnés. Si la valeur est **All**, une seule entrée est retournée pour l’abonnement complet sur une publication.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné sur lequel les informations d’abonnement sont obtenues. *Subscriber* est de **type sysname**, avec la valeur par défaut **%** , qui retourne toutes les informations d’abonnement pour les publications et les articles sélectionnés.  
  
`[ @destination_db = ] 'destination_db'` Nom de la base de données de destination. *destination_db* est de **type sysname**, avec la valeur par défaut **%** .  
  
`[ @found = ] 'found'OUTPUT` Indicateur qui signale le retour de lignes. *valeur* de **type int** et paramètre de sortie, avec la valeur par défaut 23456.  
  
 **1** indique que la publication est trouvée.  
  
 **0** indique que la publication est introuvable.  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname** et sa valeur par défaut est le nom du serveur actuel.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être spécifié, sauf s’il s’agit d’un serveur de publication Oracle.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**côté**|**sysname**|Nom de l'Abonné.|  
|**édition**|**sysname**|Nom de la publication.|  
|**article**|**sysname**|Nom de l'article.|  
|**base de données de destination**|**sysname**|Nom de la base de données de destination où sont placées les données répliquées.|  
|**État de l’abonnement**|**tinyint**|État de l'abonnement :<br /><br /> **0** = inactif<br /><br /> **1** = abonné<br /><br /> **2** = actif|  
|**type de synchronisation**|**tinyint**|Type de synchronisation d'abonnement :<br /><br /> **1** = automatique<br /><br /> **2** = aucun|  
|**type d’abonnement**|**int**|Type d'abonnement :<br /><br /> **0** = Push<br /><br /> **1** = extraction<br /><br /> **2** = anonyme|  
|**full subscription**|**bit**|Indique si l'abonnement concerne tous les articles de la publication :<br /><br /> **0** = Non<br /><br /> **1** = Oui|  
|**nom de l’abonnement**|**nvarchar(255)**|Nom de l'abonnement.|  
|**mode de mise à jour**|**int**|**0** = lecture seule<br /><br /> **1** = abonnement avec mise à jour immédiate|  
|**distribution job id**|**Binary(16**|ID du travail de l'Agent de distribution.|  
|**loopback_detection**|**bit**|La détection de boucle détermine si l'Agent de distribution renvoie à l'Abonné les transactions émanant de ce dernier :<br /><br /> **0** = renvoie.<br /><br /> **1** = n’est pas renvoyé.<br /><br /> Utilisé avec la réplication transactionnelle bidirectionnelle. Pour plus d’informations, voir [Bidirectional Transactional Replication](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).|  
|**offload_enabled**|**bit**|Indique si l'exécution du déchargement d'un Agent de réplication est configurée pour être exécuté sur l'Abonné.<br /><br /> Si la **valeur est 0**, l’agent est exécuté sur le serveur de publication.<br /><br /> Si la condition est **1**, l’agent est exécuté sur l’abonné.|  
|**offload_server**|**sysname**|Nom du serveur activé pour l'activation d'Agent à distance. Si la valeur est NULL, le offload_server actuel listé dans [MSdistribution_agents](../../relational-databases/system-tables/msdistribution-agents-transact-sql.md) table est utilisé.|  
|**dts_package_name**|**sysname**|Spécifie le nom du package DTS (Data Transformation Services).|  
|**dts_package_location**|**int**|Emplacement du package DTS (si un lot est affecté à l'abonnement). S’il existe un package, la valeur **0** spécifie l’emplacement du package sur le serveur de **distribution**. La valeur **1** spécifie l' **abonné**.|  
|**subscriber_security_mode**|**smallint**|Est le mode de sécurité de l’abonné, où **1** correspond à l’authentification Windows et **0** à l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification.|  
|**subscriber_login**|**sysname**|Nom de connexion sur l'Abonné.|  
|**subscriber_password**||Le mot de passe réel de l'Abonné n'est jamais renvoyé. Le résultat est masqué par une chaîne «**&#42;&#42;&#42;&#42;&#42;&#42;**».|  
|**job_login**|**sysname**|Nom du compte Windows sous lequel l'Agent de distribution s'exécute.|  
|**job_password**||Le mot de passe réel du travail n'est jamais renvoyé. Le résultat est masqué par une chaîne «**&#42;&#42;&#42;&#42;&#42;&#42;**».|  
|**distrib_agent_name**|**nvarchar(100**|Nom du travail de l'Agent qui synchronise l'abonnement.|  
|**subscriber_type**|**tinyint**|Type d'Abonné, parmi les types suivants :<br /><br /> **0** = abonné SQL Server<br /><br /> **1** = serveur de source de données ODBC<br /><br /> **2** = base de données Microsoft Jet (déconseillée)<br /><br /> **3** = OLE DB fournisseur|  
|**subscriber_provider**|**sysname**|Identificateur de programme unique (PROGID) avec lequel le fournisseur OLE DB de la source de données non-SQL Server est inscrit.|  
|**subscriber_datasource**|**nvarchar(4000)**|Nom de la source de données tel qu'il est interprété par le fournisseur OLE DB.|  
|**subscriber_providerstring**|**nvarchar(4000)**|Chaîne de connexion propre au fournisseur OLE DB qui identifie la source de données.|  
|**subscriber_location**|**nvarchar(4000)**|Emplacement de la base de données tel qu'il est interprété par le fournisseur OLE DB.|  
|**subscriber_catalog**|**sysname**|Catalogue à utiliser lors d’une connexion au fournisseur OLE DB.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpsubscription** est utilisé dans la réplication transactionnelle et d’instantané.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution reviennent par défaut au rôle **public** . Seules les informations des abonnements qu'ils ont créés sont renvoyées aux utilisateurs. Les informations sur tous les abonnements sont retournées aux membres du rôle serveur fixe **sysadmin** sur le serveur de publication ou aux membres du rôle de base de données fixe **db_owner** sur la base de données de publication.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
