---
description: sys.dm_exec_sessions (Transact-SQL)
title: sys.dm_exec_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_sessions_TSQL
- sys.dm_exec_sessions
- dm_exec_sessions
- sys.dm_exec_sessions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_sessions dynamic management view
ms.assetid: 2b7e8e0c-eea0-431e-819f-8ccd12ec8cfa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 49f04a21e95c0cc7a63b97fd9fd4add1729b25a7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098964"
---
# <a name="sysdm_exec_sessions-transact-sql"></a>sys.dm_exec_sessions (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une ligne par session authentifiée sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. sys.dm_exec_sessions est une vue dans l'étendue du serveur qui affiche des informations sur toutes les connexions utilisateur et les tâches internes actives. Ces informations concernent la version du client, le nom du programme client, l'heure de connexion du client, l'utilisateur connecté, le paramètre de session en cours, etc. Utilisez sys.dm_exec_sessions pour d'abord consulter la charge système en cours et pour identifier une session d'intérêt, puis pour en savoir plus sur cette session en faisant appel à d'autres vues ou fonctions de gestion dynamique.  
  
 Les vues de gestion dynamique sys.dm_exec_connections, sys.dm_exec_sessions et sys.dm_exec_requests sont mappées à la table système [sys.sysprocessus](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md) .  
  
> **Remarque :** Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_exec_sessions**.  
  
|Nom de la colonne|Type de données|Description et informations spécifiques à la version|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Identifie la session associée à chaque connexion principale active. N'accepte pas la valeur NULL.|  
|login_time|**datetime**|Heure à laquelle la session a été établie. N'accepte pas la valeur NULL. Les sessions qui n’ont pas terminé la connexion au moment de l’interrogation de cette vue de gestion dynamique sont affichées avec une heure de connexion de `1900-01-01` .|  
|host_name|**nvarchar(128)**|Nom de la station de travail cliente spécifique à une session. La valeur est NULL pour les sessions internes. Autorise la valeur NULL.<br /><br /> **Remarque relative à la sécurité :** L’application cliente fournit le nom de la station de travail et peut fournir des données inexactes. Ne vous fiez pas à HOST_NAME pour garantir la sécurité.|  
|program_name|**nvarchar(128)**|Nom du programme client qui a lancé la session. La valeur est NULL pour les sessions internes. Autorise la valeur NULL.|  
|host_process_id|**int**|ID de processus du programme client qui a lancé la session. La valeur est NULL pour les sessions internes. Autorise la valeur NULL.|  
|client_version|**int**|Version du protocole TDS de l'interface utilisée par le client pour se connecter au serveur. La valeur est NULL pour les sessions internes. Autorise la valeur NULL.|  
|client_interface_name|**nvarchar(32)**|Nom de la bibliothèque/du pilote utilisé par le client pour communiquer avec le serveur. La valeur est NULL pour les sessions internes. Autorise la valeur NULL.|  
|security_id|**varbinary(85)**|ID de sécurité Microsoft Windows associé à la connexion. N'accepte pas la valeur NULL.|  
|login_name|**nvarchar(128)**|Nom de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sous lequel la session s'exécute actuellement. Pour connaître le nom de connexion d'origine qui a créé la session, consulez original_login_name. Il peut s’agir d’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nom de connexion authentifié ou d’un nom d’utilisateur de domaine authentifié Windows. N'accepte pas la valeur NULL.|  
|nt_domain|**nvarchar(128)**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Domaine Windows du client si la session utilise l'authentification Windows ou une connexion approuvée. La valeur est NULL pour les sessions internes et les utilisateurs qui n'appartiennent pas à un domaine. Autorise la valeur NULL.|  
|nt_user_name|**nvarchar(128)**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nom d'utilisateur Windows du client si la session utilise l'authentification Windows ou une connexion approuvée. La valeur est NULL pour les sessions internes et les utilisateurs qui n'appartiennent pas à un domaine. Autorise la valeur NULL.|  
|status|**nvarchar(30)**|État de la session. Valeurs possibles :<br /><br /> **Running** - une ou plusieurs demandes sont en cours d'exécution<br /><br /> **Sleeping** - aucune demande n'est en cours d'exécution<br /><br /> **Dormant** -la session a été réinitialisée en raison d’un regroupement de connexions et est désormais dans un état de préconnexion.<br /><br /> **Preconnect** - la session est dans le classifieur du gouverneur de ressources.<br /><br /> N'accepte pas la valeur NULL.|  
|context_info|**varbinary(128)**|Valeur CONTEXT_INFO pour la session. Les informations de contexte sont définies par l’utilisateur à l’aide de l’instruction [set CONTEXT_INFO](../../t-sql/statements/set-context-info-transact-sql.md) . Autorise la valeur NULL.|  
|cpu_time|**int**|Temps processeur, en millisecondes, utilisé par cette session. N'accepte pas la valeur NULL.|  
|memory_usage|**int**|Nombre de pages de mémoire de 8 Ko utilisées par cette session. N'accepte pas la valeur NULL.|  
|total_scheduled_time|**int**|Durée totale, en millisecondes, pour laquelle l'exécution de la session (demandes comprises) a été planifiée. N'accepte pas la valeur NULL.|  
|total_elapsed_time|**int**|Temps écoulé, en millisecondes, depuis que la session a été établie. N'accepte pas la valeur NULL.|  
|endpoint_id|**int**|ID du point de terminaison associé à la session. N'accepte pas la valeur NULL.|  
|last_request_start_time|**datetime**|Heure à laquelle la dernière demande de la session a commencé. Cela inclut la demande en cours. N'accepte pas la valeur NULL.|  
|last_request_end_time|**datetime**|Heure à laquelle s'est terminée pour la dernière fois une demande de la session. Autorise la valeur NULL.|  
|lectures|**bigint**|Nombre de lectures effectuées (par des demandes dans cette session) au cours de cette session. N'accepte pas la valeur NULL.|  
|écritures|**bigint**|Nombre d'écritures effectuées (par des demandes dans cette session) au cours de cette session. N'accepte pas la valeur NULL.|  
|logical_reads|**bigint**|Nombre de lectures logiques qui ont été effectuées sur cette session. N'accepte pas la valeur NULL.|  
|is_user_process|**bit**|0 si la session est une session système. Sinon, la valeur est 1. N'accepte pas la valeur NULL.|  
|text_size|**int**|Paramètre TEXTSIZE pour la session. N'accepte pas la valeur NULL.|  
|langage|**nvarchar(128)**|Paramètre LANGUAGE pour la session. Autorise la valeur NULL.|  
|date_format|**nvarchar (3)**|Paramètre DATEFORMAT pour la session. Autorise la valeur NULL.|  
|date_first|**smallint**|Paramètre DATEFIRST pour la session. N'accepte pas la valeur NULL.|  
|quoted_identifier|**bit**|Paramètre QUOTED_IDENTIFIER pour la session. N'accepte pas la valeur NULL.|  
|arithabort|**bit**|Paramètre ARITHABORT pour la session. N'accepte pas la valeur NULL.|  
|ansi_null_dflt_on|**bit**|Paramètre ANSI_NULL_DFLT_ON pour la session. N'accepte pas la valeur NULL.|  
|ansi_defaults|**bit**|Paramètre ANSI_DEFAULTS pour la session. N'accepte pas la valeur NULL.|  
|ansi_warnings|**bit**|Paramètre ANSI_WARNINGS pour la session. N'accepte pas la valeur NULL.|  
|ansi_padding|**bit**|Paramètre ANSI_PADDING pour la session. N'accepte pas la valeur NULL.|  
|ansi_nulls|**bit**|Paramètre ANSI_NULLS pour la session. N'accepte pas la valeur NULL.|  
|concat_null_yields_null|**bit**|Paramètre CONCAT_NULL_YIELDS_NULL pour la session. N'accepte pas la valeur NULL.|  
|transaction_isolation_level|**smallint**|Niveau d'isolement des transactions de la session.<br /><br /> 0 = Non spécifié<br /><br /> 1 = ReadUncommitted<br /><br /> 2 = Lecture validée<br /><br /> 3 = RepeatableRead<br /><br /> 4 = Sérialisable<br /><br /> 5 = Instantané<br /><br /> N'accepte pas la valeur NULL.|  
|lock_timeout|**int**|Paramètre LOCK_TIMEOUT pour la session. Cette valeur est exprimée en millisecondes. N'accepte pas la valeur NULL.|  
|deadlock_priority|**int**|Paramètre DEADLOCK_PRIORITY pour la session. N'accepte pas la valeur NULL.|  
|row_count|**bigint**|Nombre de lignes retournées dans la session jusqu'à présent. N'accepte pas la valeur NULL.|  
|prev_error|**int**|ID de la dernière erreur retournée dans la session. N'accepte pas la valeur NULL.|  
|original_security_id|**varbinary(85)**|ID de sécurité [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows qui est associé à original_login_name. N'accepte pas la valeur NULL.|  
|original_login_name|**nvarchar(128)**|Nom de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé par le client pour créer cette session. Il peut s'agir d'un nom de compte de connexion authentifié [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], d'un nom d'utilisateur de domaine authentifié Windows ou d'un utilisateur de base de données autonome. Notez que la session a pu faire l'objet de nombreux changements de contexte implicites ou explicites après la connexion initiale. Par exemple, si [Execute As](../../t-sql/statements/execute-as-transact-sql.md) est utilisé. N'accepte pas la valeur NULL.|  
|last_successful_logon|**datetime**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Heure de la dernière ouverture de session réussie pour original_login_name avant le démarrage de la session actuelle.|  
|last_unsuccessful_logon|**datetime**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Heure de la dernière ouverture de session qui a échoué pour original_login_name avant le démarrage de la session actuelle.|  
|unsuccessful_logons|**bigint**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nombre d'ouvertures de sessions ayant échoué pour original_login_name entre last_successful_logon et login_time.|  
|group_id|**int**|ID du groupe de charge de travail auquel cette session appartient. N'accepte pas la valeur NULL.|  
|database_id|**smallint**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> ID de la base de données active pour chaque session.|  
|authenticating_database_id|**int**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> ID de la base de données authentifiant le principal. Pour les comptes de connexion, la valeur sera de 0. Pour les utilisateurs de base de données non autonome, la valeur sera l'ID de la base de données autonome.|  
|open_transaction_count|**int**|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Nombre de transactions ouvertes par session.|  
|pdw_node_id|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
|page_server_reads|**bigint**|**S’applique à**: Azure SQL Database hyperscale<br /><br /> Nombre de lectures du serveur de pages effectuées, par demandes dans cette session, au cours de cette session. N'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
Tout le monde peut voir ses propres informations de session.  
**[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] :** Nécessite `VIEW SERVER STATE` l’autorisation sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] pour voir toutes les sessions sur le serveur.  
**[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] :** Nécessite `VIEW DATABASE STATE` de voir toutes les connexions à la base de données actuelle. `VIEW DATABASE STATE` ne peut pas être accordé dans la `master` base de données. 
  
  
## <a name="remarks"></a>Notes  
 Lorsque l’option de configuration de serveur **conformité aux critères communs** est activée, les statistiques de connexion s’affichent dans les colonnes suivantes.  
  
-   last_successful_logon  
  
-   last_unsuccessful_logon  
  
-   unsuccessful_logons  
  
 Si cette option n'est pas activée, ces colonnes retournent des valeurs NULL. Pour plus d’informations sur la façon de définir cette option de configuration de serveur, voir [Common Criteria Compliance Enabled option de configuration de serveur](../../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md).  
 
 
 Les connexions administrateur sur Azure SQL Database verront une ligne par session authentifiée. Les sessions « sa » qui apparaissent dans le jeu de résultats n’ont aucun impact sur le quota d’utilisateur pour les sessions. Les connexions non administrateur ne voient que les informations relatives à leurs sessions utilisateur de base de données.
 
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Actif/Appliquer|Relationship|  
|----------|--------|---------------|------------------|  
|sys.dm_exec_sessions|[sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|session_id|Un-à-zéro ou un-à-plusieurs|  
|sys.dm_exec_sessions|[sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)|session_id|Un-à-zéro ou un-à-plusieurs|  
|sys.dm_exec_sessions|[sys.dm_tran_session_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)|session_id|Un-à-zéro ou un-à-plusieurs|  
|sys.dm_exec_sessions|[sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)(session_id &#124; 0)|session_id CROSS APPLY<br /><br /> OUTER APPLY|Un-à-zéro ou un-à-plusieurs|  
|sys.dm_exec_sessions|[sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)|session_id|Un-à-un|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-finding-users-that-are-connected-to-the-server"></a>R. Recherche des utilisateurs connectés au serveur  
 L'exemple suivant recherche les utilisateurs connectés au serveur et retourne le nombre de sessions pour chaque utilisateur.  
  
```sql  
SELECT login_name ,COUNT(session_id) AS session_count   
FROM sys.dm_exec_sessions   
GROUP BY login_name;  
```  
  
### <a name="b-finding-long-running-cursors"></a>B. Recherche des curseurs longs  
 L'exemple suivant recherche les curseurs qui sont ouverts pendant plus longtemps que la durée fixée, l'auteur des curseurs et la session à laquelle appartiennent les curseurs.  
  
```sql  
USE master;  
GO  
SELECT creation_time ,cursor_id   
    ,name ,c.session_id ,login_name   
FROM sys.dm_exec_cursors(0) AS c   
JOIN sys.dm_exec_sessions AS s   
   ON c.session_id = s.session_id   
WHERE DATEDIFF(mi, c.creation_time, GETDATE()) > 5;  
```  
  
### <a name="c-finding-idle-sessions-that-have-open-transactions"></a>C. Recherche des sessions inactives ayant des transactions ouvertes  
 L'exemple suivant recherche des sessions inactives ayant des transactions ouvertes. Une session inactive est une session qui n'a pas de demande en cours d'exécution.  
  
```sql  
SELECT s.*   
FROM sys.dm_exec_sessions AS s  
WHERE EXISTS   
    (  
    SELECT *   
    FROM sys.dm_tran_session_transactions AS t  
    WHERE t.session_id = s.session_id  
    )  
    AND NOT EXISTS   
    (  
    SELECT *   
    FROM sys.dm_exec_requests AS r  
    WHERE r.session_id = s.session_id  
    );  
```  
  
### <a name="d-finding-information-about-a-queries-own-connection"></a>D. Recherche d'informations à propos d'une connexion propre aux requêtes  
 Requête typique pour collecter des informations sur une connexion propre aux requêtes.  
  
```sql  
SELECT   
    c.session_id, c.net_transport, c.encrypt_option,   
    c.auth_scheme, s.host_name, s.program_name,   
    s.client_interface_name, s.login_name, s.nt_domain,   
    s.nt_user_name, s.original_login_name, c.connect_time,   
    s.login_time   
FROM sys.dm_exec_connections AS c  
JOIN sys.dm_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = @@SPID;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux exécutions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  



