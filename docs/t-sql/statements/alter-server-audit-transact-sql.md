---
description: ALTER SERVER AUDIT (Transact-SQL)
title: ALTER SERVER AUDIT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SERVER_AUDIT_TSQL
- ALTER SERVER AUDIT
dev_langs:
- TSQL
helpviewer_keywords:
- server audit [SQL Server]
- audits [SQL Server], specification
- ALTER SERVER AUDIT statement
ms.assetid: 63426d31-7a5c-4378-aa9e-afcf4f64ceb3
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: b85d564e6a1a29ec72eebfb2248ae58775c82b6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182181"
---
# <a name="alter-server-audit--transact-sql"></a>ALTER SERVER AUDIT (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie un objet d'audit du serveur à l'aide de la fonctionnalité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit. Pour plus d’informations, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER SERVER AUDIT audit_name  
{  
    [ TO { { FILE ( <file_options> [, ...n] ) } | APPLICATION_LOG | SECURITY_LOG } | URL]  
    [ WITH ( <audit_options> [ , ...n] ) ]   
    [ WHERE <predicate_expression> ]  
}  
| REMOVE WHERE  
| MODIFY NAME = new_audit_name  
[ ; ]  
  
<file_options>::=  
{  
      FILEPATH = 'os_file_path'   
    | MAXSIZE = { max_size { MB | GB | TB } | UNLIMITED }   
    | MAX_ROLLOVER_FILES = { integer | UNLIMITED }   
    | MAX_FILES = integer   
    | RESERVE_DISK_SPACE = { ON | OFF }   
}  
  
<audit_options>::=  
{  
      QUEUE_DELAY = integer   
    | ON_FAILURE = { CONTINUE | SHUTDOWN | FAIL_OPERATION }   
    | STATE = = { ON | OFF }   
}  
  
<predicate_expression>::=  
{  
    [NOT ] <predicate_factor>   
    [ { AND | OR } [NOT ] { <predicate_factor> } ]   
    [,...n ]  
}  
  
<predicate_factor>::=   
    event_field_name { = | < > | ! = | > | > = | < | < = } { number | ' string ' }  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 TO { FILE \| APPLICATION_LOG \| SECURITY \|URL}  
 Détermine l'emplacement de la cible de l'audit. Les options sont un fichier binaire, le journal des applications Windows ou le journal de sécurité Windows.  

> [!IMPORTANT]
> Dans Azure SQL Managed Instance, l’audit SQL fonctionne au niveau du serveur et stocke des fichiers `.xel` dans Stockage Blob Azure.
  
 FILEPATH **= '** _os\_file\_path_ **'**  
 Chemin d'accès de la piste d'audit. Le nom de fichier est généré en fonction du nom d'audit et du GUID d'audit.  
  
 MAXSIZE **=** _max\_size_  
 Taille maximale que peut atteindre le fichier d'audit. La valeur *max_size* doit être un entier suivi de **MB**, **GB**, **TB** ou **UNLIMITED**. La taille minimale que vous pouvez spécifier pour *max_size* est 2 **MB** et la taille maximale est 2 147 483 647 **TB**. Quand **UNLIMITED** est spécifié, la taille du fichier croît jusqu’à ce que le disque soit saturé. La spécification d’une valeur inférieure à 2 Mo génère l’erreur MSG_MAXSIZE_TOO_SMALL. La valeur par défaut est **UNLIMITED**.  
  
 MAX_ROLLOVER_FILES **=** _integer_ | **UNLIMITED**  
 Spécifie le nombre maximal de fichiers à conserver dans le système de fichiers. Quand le paramètre MAX_ROLLOVER_FILES=0 est défini, aucune limite n’est imposée quant au nombre de fichiers de substitution créés. La valeur par défaut est 0. Le nombre maximal de fichiers qui peuvent être spécifiés est 2 147 483 647.  
  
 MAX_FILES =*integer*  
 Spécifie le nombre maximal de fichiers d'audit qui peuvent être créés. N’effectue pas de substitution par le premier fichier quand la limite est atteinte. Quand la limite MAX_FILES est atteinte, toute action qui entraîne la génération d’événements d’audit supplémentaires échoue avec une erreur.  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 RESERVE_DISK_SPACE **=** { ON | OFF }  
 Cette option pré-alloue la valeur MAXSIZE au fichier sur le disque. S'applique uniquement si MAXSIZE n'est pas égal à UNLIMITED. La valeur par défaut est OFF.  
  
 QUEUE_DELAY **=** _integer_  
 Détermine la durée, en millisecondes, qui peut s'écouler avant que le traitement des actions d'audit soit forcé. Une valeur de 0 indique la remise synchrone. La valeur minimale du délai de requête définissable est 1000 (1 seconde), qui est la valeur par défaut. Le maximum est 2 147 483 647 (2 147 483,647 secondes ou 24 jours, 20 heures, 31 minutes, 23,647 secondes). La spécification d’un nombre non valide génère l’erreur MSG_INVALID_QUEUE_DELAY.  
  
 ON_FAILURE **=** { CONTINUE | SHUTDOWN | FAIL_OPERATION}  
 Indique si l'instance qui écrit dans la cible doit échouer, continuer ou s'arrêter si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas écrire dans le journal d'audit.  
  
 CONTINUE  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les opérations continuent. Les enregistrements d'audit ne sont pas conservés. L’audit poursuit sa tentative de journalisation des événements et reprend les opérations d’enregistrement une fois la condition d’échec résolue. La sélection de l’option CONTINUE peut permettre l’exécution d’une activité non auditée susceptible d’enfreindre vos stratégies de sécurité. Utilisez cette option quand la poursuite de l’opération du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est plus importante que la conservation d’un audit complet.  
  
SHUTDOWN  
Force l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à s’arrêter, si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne parvient pas à écrire des données dans la cible d’audit pour une raison quelconque. Le compte de connexion exécutant l’instruction `ALTER` doit disposer de l’autorisation `SHUTDOWN` dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le comportement d’arrêt persiste même si l’autorisation `SHUTDOWN` est révoquée ultérieurement du compte de connexion en cours d’exécution. Si l’utilisateur ne dispose pas de cette autorisation, l’instruction échoue et l’audit n’est pas modifié. Utilisez cette option si une défaillance de l'audit risque de compromettre la sécurité ou l'intégrité du système. Pour plus d’informations, consultez [SHUTDOWN](../../t-sql/language-elements/shutdown-transact-sql.md). 
  
 FAIL_OPERATION  
 Les actions de base de données échouent si elles entraînent des événements audités. Les actions qui n’entraînent pas d’événements audités peuvent continuer, mais aucun événement audité ne peut se produire. L’audit poursuit sa tentative de journalisation des événements et reprend les opérations d’enregistrement une fois la condition d’échec résolue. Utilisez cette option lorsqu'il est plus important de conserver un audit complet que de disposer d'un accès complet au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.   
  
 STATE **=** { ON | OFF }  
 Active ou désactive la collecte d'enregistrements d'audit. La modification de l'état d'un audit en cours d'exécution (de ON à OFF) crée une entrée d'audit signalant que l'audit a été arrêté et indiquant le principal qui a arrêté l'audit et l'heure d'arrêt de l'audit.  
  
 MODIFY NAME = *new_audit_name*  
 Modifie le nom de l'audit. Ne peut être utilisée avec aucune autre option.  
  
 predicate_expression  
 Spécifie l'expression de prédicat utilisée pour déterminer si un événement doit ou non être traité. Les expressions de prédicat sont limitées à 3 000 caractères, ce qui limite les arguments de chaîne.  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 event_field_name  
 Nom du champ d'événement qui identifie la source de prédicat. Les champs d’audit sont décrits dans [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md). Tous les champs peuvent être audités sauf `file_name` et `audit_file_offset`.  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 nombre  
 Tout type numérique, dont **decimal**. Le manque de mémoire physique ou un nombre trop grand pour être représenté sous forme d'entier 64 bits sont les seules limitations.  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 ' string '  
 Chaîne ANSI ou Unicode, comme requis par la comparaison de prédicat. Aucune conversion implicite de type chaîne n'est effectuée pour les fonctions de comparaison de prédicat. La transmission d'un type incorrect provoque une erreur.  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
## <a name="remarks"></a>Notes  
 Vous devez spécifier au moins l'une des clauses TO, WITH ou MODIFY NAME lorsque vous appelez ALTER AUDIT.  
  
 Vous devez définir l'état d'un audit sur l'option OFF pour apporter des modifications à un audit. Si ALTER AUDIT est exécuté pendant qu’un audit est activé avec des options autres que STATE=OFF, un message d’erreur MSG_NEED_AUDIT_DISABLED s’affiche.  
  
 Vous pouvez ajouter, modifier et supprimer des spécifications d'audit sans arrêter un audit.  
  
 Vous ne pouvez pas modifier le GUID d’un audit après sa création.  
 
 L’instruction **ALTER SERVER AUDIT** ne peut pas être utilisée à l’intérieur d’une transaction utilisateur.
 
## <a name="permissions"></a>Autorisations  
 Pour créer, modifier ou supprimer un principal de l'audit du serveur, vous devez posséder l'autorisation ALTER ANY SERVER AUDIT ou CONTROL SERVER.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-a-server-audit-name"></a>R. Modification du nom d'un audit du serveur  
 L'exemple suivant remplace le nom de l'audit du serveur `HIPAA_Audit` par `HIPAA_Audit_Old`.  
  
```sql  
USE master  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
WITH (STATE = OFF);  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
MODIFY NAME = HIPAA_Audit_Old;  
GO  
ALTER SERVER AUDIT HIPAA_Audit_Old  
WITH (STATE = ON);  
GO  
```  
  
### <a name="b-changing-a-server-audit-target"></a>B. Modification de la cible d'un audit du serveur  
 L'exemple suivant attribue à l'audit du serveur nommé `HIPAA_Audit` un fichier comme cible.  
  
```sql  
USE master  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
WITH (STATE = OFF);  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
TO FILE (FILEPATH ='\\SQLPROD_1\Audit\',  
          MAXSIZE = 1000 MB,  
          RESERVE_DISK_SPACE=OFF)  
WITH (QUEUE_DELAY = 1000,  
       ON_FAILURE = CONTINUE);  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
WITH (STATE = ON);  
GO  
```  
  
### <a name="c-changing-a-server-audit-where-clause"></a>C. Modification d'une clause WHERE d'audit du serveur  
 L’exemple suivant modifie la clause WHERE créée dans l’exemple C de [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md). La nouvelle clause WHERE applique un filtre basé sur l’ID d’événement défini par l’utilisateur 27.  
  
```sql  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = OFF)  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData]  
WHERE user_defined_event_id = 27;  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = ON);  
GO  
```  
  
### <a name="d-removing-a-where-clause"></a>D. Suppression d'une clause WHERE  
 L'exemple suivant supprime une expression de prédicat de clause WHERE.  
  
```sql  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = OFF)  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData]  
REMOVE WHERE;  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = ON);  
GO  
```  
  
### <a name="e-renaming-a-server-audit"></a>E. Renommage d’un audit du serveur  
 L'exemple suivant renomme l'audit de serveur `FilterForSensitiveData` en `AuditDataAccess`.  
  
```sql  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = OFF)  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData]  
MODIFY NAME = AuditDataAccess;  
GO  
ALTER SERVER AUDIT [AuditDataAccess] WITH (STATE = ON);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [Créer un audit du serveur et une spécification d’audit du serveur](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
