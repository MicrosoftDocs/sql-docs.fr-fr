---
description: sysmergepublications (Transact-SQL)
title: sysmergepublications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergepublications
- sysmergepublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepublications system table
ms.assetid: 7f82c6c3-22d1-47c0-a92b-4d64b98cc455
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0b2a7e7783ffb67688041a6da39ea99f945a607f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097335"
---
# <a name="sysmergepublications-transact-sql"></a>sysmergepublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque publication de fusion définie dans la base de données. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Nom du serveur par défaut.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication par défaut.|  
|**name**|**sysname**|Nom de la publication.|  
|**description**|**nvarchar(255)**|Brève description de la publication.|  
|**fixation**|**int**|Période de rétention pour l’ensemble de la publication, où l’unité est indiquée par la valeur de la colonne **retention_period_unit** .|  
|**publication_type**|**tinyint**|Indique que la publication est filtrée :<br /><br /> **0** = non filtré.<br /><br /> **1** = filtré.|  
|**pubid**|**uniqueidentifier**|Numéro d'identification unique de cette publication. Ce numéro est généré lors de l'ajout de la publication.|  
|**designmasterid**|**uniqueidentifier**|Réservé à un usage ultérieur.|  
|**parentid**|**uniqueidentifier**|Indique la publication parente à partir de laquelle la publication paire courante ou la publication de sous-ensemble a été créée (utilisé pour les topologies de publication hiérarchiques).|  
|**sync_mode**|**tinyint**|Mode de synchronisation de la publication :<br /><br /> **0** = natif.<br /><br /> **1** = caractère.|  
|**allow_push**|**int**|Indique si la publication autorise les abonnements par envoi de données (push).<br /><br /> **0** = les abonnements envoyés ne sont pas autorisés.<br /><br /> **1** = les abonnements envoyés sont autorisés.|  
|**allow_pull**|**int**|Indique si la publication autorise les abonnements par extraction de données (pull).<br /><br /> **0** = les abonnements extraits ne sont pas autorisés.<br /><br /> **1** = les abonnements extraits sont autorisés.|  
|**allow_anonymous**|**int**|Indique si la publication autorise les abonnements anonymes.<br /><br /> **0** = les abonnements anonymes ne sont pas autorisés.<br /><br /> **1** = les abonnements anonymes sont autorisés.|  
|**centralized_conflicts**|**int**|Indique si les enregistrements conflictuels sont stockés côté serveur de publication :<br /><br /> **0** = les enregistrements en conflit ne sont pas stockés sur le serveur de publication.<br /><br /> **1** = les enregistrements en conflit sont stockés sur le serveur de publication.|  
|**statut**|**tinyint**|Réservé à un usage ultérieur.|  
|**snapshot_ready**|**tinyint**|Indique l'état de l'instantané de la publication :<br /><br /> **0** = l’instantané n’est pas prêt à être utilisé.<br /><br /> **1** = la capture instantanée est prête à être utilisée.<br /><br /> **2** = un nouvel instantané de cette publication doit être créé.|  
|**enabled_for_internet**|**bit**|Indique si les fichiers de synchronisation pour la publication sont accessibles sur Internet, par l'intermédiaire de FTP et d'autres services.<br /><br /> **0** = les fichiers de synchronisation sont accessibles à partir d’Internet.<br /><br /> **1** = les fichiers de synchronisation ne sont pas accessibles à partir d’Internet.|  
|**dynamic_filters**|**bit**|Indique si la publication est filtrée à l'aide d'un filtre de lignes paramétrable.<br /><br /> **0** = la publication n’est pas filtrée par ligne.<br /><br /> **1** = la publication est filtrée par ligne.|  
|**snapshot_in_defaultfolder**|**bit**|Indique si les fichiers d'instantané sont stockés dans le dossier par défaut :<br /><br /> **0** = les fichiers d’instantanés se trouvent dans le dossier par défaut.<br /><br /> **1** = les fichiers d’instantanés sont stockés à l’emplacement spécifié par **alt_snapshot_folder**.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Emplacement de l'autre dossier pour l'instantané.|  
|**pre_snapshot_script**|**nvarchar(255)**|Pointeur vers un. fichier **SQL** que le agent de fusion exécute avant l’un des scripts d’objet de réplication lors de l’application de l’instantané sur l’abonné.|  
|**post_snapshot_script**|**nvarchar(255)**|Pointeur vers un. fichier **SQL** que le agent de fusion exécute après que tous les autres scripts et données d’objet de réplication ont été appliqués lors d’une synchronisation initiale.|  
|**compress_snapshot**|**bit**|Spécifie si l’instantané écrit dans le **alt_snapshot_folder** emplacement est compressé au [!INCLUDE[msCoName](../../includes/msconame-md.md)] format cab. **0** indique que le fichier n’est pas compressé.|  
|**ftp_address**|**sysname**|Adresse réseau du service protocole FTP (FTP) pour le serveur de distribution. Indique l'emplacement à partir duquel l'Agent de fusion peut extraire les fichiers d'instantané de la publication, si le protocole FTP est activé.|  
|**ftp_port**|**int**|Numéro de port du service FTP du serveur de distribution.|  
|**ftp_subdirectory**|**nvarchar(255)**|Sous-répertoire à partir duquel l'Agent de fusion peut extraire les fichiers d'instantané.|  
|**ftp_login**|**sysname**|Nom de l'utilisateur, utilisé pour la connexion au service FTP.|  
|**ftp_password**|**nvarchar (524)**|Mot de passe de l’utilisateur utilisé pour se connecter au service FTP.|  
|**conflict_retention**|**int**|Indique la période de rétention, en jours, pendant laquelle les conflits sont conservés. À la fin de cette période, la ligne de conflits est purgée de la table de conflits.|  
|**keep_before_values**|**int**|Indique si l'optimisation de la synchronisation intervient pour cette publication :<br /><br /> **0** = la synchronisation n’est pas optimisée et les partitions envoyées à tous les abonnés sont vérifiées lorsque les données sont modifiées dans une partition.<br /><br /> **1** = la synchronisation est optimisée et seuls les abonnés ayant des lignes dans la partition modifiée sont affectés.|  
|**allow_subscription_copy**|**bit**|Indique si la possibilité de copier la base de données d'abonnement a été activée. **0** signifie que la copie n’est pas autorisée.|  
|**allow_synctoalternate**|**bit**|Spécifie si un partenaire de synchronisation différent est autorisé pour se synchroniser avec le serveur de publication. **0** signifie qu’un partenaire de synchronisation n’est pas autorisé.|  
|**validate_subscriber_info**|**nvarchar (500)**|Donne la liste des fonctions utilisées pour extraire les informations d'Abonné et valider les critères de filtre de lignes paramétrable sur l'Abonné.|  
|**ad_guidname**|**sysname**|Spécifie si la publication est publiée dans l'annuaire [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. Un GUID valide spécifie que la publication est publiée dans le Active Directory et que le GUID correspond à l’objet de publication Active Directory **objectGUID**. Si la valeur est NULL, la publication n'est pas publiée dans l'annuaire Active Directory.|  
|**backward_comp_level**|**int**|Niveau de compatibilité de la base de données. Peut avoir l’une des valeurs suivantes :<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .|  
|**max_concurrent_merge**|**int**|Nombre maximal de processus de fusion simultanés autorisés. La valeur **0** pour cette propriété signifie qu’il n’y a aucune limite au nombre de processus de fusion simultanés en cours d’exécution à un moment donné. Cette propriété définit une limite sur le nombre de processus de fusion simultanés qui peuvent être exécutés sur une publication de fusion à un moment donné. Si, au même moment, le nombre de processus d'instantané planifiés dépasse le nombre maximal autorisé, les travaux en excès sont placés dans une file d'attente jusqu'à achèvement d'un processus de fusion en cours.|  
|**max_concurrent_dynamic_snapshots**|**int**|Nombre maximal de sessions d'instantanés de données filtrées simultanées autorisées exécutables sur la publication de fusion. Si la **valeur est 0**, le nombre maximal de sessions d’instantanés de données filtrées simultanées pouvant être exécutées simultanément sur la publication n’est pas limité à un moment donné. Cette propriété permet de définir un nombre maximal de processus d'instantané simultanés exécutables sur une publication de fusion à un moment donné. Si, au même moment, le nombre de processus d'instantané planifiés dépasse le nombre maximal autorisé, les travaux en excès sont placés dans une file d'attente jusqu'à achèvement d'un processus de fusion en cours.|  
|**use_partition_groups**|**smallint**|Spécifie si la publication utilise des partitions précalculées.|  
|**dynamic_filters_function_list**|**nvarchar (500)**|Liste délimitée par des points-virgules des fonctions utilisées dans les filtres de lignes paramétrables de la publication.|  
|**partition_id_eval_proc**|**sysname**|Spécifie le nom de la procédure qu'exécute l'Agent de fusion d'un Abonné pour déterminer l'ID de partition affecté à celui-ci.|  
|**publication_number**|**smallint**|Spécifie la colonne d’identité qui fournit un mappage sur 2 octets à **pubid**. **pubid** est un identificateur global unique pour une publication, tandis que le numéro de publication est unique dans une base de données spécifiée.|  
|**replicate_ddl**|**int**|Indique si la réplication de schéma est prise en charge pour la publication.<br /><br /> **0** = les instructions DDL ne sont pas répliquées.<br /><br /> **1** = les instructions DDL exécutées sur le serveur de publication sont répliquées.<br /><br /> Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).|  
|**allow_subscriber_initiated_snapshot**|**bit**|Indique que les Abonnés peuvent initier le processus qui génère l'instantané d'une publication à l'aide de filtres paramétrés. **1** indique que les abonnés peuvent initier le processus d’instantané.|  
|**dynamic_snapshot_queue_timeout**|**int**|Spécifie le nombre de minutes pendant lesquelles un Abonné doit patienter dans la file d'attente avant que ne démarre le processus de génération d'instantané lors de l'utilisation de filtres paramétrés.|  
|**dynamic_snapshot_ready_timeout**|**int**|Spécifie le nombre de minutes pendant lesquelles un Abonné attend que se déroule le processus de génération d'instantané lors de l'utilisation de filtres paramétrés.|  
|**conseiller**|**sysname**|Nom du serveur de distribution de la publication.|  
|**snapshot_jobid**|**Binary(16**|Identifie le travail d'Agent qui génère l'instantané lorsque l'Abonné peut initier le processus de génération d'instantané.|  
|**allow_web_synchronization**|**bit**|Spécifie si la publication est activée pour la synchronisation Web, où **1** signifie que la synchronisation Web est activée pour la publication.|  
|**web_synchronization_url**|**nvarchar (500)**|Spécifie la valeur par défaut de l'URL Internet utilisée pour la synchronisation Web.|  
|**allow_partition_realignment**|**bit**|Indique si les suppressions sont envoyées à l'Abonné lorsque la modification de la ligne sur le serveur de publication amène celui-ci à modifier sa partition.<br /><br /> **0** = les données d’une ancienne partition sont conservées sur l’abonné, où les modifications apportées à ces données sur le serveur de publication ne sont pas répliquées sur cet abonné, mais les modifications apportées sur l’abonné sont répliquées sur le serveur de publication.<br /><br /> **1** = suppressions sur l’abonné pour refléter les résultats d’une modification de partition en supprimant des données qui ne font plus partie de la partition de l’abonné.<br /><br /> Pour plus d’informations, consultez [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md).<br /><br /> Remarque : les données qui restent sur l’abonné lorsque cette valeur est **égale à 0** doivent être traitées comme si elles étaient en lecture seule ; Toutefois, cela n’est pas strictement forcé par le système de réplication.|  
|**retention_period_unit**|**tinyint**|Définit l’unité utilisée lors de la définition de la *rétention*, qui peut prendre l’une des valeurs suivantes :<br /><br /> **0** = jour.<br /><br /> **1** = semaine.<br /><br /> **2** = mois.<br /><br /> **3** = année.|  
|**decentralized_conflicts**|**int**|Indique si les enregistrements en conflit sont stockés dans l'Abonné à l'origine du conflit :<br /><br /> **0** = les enregistrements en conflit ne sont pas stockés sur l’abonné.<br /><br /> **1** = les enregistrements en conflit sont stockés sur l’abonné.|  
|**generation_leveling_threshold**|**int**|Indique le nombre de modifications contenues dans une génération. Une génération est une collection de modifications remises à un serveur de publication ou à un Abonné.|  
|**automatic_reinitialization_policy**|**bit**|Indique si les modifications sont téléchargées depuis l'Abonné avant une réinitialisation automatique.<br /><br /> **1** = les modifications sont téléchargées à partir de l’abonné avant une réinitialisation automatique.<br /><br /> **0** = les modifications ne sont pas téléchargées avant une réinitialisation automatique.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)  
  
  
