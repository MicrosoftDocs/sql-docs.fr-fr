---
description: sp_helpmergepublication (Transact-SQL)
title: sp_helpmergepublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepublication
- sp_helpmergepublication_TSQL
helpviewer_keywords:
- sp_helpmergepublication
ms.assetid: dfe1e1e1-9a65-406a-aced-6385a078e135
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8001e98b207ca9493ff71b1f874b12b0d5227306
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160611"
---
# <a name="sp_helpmergepublication-transact-sql"></a>sp_helpmergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie des informations sur une publication de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpmergepublication [ [ @publication = ] 'publication' ]  
    [ , [ @found = ] 'found' OUTPUT ]  
    [ , [ @publication_id = ] 'publication_id' OUTPUT ]  
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @publication **=** ] **«**_publication_**»**  
 Nom de la publication. *publication* est de **type sysname**, avec la valeur par défaut **%** , qui retourne des informations sur toutes les publications de fusion dans la base de données active.  
  
 sortie de [ @found **=** ] _trouvée_**  
 Indicateur qui signale le retour de lignes. *valeur* de **type int** et paramètre de sortie, avec NULL comme valeur par défaut. **1** indique que la publication est trouvée. **0** indique que la publication est introuvable.  
  
 sortie de [ @publication_id **=** ] **'**_publication_id_*_'_*  
 Numéro d'identification de la publication. *publication_id* est de type **uniqueidentifier** et un paramètre OUTPUT, avec NULL comme valeur par défaut.  
  
 [ @reserved **=** ] **'**_reserved_*_'_*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]*reserved* est de type **nvarchar (20)**, avec NULL comme valeur par défaut.  
  
 [ @publisher **=** ] **«** serveur de _publication_*_»_*  
 Nom du serveur de publication. *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
 [ @publisher_db **=** ] **'**_publisher_db_*_'_*  
 Nom de la base de données de publication. *publisher_db* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|id|**int**|Numéro séquentiel de la publication dans la liste de l'ensemble de résultats.|  
|name|**sysname**|Nom de la publication.|  
|description|**nvarchar(255)**|Description de la publication.|  
|status|**tinyint**|Indique quand les données de publication sont disponibles.|  
|retention|**int**|Temps nécessaire pour enregistrer les métadonnées relatives aux modifications des articles dans la publication. Les unités utilisées pour cette période peuvent être des jours, des semaines, des mois ou des années. Pour plus d'informations sur ces unités, consultez la colonne retention_period_unit.|  
|sync_mode|**tinyint**|Mode de synchronisation de cette publication :<br /><br /> **0** = programme de copie en bloc natif (utilitaire **BCP** )<br /><br /> **1** = copie en bloc de caractères|  
|allow_push|**int**|Détermine si des abonnements par envoi de données (push) peuvent être créés pour la publication concernée. **0** signifie qu’un abonnement par envoi de notification n’est pas autorisé.|  
|allow_pull|**int**|Détermine si des abonnements par extraction de données (pull) peuvent être créés pour la publication concernée. **0** signifie qu’un abonnement par extraction n’est pas autorisé.|  
|allow_anonymous|**int**|Détermine si des abonnements anonymes peuvent être créés pour la publication concernée. **0** signifie qu’un abonnement anonyme n’est pas autorisé.|  
|centralized_conflicts|**int**|Détermine si les enregistrements en conflit sont stockés sur le serveur de publication donné :<br /><br /> **0** = les enregistrements en conflit sont stockés sur le serveur de publication et sur l’abonné à l’origine du conflit.<br /><br /> **1** = tous les enregistrements en conflit sont stockés sur le serveur de publication.|  
|priority|**float (8)**|Priorité de l'abonnement en boucle.|  
|snapshot_ready|**tinyint**|Indique si l'instantané de cette publication est prêt :<br /><br /> **0** = la capture instantanée est prête à être utilisée.<br /><br /> **1** = l’instantané n’est pas prêt à être utilisé.|  
|publication_type|**int**|Type de publication :<br /><br /> **0** = instantané.<br /><br /> **1** = transactionnelle.<br /><br /> **2** = fusion.|  
|pubid|**uniqueidentifier**|Identificateur unique de la publication.|  
|snapshot_jobid|**Binary(16**|ID de travail de l'Agent d'instantané. Pour obtenir l’entrée pour le travail d’instantané dans la table système [sysjobs](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md) , vous devez convertir cette valeur hexadécimale en **uniqueidentifier**.|  
|enabled_for_internet|**int**|Détermine si la publication est activée pour Internet. Si la condition est **1**, les fichiers de synchronisation de la publication sont placés dans le `C:\Program Files\Microsoft SQL Server\MSSQL\Repldata\Ftp` répertoire. L'utilisateur doit créer le répertoire FTP (File Transfer Protocol). Si la **valeur est 0**, la publication n’est pas activée pour l’accès Internet.|  
|dynamic_filter|**int**|Indique si un filtre de lignes paramétrable est utilisé. **0** signifie qu’un filtre de lignes paramétrable n’est pas utilisé.|  
|has_subscription|**bit**|Indique si la publication comporte des abonnements. **0** signifie qu’il n’y a actuellement aucun abonnement à cette publication.|  
|snapshot_in_default_folder|**bit**|Spécifie si les fichiers d’instantanés sont stockés dans le dossier par défaut.<br /><br /> Si la valeur est **1**, les fichiers d’instantanés se trouvent dans le dossier par défaut.<br /><br /> Si la **valeur est 0**, les fichiers d’instantané sont stockés à l’emplacement secondaire spécifié par **alt_snapshot_folder**. Les emplacements secondaires peuvent se trouver sur un autre serveur, un lecteur réseau ou un support amovible (tel qu'un CD-ROM ou des disques amovibles). Vous pouvez également enregistrer les fichiers d'instantané sur un site FTP, pour permettre à l'Abonné de les extraire plus tard.<br /><br /> Remarque : ce paramètre peut avoir la valeur true et disposer toujours d’un emplacement dans le paramètre **alt_snapshot_folder** . Cette combinaison spécifie que les fichiers d'instantané sont stockés à la fois dans l'emplacement par défaut et dans l'emplacement secondaire.|  
|alt_snapshot_folder|**nvarchar(255)**|Indique l'emplacement du dossier de remplacement pour l'instantané.|  
|pre_snapshot_script|**nvarchar(255)**|Spécifie un pointeur vers un fichier **. SQL** que le agent de fusion exécute avant tout script d’objet répliqué lors de l’application de l’instantané sur un abonné.|  
|post_snapshot_script|**nvarchar(255)**|Spécifie un pointeur vers un fichier **. SQL** que le agent de fusion exécute une fois que tous les autres scripts et données d’objet répliqués ont été appliqués lors d’une synchronisation initiale.|  
|compress_snapshot|**bit**|Spécifie que l’instantané écrit dans le **alt_snapshot_folder** emplacement est compressé au [!INCLUDE[msCoName](../../includes/msconame-md.md)] format cab.|  
|ftp_address|**sysname**|Adresse réseau du service FTP du serveur de distribution. Spécifie l'emplacement à partir duquel l'Agent fusion peut extraire les fichiers d'instantané de la publication.|  
|ftp_port|**int**|Numéro de port du service FTP du serveur de distribution. **ftp_port** a la valeur par défaut **21**. Spécifie l'emplacement où l'Agent de fusion peut accéder aux fichiers d'instantané de la publication.|  
|ftp_subdirectory|**nvarchar(255)**|Spécifie l'emplacement où l'Agent de fusion peut accéder aux fichiers d'instantanés lorsque l'instantané est envoyé via FTP.|  
|ftp_login|**sysname**|Nom d’utilisateur utilisé pour la connexion au service FTP.|  
|conflict_retention|**int**|Indique la période de rétention, en jours, pendant laquelle les conflits sont conservés. Au terme du nombre de jours spécifié, la ligne en conflit est purgée de la table des conflits.|  
|keep_partition_changes|**int**|Indique si l'optimisation de la synchronisation intervient pour cette publication. **keep_partition_changes** a la valeur par défaut **0**. La valeur **0** signifie que la synchronisation n’est pas optimisée et que les partitions envoyées à tous les abonnés sont vérifiées lorsque les données sont modifiées dans une partition.<br /><br /> **1** signifie que la synchronisation est optimisée et que seuls les abonnés ayant des lignes dans la partition modifiée sont affectés.<br /><br /> Remarque : par défaut, les publications de fusion utilisent des partitions précalculées, ce qui fournit un niveau d’optimisation plus élevé que cette option. Pour plus d’informations, consultez [filtres de lignes paramétrables](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md) et [optimiser les performances des filtres paramétrés avec des partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).|  
|allow_subscription_copy|**int**|Spécifie si la possibilité de copier les bases de données d'abonnement qui s'abonnent à cette publication a été activée. La valeur **0** signifie que la copie n’est pas autorisée.|  
|allow_synctoalternate|**int**|Spécifie si un partenaire de synchronisation différent est autorisé pour se synchroniser avec le serveur de publication. La valeur **0** signifie qu’un partenaire de synchronisation n’est pas autorisé.|  
|validate_subscriber_info|**nvarchar (500)**|Donne la liste des fonctions utilisées pour extraire les informations d'Abonné et valider les critères de filtre de lignes paramétrable sur l'Abonné. Permet de vérifier la cohérence du partitionnement des informations avec chaque fusion.|  
|backward_comp_level|**int**|Niveau de compatibilité de la base de données. Il peut avoir une des valeurs suivantes :<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP1<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP2<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|publish_to_activedirectory|**bit**|Spécifie si les informations de publication sont publiées dans Active Directory. La valeur **0** signifie que les informations de publication ne sont pas disponibles à partir de Active Directory.<br /><br /> Ce paramètre est déconseillé et il n'est pris en charge que pour la compatibilité descendante des scripts. Vous ne pouvez plus ajouter d'informations de publication dans Active Directory.|  
|max_concurrent_merge|**int**|Nombre de processus de fusion simultanés. Si la **valeur est 0**, il n’y a aucune limite au nombre de processus de fusion simultanés en cours d’exécution à un moment donné.|  
|max_concurrent_dynamic_snapshots|**int**|Nombre maximal de sessions d'instantané filtrée pouvant être exécutées simultanément par rapport à la publication de fusion. Si la **valeur est 0**, le nombre maximal de sessions d’instantanés de données filtrées simultanées pouvant être exécutées simultanément sur la publication n’est pas limité à un moment donné.|  
|use_partition_groups|**int**|Détermine si des partitions précalculées sont utilisées. La valeur **1** signifie que les partitions précalculées sont utilisées.|  
|num_of_articles|**int**|Nombre d'articles dans la publication.|  
|replicate_ddl|**int**|Indique si les modifications de schéma des tables publiées sont répliquées. La valeur **1** signifie que les modifications de schéma sont répliquées.|  
|publication_number|**smallint**|Numéro affecté à cette publication.|  
|allow_subscriber_initiated_snapshot|**bit**|Détermine si les Abonnés peuvent lancer le processus de génération d'instantané de données filtrées. La valeur **1** signifie que les abonnés peuvent initier le processus d’instantané.|  
|allow_web_synchronization|**bit**|Détermine si la publication est activée pour la synchronisation Web. La valeur **1** signifie que la synchronisation Web est activée.|  
|web_synchronization_url|**nvarchar (500)**|URL Internet utilisé pour la synchronisation Web.|  
|allow_partition_realignment|**bit**|Détermine si les suppressions sont envoyées à l'abonné lorsque la modification de la ligne sur le serveur de publication entraîne la modification de sa partition. La valeur **1** signifie que les suppressions sont envoyées à l’abonné.  Pour plus d’informations, consultez [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md).|  
|retention_period_unit|**tinyint**|Définit l'unité utilisée lors la définition de la rétention. Il peut s’agir de l’une des valeurs suivantes :<br /><br /> **0** = jour<br /><br /> **1** = semaine<br /><br /> **2** = mois<br /><br /> **3** = année|  
|has_downloadonly_articles|**bit**|Indique si des articles qui appartiennent à la publication sont des articles téléchargeables uniquement. La valeur **1** indique qu’il existe des articles en téléchargement seul.|  
|decentralized_conflicts|**int**|Indique si les enregistrements en conflit sont stockés sur l'Abonné qui a généré le conflit. La valeur **0** indique que les enregistrements en conflit ne sont pas stockés sur l’abonné. La valeur 1 indique que les enregistrements en conflit sont stockés sur l'Abonné.|  
|generation_leveling_threshold|**int**|Spécifie le nombre de modifications contenues dans une génération. Une génération est une collection de modifications remises à un serveur de publication ou à un abonné|  
|automatic_reinitialization_policy|**bit**|Indique si les modifications sont téléchargées depuis l'Abonné avant une réinitialisation automatique. La valeur **1** indique que les modifications sont téléchargées à partir de l’abonné avant qu’une réinitialisation automatique se produise. La valeur 0 indique que des modifications ne sont pas téléchargées avant une réinitialisation automatique.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_helpmergepublication est utilisé dans la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Les membres de la liste d'accès à la publication d'une publication peuvent exécuter sp_helpmergepublication pour cette publication. Les membres du rôle de base de données fixe db_owner de la base de données de publication peuvent exécuter sp_helpmergepublication pour obtenir des informations sur toutes les publications.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_helpmergepublication](../../relational-databases/replication/codesnippet/tsql/sp-helpmergepublication-_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
