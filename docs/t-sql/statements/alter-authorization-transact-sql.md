---
description: ALTER AUTHORIZATION (Transact-SQL)
title: ALTER AUTHORIZATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/01/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_AUTHORIZATION_TSQL
- ALTER AUTHORIZATION
dev_langs:
- TSQL
helpviewer_keywords:
- owners [SQL Server], transferring
- modifying entity ownership
- full-text search [SQL Server], permissions
- owners [SQL Server], entities
- ALTER AUTHORIZATION statement
- entity ownership [SQL Server]
- transferring ownership
- search property lists [SQL Server], permissions
- TAKE OWNERSHIP
ms.assetid: 8c805ae2-91ed-4133-96f6-9835c908f373
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 95a1787412fa84c30ac0b14d670f94c72af10d30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270504"
---
# <a name="alter-authorization-transact-sql"></a>ALTER AUTHORIZATION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Change la propriété d'un élément sécurisable.

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Syntax for SQL Server
ALTER AUTHORIZATION
    ON [ <class_type>:: ] entity_name
    TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::=
     {
      OBJECT | ASSEMBLY | ASYMMETRIC KEY | AVAILABILITY GROUP | CERTIFICATE
    | CONTRACT | TYPE | DATABASE | ENDPOINT | FULLTEXT CATALOG
    | FULLTEXT STOPLIST | MESSAGE TYPE | REMOTE SERVICE BINDING
    | ROLE | ROUTE | SCHEMA | SEARCH PROPERTY LIST | SERVER ROLE
    | SERVICE | SYMMETRIC KEY | XML SCHEMA COLLECTION
     }
```

```syntaxsql
-- Syntax for SQL Database

ALTER AUTHORIZATION
    ON [ <class_type>:: ] entity_name
    TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::=
     {
    OBJECT | ASSEMBLY | ASYMMETRIC KEY | CERTIFICATE
     | TYPE | DATABASE | FULLTEXT CATALOG
     | FULLTEXT STOPLIST
     | ROLE | SCHEMA | SEARCH PROPERTY LIST
     | SYMMETRIC KEY | XML SCHEMA COLLECTION
     }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics

ALTER AUTHORIZATION ON
     [ <class_type> :: ] <entity_name>
     TO { principal_name | SCHEMA OWNER }
    [;]

    <class_type> ::= {
    SCHEMA
     | OBJECT
    }

    <entity_name> ::=
    {
    schema_name
     | [ schema_name. ] object_name
    }
```

```syntaxsql
-- Syntax for Parallel Data Warehouse

ALTER AUTHORIZATION ON
     [ <class_type> :: ] <entity_name>
     TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::= {
    DATABASE
     | SCHEMA
     | OBJECT
    }

<entity_name> ::=
    {
    database_name
     | schema_name
     | [ schema_name. ] object_name
    }
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

\<class_type> Classe sécurisable de l'entité pour laquelle il y a un changement de propriétaire. OBJECT est la valeur par défaut.

|Classe|Produit|
|-|-|
|OBJECT|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].|
|ASSEMBLY|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|ASYMMETRIC KEY|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|AVAILABILITY GROUP |**S’APPLIQUE À** : SQL Server 2012 et versions ultérieures.|
|CERTIFICATE|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|CONTRACT|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.|
|DATABASE|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Pour plus d’informations, consultez [ALTER AUTHORIZATION pour les bases de données](#alter-authorization-for-databases).|
|ENDPOINT|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.|
|FULLTEXT CATALOG|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|FULLTEXT STOPLIST|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|MESSAGE TYPE|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.|
|REMOTE SERVICE BINDING|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.|
|ROLE|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|ROUTE|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.|
|SCHEMA|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].|
|SEARCH PROPERTY LIST|**S’APPLIQUE À** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|SERVER ROLE|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.|
|SERVICE|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.|
|SYMMETRIC KEY|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|TYPE|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|XML SCHEMA COLLECTION|**S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|

*entity_name* Nom de l’entité.

*principal_name* | SCHEMA OWNER Nom du principal de sécurité qui possède l’entité. Les objets de bases de données doivent appartenir à un principal de base de données, un utilisateur de base de données ou un rôle. Les objets serveur (par exemple les bases de données) doivent appartenir à un principal serveur (un identifiant de connexion). Spécifiez **SCHEMA OWNER** comme *principal_name- pour indiquer que l’objet doit appartenir au principal qui est propriétaire du schéma de l’objet.

## <a name="remarks"></a>Notes

ALTER AUTHORIZATION peut s'utiliser pour modifier la propriété d'une entité qui a un propriétaire. La propriété d'entités contenues dans une base de données peut être transférée à n'importe quel principal de niveau base de données. La propriété d'entités de niveau serveur peut être transférée à n'importe quel principal de niveau serveur.

> [!IMPORTANT]
> À partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], un utilisateur peut être propriétaire d'un OBJECT ou d'un TYPE contenu dans un schéma appartenant à un autre utilisateur de base de données. Il s'agit là d'une différence par rapport aux versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md) et [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md).

La propriété des entités suivantes, contenues dans un schéma et de type « objet » peut être transférée : tables, vues, fonctions, procédures, files d'attente et synonymes.

La propriété des entités suivantes ne peut pas être transférée : serveurs liés, statistiques, contraintes, règles, valeurs par défaut, déclencheurs, files d'attente [!INCLUDE[ssSB](../../includes/sssb-md.md)], informations d'identification, fonctions de partition, schémas de partition, clés principales de base de données, clé principale de service et notifications d'événements.

La propriété des membres des classes sécurisables suivantes ne peut pas être transférée : serveur, connexion, utilisateur, rôle d'application et colonne.

L'option SCHEMA OWNER n'est valide que lorsque vous transférez la propriété d'une entité contenue dans un schéma. SCHEMA OWNER transférera la propriété de l'entité au propriétaire du schéma dans lequel elle réside. Seules les entités des classes OBJECT, TYPE ou XML SCHEMA COLLECTION sont contenues dans des schémas.

Si l'entité cible n'est pas une base de données et que l'entité est transférée à un nouveau propriétaire, toutes les autorisations de la cible seront supprimées.

> [!CAUTION]
> Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], le comportement des schémas n'est pas le même que dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un code qui suppose que les schémas sont équivalents aux utilisateurs de base de données peut retourner des résultats incorrects. Vous ne devez pas recourir aux anciens affichages catalogue, notamment sysobjects, dans une base de données où une des instructions DDL suivantes a été utilisée : CREATE SCHEMA, ALTER SCHEMA, DROP SCHEMA, CREATE USER, ALTER USER, DROP USER, CREATE ROLE, ALTER ROLE, DROP ROLE, CREATE APPROLE, ALTER APPROLE, DROP APPROLE, ALTER AUTHORIZATION. Dans une base de données où une de ces instructions a été utilisée, vous devez recourir aux nouveaux affichages catalogue. Les nouvelles vues de catalogue prennent en compte la séparation des principaux et des schémas introduite dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Pour plus d’informations sur les affichages catalogue, consultez [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md).

 En outre, notez les points suivants :

> [!IMPORTANT]
> La seule méthode fiable pour rechercher le propriétaire d’un objet est d’interroger la vue de catalogue **sys.objects**. La seule méthode fiable pour rechercher le propriétaire d'un type est d'utiliser la fonction TYPEPROPERTY.

## <a name="special-cases-and-conditions"></a>Cas particuliers et conditions

Le tableau suivant récapitule les cas particuliers, les exceptions et les conditions qui s'appliquent à l'autorisation de modification.

|Classe|Condition|
|-----------|---------------|
|OBJECT|Impossible de modifier la propriété des déclencheurs, des contraintes, des règles, des valeurs par défaut, des statistiques, des objets système, des files d'attente, des vues indexées ou des tables avec des vues indexées.|
|SCHEMA|Lors d'un transfert de propriété, les autorisations sur des objets contenus dans des schémas n'ayant pas de propriétaires explicites seront supprimées. Impossible de modifier le propriétaire de sys, dbo ou information_schema.|
|TYPE|Impossible de modifier le propriétaire d'un TYPE qui appartient à sys ou information_schema.|
|CONTRACT, MESSAGE TYPE ou SERVICE|Impossible de modifier la propriété des entités système.|
|SYMMETRIC KEY|Impossible de modifier la propriété des clés temporaires globales.|
|CERTIFICATE ou ASYMMETRIC KEY|Impossible de transférer la propriété de ces entités vers un rôle ou un groupe.|
|ENDPOINT|Le principal doit être une connexion.|

## <a name="alter-authorization-for-databases"></a> ALTER AUTHORIZATION pour les bases de données

### <a name="for-sql-server"></a>Pour SQL Server

**Conditions requises pour le nouveau propriétaire :** Le nouveau principal de propriétaire doit être l’un des éléments suivants :

- Un compte de connexion d’authentification SQL Server.
- Un compte de connexion d’authentification Windows représentant un utilisateur Windows (et non un groupe).
- Un utilisateur Windows qui s’authentifie via un compte de connexion d’authentification Windows représentant un groupe Windows.

**Configuration requise pour la personne qui exécute l’instruction ALTER AUTHORIZATION :** Si vous n’êtes pas membre du rôle serveur fixe **sysadmin**, vous devez disposer au moins de l’autorisation TAKE OWNERSHIP sur la base de données et vous devez disposer de l’autorisation IMPERSONATE sur le nouveau compte de connexion de propriétaire.

### <a name="for-azure-sql-database"></a>Pour Azure SQL Database

**Conditions requises pour le nouveau propriétaire :** Le nouveau principal de propriétaire doit être l’un des éléments suivants :

- Un compte de connexion d’authentification SQL Server.
- Un utilisateur fédéré (et pas un groupe) présent dans Azure AD.
- Un utilisateur géré (et pas un groupe) ou une application présents dans Azure AD.

Si le nouveau propriétaire est un utilisateur Azure Active Directory, il ne peut pas exister comme utilisateur dans la base de données dans laquelle le nouveau propriétaire sera le nouveau propriétaire de la base de données (DBO). Un tel utilisateur Azure AD doit tout d’abord être supprimé de la base de données avant d’exécuter l’instruction ALTER AUTHORIZATION qui affecte la propriété de la base de données au nouvel utilisateur. Pour plus d’informations sur la configuration d’utilisateur Azure AD avec SQL Database, consultez [Connexion à SQL Database ou [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] en utilisant l’authentification Azure AD](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/).

**Configuration requise pour la personne qui exécute l’instruction ALTER AUTHORIZATION :** Vous devez vous connecter à la base de données cible pour changer le propriétaire de cette base de données.

Les types de comptes suivants peuvent changer le propriétaire d’une base de données.

- Le compte de connexion au principal au niveau du service. (L’administrateur SQL Azure provisionné au moment de la création du serveur SQL Database.)
- L’administrateur Azure Active Directory pour Azure SQL Server.
- Le propriétaire actuel de la base de données.

Le tableau suivant récapitule les conditions requises :

Exécuteur  |Cible  |Résultats
---------|---------|---------
Connexion d’authentification SQL Server  |Connexion d’authentification SQL Server|Succès
Connexion d’authentification SQL Server  |Utilisateur Azure AD|Échec
Utilisateur Azure AD  |Connexion d’authentification SQL Server|Succès
Utilisateur Azure AD  |Utilisateur Azure AD|Succès

Pour vérifier un propriétaire Azure AD de la base de données, exécutez la commande Transact-SQL suivante dans une base de données utilisateur (`testdb` dans cet exemple).

```sql
SELECT CAST(owner_sid as uniqueidentifier) AS Owner_SID
FROM sys.databases
WHERE name = 'testdb';
```

La sortie est un identificateur (par exemple 6D8B81F6-7C79-444C-8858-4AF896C03C67) qui correspond à l’ObjectID Azure AD assigné à `richel@cqclinic.onmicrosoft.com`. Quand un utilisateur de connexion d’authentification SQL Server est le propriétaire de la base de données, exécutez l’instruction suivante dans la base de données MASTER pour vérifier le propriétaire de la base de données :

```sql
SELECT d.name, d.owner_sid, sl.name
FROM sys.databases AS d
JOIN sys.sql_logins AS sl
ON d.owner_sid = sl.sid;

```

### <a name="best-practice"></a>Bonne pratique

Au lieu d’utiliser des utilisateurs Azure AD comme propriétaires individuels de la base de données, utilisez un groupe Azure AD comme membre du rôle de base de données fixe **db_owner**. Les étapes suivantes montrent comment configurer un compte de connexion désactivé comme propriétaire de la base de données et rendre un groupe Azure Active Directory (`mydbogroup`) membre du rôle **db_owner**.

1. Connectez-vous à SQL Server comme administrateur Azure AD, puis remplacez le propriétaire de la base de données par un compte de connexion d’authentification SQL Server désactivé. Par exemple, à partir de la base de données utilisateur, exécutez :

   ```sql
   ALTER AUTHORIZATION ON database::testdb TO DisabledLogin;
   ```

1. Créez un groupe Azure AD qui doit être propriétaire de la base de données, puis ajoutez-le comme utilisateur à la base de données utilisateur. Par exemple :

   ```sql
   CREATE USER [mydbogroup] FROM EXTERNAL PROVIDER;
   ```

1. Dans la base de données utilisateur, ajoutez l’utilisateur qui représente le groupe Azure AD au rôle de base de données fixe **db_owner**. Par exemple :

   ```sql
   ALTER ROLE db_owner ADD MEMBER mydbogroup;
   ```

Les membres de `mydbogroup` peuvent maintenant gérer de manière centralisée la base de données comme des membres du rôle **db_owner**.

- Quand des membres de ce groupe sont supprimés du groupe Azure AD, ils perdent automatiquement les autorisations dbo pour cette base de données.
- Même si de nouveaux membres sont ajoutés au groupe Azure AD `mydbogroup`, ils obtiennent automatiquement un accès dbo pour cette base de données.

Pour vérifier si un utilisateur spécifique dispose de l’autorisation dbo effective, demandez-lui d’exécuter l’instruction suivante :

```sql
SELECT IS_MEMBER ('db_owner');
```

La valeur de retour 1 indique que l’utilisateur est membre du rôle.

## <a name="permissions"></a>Autorisations

Il faut obligatoirement l'autorisation TAKE OWNERSHIP sur l'entité. Si le nouveau propriétaire n'est pas l'utilisateur qui exécute cette instruction, il faut aussi soit 1) l'autorisation IMPERSONATE sur le nouveau propriétaire s'il s'agit d'un utilisateur ou d'une connexion, soit 2) si le nouveau propriétaire est un rôle, l'appartenance à ce rôle ou l'autorisation ALTER sur le rôle, soit 3) si le nouveau propriétaire est un rôle d'application, l'autorisation ALTER sur le rôle d'application.

## <a name="examples"></a>Exemples

### <a name="a-transfer-ownership-of-a-table"></a>R. Transfert de la propriété d'une table

L'exemple suivant transfère la propriété de la table `Sprockets` à l'utilisateur `MichikoOsada`. La table se trouve dans le schéma `Parts`.

```sql
ALTER AUTHORIZATION ON OBJECT::Parts.Sprockets TO MichikoOsada;
GO
```

La requête peut aussi se présenter de la façon suivante :

```sql
ALTER AUTHORIZATION ON Parts.Sprockets TO MichikoOsada;
GO
```

Si le schéma d’objets n’est pas inclus en même temps que l’instruction, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] recherche l’objet dans le schéma par défaut des utilisateurs. Par exemple :

```sql
ALTER AUTHORIZATION ON Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::Sprockets TO MichikoOsada;
```

### <a name="b-transfer-ownership-of-a-view-to-the-schema-owner"></a>B. Transfert de la propriété d'une vue au propriétaire du schéma

L'exemple suivant transfère la propriété de la vue `ProductionView06` au propriétaire du schéma qui la contient. La vue se trouve dans le schéma `Production`.

```sql
ALTER AUTHORIZATION ON OBJECT::Production.ProductionView06 TO SCHEMA OWNER;
GO
```

### <a name="c-transfer-ownership-of-a-schema-to-a-user"></a>C. Transfert de la propriété d'un schéma à un utilisateur

L'exemple suivant transfère la propriété du schéma `SeattleProduction11` à l'utilisateur `SandraAlayo`.

```sql
ALTER AUTHORIZATION ON SCHEMA::SeattleProduction11 TO SandraAlayo;
GO
```

### <a name="d-transfer-ownership-of-an-endpoint-to-a-sql-server-login"></a>D. Transfert de la propriété d'un point de terminaison à un compte de connexion SQL Server

L'exemple suivant transfère la propriété du point de terminaison `CantabSalesServer1` à `JaePak`. Comme le point de terminaison est un élément sécurisable au niveau serveur, il ne peut être transféré qu'à un principal au niveau du serveur.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.

```sql
ALTER AUTHORIZATION ON ENDPOINT::CantabSalesServer1 TO JaePak;
GO
```

### <a name="e-changing-the-owner-of-a-table"></a>E. Changement du propriétaire d’une table

Chacun des exemples suivants remplace le propriétaire de la table `Sprockets` dans la base de données `Parts` par l’utilisateur de base de données `MichikoOsada`.

```sql
ALTER AUTHORIZATION ON Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON dbo.Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::dbo.Sprockets TO MichikoOsada;
```

### <a name="f-changing-the-owner-of-a-database"></a>F. Changement du propriétaire d’une base de données

 **S’APPLIQUE À** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].

 L’exemple suivant remplace le propriétaire de la base de données `Parts` par le compte de connexion `MichikoOsada`.

```sql
ALTER AUTHORIZATION ON DATABASE::Parts TO MichikoOsada;
```

### <a name="g-changing-the-owner-of-a-sql-database-to-an-azure-ad-user"></a>G. Remplacement du propriétaire d’une base de données SQL par un utilisateur Azure AD

Dans l’exemple suivant, un administrateur Azure Active Directory pour SQL Server d’une organisation avec un annuaire actif nommé `cqclinic.onmicrosoft.com` peut changer la propriété actuelle d’une base de données `targetDB` et nommer un utilisateur AAD `richel@cqclinic.onmicorsoft.com` nouveau propriétaire de la base de données à l’aide de la commande suivante :

```sql
ALTER AUTHORIZATION ON database::targetDB TO [rachel@cqclinic.onmicrosoft.com];
```

Azure AD requiert des crochets `[]` autour du nom d’utilisateur.

## <a name="see-also"></a>Voir aussi
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md) [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md) [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)

