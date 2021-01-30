---
description: sys.dm_exec_requests (Transact-SQL)
title: sys.dm_exec_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_requests_TSQL
- sys.dm_exec_requests
- dm_exec_requests_TSQL
- dm_exec_requests
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_requests dynamic management view
ms.assetid: 4161dc57-f3e7-4492-8972-8cfb77b29643
author: pmasl
ms.author: pelopes
ms.reviewer: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 79fdfc18a80ebff0e6e737db4efcdc81e5ae265d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192976"
---
# <a name="sysdm_exec_requests-transact-sql"></a>sys.dm_exec_requests (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne des informations sur chaque requête qui s’exécute dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations sur les demandes, consultez le [Guide d’architecture des threads et des tâches](../../relational-databases/thread-and-task-architecture-guide.md).
   
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|ID de la session à laquelle cette demande est liée. N'accepte pas la valeur NULL.|  
|request_id|**int**|ID de la demande. Unique dans le contexte de la session. N'accepte pas la valeur NULL.|  
|start_time|**datetime**|Horodateur lors la réception de la demande. N'accepte pas la valeur NULL.|  
|status|**nvarchar(30)**|Statut de la demande. Il peut s'agir de l'une des ressources suivantes :<br /><br /> Arrière-plan<br />Exécution en cours<br />Exécutable<br />En état de veille<br />Interrompu<br /><br /> N'accepte pas la valeur NULL.|  
|.|**nvarchar(32)**|Identifie le type de commande en cours de traitement. Les types de commandes courants comprennent notamment :<br /><br /> SELECT<br />INSERT<br />UPDATE<br />Suppression<br />BACKUP LOG<br />BACKUP DATABASE<br />DBCC<br />FOR<br /><br /> Le texte de la demande peut être extrait à l'aide de sys.dm_exec_sql_text avec le paramètre sql_handle correspondant pour la demande. Des processus système internes définissent la commande en fonction du type de tâche à exécuter. Il peut s'agir des tâches suivantes :<br /><br /> LOCK MONITOR<br />CHECKPOINTLAZY<br />WRITER<br /><br /> N'accepte pas la valeur NULL.|  
|sql_handle|**varbinary(64)**|Jeton qui identifie de façon unique le lot ou la procédure stockée dont fait partie la requête. Autorise la valeur NULL.| 
|statement_start_offset|**int**|Indique, en octets, à partir de 0, la position de départ de l’instruction en cours d’exécution pour le lot en cours d’exécution ou l’objet persistant. Peut être utilisé avec `sql_handle` , `statement_end_offset` et la `sys.dm_exec_sql_text` fonction de gestion dynamique pour récupérer l’instruction en cours d’exécution pour la demande. Autorise la valeur NULL.|  
|statement_end_offset|**int**|Indique, en octets, à partir de 0, la position de fin de l’instruction en cours d’exécution pour le lot en cours d’exécution ou l’objet persistant. Peut être utilisé avec `sql_handle` , `statement_start_offset` et la `sys.dm_exec_sql_text` fonction de gestion dynamique pour récupérer l’instruction en cours d’exécution pour la demande. Autorise la valeur NULL.|  
|plan_handle|**varbinary(64)**|Jeton qui identifie de façon unique un plan d’exécution de requête pour un lot en cours d’exécution. Autorise la valeur NULL.|  
|database_id|**smallint**|ID de la base de données dans laquelle la requête s'exécute. N'accepte pas la valeur NULL.|  
|user_id|**int**|ID de l'utilisateur qui a envoyé la demande. N'accepte pas la valeur NULL.|  
|connection_id|**uniqueidentifier**|ID de la connexion à laquelle la demande est parvenue. Autorise la valeur NULL.|  
|blocking_session_id|**smallint**|ID de la session qui bloque la demande. Si cette colonne est NULL ou égale à 0, la demande n’est pas bloquée ou les informations de session de la session de blocage ne sont pas disponibles (ou ne peuvent pas être identifiées).<br /><br /> -2 = La ressource qui bloque la demande appartient à une transaction distribuée orpheline.<br /><br /> -3 = La ressource qui bloque la demande appartient à une transaction de récupération différée.<br /><br /> -4 = L'ID de session du propriétaire du verrou qui bloque la demande n'a pas pu être déterminé pour le moment en raison de transitions d'état de verrou interne.|  
|wait_type|**nvarchar(60)**|Si la demande est actuellement bloquée, cette colonne retourne le type d'attente. Autorise la valeur NULL.<br /><br /> Pour plus d’informations sur les types d’attentes, consultez [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|wait_time|**int**|Si la demande est actuellement bloquée, cette colonne retourne la durée de l'attente, en millisecondes. N'accepte pas la valeur NULL.|  
|last_wait_type|**nvarchar(60)**|Si la demande a été bloquée précédemment, cette colonne indique le type de la dernière attente. N'accepte pas la valeur NULL.|  
|wait_resource|**nvarchar (256)**|Si la demande est actuellement bloquée, cette colonne retourne la ressource attendue par la demande. N'accepte pas la valeur NULL.|  
|open_transaction_count|**int**|Nombre de transactions ouvertes pour cette demande. N'accepte pas la valeur NULL.|  
|open_resultset_count|**int**|Nombre de jeux de résultats ouverts pour cette demande. N'accepte pas la valeur NULL.|  
|transaction_id|**bigint**|ID de la transaction dans laquelle cette demande s'exécute. N'accepte pas la valeur NULL.|  
|context_info|**varbinary(128)**|Valeur CONTEXT_INFO de la session. Autorise la valeur NULL.|  
|percent_complete|**real**|Le pourcentage de travail terminé pour les commandes suivantes :<br /><br /> ALTER INDEX REORGANIZE<br />Option AUTO_SHRINK avec ALTER DATABASE<br />BACKUP DATABASE<br />DBCC CHECKDB<br />DBCC CHECKFILEGROUP<br />DBCC CHECKTABLE<br />DBCC INDEXDEFRAG<br />DBCC SHRINKDATABASE<br />DBCC SHRINKFILE<br />RECOVERY<br />RESTORE DATABASE<br />ROLLBACK<br />TDE ENCRYPTION<br /><br /> N'accepte pas la valeur NULL.|  
|estimated_completion_time|**bigint**|Interne uniquement. N'accepte pas la valeur NULL.|  
|cpu_time|**int**|Quantité de temps UC (en millisecondes) utilisée par la demande. N'accepte pas la valeur NULL.|  
|total_elapsed_time|**int**|Temps total écoulé en millisecondes depuis l'arrivée de la demande. N'accepte pas la valeur NULL.|  
|scheduler_id|**int**|ID du planificateur qui planifie cette demande. N'accepte pas la valeur NULL.|  
|task_address|**varbinary (8)**|Adresse mémoire de la tâche associée à la demande. Autorise la valeur NULL.|  
|lectures|**bigint**|Nombre de lectures effectuées par la demande. N'accepte pas la valeur NULL.|  
|écritures|**bigint**|Nombre d'écritures effectuées par la demande. N'accepte pas la valeur NULL.|  
|logical_reads|**bigint**|Nombre de lectures logiques effectuées par la demande. N'accepte pas la valeur NULL.|  
|text_size|**int**|Valeur du paramètre TEXTSIZE pour la demande. N'accepte pas la valeur NULL.|  
|langage|**nvarchar(128)**|Valeur du paramètre de langue pour la demande. Autorise la valeur NULL.|  
|date_format|**nvarchar (3)**|Valeur du paramètre DATEFORMAT pour la demande. Autorise la valeur NULL.|  
|date_first|**smallint**|Valeur du paramètre DATEFIRST pour la demande. N'accepte pas la valeur NULL.|  
|quoted_identifier|**bit**|1 = QUOTED_IDENTIFIER est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|arithabort|**bit**|1 = ARITHABORT est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|ansi_null_dflt_on|**bit**|1 = ANSI_NULL_DFLT_ON est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|ansi_defaults|**bit**|1 = ANSI_DEFAULTS est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|ansi_warnings|**bit**|1 = ANSI_WARNINGS est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|ansi_padding|**bit**|1 = ANSI_PADDING est activé (ON) pour la demande.<br /><br /> Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|ansi_nulls|**bit**|1 = ANSI_NULLS est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|concat_null_yields_null|**bit**|1 = CONCAT_NULL_YIELDS_NULL est activé (ON) pour la demande. Sinon, la valeur est 0.<br /><br /> N'accepte pas la valeur NULL.|  
|transaction_isolation_level|**smallint**|Niveau d'isolation avec lequel la transaction pour cette demande est créée. N'accepte pas la valeur NULL.<br /> 0 = Non spécifié<br /> 1 = Lecture non validée<br /> 2 = Lecture validée<br /> 3 = Répétable<br /> 4 = Sérialisable<br /> 5 = Instantané|  
|lock_timeout|**int**|Délai d'attente de verrou externe pour la demande, en millisecondes. N'accepte pas la valeur NULL.|  
|deadlock_priority|**int**|Paramètre DEADLOCK_PRIORITY de la demande. N'accepte pas la valeur NULL.|  
|row_count|**bigint**|Nombre de lignes retournées au client par cette demande. N'accepte pas la valeur NULL.|  
|prev_error|**int**|Dernière erreur générée pendant l'exécution de la demande. N'accepte pas la valeur NULL.|  
|nest_level|**int**|Niveau d'imbrication actuel du code en cours d'exécution sur la demande. N'accepte pas la valeur NULL.|  
|granted_query_memory|**int**|Nombre de pages allouées à l'exécution d'une requête dans la demande. N'accepte pas la valeur NULL.|  
|executing_managed_code|**bit**|Indique si une demande spécifique est en train d'exécuter des objets CLR (Common Language Runtime) tels que des routines, des types et des déclencheurs. Cette valeur est définie pour toute la durée pendant laquelle un objet CLR réside dans la pile, même lorsqu'elle exécute [!INCLUDE[tsql](../../includes/tsql-md.md)] à partir du CLR. N'accepte pas la valeur NULL.|  
|group_id|**int**|ID du groupe de charge de travail auquel cette requête appartient. N'accepte pas la valeur NULL.|  
|query_hash|**Binary(8**|La valeur de hachage binaire calculée sur la requête et utilisée pour identifier des requêtes avec une logique similaire. Vous pouvez utiliser le hachage de requête pour déterminer l'utilisation des ressources globale pour les requêtes qui diffèrent uniquement par les valeurs littérales.|  
|query_plan_hash|**Binary(8**|Valeur de hachage binaire calculée sur le plan d'exécution de requête et utilisée pour identifier des plans d'exécution de requête semblables. Vous pouvez utiliser le hachage de plan de requête pour rechercher le coût cumulatif de requêtes avec les plans d'exécution semblables.|  
|statement_sql_handle|**varbinary(64)**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Descripteur SQL de la requête individuelle.<br /><br />Cette colonne a la valeur NULL si Magasin des requêtes n’est pas activée pour la base de données. |  
|statement_context_id|**bigint**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Clé étrangère facultative à sys.query_context_settings.<br /><br />Cette colonne a la valeur NULL si Magasin des requêtes n’est pas activée pour la base de données. |  
|dop |**int** |**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures.<br /><br /> Degré de parallélisme de la requête. |  
|parallel_worker_count |**int** |**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures.<br /><br /> Nombre de threads de travail parallèles réservés s’il s’agit d’une requête parallèle.  |  
|external_script_request_id |**uniqueidentifier** |**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures.<br /><br /> ID de demande de script externe associé à la requête actuelle. |  
|is_resumable |**bit** |**S’applique à** : [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] et versions ultérieures.<br /><br /> Indique si la demande est une opération d’index pouvant être reprise. |  
|page_resource |**Binary(8** |**S’applique à** : [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]<br /><br /> Représentation hexadécimale sur 8 octets de la ressource de page si la `wait_resource` colonne contient une page. Pour plus d’informations, consultez [sys.fn_PageResCracker](../../relational-databases/system-functions/sys-fn-pagerescracker-transact-sql.md). |  
|page_server_reads|**bigint**|**S’applique à**: Azure SQL Database hyperscale<br /><br /> Nombre de lectures du serveur de pages effectuées par cette demande. N'accepte pas la valeur NULL.|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="remarks"></a>Notes 
Pour exécuter du code externe à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (par exemple, des procédures stockées étendues et des requêtes distribuées), un thread doit s'exécuter en dehors du contrôle du planificateur non préemptif. Pour ce faire, un processus de travail passe en mode préemptif. Les valeurs temporelles retournées par cette vue de gestion dynamique n'incluent pas le temps passé en mode préemptif.

Lors de l’exécution de requêtes parallèles en [mode ligne](../../relational-databases/query-processing-architecture-guide.md#row-mode-execution), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assigne un thread de travail pour coordonner les threads de travail chargés d’effectuer les tâches qui leur sont attribuées. Dans cette vue de gestion dynamique, seul le thread coordinateur est visible pour la demande. Les colonnes **reads**, **writes**, **LOGICAL_READS** et **row_count** ne sont **pas mises à jour** pour le thread coordinateur. Les colonnes **wait_type**, **wait_time**, **last_wait_type**, **wait_resource** et **granted_query_memory** sont **uniquement mises à jour** pour le thread coordinateur. Pour plus d’informations, consultez le [Guide de l’architecture des threads et des tâches](../../relational-databases/thread-and-task-architecture-guide.md).

## <a name="permissions"></a>Autorisations
Si l’utilisateur dispose `VIEW SERVER STATE` de l’autorisation sur le serveur, il voit toutes les sessions en cours d’exécution sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; sinon, il ne verra que la session en cours. `VIEW SERVER STATE` ne peut pas être accordé dans Azure SQL Database donc `sys.dm_exec_requests` est toujours limité à la connexion actuelle.

Dans Always-On scénarios, si le réplica secondaire est défini sur **lecture-intention uniquement**, la connexion à l’application secondaire doit spécifier son intention d’application dans les paramètres de chaîne de connexion en ajoutant `applicationintent=readonly` . Dans le cas contraire, la vérification de l’accès `sys.dm_exec_requests` ne réussira pas pour les bases de données du groupe de disponibilité, même si l' `VIEW SERVER STATE` autorisation est présente.

  
## <a name="examples"></a>Exemples  
  
### <a name="a-finding-the-query-text-for-a-running-batch"></a>R. Recherche du texte de la requête pour un lot en cours d'exécution

 L'exemple suivant interroge `sys.dm_exec_requests` afin de rechercher la requête qui vous intéresse et de copier son `sql_handle` de la sortie.  

```sql
SELECT * FROM sys.dm_exec_requests;  
GO  
```  

Puis, pour obtenir le texte d'instruction, utilisez le `sql_handle` copié avec la fonction système `sys.dm_exec_sql_text(sql_handle)`.  

```sql
SELECT * FROM sys.dm_exec_sql_text(< copied sql_handle >);  
GO  
```

### <a name="b-finding-all-locks-that-a-running-batch-is-holding"></a>B. Recherche de tous les verrous maintenus par un lot en cours d'exécution

L’exemple suivant interroge **sys.dm_exec_requests** pour trouver le lot intéressant et copier son `transaction_id` à partir de la sortie.

```sql
SELECT * FROM sys.dm_exec_requests;  
GO
```

Ensuite, pour rechercher des informations de verrou, utilisez le copié `transaction_id` avec la fonction système **sys.dm_tran_locks**.  

```sql
SELECT * FROM sys.dm_tran_locks
WHERE request_owner_type = N'TRANSACTION'
    AND request_owner_id = < copied transaction_id >;
GO  
```

### <a name="c-finding-all-currently-blocked-requests"></a>C. Recherche de toutes les demandes bloquées actuellement

L’exemple suivant interroge **sys.dm_exec_requests** pour trouver des informations sur les demandes bloquées.  

```sql
SELECT session_id ,status ,blocking_session_id  
    ,wait_type ,wait_time ,wait_resource
    ,transaction_id
FROM sys.dm_exec_requests
WHERE status = N'suspended';  
GO  
```  

### <a name="d-ordering-existing-requests-by-cpu"></a>D. Classement des demandes existantes par UC

```sql
SELECT 
   req.session_id
   , req.start_time
   , cpu_time 'cpu_time_ms'
   , object_name(st.objectid,st.dbid) 'ObjectName' 
   , substring
      (REPLACE
        (REPLACE
          (SUBSTRING
            (ST.text
            , (req.statement_start_offset/2) + 1
            , (
               (CASE statement_end_offset
                  WHEN -1
                  THEN DATALENGTH(ST.text)  
                  ELSE req.statement_end_offset
                  END
                    - req.statement_start_offset)/2) + 1)
       , CHAR(10), ' '), CHAR(13), ' '), 1, 512)  AS statement_text  
FROM sys.dm_exec_requests AS req  
   CROSS APPLY sys.dm_exec_sql_text(req.sql_handle) as ST
   ORDER BY cpu_time desc;
GO
```

## <a name="see-also"></a>Voir aussi
[Fonctions et vues de gestion dynamique](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)     
[Fonctions et vues de gestion dynamique liées à l’exécution](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)      
[sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)     
[sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)     
[sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)    
[sys.dm_exec_query_plan](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)      
[SQL Server, objet SQL Statistics](../../relational-databases/performance-monitor/sql-server-sql-statistics-object.md)     
[Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md#DOP)       
[Guide d’architecture de thread et de tâche](../../relational-databases/thread-and-task-architecture-guide.md)    
