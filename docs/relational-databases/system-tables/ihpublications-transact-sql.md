---
description: IHpublications (Transact-SQL)
title: IHpublications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHpublications_TSQL
- IHpublications
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublications system table
ms.assetid: b519a101-fa53-44be-bd55-6ea79245b5d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: a97462b4224244633a945f2108370b8c4f549f63
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098238"
---
# <a name="ihpublications-transact-sql"></a>IHpublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table système **IHpublications** contient une ligne pour chaque publication non SQL Server à l’aide du serveur de distribution actuel. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**pubid**|**int**|Colonne d'identité fournissant un ID unique pour la publication.|  
|**name**|**sysname**|Nom unique associé à la publication.|  
|**repl_freq**|**tinyint**|Fréquence de réplication :<br /><br /> **0** = basé sur la transaction.<br /><br /> **1** = actualisation planifiée de la table.|  
|**statut**|**tinyint**|État de la publication pouvant prendre la valeur :<br /><br /> **0** = inactif.<br /><br /> **1** = actif.|  
|**sync_method**|**tinyint**|Méthode de synchronisation :<br /><br /> **1** = copie en bloc de caractères.<br /><br /> **4** = concurrent_c, ce qui signifie que la copie en bloc de caractères est utilisée, mais que les tables ne sont pas verrouillées au cours de l’instantané.|  
|**snapshot_jobid**|**binary**|ID de tâche planifiée.|  
|**enabled_for_internet**|**bit**|Indique si les fichiers de synchronisation de la publication sont exposés à Internet via FTP et d’autres services, où **1** signifie qu’ils sont accessibles à partir d’Internet.|  
|**immediate_sync_ready**|**bit**|Indique si les fichiers de synchronisation sont disponibles, où **1** signifie qu’ils sont disponibles. *Non pris en charge pour les serveurs de publication non SQL.*|  
|**allow_queued_tran**|**bit**|Indique si la mise en file d'attente des modifications sur l'Abonné jusqu'à leur application sur le serveur de publication est activée. Si **1**, les modifications sur l’abonné sont mises en file d’attente. *Non pris en charge pour les serveurs de publication non SQL.*|  
|**allow_sync_tran**|**bit**|Indique si les abonnements de mise à jour immédiate sont autorisés pour la publication. **1** signifie que les abonnements avec mise à jour immédiate sont autorisés. *Non pris en charge pour les serveurs de publication non SQL.*|  
|**autogen_sync_procs**|**bit**|Indique si la procédure stockée de synchronisation pour les abonnements de mise à jour immédiate est générée par le serveur de distribution. **1** signifie qu’il est généré sur le serveur de publication. *Non pris en charge pour les serveurs de publication non SQL.*|  
|**snapshot_in_defaultfolder**|**bit**|Spécifie si les fichiers d’instantanés sont stockés dans le dossier par défaut. Si la **valeur est 0**, les fichiers d’instantanés ont été stockés à l’emplacement secondaire spécifié par *alternate_snapshot_folder*. Si la valeur est **1**, les fichiers d’instantanés se trouvent dans le dossier par défaut.|  
|**alt_snapshot_folder**|**nvarchar (510)**|Indique l'emplacement du dossier de remplacement pour l'instantané.|  
|**pre_snapshot_script**|**nvarchar (510)**|Spécifie un pointeur vers un emplacement de fichier **. SQL** . L'Agent de distribution exécute le script de pré-instantané avant toute exécution de scripts d'objets répliqués, lors de l'application d'un instantané sur un Abonné.|  
|**post_snapshot_script**|**nvarchar (510)**|Spécifie un pointeur vers un emplacement de fichier **. SQL** . L'Agent de distribution exécute le script de post-instantané après que tous les autres scripts d'objets et les données répliqués ont été appliqués lors d'une synchronisation initiale.|  
|**compress_snapshot**|**bit**|Spécifie que l’instantané écrit dans l’emplacement de *alt_snapshot_folder* doit être compressé au [!INCLUDE[msCoName](../../includes/msconame-md.md)] format cab. **0** indique que l’instantané ne sera pas compressé.|  
|**ftp_address**|**sysname**|Adresse réseau du service FTP du serveur de distribution. Spécifie l'emplacement d'où l'Agent de distribution peut extraire les fichiers d'instantané de la publication.|  
|**ftp_port**|**int**|Numéro de port du service FTP du serveur de distribution. Indique l'emplacement à partir duquel l'Agent de distribution peut extraire les fichiers d'instantané de la publication.|  
|**ftp_subdirectory**|**nvarchar (510)**|Spécifie l'emplacement d'où l'Agent de distribution peut extraire les fichiers d'instantané si la publication prend en charge la propagation d'instantanés via FTP.|  
|**ftp_login**|**nvarchar (256)**|Nom d'utilisateur, utilisé pour la connexion au service FTP.|  
|**ftp_password**|**nvarchar (1048)**|Mot de passe de l’utilisateur utilisé pour se connecter au service FTP.|  
|**allow_dts**|**bit**|Indique que la publication autorise les transformations de données. **1** spécifie que les transformations DTS sont autorisées. *Non pris en charge pour les serveurs de publication non SQL.*|  
|**allow_anonymous**|**bit**|Indique si les abonnements anonymes sont autorisés sur la publication, où **1** signifie qu’ils sont autorisés.|  
|**centralized_conflicts**|**bit**|Spécifie si les enregistrements en conflit sont stockés sur le serveur de publication :<br /><br /> **0** = les enregistrements en conflit sont stockés sur le serveur de publication et sur l’abonné à l’origine du conflit.<br /><br /> **1** = les enregistrements en conflit sont stockés sur le serveur de publication.<br /><br /> *Non pris en charge pour les serveurs de publication non SQL.*|  
|**conflict_retention**|**int**|Spécifie la durée de rétention des conflits en jours. *Non pris en charge pour les serveurs de publication non SQL.*|  
|**conflict_policy**|**int**|Spécifie la stratégie de résolution de conflits à suivre lorsque l'option d'abonné avec mise à jour en attente est utilisée. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** = le serveur de publication gagne le conflit.<br /><br /> **2** = l’abonné gagne le conflit.<br /><br /> **3** = l’abonnement est réinitialisé.<br /><br /> *Non pris en charge pour les serveurs de publication non SQL.*|  
|**queue_type**|**int**|Spécifie le type de file d'attente utilisé. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** = MSMQ, qui utilise [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing pour stocker les transactions.<br /><br /> **2** = SQL, qui utilise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour stocker les transactions.<br /><br /> Cette colonne n’est pas utilisée par les serveurs de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Remarque : l’utilisation [!INCLUDE[msCoName](../../includes/msconame-md.md)] de Message Queuing est dépréciée et n’est plus prise en charge.<br /><br /> *Cette colonne n’est pas prise en charge pour les serveurs de publication non-SQL.*|  
|**ad_guidname**|**sysname**|Spécifie si la publication est publiée dans l'annuaire [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. Un identificateur global unique (GUID) valide spécifie que la publication est publiée dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory, et le GUID correspond à l' **objectGUID** d’objet de publication Active Directory correspondante. Si la valeur est NULL, la publication n'est pas publiée dans l'annuaire [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. *Non pris en charge pour les serveurs de publication non SQL.*|  
|**backward_comp_level**|**int**|Le niveau de compatibilité des bases de données peut avoir une des valeurs suivantes :<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .<br /><br /> *Non pris en charge pour les serveurs de publication non SQL.*|  
|**description**|**nvarchar(255)**|Entrée décrivant la publication.|  
|**independent_agent**|**bit**|Spécifie s’il existe un Agent de distribution autonome pour cette publication.<br /><br /> **0** = la publication utilise un agent de distribution partagé, et chaque paire base de données du serveur de publication/base de données de l’abonné possède un seul agent partagé.<br /><br /> **1** = il existe un agent de distribution autonome pour cette publication.|  
|**immediate_sync**|**bit**|Indique si les fichiers de synchronisation sont créés ou recréés à chaque exécution du Agent d’instantané, où **1** signifie qu’ils sont créés chaque fois que l’agent s’exécute.|  
|**allow_push**|**bit**|Indique si les abonnements par envoi de notification sont autorisés sur la publication, où **1** signifie qu’ils sont autorisés.|  
|**allow_pull**|**bit**|Indique si les abonnements par extraction sont autorisés sur la publication, où **1** signifie qu’ils sont autorisés.|  
|**fixation**|**int**|Quantité de modification, en heures, à enregistrer pour la publication donnée.|  
|**allow_subscription_copy**|**bit**|Spécifie si la possibilité de copier les bases de données d'abonnement qui s'abonnent à cette publication a été activée. **1** signifie que la copie est autorisée.|  
|**allow_initialize_from_backup**|**bit**|Indique si les Abonnés peuvent initialiser un abonnement à cette publication à partir d'une sauvegarde plutôt qu'à partir de son instantané initial. **1** signifie que les abonnements peuvent être initialisés à partir d’une sauvegarde, et **0** signifie qu’ils ne le peuvent pas. Pour plus d’informations, consultez [Initialiser un abonnement transactionnel sans instantané](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md). *Non pris en charge pour les serveurs de publication non SQL.*|  
|**min_autonosync_lsn**|**binaire (1)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**replicate_ddl**|**int**|Précise si la réplication de schéma est prise en charge pour la publication. **1** indique que les instructions DDL exécutées sur le serveur de publication sont répliquées, et **0** indique que les instructions DDL ne sont pas répliquées. Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md). *Non pris en charge pour les serveurs de publication non SQL.*|  
|**options**|**int**|Image précisant les options de publication supplémentaires, où les valeurs des options au niveau du bit peuvent être :<br /><br /> **0x1** -activé pour la réplication d’égal à égal.<br /><br /> **0X2** -publie uniquement les modifications locales.<br /><br /> **0x4** -activé pour les abonnés non-SQL Server.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addpublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [SYSPUBLICATIONS &#40;vue système&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/syspublications-system-view-transact-sql.md)   
 [SYSPUBLICATIONS &#40;Transact-SQL&#41;](../../relational-databases/system-tables/syspublications-transact-sql.md)  
  
  
