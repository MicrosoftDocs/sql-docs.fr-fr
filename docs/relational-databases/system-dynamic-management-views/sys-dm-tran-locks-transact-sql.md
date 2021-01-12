---
description: sys.dm_tran_locks (Transact-SQL)
title: sys.dm_tran_locks (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_tran_locks
- sys.dm_tran_locks
- sys.dm_tran_locks_TSQL
- dm_tran_locks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_locks dynamic management view
ms.assetid: f0d3b95a-8a00-471b-9da4-14cb8f5b045f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e3dad8a8c42e2189a29b2e75743653aca5055eae
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101462"
---
# <a name="sysdm_tran_locks-transact-sql"></a>sys.dm_tran_locks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne des informations sur les ressources actives du gestionnaire de verrous dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Chaque ligne représente une demande active au gestionnaire de verrous pour un verrou autorisé ou en attente d'autorisation.  
  
 Les colonnes du jeu de résultats sont réparties en deux groupes principaux : ressource et demande. Le groupe ressource décrit la ressource sur laquelle la demande de verrou a lieu ; le groupe demande décrit la demande de verrou.  
  
> [!NOTE]  
> Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_tran_locks**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**resource_type**|**nvarchar(60)**|Représente le type de ressource. Il peut s'agir de l'une des valeurs suivantes : DATABASE, FILE, OBJECT, PAGE, KEY, EXTENT, RID, APPLICATION, METADATA, HOBT ou ALLOCATION_UNIT.|  
|**resource_subtype**|**nvarchar(60)**|Représente un sous-type de **resource_type**. D'un point de vue technique, il est possible d'acquérir un verrou avec sous-type sans conserver un verrou sans sous-type du type parent. Les différents sous-types n'entrent pas en conflit entre eux ou avec le type parent sans sous-type. Toutes les ressources ne comportent pas de sous-types.|  
|**resource_database_id**|**int**|ID de la base de données dans laquelle cette ressource s'applique. L'ID de la base de données définit l'étendue de toutes les ressources gérées par le gestionnaire de verrous.|  
|**resource_description**|**nvarchar (256)**|Description de la ressource qui contient uniquement les informations non disponibles dans d'autres colonnes de ressources.|  
|**resource_associated_entity_id**|**bigint**|ID de l'entité dans une base de données à laquelle une ressource est associée. Selon le type de ressource, il peut s'agir d'un ID d'objet, d'un ID Hobt ou d'un ID d'unité d'allocation.|  
|**resource_lock_partition**|**Int**|ID de la partition de verrou pour une ressource de verrou partitionnée. La valeur pour les ressources de verrou non partitionnées est 0.|  
|**request_mode**|**nvarchar(60)**|Mode de la demande. Pour les demandes autorisées, il s'agit du mode autorisé ; pour les demandes en attente, il s'agit du mode demandé. <br /><br /> NULL = aucun accès n'est accordé à la ressource. Sert d'espace réservé.<br /><br /> SCH-S (stabilité du schéma) = garantit qu’un élément de schéma, tel qu’une table ou un index, n’est pas supprimé pendant qu’une session maintient un verrou de stabilité de schéma sur l’élément de schéma.<br /><br /> SCH-M (modification du schéma) = doit être détenu par une session qui souhaite modifier le schéma de la ressource spécifiée. Garantit qu'aucune autre session ne fait référence à l'objet indiqué.<br /><br /> S (partagé) = la session Holding dispose d’un accès partagé à la ressource.<br /><br /> U (mise à jour) = indique un verrou de mise à jour acquis sur les ressources qui peuvent finalement être mises à jour. Utilisé pour empêcher l'occurrence d'une forme de blocage courante qui apparaît lorsque plusieurs sessions verrouillent les ressources pour une mise à jour potentielle ultérieure.<br /><br /> X (exclusif) = la session Holding dispose d’un accès exclusif à la ressource.<br /><br /> IS (Intent partagé) = indique l’intention de placer des verrous sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> IU (mise à jour intentionnelle) = indique l’intention de placer des verrous U sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> IX (Intent exclusif) = indique l’intention de placer des verrous X sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> SIU (Shared Intent Update) = indique un accès partagé à une ressource dans le but d’acquérir des verrous de mise à jour sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> SIX (mode partagé Intent exclusif) = indique un accès partagé à une ressource dans le but d’acquérir des verrous exclusifs sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> UIX (mise à jour intentionnelle exclusive) = indique qu’un verrou de mise à jour est bloqué sur une ressource dans le but d’acquérir des verrous exclusifs sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> BU = utilisé par les opérations en bloc.<br /><br /> RangeS_S (Key-Range partagé et verrou de ressource partagé) = indique une analyse de plage sérialisable.<br /><br /> RangeS_U (Key-Range partagé et mettre à jour le verrou de ressource) = indique une analyse de mise à jour sérialisable.<br /><br /> RangeI_N (Insert Key-Range et verrou de ressource null) = utilisé pour tester des plages avant d’insérer une nouvelle clé dans un index.<br /><br /> RangeI_S = Key-Range verrou de conversion, créé par un chevauchement des verrous RangeI_N et S.<br /><br /> RangeI_U = Key-Range verrou de conversion, créé par un chevauchement de RangeI_N et de verrous U.<br /><br /> RangeI_X = Key-Range verrou de conversion, créé par un chevauchement de RangeI_N et de verrous X.<br /><br /> RangeX_S = Key-Range verrou de conversion, créé par un chevauchement de RangeI_N et RangeS_S. RangeS_S.<br /><br /> RangeX_U = Key-Range verrou de conversion, créé par un chevauchement de RangeI_N et RangeS_U des verrous.<br /><br /> RangeX_X (verrou de ressource exclusif Key-Range et exclusif) = il s’agit d’un verrou de conversion utilisé lors de la mise à jour d’une clé dans une plage.|  
|**request_type**|**nvarchar(60)**|Type de la demande. La valeur est LOCK.|  
|**request_status**|**nvarchar(60)**|État actuel de cette demande. Les valeurs possibles sont GRANTED, CONVERT, WAIT, LOW_PRIORITY_CONVERT, LOW_PRIORITY_WAIT ou ABORT_BLOCKERS. Pour plus d’informations sur les blocages Wait et Abort de faible priorité, consultez la section *low_priority_lock_wait* de la rubrique [ALTER index &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).|  
|**request_reference_count**|**smallint**|Retourne le nombre approximatif de fois que le même demandeur a demandé cette ressource.|  
|**request_lifetime**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**request_session_id**|**int**|ID de la session actuellement propriétaire de la demande. L'ID de la session propriétaire peut changer pour les transactions distribuées et liées. La valeur -2 indique que la demande appartient à une transaction distribuée orpheline. -3 indique que la demande appartient à une transaction de récupération différée, par exemple une transaction dont l'annulation a été différée à la récupération car l'annulation ne s'est pas déroulée correctement.|  
|**request_exec_context_id**|**int**|ID du contexte d'exécution du processus actuellement propriétaire de cette demande.|  
|**request_request_id**|**int**|ID de la demande (ID du traitement) du processus actuellement propriétaire de cette demande. Cette valeur change chaque fois que la connexion MARS (Multiple Active Result Set) active d'une transaction change.|  
|**request_owner_type**|**nvarchar(60)**|Type de l'entité propriétaire de la demande. Diverses entités peuvent être propriétaires des demandes de gestionnaire de verrous. Les valeurs possibles sont les suivantes :<br /><br /> TRANSACTION = Une transaction est propriétaire de la demande.<br /><br /> CURSOR = Un curseur est propriétaire de la demande.<br /><br /> SESSION = Une session utilisateur est propriétaire de la demande.<br /><br /> SHARED_TRANSACTION_WORKSPACE = La partie partagée de l'espace de travail des transactions est propriétaire de la demande.<br /><br /> EXCLUSIVE_TRANSACTION_WORKSPACE = La partie exclusive de l'espace de travail des transactions est propriétaire de la demande.<br /><br /> NOTIFICATION_OBJECT = Un composant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interne est propriétaire de la demande. Ce composant a demandé au gestionnaire de verrous de l'informer lorsqu'un autre composant attend le moment d'acquérir le verrou. La fonctionnalité FileTable est un composant qui utilise cette valeur.<br /><br /> **Remarque :** Les espaces de travail sont utilisés en interne pour maintenir des verrous pour les sessions inscrites.|  
|**request_owner_id**|**bigint**|ID du propriétaire spécifique de cette demande.<br /><br /> Lorsqu'une transaction est propriétaire de la demande, cette valeur contient l'ID de transaction.<br /><br /> Lorsqu'un FileTable est propriétaire de la demande, **request_owner_id** prend l'une des valeurs suivantes :<br /> <ul><li>**-4** : un filetable a pris un verrou de base de données.<li> **-3** : un filetable a pris un verrou de table.<li> **Autre valeur** : la valeur représente un descripteur de fichier. Cette valeur apparaît également comme **fcb_id** dans la vue de gestion dynamique [sys.dm_filestream_non_transacted_handles &#40;&#41;Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md).</li></ul>|  
|**request_owner_guid**|**uniqueidentifier**|GUID du propriétaire spécifique de cette demande. Cette valeur est utilisée uniquement par une transaction distribuée dans laquelle la valeur correspond au GUID MS DTC de cette transaction.|  
|**request_owner_lockspace_id**|**nvarchar(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Cette valeur représente l'ID de l'espace de verrouillage du demandeur. Cet ID détermine si deux demandeurs sont mutuellement compatibles et s'il est possible de leur accorder des verrous dans des modes conflictuels.|  
|**lock_owner_address**|**varbinary (8)**|Adresse mémoire de la structure des données internes utilisées pour suivre cette demande. Cette colonne peut être jointe à la colonne **resource_address** dans **sys.dm_os_waiting_tasks**.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations
Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
 
## <a name="remarks"></a>Notes  
 Un état de demande autorisée indique qu'un verrou a été accordé au demandeur sur une ressource. Une demande en attente indique que la demande n'est pas encore autorisée. La colonne **request_status** retourne les types de demandes en attente suivants :  
  
-   Une demande de conversion indique que le demandeur a déjà reçu l'autorisation pour la ressource et attend l'autorisation de mise à jour de la demande initiale.  
  
-   Une demande en attente indique que le demandeur n'a pas reçu l'autorisation pour la ressource.  
  
 Étant donné que **sys.dm_tran_locks** est remplie avec des structures de données internes du gestionnaire de verrous, la conservation de ces informations ne génère pas de charge supplémentaire dans le traitement normal. La matérialisation de la vue ne nécessite pas l'accès aux structures des données internes du gestionnaire de verrous. Ceci minimise les effets sur le traitement normal du serveur. Ces effets doivent être imperceptibles et affecter uniquement les ressources utilisées intensivement. Du fait que les données de cette vue correspondent à l'état actif du gestionnaire de verrous, elles peuvent changer à tout moment ; des lignes sont ajoutées et supprimées lorsque des verrous sont acquis et libérés. Cette vue ne comporte pas d'informations historiques.  
  
 Deux demandes agissent sur la même ressource uniquement si toutes les colonnes du groupe de ressources sont égales.  
  
 Vous pouvez contrôler le verrouillage des opérations de lecture à l'aide des outils suivants :  
  
-   SET TRANSACTION ISOLATION LEVEL pour spécifier le niveau de verrouillage d'une session. Pour plus d’informations, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
-   Conseils de verrouillage des tables pour spécifier le niveau de verrouillage de la référence d'une table dans une clause FROM. Pour connaître la syntaxe et les restrictions, consultez [indicateurs de Table &#40;&#41;Transact-SQL ](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Une ressource qui est exécutée sous un ID de session peut comporter plusieurs verrous autorisés. Différentes entités s'exécutant dans une session peuvent avoir chacune un verrou sur la même ressource ; les informations sont affichées dans les colonnes **request_owner_type** et **request_owner_id** retournées par **sys.dm_tran_locks**. S'il existe plusieurs instances pour un même **request_owner_type**, la colonne **request_owner_id** est utilisée pour les distinguer. Pour les transactions distribuées, les colonnes **request_owner_type** et **request_owner_guid** affichent des informations sur les différentes entités.  
  
 Par exemple, la Session S1 possède un verrou partagé sur la **Table1** ; et une transaction T1 exécutée dans la session S1 possède également un verrou partagé sur la **Table1**. Dans ce cas, la colonne **resource_description** retournée par **sys.dm_tran_locks** affiche deux instances de la même ressource. La colonne **request_owner_type** affiche une instance comme une session et l'autre comme une transaction. Les valeurs de la colonne **resource_owner_id** sont également différentes.  
  
 Il n'est pas possible de distinguer plusieurs curseurs d'une même session qui sont traités comme une seule entité.  
  
 Les transactions distribuées non associées à un ID de session sont des transactions orphelines. L'ID de session -2 leur est attribué. Pour plus d’informations, consultez [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md).  

## <a name="locks"></a><a name="locks"></a> Verrous
Des verrous sont placés sur les ressources [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , telles que les lignes lues ou modifiées lors d'une transaction, pour empêcher d'autres transactions d'utiliser simultanément les ressources. Par exemple, si un verrou exclusif (X) est mis en place dans une ligne de table par une transaction, aucune autre transaction ne peut modifier cette ligne jusqu'à ce que le verrou soit débloqué. Un nombre minimal de verrous favorise la concurrence, ce qui peut améliorer les performances. 

## <a name="resource-details"></a>Détails des ressources  
 Le tableau suivant répertorie les ressources qui sont représentées dans la colonne **resource_associated_entity_id**.  
  
|Type de ressource|Description de la ressource|Resource_associated_entity_id|  
|-------------------|--------------------------|--------------------------------------|  
|DATABASE|Représente une base de données.|Non applicable|  
|FILE|Représente un fichier de base de données. Il peut s'agir d'un fichier journal ou d'un fichier de données.|Non applicable|  
|OBJECT|Représente un objet de base de données. Il peut s'agir d'une table de données, d'une vue, d'une procédure stockée, d'une procédure stockée étendue ou de tout objet possédant un ID d'objet.|ID de l'objet|  
|PAGE|Représente une seule page dans un fichier de données.|ID HoBt. Cette valeur correspond à **sys.partitions.hobt_id**. L'ID HoBt n'est pas toujours disponible pour les ressources PAGE car il se compose d'informations supplémentaires que peut fournir l'appelant, et tous les appelants ne sont pas capables de fournir ces informations.|  
|KEY|Représente une ligne dans un index.|ID HoBt. Cette valeur correspond à **sys.partitions.hobt_id**.|  
|EXTENT|Représente une étendue d'un fichier de données. Une étendue est un groupe de huit pages contiguës.|Non applicable|  
|RID|Représente une ligne physique dans un segment de mémoire.|ID HoBt. Cette valeur correspond à **sys.partitions.hobt_id**. L'ID HoBt n'est pas toujours disponible pour les ressources RID car il se compose d'informations supplémentaires que peut fournir l'appelant, et tous les appelants ne sont pas capables de fournir ces informations.|  
|APPLICATION|Représente une ressource spécifiée pour une application.|Non applicable|  
|METADATA|Représente des informations de métadonnées.|Non applicable|  
|HOBT|Représente un segment de mémoire ou un arbre B-tree. Structures des chemins d'accès de base.|ID HoBt. Cette valeur correspond à **sys.partitions.hobt_id**.|  
|ALLOCATION_UNIT|Représente un ensemble de pages liées (par exemple, une partition d'index). Chaque unité d'allocation couvre une seule chaîne de pages IAM.|ID d'unité d'allocation. Cette valeur correspond à **sys.allocation_units.allocation_unit_id**.|  
  
 Le tableau suivant répertorie les sous-types qui sont associés à chaque type de ressource.  
  
|ResourceSubType|Synchronise|  
|---------------------|------------------|  
|ALLOCATION_UNIT.BULK_OPERATION_PAGE|Pages préallouées utilisées pour les opérations en bloc.|  
|ALLOCATION_UNIT.PAGE_COUNT|Statistiques du nombre de pages des unités d'allocation pendant les opérations de suppression différées.|  
|DATABASE.BULKOP_BACKUP_DB|Sauvegardes de bases de données avec des opérations en bloc.|  
|DATABASE.BULKOP_BACKUP_LOG|Sauvegardes du journal de base de données avec des opérations en bloc.|  
|DATABASE.CHANGE_TRACKING_CLEANUP|Tâches de nettoyage du suivi des modifications.|  
|DATABASE.CT_DDL|Opérations DDL de suivi des modifications aux niveaux de la base de données et de la table.|  
|DATABASE.CONVERSATION_PRIORITY|Opérations de priorité de conversation de Service Broker telles que CREATE BROKER PRIORITY.|  
|DATABASE.DDL|Opérations DDL (Data Definition Language) avec des opérations sur des groupes de fichiers (par exemple, des suppressions).|  
|DATABASE.ENCRYPTION_SCAN|Synchronisation de chiffrement TDE.|  
|DATABASE.PLANGUIDE|Synchronisation de repère de plan.|  
|DATABASE.RESOURCE_GOVERNOR_DDL|Opérations DDL pour les opérations du gouverneur de ressources telles qu'ALTER RESOURCE POOL.|  
|DATABASE.SHRINK|Opérations de réduction de base de données.|  
|DATABASE.STARTUP|Utilisé pour la synchronisation du démarrage des bases de données.|  
|FILE.SHRINK|Opérations de réduction de fichier.|  
|HOBT.BULK_OPERATION|Opérations de chargement en masse optimisées sur les segments de mémoire avec analyse simultanée sous les niveaux d'isolation suivants : instantané, lecture non validée et lecture validée utilisant le contrôle de version de ligne.|  
|HOBT.INDEX_REORGANIZE|Opérations de réorganisation d'index ou de segments mémoire.|  
|OBJECT.COMPILE|Compilation de procédure stockée.|  
|OBJECT.INDEX_OPERATION|Opérations d'index.|  
|OBJECT.UPDSTATS|Mises à jour des statistiques d'une table.|  
|METADATA.ASSEMBLY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_CLR_NAME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_TOKEN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASYMMETRIC_KEY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_ACTIONS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_SPECIFICATION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AVAILABILITY_GROUP|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CERTIFICATE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CHILD_INSTANCE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_FRAGMENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_ROWSET|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_RECV|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_SEND|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_GROUP|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_PRIORITY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CREDENTIAL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CRYPTOGRAPHIC_PROVIDER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATA_SPACE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE_PRINCIPAL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_SESSION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_WITNESS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_PRINCIPAL_SID|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT_WEBMETHOD|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_COLUMN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_CATALOG|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_INDEX|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_STOPLIST|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEX_EXTENSION_SCHEME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEXSTATS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INSTANTIATED_TYPE_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.MESSAGE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.METADATA_CACHE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PARTITION_FUNCTION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PASSWORD_POLICY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PERMISSIONS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE_SCOPE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.REMOTE_SERVICE_BINDING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ROUTE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SCHEMA|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_CACHE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_DESCRIPTOR|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SEQUENCE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_EVENT_SESSIONS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_PRINCIPAL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_BROKER_GUID|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_CONTRACT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_MESSAGE_TYPE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.STATS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SYMMETRIC_KEY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.USER_TYPE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COLLECTION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COMPONENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_INDEX_QNAME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
 Le tableau suivant fournit le format de la colonne **resource_description** pour chaque type de ressource.  
  
|Ressource|Format|Description|  
|--------------|------------|-----------------|  
|DATABASE|Non applicable|L'ID de la base de données est déjà disponible dans la colonne **resource_database_id**.|  
|FILE|<file_id>|ID du fichier représenté par cette ressource.|  
|OBJECT|<object_id>|ID de l'objet représenté par cette ressource. Il peut s'agir de n'importe quel objet figurant dans **sys.objects**, et pas seulement d'une table.|  
|PAGE|<file_id>:<page_in_file>|Représente le fichier et l'ID de la page qui est représentée par cette ressource.|  
|KEY|<hash_value>|Représente un hachage des colonnes clés de la ligne représentée par cette ressource.|  
|EXTENT|<file_id>:<page_in_file>|Représente le fichier et l'ID de la page de l'étendue représentée par cette ressource. L'ID d'étendue est identique à l'ID de la première page de l'étendue.|  
|RID|<file_id>:<page_in_file>:<row_on_page>|Représente l'ID de page et l'ID de ligne représentée par cette ressource. Remarquez que si l'ID de l'objet associé est égal à 99, cette ressource représente l'une des huit pages mélangées de la première page IAM d'une chaîne IAM.|  
|APPLICATION|\<DbPrincipalId>: \<upto 32 characters> :(<hash_value>)|Représente l'ID du principal de base de données qui est utilisé pour fixer l'étendue de la ressource des verrous sur l'application. Jusqu'à 32 caractères de la chaîne des ressources correspondent à cette ressource de verrous pour l'application. Dans certains cas, il est possible d'afficher seulement 2 caractères du fait que la chaîne complète n'est plus disponible ; cela se produit uniquement lors de la récupération d'une base de données pour les verrous d'application qui sont repris au cours de la récupération. La valeur de hachage représente un hachage de l'ensemble de la chaîne de ressource qui correspond à cette ressource de verrous pour l'application.|  
|HOBT|Non applicable|l'ID HoBt est inclus en tant que **resource_associated_entity_id**.|  
|ALLOCATION_UNIT|Non applicable|L'ID d'unité d'allocation est inclus en tant que **resource_associated_entity_id**.|  
|METADATA.ASSEMBLY|assembly_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_CLR_NAME|$qname_id = Q|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_TOKEN|assembly_id = A, $token_id|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSYMMETRIC_KEY|asymmetric_key_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT|audit_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_ACTIONS|device_id = D, major_id = M|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_SPECIFICATION|audit_specification_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AVAILABILITY_GROUP|availability_group_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CERTIFICATE|certificate_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CHILD_INSTANCE|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_FRAGMENT|object_id = O , compressed_fragment_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_ROW|object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_RECV|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_SEND|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_GROUP|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_PRIORITY|conversation_priority_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CREDENTIAL|credential_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CRYPTOGRAPHIC_PROVIDER|provider_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATA_SPACE|data_space_id = D|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE|database_id = D|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE_PRINCIPAL|principal_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_SESSION|database_id = D|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_WITNESS|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_PRINCIPAL_SID|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT|endpoint_id = E|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT_WEBMETHOD|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_CATALOG|fulltext_catalog_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_INDEX|object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_COLUMN|object_id = O, column_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_HASH|object_id = O, $hash = H|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_CATALOG|fulltext_catalog_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_INDEX|object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_STOPLIST|fulltext_stoplist_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEX_EXTENSION_SCHEME|index_extension_id = I|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEXSTATS|object_id = O, index_id ou stats_id = I|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INSTANTIATED_TYPE_HASH|user_type_id = U, hash = H|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.MESSAGE|message_id = M|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.METADATA_CACHE|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PARTITION_FUNCTION|function_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PASSWORD_POLICY|principal_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PERMISSIONS|class = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE|plan_guide_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA. PLAN_GUIDE_HASH|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA. PLAN_GUIDE_SCOPE|scope_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME|$qname_id = Q|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME_HASH|$qname_scope_id = Q, $qname_hash = H|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.REMOTE_SERVICE_BINDING|remote_service_binding_id = R|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ROUTE|route_id = R|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SCHEMA|schema_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_CACHE|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_DESCRIPTOR|sd_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SEQUENCE|$seq_type = S, object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER|server_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_EVENT_SESSIONS|event_session_id = E|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_PRINCIPAL|principal_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE|service_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_BROKER_GUID|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_CONTRACT|service_contract_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_MESSAGE_TYPE|message_type_id = M|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.STATS|object_id = O, stats_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SYMMETRIC_KEY|symmetric_key_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.USER_TYPE|user_type_id = U|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COLLECTION|xml_collection_id = X|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COMPONENT|xml_component_id = X|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_INDEX_QNAME|object_id = O, $qname_id = Q|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
 Les XEvents suivants sont liés au **commutateur** de partition et à la reconstruction d’index en ligne. Pour plus d’informations sur la syntaxe, consultez [ALTER TABLE &#40;Transact-sql&#41;](../../t-sql/statements/alter-table-transact-sql.md) et [ALTER index &#40;transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
-   lock_request_priority_state  
  
-   process_killed_by_abort_blockers  
  
-   ddl_with_wait_at_low_priority  
  
 Le **progress_report_online_index_operation** existant XEvent pour les opérations d’index en ligne a été étendu en ajoutant des **partition_number** et des **partition_id**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-sysdm_tran_locks-with-other-tools"></a>R. Utilisation de sys.dm_tran_locks avec d'autres outils  
 L'exemple suivant fonctionne dans un scénario dans lequel une mise à jour est bloquée par une autre transaction. **sys.dm_tran_locks** et d'autres outils fournissent des informations sur les ressources de verrouillage.  
  
```sql  
USE tempdb;  
GO  
  
-- Create test table and index.  
CREATE TABLE t_lock  
    (  
    c1 int, c2 int  
    );  
GO  
  
CREATE INDEX t_lock_ci on t_lock(c1);  
GO  
  
-- Insert values into test table  
INSERT INTO t_lock VALUES (1, 1);  
INSERT INTO t_lock VALUES (2,2);  
INSERT INTO t_lock VALUES (3,3);  
INSERT INTO t_lock VALUES (4,4);  
INSERT INTO t_lock VALUES (5,5);  
INSERT INTO t_lock VALUES (6,6);  
GO  
  
-- Session 1  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  
  
BEGIN TRAN  
    SELECT c1  
        FROM t_lock  
        WITH(holdlock, rowlock);  
  
-- Session 2  
BEGIN TRAN  
    UPDATE t_lock SET c1 = 10  
```  
  
 La requête suivante affiche des informations sur les verrous. La valeur de `<dbid>` doit être remplacée par la valeur **database_id** de **sys.databases**.  
  
```sql  
SELECT resource_type, resource_associated_entity_id,  
    request_status, request_mode,request_session_id,  
    resource_description   
    FROM sys.dm_tran_locks  
    WHERE resource_database_id = <dbid>  
```  
  
 La requête suivante retourne des informations d'objet à l'aide de la valeur `resource_associated_entity_id` issue de la requête précédente. Cette requête doit être exécutée alors que vous êtes connecté à la base de données qui contient l'objet.  
  
```  
SELECT object_name(object_id), *  
    FROM sys.partitions  
    WHERE hobt_id=<resource_associated_entity_id>  
```  
  
 La requête suivante affiche des informations de blocage.  
  
```sql  
SELECT   
        t1.resource_type,  
        t1.resource_database_id,  
        t1.resource_associated_entity_id,  
        t1.request_mode,  
        t1.request_session_id,  
        t2.blocking_session_id  
    FROM sys.dm_tran_locks as t1  
    INNER JOIN sys.dm_os_waiting_tasks as t2  
        ON t1.lock_owner_address = t2.resource_address;  
```  
  
 Libérez les ressources en annulant les transactions.  
  
```sql  
-- Session 1  
ROLLBACK;  
GO  
  
-- Session 2  
ROLLBACK;  
GO  
```  
  
### <a name="b-linking-session-information-to-operating-system-threads"></a>B. Liaison des informations de session avec des threads du système d'exploitation  
 L'exemple suivant retourne des informations qui associent un ID de session à l'ID d'un thread Windows. Il est possible de surveiller les performances d'un thread dans l'Analyseur de performances Windows. Cette requête ne retourne pas des ID de sessions en veille.  
  
```sql  
SELECT STasks.session_id, SThreads.os_thread_id  
    FROM sys.dm_os_tasks AS STasks  
    INNER JOIN sys.dm_os_threads AS SThreads  
        ON STasks.worker_address = SThreads.worker_address  
    WHERE STasks.session_id IS NOT NULL  
    ORDER BY STasks.session_id;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
[sys.dm_tran_database_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md)      
[Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)     
[Fonctions et vues de gestion dynamique liées aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)      
[SQL Server, objet Locks](../../relational-databases/performance-monitor/sql-server-locks-object.md)      
