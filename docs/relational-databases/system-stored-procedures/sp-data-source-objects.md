---
description: sp_data_source_objects (Transact-SQL)
title: sp_data_source_objects | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_objects
helpviewer_keywords:
- PolyBase
ms.assetid: 48066431-fed2-4a8a-85af-ac704689e183
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4c76026dcd24572b4d52c6b82246e5293c22e713
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596688"
---
# <a name="sp_data_source_objects-transact-sql"></a>sp_data_source_objects (Transact-SQL)

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Retourne la liste des objets de table qui sont disponibles pour la virtualisation.

> [!NOTE]
> Cette procédure est présentée dans [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5).

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntaxe  

```syntaxsql
sp_data_source_objects  
         [ @data_source = ] 'data_source'
     [ , [ @object_root_name = ] 'object_root_name' ]
     [ , [ @max_search_depth = ] max_search_depth ]
     [ , [ @search_options = ] 'search_options' ]
[ ; ]
```  
  
## <a name="arguments"></a>Arguments  

`[ @data_source = ] 'data_source'`   
Nom de la source de données externe à partir de laquelle récupérer les métadonnées. `@data_source` a la valeur `sysname`.  

`[ @object_root_name = ] 'object_root_name'`   
Ce paramètre est la racine du nom du ou des objets à rechercher. `@object_root_name` est `nvarchar(max)` de, avec la valeur par défaut `NULL` .

Cet appel retourne uniquement les objets externes qui commencent par la valeur définie pour `@object_root_name` .

Si une source de données ODBC se connecte à un système de gestion de base de données relationnelle (SGBDR) qui utilise des noms en trois parties, `@object_root_name` ne peut pas contenir un nom de base de données partiel. Dans ce cas, le paramètre `@object_root_name` doit contenir les trois parties, la troisième étant le nom de l’objet à rechercher.
> [!CAUTION]
> En raison des différences entre les plateformes de données externes, certaines plateformes ne retournent pas de résultats si la valeur par défaut de `NULL` est fournie. Certains considèrent `NULL` comme un manque de filtre. Par exemple, Oracle RDMBS ne retourne pas de résultats si `NULL` est fourni pour `@object_root_name` .

`[ @max_search_depth = ] max_search_depth`   
Cette valeur spécifie la profondeur maximale (en parties) au-delà du `@object_root_name` que nous souhaitons Rechercher. `@max_search_depth` est `int` de avec 1 comme valeur par défaut.

Par exemple, un `@max_search_depth` de 1, avec un `@object_root_name` qui est le nom d’une base de données SQL Server, retourne schemata contenu dans la base de données.

Un `@max_search_depth` de `NULL` retourne des informations sur `@object_root_name` s’il existe et n’est pas vide, dans le cas d’un catalogue ou d’un schéma.

`[ @search_options = ] 'search_options'`   
Le `search_options` paramètre est de type nvarchar (max) avec la valeur par défaut `NULL` .

Ce paramètre n’est pas utilisé, mais il peut être implémenté à l’avenir.

## <a name="result-sets"></a>Jeux de résultats

| Nom de la colonne | Type de données | Description |
|--|--|--|
| Object_Type | nvarchar(200) | Type de l’objet (exemple : TABLE ou base de données). |
| OBJECT_NAME | nvarchar(max) | Nom qualifié complet de l’objet. Placé dans une séquence d’échappement à l’aide d’un guillemet spécifique au backend. |
| OBJECT_LEAF_NAME | nvarchar(max) | Nom de l’objet non qualifié. |
| TABLE_LOCATION | nvarchar(max) | Chaîne d’emplacement de table valide qui peut être utilisée pour une instruction CREATe EXTERNAL TABLE. Est `NULL` s’il n’est pas applicable. |
  
## <a name="permissions"></a>Autorisations

Exige l’autorisation ALTER ANY EXTERNAL DATA SOURCE.  

## <a name="remarks"></a>Notes  

La fonctionnalité  [Polybase](../../relational-databases/polybase/polybase-guide.md) doit être installée sur l’instance SQL Server.

Cette procédure stockée prend en charge les connecteurs pour :

- SQL Server
- Oracle
- Teradata
- MongoDB
- CosmosDB

La procédure stockée ne prend pas en charge les connecteurs sources ODBC DTA génériques.

La notion de Empty et non Empty est associée au comportement du pilote ODBC et à la [ `SQLTables` fonction](../native-client-odbc-api/sqltables.md). Non vide indique qu’un objet contient des tables, et non des lignes. Par exemple, un schéma vide ne contient aucune table dans SQL Server. Une base de données vide contient sans table dans Teradata.

Les types d’objets sont déterminés par le pilote ODBC de la source de données externe. Chaque source de données externe détermine ce qui est qualifié de table. Cela peut inclure des objets de base de données tels que des fonctions dans TeraData ou des synonymes dans Oracle. Polybase ne peut pas se connecter à certains objets ODBC en tant que tables externes et n’a donc pas de valeur dans la colonne TABLE_LOCATION. En dépit de cela, la présence de l’un de ces objets peut rendre une base de données ou un schéma non vide.

Utilisez `sp_data_source_objects` et [`sp_data_source_table_columns`](sp-data-source-table-columns.md) pour découvrir des objets externes. Ces procédures stockées système retournent le schéma des tables disponibles pour la virtualisation. Azure Data Studio utilise ces deux procédures stockées pour prendre en charge [la virtualisation des données](../../azure-data-studio/extensions/data-virtualization-extension.md). Utilisez [sp_data_source_table_columns](sp-data-source-table-columns.md) pour détecter les schémas de table externes représentés dans des types de données SQL Server.

### <a name="data-source-type-specific-remarks"></a>Remarques spécifiques relatives au type de source de données

* Teradata

   Les vues système Teradata n’utilisent pas la sécurité au niveau des lignes (RLS), et les utilisateurs peuvent ainsi voir l’existence de tables qu’elles ne peuvent pas interroger.

* MongoDB

   Certaines versions antérieures de MongoDB restreignent la possibilité de répertorier toutes les bases de données pour les utilisateurs de type administrateur. Les utilisateurs sans cette autorisation peuvent obtenir des erreurs d’authentification tentant d’exécuter cette procédure avec une valeur null `@object_root_name` .

## <a name="examples"></a>Exemples  

### <a name="sql-server"></a>SQL Server

L’exemple suivant retourne toutes les bases de données, schemata et tables/vues

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 3;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | database | NULL |
| SCHEMA | « base de données ». dbo | dbo | NULL |
| TABLE | « base de données ». dbo « . » assistance | client | [base de données]. [dbo]. assistance |
| TABLE | « base de données ». dbo « . » fiche | item | [base de données]. [dbo]. fiche |
| TABLE | « base de données ». dbo « . » Observateur | Observateur | [base de données]. [dbo]. Observateur |

L’exemple suivant retourne toutes les bases de données

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "UserDatabase" | UserDatabase | NULL |
| DATABASE | « master » | master | NULL |
| DATABASE | msdb | msdb | NULL |
| DATABASE | temporaire | tempdb | NULL |
| DATABASE | "database" | database | NULL |

L’exemple suivant retourne tous les schemata d’une base de données

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| SCHEMA | « base de données ». dbo | dbo | NULL |
| SCHEMA | « base de données ». INFORMATION_SCHEMA» | INFORMATION_SCHEMA | NULL |
| SCHEMA | « base de données ». table | sys | NULL |

L’exemple suivant retourne toutes les tables du schéma 

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database].[dbo]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| TABLE | « base de données ». dbo « . » assistance | client | [base de données]. [dbo]. assistance |
| TABLE | « base de données ». dbo « . » fiche | item | [base de données]. [dbo]. fiche |
| TABLE | « base de données ». dbo « . » Observateur | Observateur | [base de données]. [dbo]. Observateur |
| TABLE | « base de données ». dbo « . » o.f. | orders | [base de données]. [dbo]. o.f. |
| TABLE | « base de données ». dbo « . » étape | étape | [base de données]. [dbo]. étape |

### <a name="oracle"></a>Oracle

L’exemple suivant retourne la schemata complète, les tables, les fonctions, les vues, etc.

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[OracleObjectRoot]'; 
DECLARE @max_search_depth INT = 2; 
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| VIEW | « SYS ». ALL_SQLSET_STATEMENTS» | ALL_SQLSET_STATEMENTS | [ORACLEOBJECTROOT]. [SYS]. [ALL_SQLSET_STATEMENTS] |
| SYSTEM TABLE | « SYS ». BOOTSTRAP $ " | BOOTSTRAP $ | [ORACLEOBJECTROOT]. [SYS]. [BOOTSTRAP $] |
| SYNONYM | « PUBLIC ».» ALL_ALL_TABLES» | ALL_ALL_TABLES | NULL |
| SCHEMA | "database" | database | NULL |
| TABLE | « base de données ». assistance | client | [ORACLEOBJECTROOT]. [base de données]. assistance |

### <a name="teradata"></a>Teradata

L’exemple suivant retourne toutes les bases de données, les tables, les fonctions, les vues, etc.

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |  
|--|--|--|--|
| FUNCTION | "SYSLIB"." ExtractRoles" | ExtractRoles | NULL |  
| SYSTEM TABLE | « DBC ».» UDTCast" | UDTCast | [DBC]. [UDTCast] |  
| TYPE | "SYSUDTLIB"." LANGAGE | XML | NULL |  
| DATABASE | "database" | database | NULL |
| TABLE | « base de données ». assistance | client | [base de données]. assistance |  

### <a name="mongo-db"></a>Mongo DB

L’exemple suivant retourne toutes les bases de données et tables.

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | database | NULL |
| TABLE | « base de données ». assistance | client | [base de données]. assistance |
| TABLE | « base de données ». fiche | item | [base de données]. fiche |
| TABLE | « base de données ». Observateur | Observateur | [base de données]. Observateur |
| TABLE | « base de données ». o.f. | orders | [base de données]. o.f. |

## <a name="see-also"></a>Voir aussi

- [sp_data_source_columns](./sp-data-source-table-columns.md)   
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)
- [Extension de virtualisation de données pour Azure Data Studio](../../azure-data-studio/extensions/data-virtualization-extension.md)   
- [Prise en main de PolyBase](../polybase/polybase-guide.md)   
- [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)