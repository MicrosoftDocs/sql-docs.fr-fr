---
description: syspublications (vue système) (Transact-SQL)
title: SYSPUBLICATIONS (vue système) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- syspublications
- syspublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspublications view
ms.assetid: e5f57c32-efc0-4455-a74f-684dc2ae51f8
author: stevestein
ms.author: sstein
ms.openlocfilehash: 9a17f9371d82ca50a1a27ef0acb0e7281a26fb92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181557"
---
# <a name="syspublications-system-view-transact-sql"></a>syspublications (vue système) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vue **SYSPUBLICATIONS** expose les informations de publication. Cette vue est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**description**|**nvarchar(255)**|Entrée descriptive de la publication.|  
|**name**|**sysname**|Nom unique associé à la publication.|  
|**pubid**|**int**|Colonne d'identité fournissant un ID unique pour la publication.|  
|**repl_freq**|**tinyint**|Fréquence de réplication :<br /><br /> **0** = basé sur la transaction (transactionnel).<br /><br /> **1** = actualisation planifiée de la table (instantané).|  
|**statut**|**tinyint**|État de la publication :<br /><br /> **0** = inactif.<br /><br /> **1** = actif.|  
|**sync_method**|**tinyint**|Méthode de synchronisation :<br /><br /> **0** = utilitaire de copie en bloc natif (BCP).<br /><br /> **1** = BCP de caractère.<br /><br /> **3** = simultané, ce qui signifie que le BCP natif est utilisé, mais que les tables ne sont pas verrouillées au cours de l’instantané.<br /><br /> **4** = concurrent_c, ce qui signifie que le caractère BCP est utilisé, mais que les tables ne sont pas verrouillées au cours de l’instantané.|  
|**snapshot_jobid**|**Binary(16**|Identifie le travail d'agent planifié pour créer l'instantané initial.|  
|**independent_agent**|**bit**|Spécifie s’il existe un Agent de distribution autonome pour cette publication.<br /><br /> **0** = la publication utilise un agent de distribution partagé, et chaque paire base de données du serveur de publication/base de données de l’abonné possède un seul agent partagé.<br /><br /> **1** = il existe un agent de distribution autonome pour cette publication.|  
|**immediate_sync**|**bit**|Indique si les fichiers de synchronisation sont créés ou recréés à chaque exécution du Agent d’instantané, où **1** signifie qu’ils sont créés chaque fois que l’agent s’exécute.|  
|**enabled_for_internet**|**bit**|Indique si les fichiers de synchronisation de la publication sont exposés à Internet via le protocole FTP (File Transfer Protocol) et d’autres services, où **1** signifie qu’ils sont accessibles à partir d’Internet.|  
|**allow_push**|**bit**|Indique si les abonnements par envoi de notification sont autorisés sur la publication, où **1** signifie qu’ils sont autorisés.|  
|**allow_pull**|**bit**|Indique si les abonnements par extraction sont autorisés sur la publication, où **1** signifie qu’ils sont autorisés.|  
|**allow_anonymous**|**bit**|Indique si les abonnements anonymes sont autorisés sur la publication, où **1** signifie qu’ils sont autorisés.|  
|**immediate_sync_ready**|**bit**|Indique si l'instantané a été généré par l'Agent d'instantané et peut être utilisé par les nouveaux abonnements. Cette colonne n'est pertinente que pour les publications à mise à jour immédiate. **1** indique que l’instantané est prêt.|  
|**allow_sync_tran**|**bit**|Indique si les abonnements de mise à jour immédiate sont autorisés pour la publication. **1** signifie que les abonnements avec mise à jour immédiate sont autorisés.|  
|**autogen_sync_procs**|**bit**|Indique si la procédure stockée de synchronisation pour les abonnements de mise à jour immédiate est générée par le serveur de distribution. **1** signifie qu’il est généré sur le serveur de publication.|  
|**fixation**|**int**|Durée, en heures, pendant laquelle les modifications de la publication sont conservées dans la base de données de distribution.|  
|**allow_queued_tran**|**bit**|Indique si la mise en file d'attente des modifications sur l'Abonné jusqu'à leur application sur le serveur de publication est activée. Si **1**, les modifications sur l’abonné sont mises en file d’attente.|  
|**snapshot_in_defaultfolder**|**bit**|Spécifie si les fichiers d’instantanés sont stockés dans le dossier par défaut. Si la **valeur est 0**, les fichiers d’instantanés ont été stockés à l’emplacement secondaire spécifié par *alternate_snapshot_folder*. Si la valeur est égale à 1, les fichiers d'instantané se trouvent dans le dossier par défaut.|  
|**alt_snapshot_folder**|**nvarchar (510)**|Indique l'emplacement du dossier de remplacement pour l'instantané.|  
|**pre_snapshot_script**|**nvarchar (510)**|Spécifie un pointeur vers un emplacement de fichier **. SQL** . L'Agent de distribution exécute le script de pré-instantané avant toute exécution de scripts d'objets répliqués, lors de l'application d'un instantané sur un Abonné.|  
|**post_snapshot_script**|**nvarchar (510)**|Spécifie un pointeur vers un emplacement de fichier **. SQL** . L'Agent de distribution exécute le script de post-instantané après que tous les autres scripts d'objets et les données répliqués ont été appliqués lors d'une synchronisation initiale.|  
|**compress_snapshot**|**bit**|Spécifie que l’instantané écrit dans l’emplacement de *alt_snapshot_folder* doit être compressé au [!INCLUDE[msCoName](../../includes/msconame-md.md)] format cab. **1** signifie que l’instantané sera compressé.|  
|**ftp_address**|**sysname**|Adresse réseau du service FTP du serveur de distribution. Spécifie l'emplacement d'où l'Agent de distribution peut extraire les fichiers d'instantané de la publication.|  
|**ftp_port**|**int**|Numéro de port du service FTP du serveur de distribution. Spécifie l’emplacement des fichiers d’instantané de la publication pour le Agent de distribution à récupérer.|  
|**ftp_subdirectory**|**nvarchar (510)**|Spécifie l'emplacement d'où l'Agent de distribution peut extraire les fichiers d'instantané si la publication prend en charge la propagation d'instantanés via FTP.|  
|**ftp_login**|**nvarchar (256)**|Nom d'utilisateur, utilisé pour la connexion au service FTP.|  
|**ftp_password**|**nvarchar (1048)**|Mot de passe de l’utilisateur utilisé pour se connecter au service FTP.|  
|**allow_dts**|**bit**|Spécifie si la publication autorise les transformations DTS (Data Transformation Services) [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. **1** spécifie que les transformations DTS sont autorisées.|  
|**allow_subscription_copy**|**bit**|Spécifie si la possibilité de copier les bases de données d'abonnement qui s'abonnent à cette publication a été activée. **1** signifie que la copie est autorisée.|  
|**centralized_conflicts**|**bit**|Spécifie si les enregistrements en conflit sont stockés sur le serveur de publication :<br /><br /> **0** = les enregistrements en conflit sont stockés sur le serveur de publication et sur l’abonné à l’origine du conflit.<br /><br /> **1** = les enregistrements en conflit sont stockés sur le serveur de publication.|  
|**conflict_retention**|**int**|Spécifie (en jours) la durée de rétention des enregistrements des conflits.|  
|**conflict_policy**|**int**|Spécifie la stratégie de résolution de conflits à suivre lorsque l'option d'abonné avec mise à jour en attente est utilisée. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** = le serveur de publication gagne le conflit.<br /><br /> **2** = l’abonné gagne le conflit.<br /><br /> **3** = l’abonnement est réinitialisé.|  
|**queue_type**|**int**|Spécifie le type de file d'attente utilisé. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** =. MSMQ, qui utilise [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing pour stocker les transactions.<br /><br /> **2** =. SQL, qui utilise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour stocker les transactions.<br /><br /> Remarque : l’utilisation [!INCLUDE[msCoName](../../includes/msconame-md.md)] de Message Queuing est dépréciée et n’est plus prise en charge.|  
|**ad_guidname**|**sysname**|Spécifie si la publication est publiée dans l'annuaire [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. Un identificateur global unique (GUID) valide indique que la publication est publiée dans l'annuaire Active Directory ; le GUID correspond alors à l'objet de publication Active Directory objectGUID. Si la valeur est NULL, la publication n'est pas publiée dans l'annuaire Active Directory.<br /><br /> Remarque : la publication sur Active Directory n’est plus prise en charge.|  
|**backward_comp_level**|**int**|Le niveau de compatibilité des bases de données peut avoir une des valeurs suivantes :<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .|  
|**allow_initialize_from_backup**|**bit**|Indique si les Abonnés peuvent initialiser un abonnement à cette publication à partir d'une sauvegarde plutôt que d'un instantané initial. **1** signifie que les abonnements peuvent être initialisés à partir d’une sauvegarde, et **0** signifie qu’ils ne le peuvent pas. Pour plus d’informations, consultez [Initialiser un abonnement transactionnel sans instantané](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).|  
|**min_autonosync_lsn**|**binaire (1)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**replicate_ddl**|**int**|Indique si la réplication de schéma est prise en charge pour la publication.<br /><br /> **1** = les instructions DDL exécutées sur le serveur de publication sont répliquées.<br /><br /> **0** = indique que les instructions DDL ne sont pas répliquées. Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).|  
|**options**|**int**|Bitmap qui spécifie d'autres options de publication. Les valeurs des options au niveau des bits sont les suivantes :<br /><br /> **0x1** -activé pour la réplication d’égal à égal.<br /><br /> **0X2** -publie uniquement les modifications locales pour la réplication d’égal à égal.<br /><br /> **0x4** -activée pour les abonnés non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> **0x8** -activé pour la détection de conflit d’égal à égal.|  
|**originator_id**|**smallint**|Identifie chaque nœud dans la topologie de réplication d'égal à égal pour les besoins de la détection de conflit. Pour plus d’informations, voir [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_addpublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)  
  
  
