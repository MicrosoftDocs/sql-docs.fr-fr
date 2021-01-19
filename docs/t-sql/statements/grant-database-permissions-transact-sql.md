---
description: GRANT – octroi d'autorisations de base de données (Transact-SQL)
title: GRANT - Octroyer des autorisations de base de données (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server]
- database permissions [SQL Server]
- granting permissions [SQL Server], databases
- permissions [SQL Server], databases
- database permissions [SQL Server], granting
- GRANT statement, databases
ms.assetid: 499e5ed6-945c-4791-ab45-68dec0b9c289
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 18a18c170ffddafca6656f7f3092a22e1f3b6524
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171711"
---
# <a name="grant-database-permissions-transact-sql"></a>GRANT – octroi d'autorisations de base de données (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Permet d'accorder des autorisations sur une base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql

GRANT <permission> [ ,...n ]
    TO <database_principal> [ ,...n ] [ WITH GRANT OPTION ]
    [ AS <database_principal> ]

<permission>::=
permission | ALL [ PRIVILEGES ]

<database_principal> ::=
    Database_user
  | Database_role
  | Application_role
  | Database_user_mapped_to_Windows_User
  | Database_user_mapped_to_Windows_Group
  | Database_user_mapped_to_certificate
  | Database_user_mapped_to_asymmetric_key
  | Database_user_with_no_login
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*permission* Spécifie une autorisation qui peut être accordée sur une base de données. Pour obtenir la liste des autorisations, consultez la section Notes plus loin dans cette rubrique.

ALL Cette option n’accorde pas toutes les autorisations possibles. Elle revient à accorder les autorisations suivantes : BACKUP DATABASE, BACKUP LOG, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE et CREATE VIEW.

PRIVILEGES Inclus pour la conformité aux normes ANSI-92. Ne change pas le comportement de l'option ALL.

WITH GRANT OPTION Indique que le principal a également la possibilité d’accorder l’autorisation spécifiée à d’autres principaux.

AS \<database_principal> spécifie un principal dont le principal qui exécute cette requête dérive son droit d’accorder l’autorisation.

*Database_user* Spécifie un utilisateur de base de données.

*Database_role* Spécifie un rôle de base de données.

*Application_role*
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

Spécifie un rôle d'application.

*Database_user_mapped_to_Windows_User*
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures

Spécifie un utilisateur de base de données mappé sur un utilisateur Windows.

*Database_user_mapped_to_Windows_Group*
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures

Spécifie un utilisateur de base de données mappé à un groupe Windows.

*Database_user_mapped_to_certificate*
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures

Spécifie un utilisateur de base de données mappé sur un certificat.

*Database_user_mapped_to_asymmetric_key*
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures

Spécifie un utilisateur de base de données mappé à une clé asymétrique.

*Database_user_with_no_login* Spécifie un utilisateur de base de données sans principal au niveau du serveur correspondant.

## <a name="remarks"></a>Notes

> [!IMPORTANT]
> Dans certains cas, une combinaison d'autorisations ALTER et REFERENCE pourrait autoriser le bénéficiaire des autorisations à afficher des données ou à exécuter des fonctions non autorisées. Par exemple : un utilisateur avec une autorisation ALTER sur une table et une autorisation REFERENCE sur une fonction peut créer une colonne calculée sur une fonction et l’exécuter. Dans ce cas, l'utilisateur doit également disposer d'une autorisation SELECT sur la colonne calculée.

Une base de données est un élément sécurisable contenu par le serveur qui est son parent dans la hiérarchie des autorisations. Les autorisations les plus spécifiques et limitées qu'il est possible d'accorder sur une base de données sont répertoriées dans le tableau ci-dessous, avec les autorisations plus générales qui les incluent de manière implicite.

|Autorisation de base de données|Impliquée par une autorisation de base de données|Déduite d'une autorisation de serveur|
|-------------------------|------------------------------------|----------------------------------|
|ADMINISTER DATABASE BULK OPERATIONS<br/>**S’applique à :** [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].|CONTROL|CONTROL SERVER|
|ALTER|CONTROL|ALTER ANY DATABASE|
|ALTER ANY APPLICATION ROLE|ALTER|CONTROL SERVER|
|ALTER ANY ASSEMBLY|ALTER|CONTROL SERVER|
|ALTER ANY ASYMMETRIC KEY|ALTER|CONTROL SERVER|
|ALTER ANY CERTIFICATE|ALTER|CONTROL SERVER|
|ALTER ANY COLUMN ENCRYPTION KEY|ALTER|CONTROL SERVER|
|ALTER ANY COLUMN MASTER KEY DEFINITION|ALTER|CONTROL SERVER|
|ALTER ANY CONTRACT|ALTER|CONTROL SERVER|
|ALTER ANY DATABASE AUDIT|ALTER|ALTER ANY SERVER AUDIT|
|ALTER ANY DATABASE DDL TRIGGER|ALTER|CONTROL SERVER|
|ALTER ANY DATABASE EVENT NOTIFICATION|ALTER|ALTER ANY EVENT NOTIFICATION|
|ALTER ANY DATABASE EVENT SESSION<br />**S'applique à**: [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].|ALTER|ALTER ANY EVENT SESSION|
|ALTER ANY DATABASE SCOPED CONFIGURATION<br /> **S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].|CONTROL|CONTROL SERVER|
|ALTER ANY DATASPACE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL DATA SOURCE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL FILE FORMAT|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL LIBRARY <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].|CONTROL|CONTROL SERVER |
|ALTER ANY FULLTEXT CATALOG|ALTER|CONTROL SERVER|
|ALTER ANY MASK|CONTROL|CONTROL SERVER|
|ALTER ANY MESSAGE TYPE|ALTER|CONTROL SERVER|
|ALTER ANY REMOTE SERVICE BINDING|ALTER|CONTROL SERVER|
|ALTER ANY ROLE|ALTER|CONTROL SERVER|
|ALTER ANY ROUTE|ALTER|CONTROL SERVER|
|ALTER ANY SCHEMA|ALTER|CONTROL SERVER|
|MODIFIER UNE STRATÉGIE DE SÉCURITÉ<br /> **S'applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|CONTROL SERVER|
|ALTER ANY SERVICE|ALTER|CONTROL SERVER|
|ALTER ANY SYMMETRIC KEY|ALTER|CONTROL SERVER|
|ALTER ANY USER|ALTER|CONTROL SERVER|
|AUTHENTICATE|CONTROL|AUTHENTICATE SERVER|
|BACKUP DATABASE|CONTROL|CONTROL SERVER|
|BACKUP LOG|CONTROL|CONTROL SERVER|
|CHECKPOINT|CONTROL|CONTROL SERVER|
|CONNECT|CONNECT REPLICATION|CONTROL SERVER|
|CONNECT REPLICATION|CONTROL|CONTROL SERVER|
|CONTROL|CONTROL|CONTROL SERVER|
|CREATE AGGREGATE|ALTER|CONTROL SERVER|
|CREATE ANY EXTERNAL LIBRARY <br /> **S'applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].|CONTROL|CONTROL SERVER |
|CREATE ASSEMBLY|ALTER ANY ASSEMBLY|CONTROL SERVER|
|CREATE ASYMMETRIC KEY|ALTER ANY ASYMMETRIC KEY|CONTROL SERVER|
|CREATE CERTIFICATE|ALTER ANY CERTIFICATE|CONTROL SERVER|
|CREATE CONTRACT|ALTER ANY CONTRACT|CONTROL SERVER|
|CREATE DATABASE|CONTROL|CREATE ANY DATABASE|
|CREATE DATABASE DDL EVENT NOTIFICATION|ALTER ANY DATABASE EVENT NOTIFICATION|CREATE DDL EVENT NOTIFICATION|
|CREATE DEFAULT|ALTER|CONTROL SERVER|
|CREATE FULLTEXT CATALOG|ALTER ANY FULLTEXT CATALOG|CONTROL SERVER|
|CREATE FUNCTION|ALTER|CONTROL SERVER|
|CREATE MESSAGE TYPE|ALTER ANY MESSAGE TYPE|CONTROL SERVER|
|CREATE PROCEDURE|ALTER|CONTROL SERVER|
|CREATE QUEUE|ALTER|CONTROL SERVER|
|CREATE REMOTE SERVICE BINDING|ALTER ANY REMOTE SERVICE BINDING|CONTROL SERVER|
|CREATE ROLE|ALTER ANY ROLE|CONTROL SERVER|
|CREATE ROUTE|ALTER ANY ROUTE|CONTROL SERVER|
|CREATE RULE|ALTER|CONTROL SERVER|
|CREATE SCHEMA|ALTER ANY SCHEMA|CONTROL SERVER|
|CREATE SERVICE|ALTER ANY SERVICE|CONTROL SERVER|
|CREATE SYMMETRIC KEY|ALTER ANY SYMMETRIC KEY|CONTROL SERVER|
|CREATE SYNONYM|ALTER|CONTROL SERVER|
|CREATE TABLE|ALTER|CONTROL SERVER|
|CREATE TYPE|ALTER|CONTROL SERVER|
|CREATE VIEW|ALTER|CONTROL SERVER|
|CREATE XML SCHEMA COLLECTION|ALTER|CONTROL SERVER|
|Suppression|CONTROL|CONTROL SERVER|
|Exécutez|CONTROL|CONTROL SERVER|
|EXECUTE ANY EXTERNAL SCRIPT <br /> **S'applique à**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)].|CONTROL|CONTROL SERVER|
|EXECUTE EXTERNAL SCRIPT <br /> **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssqlv15-md.md)].|EXECUTE ANY EXTERNAL SCRIPT|CONTROL SERVER|
|INSERT|CONTROL|CONTROL SERVER|
|KILL DATABASE CONNECTION<br />**S'applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|ALTER ANY CONNECTION|
|REFERENCES|CONTROL|CONTROL SERVER|
|SELECT|CONTROL|CONTROL SERVER|
|SHOWPLAN|CONTROL|ALTER TRACE|
|SUBSCRIBE QUERY NOTIFICATIONS|CONTROL|CONTROL SERVER|
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|
|UNMASK|CONTROL|CONTROL SERVER|
|UPDATE|CONTROL|CONTROL SERVER|
|VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW ANY COLUMN MASTER KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW DATABASE STATE|CONTROL|VIEW SERVER STATE|
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|

## <a name="permissions"></a>Autorisations

Le fournisseur d'autorisations (ou le principal spécifié avec l'option AS) doit posséder l'autorisation elle-même avec l'option GRANT OPTION ou une autorisation plus élevée qui implique l'autorisation accordée.

Si vous utilisez l'option AS, les conditions supplémentaires ci-dessous s'appliquent.

|AS *granting_principal*|Autres autorisations nécessaires|
|------------------------------|------------------------------------|
|Utilisateur de base de données|Autorisation IMPERSONATE sur l'utilisateur, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|
|Utilisateur de base de données mappé à une connexion Windows|Autorisation IMPERSONATE sur l'utilisateur, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|
|Utilisateur de base de données mappé à un groupe Windows|Appartenance au groupe Windows, appartenance aux rôles de base de données fixe db_securityadmin ou db_owner ou appartenance au rôle serveur fixe sysadmin.|
|Utilisateur de base de données mappé à un certificat|Appartenance aux rôles de base de données fixe db_securityadmin ou db_owner ou appartenance au rôle serveur fixe sysadmin.|
|Utilisateur de base de données mappé à une clé asymétrique|Appartenance aux rôles de base de données fixe db_securityadmin ou db_owner ou appartenance au rôle serveur fixe sysadmin.|
|Utilisateur de base de données qui n'est mappé sur aucun principal d'un serveur|Autorisation IMPERSONATE sur l'utilisateur, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|
|Rôle de base de données|Autorisation ALTER sur le rôle, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|
|Rôle d'application|Autorisation ALTER sur le rôle, appartenance au rôle de base de données fixe db_securityadmin, appartenance au rôle de base de données fixe db_owner ou appartenance au rôle serveur fixe sysadmin.|

Les propriétaires d'objets peuvent accorder des autorisations sur les objets qu'ils possèdent. Les principaux qui possèdent l'autorisation CONTROL sur un élément sécurisable peuvent accorder une autorisation sur cet élément sécurisable.

Les détenteurs de l'autorisation CONTROL SERVER, tels que les membres du rôle serveur fixe sysadmin, peuvent accorder une autorisation sur n'importe quel élément sécurisable du serveur.

## <a name="examples"></a>Exemples

### <a name="a-granting-permission-to-create-tables"></a>R. Octroi d'une autorisation pour créer des tables

Dans l’exemple suivant, l’autorisation `CREATE TABLE` sur la base de données `AdventureWorks` est accordée à l’utilisateur `MelanieK`.

```sql
USE AdventureWorks;
GRANT CREATE TABLE TO MelanieK;
GO
```

### <a name="b-granting-showplan-permission-to-an-application-role"></a>B. Octroi d'une autorisation SHOWPLAN à un rôle d'application

 Dans l'exemple ci-dessous, l'autorisation `SHOWPLAN` sur la base de données `AdventureWorks2012` est accordée au rôle d'application `AuditMonitor`.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

```sql
USE AdventureWorks2012;
GRANT SHOWPLAN TO AuditMonitor;
GO
```

### <a name="c-granting-create-view-with-grant-option"></a>C. Octroi d'une autorisation CREATE VIEW avec l'option GRANT OPTION

 Dans l'exemple ci-dessous, l'autorisation `CREATE VIEW` sur la base de données `AdventureWorks2012` est accordée à l'utilisateur `CarmineEs` avec le droit d'accorder `CREATE VIEW` à d'autres principaux.

```sql
USE AdventureWorks2012;
GRANT CREATE VIEW TO CarmineEs WITH GRANT OPTION;
GO
```

### <a name="d-granting-control-permission-to-a-database-user"></a>D. Octroi d’une autorisation CONTROL à un utilisateur de base de données

 Dans l’exemple suivant, l’autorisation `CONTROL` sur la base de données `AdventureWorks2012` est accordée à l’utilisateur de base de données `Sarah`. Celui-ci doit exister dans la base de données ; par ailleurs, le contexte doit être défini sur la base de données.

```sql
USE AdventureWorks2012;
GRANT CONTROL ON DATABASE::AdventureWorks2012 TO Sarah;
GO
```

## <a name="see-also"></a>Voir aussi

- [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)
- [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [GRANT](../../t-sql/statements/grant-transact-sql.md)
- [autorisations](../../relational-databases/security/permissions-database-engine.md)
- [Principaux](../../relational-databases/security/authentication-access/principals-database-engine.md)
