---
description: sp_helppullsubscription (Transact-SQL)
title: sp_helppullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helppullsubscription_TSQL
- sp_helppullsubscription
helpviewer_keywords:
- sp_helppullsubscription
ms.assetid: a0d9c3f1-1fe9-497c-8e2f-5b74f47a7346
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3e7711e7034190f5862d512c3da02df685c3aaf5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210851"
---
# <a name="sp_helppullsubscription-transact-sql"></a>sp_helppullsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Affiche des informations relatives à un ou plusieurs abonnements de l'Abonné. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helppullsubscription [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]   
    [ , [ @publication = ] 'publication' ]  
    [ , [ @show_push = ] 'show_push' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur distant. *Publisher* est de **type sysname**, avec la valeur par défaut **%** , qui retourne des informations pour tous les serveurs de publication.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, avec la valeur par défaut **%** , qui retourne toutes les bases de données du serveur de publication.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec la valeur par défaut **%** , qui retourne toutes les publications. Si ce paramètre est égal à ALL, seuls les abonnements par extraction avec independent_agent = **0** sont retournés.  
  
`[ @show_push = ] 'show_push'` Indique si tous les abonnements par envoi de notification doivent être retournés. *show_push* est de type **nvarchar (5)**, avec false comme valeur par défaut, qui ne retourne pas d’abonnements par envoi de notification.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Nom du serveur de publication.|  
|**base de données du serveur de publication**|**sysname**|Nom de la base de données du serveur de publication.|  
|**édition**|**sysname**|Nom de la publication.|  
|**independent_agent**|**bit**|Indique s'il existe un Agent de distribution autonome pour cette publication.|  
|**type d’abonnement**|**int**|Type d'abonnement à la publication.|  
|**agent de distribution**|**nvarchar(100**|Agent de distribution traitant l'abonnement.|  
|**publication description**|**nvarchar(255)**|Description de la publication.|  
|**last updating time**|**date**|Heure à laquelle les informations d'abonnement ont été mises à jour. Il s'agit d'une chaîne UNICODE de date ISO (114) et d'heure ODBC (121). Le format est aaaammjj hh:mi:sss.mmm où aaaa représente l'année, mm le mois, jj le jour, hh l'heure, mi les minutes, sss les secondes et mmm les millisecondes.|  
|**nom de l’abonnement**|**varchar (386)**|Nom de l'abonnement.|  
|**horodateur de la dernière transaction**|**varbinary(16)**|Horodateur de la dernière transaction dupliquée.|  
|**mode de mise à jour**|**tinyint**|Types de mise à jour autorisés|  
|**distribution agent job_id**|**int**|ID du travail de l'Agent de distribution.|  
|**enabled_for_synmgr**|**int**|Indique si l'abonnement peut être synchronisé à l'aide du gestionnaire de synchronisation de [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|**GUID de l’abonnement**|**Binary(16**|Identificateur global de la version d'abonnement associée à une publication|  
|**subid**|**Binary(16**|Identificateur global d'un abonnement anonyme|  
|**immediate_sync**|**bit**|Indique si les fichiers de synchronisation sont créés ou recréés à chaque exécution de l’Agent d'instantané.|  
|**connexion de l’éditeur**|**sysname**|ID de connexion utilisé côté serveur de publication pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**publisher password**|**nvarchar (524)**|Mot de passe (chiffré) utilisé côté serveur de publication pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**publisher security_mode**|**int**|Mode de sécurité implémenté sur le serveur de publication :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification<br /><br /> **1** = authentification Windows<br /><br /> **2** = les déclencheurs de synchronisation utilisent une entrée **sysservers** statique pour effectuer un appel de procédure distante (RPC), et le serveur de *publication* doit être défini dans la table **sysservers** en tant que serveur distant ou serveur lié.|  
|**conseiller**|**sysname**|Nom du serveur de distribution.|  
|**distributor_login**|**sysname**|ID de connexion utilisé côté serveur de distribution pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**distributor_password**|**nvarchar (524)**|Mot de passe (chiffré) utilisé sur le serveur de distribution pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.|  
|**distributor_security_mode**|**int**|Mode de sécurité implémenté sur le serveur de distribution :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification<br /><br /> **1** = authentification Windows|  
|**ftp_address**|**sysname**|Pour compatibilité descendante uniquement.|  
|**ftp_port**|**int**|Pour compatibilité descendante uniquement.|  
|**ftp_login**|**sysname**|Pour compatibilité descendante uniquement.|  
|**ftp_password**|**nvarchar (524)**|Pour compatibilité descendante uniquement.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Emplacement de stockage du dossier d'instantané si cet emplacement est différent ou en complément de l'emplacement par défaut.|  
|**working_directory**|**nvarchar(255)**|Chemin complet du répertoire dans lequel les fichiers d'instantané sont transférés via FTP (File Transfer Protocol) lorsque cette option est spécifiée.|  
|**use_ftp**|**bit**|L'abonnement souscrit à la publication via Internet et les propriétés d'adressage FTP sont configurées. Si la **valeur est 0**, l’abonnement n’utilise pas FTP. Si la condition est **1**, l’abonnement utilise FTP.|  
|**publication_type**|**int**|Indique le type de réplication de la publication :<br /><br /> **0** = réplication transactionnelle<br /><br /> **1** = réplication de capture instantanée<br /><br /> **2** = réplication de fusion|  
|**dts_package_name**|**sysname**|Spécifie le nom du package DTS (Data Transformation Services).|  
|**dts_package_location**|**int**|Emplacement auquel le package DTS est enregistré :<br /><br /> **0** = serveur de distribution<br /><br /> **1** = abonné|  
|**offload_agent**|**bit**|Indique si l'agent peut être activé à distance. Si la **valeur est 0**, l’agent ne peut pas être activé à distance.|  
|**offload_server**|**sysname**|Indique le nom de réseau du serveur utilisé pour l'activation à distance.|  
|**last_sync_status**|**int**|État de l'abonnement :<br /><br /> **0** = tous les travaux sont en attente de démarrage<br /><br /> **1** = un ou plusieurs travaux commencent<br /><br /> **2** = toutes les tâches ont été exécutées avec succès<br /><br /> **3** = au moins un travail est en cours d’exécution<br /><br /> **4** = tous les travaux sont planifiés et inactifs<br /><br /> **5** = au moins un travail tente de s’exécuter après un échec précédent<br /><br /> **6** = au moins un travail n’a pas réussi à s’exécuter correctement|  
|**last_sync_summary**|**sysname**|Description des résultats de la dernière synchronisation.|  
|**last_sync_time**|**datetime**|Heure à laquelle les informations d'abonnement ont été mises à jour. Il s'agit d'une chaîne UNICODE de date ISO (114) et d'heure ODBC (121). Le format est aaaammjj hh:mi:sss.mmm où aaaa représente l'année, mm le mois, jj le jour, hh l'heure, mi les minutes, sss les secondes et mmm les millisecondes.|  
|**job_login**|**nvarchar(512)**|Compte Windows sous lequel s’exécute l’agent de distribution, qui est retourné au format *domaine* \\ *nom d’utilisateur*.|  
|**job_password**|**sysname**|Pour des raisons de sécurité, la valeur « **\*\*\*\*\*\*\*\*\*\*** » est toujours retournée.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helppullsubscription** est utilisé dans la réplication transactionnelle et d’instantané.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_helppullsubscription** .  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_droppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
