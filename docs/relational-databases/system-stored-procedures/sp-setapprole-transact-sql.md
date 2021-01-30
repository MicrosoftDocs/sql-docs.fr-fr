---
description: sp_setapprole (Transact-SQL)
title: sp_setapprole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/12/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_setapprole
- sp_setapprole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_setapprole
ms.assetid: cf0901c0-5f90-42d4-9d5b-8772c904062d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f34be78bc4d3f47ff2a11d0a6ed09c22c3670dca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209275"
---
# <a name="sp_setapprole-transact-sql"></a>sp_setapprole (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Active les autorisations associées à un rôle d'application dans la base de données active.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  

```sql
sp_setapprole [ @rolename = ] 'role',  
    [ @password = ] { encrypt N'password' }
      |  
        'password' [ , [ @encrypt = ] { 'none' | 'odbc' } ]  
        [ , [ @fCreateCookie = ] true | false ]  
    [ , [ @cookie = ] @cookie OUTPUT ]  
```

## <a name="arguments"></a>Arguments

`[ @rolename = ] 'role'` Nom du rôle d’application défini dans la base de données active. *role* est de **type sysname**, sans valeur par défaut. le *rôle* doit exister dans la base de données actuelle.  
  
`[ @password = ] { encrypt N'password' }` Mot de passe requis pour activer le rôle d’application. *Password* est de **type sysname**, sans valeur par défaut. le *mot de passe* peut être obscurci à l’aide de la fonction ODBC **Encrypt** . Lorsque vous utilisez la fonction **Encrypt** , le mot de passe doit être converti en chaîne Unicode en plaçant **N** avant le premier guillemet.  
  
 L’option encrypt n’est pas prise en charge sur les connexions qui utilisent **SqlClient**.  
  
> [!IMPORTANT]  
> La fonction **Encrypt** de ODBC ne fournit pas de chiffrement. Vous ne devez pas compter sur cette fonction pour protéger des mots de passe transmis sur un réseau. Si ces informations sont transmises sur un réseau, utilisez TLS ou IPSec.
  
 **@encrypt = 'none'**  
 Indique qu'aucun codage ne doit être utilisé. Le mot de passe est transmis à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sous forme de texte brut. Il s’agit de la valeur par défaut.  
  
 **@encrypt= 'ODBC'**  
 Spécifie que le mot de passe sera obscurci par ODBC à l’aide de la fonction ODBC **Encrypt** avant d’envoyer le mot de passe au [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Ceci peut être spécifié seulement si vous utilisez un client ODBC ou le fournisseur OLE DB pour SQL Server.  
  
`[ @fCreateCookie = ] true | false` Spécifie si un cookie doit être créé. **true** est implicitement converti en 1. la **valeur false** est implicitement convertie en 0.  
  
`[ @cookie = ] @cookie OUTPUT` Spécifie un paramètre de sortie pour contenir le cookie. Le cookie est généré uniquement si la valeur de **\@ fCreateCookie** est **true**. **varbinary(8000)**  
  
> [!NOTE]  
> Le paramètre **OUTPUT** de cookie pour **sp_setapprole** est actuellement documenté comme **varbinary(8000)** , ce qui correspond à la longueur maximale correcte. Cependant, l’implémentation actuelle retourne **varbinary(50)** . Les applications doivent continuer à réserver **varbinary (8000)** afin que l’application continue à fonctionner correctement si la taille de retour du cookie augmente dans une version ultérieure.
  
## <a name="return-code-values"></a>Codet de retour

 0 (succès) et 1 (échec)  
  
## <a name="remarks"></a>Notes

 Une fois qu’un rôle d’application est activé à l’aide de **sp_setapprole**, le rôle reste actif jusqu’à ce que l’utilisateur se déconnecte du serveur ou exécute **sp_unsetapprole**. **sp_setapprole** peut être exécutée uniquement par [!INCLUDE[tsql](../../includes/tsql-md.md)] les instructions directes, au niveau ad hoc et non dans une autre procédure stockée, déclencheur ou dans une transaction définie par l’utilisateur.  
  
 Pour obtenir une vue d’ensemble des rôles d’application, consultez [rôles d’application](../../relational-databases/security/authentication-access/application-roles.md).  
  
> [!IMPORTANT]  
> Pour protéger le mot de passe du rôle d’application lorsqu’il est transmis sur un réseau, vous devez toujours utiliser une connexion chiffrée lors de l’activation d’un rôle d’application.
> L' [!INCLUDE[msCoName](../../includes/msconame-md.md)] option de **chiffrement** ODBC n’est pas prise en charge par **SqlClient**. Si vous devez stocker des informations d'identification, chiffrez-les à l'aide des fonctions API de chiffrement. Le *mot de passe* du paramètre est stocké sous la forme d’un hachage unidirectionnel. Pour préserver la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la stratégie de complexité de mot de passe n’est pas appliquée par **sp_addapprole**. Pour appliquer la stratégie de complexité du mot de passe, utilisez [créer un rôle d’application](../../t-sql/statements/create-application-role-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations

Requiert l’appartenance au **public** et la connaissance du mot de passe pour le rôle.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-activating-an-application-role-without-the-encrypt-option"></a>R. Activation d'un rôle d'application sans l'option encrypt

 Dans l'exemple ci-dessous, un rôle d'application nommé `SalesAppRole` est activé avec le mot de passe en texte brut `AsDeF00MbXX`, créé avec les autorisations spécialement conçues pour l'application utilisée par l'utilisateur en cours.

```sql
EXEC sys.sp_setapprole 'SalesApprole', 'AsDeF00MbXX';  
GO
```

### <a name="b-activating-an-application-role-with-a-cookie-and-then-reverting-to-the-original-context"></a>B. Activation d'un rôle d'application avec un cookie, puis retour au contexte d'origine

 Dans l'exemple ci-dessous, le rôle d'application `Sales11` est activé avec le mot de passe `fdsd896#gfdbfdkjgh700mM` et un cookie est créé. L'exemple retourne le nom de l'utilisateur actuel, puis revient au contexte d'origine en exécutant `sp_unsetapprole`.  

```sql
DECLARE @cookie varbinary(8000);  
EXEC sys.sp_setapprole 'Sales11', 'fdsd896#gfdbfdkjgh700mM'  
    , @fCreateCookie = true, @cookie = @cookie OUTPUT;  
-- The application role is now active.  
SELECT USER_NAME();  
-- This will return the name of the application role, Sales11.  
EXEC sys.sp_unsetapprole @cookie;  
-- The application role is no longer active.  
-- The original context has now been restored.  
GO  
SELECT USER_NAME();  
-- This will return the name of the original user.
GO
```

## <a name="see-also"></a>Voir aussi

 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [sp_unsetapprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unsetapprole-transact-sql.md)
