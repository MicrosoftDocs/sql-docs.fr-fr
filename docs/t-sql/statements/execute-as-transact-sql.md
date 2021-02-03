---
description: EXECUTE AS (Transact-SQL)
title: EXECUTE AS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EXECUTE AS
- EXECUTE_AS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REVERT statement
- WITH NO REVERT clause
- sessions [SQL Server], execution context
- EXECUTE AS
- execution context [SQL Server]
- switching execution context
ms.assetid: 613b8271-7f7d-4378-b7a2-5a7698551dbd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: 4a39e2cd219050ea0d279a5f67bfb7bb69c0238c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193649"
---
# <a name="execute-as-transact-sql"></a>EXECUTE AS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Définit le contexte d'exécution d'une session.  
  
 Par défaut, une session commence lorsqu'un utilisateur se connecte et se termine lorsqu'il se déconnecte. Au cours d'une session, toutes les opérations sont soumises à des vérifications d'autorisations pour cet utilisateur. Quand une instruction **EXECUTE AS** est exécutée, le contexte d’exécution de la session change vers le nom de connexion ou d’utilisateur spécifié. Suite au changement de contexte, les autorisations sont vérifiées par rapport aux jetons de sécurité de connexion et d’utilisateur relatifs à ce compte au lieu de la personne qui appelle l’instruction **EXECUTE AS**. L'identité du compte d'utilisateur ou de connexion est naturellement empruntée pour toute la durée de la session ou de l'exécution du module, ou le changement de contexte est explicitement annulé.  
  

  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
{ EXEC | EXECUTE } AS <context_specification>  
[;]  
  
<context_specification>::=  
{ LOGIN | USER } = 'name'  
    [ WITH { NO REVERT | COOKIE INTO @varbinary_variable } ]   
| CALLER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 Connexion  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Spécifie que le contexte d'exécution dont l'identité est empruntée est une connexion. L'étendue de l'emprunt d'identité est au niveau du serveur.  
  
> [!NOTE]  
>  Cette option n’est pas disponible dans une base de données autonome ou SQL Database ou [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
 Utilisateur  
 Spécifie que le contexte dont l'identité doit être empruntée est un utilisateur de la base de données active. L'étendue de l'emprunt d'identité est limitée à la base de données active. Le changement de contexte vers un utilisateur de base de données n'hérite pas des autorisations de cet utilisateur au niveau serveur.  
  
> [!IMPORTANT]  
>  Tant que le changement de contexte en faveur d'un utilisateur de base de données est en vigueur, toute tentative d'accès à des ressources situées en dehors de la base de données causera l'échec de l'instruction. Cela inclut les instructions USE *database*, les requêtes distribuées et les requêtes qui référencent une autre base de données utilisant des identificateurs en trois ou quatre parties.  
  
 '*name*' est un nom d'utilisateur ou de connexion valide. *name* doit être membre du rôle serveur fixe **sysadmin**, ou exister comme principal respectivement dans [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) ou [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 *name* peut être spécifié sous forme de variable locale.  
  
 *name* doit être un compte singleton. Ce ne peut pas être un groupe, un rôle, un certificat, une clé ni un compte intégré, tel que NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService ou NT AUTHORITY\LocalSystem.  
  
 Pour plus d’informations, consultez [Spécification d’un nom d’utilisateur ou d’un ID de connexion](#_user), plus loin dans cette rubrique.  
  
 NO REVERT  
 Spécifie qu'il n'est pas possible de restaurer le changement de contexte pour revenir au contexte précédent. L’option **NO REVERT** peut uniquement être utilisée au niveau adhoc.  
  
 Pour plus d’informations sur la restauration du contexte précédent, consultez [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md).  
  
 COOKIE INTO @*varbinary_variable*  
 Spécifie que le contexte d’exécution peut être restauré vers le contexte précédent uniquement si l’instruction REVERT WITH COOKIE appelante contient la valeur @*varbinary_variable* correcte. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] passe le cookie à @*varbinary_variable*. L’option **COOKIE INTO** peut uniquement être utilisée au niveau adhoc.  
  
 @*varbinary_variable* est **varbinary(8000)** .  
  
> [!NOTE]  
>  Le paramètre **OUTPUT** de cookie est actuellement documenté comme **varbinary(8000)** , ce qui correspond à la longueur maximale correcte. Cependant, l’implémentation actuelle retourne **varbinary(100)** . Les applications doivent réserver **varbinary(8000)** pour continuer à fonctionner correctement si la taille de retour des cookies augmente dans une version ultérieure.  
  
 CALLER  
 Dans un module, il spécifie que les instructions de ce module sont exécutées dans le contexte de l'appelant du module.
En dehors d'un module, cette instruction n'a pas d'effet.
 > [!NOTE]  
>  Cette option n'est pas disponible dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
## <a name="remarks"></a>Notes  
 Le changement dans le contexte d'exécution reste en vigueur jusqu'à ce que se produise l'une des actions suivantes :  
  
-   Une autre instruction EXECUTE AS s'exécute.  
  
-   Une instruction REVERT s'exécute.  
  
-   Suppression de la session.  
  
-   La procédure stockée ou le déclencheur où la commande exécutée se termine.  
  
Vous pouvez créer une pile de contextes d'exécution en appelant l'instruction EXECUTE AS plusieurs fois sur plusieurs principaux. Lorsqu'elle est appelée, l'instruction REVERT bascule le contexte vers la connexion ou l'utilisateur du niveau supérieur dans la pile de contexte. Pour avoir une démonstration de ce comportement, consultez [Exemple A](#_exampleA).  
  
##  <a name="specifying-a-user-or-login-name"></a><a name="_user"></a> Spécification d’un nom d’utilisateur ou d’un ID de connexion  
 Le nom d’utilisateur ou de connexion spécifié dans EXECUTE AS \<context_specification> doit exister comme principal respectivement dans **sys.database_principals** ou **sys.server_principals**, à défaut de quoi, l’instruction EXECUTE AS échoue. De plus, les autorisations IMPERSONATE doivent être accordées sur le principal. Sauf si l’appelant est le propriétaire de la base de données, ou un membre du rôle serveur fixe **sysadmin**, le principal doit exister même quand l’utilisateur a accès à la base de données ou l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du fait de son appartenance à un groupe Windows. Par exemple, supposons les conditions suivantes : 
  
-   Le groupe **CompanyDomain\SQLUsers** a accès à la base de données **Sales**.  
  
-   **CompanyDomain\SqlUser1** est membre de **SQLUsers** et bénéficie donc d’un accès implicite à la base de données **Sales**.  
  
 **CompanyDomain\SqlUser1** a accès à la base de données du fait de son appartenance au groupe **SQLUsers**, mais l’instruction `EXECUTE AS USER = 'CompanyDomain\SqlUser1'` échoue parce que `CompanyDomain\SqlUser1` n’existe pas comme principal dans la base de données.  
  
Si l’utilisateur est orphelin (la connexion associée n’existant plus) et qu’il n’a pas été créé avec la clause **WITHOUT LOGIN**, l’instruction **EXECUTE AS** échoue pour cet utilisateur.  
  
## <a name="best-practice"></a>Bonne pratique  
 Spécifiez une connexion ou un utilisateur bénéficiant du minimum de privilèges indispensable pour effectuer les opérations dans la session. Par exemple, ne spécifiez pas de nom de connexion avec des autorisations de niveau serveur, si seules les autorisations de niveau base de données sont requises ; ne spécifiez pas non plus un compte de propriétaire de base de données à moins que ces autorisations ne soient nécessaires.  
  
> [!CAUTION]  
>  L'instruction EXECUTE AS peut réussir tant que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut résoudre le nom. Si un utilisateur de domaine existe, Windows peut être en mesure de résoudre l'utilisateur pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)], même si l'utilisateur windows n'a pas accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cela peut entraîner une condition selon laquelle une connexion sans accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] semble être connectée, alors que la connexion avec emprunt d'identité dispose uniquement des autorisations accordées au public ou à l'invité.  
  
## <a name="using-with-no-revert"></a>Utilisation de WITH NO REVERT  
 Lorsque l'instruction EXECUTE AS comporte la clause WITH NO REVERT facultative, le contexte d'exécution d'une session ne peut pas être réinitialisé à l'aide de REVERT ou en exécutant une autre instruction EXECUTE AS. Le contexte défini par l'instruction reste en vigueur jusqu'à ce que la session soit supprimée.  
  
 Quand la clause WITH NO REVERT COOKIE = @*varbinary_variable* est spécifiée, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] passe la valeur de cookie à @*varbinary_variable*. Le contexte d’exécution défini par cette instruction ne peut être restauré vers le contexte précédent que si l’instruction REVERT WITH COOKIE = @*varbinary_variable* appelante contient la même valeur *\@varbinary_variable*.  
  
 Cette option est utile dans un environnement où le groupement de connexions est utilisé. Le groupement de connexions est la maintenance d'un groupe de connexions de base de données à réutiliser par des applications sur un serveur d'applications. Comme la valeur passée à *\@varbinary_variable* n’est connue que de l’appelant de l’instruction EXECUTE AS, celui-ci peut garantir que le contexte d’exécution qu’il établit ne sera pas modifié par un autre utilisateur.  
  
## <a name="determining-the-original-login"></a>Identification de la connexion originale  
 Utilisez la fonction [ORIGINAL_LOGIN](../../t-sql/functions/original-login-transact-sql.md) pour retourner le nom de la connexion utilisée pour se connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez utiliser cette fonction pour renvoyer l'identité de la connexion d'origine dans les sessions où il y a un grand nombre de changements de contexte implicites ou explicites.  
  
## <a name="permissions"></a>Autorisations  
 Pour spécifier **EXECUTE AS** sur une connexion, l’appelant doit avoir l’autorisation **IMPERSONATE** sur le nom de connexion spécifié et l’autorisation **IMPERSONATE ANY LOGIN** ne doit pas lui être refusée. Pour spécifier **EXECUTE AS** sur un utilisateur de base de données, l’appelant doit avoir les autorisations **IMPERSONATE** sur le nom d’utilisateur spécifié. Quand **EXECUTE AS CALLER** est spécifié, les autorisations **IMPERSONATE** ne sont pas nécessaires.  
  
## <a name="examples"></a>Exemples  
  
###  <a name="a-using-execute-as-and-revert-to-switch-context"></a><a name="_exampleA"></a> A. Utilisation de EXECUTE AS et REVERT pour changer de contexte  
 L'exemple suivant crée une pile d'exécutions de contexte à l'aide de plusieurs principaux. L'instruction `REVERT` est ensuite utilisée pour réinitialiser le contexte d'exécution à l'appelant précédent. L'instruction `REVERT` est exécutée plusieurs fois en remontant la pile jusqu'à ce que le contexte d'exécution soit défini pour l'appelant d'origine.  
  

```sql
USE AdventureWorks2012;  
GO  
--Create two temporary principals  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE LOGIN login2 WITH PASSWORD = 'Uor80$23b';  
GO  
CREATE USER user1 FOR LOGIN login1;  
CREATE USER user2 FOR LOGIN login2;  
GO  
--Give IMPERSONATE permissions on user2 to user1  
--so that user1 can successfully set the execution context to user2.  
GRANT IMPERSONATE ON USER:: user2 TO user1;  
GO  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- Set the execution context to login1.   
EXECUTE AS LOGIN = 'login1';  
--Verify the execution context is now login1.  
SELECT SUSER_NAME(), USER_NAME();  
--Login1 sets the execution context to login2.  
EXECUTE AS USER = 'user2';  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- The execution context stack now has three principals: the originating caller, login1 and login2.  
--The following REVERT statements will reset the execution context to the previous context.  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
  
--Remove temporary principals.  
DROP LOGIN login1;  
DROP LOGIN login2;  
DROP USER user1;  
DROP USER user2;  
GO  
```  
  
### <a name="b-using-the-with-cookie-clause"></a>B. Utilisation de la clause WITH COOKIE  
 L’exemple suivant définit le contexte d’exécution d’une session sur un utilisateur spécifié et précise la clause WITH NO REVERT COOKIE = @*varbinary_variable*. L'instruction `REVERT` doit spécifier la valeur passée à la variable `@cookie` dans l'instruction `EXECUTE AS` pour ramener le contexte à l'appelant. Pour exécuter cet exemple, la connexion `login1` et l'utilisateur `user1` créés dans l'exemple A doivent exister.  
  

```sql 
DECLARE @cookie VARBINARY(8000);  
EXECUTE AS USER = 'user1' WITH COOKIE INTO @cookie;  
-- Store the cookie in a safe location in your application.  
-- Verify the context switch.  
SELECT SUSER_NAME(), USER_NAME();  
--Display the cookie value.  
SELECT @cookie;  
GO  
-- Use the cookie in the REVERT statement.  
DECLARE @cookie VARBINARY(8000);  
-- Set the cookie value to the one from the SELECT @cookie statement.  
SET @cookie = <value from the SELECT @cookie statement>;  
REVERT WITH COOKIE = @cookie;  
-- Verify the context switch reverted.  
SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)   
 [Clause EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md)  
  
  

