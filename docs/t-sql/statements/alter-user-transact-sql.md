---
description: ALTER USER (Transact-SQL)
title: ALTER USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_USER_TSQL
- ALTER USER
dev_langs:
- TSQL
helpviewer_keywords:
- modifying database users
- ALTER USER statement
- renaming databases
- schemas [SQL Server], default
- database user names [SQL Server]
- names [SQL Server], database users
- default schemas
- modifying default schemas
ms.assetid: 344fc6ce-a008-47c8-a02e-47fae66cc590
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ff06180d1abb67239407d91c564558e4d45c2550
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644415"
---
# <a name="alter-user-transact-sql"></a>ALTER USER (Transact-SQL)

Renomme un utilisateur de base de données ou change son schéma par défaut.

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Base de données SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for SQL Server

ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
```

## <a name="arguments"></a>Arguments

 *userName* : spécifie le nom qui identifie l'utilisateur dans cette base de données.

 LOGIN **=** _loginName_ : remappe un utilisateur à une autre connexion en modifiant l'identificateur de sécurité (SID) de l'utilisateur de manière à ce qu'il corresponde au SID de la connexion.

 NAME **=** _newUserName_ : spécifie le nouveau nom de cet utilisateur. *newUserName* ne doit pas déjà exister dans la base de données active.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } : spécifie le premier schéma que le serveur va interroger pour résoudre les noms des objets associés à cet utilisateur. La définition du schéma par défaut sur NULL supprime un schéma par défaut d'un groupe Windows. L'option NULL ne peut pas être utilisée avec un utilisateur Windows.

 PASSWORD **=** '*password*'  **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Spécifie le mot de passe de l'utilisateur à modifier. Les mots de passe respectent la casse.

> [!NOTE]
> Cette option est uniquement disponible pour les utilisateurs à relation contenant-contenu. Pour plus d’informations, consultez [Bases de données autonomes](../../relational-databases/databases/contained-databases.md) et [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_ **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Mot de passe de l’utilisateur actuel qui sera remplacé par '*password*'. Les mots de passe respectent la casse. *OLD_PASSWORD* est obligatoire pour changer un mot de passe, sauf si vous disposez de l’autorisation **ALTER ANY USER**. La spécification obligatoire du paramètre *OLD_PASSWORD* empêche les utilisateurs disposant de l’autorisation **IMPERSONATION** de changer le mot de passe.

> [!NOTE]
> Cette option est uniquement disponible pour les utilisateurs à relation contenant-contenu.

 DEFAULT_LANGUAGE **=** _{ NONE \| \<lcid> \| \<language name> \| \<language alias> }_ **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.

 Spécifie une langue par défaut à affecter à l'utilisateur. Si cette option a la valeur NONE, la langue par défaut est la langue par défaut actuellement définie pour la base de données. Si la langue par défaut de la base de données est modifiée ultérieurement, la langue par défaut de l'utilisateur reste inchangée. *DEFAULT_LANGUAGE* peut être l’ID local (lcid), le nom de la langue ou l’alias de langue.

> [!NOTE]
> Cette option peut être spécifiée uniquement dans une base de données autonome et uniquement pour les utilisateurs autonomes.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

 Supprime les contrôles de métadonnées de chiffrement sur le serveur dans les opérations de copie en bloc. Cela permet à l’utilisateur de copier en bloc des données chiffrées entre des tables ou des bases de données, sans déchiffrer les données. La valeur par défaut est OFF.

> [!WARNING]
> Une utilisation incorrecte de cette option peut entraîner une altération des données. Pour plus d’informations, consultez [Migrer des données sensibles protégées par Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Notes

 Le schéma par défaut correspond au premier schéma que le serveur doit interroger pour résoudre les noms des objets associés à cet utilisateur de base de données. Sauf spécification contraire, le schéma par défaut sera le propriétaire des objets créés par cet utilisateur de la base de données.

 Si l'utilisateur possède un schéma par défaut, ce schéma par défaut est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, mais qu'il est membre d'un groupe qui dispose d'un schéma par défaut, le schéma par défaut du groupe est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, et qu'il est membre de plus d'un groupe, le schéma par défaut de l'utilisateur sera celui du groupe Windows avec le principal_id le plus bas et un schéma par défaut défini explicite. Si aucun schéma par défaut ne peut être déterminé pour un utilisateur, le schéma **dbo** est utilisé.

 La valeur de DEFAULT_SCHEMA peut désigner un schéma qui n'existe pas encore dans la base de données. Vous pouvez donc affecter un schéma DEFAULT_SCHEMA à un utilisateur avant de créer le schéma en question.

 En revanche, vous ne pouvez pas spécifier DEFAULT_SCHEMA pour un utilisateur mappé à un certificat ou à une clé asymétrique.

> [!IMPORTANT]
> La valeur DEFAULT_SCHEMA est ignorée si l’utilisateur est membre du rôle serveur fixe **sysadmin**. Tous les membres du rôle serveur fixe **sysadmin** possèdent le schéma par défaut `dbo`.

 Vous ne pouvez modifier le nom d'un utilisateur associé à une connexion d'accès ou à un groupe Windows que si le SID du nouveau nom d'utilisateur correspond au SID enregistré dans la base de données. Cette vérification permet d'empêcher l'usurpation des identités de connexion Windows dans la base de données.

 La clause WITH LOGIN active le remappage d'un utilisateur à une connexion différente. Les utilisateurs sans connexion, ceux mappés à un certificat ou bien ceux mappés à une clé asymétrique ne peuvent pas être remappés avec cette clause. Seuls les utilisateurs SQL et les utilisateurs (ou groupes) Windows peuvent être remappés. La clause WITH LOGIN ne peut pas être utilisée pour modifier le type d'utilisateur, par exemple pour modifier un compte Windows en connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Le nom de l'utilisateur sera renommé automatiquement avec le nom de la connexion si les conditions suivantes sont remplies.

- L'utilisateur est un utilisateur Windows.

- Le nom est un nom Windows (contient une barre oblique inverse).

- Aucun nouveau nom n'a été spécifié.

- Le nom actuel diffère du nom de connexion.

 Sinon, l'utilisateur ne sera pas renommé, sauf si l'appelant appelle également la clause NAME.

Le nom d’un utilisateur mappé à un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificat ou une clé asymétrique ne peut pas contenir de barre oblique inverse (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Sécurité

> [!NOTE]
> Un utilisateur bénéficiant de l’autorisation **ALTER ANY USER** peut changer le schéma par défaut de n’importe quel utilisateur. Un utilisateur dont le schéma a été modifié peut, sans le savoir, sélectionner des données dans la mauvaise table ou exécuter du code à partir du mauvais schéma.

### <a name="permissions"></a>Autorisations

 Pour changer le nom d’un utilisateur, vous devez disposer de l’autorisation **ALTER ANY USER**.

 Pour changer la connexion cible d’un utilisateur, vous devez disposer de l’autorisation **CONTROL** sur la base de données.

 Pour changer le nom d’un utilisateur bénéficiant de l’autorisation **CONTROL** sur la base de données, vous devez disposer de l’autorisation **CONTROL** sur la base de données.

 Pour changer le schéma ou la langue par défaut, vous devez disposer de l’autorisation **ALTER** sur l’utilisateur. Les utilisateurs peuvent modifier leur propre schéma ou langue par défaut.

## <a name="examples"></a>Exemples

Tous les exemples sont exécutés dans une base de données utilisateur.

### <a name="a-changing-the-name-of-a-database-user"></a>R. Modification du nom d'un utilisateur de base de données

 L'exemple suivant modifie le nom de l'utilisateur de base de données `Mary5` en `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Modification du schéma par défaut d'un utilisateur

 L'exemple suivant modifie le schéma par défaut de l'utilisateur `Mary51` en `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Modification de plusieurs options à la fois

 L'exemple suivant modifie plusieurs options pour un utilisateur de base de données autonome dans une instruction.

**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

## <a name="see-also"></a>Voir aussi

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Bases de données autonomes](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Database \*_**
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL Database

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for Azure SQL Database

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = schemaName
| LOGIN = loginName
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
[;]

-- Azure SQL Database Update Syntax
ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- SQL Database syntax when connected to a federation member
ALTER USER userName
 WITH <set_item> [ ,... n ]
[;]

<set_item> ::=
 NAME = newUserName
```

## <a name="arguments"></a>Arguments

 *userName* : spécifie le nom qui identifie l'utilisateur dans cette base de données.

 LOGIN **=** _loginName_ : remappe un utilisateur à une autre connexion en modifiant l'identificateur de sécurité (SID) de l'utilisateur de manière à ce qu'il corresponde au SID de la connexion.

 Si l’instruction ALTER USER est la seule instruction d’un lot SQL, Azure SQL Database prend en charge la clause WITH LOGIN. Si l'instruction ALTER USER n'est pas la seule instruction d'un lot SQL ou est exécutée en SQL dynamique, la clause WITH LOGIN n'est pas prise en charge.

 NAME **=** _newUserName_ : spécifie le nouveau nom de cet utilisateur. *newUserName* ne doit pas déjà exister dans la base de données active.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } : spécifie le premier schéma que le serveur va interroger pour résoudre les noms des objets associés à cet utilisateur. La définition du schéma par défaut sur NULL supprime un schéma par défaut d'un groupe Windows. L'option NULL ne peut pas être utilisée avec un utilisateur Windows.

 PASSWORD **=** '*password*'  **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Spécifie le mot de passe de l'utilisateur à modifier. Les mots de passe respectent la casse.

> [!NOTE]
> Cette option est uniquement disponible pour les utilisateurs à relation contenant-contenu. Pour plus d’informations, consultez [Bases de données autonomes](../../relational-databases/databases/contained-databases.md) et [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_ **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Mot de passe de l’utilisateur actuel qui sera remplacé par '*password*'. Les mots de passe respectent la casse. *OLD_PASSWORD* est obligatoire pour changer un mot de passe, sauf si vous disposez de l’autorisation **ALTER ANY USER**. La spécification obligatoire du paramètre *OLD_PASSWORD* empêche les utilisateurs disposant de l’autorisation **IMPERSONATION** de changer le mot de passe.

> [!NOTE]
> Cette option est uniquement disponible pour les utilisateurs à relation contenant-contenu.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

 Supprime les contrôles de métadonnées de chiffrement sur le serveur dans les opérations de copie en bloc. Cela permet à l’utilisateur de copier en bloc des données chiffrées entre des tables ou des bases de données, sans déchiffrer les données. La valeur par défaut est OFF.

> [!WARNING]
> Une utilisation incorrecte de cette option peut entraîner une altération des données. Pour plus d’informations, consultez [Migrer des données sensibles protégées par Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Notes

 Le schéma par défaut correspond au premier schéma que le serveur doit interroger pour résoudre les noms des objets associés à cet utilisateur de base de données. Sauf spécification contraire, le schéma par défaut sera le propriétaire des objets créés par cet utilisateur de la base de données.

 Si l'utilisateur possède un schéma par défaut, ce schéma par défaut est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, mais qu'il est membre d'un groupe qui dispose d'un schéma par défaut, le schéma par défaut du groupe est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, et qu'il est membre de plus d'un groupe, le schéma par défaut de l'utilisateur sera celui du groupe Windows avec le principal_id le plus bas et un schéma par défaut défini explicite. Si aucun schéma par défaut ne peut être déterminé pour un utilisateur, le schéma **dbo** est utilisé.

 La valeur de DEFAULT_SCHEMA peut désigner un schéma qui n'existe pas encore dans la base de données. Vous pouvez donc affecter un schéma DEFAULT_SCHEMA à un utilisateur avant de créer le schéma en question.

 En revanche, vous ne pouvez pas spécifier DEFAULT_SCHEMA pour un utilisateur mappé à un certificat ou à une clé asymétrique.

> [!IMPORTANT]
> La valeur DEFAULT_SCHEMA est ignorée si l’utilisateur est membre du rôle serveur fixe **sysadmin**. Tous les membres du rôle serveur fixe **sysadmin** possèdent le schéma par défaut `dbo`.

 Vous ne pouvez modifier le nom d'un utilisateur associé à une connexion d'accès ou à un groupe Windows que si le SID du nouveau nom d'utilisateur correspond au SID enregistré dans la base de données. Cette vérification permet d'empêcher l'usurpation des identités de connexion Windows dans la base de données.

 La clause WITH LOGIN active le remappage d'un utilisateur à une connexion différente. Les utilisateurs sans connexion, ceux mappés à un certificat ou bien ceux mappés à une clé asymétrique ne peuvent pas être remappés avec cette clause. Seuls les utilisateurs SQL et les utilisateurs (ou groupes) Windows peuvent être remappés. La clause WITH LOGIN ne peut pas être utilisée pour modifier le type d'utilisateur, par exemple pour modifier un compte Windows en connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Le nom de l'utilisateur sera renommé automatiquement avec le nom de la connexion si les conditions suivantes sont remplies.

- L'utilisateur est un utilisateur Windows.

- Le nom est un nom Windows (contient une barre oblique inverse).

- Aucun nouveau nom n'a été spécifié.

- Le nom actuel diffère du nom de connexion.

 Sinon, l'utilisateur ne sera pas renommé, sauf si l'appelant appelle également la clause NAME.

Le nom d’un utilisateur mappé à un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificat ou une clé asymétrique ne peut pas contenir de barre oblique inverse (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Sécurité

> [!NOTE]
> Un utilisateur bénéficiant de l’autorisation **ALTER ANY USER** peut changer le schéma par défaut de n’importe quel utilisateur. Un utilisateur dont le schéma a été modifié peut, sans le savoir, sélectionner des données dans la mauvaise table ou exécuter du code à partir du mauvais schéma.

### <a name="permissions"></a>Autorisations

 Pour changer le nom d’un utilisateur, vous devez disposer de l’autorisation **ALTER ANY USER**.

 Pour changer la connexion cible d’un utilisateur, vous devez disposer de l’autorisation **CONTROL** sur la base de données.

 Pour changer le nom d’un utilisateur bénéficiant de l’autorisation **CONTROL** sur la base de données, vous devez disposer de l’autorisation **CONTROL** sur la base de données.

 Pour changer le schéma ou la langue par défaut, vous devez disposer de l’autorisation **ALTER** sur l’utilisateur. Les utilisateurs peuvent modifier leur propre schéma ou langue par défaut.

## <a name="examples"></a>Exemples

Tous les exemples sont exécutés dans une base de données utilisateur.

### <a name="a-changing-the-name-of-a-database-user"></a>R. Modification du nom d'un utilisateur de base de données

 L'exemple suivant modifie le nom de l'utilisateur de base de données `Mary5` en `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Modification du schéma par défaut d'un utilisateur

 L'exemple suivant modifie le schéma par défaut de l'utilisateur `Mary51` en `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Modification de plusieurs options à la fois

 L'exemple suivant modifie plusieurs options pour un utilisateur de base de données autonome dans une instruction.

```sql
ALTER USER Philip
WITHNAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per';
GO
```

## <a name="see-also"></a>Voir aussi

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Bases de données autonomes](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

## <a name="syntax"></a>Syntaxe

> [!IMPORTANT]
> Seules les options suivantes sont prises en charge pour Azure SQL Managed Instance lors de l’application à des utilisateurs avec des connexions Azure AD : `DEFAULT_SCHEMA = { schemaName | NULL }` et `DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }`
> </br> </br> Une nouvelle extension de syntaxe a été ajoutée pour permettre le remappage des utilisateurs dans une base de données qui a été migrée vers Azure SQL Managed Instance. La syntaxe ALTER USER permet de mapper les utilisateurs de base de données dans un domaine fédéré et synchronisé avec Azure AD pour les connexions Azure AD.

```syntaxsql
-- Syntax for SQL Managed Instance
ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- Users or groups that are migrated as federated and synchronized with Azure AD have the following syntax:

/** Applies to Windows users that were migrated and have the following user names:
- Windows user <domain\user>
- Windows group <domain\MyWindowsGroup>
- Windows alias <MyWindowsAlias>
**/

ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
 NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
```

## <a name="arguments"></a>Arguments

 *userName* : spécifie le nom qui identifie l'utilisateur dans cette base de données.

 LOGIN **=** _loginName_ : remappe un utilisateur à une autre connexion en modifiant l'identificateur de sécurité (SID) de l'utilisateur de manière à ce qu'il corresponde au SID de la connexion.

 Si l’instruction ALTER USER est la seule instruction d’un lot SQL, Azure SQL Database prend en charge la clause WITH LOGIN. Si l'instruction ALTER USER n'est pas la seule instruction d'un lot SQL ou est exécutée en SQL dynamique, la clause WITH LOGIN n'est pas prise en charge.

 NAME **=** _newUserName_ : spécifie le nouveau nom de cet utilisateur. *newUserName* ne doit pas déjà exister dans la base de données active.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } : spécifie le premier schéma que le serveur va interroger pour résoudre les noms des objets associés à cet utilisateur. La définition du schéma par défaut sur NULL supprime un schéma par défaut d'un groupe Windows. L'option NULL ne peut pas être utilisée avec un utilisateur Windows.

 PASSWORD **=** '*password*'

 Spécifie le mot de passe de l'utilisateur à modifier. Les mots de passe respectent la casse.

> [!NOTE]
> Cette option est uniquement disponible pour les utilisateurs à relation contenant-contenu. Pour plus d’informations, consultez [Bases de données autonomes](../../relational-databases/databases/contained-databases.md) et [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_

 Mot de passe de l’utilisateur actuel qui sera remplacé par '*password*'. Les mots de passe respectent la casse. *OLD_PASSWORD* est obligatoire pour changer un mot de passe, sauf si vous disposez de l’autorisation **ALTER ANY USER**. La spécification obligatoire du paramètre *OLD_PASSWORD* empêche les utilisateurs disposant de l’autorisation **IMPERSONATION** de changer le mot de passe.

> [!NOTE]
> Cette option est uniquement disponible pour les utilisateurs à relation contenant-contenu.

 DEFAULT_LANGUAGE **=** _{ NONE | \<lcid> | \<language name> | \<language alias> }_

 Spécifie une langue par défaut à affecter à l'utilisateur. Si cette option a la valeur NONE, la langue par défaut est la langue par défaut actuellement définie pour la base de données. Si la langue par défaut de la base de données est modifiée ultérieurement, la langue par défaut de l'utilisateur reste inchangée. *DEFAULT_LANGUAGE* peut être l’ID local (lcid), le nom de la langue ou l’alias de langue.

> [!NOTE]
> Cette option peut être spécifiée uniquement dans une base de données autonome et uniquement pour les utilisateurs autonomes.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]

 Supprime les contrôles de métadonnées de chiffrement sur le serveur dans les opérations de copie en bloc. Cela permet à l’utilisateur de copier en bloc des données chiffrées entre des tables ou des bases de données, sans déchiffrer les données. La valeur par défaut est OFF.

> [!WARNING]
> Une utilisation incorrecte de cette option peut entraîner une altération des données. Pour plus d’informations, consultez [Migrer des données sensibles protégées par Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Notes

 Le schéma par défaut correspond au premier schéma que le serveur doit interroger pour résoudre les noms des objets associés à cet utilisateur de base de données. Sauf spécification contraire, le schéma par défaut sera le propriétaire des objets créés par cet utilisateur de la base de données.

 Si l'utilisateur possède un schéma par défaut, ce schéma par défaut est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, mais qu'il est membre d'un groupe qui dispose d'un schéma par défaut, le schéma par défaut du groupe est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, et qu'il est membre de plus d'un groupe, le schéma par défaut de l'utilisateur sera celui du groupe Windows avec le principal_id le plus bas et un schéma par défaut défini explicite. Si aucun schéma par défaut ne peut être déterminé pour un utilisateur, le schéma **dbo** est utilisé.

 La valeur de DEFAULT_SCHEMA peut désigner un schéma qui n'existe pas encore dans la base de données. Vous pouvez donc affecter un schéma DEFAULT_SCHEMA à un utilisateur avant de créer le schéma en question.

 En revanche, vous ne pouvez pas spécifier DEFAULT_SCHEMA pour un utilisateur mappé à un certificat ou à une clé asymétrique.

> [!IMPORTANT]
> La valeur DEFAULT_SCHEMA est ignorée si l’utilisateur est membre du rôle serveur fixe **sysadmin**. Tous les membres du rôle serveur fixe **sysadmin** possèdent le schéma par défaut `dbo`.

 Vous ne pouvez modifier le nom d'un utilisateur associé à une connexion d'accès ou à un groupe Windows que si le SID du nouveau nom d'utilisateur correspond au SID enregistré dans la base de données. Cette vérification permet d'empêcher l'usurpation des identités de connexion Windows dans la base de données.

 La clause WITH LOGIN active le remappage d'un utilisateur à une connexion différente. Les utilisateurs sans connexion, ceux mappés à un certificat ou bien ceux mappés à une clé asymétrique ne peuvent pas être remappés avec cette clause. Seuls les utilisateurs SQL et les utilisateurs (ou groupes) Windows peuvent être remappés. La clause WITH LOGIN ne peut pas être utilisée pour modifier le type d'utilisateur, par exemple pour modifier un compte Windows en connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La seule exception est lors de la modification d’un utilisateur Windows en un utilisateur Azure AD.

> [!NOTE]
> Les règles suivantes ne s’appliquent pas aux utilisateurs Windows sur Azure SQL Managed Instance, car nous ne prenons pas en charge la création de connexions Windows sur Azure SQL Managed Instance. L’option WITH LOGIN peut être utilisée uniquement si des connexions Azure AD sont présentes.

 Le nom de l'utilisateur sera renommé automatiquement avec le nom de la connexion si les conditions suivantes sont remplies.

- L'utilisateur est un utilisateur Windows.

- Le nom est un nom Windows (contient une barre oblique inverse).

- Aucun nouveau nom n'a été spécifié.

- Le nom actuel diffère du nom de connexion.

 Sinon, l'utilisateur ne sera pas renommé, sauf si l'appelant appelle également la clause NAME.

Le nom d’un utilisateur mappé à un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificat ou une clé asymétrique ne peut pas contenir de barre oblique inverse (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

### <a name="remarks-for-windows-users-in-sql-on-premises-migrated-to-azure-sql-managed-instance"></a>Remarques pour les utilisateurs Windows dans SQL local migrés vers Azure SQL Managed Instance

Ces remarques s’appliquent à l’authentification en tant qu’utilisateurs Windows qui ont été fédérés et synchronisés avec Azure AD.

> [!NOTE]
> La fonctionnalité d’administration Azure AD pour Azure SQL Managed Instance a changé après la création. Pour plus d’informations, consultez [Nouvelle fonctionnalité d’administration Azure AD pour MI](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

- La validation des utilisateurs ou des groupes Windows mappés à Azure AD s’effectue par défaut via l’API Graph dans toutes les versions de la syntaxe ALTER USER utilisée à des fins de migration.
- Les utilisateurs locaux à qui un nom d’alias a été attribué (à l’aide d’un nom différent du compte Windows d’origine) conserveront le nom d’alias.
- Pour l’authentification Azure AD, le paramètre LOGIN s’applique uniquement à Azure SQL Managed Instance et ne peut pas être utilisé avec SQL Database.
- Pour afficher les connexions pour les principaux Azure AD, utilisez la commande suivante : `select * from sys.server_principals`.
- Vérifiez que le type indiqué de la connexion est `E` ou `X`.
- L’option PASSWORD ne peut pas être utilisée pour les utilisateurs Azure AD.
- Dans tous les cas de migration, les rôles et les autorisations des utilisateurs ou des groupes Windows sont automatiquement transférés vers les nouveaux utilisateurs ou groupes Azure AD.
- Une nouvelle extension de syntaxe, **FROM EXTERNAL PROVIDER** est disponible pour la modification des utilisateurs et des groupes Windows à partir de SQL local pour les utilisateurs et les groupes Azure AD. Le domaine Windows doit être fédéré avec Azure AD et tous les membres du domaine Windows doivent exister dans Azure AD lors de l’utilisation de cette extension. La syntaxe **FROM EXTERNAL PROVIDER** s’applique à Azure SQL Managed Instance et doit être utilisée si les utilisateurs Windows n’ont pas de connexion sur l’instance SQL d’origine et doivent être mappés à des utilisateurs de base de données Azure AD autonome.
- Dans ce cas, le nom d’utilisateur (userName) autorisé peut être :
- Un utilisateur Widows (_domaine\utilisateur_).
- Un groupe Windows (_MyWidnowsGroup_).
- Un alias Windows (_MyWindowsAlias_).
- Le résultat de la commande ALTER remplace l’ancien userName par le nom correspondant trouvé dans Azure AD en fonction du SID d’origine de l’ancien userName. Le nom modifié est remplacé et stocké dans les métadonnées de la base de données :
- (_domaine\utilisateur_) sera remplacé par Azure AD user@domain.com.
- (_domaine\\MyWidnowsGroup_) sera remplacé par un groupe Azure AD.
- (_MyWindowsAlias_) restera inchangé, mais le SID de cet utilisateur sera archivé Azure AD.

> [!NOTE]
> Si le SID de l’utilisateur d’origine converti en objectID est introuvable dans Azure AD, la commande ALTER USER échoue.

- Pour afficher les utilisateurs modifiés, utilisez la commande suivante : `select * from sys.database_principals`
- Vérifiez que le type indiqué de l’utilisateur est `E` ou `X`.
- Quand NAME est utilisé pour migrer des utilisateurs Windows vers des utilisateurs Azure AD, les restrictions suivantes s’appliquent :
- Une valeur LOGIN valide doit être spécifiée.
- La valeur NAME sera vérifiée dans Azure AD et ne peut être que :
- Le nom de la valeur LOGIN.
- Un alias - le nom ne peut pas exister dans Azure AD.
- Dans tous les autres cas, la syntaxe échoue.

## <a name="security"></a>Sécurité

> [!NOTE]
> Un utilisateur bénéficiant de l’autorisation **ALTER ANY USER** peut changer le schéma par défaut de n’importe quel utilisateur. Un utilisateur dont le schéma a été modifié peut, sans le savoir, sélectionner des données dans la mauvaise table ou exécuter du code à partir du mauvais schéma.

### <a name="permissions"></a>Autorisations

 Pour changer le nom d’un utilisateur, vous devez disposer de l’autorisation **ALTER ANY USER**.

 Pour changer la connexion cible d’un utilisateur, vous devez disposer de l’autorisation **CONTROL** sur la base de données.

 Pour changer le nom d’un utilisateur bénéficiant de l’autorisation **CONTROL** sur la base de données, vous devez disposer de l’autorisation **CONTROL** sur la base de données.

 Pour changer le schéma ou la langue par défaut, vous devez disposer de l’autorisation **ALTER** sur l’utilisateur. Les utilisateurs peuvent modifier leur propre schéma ou langue par défaut.

## <a name="examples"></a>Exemples

Tous les exemples sont exécutés dans une base de données utilisateur.

### <a name="a-changing-the-name-of-a-database-user"></a>R. Modification du nom d'un utilisateur de base de données

 L'exemple suivant modifie le nom de l'utilisateur de base de données `Mary5` en `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Modification du schéma par défaut d'un utilisateur

 L'exemple suivant modifie le schéma par défaut de l'utilisateur `Mary51` en `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Modification de plusieurs options à la fois

 L'exemple suivant modifie plusieurs options pour un utilisateur de base de données autonome dans une instruction.

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

### <a name="d-map-the-user-in-the-database-to-an-azure-ad-login-after-migration"></a>D. Mapper l’utilisateur de la base de données à une connexion Azure AD après la migration

L’exemple suivant remappe l’utilisateur, `westus/joe` à un utilisateur Azure AD, `joe@westus.com`. Cet exemple concerne les connexions qui existent déjà dans l’instance managée. Cette opération doit être effectuée une fois que vous avez terminé la migration d’une base de données vers Azure SQL Managed Instance et si vous voulez utiliser la connexion Azure AD pour l’authentification.

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com
```

### <a name="e-map-an-old-windows-user-in-the-database-without-a-login-in-azure-sql-managed-instance-to-an-azure-ad-user"></a>E. Mapper un ancien utilisateur Windows dans la base de données sans connexion dans Azure SQL Managed Instance à un utilisateur Azure AD

L’exemple suivant remappe l’utilisateur, `westus/joe` sans connexion, à un utilisateur Azure AD `joe@westus.com`. L’utilisateur fédéré doit exister dans Azure AD.

```sql
ALTER USER [westus/joe] FROM EXTERNAL PROVIDER
```

### <a name="f-map-the-user-alias-to-an-existing-azure-ad-login"></a>F. Mapper l’alias utilisateur à une connexion Azure AD existante

L’exemple suivant remappe le nom d’utilisateur `westus\joe` à `joe_alias`. Dans ce cas, la connexion Azure AD correspondante est `joe@westus.com`.

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com, name= joe_alias
```

### <a name="g-map-a-windows-group-that-was-migrated-in-azure-sql-managed-instance-to-an-azure-ad-group"></a>G. Mapper un groupe Windows qui a été migré dans Azure SQL Managed Instance à un groupe Azure AD

L’exemple suivant remappe l’ancien groupe local, `westus\mygroup` à un groupe Azure AD `mygroup` dans l’instance managée. Le groupe doit exister dans Azure AD.

```sql
ALTER USER [westus\mygroup] WITH LOGIN = mygroup
```

## <a name="see-also"></a>Voir aussi

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Bases de données autonomes](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)
- [Tutoriel : Migration d’utilisateurs et de groupes Windows locaux SQL Server vers SQL Managed Instance à l’aide de la syntaxe DDL T-SQL](/azure/sql-database/tutorial-managed-instance-azure-active-directory-migration)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for Azure Synapse

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>Arguments

 *userName* : spécifie le nom qui identifie l'utilisateur dans cette base de données.

 LOGIN **=** _loginName_ : remappe un utilisateur à une autre connexion en modifiant l'identificateur de sécurité (SID) de l'utilisateur de manière à ce qu'il corresponde au SID de la connexion.

 Si l’instruction ALTER USER est la seule instruction d’un lot SQL, Azure SQL Database prend en charge la clause WITH LOGIN. Si l'instruction ALTER USER n'est pas la seule instruction d'un lot SQL ou est exécutée en SQL dynamique, la clause WITH LOGIN n'est pas prise en charge.

 NAME **=** _newUserName_ : spécifie le nouveau nom de cet utilisateur. *newUserName* ne doit pas déjà exister dans la base de données active.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } : spécifie le premier schéma que le serveur va interroger pour résoudre les noms des objets associés à cet utilisateur. La définition du schéma par défaut sur NULL supprime un schéma par défaut d'un groupe Windows. L'option NULL ne peut pas être utilisée avec un utilisateur Windows.

## <a name="remarks"></a>Notes

 Le schéma par défaut correspond au premier schéma que le serveur doit interroger pour résoudre les noms des objets associés à cet utilisateur de base de données. Sauf spécification contraire, le schéma par défaut sera le propriétaire des objets créés par cet utilisateur de la base de données.

 Si l'utilisateur possède un schéma par défaut, ce schéma par défaut est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, mais qu'il est membre d'un groupe qui dispose d'un schéma par défaut, le schéma par défaut du groupe est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, et qu'il est membre de plus d'un groupe, le schéma par défaut de l'utilisateur sera celui du groupe Windows avec le principal_id le plus bas et un schéma par défaut défini explicite. Si aucun schéma par défaut ne peut être déterminé pour un utilisateur, le schéma **dbo** est utilisé.

 La valeur de DEFAULT_SCHEMA peut désigner un schéma qui n'existe pas encore dans la base de données. Vous pouvez donc affecter un schéma DEFAULT_SCHEMA à un utilisateur avant de créer le schéma en question.

 En revanche, vous ne pouvez pas spécifier DEFAULT_SCHEMA pour un utilisateur mappé à un certificat ou à une clé asymétrique.

> [!IMPORTANT]
> La valeur DEFAULT_SCHEMA est ignorée si l’utilisateur est membre du rôle serveur fixe **sysadmin**. Tous les membres du rôle serveur fixe **sysadmin** possèdent le schéma par défaut `dbo`.

 La clause WITH LOGIN active le remappage d'un utilisateur à une connexion différente. Les utilisateurs sans connexion, ceux mappés à un certificat ou bien ceux mappés à une clé asymétrique ne peuvent pas être remappés avec cette clause. Seuls les utilisateurs SQL et les utilisateurs (ou groupes) Windows peuvent être remappés. La clause WITH LOGIN ne peut pas être utilisée pour modifier le type d'utilisateur, par exemple pour modifier un compte Windows en connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Le nom de l'utilisateur sera renommé automatiquement avec le nom de la connexion si les conditions suivantes sont remplies.

- Aucun nouveau nom n'a été spécifié.

- Le nom actuel diffère du nom de connexion.

 Sinon, l'utilisateur ne sera pas renommé, sauf si l'appelant appelle également la clause NAME.

Le nom d’un utilisateur mappé à un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificat ou une clé asymétrique ne peut pas contenir de barre oblique inverse (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Sécurité

> [!NOTE]
> Un utilisateur bénéficiant de l’autorisation **ALTER ANY USER** peut changer le schéma par défaut de n’importe quel utilisateur. Un utilisateur dont le schéma a été modifié peut, sans le savoir, sélectionner des données dans la mauvaise table ou exécuter du code à partir du mauvais schéma.

### <a name="permissions"></a>Autorisations

 Pour changer le nom d’un utilisateur, vous devez disposer de l’autorisation **ALTER ANY USER**.

 Pour changer la connexion cible d’un utilisateur, vous devez disposer de l’autorisation **CONTROL** sur la base de données.

 Pour changer le nom d’un utilisateur bénéficiant de l’autorisation **CONTROL** sur la base de données, vous devez disposer de l’autorisation **CONTROL** sur la base de données.

 Pour changer le schéma ou la langue par défaut, vous devez disposer de l’autorisation **ALTER** sur l’utilisateur. Les utilisateurs peuvent modifier leur propre schéma ou langue par défaut.

## <a name="examples"></a>Exemples

Tous les exemples sont exécutés dans une base de données utilisateur.

### <a name="a-changing-the-name-of-a-database-user"></a>R. Modification du nom d'un utilisateur de base de données

 L'exemple suivant modifie le nom de l'utilisateur de base de données `Mary5` en `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Modification du schéma par défaut d'un utilisateur

L'exemple suivant modifie le schéma par défaut de l'utilisateur `Mary51` en `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>Voir aussi

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Bases de données autonomes](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
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

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>Arguments

 *userName* : spécifie le nom qui identifie l'utilisateur dans cette base de données.

 LOGIN **=** _loginName_ : remappe un utilisateur à une autre connexion en modifiant l'identificateur de sécurité (SID) de l'utilisateur de manière à ce qu'il corresponde au SID de la connexion.

 Si l’instruction ALTER USER est la seule instruction d’un lot SQL, Azure SQL Database prend en charge la clause WITH LOGIN. Si l'instruction ALTER USER n'est pas la seule instruction d'un lot SQL ou est exécutée en SQL dynamique, la clause WITH LOGIN n'est pas prise en charge.

 NAME **=** _newUserName_ : spécifie le nouveau nom de cet utilisateur. *newUserName* ne doit pas déjà exister dans la base de données active.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } : spécifie le premier schéma que le serveur va interroger pour résoudre les noms des objets associés à cet utilisateur. La définition du schéma par défaut sur NULL supprime un schéma par défaut d'un groupe Windows. L'option NULL ne peut pas être utilisée avec un utilisateur Windows.

## <a name="remarks"></a>Notes

 Le schéma par défaut correspond au premier schéma que le serveur doit interroger pour résoudre les noms des objets associés à cet utilisateur de base de données. Sauf spécification contraire, le schéma par défaut sera le propriétaire des objets créés par cet utilisateur de la base de données.

 Si l'utilisateur possède un schéma par défaut, ce schéma par défaut est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, mais qu'il est membre d'un groupe qui dispose d'un schéma par défaut, le schéma par défaut du groupe est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, et qu'il est membre de plus d'un groupe, le schéma par défaut de l'utilisateur sera celui du groupe Windows avec le principal_id le plus bas et un schéma par défaut défini explicite. Si aucun schéma par défaut ne peut être déterminé pour un utilisateur, le schéma **dbo** est utilisé.

 La valeur de DEFAULT_SCHEMA peut désigner un schéma qui n'existe pas encore dans la base de données. Vous pouvez donc affecter un schéma DEFAULT_SCHEMA à un utilisateur avant de créer le schéma en question.

 En revanche, vous ne pouvez pas spécifier DEFAULT_SCHEMA pour un utilisateur mappé à un certificat ou à une clé asymétrique.

> [!IMPORTANT]
> La valeur DEFAULT_SCHEMA est ignorée si l’utilisateur est membre du rôle serveur fixe **sysadmin**. Tous les membres du rôle serveur fixe **sysadmin** possèdent le schéma par défaut `dbo`.

 La clause WITH LOGIN active le remappage d'un utilisateur à une connexion différente. Les utilisateurs sans connexion, ceux mappés à un certificat ou bien ceux mappés à une clé asymétrique ne peuvent pas être remappés avec cette clause. Seuls les utilisateurs SQL et les utilisateurs (ou groupes) Windows peuvent être remappés. La clause WITH LOGIN ne peut pas être utilisée pour modifier le type d'utilisateur, par exemple pour modifier un compte Windows en connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 Le nom de l'utilisateur sera renommé automatiquement avec le nom de la connexion si les conditions suivantes sont remplies.

- Aucun nouveau nom n'a été spécifié.

- Le nom actuel diffère du nom de connexion.

 Sinon, l'utilisateur ne sera pas renommé, sauf si l'appelant appelle également la clause NAME.

Le nom d’un utilisateur mappé à un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificat ou une clé asymétrique ne peut pas contenir de barre oblique inverse (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Sécurité

> [!NOTE]
> Un utilisateur bénéficiant de l’autorisation **ALTER ANY USER** peut changer le schéma par défaut de n’importe quel utilisateur. Un utilisateur dont le schéma a été modifié peut, sans le savoir, sélectionner des données dans la mauvaise table ou exécuter du code à partir du mauvais schéma.

### <a name="permissions"></a>Autorisations

 Pour changer le nom d’un utilisateur, vous devez disposer de l’autorisation **ALTER ANY USER**.

 Pour changer la connexion cible d’un utilisateur, vous devez disposer de l’autorisation **CONTROL** sur la base de données.

 Pour changer le nom d’un utilisateur bénéficiant de l’autorisation **CONTROL** sur la base de données, vous devez disposer de l’autorisation **CONTROL** sur la base de données.

 Pour changer le schéma ou la langue par défaut, vous devez disposer de l’autorisation **ALTER** sur l’utilisateur. Les utilisateurs peuvent modifier leur propre schéma ou langue par défaut.

## <a name="examples"></a>Exemples

Tous les exemples sont exécutés dans une base de données utilisateur.

### <a name="a-changing-the-name-of-a-database-user"></a>R. Modification du nom d'un utilisateur de base de données

 L'exemple suivant modifie le nom de l'utilisateur de base de données `Mary5` en `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Modification du schéma par défaut d'un utilisateur
 L'exemple suivant modifie le schéma par défaut de l'utilisateur `Mary51` en `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>Voir aussi

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Bases de données autonomes](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
