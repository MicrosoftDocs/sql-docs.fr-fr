---
description: sp_change_subscription_properties (Transact-SQL)
title: sp_change_subscription_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_change_subscription_properties_TSQL
- sp_change_subscription_properties
helpviewer_keywords:
- sp_change_subscription_properties
ms.assetid: cf8137f9-f346-4aa1-ae35-91a2d3c16f17
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed6615b10487744ae5ac5c0775627bea97e29c66
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208298"
---
# <a name="sp_change_subscription_properties-transact-sql"></a>sp_change_subscription_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Met à jour les informations pour les abonnements par extraction de données (pull). Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_change_subscription_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @property = ] 'property'  
        , [ @value = ] 'value'  
    [ , [ @publication_type = ] publication_type ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @property = ] 'property'` Propriété à modifier. *Property* est de **type sysname**.  
  
`[ @value = ] 'value'` Nouvelle valeur de la propriété. la *valeur* est de type **nvarchar (1000)**, sans valeur par défaut.  
  
`[ @publication_type = ] publication_type` Spécifie le type de réplication de la publication. *publication_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Type de publication|  
|-----------|----------------------|  
|**0**|Transactionnelle|  
|**1**|Instantané|  
|**2**|Fusionner|  
|NULL (par défaut)|La réplication détermine le type de publication. La procédure stockée devant consulter plusieurs tables, cette option est plus lente que lorsque le type de publication exact est fourni.|  
  
 Le tableau ci-dessous décrit les propriétés des articles et les valeurs de ces propriétés.  
  
|Propriété|Valeur|Description|  
|--------------|-----------|-----------------|  
|**alt_snapshot_folder**||Indique l'emplacement du dossier de remplacement pour l'instantané. Si l'argument est défini à NULL, les fichiers d'instantané sont prélevés à l'emplacement par défaut spécifié par le serveur de publication.|  
|**distrib_job_login**||Nom de connexion du compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l'Agent s'exécute.|  
|**distrib_job_password**||Mot de passe du compte Windows sous lequel l’agent s’exécute.|  
|**distributor_login**||Connexion au serveur de distribution.|  
|**distributor_password**||Mot de passe du serveur de distribution.|  
|**distributor_security_mode**|**1**|Utilise l'authentification Windows pour la connexion au serveur de distribution.|  
||**0**|Utilise l'authentification  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la connexion au serveur de distribution.|  
|**dts_package_name**||Définit le nom du package DTS (Data Transformation Services) SQL Server 2000. Cette valeur peut être spécifiée seulement s'il s'agit d'une publication transactionnelle ou d'instantané.|  
|**dts_package_password**||Spécifie le mot de passe du package. *dts_package_password* est de **type sysname** , avec NULL comme valeur par défaut, qui spécifie que la propriété de mot de passe doit rester inchangée.<br /><br /> Remarque : un package DTS doit avoir un mot de passe.<br /><br /> Cette valeur peut être spécifiée seulement s'il s'agit d'une publication transactionnelle ou d'instantané.|  
|**dts_package_location**||Emplacement où le package DTS est stocké. Cette valeur peut être spécifiée seulement s'il s'agit d'une publication transactionnelle ou d'instantané.|  
|**dynamic_snapshot_location**||Spécifie le chemin d'accès au dossier où les fichiers d'instantané sont enregistrés. Cette valeur peut être spécifiée seulement s'il s'agit d'une publication de fusion.|  
|**ftp_address**||Pour compatibilité descendante uniquement.|  
|**ftp_login**||Pour compatibilité descendante uniquement.|  
|**ftp_password**||Pour compatibilité descendante uniquement.|  
|**ftp_port**||Pour compatibilité descendante uniquement.|  
|**hostname**||Nom d’hôte utilisé lors de la connexion au serveur de publication.|  
|**internet_login**||Connexion que l'Agent de fusion utilise pour se connecter, à l'aide de l'authentification de base, au serveur Web qui héberge la synchronisation Web.|  
|**internet_password**||Mot de passe qu'utilise l'Agent de fusion lors de la connexion au serveur Web qui héberge la synchronisation Web avec l'authentification de base.|  
|**internet_security_mode**|**1**|Utilise l'authentification intégrée Windows pour la synchronisation Web. Il est recommandé d'utiliser l'authentification de base pour la synchronisation Web. Pour plus d’informations, consultez [Configurer la synchronisation Web](../../relational-databases/replication/configure-web-synchronization.md).|  
||**0**|Utiliser l'authentification de base pour la synchronisation Web.<br /><br /> Remarque : la synchronisation Web requiert une connexion TLS au serveur Web.|  
|**internet_timeout**||Délai en secondes avant l'expiration d'une demande de synchronisation Web.|  
|**internet_url**||URL qui représente l'emplacement de l'écouteur de réplication pour la synchronisation Web.|  
|**merge_job_login**||Connexion pour le compte Windows sous lequel l’agent s’exécute.|  
|**merge_job_password**||Mot de passe du compte Windows sous lequel l’agent s’exécute.|  
|**publisher_login**||Nom de connexion du serveur de publication La modification de *publisher_login* est prise en charge uniquement pour les abonnements aux publications de fusion.|  
|**publisher_password**||Mot de passe de l’éditeur. La modification de *publisher_password* est prise en charge uniquement pour les abonnements aux publications de fusion.|  
|**publisher_security_mode**|**1**|Utiliser l'authentification Windows pour la connexion au serveur de publication. La modification de *publisher_security_mode* est prise en charge uniquement pour les abonnements aux publications de fusion.|  
||**0**|Utiliser l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la connexion au serveur de publication.|  
|**use_ftp**|**true**|Utiliser FTP au lieu du protocole standard pour extraire les instantanés.|  
||**false**|Utiliser le protocole standard pour extraire les instantanés.|  
|**use_web_sync**|**true**|Active la synchronisation Web.|  
||**false**|Désactive la synchronisation Web.|  
|**working_directory**||Nom du répertoire de travail utilisé pour stocker temporairement les fichiers de données et de schéma de la publication lorsque le protocole FTP (File Transfer Protocol) est utilisé pour transférer des fichiers d'instantané.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_change_subscription_properties** est utilisé dans tous les types de réplications.  
  
 **sp_change_subscription_properties** est utilisé pour les abonnements par extraction.  
  
 Pour les serveurs de publication Oracle, la valeur de *publisher_db* est ignorée, car Oracle n’autorise qu’une seule base de données par instance du serveur.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_change_subscription_properties**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un abonnement par extraction](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [sp_addmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)   
 [sp_addpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
