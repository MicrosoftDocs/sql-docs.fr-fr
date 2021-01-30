---
description: sp_addmergepublication (Transact-SQL)
title: sp_addmergepublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepublication
- sp_addmergepublication_TSQL
helpviewer_keywords:
- sp_addmergepublication
ms.assetid: 28a629a1-7374-4614-9b04-279d290a942a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f0c132ffc9315bafc28530afeef83f779c2975e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209438"
---
# <a name="sp_addmergepublication-transact-sql"></a>sp_addmergepublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée une nouvelle publication de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données publiée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addmergepublication [ @publication = ] 'publication'   
    [ , [ @description = ] 'description'   
    [ , [ @retention = ] retention ]   
    [ , [ @sync_mode = ] 'sync_mode' ]   
    [ , [ @allow_push = ] 'allow_push' ]   
    [ , [ @allow_pull = ] 'allow_pull' ]   
    [ , [ @allow_anonymous = ] 'allow_anonymous' ]   
    [ , [ @enabled_for_internet = ] 'enabled_for_internet' ]   
    [ , [ @centralized_conflicts = ] 'centralized_conflicts' ]   
    [ , [ @dynamic_filters = ] 'dynamic_filters' ]   
    [ , [ @snapshot_in_defaultfolder = ] 'snapshot_in_default_folder' ]   
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]   
    [ , [ @pre_snapshot_script = ] 'pre_snapshot_script' ]   
    [ , [ @post_snapshot_script = ] 'post_snapshot_script' ]   
    [ , [ @compress_snapshot = ] 'compress_snapshot' ]   
    [ , [ @ftp_address = ] 'ftp_address' ]   
    [ , [ @ftp_port = ] ftp_port ]   
    [ , [ @ftp_subdirectory = ] 'ftp_subdirectory' ]   
    [ , [ @ftp_login = ] 'ftp_login' ]   
    [ , [ @ftp_password = ] 'ftp_password' ]   
    [ , [ @conflict_retention = ] conflict_retention ]   
    [ , [ @keep_partition_changes = ] 'keep_partition_changes' ]   
    [ , [ @allow_subscription_copy = ] 'allow_subscription_copy' ]   
    [ , [ @allow_synctoalternate = ] 'allow_synctoalternate' ]   
    [ , [ @validate_subscriber_info = ] 'validate_subscriber_info' ]   
    [ , [ @add_to_active_directory = ] 'add_to_active_directory' ]   
    [ , [ @max_concurrent_merge = ] maximum_concurrent_merge ]   
    [ , [ @max_concurrent_dynamic_snapshots = ] max_concurrent_dynamic_snapshots ]  
    [ , [ @use_partition_groups = ] 'use_partition_groups' ]  
    [ , [ @publication_compatibility_level = ] 'backward_comp_level' ]  
    [ , [ @replicate_ddl = ] replicate_ddl ]  
    [ , [ @allow_subscriber_initiated_snapshot = ] 'allow_subscriber_initiated_snapshot' ]   
    [ , [ @allow_web_synchronization = ] 'allow_web_synchronization' ]   
    [ , [ @web_synchronization_url = ] 'web_synchronization_url' ]  
    [ , [ @allow_partition_realignment = ] 'allow_partition_realignment' ]  
    [ , [ @retention_period_unit = ] 'retention_period_unit' ]  
    [ , [ @generation_leveling_threshold = ] generation_leveling_threshold ]  
    [ , [ @automatic_reinitialization_policy = ] automatic_reinitialization_policy ]  
    [ , [ @conflict_logging = ] 'conflict_logging' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication de fusion à créer. *publication* est de **type sysname**, sans valeur par défaut et ne doit pas être le mot clé ALL. Le nom de la publication doit être unique dans la base de données.  
  
`[ @description = ] 'description'` Description de la publication. *Description* est de type **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
`[ @retention = ] retention` Période de rétention, en unités de période de rétention, pour laquelle enregistrer les modifications pour la *publication* donnée. la *rétention* est de **type int**, avec 14 unités comme valeur par défaut. Les unités de période de rétention sont définies par *retention_period_unit*. L'abonnement expire et doit être réinitialisé s'il n'est pas synchronisé pendant la période de rétention et que les modifications en attente qu'il aurait dû recevoir ont été supprimées par une opération de nettoyage sur le serveur de distribution. La période de rétention maximum autorisable est le nombre de jours compris entre le 31 décembre 9999 et la date actuelle.  
  
> [!NOTE]  
>  La période de rétention des publications de fusion dispose d'un délai de grâce de 24 heures pour prendre en charge les Abonnés situés dans différents fuseaux horaires. Si, par exemple, vous définissez une période de rétention d'un jour, la période de rétention réelle est de 48 heures.  
  
`[ @sync_mode = ] 'sync_mode'` Mode de la synchronisation initiale des abonnés à la publication. *sync_mode* est de type **nvarchar (10)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**natif** (par défaut)|Produit une copie par bloc en mode natif de toutes les tables.|  
|**character**|Produit une copie par bloc en mode caractère de toutes les tables. Requis pour prendre en charge [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssEW](../../includes/ssew-md.md)] les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés et non.|  
  
`[ @allow_push = ] 'allow_push'` Spécifie si des abonnements par émission de données peuvent être créés pour la publication donnée. *allow_push* est de type **nvarchar (5)**, avec true comme valeur par défaut, ce qui permet l’envoi d’abonnements à la publication.  
  
`[ @allow_pull = ] 'allow_pull'` Spécifie si des abonnements par extraction de données peuvent être créés pour la publication donnée. *allow_pull* est de type **nvarchar (5)**, avec true comme valeur par défaut, qui autorise les abonnements par extraction de la publication. Vous devez spécifier la valeur true pour prendre en charge les [!INCLUDE[ssEW](../../includes/ssew-md.md)] abonnés.  
  
`[ @allow_anonymous = ] 'allow_anonymous'` Spécifie si des abonnements anonymes peuvent être créés pour la publication donnée. *allow_anonymous* est de type **nvarchar (5)**, avec true comme valeur par défaut, ce qui autorise les abonnements anonymes à la publication. Pour prendre en charge les [!INCLUDE[ssEW](../../includes/ssew-md.md)] abonnés, vous devez spécifier **true**.  
  
`[ @enabled_for_internet = ] 'enabled_for_internet'` Spécifie si la publication est activée pour Internet et détermine si le protocole FTP (File Transfer Protocol) peut être utilisé pour transférer les fichiers d’instantanés à un abonné. *enabled_for_internet* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est true**, les fichiers de synchronisation de la publication sont placés dans le répertoire C:\Program Files\Microsoft SQL Server\MSSQL\MSSQL.x\Repldata\Ftp. L'utilisateur doit créer le répertoire FTP. Si la **valeur est false**, la publication n’est pas activée pour l’accès Internet.  
  
`[ @centralized_conflicts = ] 'centralized_conflicts'` Ce paramètre est déconseillé et n’est pris en charge que pour la compatibilité descendante des scripts. Utilisez *conflict_logging* pour spécifier l’emplacement de stockage des enregistrements en conflit.  
  
`[ @dynamic_filters = ] 'dynamic_filters'` Permet à la publication de fusion d’utiliser des filtres de lignes paramétrables. *dynamic_filters* est de type **nvarchar (5)**, avec false comme valeur par défaut.  
  
> [!NOTE]  
>  Vous ne devez pas spécifier vous-même ce paramètre mais autoriser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à déterminer automatiquement si les filtres de lignes paramétrables sont utilisés. Si vous spécifiez la valeur **true** pour *dynamic_filters*, vous devez définir un filtre de lignes paramétrable pour l’article. Pour plus d'informations, voir [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
`[ @snapshot_in_defaultfolder = ] 'snapshot_in_default_folder'` Spécifie si les fichiers d’instantanés sont stockés dans le dossier par défaut. *snapshot_in_default_folder* est de type **nvarchar (5)**, avec true comme valeur par défaut. Si la **valeur est true**, les fichiers d’instantanés se trouvent dans le dossier par défaut. Si la **valeur est false**, les fichiers d’instantané sont stockés à l’emplacement secondaire spécifié par *alternate_snapshot_folder*. Les emplacements secondaires peuvent se trouver sur un autre serveur, un lecteur réseau ou un support amovible (tel qu'un CD-ROM ou des disques amovibles). Vous pouvez également enregistrer les fichiers d'instantané sur un site FTP, pour qu'ils soient récupérés ultérieurement par l'abonné. Notez que ce paramètre peut avoir la valeur true et qu’il a toujours un emplacement spécifié par *alt_snapshot_folder*. Cette combinaison indique que les fichiers d'instantané sont stockés dans les emplacements par défaut et secondaires.  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'` Spécifie l’emplacement du dossier de remplacement pour l’instantané. *alternate_snapshot_folder* est de type **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
`[ @pre_snapshot_script = ] 'pre_snapshot_script'` Spécifie un pointeur vers un emplacement de fichier **. SQL** . *pre_snapshot_script* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. L'Agent de fusion exécute le script de pré-instantané avant tous les scripts d'objets répliqués, lors de l'application de l'instantané chez un abonné. Le script est exécuté dans le contexte de sécurité utilisé par l'Agent de fusion lors de la connexion à la base de données d'abonnement. Les scripts de pré-instantané ne sont pas exécutés sur les [!INCLUDE[ssEW](../../includes/ssew-md.md)] abonnés.  
  
`[ @post_snapshot_script = ] 'post_snapshot_script'` Spécifie un pointeur vers un emplacement de fichier **. SQL** . *post_snapshot_script* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. L'Agent de fusion exécute le script de post-instantané après que tous les autres scripts d'objets et données répliqués ont été appliqués lors d'une synchronisation initiale. Le script est exécuté dans le contexte de sécurité utilisé par l'Agent de fusion lors de la connexion à la base de données d'abonnement. Les scripts postérieurs à l’instantané ne sont pas exécutés sur les [!INCLUDE[ssEW](../../includes/ssew-md.md)] abonnés.  
  
`[ @compress_snapshot = ] 'compress_snapshot'`Spécifie que l’instantané écrit à l’emplacement de **\@ alt_snapshot_folder** doit être compressé au [!INCLUDE[msCoName](../../includes/msconame-md.md)] format cab. *compress_snapshot* est de type **nvarchar (5)**, avec false comme valeur par défaut. **false** spécifie que l’instantané ne sera pas compressé ; **true** spécifie que l’instantané doit être compressé. Les fichiers d'instantané dépassant 2 Go ne peuvent pas être compressés. Les fichiers d'instantané compressés sont décompressés à l'emplacement d'exécution de l'Agent de fusion. Les abonnements par extraction de données sont généralement utilisés avec des instantanés compressés, afin que les fichiers soient décompressés chez l'abonné. L'instantané se trouvant dans le dossier par défaut ne peut pas être compressé. Pour prendre en charge les [!INCLUDE[ssEW](../../includes/ssew-md.md)] abonnés, vous devez spécifier **false**.  
  
`[ @ftp_address = ] 'ftp_address'` Adresse réseau du service FTP du serveur de distribution. *ftp_address* est de **type sysname**, avec NULL comme valeur par défaut. Indique l'emplacement à partir duquel l'Agent de fusion d'un abonné peut extraire les fichiers d'instantané de la publication. Étant donné que cette propriété est stockée pour chaque publication, chaque publication peut avoir une *ftp_address* différente. La publication doit prendre en charge la propagation des instantanés à l'aide du protocole FTP.  
  
`[ @ftp_port = ] ftp_port` Numéro de port du service FTP du serveur de distribution. *ftp_port* est de **type int**, avec 21 comme valeur par défaut. Indique l'emplacement à partir duquel l'Agent de fusion d'un Abonné peut extraire les fichiers d'instantané de la publication. Étant donné que cette propriété est stockée pour chaque publication, chaque publication peut avoir sa propre *ftp_port*.  
  
`[ @ftp_subdirectory = ] 'ftp_subdirectory'` Spécifie l’emplacement où les fichiers d’instantanés seront disponibles pour la Agent de fusion de l’abonné à récupérer si la publication prend en charge la propagation d’instantanés à l’aide de FTP. *ftp_subdirectory* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. Étant donné que cette propriété est stockée pour chaque publication, chaque publication peut avoir sa propre *ftp_subdirctory* ou choisir de n’avoir aucun sous-répertoire, indiqué par une valeur null.  
  
 Lors de la prégénération d'instantanés pour les publications avec des filtres paramétrables, l'instantané de données pour chaque partition d'abonné doit se trouver dans son propre dossier. La structure de répertoire pour les instantanés prégénérés à l'aide de FTP doit répondre aux conditions suivantes :  
  
 *alternate_snapshot_folder*\ftp \\ *publisher_publicationDB_publication* \\ *PartitionID*.  
  
> [!NOTE]  
>  Les valeurs en italique ci-dessus dépendent des détails de la publication et de la partition de l'abonné.  
  
`[ @ftp_login = ] 'ftp_login'` Nom d’utilisateur utilisé pour la connexion au service FTP. *ftp_login* est de **type sysname**, avec « Anonymous » comme valeur par défaut.  
  
`[ @ftp_password = ] 'ftp_password'` Mot de passe de l’utilisateur utilisé pour la connexion au service FTP. *ftp_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort.  
  
`[ @conflict_retention = ] conflict_retention` Spécifie la période de rétention, en jours, pendant laquelle les conflits sont conservés. *conflict_retention* est de **type int**, avec 14 jours par défaut avant que la ligne en conflit soit purgée de la table de conflits.  
  
`[ @keep_partition_changes = ] 'keep_partition_changes'` Spécifie s’il faut activer les optimisations de modification de partition lorsque les partitions précalculées ne peuvent pas être utilisées. *keep_partition_changes* est de type **nvarchar (5)**, avec true comme valeur par défaut. **false** signifie que les modifications de partition ne sont pas optimisées et que les partitions précalculées ne sont pas utilisées, les partitions envoyées à tous les abonnés sont vérifiées lorsque les données sont modifiées dans une partition. **true** signifie que les modifications de partition sont optimisées et que seuls les abonnés ayant des lignes dans les partitions modifiées sont affectés. Lorsque vous utilisez des partitions précalculées, affectez la valeur **true** à *use_partition_groups* et affectez à *keep_partition_changes* la valeur **false**. Pour plus d’informations, consultez [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
> [!NOTE]  
>  Si vous spécifiez la valeur **true** pour *keep_partition_changes*, spécifiez la valeur **1** pour le paramètre agent d’instantané **-MaxNetworkOptimization**. Pour plus d’informations sur ce paramètre, consultez [réplication agent d’instantané](../../relational-databases/replication/agents/replication-snapshot-agent.md). Pour plus d’informations sur la spécification des paramètres de l’agent, voir [administration de l’agent de réplication](../../relational-databases/replication/agents/replication-agent-administration.md).  
  
 Avec les [!INCLUDE[ssEW](../../includes/ssew-md.md)] abonnés, *keep_partition_changes* doit avoir la valeur true pour garantir que les suppressions sont correctement propagées. Lorsque la valeur est false, l'abonné peut avoir plus de lignes que prévu.  
  
`[ @allow_subscription_copy = ] 'allow_subscription_copy'` Active ou désactive la possibilité de copier les bases de données d’abonnement qui s’abonnent à cette publication. *allow_subscription_copy* est de type **nvarchar (5)**, avec false comme valeur par défaut. La taille de la base de données d'abonnement copiée doit être inférieure à 2 gigaoctets (Go).  
  
`[ @allow_synctoalternate = ] 'allow_synctoalternate'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @validate_subscriber_info = ] 'validate_subscriber_info'` Répertorie les fonctions utilisées pour définir une partition d’abonné des données publiées lorsque les filtres de lignes paramétrables sont utilisés. *validate_subscriber_info* est de type **nvarchar (500)**, avec NULL comme valeur par défaut. Ces informations sont utilisées par l'Agent de fusion pour valider la partition de l'abonné. Par exemple, si [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) est utilisée dans le filtre de lignes paramétrable, le paramètre doit être `@validate_subscriber_info=N'SUSER_SNAME()'` .  
  
> [!NOTE]  
>  Vous ne devez pas spécifier vous-même ce paramètre mais autoriser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à déterminer automatiquement le critère de filtrage.  
  
`[ @add_to_active_directory = ] 'add_to_active_directory'` Ce paramètre est déconseillé et n’est pris en charge que pour la compatibilité descendante des scripts. Vous ne pouvez plus ajouter d'informations de publication à [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory.  
  
`[ @max_concurrent_merge = ] maximum_concurrent_merge` Nombre maximal de processus de fusion simultanés. *maximum_concurrent_merge* est de **type int** avec 0 comme valeur par défaut. La valeur **0** pour cette propriété signifie qu’il n’y a aucune limite au nombre de processus de fusion simultanés en cours d’exécution à un moment donné. Cette propriété permet de définir un nombre maximal de processus de fusion simultanés exécutables sur une publication de fusion à un moment donné. Si le nombre de processus de fusion planifiés en même temps dépasse le nombre maximal autorisé à l'exécution, les travaux en trop sont placés dans une file d'attente jusqu'à l'achèvement d'un processus de fusion en cours.  
  
`[ @max_concurrent_dynamic_snapshots = ] max_concurrent_dynamic_snapshots` Nombre maximal de sessions Agent d’instantané qui peuvent être exécutées simultanément pour générer des instantanés de données filtrées pour les partitions d’abonnés. *maximum_concurrent_dynamic_snapshots* est de **type int** avec 0 comme valeur par défaut. Si la **valeur est 0**, le nombre de sessions d’instantané n’est pas limité. Si, au même moment, le nombre de processus d'instantané planifiés dépasse le nombre maximal autorisé, les travaux en excès sont placés dans une file d'attente jusqu'à achèvement d'un processus d'instantané en cours.  
  
`[ @use_partition_groups = ] 'use_partition_groups'` Spécifie que les partitions précalculées doivent être utilisées pour optimiser le processus de synchronisation. *use_partition_groups* est de type **nvarchar (5)** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**true**|La publication utilise des partitions précalculées.|  
|**false**|La publication n'utilise pas de partitions précalculées.|  
|NULL (valeur par défaut)|Le système détermine la stratégie de partitionnement.|  
  
 Les partitions précalculées sont utilisées par défaut. Pour éviter d’utiliser des partitions précalculées, *use_partition_groups* doit avoir la valeur **false**. Lorsqu'il a la valeur NULL, le système décide si les partitions précalculées peuvent être utilisées. Si les partitions précalculées ne peuvent pas être utilisées, cette valeur devient alors **false** sans générer d’erreurs. Dans ce cas, *keep_partition_changes* peut avoir la valeur **true** pour offrir une certaine optimisation. Pour plus d’informations, consultez [filtres de lignes paramétrables](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md) et [optimiser les performances des filtres paramétrés avec des partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
`[ @publication_compatibility_level = ] backward_comp_level` Indique la compatibilité descendante de la publication. *backward_comp_level* est de type **nvarchar (6)** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Version|  
|-----------|-------------|  
|**90RTM**|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|  
|**100RTM**|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
  
`[ @replicate_ddl = ] replicate_ddl` Indique si la réplication de schéma est prise en charge pour la publication. *replicate_ddl* est de **type int**, avec 1 comme valeur par défaut. **1** indique que les instructions DDL (Data Definition Language) exécutées sur le serveur de publication sont répliquées, et **0** indique que les instructions DDL ne sont pas répliquées. Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
 Le paramètre *\@ replicate_ddl* est respecté lorsqu’une instruction DDL ajoute une colonne. Le paramètre *\@ replicate_ddl* est ignoré lorsqu’une instruction DDL modifie ou supprime une colonne pour les raisons suivantes.  
  
-   Lorsqu'une colonne est supprimée, sysarticlecolumns doit être mis à jour pour empêcher de nouvelles instructions DML d'inclure la colonne supprimée, ce qui provoquerait l'échec de l'agent de distribution. Le paramètre *\@ replicate_ddl* est ignoré, car la réplication doit toujours répliquer la modification de schéma.  
  
-   Lorsqu'une colonne est modifiée, le type de données source ou la possibilité d'une valeur NULL peuvent avoir changé et les instructions DML peuvent contenir une valeur non compatible avec la table sur l'abonné. Ces instructions DML peuvent entraîner l'échec de l'agent de distribution. Le paramètre *\@ replicate_ddl* est ignoré, car la réplication doit toujours répliquer la modification de schéma.  
  
-   Lorsqu'une instruction DDL ajoute une nouvelle colonne, sysarticlecolumns n'inclut pas la nouvelle colonne. Les instructions DML n'essayeront pas de répliquer les données pour la nouvelle colonne. Le paramètre est respecté parce que la réplication ou la réplication DDL est acceptable.  
  
`[ @allow_subscriber_initiated_snapshot = ] 'allow_subscriber_initiated_snapshot'` Indique si les abonnés à cette publication peuvent initier le processus d’instantané pour générer l’instantané filtré pour leur partition de données. *allow_subscriber_initiated_snapshot* est de type **nvarchar (5)**, avec false comme valeur par défaut. la **valeur true** indique que les abonnés peuvent initier le processus d’instantané.  
  
`[ @allow_web_synchronization = ] 'allow_web_synchronization'` Spécifie si la publication est activée pour la synchronisation Web. *allow_web_synchronization* est de type **nvarchar (5)**, avec false comme valeur par défaut. la **valeur true** indique que les abonnements à cette publication peuvent être synchronisés via HTTPS. Pour plus d’informations, voir [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md). Pour prendre en charge les [!INCLUDE[ssEW](../../includes/ssew-md.md)] abonnés, vous devez spécifier **true**.  
  
`[ @web_synchronization_url = ] 'web_synchronization_url'` Spécifie la valeur par défaut de l’URL Internet utilisée pour la synchronisation Web. *web_synchronization_url i* s **nvarchar (500)**, avec NULL comme valeur par défaut. Définit l’URL Internet par défaut si aucune URL n’est définie explicitement lors de l’exécution de [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) .  
  
`[ @allow_partition_realignment = ] 'allow_partition_realignment'` Détermine si les suppressions sont envoyées à l’abonné lorsque la modification de la ligne sur le serveur de publication entraîne la modification de sa partition. *allow_partition_realignment* est de type **nvarchar (5)**, avec true comme valeur par défaut. **true** envoie des suppressions à l’abonné pour refléter les résultats d’une modification de partition en supprimant des données qui ne font plus partie de la partition de l’abonné. **false** laisse les données d’une ancienne partition sur l’abonné, où les modifications apportées à ces données sur le serveur de publication ne sont pas répliquées sur cet abonné, mais les modifications apportées sur l’abonné sont répliquées sur le serveur de publication. L’affectation de la **valeur false** à *allow_partition_realignment* est utilisée pour conserver les données dans un abonnement à partir d’une ancienne partition lorsque les données doivent être accessibles à des fins d’historique.  
  
> [!NOTE]  
>  Les données qui demeurent sur l’abonné en raison de la définition de *allow_partition_realignment* sur **false** doivent être traitées comme si elles étaient en lecture seule ; Toutefois, cela n’est pas appliqué par le système de réplication.  
  
`[ @retention_period_unit = ] 'retention_period_unit'` Spécifie les unités pour la période de rétention définie par *rétention*. *retention_period_unit* est de type **nvarchar (10)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Version|  
|-----------|-------------|  
|**jour** (par défaut)|La période de rétention est spécifiée en jours.|  
|**week**|La période de rétention est spécifiée en semaines.|  
|**month**|La période de rétention est spécifiée en mois.|  
|**year**|La période de rétention est spécifiée en années.|  
  
`[ @generation_leveling_threshold = ] generation_leveling_threshold` Spécifie le nombre de modifications contenues dans une génération. Une génération est une collection de modifications remises à un serveur de publication ou à un Abonné. *generation_leveling_threshold* est de **type int**, avec une valeur par défaut de 1000.  
  
`[ @automatic_reinitialization_policy = ] automatic_reinitialization_policy`Spécifie si les modifications sont téléchargées à partir de l’abonné avant une réinitialisation automatique requise par une modification apportée à la publication, où la valeur **1** a été spécifiée pour **\@ force_reinit_subscription**. *automatic_reinitialization_policy* est de bit, avec 0 comme valeur par défaut. **1** signifie que les modifications sont téléchargées à partir de l’abonné avant une réinitialisation automatique.  
  
> [!IMPORTANT]  
>  Si vous ajoutez, supprimez ou modifiez un filtre paramétré, les modifications en attente chez l'abonné ne peuvent pas être chargées sur le serveur de publication pendant la réinitialisation. Si vous voulez télécharger les modifications en attente, synchronisez tous les abonnements avant de modifier le filtre.  
  
`[ @conflict_logging = ] 'conflict_logging'` Spécifie l’emplacement de stockage des enregistrements en conflit. *conflict_logging* est de type **nvarchar (15)** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**publisher**|Les enregistrements en conflit sont stockés sur le serveur de publication.|  
|**côté**|Les enregistrements en conflit sont stockés dans l'Abonné à l'origine du conflit. Non pris en charge pour les [!INCLUDE[ssEW](../../includes/ssew-md.md)] abonnés.|  
|**versions**|Les enregistrements en conflit sont stockés dans le serveur de publication et l'Abonné.|  
|NULL (par défaut)|La réplication définit automatiquement *conflict_logging* à la **fois** lorsque la valeur *backward_comp_level* est **90RTM** et à **Publisher** dans tous les autres cas.|  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addmergepublication** est utilisé dans la réplication de fusion.  
  
 Pour répertorier les objets de publication sur le Active Directory à l’aide du paramètre **\@ add_to_active_directory** , l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objet doit déjà être créé dans le Active Directory.  
  
 S’il existe plusieurs publications qui publient le même objet de base de données, seules les publications dont la valeur de *replicate_ddl* est **1** RÉPLIQUENT les instructions ALTER TABLE, ALTER VIEW, ALTER PROCEDURE, ALTER FUNCTION et ALTER TRIGGER DDL. Cependant, une instruction ALTER TABLE DROP COLUMN DDL sera répliquée par toutes les publications publiant la colonne supprimée.  
  
 Pour [!INCLUDE[ssEW](../../includes/ssew-md.md)] les abonnés, la valeur de *alternate_snapshot_folder* est utilisée uniquement lorsque la valeur de *snapshot_in_default_folder* est **false**.  
  
 Si la réplication DDL est activée (_replicate_ddl_**= 1**) pour une publication, afin d’effectuer des modifications DDL non répliquées dans la publication, [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) doit d’abord être exécutée pour définir *replicate_ddl* sur **0**. Après l’émission des instructions DDL non répliquées, **sp_changemergepublication** peut être réexécutée pour réactiver la réplication DDL.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_AddMergePub](../../relational-databases/replication/codesnippet/tsql/sp-addmergepublication-t_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addmergepublication**.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
