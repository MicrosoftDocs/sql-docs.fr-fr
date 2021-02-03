---
description: ALTER LOGIN (Transact-SQL)
title: ALTER LOGIN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_LOGIN_TSQL
- ALTER LOGIN
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER LOGIN statement
- change password
- mapping logins [SQL Server]
- logins [SQL Server], modifying
- passwords [SQL Server], modifying
- names [SQL Server], logins
- modifying login accounts
ms.assetid: e247b84e-c99e-4af8-8b50-57586e1cb1c5
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6e15a0c6e1f0e52f530f6c0178f8c6c55cd3d650
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204131"
---
# <a name="alter-login-transact-sql"></a>ALTER LOGIN (Transact-SQL)

Modifie les propriétés d'un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Base de données SQL](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for SQL Server

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    | <cryptographic_credential_option>
    }
[;]

<status_option> ::=
        ENABLE | DISABLE

<set_option> ::=
    PASSWORD = 'password' | hashed_password HASHED
    [
      OLD_PASSWORD = 'oldpassword'
      | <password_option> [<password_option> ]
    ]
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }
    | CREDENTIAL = credential_name
    | NO CREDENTIAL

<password_option> ::=
    MUST_CHANGE | UNLOCK
  
<cryptographic_credentials_option> ::=
    ADD CREDENTIAL credential_name
  | DROP CREDENTIAL credential_name
```

## <a name="arguments"></a>Arguments

*login_name* spécifie le nom de la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cours de modification. Les connexions de domaine doivent être placées entre crochets au format [domaine\utilisateur].

ENABLE | DISABLE active ou désactive cette connexion. La désactivation d'une connexion n'a aucune incidence sur le comportement des connexions qui sont déjà en cours. (Utilisez l’instruction `KILL` pour mettre fin à des connexions existantes.) Les connexions désactivées conservent leurs autorisations et peuvent toujours être usurpées.

PASSWORD **='** _password_ **'** s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie le mot de passe de la connexion en cours de modification. Les mots de passe respectent la casse.

PASSWORD **=** _hashed\_password_ S’applique uniquement au mot clé HASHED. Spécifie la valeur hachée du mot de passe de la connexion créée.

> [!IMPORTANT]
> Lorsqu'un compte de connexion (ou un utilisateur de base de données autonome) se connecte et est authentifié, la connexion met en cache les informations d'identité sur la connexion. Dans le cas d'une connexion d'authentification Windows, ces informations incluent des données sur l'appartenance aux groupes Windows. L'identité de la connexion reste authentifiée tant que la connexion est conservée. Pour imposer des modifications d'identité, une réinitialisation du mot de passe, par exemple, ou la modification de l'appartenance au groupe Windows, le compte de connexion doit fermer une session de l'autorité d'authentification (Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) et ouvrir une nouvelle session. Un membre du rôle serveur fixe **sysadmin** ou tout compte de connexion doté de l’autorisation **ALTER ANY CONNECTION** peut utiliser la commande **KILL** pour mettre fin à une connexion et obliger le compte de connexion à se reconnecter. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] peut réutiliser les informations de connexion lors de l’ouverture de plusieurs connexions dans les fenêtres de l’Explorateur d’objets et de l’éditeur de requête. Fermez toutes les connexions pour imposer une reconnexion.

HASHED s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie que le mot de passe entré après l'argument PASSWORD est déjà haché. Si cette option n'est pas sélectionnée, le mot de passe est haché avant d'être stocké dans la base de données. Cette option doit être utilisée uniquement pour la synchronisation de connexion entre deux serveurs. N'utilisez pas l'option HASHED pour modifier des mots de passe de manière régulière.

OLD_PASSWORD **='** _oldpassword_ **'** s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Mot de passe actuel de la connexion à laquelle un nouveau mot de passe doit être attribué. Les mots de passe respectent la casse.

MUST_CHANGE s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous incluez cette option, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] demande un mot de passe actualisé lors de la première utilisation de la connexion modifiée.

DEFAULT_DATABASE **=** _database_ spécifie une base de données par défaut à affecter à la connexion.

DEFAULT_LANGUAGE **=** _language_ spécifie une langue par défaut à affecter à la connexion. La langue par défaut pour toutes les connexions SQL Database est l’anglais. Ceci ne peut pas être modifié. La langue par défaut de la connexion `sa` sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Linux est l’anglais, mais vous pouvez la modifier.

NAME = *login_name* correspond au nouveau nom de la connexion à renommer. S'il s'agit d'une connexion Windows, le SID du principal Windows correspondant au nouveau nom doit correspondre au SID de la connexion dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le nouveau nom d’une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas contenir une barre oblique inverse (\\).

CHECK_EXPIRATION = { ON | **OFF** } s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie si les règles d'expiration des mots de passe doivent être imposées sur cette connexion. La valeur par défaut est OFF.

CHECK_POLICY **=** { **ON** | OFF } s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie que les stratégies de mot de passe Windows de l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute doivent être imposées sur cette connexion. La valeur par défaut est ON.

CREDENTIAL = *credential_name* correspond au nom des informations d’identification à mapper sur une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les informations d'identification doivent déjà exister sur le serveur. Pour plus d’informations, consultez [Informations d’identification](../../relational-databases/security/authentication-access/credentials-database-engine.md). Les informations d’identification ne peuvent pas être mappées à la connexion sa.

NO CREDENTIAL supprime tout mappage existant de la connexion sur des informations d’identification du serveur. Pour plus d’informations, consultez [Informations d’identification](../../relational-databases/security/authentication-access/credentials-database-engine.md).

UNLOCK s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie qu'une connexion verrouillée doit être déverrouillée.

ADD CREDENTIAL ajoute des informations d’identification du fournisseur EKM (Extensible Key Management) à la connexion. Pour plus d’informations, consultez [Gestion de clés extensible (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md).

DROP CREDENTIAL supprime des informations d’identification du fournisseur EKM (Extensible Key Management) de la connexion. Pour plus d’informations, consultez [Gestion de clés extensible (EKM)] (../.. /relational-databases/security/encryption/extensible-key-management-ekm.md).

## <a name="remarks"></a>Notes

Lorsque CHECK_POLICY a la valeur ON, l'argument HASHED ne peut pas être utilisé.

Lorsque CHECK_POLICY prend la valeur ON, le comportement suivant a lieu :

- L'historique du mot de passe est initialisé avec la valeur du hachage de mot de passe actuel.

  Lorsque CHECK_POLICY prend la valeur OFF, le comportement suivant a lieu :

- CHECK_EXPIRATION ON prend également la valeur OFF.
- L'historique du mot de passe est supprimé.
- La valeur de *lockout_time* est réinitialisée.

Si MUST_CHANGE est spécifié, CHECK_EXPIRATION et CHECK_POLICY doivent prendre la valeur ON. Sans quoi, l'instruction échoue.

Si CHECK_POLICY prend la valeur OFF, CHECK_EXPIRATION ne peut pas prendre la valeur ON. Si cette combinaison d'options est utilisée dans une instruction ALTER LOGIN, l'instruction échoue.

Vous ne pouvez pas utiliser ALTER LOGIN avec l’argument DISABLE pour refuser l’accès à un groupe Windows. Par exemple, ALTERLOGIN [*domaine\groupe*] DISABLE retourne le message d’erreur suivant :

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

C'est la procédure normale.
  
Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], les données de connexion exigées pour authentifier une connexion et les règles de pare-feu de niveau serveur sont temporairement mises en cache dans chaque base de données. Ce cache est régulièrement actualisé. Pour forcer une actualisation du cache d’authentification et garantir qu’une base de données a la version la plus récente de la table de connexions, exécutez [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Autorisations

Nécessite l'autorisation ALTER ANY LOGIN.

Si l'option CREDENTIAL est utilisée, exige également l'autorisation ALTER ANY CREDENTIAL.

Si la connexion en cours de modification est membre du rôle serveur fixe **sysadmin** ou détient l’autorisation CONTROL SERVER, elle exige également l’autorisation CONTROL SERVER lors des modifications suivantes :

- réinitialisation du mot de passe sans fournir l'ancien mot de passe ;
- activation de MUST_CHANGE, CHECK_POLICY ou CHECK_EXPIRATION ;
- modification du nom de la connexion ;
- activation ou désactivation de la connexion ;
- mappage de la connexion sur une autre information d'identification.

Un principal peut modifier le mot de passe, la langue et la base de données par défaut de sa propre connexion.

## <a name="examples"></a>Exemples

### <a name="a-enabling-a-disabled-login"></a>R. Activation d'une connexion désactivée

L'exemple suivant active la connexion `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. Modification du mot de passe d'une connexion

L'exemple suivant remplace le mot de passe de la connexion `Mary5` par un mot de passe fort.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-password-of-a-login-when-logged-in-as-the-login"></a>C. Modification du mot de passe d’une session à laquelle vous êtes connecté

Si vous essayez de modifier le mot de passe de la session à laquelle vous êtes actuellement connecté et que vous n’avez pas l’autorisation `ALTER ANY LOGIN`, vous devez spécifier l’option `OLD_PASSWORD`.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>' OLD_PASSWORD = '<oldWeakPasswordHere>';
```

### <a name="d-changing-the-name-of-a-login"></a>D. Modification du nom d'une connexion

L'exemple suivant remplace le nom de connexion `Mary5` par `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="e-mapping-a-login-to-a-credential"></a>E. Mappage d'une connexion sur des informations d'identification

L'exemple suivant mappe la connexion `John2` aux informations d'identification `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="f-mapping-a-login-to-an-extensible-key-management-credential"></a>F. Mappage d'une connexion à des informations d'identification de gestion de clés extensible

L'exemple suivant mappe la connexion `Mary5` aux informations d'identification EKM `EKMProvider1`.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. Déverrouillage d'une connexion

Pour déverrouiller une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], exécutez l'instruction suivante, en remplaçant \*\*\*\* par le mot de passe de compte souhaité.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Pour déverrouiller une connexion sans modifier le mot de passe, désactivez la stratégie de contrôle, puis réactivez-la.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. Modification du mot de passe d'une connexion à l'aide de HASHED

L'exemple suivant modifie le mot de passe de la connexion `TestUser` en une valeur déjà hachée.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>Voir aussi

- [Informations d'identification](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Gestion de clés extensible (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Database \*_**
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL Database

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for Azure SQL Database

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE

<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
    ]
    | NAME = login_name
```

## <a name="arguments"></a>Arguments

*login_name* spécifie le nom de la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cours de modification. Les connexions de domaine doivent être placées entre crochets au format [domaine\utilisateur].

ENABLE | DISABLE active ou désactive cette connexion. La désactivation d'une connexion n'a aucune incidence sur le comportement des connexions qui sont déjà en cours. (Utilisez l’instruction `KILL` pour mettre fin à des connexions existantes.) Les connexions désactivées conservent leurs autorisations et peuvent toujours être usurpées.

PASSWORD **='** _password_ **'** s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie le mot de passe de la connexion en cours de modification. Les mots de passe respectent la casse.

Des connexions actives en permanence à SQL Database nécessitent une réautorisation (effectuée par le moteur de base de données) au moins toutes les 10 heures. Le moteur de base de données tente une réautorisation en utilisant le mot de passe envoyé à l’origine et aucune entrée utilisateur n’est nécessaire. Pour des raisons de performances, quand un mot de passe est réinitialisé dans SQL Database, la connexion n’est pas authentifiée de nouveau, même si elle est réinitialisée suite à un regroupement de connexions. Cela est différent du comportement local de SQL Server. Si le mot de passe a été modifié depuis que la connexion a été initialement autorisée, la connexion doit être interrompue et une nouvelle connexion établie à l’aide du nouveau mot de passe. Un utilisateur avec l’autorisation KILL DATABASE CONNECTION peut mettre fin explicitement à une connexion à SQL Database à l’aide de la commande KILL. Pour plus d’informations, consultez [KILL](../../t-sql/language-elements/kill-transact-sql.md).

> [!IMPORTANT]
> Lorsqu'un compte de connexion (ou un utilisateur de base de données autonome) se connecte et est authentifié, la connexion met en cache les informations d'identité sur la connexion. Dans le cas d'une connexion d'authentification Windows, ces informations incluent des données sur l'appartenance aux groupes Windows. L'identité de la connexion reste authentifiée tant que la connexion est conservée. Pour imposer des modifications d'identité, une réinitialisation du mot de passe, par exemple, ou la modification de l'appartenance au groupe Windows, le compte de connexion doit fermer une session de l'autorité d'authentification (Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) et ouvrir une nouvelle session. Un membre du rôle serveur fixe **sysadmin** ou tout compte de connexion doté de l’autorisation **ALTER ANY CONNECTION** peut utiliser la commande **KILL** pour mettre fin à une connexion et obliger le compte de connexion à se reconnecter. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] peut réutiliser les informations de connexion lors de l’ouverture de plusieurs connexions dans les fenêtres de l’Explorateur d’objets et de l’éditeur de requête. Fermez toutes les connexions pour imposer une reconnexion.

OLD_PASSWORD **='** _oldpassword_ **'** s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Mot de passe actuel de la connexion à laquelle un nouveau mot de passe doit être attribué. Les mots de passe respectent la casse.

NAME = *login_name* correspond au nouveau nom de la connexion à renommer. S'il s'agit d'une connexion Windows, le SID du principal Windows correspondant au nouveau nom doit correspondre au SID de la connexion dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le nouveau nom d’une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas contenir une barre oblique inverse (\\).

## <a name="remarks"></a>Notes

Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], les données de connexion exigées pour authentifier une connexion et les règles de pare-feu de niveau serveur sont temporairement mises en cache dans chaque base de données. Ce cache est régulièrement actualisé. Pour forcer une actualisation du cache d’authentification et garantir qu’une base de données a la version la plus récente de la table de connexions, exécutez [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Autorisations

Nécessite l'autorisation ALTER ANY LOGIN.

Si la connexion en cours de modification est membre du rôle serveur fixe **sysadmin** ou détient l’autorisation CONTROL SERVER, elle exige également l’autorisation CONTROL SERVER lors des modifications suivantes :

- réinitialisation du mot de passe sans fournir l'ancien mot de passe ;
- modification du nom de la connexion ;
- activation ou désactivation de la connexion ;
- mappage de la connexion sur une autre information d'identification.

Un principal peut modifier le mot de passe pour sa propre connexion.

## <a name="examples"></a>Exemples

Ces exemples incluent également des exemples d’utilisation d’autres produits SQL. Vérifiez les arguments pris en charge ci-dessus.

### <a name="a-enabling-a-disabled-login"></a>R. Activation d'une connexion désactivée

L'exemple suivant active la connexion `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. Modification du mot de passe d'une connexion

L'exemple suivant remplace le mot de passe de la connexion `Mary5` par un mot de passe fort.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. Modification du nom d'une connexion

L'exemple suivant remplace le nom de connexion `Mary5` par `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. Mappage d'une connexion sur des informations d'identification

L'exemple suivant mappe la connexion `John2` aux informations d'identification `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. Mappage d'une connexion à des informations d'identification de gestion de clés extensible

L'exemple suivant mappe la connexion `Mary5` aux informations d'identification EKM `EKMProvider1`.


**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. Déverrouillage d'une connexion

Pour déverrouiller une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], exécutez l'instruction suivante, en remplaçant \*\*\*\* par le mot de passe de compte souhaité.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Pour déverrouiller une connexion sans modifier le mot de passe, désactivez la stratégie de contrôle, puis réactivez-la.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. Modification du mot de passe d'une connexion à l'aide de HASHED

L'exemple suivant modifie le mot de passe de la connexion `TestUser` en une valeur déjà hachée.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>Voir aussi

- [Informations d'identification](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Gestion de clés extensible (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for SQL Server and Azure SQL Managed Instance

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    | <cryptographic_credential_option>
    }
[;]

<status_option> ::=
        ENABLE | DISABLE
  
<set_option> ::=
    PASSWORD = 'password' | hashed_password HASHED
    [
      OLD_PASSWORD = 'oldpassword'
      | <password_option> [<password_option> ]
    ]
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }
    | CREDENTIAL = credential_name
    | NO CREDENTIAL

<password_option> ::=
    MUST_CHANGE | UNLOCK

<cryptographic_credentials_option> ::=
    ADD CREDENTIAL credential_name
  | DROP CREDENTIAL credential_name
```

> [!NOTE]
> L’administrateur Azure AD de la fonctionnalité d’instance qui a été gérée après la création a changé. Pour plus d’informations, consultez [Nouvelle fonctionnalité d’administration Azure AD pour MI](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

```syntaxsql
-- Syntax for Azure SQL Managed Instance using Azure AD logins

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE

<set_option> ::=
     DEFAULT_DATABASE = database
   | DEFAULT_LANGUAGE = language
```

## <a name="arguments"></a>Arguments

### <a name="arguments-applicable-to-sql-and-azure-ad-logins"></a>Arguments applicables aux connexions SQL et Azure AD

*login_name* spécifie le nom de la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cours de modification. Les connexions AD Azure doivent être spécifiées au format user@domain. Par exemple, john.smith@contoso.com, ou sous forme de groupe ou de nom d’application Azure AD. Pour les connexions Azure AD, *login_name* doit correspondre à une connexion Azure AD existante créée dans la base de données master.

ENABLE | DISABLE active ou désactive cette connexion. La désactivation d'une connexion n'a aucune incidence sur le comportement des connexions qui sont déjà en cours. (Utilisez l’instruction `KILL` pour mettre fin à une connexion existante.) Les connexions désactivées conservent leurs autorisations et peuvent toujours être usurpées.

DEFAULT_DATABASE **=** _database_ spécifie une base de données par défaut à affecter à la connexion.

DEFAULT_LANGUAGE **=** _language_ spécifie une langue par défaut à affecter à la connexion. La langue par défaut pour toutes les connexions SQL Database est l’anglais. Ceci ne peut pas être modifié. La langue par défaut de la connexion `sa` sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Linux est l’anglais, mais vous pouvez la modifier.

### <a name="arguments-applicable-only-to-sql-logins"></a>Arguments applicables uniquement aux connexions SQL

PASSWORD **='** _password_ **'** s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie le mot de passe de la connexion en cours de modification. Les mots de passe respectent la casse. Les mots de passe ne s’appliquent pas non plus en cas d’utilisation avec des connexions externes, telles que des connexions Azure AD.

Des connexions actives en permanence à SQL Database nécessitent une réautorisation (effectuée par le moteur de base de données) au moins toutes les 10 heures. Le moteur de base de données tente une réautorisation en utilisant le mot de passe envoyé à l’origine et aucune entrée utilisateur n’est nécessaire. Pour des raisons de performances, quand un mot de passe est réinitialisé dans SQL Database, la connexion n’est pas authentifiée de nouveau, même si elle est réinitialisée suite à un regroupement de connexions. Cela est différent du comportement local de SQL Server. Si le mot de passe a été modifié depuis que la connexion a été initialement autorisée, la connexion doit être interrompue et une nouvelle connexion établie à l’aide du nouveau mot de passe. Un utilisateur avec l’autorisation KILL DATABASE CONNECTION peut mettre fin explicitement à une connexion à SQL Database à l’aide de la commande KILL. Pour plus d’informations, consultez [KILL](../../t-sql/language-elements/kill-transact-sql.md).

PASSWORD **=** _hashed\_password_ S’applique uniquement au mot clé HASHED. Spécifie la valeur hachée du mot de passe de la connexion créée.

HASHED s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie que le mot de passe entré après l'argument PASSWORD est déjà haché. Si cette option n'est pas sélectionnée, le mot de passe est haché avant d'être stocké dans la base de données. Cette option doit être utilisée uniquement pour la synchronisation de connexion entre deux serveurs. N'utilisez pas l'option HASHED pour modifier des mots de passe de manière régulière.

OLD_PASSWORD **='** _oldpassword_ **'** s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Mot de passe actuel de la connexion à laquelle un nouveau mot de passe doit être attribué. Les mots de passe respectent la casse.

MUST_CHANGE<br>
S'applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous incluez cette option, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] demande un mot de passe actualisé lors de la première utilisation de la connexion modifiée.

NAME = *login_name* correspond au nouveau nom de la connexion à renommer. Si la connexion est une connexion Windows, le SID du principal Windows correspondant au nouveau nom doit correspondre au SID de la connexion dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le nouveau nom d’une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas contenir une barre oblique inverse (\\).

CHECK_EXPIRATION = { ON | **OFF** } s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie si les règles d'expiration des mots de passe doivent être imposées sur cette connexion. La valeur par défaut est OFF.

CHECK_POLICY **=** { **ON** | OFF } s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie que les stratégies de mot de passe Windows de l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute doivent être imposées sur cette connexion. La valeur par défaut est ON.

CREDENTIAL = *credential_name* correspond au nom des informations d’identification à mapper sur une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les informations d'identification doivent déjà exister sur le serveur. Pour plus d’informations, consultez [Informations d’identification](../../relational-databases/security/authentication-access/credentials-database-engine.md). Les informations d’identification ne peuvent pas être mappées à la connexion sa.

NO CREDENTIAL supprime tout mappage existant de la connexion sur des informations d’identification du serveur. Pour plus d’informations, consultez [Informations d’identification](../../relational-databases/security/authentication-access/credentials-database-engine.md).

UNLOCK s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie qu'une connexion verrouillée doit être déverrouillée.

ADD CREDENTIAL ajoute des informations d’identification du fournisseur EKM (Extensible Key Management) à la connexion. Pour plus d’informations, consultez [Gestion de clés extensible (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md).

DROP CREDENTIAL supprime des informations d’identification du fournisseur EKM (Extensible Key Management) de la connexion. Pour plus d’informations, consultez [Gestion de clés extensible (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md).

## <a name="remarks"></a>Notes

Lorsque CHECK_POLICY a la valeur ON, l'argument HASHED ne peut pas être utilisé.

Lorsque CHECK_POLICY prend la valeur ON, le comportement suivant a lieu :

- L'historique du mot de passe est initialisé avec la valeur du hachage de mot de passe actuel.

  Lorsque CHECK_POLICY prend la valeur OFF, le comportement suivant a lieu :

- CHECK_EXPIRATION ON prend également la valeur OFF.
- L'historique du mot de passe est supprimé.
- La valeur de *lockout_time* est réinitialisée.

Si MUST_CHANGE est spécifié, CHECK_EXPIRATION et CHECK_POLICY doivent prendre la valeur ON. Sans quoi, l'instruction échoue.

Si CHECK_POLICY prend la valeur OFF, CHECK_EXPIRATION ne peut pas prendre la valeur ON. Si cette combinaison d'options est utilisée dans une instruction ALTER LOGIN, l'instruction échoue.

Vous ne pouvez pas utiliser ALTER_LOGIN avec l'argument DISABLE pour refuser l'accès à un groupe Windows. C'est la procédure normale. Par exemple, ALTER_LOGIN [*domain\group*] DISABLE retourne le message d’erreur suivant :

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], les données de connexion exigées pour authentifier une connexion et les règles de pare-feu de niveau serveur sont temporairement mises en cache dans chaque base de données. Ce cache est régulièrement actualisé. Pour forcer une actualisation du cache d’authentification et garantir qu’une base de données a la version la plus récente de la table de connexions, exécutez [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Autorisations

Nécessite l'autorisation ALTER ANY LOGIN.

Si l'option CREDENTIAL est utilisée, exige également l'autorisation ALTER ANY CREDENTIAL.

Si la connexion en cours de modification est membre du rôle serveur fixe **sysadmin** ou détient l’autorisation CONTROL SERVER, elle exige également l’autorisation CONTROL SERVER lors des modifications suivantes :

- réinitialisation du mot de passe sans fournir l'ancien mot de passe ;
- activation de MUST_CHANGE, CHECK_POLICY ou CHECK_EXPIRATION ;
- modification du nom de la connexion ;
- activation ou désactivation de la connexion ;
- mappage de la connexion sur une autre information d'identification.

Un principal peut modifier le mot de passe, la langue et la base de données par défaut de sa propre connexion.

Seul un principal SQL avec des privilèges `sysadmin` peut exécuter une commande ALTER LOGIN sur une connexion Azure AD.

## <a name="examples"></a>Exemples

Ces exemples incluent également des exemples d’utilisation d’autres produits SQL. Vérifiez les arguments pris en charge ci-dessus.

### <a name="a-enabling-a-disabled-login"></a>R. Activation d'une connexion désactivée

L'exemple suivant active la connexion `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. Modification du mot de passe d'une connexion

L'exemple suivant remplace le mot de passe de la connexion `Mary5` par un mot de passe fort.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. Modification du nom d'une connexion

L'exemple suivant remplace le nom de connexion `Mary5` par `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. Mappage d'une connexion sur des informations d'identification

L'exemple suivant mappe la connexion `John2` aux informations d'identification `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. Mappage d'une connexion à des informations d'identification de gestion de clés extensible

L'exemple suivant mappe la connexion `Mary5` aux informations d'identification EKM `EKMProvider1`.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures ainsi qu’à Azure SQL Managed Instance.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. Déverrouillage d'une connexion

Pour déverrouiller une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], exécutez l'instruction suivante, en remplaçant \*\*\*\* par le mot de passe de compte souhaité.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Pour déverrouiller une connexion sans modifier le mot de passe, désactivez la stratégie de contrôle, puis réactivez-la.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. Modification du mot de passe d'une connexion à l'aide de HASHED

L'exemple suivant modifie le mot de passe de la connexion `TestUser` en une valeur déjà hachée.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures ainsi qu’à Azure SQL Managed Instance.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

### <a name="h-disabling-the-login-of-an-azure-ad-user"></a>H. Désactivation de la connexion d’un utilisateur Azure AD

L’exemple suivant désactive la connexion d’un utilisateur Azure AD, joe@contoso.com.

```sql
ALTER LOGIN [joe@contoso.com] DISABLE
```

## <a name="see-also"></a>Voir aussi

- [Informations d'identification](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Gestion de clés extensible (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for Azure Synapse

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE
  
<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
    ]
    | NAME = login_name
```

## <a name="arguments"></a>Arguments

*login_name* spécifie le nom de la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cours de modification. Les connexions de domaine doivent être placées entre crochets au format [domaine\utilisateur].

ENABLE | DISABLE active ou désactive cette connexion. La désactivation d'une connexion n'a aucune incidence sur le comportement des connexions qui sont déjà en cours. (Utilisez l’instruction `KILL` pour mettre fin à des connexions existantes.) Les connexions désactivées conservent leurs autorisations et peuvent toujours être usurpées.

PASSWORD **='** _password_ **'** s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie le mot de passe de la connexion en cours de modification. Les mots de passe respectent la casse.

Des connexions actives en permanence à SQL Database nécessitent une réautorisation (effectuée par le moteur de base de données) au moins toutes les 10 heures. Le moteur de base de données tente une réautorisation en utilisant le mot de passe envoyé à l’origine et aucune entrée utilisateur n’est nécessaire. Pour des raisons de performances, quand un mot de passe est réinitialisé dans SQL Database, la connexion n’est pas authentifiée de nouveau, même si elle est réinitialisée suite à un regroupement de connexions. Cela est différent du comportement local de SQL Server. Si le mot de passe a été modifié depuis que la connexion a été initialement autorisée, la connexion doit être interrompue et une nouvelle connexion établie à l’aide du nouveau mot de passe. Un utilisateur avec l’autorisation KILL DATABASE CONNECTION peut mettre fin explicitement à une connexion à SQL Database à l’aide de la commande KILL. Pour plus d’informations, consultez [KILL](../../t-sql/language-elements/kill-transact-sql.md).

> [!IMPORTANT]
> Lorsqu'un compte de connexion (ou un utilisateur de base de données autonome) se connecte et est authentifié, la connexion met en cache les informations d'identité sur la connexion. Dans le cas d'une connexion d'authentification Windows, ces informations incluent des données sur l'appartenance aux groupes Windows. L'identité de la connexion reste authentifiée tant que la connexion est conservée. Pour imposer des modifications d'identité, une réinitialisation du mot de passe, par exemple, ou la modification de l'appartenance au groupe Windows, le compte de connexion doit fermer une session de l'autorité d'authentification (Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) et ouvrir une nouvelle session. Un membre du rôle serveur fixe **sysadmin** ou tout compte de connexion doté de l’autorisation **ALTER ANY CONNECTION** peut utiliser la commande **KILL** pour mettre fin à une connexion et obliger le compte de connexion à se reconnecter. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] peut réutiliser les informations de connexion lors de l’ouverture de plusieurs connexions dans les fenêtres de l’Explorateur d’objets et de l’éditeur de requête. Fermez toutes les connexions pour imposer une reconnexion.

OLD_PASSWORD **='** _oldpassword_ **'** s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Mot de passe actuel de la connexion à laquelle un nouveau mot de passe doit être attribué. Les mots de passe respectent la casse.

NAME = *login_name* correspond au nouveau nom de la connexion à renommer. S'il s'agit d'une connexion Windows, le SID du principal Windows correspondant au nouveau nom doit correspondre au SID de la connexion dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le nouveau nom d’une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas contenir une barre oblique inverse (\\).

## <a name="remarks"></a>Notes

Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], les données de connexion exigées pour authentifier une connexion et les règles de pare-feu de niveau serveur sont temporairement mises en cache dans chaque base de données. Ce cache est régulièrement actualisé. Pour forcer une actualisation du cache d’authentification et garantir qu’une base de données a la version la plus récente de la table de connexions, exécutez [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Autorisations

Nécessite l'autorisation ALTER ANY LOGIN.

Si la connexion en cours de modification est membre du rôle serveur fixe **sysadmin** ou détient l’autorisation CONTROL SERVER, elle exige également l’autorisation CONTROL SERVER lors des modifications suivantes :

- réinitialisation du mot de passe sans fournir l'ancien mot de passe ;
- modification du nom de la connexion ;
- activation ou désactivation de la connexion ;
- mappage de la connexion sur une autre information d'identification.

Un principal peut modifier le mot de passe pour sa propre connexion.

## <a name="examples"></a>Exemples

Ces exemples incluent également des exemples d’utilisation d’autres produits SQL. Vérifiez les arguments pris en charge ci-dessus.

### <a name="a-enabling-a-disabled-login"></a>R. Activation d'une connexion désactivée

L'exemple suivant active la connexion `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. Modification du mot de passe d'une connexion

L'exemple suivant remplace le mot de passe de la connexion `Mary5` par un mot de passe fort.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. Modification du nom d'une connexion

L'exemple suivant remplace le nom de connexion `Mary5` par `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. Mappage d'une connexion sur des informations d'identification

L'exemple suivant mappe la connexion `John2` aux informations d'identification `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. Mappage d'une connexion à des informations d'identification de gestion de clés extensible

L'exemple suivant mappe la connexion `Mary5` aux informations d'identification EKM `EKMProvider1`.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. Déverrouillage d'une connexion

Pour déverrouiller une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], exécutez l'instruction suivante, en remplaçant \*\*\*\* par le mot de passe de compte souhaité.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Pour déverrouiller une connexion sans modifier le mot de passe, désactivez la stratégie de contrôle, puis réactivez-la.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. Modification du mot de passe d'une connexion à l'aide de HASHED

L'exemple suivant modifie le mot de passe de la connexion `TestUser` en une valeur déjà hachée.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>Voir aussi

- [Informations d'identification](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Gestion de clés extensible (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Système de la plateforme d'analyse

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for Analytics Platform System

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    }

<status_option> ::=ENABLE | DISABLE

<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
      | <password_option> [<password_option> ]
    ]
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }

<password_option> ::=
    MUST_CHANGE | UNLOCK
```

## <a name="arguments"></a>Arguments

*login_name* spécifie le nom de la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cours de modification. Les connexions de domaine doivent être placées entre crochets au format [domaine\utilisateur].

ENABLE | DISABLE active ou désactive cette connexion. La désactivation d'une connexion n'a aucune incidence sur le comportement des connexions qui sont déjà en cours. (Utilisez l’instruction `KILL` pour mettre fin à une connexion existante.) Les connexions désactivées conservent leurs autorisations et peuvent toujours être usurpées.

PASSWORD **='** _password_ **'** s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie le mot de passe de la connexion en cours de modification. Les mots de passe respectent la casse.

> [!IMPORTANT]
> Lorsqu'un compte de connexion (ou un utilisateur de base de données autonome) se connecte et est authentifié, la connexion met en cache les informations d'identité sur la connexion. Dans le cas d'une connexion d'authentification Windows, ces informations incluent des données sur l'appartenance aux groupes Windows. L'identité de la connexion reste authentifiée tant que la connexion est conservée. Pour imposer des modifications d'identité, une réinitialisation du mot de passe, par exemple, ou la modification de l'appartenance au groupe Windows, le compte de connexion doit fermer une session de l'autorité d'authentification (Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) et ouvrir une nouvelle session. Un membre du rôle serveur fixe **sysadmin** ou tout compte de connexion doté de l’autorisation **ALTER ANY CONNECTION** peut utiliser la commande **KILL** pour mettre fin à une connexion et obliger le compte de connexion à se reconnecter. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] peut réutiliser les informations de connexion lors de l’ouverture de plusieurs connexions dans les fenêtres de l’Explorateur d’objets et de l’éditeur de requête. Fermez toutes les connexions pour imposer une reconnexion.

OLD_PASSWORD **='** _oldpassword_ **'** s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Mot de passe actuel de la connexion à laquelle un nouveau mot de passe doit être attribué. Les mots de passe respectent la casse.

MUST_CHANGE s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous incluez cette option, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] demande un mot de passe actualisé lors de la première utilisation de la connexion modifiée.

NAME = *login_name* correspond au nouveau nom de la connexion à renommer. Si la connexion est une connexion Windows, le SID du principal Windows correspondant au nouveau nom doit correspondre au SID de la connexion dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le nouveau nom d’une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas contenir une barre oblique inverse (\\).

CHECK_EXPIRATION = { ON | **OFF** } s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie si les règles d'expiration des mots de passe doivent être imposées sur cette connexion. La valeur par défaut est OFF.

CHECK_POLICY **=** { **ON** | OFF } s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie que les stratégies de mot de passe Windows de l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute doivent être imposées sur cette connexion. La valeur par défaut est ON.

UNLOCK s’applique uniquement aux connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifie qu'une connexion verrouillée doit être déverrouillée.

## <a name="remarks"></a>Notes

Lorsque CHECK_POLICY a la valeur ON, l'argument HASHED ne peut pas être utilisé.

Lorsque CHECK_POLICY prend la valeur ON, le comportement suivant a lieu :

- L'historique du mot de passe est initialisé avec la valeur du hachage de mot de passe actuel.

  Lorsque CHECK_POLICY prend la valeur OFF, le comportement suivant a lieu :

- CHECK_EXPIRATION ON prend également la valeur OFF.
- L'historique du mot de passe est supprimé.
- La valeur de *lockout_time* est réinitialisée.

Si MUST_CHANGE est spécifié, CHECK_EXPIRATION et CHECK_POLICY doivent prendre la valeur ON. Sans quoi, l'instruction échoue.

Si CHECK_POLICY prend la valeur OFF, CHECK_EXPIRATION ne peut pas prendre la valeur ON. Si cette combinaison d'options est utilisée dans une instruction ALTER LOGIN, l'instruction échoue.

Vous ne pouvez pas utiliser ALTER_LOGIN avec l'argument DISABLE pour refuser l'accès à un groupe Windows. C'est la procédure normale. Par exemple, ALTER_LOGIN [*domain\group*] DISABLE retourne le message d’erreur suivant :

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], les données de connexion exigées pour authentifier une connexion et les règles de pare-feu de niveau serveur sont temporairement mises en cache dans chaque base de données. Ce cache est régulièrement actualisé. Pour forcer une actualisation du cache d’authentification et garantir qu’une base de données a la version la plus récente de la table de connexions, exécutez [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Autorisations

Nécessite l'autorisation ALTER ANY LOGIN.

Si l'option CREDENTIAL est utilisée, exige également l'autorisation ALTER ANY CREDENTIAL.

Si la connexion en cours de modification est membre du rôle serveur fixe **sysadmin** ou détient l’autorisation CONTROL SERVER, elle exige également l’autorisation CONTROL SERVER lors des modifications suivantes :

- réinitialisation du mot de passe sans fournir l'ancien mot de passe ;
- activation de MUST_CHANGE, CHECK_POLICY ou CHECK_EXPIRATION ;
- modification du nom de la connexion ;
- activation ou désactivation de la connexion ;
- mappage de la connexion sur une autre information d'identification.

Un principal peut modifier le mot de passe, la langue et la base de données par défaut de sa propre connexion.

## <a name="examples"></a>Exemples

Ces exemples incluent également des exemples d’utilisation d’autres produits SQL. Vérifiez les arguments pris en charge ci-dessus.

### <a name="a-enabling-a-disabled-login"></a>R. Activation d'une connexion désactivée

L'exemple suivant active la connexion `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. Modification du mot de passe d'une connexion

L'exemple suivant remplace le mot de passe de la connexion `Mary5` par un mot de passe fort.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. Modification du nom d'une connexion

L'exemple suivant remplace le nom de connexion `Mary5` par `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. Mappage d'une connexion sur des informations d'identification

L'exemple suivant mappe la connexion `John2` aux informations d'identification `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. Mappage d'une connexion à des informations d'identification de gestion de clés extensible

L'exemple suivant mappe la connexion `Mary5` aux informations d'identification EKM `EKMProvider1`.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. Déverrouillage d'une connexion

Pour déverrouiller une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], exécutez l'instruction suivante, en remplaçant \*\*\*\* par le mot de passe de compte souhaité.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Pour déverrouiller une connexion sans modifier le mot de passe, désactivez la stratégie de contrôle, puis réactivez-la.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. Modification du mot de passe d'une connexion à l'aide de HASHED

L'exemple suivant modifie le mot de passe de la connexion `TestUser` en une valeur déjà hachée.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>Voir aussi

- [Informations d'identification](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Gestion de clés extensible (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
