---
description: KILL (Transact-SQL)
title: KILL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/31/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- KILL_TSQL
- KILL
dev_langs:
- TSQL
helpviewer_keywords:
- WITH STATUSONLY option
- terminating distributed transactions
- distributed transactions [SQL Server], terminating
- in-doubt transactions
- IDs [SQL Server], user processes
- ending distributed transactions [SQL Server]
- stopping distributed transactions
- session IDs [SQL Server]
- system process termination [SQL Server]
- stopping processes
- ending processes [SQL Server]
- UOW [SQL Server]
- orphaned transactions [SQL Server]
- unit of work [SQL Server]
- process termination [SQL Server]
- KILL statement
- terminating process
ms.assetid: 071cf260-c794-4b45-adc0-0e64097938c0
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 13afe3aa357bfd968874ae1f89bf0720c39fe49c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644455"
---
# <a name="kill-transact-sql"></a>KILL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Met fin à un processus utilisateur basé sur l'ID de session ou sur l'unité de travail (UOW). Si l’ID de session ou l’UOW spécifié implique l’annulation de nombreuses tâches, l’instruction KILL peut mettre du temps à s’exécuter. Le processus prend notamment plus de temps à s’exécuter lorsque le processus implique la restauration d’une transaction longue.  
  
KILL met fin à une connexion normale, ce qui arrête en interne les transactions associées à l'ID de session spécifié. Il est possible que Distributed Transaction Coordinator (MS DTC) [!INCLUDE[msCoName](../../includes/msconame-md.md)] soit en cours d’utilisation. Si MS DTC est en cours d’utilisation, vous pouvez également utiliser l’instruction pour mettre fins à des transactions distribuées orphelines et incertaines.  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for SQL Server  
  
KILL { session ID [ WITH STATUSONLY ] | UOW [ WITH STATUSONLY | COMMIT | ROLLBACK ] }    

```  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
KILL 'session_id'  
[;]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

_ID de session_   
ID de session du processus auquel mettre fin. `session_id` est un entier unique (**int**) affecté à chaque connexion utilisateur lors de l’établissement de la connexion. La valeur de l'ID de session est liée à la connexion pendant la durée de la connexion. Lorsque la connexion se termine, la valeur entière est libérée et peut être réaffectée à une nouvelle connexion.  

La requête suivante peut vous aider à identifier quel `session_id` tuer :  

 ```sql  
 SELECT conn.session_id, host_name, program_name,
     nt_domain, login_name, connect_time, last_request_end_time 
FROM sys.dm_exec_sessions AS sess
JOIN sys.dm_exec_connections AS conn
    ON sess.session_id = conn.session_id;

```  


_UOW_   
Identifie l'ID de l'UOW (Unit of Work) des transactions distribuées. _UOW_ est un GUID qui peut être obtenu à partir de la colonne request_owner_guid de la vue de gestion dynamique `sys.dm_tran_locks`. _UOW_ peut également être obtenu à partir du journal des erreurs ou par le biais du moniteur MS DTC. Pour plus d'informations sur la surveillance des transactions distribuées, reportez-vous à la documentation de MS DTC.  
  
Utilisez KILL \<UOW> pour arrêter les transactions distribuées non résolues. Ces transactions ne sont associées à aucun ID de session réel, mais elles sont artificiellement associées à l'ID de session = « -2 ». Cet ID de session facilite l’identification des transactions non résolues en interrogeant la colonne d’ID de session dans les vues de gestion dynamique `sys.dm_tran_locks`, ` sys.dm_exec_sessions` ou `sys.dm_exec_requests`.  

_WITH STATUSONLY_   
Est utilisé pour générer un rapport de progression pour l’_UOW_ ou le `session_id` que vous spécifiez en cours de restauration en raison d’une instruction KILL antérieure. KILL WITH STATUSONLY ne met pas fin l’ID de session ou à UOW et ne les restaure pas non plus. La commande ne fait qu’afficher la progression actuelle de la restauration.

_WITH COMMIT_   
Est utilisé pour supprimer une transaction distribuée non résolue avec commit. Applicable uniquement aux transactions distribuées, vous devez spécifier une valeur d’_UOW_ pour utiliser cette option.  Pour plus d’informations, consultez [Transactions distribuées](../../database-engine/availability-groups/windows/configure-availability-group-for-distributed-transactions.md#manage-unresolved-transactions).

_WITH ROLLBACK_   
Est utilisé pour supprimer une transaction distribuée non résolue avec restauration. Applicable uniquement aux transactions distribuées, vous devez spécifier une valeur d’_UOW_ pour utiliser cette option.  Pour plus d’informations, consultez [Transactions distribuées](../../database-engine/availability-groups/windows/configure-availability-group-for-distributed-transactions.md#manage-unresolved-transactions).


  
## <a name="remarks"></a>Notes  
KILL est couramment utilisé pour mettre fin à un processus qui bloque d’autres processus importants avec des verrous. KILL peut également être utilisé pour arrêter un processus qui exécute une requête utilisant des ressources système nécessaires. Il n'est pas possible de mettre fin aux processus système et aux processus exécutant une procédure stockée étendue.  
  
Utilisez l’instruction KILL avec précaution, particulièrement lorsque des processus critiques sont en cours d’exécution. Vous ne pouvez pas tuer votre propre processus. Vous ne devez pas non plus arrêter les processus suivants :  
  
-   AWAITING COMMAND  
-   CHECKPOINT SLEEP  
-   LAZY WRITER  
-   LOCK MONITOR  
-   SIGNAL HANDLER  
  
Utilisez @@SPID pour afficher la valeur de l’ID de la session en cours.  
  
Pour obtenir un rapport des valeurs d'ID de session active, interrogez la colonne session_id des vues de gestion dynamique sys.dm_tran_locks, sys.dm_exec_sessions et sys.dm_exec_requests. Vous pouvez également afficher la colonne SPID renvoyée par la procédure stockée système sp_who. Si une restauration est en cours pour un SPID spécifique, la colonne cmd associée à celui-ci dans le jeu de résultats sp_who indique « KILLED/ROLLBACK ».  
  
Lorsqu'une connexion particulière possède un verrou sur une ressource de base de données et qu'elle bloque le déroulement d'une autre connexion, l'ID de session de la connexion bloquante apparaît dans la colonne `blocking_session_id` de `sys.dm_exec_requests` ou dans la colonne `blk` retournée par `sp_who`.  
  
La commande KILL permet de résoudre des transactions distribuées incertaines. Ce sont des transactions distribuées non résolues dues à des démarrages non planifiés du serveur de base de données ou du coordinateur MS DTC. Pour plus d’informations sur les transactions incertaines, consultez la section « Validation en deux phases » dans [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
## <a name="using-with-statusonly"></a>Utilisation de WITH STATUSONLY  
KILL WITH STATUSONLY génère un rapport si l’ID de session ou l’UOW est restauré suite à une instruction KILL _session ID_|_UOW_ antérieure. Le rapport de progression indique la proportion d'annulation réalisée (en pourcentage) et le temps restant estimé (en secondes). Le rapport indique les états sous la forme suivante :  
  
`Spid|UOW <xxx>: Transaction rollback in progress. Estimated rollback completion: <yy>% Estimated time left: <zz> seconds`  
  
Si la restauration de l’ID de session ou de l’UOW est terminée avant l’exécution de l’instruction KILL _session ID_|_UOW_ WITH STATUSONLY, KILL _session ID_|_UOW_ WITH STATUSONLY renvoie l’erreur suivante :  
  
```
"Msg 6120, Level 16, State 1, Line 1"  
"Status report cannot be obtained. Rollback operation for Process ID <session ID> is not in progress."
```  
Cette erreur se produit également si aucun ID de session ni aucun UOW ne sont en cours de restauration.


Le même rapport d’état peut être obtenu en répétant la même instruction KILL _session ID_|_UOW_ sans utiliser l’option WITH STATUSONLY. Toutefois, il n’est pas recommandé de répéter l’option de cette façon. Si vous répétez une instruction KILL _session ID_, il est possible que le nouveau processus s’arrête si la restauration est achevée et que l’ID de session a été réaffecté à une nouvelle tâche avant l’exécution de la nouvelle instruction KILL. Empêchez le nouveau processus de s’arrêter en spécifiant WITH STATUSONLY.  
  
## <a name="permissions"></a>Autorisations  
**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :** Nécessite l’autorisation ALTER ANY CONNECTION. ALTER ANY CONNECTION est incluse avec appartenance au rôle de serveur fixe sysadmin ou processadmin.  
  
**[!INCLUDE[ssSDS](../../includes/sssds-md.md)] :** Nécessite l’autorisation KILL DATABASE CONNECTION. La connexion du principal au niveau du serveur a l’autorisation KILL DATABASE CONNECTION.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-kill-to-stop-a-session"></a>R. Utilisation de KILL pour arrêter une session  
 L'exemple suivant indique comment arrêter l'ID de session `53`.  
  
```sql  
KILL 53;  
GO  
```  
  
### <a name="b-using-kill-session-id-with-statusonly-to-obtain-a-progress-report"></a>B. Utilisation de l'instruction KILL ID de session WITH STATUSONLY pour obtenir un rapport de progression  
L'exemple suivant génère un état sur le processus de restauration d'un ID de session spécifique.  
  
```sql  
KILL 54;  
KILL 54 WITH STATUSONLY;  
GO  
  
--This is the progress report.  
spid 54: Transaction rollback in progress. Estimated rollback completion: 80% Estimated time left: 10 seconds.  
```  
  
### <a name="c-using-kill-to-stop-an-orphaned-distributed-transaction"></a>C. Utilisation de KILL pour arrêter une transaction distribuée orpheline  
L’exemple suivant indique comment arrêter une transaction distribuée orpheline (ID de session = -2) avec un *UOW* de valeur `D5499C66-E398-45CA-BF7E-DC9C194B48CF`.  
  
```sql  
KILL 'D5499C66-E398-45CA-BF7E-DC9C194B48CF';  
```  

  
## <a name="see-also"></a>Voir aussi  
[KILL STATS JOB &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-stats-job-transact-sql.md)   
[KILL QUERY NOTIFICATION SUBSCRIPTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
[Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
[SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md)   
[@@SPID &#40;Transact-SQL&#41;](../../t-sql/functions/spid-transact-sql.md)   
[sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
[sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
[sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)   
[sp_lock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
[sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
  
