---
description: sp_helpmergepullsubscription (Transact-SQL)
title: sp_helpmergepullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepullsubscription
- sp_helpmergepullsubscription_TSQL
helpviewer_keywords:
- sp_helpmergepullsubscription
ms.assetid: 6f3125f3-0dfa-40bd-b725-8aa1591234f6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 91266e8d825fd669979e925753804230fd62027c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160598"
---
# <a name="sp_helpmergepullsubscription-transact-sql"></a>sp_helpmergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie des informations sur des abonnements par extraction de données (pull) existant sur l'Abonné. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpmergepullsubscription [ [ @publication=] 'publication']  
    [ , [ @publisher=] 'publisher']  
    [ , [ @publisher_db=] 'publisher_db']  
    [ , [ @subscription_type=] 'subscription_type']  
```  
  
## <a name="argument"></a>Argument  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec la valeur par défaut **%** . Si la *publication* est **%** , les informations sur toutes les publications de fusion et tous les abonnements de la base de données actuelle sont retournées.  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, avec la valeur par défaut **%** .  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, avec la valeur par défaut **%** .  
  
`[ @subscription_type = ] 'subscription_type'` Indique s’il faut afficher les abonnements par extraction. *subscription_type* est de type **nvarchar (10)**, avec **« pull »** comme valeur par défaut. Les valeurs valides sont **'push'**, **'pull'** ou **'both'**.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**subscription_name**|**nvarchar(1000)**|Nom de l'abonnement.|  
|**édition**|**sysname**|Nom de la publication.|  
|**publisher**|**sysname**|Nom du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**côté**|**sysname**|Nom de l'Abonné.|  
|**subscription_db**|**sysname**|Nom de la base de données d'abonnement.|  
|**statut**|**int**|État de l'abonnement :<br /><br /> **0** = abonnement inactif<br /><br /> **1** = abonnement actif<br /><br /> **2** = abonnement supprimé<br /><br /> **3** = abonnement détaché<br /><br /> **4** = abonnement attaché<br /><br /> **5** = abonnement marqué pour réinitialisation avec chargement<br /><br /> **6** = échec de l’attachement de l’abonnement<br /><br /> **7** = abonnement restauré à partir d’une sauvegarde|  
|**subscriber_type**|**int**|Type d'Abonné :<br /><br /> **1** = global<br /><br /> **2** = local<br /><br /> **3** = anonyme|  
|**subscription_type**|**int**|Type d'abonnement :<br /><br /> **0** = Push<br /><br /> **1** = extraction<br /><br /> **2** = anonyme|  
|**priority**|**float (8)**|Priorité de l'abonnement. La valeur doit être inférieure à **100,00**.|  
|**sync_type**|**tinyint**|Type de synchronisation d'abonnement :<br /><br /> **1** = automatique<br /><br /> **2** = la capture instantanée n’est pas utilisée.|  
|**description**|**nvarchar(255)**|Brève description de l’abonnement par extraction.|  
|**merge_jobid**|**Binary(16**|ID de travail de l'Agent de fusion.|  
|**enabled_for_syncmgr**|**int**|Indique si l'abonnement peut être synchronisé à l'aide du gestionnaire de synchronisation de [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|**last_updated**|**nvarchar (26)**|Date et heure de la dernière synchronisation de l'abonnement effectuée par l'Agent de fusion.|  
|**publisher_login**|**sysname**|Nom de connexion de l’éditeur.|  
|**publisher_password**|**sysname**|Mot de passe de l’éditeur.|  
|**publisher_security_mode**|**int**|Spécifie le mode de sécurité du serveur de publication :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification<br /><br /> **1** = authentification Windows|  
|**conseiller**|**sysname**|Nom du serveur de distribution.|  
|**distributor_login**|**sysname**|Nom de connexion du serveur de distribution.|  
|**distributor_password**|**sysname**|Mot de passe du serveur de distribution.|  
|**distributor_security_mode**|**int**|Spécifie le mode de sécurité du serveur de distribution :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification<br /><br /> **1** = authentification Windows|  
|**ftp_address**|**sysname**|Disponible pour compatibilité descendante uniquement. Adresse réseau du service FTP (File Transfer Protocol) pour le serveur de distribution.|  
|**ftp_port**|**int**|Disponible pour compatibilité descendante uniquement. Numéro de port du service FTP du serveur de distribution.|  
|**ftp_login**|**sysname**|Disponible pour compatibilité descendante uniquement. Nom d’utilisateur utilisé pour la connexion au service FTP.|  
|**ftp_password**|**sysname**|Disponible pour compatibilité descendante uniquement. Mot de passe de l'utilisateur utilisé pour la connexion au service FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Emplacement de stockage du dossier d'instantané si cet emplacement est différent ou en complément de l'emplacement par défaut.|  
|**working_directory**|**nvarchar(255)**|Chemin complet du répertoire dans lequel les fichiers d'instantané sont transférés via FTP lorsque cette option est spécifiée.|  
|**use_ftp**|**bit**|Indique si l'abonnement à la publication s'effectue via Internet, et si les propriétés d'adressage FTP sont configurées. Si la **valeur est 0**, l’abonnement n’utilise pas FTP. Si la condition est **1**, l’abonnement utilise FTP.|  
|**offload_agent**|**bit**|Indique si l'Agent peut être activé et exécuté à distance. Si la **valeur est 0**, l’agent ne peut pas être activé à distance.|  
|**offload_server**|**sysname**|Nom du serveur utilisé pour l'activation à distance.|  
|**use_interactive_resolver**|**int**|Indique si le composant résolveur interactif est utilisé ou non au cours de la réconciliation. Si la **valeur est 0**, le programme de résolution interactif n’est pas utilisé.|  
|**subid**|**uniqueidentifier**|ID de l'Abonné.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Chemin d'accès du dossier dans lequel les fichiers d'instantané sont enregistrés.|  
|**last_sync_status**|**int**|État de la synchronisation :<br /><br /> **1** = à partir de<br /><br /> **2** = réussite<br /><br /> **3** = en cours<br /><br /> **4** = inactif<br /><br /> **5** = nouvelle tentative après un échec précédent<br /><br /> **6** = échec<br /><br /> **7** = échec de validation<br /><br /> **8** = validation passée<br /><br /> **9** = arrêt demandé|  
|**last_sync_summary**|**sysname**|Description des résultats de la dernière synchronisation.|  
|**use_web_sync**|**bit**|Spécifie si l’abonnement peut être synchronisé via HTTPs, où la valeur **1** signifie que cette fonctionnalité est activée.|  
|**internet_url**|**nvarchar(260)**|URL qui représente l'emplacement de l'écouteur de réplication pour la synchronisation Web.|  
|**internet_login**|**nvarchar(128)**|Connexion que l'Agent de fusion utilise pour se connecter, à l'aide de l'authentification de base, au serveur Web qui héberge la synchronisation Web.|  
|**internet_password**|**nvarchar (524)**|Mot de passe de la connexion que l'Agent de fusion utilise pour se connecter, à l'aide de l'authentification de base, au serveur Web qui héberge la synchronisation Web.|  
|**internet_security_mode**|**int**|Mode d'authentification utilisé pour se connecter au serveur Web hôte de la synchronisation Web. La valeur **1** signifie l’authentification Windows et la valeur **0** signifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.|  
|**internet_timeout**|**int**|Délai en secondes avant l'expiration d'une demande de synchronisation Web.|  
|**hostname**|**nvarchar(128)**|Spécifie une valeur surchargée pour [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) lorsque cette fonction est utilisée dans la clause WHERE d’un filtre de lignes paramétrable.|  
|**job_login**|**nvarchar(512)**|Compte Windows sous lequel l’agent de fusion s’exécute, qui est retourné au format *domaine* \\ *nom d’utilisateur*.|  
|**job_password**|**sysname**|Pour des raisons de sécurité, la valeur « **\*\*\*\*\*\*\*\*\*\*** » est toujours retournée.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpmergepullsubscription** est utilisé dans la réplication de fusion. Dans le jeu de résultats, la date retournée dans **last_updated** est au format *AAAAMMJJ hh : mm : SS. fff*.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** et du rôle de base de données fixe **db_owner** peuvent exécuter **sp_helpmergepullsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_changemergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
