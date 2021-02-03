---
description: ALTER SERVER ROLE (Transact-SQL)
title: ALTER SERVER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SERVER_ROLE_TSQL
- ALTER SERVER ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE, ALTER
- ALTER SERVER ROLE statement
ms.assetid: 7a4db7bb-c442-4e12-9a8a-114da5bc7710
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d0708ee4181ce959b1bc307d9fa29b3c7ae408c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182150"
---
# <a name="alter-server-role-transact-sql"></a>ALTER SERVER ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]

Modifie l'appartenance d'un rôle serveur ou modifie le nom d'un rôle serveur défini par l'utilisateur. Les rôles serveur fixes ne peuvent pas être renommés.  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server  
  
ALTER SERVER ROLE server_role_name   
{  
    [ ADD MEMBER server_principal ]  
  | [ DROP MEMBER server_principal ]  
  | [ WITH NAME = new_server_role_name ]  
} [ ; ]  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
ALTER SERVER ROLE  server_role_name  ADD MEMBER login;  
  
ALTER SERVER ROLE  server_role_name  DROP MEMBER login;  
```  
  
## <a name="arguments"></a>Arguments  
*server_role_name*  
Nom du rôle de serveur à modifier.  
  
ADD MEMBER *server_principal*  
Ajoute le principal de serveur spécifié au rôle de serveur. *server_principal* peut être un compte de connexion ou un rôle serveur défini par l’utilisateur. *server_principal* ne peut pas être un rôle serveur fixe, un rôle de base de données ou un administrateur système.  
  
DROP MEMBER *server_principal*  
Supprime le principal de serveur spécifié du rôle de serveur. *server_principal* peut être un compte de connexion ou un rôle serveur défini par l’utilisateur. *server_principal* ne peut pas être un rôle serveur fixe, un rôle de base de données ou un administrateur système.  
  
WITH NAME **=** _new_server_role_name_  
Spécifie le nouveau nom du rôle de serveur défini par l'utilisateur. Ce nom ne peut pas déjà exister dans le serveur.  
  
## <a name="remarks"></a>Notes  
La modification du nom d'un rôle de serveur défini par l'utilisateur ne modifie pas le numéro d'identification, le propriétaire ou les autorisations du rôle.  
  
Pour modifier l’appartenance au rôle, `ALTER SERVER ROLE` remplace sp_addsrvrolemember et sp_dropsrvrolemember. Ces procédures stockées sont déconseillées.  
  
Vous pouvez consulter des rôles de serveur en interrogeant les affichages catalogue `sys.server_role_members` et `sys.server_principals`.  
  
Pour modifier le propriétaire d’un rôle serveur défini par l’utilisateur, utilisez [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’autorisation `ALTER ANY SERVER ROLE` sur le serveur pour changer le nom d’un rôle serveur défini par l’utilisateur.  
  
**Rôles serveur fixes**  
  
Pour ajouter un membre à un rôle serveur fixe, vous devez être membre de ce rôle serveur fixe ou du rôle serveur fixe `sysadmin`.  
  
> [!NOTE]  
>  Les autorisations `CONTROL SERVER` et `ALTER ANY SERVER ROLE` ne sont pas suffisantes pour exécuter `ALTER SERVER ROLE` pour un rôle serveur fixe, et l’autorisation `ALTER` ne peut pas être accordée sur un rôle serveur fixe.  
  
**Rôles de serveur définis par l'utilisateur**  
  
Pour ajouter un membre à un rôle serveur défini par l’utilisateur, vous devez être membre du rôle serveur fixe `sysadmin`, ou disposer de l’autorisation`CONTROL SERVER` ou `ALTER ANY SERVER ROLE`. Sinon, vous devez disposer de l’autorisation `ALTER` sur ce rôle.  
  
> [!NOTE]  
>  Contrairement aux rôles serveur fixes, les membres d'un rôle de serveur défini par l'utilisateur n'ont pas intrinsèquement l'autorisation d'ajouter des membres à ce même rôle.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-name-of-a-server-role"></a>R. Modification du nom d'un rôle de serveur  
L'exemple suivant crée un rôle de serveur nommé `Product`, puis modifie le nom du rôle de serveur en `Production`.  
  
```sql
CREATE SERVER ROLE Product ;  
ALTER SERVER ROLE Product WITH NAME = Production ;  
GO  
```  
  
### <a name="b-adding-a-domain-account-to-a-server-role"></a>B. Ajout d'un compte de domaine à un rôle de serveur  
L’exemple suivant ajoute un compte de domaine nommé `adventure-works\roberto0` au rôle serveur défini par l’utilisateur nommé `Production`.  
  
```sql
ALTER SERVER ROLE Production ADD MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="c-adding-a-sql-server-login-to-a-server-role"></a>C. Ajout d'un compte de connexion SQL Server à un rôle de serveur  
L’exemple suivant ajoute un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nommé `Ted` au rôle serveur fixe `diskadmin`.  
  
```sql
ALTER SERVER ROLE diskadmin ADD MEMBER Ted ;  
GO  
```  
  
### <a name="d-removing-a-domain-account-from-a-server-role"></a>D. Suppression d'un compte de domaine d'un rôle de serveur  
L’exemple suivant supprime un compte de domaine nommé `adventure-works\roberto0` du rôle serveur défini par l’utilisateur nommé `Production`.  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="e-removing-a-sql-server-login-from-a-server-role"></a>E. Suppression d'un compte de connexion SQL Server d'un rôle de serveur  
L’exemple suivant supprime le compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`Ted` du rôle serveur fixe `diskadmin`.  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER Ted ;  
GO  
```  
  
### <a name="f-granting-a-login-the-permission-to-add-logins-to-a-user-defined-server-role"></a>F. Octroi à un compte de connexion de l'autorisation d'ajouter des comptes de connexion à un rôle de serveur défini par l'utilisateur  
L'exemple suivant permet à `Ted` d'ajouter d'autres comptes de connexion au rôle de serveur défini par l'utilisateur nommé `Production`.  
  
```sql
GRANT ALTER ON SERVER ROLE::Production TO Ted ;  
GO  
```  
  
### <a name="g-to-view-role-membership"></a>G. Pour consulter l'appartenance à un rôle  
Pour afficher l’appartenance à un rôle, utilisez la page **Rôle serveur (Membres)** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou exécutez la requête suivante :  
  
```sql
SELECT SRM.role_principal_id, SP.name AS Role_Name,   
SRM.member_principal_id, SP2.name  AS Member_Name  
FROM sys.server_role_members AS SRM  
JOIN sys.server_principals AS SP  
    ON SRM.Role_principal_id = SP.principal_id  
JOIN sys.server_principals AS SP2   
    ON SRM.member_principal_id = SP2.principal_id  
ORDER BY  SP.name,  SP2.name  
```  
  
## <a name="examples-sspdw"></a>Exemples : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="h-basic-syntax"></a>H. Syntaxe de base  
L’exemple suivant ajoute le compte de connexion `Anna` au rôle serveur `LargeRC`.  
  
```sql
ALTER SERVER ROLE LargeRC ADD MEMBER Anna;  
```  
  
### <a name="i-remove-a-login-from-a-resource-class"></a>I. Supprimer un compte de connexion d’une classe de ressources  
L’exemple suivant supprime l’appartenance d’Anna au rôle serveur `LargeRC`.  
  
```sql
ALTER SERVER ROLE LargeRC DROP MEMBER Anna;  
```  
  
## <a name="see-also"></a>Voir aussi  
[CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md)   
[DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
[CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
[ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
[DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
[Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
[Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
[Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
[sys.server_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)   
[sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
