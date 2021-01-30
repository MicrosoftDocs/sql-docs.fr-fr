---
description: sp_helpsubscription_properties (Transact-SQL)
title: sp_helpsubscription_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscription_properties
- sp_helpsubscription_properties_TSQL
helpviewer_keywords:
- sp_helpsubscription_properties
ms.assetid: 7a76a645-97eb-47ac-b3ea-e2d75012cbed
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 681a72a604af765311bb10833cc17842819e97f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99103184"
---
# <a name="sp_helpsubscription_properties-transact-sql"></a>sp_helpsubscription_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Récupère des informations de sécurité à partir de la table [MSsubscription_properties](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) . Cette procédure stockée est exécutée sur l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpsubscription_properties [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db =] 'publisher_db' ]   
    [ , [ @publication =] 'publication' ]  
    [ , [ @publication_type = ] publication_type ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, avec la valeur par défaut **%** , qui retourne des informations sur tous les serveurs de publication.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, avec la valeur par défaut **%** , qui retourne des informations sur toutes les bases de données du serveur de publication.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec la valeur par défaut **%** , qui retourne des informations sur toutes les publications.  
  
`[ @publication_type = ] publication_type` Type de publication. *publication_type* est de **type int**, avec NULL comme valeur par défaut. S’il est fourni, *publication_type* doit prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Publication transactionnelle|  
|**1**|Publication d'instantané|  
|**2**|Publication de fusion|  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Nom du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**édition**|**sysname**|Nom de la publication.|  
|**publication_type**|**int**|Type de publication :<br /><br /> **0** = transactionnel<br /><br /> **1** = instantané<br /><br /> **2** = fusion|  
|**publisher_login**|**sysname**|ID de connexion utilisé côté serveur de publication pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**publisher_password**|**nvarchar (524)**|Mot de passe utilisé côté serveur de publication pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (chiffré).|  
|**publisher_security_mode**|**int**|Mode de sécurité utilisé sur le serveur de publication.<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification<br /><br /> **1** = authentification Windows|  
|**conseiller**|**sysname**|Nom du serveur de distribution.|  
|**distributor_login**|**sysname**|Connexion au serveur de distribution.|  
|**distributor_password**|**nvarchar (524)**|Mot de passe du serveur de distribution (chiffré).|  
|**distributor_security_mode**|**int**|Mode de sécurité utilisé sur le serveur de distribution.<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification<br /><br /> **1** = authentification Windows|  
|**ftp_address**|**sysname**|Pour compatibilité descendante uniquement. Adresse réseau du service FTP (File Transfer Protocol) du serveur de distribution.|  
|**ftp_port**|**int**|Pour compatibilité descendante uniquement. Numéro de port du service FTP du serveur de distribution.|  
|**ftp_login**|**sysname**|Pour compatibilité descendante uniquement. Nom d'utilisateur permettant la connexion au service FTP.|  
|**ftp_password**|**nvarchar (524)**|Pour compatibilité descendante uniquement. Mot de passe de l’utilisateur, utilisé pour la connexion au service FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Indique l'emplacement du dossier de remplacement pour l'instantané.|  
|**working_directory**|**nvarchar(255)**|Nom du répertoire de travail utilisé pour stocker les fichiers de schéma et de données.|  
|**use_ftp**|**bit**|Spécifie l’utilisation de FTP au lieu du protocole normal pour récupérer des instantanés. Si la **1** est utilisée, FTP est utilisé.|  
|**dts_package_name**|**sysame**|Spécifie le nom du package DTS (Data Transformation Services).|  
|**dts_package_password**|**nvarchar (524)**|Spécifie le mot de passe du package, s'il existe.|  
|**dts_package_location**|**int**|Emplacement où le package DTS est stocké.<br /><br /> **0** = l’emplacement du package est sur le serveur de distribution.<br /><br /> **1** = l’emplacement du package est sur l’abonné.|  
|**offload_agent**|**bit**|Indique si l'agent peut être activé à distance. Si la **valeur est 0**, l’agent ne peut pas être activé à distance.|  
|**offload_server**|**sysname**|Indique le nom de réseau du serveur utilisé pour l'activation à distance.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Spécifie le chemin d'accès au dossier où les fichiers d'instantané sont enregistrés.|  
|**use_web_sync**|**bit**|Spécifie si l’abonnement peut être synchronisé via HTTPs, où la valeur **1** signifie que cette fonctionnalité est activée.|  
|**internet_url**|**nvarchar(260)**|URL qui représente l'emplacement de l'écouteur de réplication pour la synchronisation Web.|  
|**internet_login**|**nvarchar(128)**|Connexion que l'Agent de fusion utilise pour se connecter, à l'aide de l'authentification de base, au serveur Web qui héberge la synchronisation Web.|  
|**internet_password**|**nvarchar (524)**|Mot de passe de la connexion que l'Agent de fusion utilise pour se connecter, à l'aide de l'authentification de base, au serveur Web qui héberge la synchronisation Web.|  
|**internet_security_mode**|**int**|Mode d’authentification utilisé lors de la connexion au serveur Web qui héberge la synchronisation Web, où la valeur **1** correspond à l’authentification Windows et la valeur **0** à l’authentification de base.|  
|**internet_timeout**|**int**|Délai en secondes avant l'expiration d'une demande de synchronisation Web.|  
|**hostname**|**nvarchar(128)**|Indique la valeur de HOST_NAME() lorsque cette fonction est utilisée dans la clause WHERE du filtre de lignes paramétrable.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpsubscription_properties** est utilisé dans la réplication d’instantané, la réplication transactionnelle et la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_helpsubscription_properties**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
