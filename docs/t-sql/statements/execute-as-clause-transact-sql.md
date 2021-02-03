---
description: Clause EXECUTE AS (Transact-SQL)
title: EXECUTE AS Clause (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AS
- AS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- permission sets [SQL Server]
- queues [SQL Server]
- stored procedures [SQL Server], executing
- user-defined functions [SQL Server], execution context
- EXECUTE AS
- triggers [SQL Server], execution context
- execution context [SQL Server]
- switching execution context
- functions [SQL Server], execution context
ms.assetid: bd517aa3-f06e-4356-87d8-70de5df4494a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f36b2dd484e1308e8b7ee71d19d8f1f6315c3f59
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159152"
---
# <a name="execute-as-clause-transact-sql"></a>Clause EXECUTE AS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez définir le contexte d'exécution de ces modules définis par l'utilisateur : fonctions (à l'exception des fonctions table incluses), procédures, files d'attente et déclencheurs.  
  
 En spécifiant le contexte dans lequel le module s'exécute, vous contrôlez le compte d'utilisateur que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise pour valider les autorisations sur des objets référencés par le module. Cela améliore la souplesse et offre une meilleure gestion des autorisations sur la chaîne d'objets qui existe entre les modules définis par l'utilisateur et les objets qu'ils référencent. Les autorisations doivent être accordées aux utilisateurs uniquement sur le module lui-même, sans qu'il soit nécessaire de leur accorder des autorisations explicites sur les objets référencés. Seul l'utilisateur du module en cours d'exécution doit avoir les autorisations sur les objets auxquels le module accède.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- SQL Server Syntax  
Functions (except inline table-valued functions), Stored Procedures, and DML Triggers  
{ EXEC | EXECUTE } AS { CALLER | SELF | OWNER | 'user_name' }   
  
DDL Triggers with Database Scope  
{ EXEC | EXECUTE } AS { CALLER | SELF | 'user_name' }   
  
DDL Triggers with Server Scope and logon triggers  
{ EXEC | EXECUTE } AS { CALLER | SELF | 'login_name' }   
  
Queues  
{ EXEC | EXECUTE } AS { SELF | OWNER | 'user_name' }   
```  
  
```syntaxsql
-- Azure SQL Database Syntax  
Functions (except inline table-valued functions), Stored Procedures, and DML Triggers  
  
{ EXEC | EXECUTE } AS { CALLER | SELF | OWNER | 'user_name' }   
  
DDL Triggers with Database Scope  
  
{ EXEC | EXECUTE } AS { CALLER | SELF | 'user_name' }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 **CALLER**  
 Spécifie que les instructions à l'intérieur du module sont exécutées dans le contexte de l'appelant du module. L'utilisateur qui exécute le module doit disposer des autorisations nécessaires non seulement sur le module lui-même, mais également sur tout objet de base de données référencé par le module.  
  
 CALLER est l'argument par défaut pour tous les modules, à l'exception des files d'attente. Son comportement est identique dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
 Vous ne pouvez pas spécifier CALLER dans une instruction CREATE QUEUE ou ALTER QUEUE.  
  
 **SELF**  
 EXECUTE AS SELF est équivalent à EXECUTE AS *user_name*, où l’utilisateur spécifié est la personne qui crée ou modifie le module. L’ID utilisateur en cours de la personne qui crée ou modifie les modules est stocké dans la colonne **execute_as_principal_id** de la vue de catalogue **sys.sql_modules** ou **sys.service_queues**.  
  
 SELF est l'argument par défaut pour les files d'attente.  
  
> [!NOTE]  
>  Pour modifier l’ID utilisateur de **execute_as_principal_id** dans la vue de catalogue **sys.service_queues**, vous devez spécifier explicitement la clause EXECUTE AS dans l’instruction ALTER QUEUE.  
  
 OWNER  
 Spécifie que les instructions à l'intérieur du module sont exécutées dans le contexte du propriétaire actuel du module. Si aucun propriétaire n'est spécifié pour le module, le propriétaire du schéma du module est utilisé. Il n'est pas possible de spécifier OWNER pour des déclencheurs DDL ou de connexion.  
  
> [!IMPORTANT]  
>  OWNER doit mapper à un compte singleton et ne peut pas être un rôle ou un groupe.  
  
 **'** *user_name* **'**  
 Spécifie que les instructions à l’intérieur du module sont exécutées dans le contexte de l’utilisateur spécifié dans *user_name*. Les autorisations sur n’importe quel objet du module sont vérifiées par rapport à l’utilisateur *user_name*. Vous ne pouvez pas spécifier *user_name* pour des déclencheurs DDL sur l’étendue du serveur ou des déclencheurs de connexion. Utilisez *login_name* à la place.  
  
 *user_name* doit exister dans la base de données active et doit être un compte singleton. *user_name* ne peut pas être un groupe, un rôle, un certificat, une clé ou un compte intégré tel que NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService ou NT AUTHORITY\LocalSystem.  
  
 L’ID utilisateur du contexte d’exécution est stocké dans les métadonnées. Vous pouvez l’afficher dans la colonne **execute_as_principal_id** de la vue de catalogue **sys.sql_modules** ou **sys.assembly_modules**.  
  
 **'** *login_name* **'**  
 Spécifie que les instructions à l’intérieur du module sont exécutées dans le contexte de la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifiée dans *login_name*. Les autorisations sur n’importe quel objet du module sont vérifiées pour l’utilisateur *login_name*. Vous pouvez spécifier *login_name* uniquement pour des déclencheurs DDL sur l’étendue du serveur ou des déclencheurs de connexion.  
  
 *login_name* ne peut pas être un groupe, un rôle, un certificat, une clé ou un compte intégré tel que NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService ou NT AUTHORITY\LocalSystem.  
  
## <a name="remarks"></a>Remarques  
 La façon dont le [!INCLUDE[ssDE](../../includes/ssde-md.md)] évalue les autorisations sur les objets référencés dans le module dépend de la chaîne d’appartenance entre les objets appelants et les objets référencés. Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la chaîne d'appartenance était la seule méthode qui permettait d'éviter d'accorder l'accès de l'utilisateur appelant à tous les objets référencés.  
  
 La chaîne d'appartenance est soumise aux limitations suivantes :  
  
-   S'applique uniquement aux instructions DML : SELECT, INSERT, UPDATE et DELETE.  
  
-   Les propriétaires des objets appelants et appelés doivent être identiques.  
  
-   Elle ne s'applique pas aux requêtes dynamiques à l'intérieur du module.  
  
 Indépendamment du contexte d'exécution spécifié dans le module, les actions suivantes s'appliquent toujours :  
  
-   Quand le module est exécuté, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie d’abord que l’utilisateur qui exécute le module a l’autorisation EXECUTE sur ce module.  
  
-   Les règles de la chaîne d'appartenance s'appliquent toujours. Cela signifie que, si les propriétaires des objets appelants et appelés sont identiques, aucune autorisation n'est vérifiée sur les objets sous-jacents.  
  
 Lorsqu'un utilisateur exécute un module spécifié pour s'exécuter dans un contexte différent de CALLER, l'autorisation de l'utilisateur pour exécuter le module est vérifiée. Cependant, des vérifications supplémentaires sur les objets accédés par le module sont effectuées par rapport au compte d'utilisateur spécifié dans la clause EXECUTE AS. En effet, l'utilisateur qui exécute le module emprunte l'identité de l'utilisateur spécifié.  
  
 Le contexte spécifié dans la clause EXECUTE AS du module est applicable uniquement pour la durée d'exécution du module. Le contexte de l'appelant est rétabli à la fin de l'exécution du module.  
  
## <a name="specifying-a-user-or-login-name"></a>Spécification d'un utilisateur ou d'un nom d'accès  
 Vous ne pouvez pas supprimer un utilisateur de base de données ou un nom de connexion au serveur dans la clause EXECUTE AS tant que le module n'a pas été modifié pour s'exécuter dans un autre contexte.  
  
 L’utilisateur ou le nom de connexion spécifié dans la clause EXECUTE AS doit exister en tant que principal respectivement dans **sys.database_principals** ou **sys.server_principals**, faute de quoi la création ou la modification du module échoue. De plus, l'utilisateur qui crée ou modifie le module doit posséder les autorisations IMPERSONATE sur le principal.  
  
 Si l'utilisateur dispose de l'accès implicite à la base de données ou à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par l'intermédiaire de l'appartenance à un groupe Windows, l'utilisateur spécifié dans la clause EXECUTE AS est implicitement créé lors de la création du module lorsqu'une des conditions suivantes est remplie :  
  
-   L’utilisateur ou le nom de connexion spécifié est membre du rôle serveur fixe **sysadmin**.  
  
-   L'utilisateur qui crée le module possède l'autorisation de créer des principaux.  
  
 Si aucune de ces conditions n'est remplie, la création du module échoue.  
  
> [!IMPORTANT]  
>  Si le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER) s'exécute en tant que compte local (service local ou compte d'utilisateur local), il n'a pas les privilèges pour obtenir les appartenances aux groupes d'un compte de domaine Windows spécifié dans la clause EXECUTE AS. Dans ce cas, l'exécution du module échoue.  
  
 Par exemple, supposons les conditions suivantes :  
  
-   Le groupe **CompanyDomain\SQLUsers** a accès à la base de données **Sales**.  
  
-   L’utilisateur **CompanyDomain\SqlUser1** est membre de **SQLUsers** : il a donc accès à la base de données **Sales**.  
  
-   L'utilisateur qui crée ou modifie le module a les autorisations pour créer des principaux.  
  
 Lorsque l'instruction `CREATE PROCEDURE` suivante est exécutée, l'utilisateur `CompanyDomain\SqlUser1` est implicitement créé en tant que principal dans la base de données `Sales`.  
  
```sql  
USE Sales;  
GO  
CREATE PROCEDURE dbo.usp_Demo  
WITH EXECUTE AS 'CompanyDomain\SqlUser1'  
AS  
SELECT user_name();  
GO  
```  
  
## <a name="using-execute-as-caller-stand-alone-statement"></a>Utilisation de l'instruction autonome EXECUTE AS CALLER  
 Utilisez l'instruction autonome EXECUTE AS CALLER dans un module pour définir le contexte d'exécution de l'appelant du module.  
  
 Supposons que `SqlUser2` appelle la procédure stockée suivante.  
  
```sql  
CREATE PROCEDURE dbo.usp_Demo  
WITH EXECUTE AS 'SqlUser1'  
AS  
SELECT user_name(); -- Shows execution context is set to SqlUser1.  
EXECUTE AS CALLER;  
SELECT user_name(); -- Shows execution context is set to SqlUser2, the caller of the module.  
REVERT;  
SELECT user_name(); -- Shows execution context is set to SqlUser1.  
GO  
```  
  
## <a name="using-execute-as-to-define-custom-permission-sets"></a>Utilisation de la clause EXECUTE AS pour définir des ensembles d'autorisations personnalisées  
 La spécification d'un contexte d'exécution pour un module peut être très utile lorsque vous voulez définir des ensembles d'autorisations personnalisées. Par exemple, il n'est pas possible d'accorder des autorisations à certaines actions, telles que TRUNCATE TABLE. En incorporant l'instruction TRUNCATE TABLE dans un module et en spécifiant que le module s'exécute en tant qu'utilisateur qui a les autorisations nécessaires pour modifier la table, vous pouvez étendre les autorisations à l'utilisateur auquel vous accordez les autorisations EXECUTE sur le module.  
  
 Pour afficher la définition du module avec le contexte d’exécution spécifié, utilisez la vue de catalogue [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
## <a name="best-practice"></a>Bonne pratique  
 Spécifiez une connexion ou un utilisateur qui possède les privilèges minimum requis pour effectuer les opérations définies dans le module. Par exemple, ne spécifiez pas le compte d'un propriétaire de base de données à moins que ces autorisations soient requises.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter un module spécifié avec la clause EXECUTE AS, l'appelant doit avoir les autorisations EXECUTE sur le module.  
  
 Pour exécuter un module CLR spécifié avec la clause EXECUTE AS et qui a accès aux ressources d'une autre base de données ou serveur, la base de données ou le serveur cible doit faire confiance à l'authentificateur de la base de données d'origine du module (base de données source).  
  
 Pour spécifier la clause EXECUTE AS lorsque vous créez ou modifiez un module, vous devez avoir les autorisations IMPERSONATE sur le principal spécifié, ainsi que les autorisations de création du module. Vous pouvez toujours emprunter votre propre identité. Quand aucun contexte d’exécution n’est spécifié ou quand EXECUTE AS CALLER est spécifié, les autorisations IMPERSONATE ne sont pas nécessaires.  
  
 Pour spécifier un *login_name* ou *user_name* qui accède implicitement à la base de données par l’intermédiaire de l’appartenance à un groupe Windows, vous devez avoir les autorisations CONTROL sur la base de données.  
  
## <a name="examples"></a>Exemples  
 Le code exemple suivant crée une procédure stockée dans la base de [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] et affecte le contexte d'exécution à `OWNER`.  
  
```sql  
CREATE PROCEDURE HumanResources.uspEmployeesInDepartment   
@DeptValue int  
WITH EXECUTE AS OWNER  
AS  
    SET NOCOUNT ON;  
    SELECT e.BusinessEntityID, c.LastName, c.FirstName, e.JobTitle  
    FROM Person.Person AS c   
    INNER JOIN HumanResources.Employee AS e  
        ON c.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN HumanResources.EmployeeDepartmentHistory AS edh  
        ON e.BusinessEntityID = edh.BusinessEntityID  
    WHERE edh.DepartmentID = @DeptValue  
    ORDER BY c.LastName, c.FirstName;  
GO  
  
-- Execute the stored procedure by specifying department 5.  
EXECUTE HumanResources.uspEmployeesInDepartment 5;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.service_queues &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-service-queues-transact-sql.md)   
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)   
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)  
  
  
