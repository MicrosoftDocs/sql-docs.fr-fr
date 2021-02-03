---
description: CREATE SERVER AUDIT (Transact-SQL)
title: CREATE SERVER AUDIT (Transact-SQL)
ms.custom: ''
ms.date: 01/07/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SERVER_AUDIT_TSQL
- SERVER AUDIT
- SERVER_AUDIT_TSQL
- CREATE SERVER AUDIT
dev_langs:
- TSQL
helpviewer_keywords:
- server audit [SQL Server]
- CREATE SERVER AUDIT statement
- audits [SQL Server], creating
ms.assetid: 1c321680-562e-41f1-8eb1-e7fa5ae45cc5
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 9ee8cbb22293995220f6a6cb9126c889a093222d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188474"
---
# <a name="create-server-audit-transact-sql"></a>CREATE SERVER AUDIT (Transact-SQL)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée un objet d'audit de serveur à l'aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit. Pour plus d’informations, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  

 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE SERVER AUDIT audit_name  
{  
    TO { [ FILE (<file_options> [ , ...n ] ) ] | APPLICATION_LOG | SECURITY_LOG | URL | EXTERNAL_MONITOR }  
    [ WITH ( <audit_options> [ , ...n ] ) ]   
    [ WHERE <predicate_expression> ]  
}  
[ ; ]  
  
<file_options>::=  
{  
        FILEPATH = 'os_file_path'  
    [ , MAXSIZE = { max_size { MB | GB | TB } | UNLIMITED } ]  
    [ , { MAX_ROLLOVER_FILES = { integer | UNLIMITED } } | { MAX_FILES = integer } ]  
    [ , RESERVE_DISK_SPACE = { ON | OFF } ]   
}  
  
<audit_options>::=  
{  
    [   QUEUE_DELAY = integer ]  
    [ , ON_FAILURE = { CONTINUE | SHUTDOWN | FAIL_OPERATION } ]  
    [ , AUDIT_GUID = uniqueidentifier ]  
}  
  
<predicate_expression>::=  
{  
    [NOT ] <predicate_factor>   
    [ { AND | OR } [NOT ] { <predicate_factor> } ]   
    [,...n ]  
}  
  
<predicate_factor>::=   
    event_field_name { = | < > | ! = | > | > = | < | < = | LIKE } { number | ' string ' }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 TO { FILE \| APPLICATION_LOG \| SECURITY_LOG \| URL \| EXTERNAL_MONITOR } Détermine l’emplacement de la cible d’audit. Les options sont un fichier binaire, le journal des applications Windows ou le journal de sécurité Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas écrire dans le journal de sécurité Windows sans configurer d'autres paramètres dans Windows. Pour plus d’informations, consultez [Écrire des événements d’audit SQL Server dans le journal de sécurité](../../relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log.md).  

> [!IMPORTANT]
> Dans Azure SQL Managed Instance, l’audit SQL fonctionne au niveau du serveur. Les emplacements peuvent uniquement être `URL` ou `EXTERNAL_MONITOR`.
  
 FILEPATH ='*os_file_path*'  
 Chemin d'accès du journal d'audit. Le nom de fichier est généré en fonction du nom d'audit et du GUID d'audit.  
  
 MAXSIZE = { *max_size }*  
 Taille maximale que peut atteindre le fichier d'audit. La valeur *max_size* doit être un entier suivi de MB, GB, TB ou UNLIMITED. La taille minimale que vous pouvez spécifier pour *max_size* est 2 MB et la taille maximale est 2 147 483 647 TB. Lorsque UNLIMITED est spécifié, la taille du fichier peut croître jusqu'à ce que le disque soit saturé. (0 correspond également à UNLIMITED.) La spécification d’une valeur inférieure à 2 Mo génère l’erreur MSG_MAXSIZE_TOO_SMALL. La valeur par défaut est UNLIMITED.  
  
 MAX_ROLLOVER_FILES = *{ integer* | UNLIMITED }  
 Spécifie le nombre maximal de fichiers à conserver dans le système de fichiers en plus du fichier actuel. La valeur *MAX_ROLLOVER_FILES* doit être un entier ou UNLIMITED. La valeur par défaut est UNLIMITED. Ce paramètre est évalué chaque fois que l’audit redémarre (ce qui peut se produire quand l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] redémarre ou quand l’audit est désactivé, puis réactivé) ou qu’un nouveau fichier est nécessaire car la valeur de MAXSIZE a été atteinte. Quand *MAX_ROLLOVER_FILES* est évalué, si le nombre de fichiers dépasse la valeur du paramètre *MAX_ROLLOVER_FILES*, le fichier le plus ancien est supprimé. Ainsi, quand la valeur du paramètre *MAX_ROLLOVER_FILES* est 0, un fichier est créé chaque fois que le paramètre *MAX_ROLLOVER_FILES* est évalué. Un seul fichier est automatiquement supprimé quand le paramètre *MAX_ROLLOVER_FILES* est évalué. Ainsi, quand la valeur de *MAX_ROLLOVER_FILES* diminue, le nombre de fichiers n’est pas réduit tant que les anciens fichiers ne sont pas supprimés manuellement. Le nombre maximal de fichiers qui peuvent être spécifiés est 2 147 483 647.  
  
 MAX_FILES =*integer*  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Spécifie le nombre maximal de fichiers d'audit qui peuvent être créés. N'effectue pas de substitution vers le premier fichier lorsque la limite est atteinte. Quand la limite MAX_FILES est atteinte, toute action qui entraîne la génération d’événements d’audit supplémentaires échoue avec une erreur.  
  
 RESERVE_DISK_SPACE = { ON | OFF }  
 Cette option pré-alloue la valeur MAXSIZE au fichier sur le disque. Elle s'applique uniquement si MAXSIZE n'est pas égal à UNLIMITED. La valeur par défaut est OFF.  
  
 QUEUE_DELAY =*integer*  
 Détermine la durée, en millisecondes, qui peut s’écouler avant que le traitement des actions d’audit soit forcé. Une valeur de 0 indique la remise synchrone. La valeur minimale du délai de requête définissable est 1000 (1 seconde), qui est la valeur par défaut. Le maximum est 2 147 483 647 (2 147 483,647 secondes ou 24 jours, 20 heures, 31 minutes, 23,647 secondes). La spécification d’un nombre non valide génère l’erreur MSG_INVALID_QUEUE_DELAY.  
  
 ON_FAILURE = { CONTINUE | SHUTDOWN | FAIL_OPERATION }  
 Indique si l'instance qui écrit dans la cible doit échouer, continuer ou arrêter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si la cible ne peut pas écrire dans le journal d'audit. La valeur par défaut est CONTINUE.  
  
 CONTINUE  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les opérations continuent. Les enregistrements d'audit ne sont pas conservés. L’audit poursuit sa tentative de journalisation des événements et reprend les opérations d’enregistrement une fois la condition d’échec résolue. La sélection de l’option CONTINUE peut permettre l’exécution d’une activité non auditée susceptible d’enfreindre vos stratégies de sécurité. Utilisez cette option quand la poursuite de l’opération du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est plus importante que la conservation d’un audit complet.  
  
SHUTDOWN  
Force l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à s’arrêter, si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne parvient pas à écrire des données dans la cible d’audit pour une raison quelconque. Le compte de connexion exécutant l’instruction `CREATE SERVER AUDIT` doit disposer de l’autorisation `SHUTDOWN` dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le comportement d’arrêt persiste même si l’autorisation `SHUTDOWN` est révoquée ultérieurement du compte de connexion en cours d’exécution. Si l’utilisateur ne dispose pas de cette autorisation, l’instruction échoue et l’audit n’est pas créé. Utilisez cette option si une défaillance de l'audit risque de compromettre la sécurité ou l'intégrité du système. Pour plus d’informations, consultez [SHUTDOWN](../../t-sql/language-elements/shutdown-transact-sql.md).  
  
 FAIL_OPERATION  
 Les actions de base de données échouent si elles entraînent des événements audités. Les actions qui n’entraînent pas d’événements audités peuvent continuer, mais aucun événement audité ne peut se produire. L’audit poursuit sa tentative de journalisation des événements et reprend les opérations d’enregistrement une fois la condition d’échec résolue. Utilisez cette option lorsqu'il est plus important de conserver un audit complet que de disposer d'un accès complet au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.

 AUDIT_GUID =*uniqueidentifier*  
 Pour prendre en charge des scénarios tels que la mise en miroir de bases de données, un audit a besoin d'un GUID spécifique qui correspond au GUID trouvé dans la base de données mise en miroir. Le GUID ne peut pas être modifié après la création de l'audit.  
  
 predicate_expression  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Spécifie l'expression de prédicat utilisée pour déterminer si un événement doit ou non être traité. Les expressions de prédicat sont limitées à 3 000 caractères, ce qui limite les arguments de chaîne.  
  
 event_field_name  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Nom du champ d'événement qui identifie la source de prédicat. Les champs d’audit sont décrits dans [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md). Tous les champs peuvent être filtrés sauf `file_name`, `audit_file_offset` et `event_time`.  

> [!NOTE]  
>  Alors que les champs `action_id` et `class_type` sont de type **varchar** dans sys.fn_get_audit_file, ils peuvent être utilisés uniquement avec des nombres quand ils sont une source de prédicat pour le filtrage. Pour obtenir la liste de valeurs à utiliser avec `class_type`, exécutez la requête suivante :  
> ```sql
> SELECT spt.[name], spt.[number]
> FROM   [master].[dbo].[spt_values] spt
> WHERE  spt.[type] = N'EOD'
> ORDER BY spt.[name];
> ```


 nombre  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Tout type numérique, dont **decimal**. Le manque de mémoire physique ou un nombre trop grand pour être représenté sous forme d'entier 64 bits sont les seules limitations.  
  
 ' string '  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Chaîne ANSI ou Unicode, comme requis par la comparaison de prédicat. Aucune conversion implicite de type chaîne n'est effectuée pour les fonctions de comparaison de prédicat. La transmission d'un type incorrect provoque une erreur.  
  
## <a name="remarks"></a>Notes  
 Lorsqu'un audit de serveur est créé, il est dans un état désactivé.  
  
 L'instruction CREATE SERVER AUDIT fait partie de l'étendue d'une transaction. Si la transaction est restaurée, l'instruction l'est également.  
  
## <a name="permissions"></a>Autorisations  
 Pour créer, modifier ou supprimer un audit du serveur, les principaux requièrent l'autorisation ALTER ANY SERVER AUDIT ou CONTROL SERVER.  
  
 Lorsque vous enregistrez des informations d'audit dans un fichier, pour éviter toute falsification, limitez l'accès à l'emplacement du fichier.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-server-audit-with-a-file-target"></a>R. Création d'un audit du serveur avec une cible de fichier  
 L'exemple suivant crée un audit du serveur nommé `HIPAA_Audit` avec un fichier binaire comme cible et aucune option.  
  
```sql  
CREATE SERVER AUDIT HIPAA_Audit  
    TO FILE ( FILEPATH ='\\SQLPROD_1\Audit\' );  
```  
  
### <a name="b-creating-a-server-audit-with-a-windows-application-log-target-with-options"></a>B. Création d'un audit du serveur avec le journal des applications Windows comme cible et des options  
 L'exemple suivant crée un audit du serveur nommé `HIPAA_Audit` avec la cible définie sur le journal des applications Windows. La file d'attente est écrite chaque seconde et arrête le moteur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cas d'échec.  
  
```sql  
CREATE SERVER AUDIT HIPAA_Audit  
    TO APPLICATION_LOG  
    WITH ( QUEUE_DELAY = 1000,  ON_FAILURE = SHUTDOWN);  
```  
  
###  <a name="c-creating-a-server-audit-containing-a-where-clause"></a><a name="ExampleWhere"></a> C. Création d'un audit de serveur contenant une clause WHERE  
 L'exemple suivant crée une base de données, un schéma et deux tables pour l'exemple. La table `DataSchema.SensitiveData` contient des données confidentielles et tout accès à cette table doit être enregistré dans l’audit. La table `DataSchema.GeneralData` ne contient pas de données confidentielles. La spécification de l'audit de la base de données audite les accès à tous les objets du schéma `DataSchema`. L'audit du serveur est créé avec une clause WHERE qui limite l'audit à la seule table `SensitiveData`. L’audit du serveur présume qu’un dossier d’audit existe dans `C:\SQLAudit`.  
  
```sql  
CREATE DATABASE TestDB;  
GO  
USE TestDB;  
GO  
CREATE SCHEMA DataSchema;  
GO  
CREATE TABLE DataSchema.GeneralData (ID int PRIMARY KEY, DataField varchar(50) NOT NULL);  
GO  
CREATE TABLE DataSchema.SensitiveData (ID int PRIMARY KEY, DataField varchar(50) NOT NULL);  
GO  
-- Create the server audit in the master database  
USE master;  
GO  
CREATE SERVER AUDIT AuditDataAccess  
    TO FILE ( FILEPATH ='C:\SQLAudit\' )  
    WHERE object_name = 'SensitiveData' ;  
GO  
ALTER SERVER AUDIT AuditDataAccess WITH (STATE = ON);  
GO  
-- Create the database audit specification in the TestDB database  
USE TestDB;  
GO  
CREATE DATABASE AUDIT SPECIFICATION [FilterForSensitiveData]  
FOR SERVER AUDIT [AuditDataAccess]   
ADD (SELECT ON SCHEMA::[DataSchema] BY [public])  
WITH (STATE = ON);  
GO  
-- Trigger the audit event by selecting from tables  
SELECT ID, DataField FROM DataSchema.GeneralData;  
SELECT ID, DataField FROM DataSchema.SensitiveData;  
GO  
-- Check the audit for the filtered content  
SELECT * FROM fn_get_audit_file('C:\SQLAudit\AuditDataAccess_*.sqlaudit',default,default);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
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
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Créer un audit du serveur et une spécification d’audit du serveur](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
