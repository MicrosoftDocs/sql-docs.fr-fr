---
description: CREATE TABLE (Transact-SQL)
title: CREATE TABLE (Transact-SQL)
ms.custom: ''
ms.date: 09/22/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILESTREAM_TSQL
- TABLE
- CREATE_TABLE_TSQL
- CREATE TABLE
- FILESTREAM
- TABLE_TSQL
- FILESTREAM_ON
- FILESTREAM_ON_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHECK constraints
- global temporary tables [SQL Server]
- local temporary tables [SQL Server]
- size [SQL Server], tables
- UNIQUE constraints [SQL Server], creating
- constraints [SQL Server], columns
- maximum number of indexes per table
- number of tables per database
- number of indexes per table
- table creation [SQL Server], CREATE TABLE
- temporary tables [SQL Server], creating
- table size [SQL Server]
- nullability [SQL Server]
- partitioned tables [SQL Server], creating
- DEFAULT definition
- null values [SQL Server], columns
- number of rows per table
- maximum number of columns per table
- FOREIGN KEY constraints, creating
- PRIMARY KEY constraints
- number of bytes per row
- CREATE TABLE statement
- number of columns per table
- maximum number of bytes per row
- data retention policy
ms.assetid: 1e068443-b9ea-486a-804f-ce7b6e048e8b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cbbf7efce0a0aeac175bc98063323b5f4b7a325a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237329"
---
# <a name="create-table-transact-sql"></a>CREATE TABLE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crée une table dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

> [!NOTE]
> Pour la syntaxe [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], consultez [CREATE TABLE ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/statements/create-table-azure-sql-data-warehouse.md).

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="simple-syntax"></a>Syntaxe simple

```syntaxsql
-- Simple CREATE TABLE Syntax (common if not using options)
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition> } [ ,...n ] )
[ ; ]
```

## <a name="full-syntax"></a>Syntaxe complète

```syntaxsql
-- Disk-Based CREATE TABLE Syntax
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ AS FileTable ]
    ( {   <column_definition>
        | <computed_column_definition>
        | <column_set_definition>
        | [ <table_constraint> ] [ ,... n ]
        | [ <table_index> ] }
          [ ,...n ]
          [ PERIOD FOR SYSTEM_TIME ( system_start_time_column_name
             , system_end_time_column_name ) ]
      )
    [ ON { partition_scheme_name ( partition_column_name )
           | filegroup
           | "default" } ]
    [ TEXTIMAGE_ON { filegroup | "default" } ]
    [ FILESTREAM_ON { partition_scheme_name
           | filegroup
           | "default" } ]
    [ WITH ( <table_option> [ ,...n ] ) ]
[ ; ]
  
<column_definition> ::=
column_name <data_type>
    [ FILESTREAM ]
    [ COLLATE collation_name ]
    [ SPARSE ]
    [ MASKED WITH ( FUNCTION = ' mask_function ') ]
    [ [ CONSTRAINT constraint_name ] DEFAULT constant_expression ]
    [ IDENTITY [ ( seed,increment ) ]
    [ NOT FOR REPLICATION ]
    [ GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] ]
    [ NULL | NOT NULL ]
    [ ROWGUIDCOL ]
    [ ENCRYPTED WITH
        ( COLUMN_ENCRYPTION_KEY = key_name ,
          ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } ,
          ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ) ]
    [ <column_constraint> [, ...n ] ]
    [ <column_index> ]
  
<data_type> ::=
[ type_schema_name . ] type_name
    [ ( precision [ , scale ] | max |
        [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]
  
<column_constraint> ::=
[ CONSTRAINT constraint_name ]
{     { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        [
            WITH FILLFACTOR = fillfactor
          | WITH ( < index_option > [ , ...n ] )
        ]
        [ ON { partition_scheme_name ( partition_column_name )
            | filegroup | "default" } ]
  
  | [ FOREIGN KEY ]
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
  
  | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
}
  
<column_index> ::=
 INDEX index_name [ CLUSTERED | NONCLUSTERED ]
    [ WITH ( <index_option> [ ,... n ] ) ]
    [ ON { partition_scheme_name (column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
<computed_column_definition> ::=
column_name AS computed_column_expression
[ PERSISTED [ NOT NULL ] ]
[
    [ CONSTRAINT constraint_name ]
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        [
            WITH FILLFACTOR = fillfactor
          | WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name ( partition_column_name )
        | filegroup | "default" } ]
  
    | [ FOREIGN KEY ]
        REFERENCES referenced_table_name [ ( ref_column ) ]
        [ ON DELETE { NO ACTION | CASCADE } ]
        [ ON UPDATE { NO ACTION } ]
        [ NOT FOR REPLICATION ]
  
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
]
  
<column_set_definition> ::=
column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS
  
< table_constraint > ::=
[ CONSTRAINT constraint_name ]
{
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        (column [ ASC | DESC ] [ ,...n ] )
        [
            WITH FILLFACTOR = fillfactor
           |WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name (partition_column_name)
            | filegroup | "default" } ]
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )

< table_index > ::=
{  
    {  
      INDEX index_name  [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ]
         (column_name [ ASC | DESC ] [ ,... n ] )
    | INDEX index_name CLUSTERED COLUMNSTORE
    | INDEX index_name [ NONCLUSTERED ] COLUMNSTORE (column_name [ ,... n ] )
    }
    [ WITH ( <index_option> [ ,... n ] ) ]
    [ ON { partition_scheme_name (column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
}

<table_option> ::=
{  
    [DATA_COMPRESSION = { NONE | ROW | PAGE }
      [ ON PARTITIONS ( { <partition_number_expression> | <range> }
      [ , ...n ] ) ]]
    [ FILETABLE_DIRECTORY = <directory_name> ]
    [ FILETABLE_COLLATE_FILENAME = { <collation_name> | database_default } ]
    [ FILETABLE_PRIMARY_KEY_CONSTRAINT_NAME = <constraint_name> ]
    [ FILETABLE_STREAMID_UNIQUE_CONSTRAINT_NAME = <constraint_name> ]
    [ FILETABLE_FULLPATH_UNIQUE_CONSTRAINT_NAME = <constraint_name> ]
    [ SYSTEM_VERSIONING = ON [ ( HISTORY_TABLE = schema_name . history_table_name
        [, DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ] ]
    [ REMOTE_DATA_ARCHIVE =
      {
          ON [ ( <table_stretch_options> [,...n] ) ]
        | OFF ( MIGRATION_STATE = PAUSED )
      }
    ]   
    [ DATA_DELETION = ON  
          {( 
             FILTER_COLUMN = column_name,   
             RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS 
                              | MONTH | MONTHS | YEAR | YEARS }
        )}  
     ]
}
  
<table_stretch_options> ::=
{  
    [ FILTER_PREDICATE = { null | table_predicate_function } , ]
      MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }
 }   
  
<index_option> ::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | STATISTICS_INCREMENTAL = { ON | OFF }
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS = { ON | OFF }
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
  | COMPRESSION_DELAY= {0 | delay [Minutes]}
  | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }
       [ ON PARTITIONS ( { <partition_number_expression> | <range> }
       [ , ...n ] ) ]
}
<range> ::=
<partition_number_expression> TO <partition_number_expression>
```

```
-- Memory optimized CREATE TABLE Syntax
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition>
    | [ <table_constraint> ] [ ,... n ]
    | [ <table_index> ]
      [ ,... n ] }
      [ PERIOD FOR SYSTEM_TIME ( system_start_time_column_name
        , system_end_time_column_name ) ]
)
    [ WITH ( <table_option> [ ,... n ] ) ]
 [ ; ]

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] ]
    [ NULL | NOT NULL ]
[
    [ CONSTRAINT constraint_name ] DEFAULT memory_optimized_constant_expression ]
    | [ IDENTITY [ ( 1, 1 ) ]
]
    [ <column_constraint> ]
    [ <column_index> ]
  
<data_type> ::=
 [type_schema_name . ] type_name [ (precision [ , scale ]) ]

<column_constraint> ::=
 [ CONSTRAINT constraint_name ]
{
  { PRIMARY KEY | UNIQUE }
      {   NONCLUSTERED
        | NONCLUSTERED HASH WITH (BUCKET_COUNT = bucket_count)
      }
  | [ FOREIGN KEY ]
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]
  | CHECK ( logical_expression )
}
  
< table_constraint > ::=
 [ CONSTRAINT constraint_name ]
{
   { PRIMARY KEY | UNIQUE }
     {
       NONCLUSTERED (column [ ASC | DESC ] [ ,... n ])
       | NONCLUSTERED HASH (column [ ,... n ] ) WITH ( BUCKET_COUNT = bucket_count )
                    }
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
    | CHECK ( logical_expression )
}

<column_index> ::=
  INDEX index_name
{ [ NONCLUSTERED ] | [ NONCLUSTERED ] HASH WITH (BUCKET_COUNT = bucket_count)}

<table_index> ::=
  INDEX index_name
{   [ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count)
  | [ NONCLUSTERED ] (column [ ASC | DESC ] [ ,... n ] )
      [ ON filegroup_name | default ]
  | CLUSTERED COLUMNSTORE [WITH ( COMPRESSION_DELAY = {0 | delay [Minutes]})]
      [ ON filegroup_name | default ]
  
}
  
<table_option> ::=
{  
    MEMORY_OPTIMIZED = ON
  | DURABILITY = {SCHEMA_ONLY | SCHEMA_AND_DATA}
  | SYSTEM_VERSIONING = ON [ ( HISTORY_TABLE = schema_name . history_table_name
        [, DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ]
  
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*database_name* correspond au nom de la base de données dans laquelle la table est créée. *database_name* doit spécifier le nom d’une base de données existante. Si aucun nom n’est spécifié, *database_name* correspond par défaut à la base de données actuelle. Le nom d’accès de la connexion actuelle doit être associé à un ID d’utilisateur existant dans la base de données spécifiée par *database_name*, et cet ID d’utilisateur doit disposer des autorisations CREATE TABLE.

*schema_name* correspond au nom du schéma auquel appartient la nouvelle table.

*table_name* correspond au nom de la nouvelle table. Les noms de tables doivent respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md). *table_name* peut comprendre un maximum de 128 caractères, à l’exception des noms de tables temporaires locales (noms précédés du signe #) qui ne peuvent pas dépasser 116 caractères.

AS FileTable **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures).

Crée la table en tant que FileTable. Vous ne spécifiez pas de colonnes car un FileTable dispose d'un schéma fixe. Pour plus d’informations, consultez [FileTables](../../relational-databases/blob/filetables-sql-server.md).

*column_name*
*computed_column_expression* est une expression définissant la valeur d’une colonne calculée. Une colonne calculée est une colonne virtuelle qui n'est pas stockée physiquement dans la table, à moins que la colonne ne soit indiquée comme PERSISTED. La colonne est calculée à partir d'une expression qui utilise d'autres colonnes dans la même table. Par exemple, une colonne calculée peut avoir la définition : **cost** AS **price** \* **qty**. L’expression peut être un nom de colonne non calculée, une constante, une fonction, une variable et toute combinaison de ces éléments reliés par un ou plusieurs opérateurs. L'expression ne peut pas être une sous-requête ou contenir des types de données d'alias.

Les colonnes calculées peuvent être utilisées dans des listes de sélection, des clauses WHERE, des clauses ORDER BY ou à tout autre emplacement où il est possible d'utiliser des expressions régulières, aux exceptions suivantes près :

- Les colonnes calculées doivent être marquées comme PERSISTED pour participer à des contraintes FOREIGN KEY ou CHECK.
- Une colonne calculée peut être utilisée en tant que colonne clé dans un index ou en tant que composante d’une contrainte PRIMARY KEY ou UNIQUE quelconque, si sa valeur est définie par une expression déterministe et si le type de données du résultat est autorisé dans les colonnes d’index.

   Par exemple, si la table possède les colonnes de type entier **a** et **b**, la colonne calculée **a+b** peut être indexée, contrairement à la colonne calculée **a+DATEPART(dd, GETDATE())** dont la valeur est susceptible d’évoluer au fil des appels.

- Une colonne calculée ne peut pas être la cible d'une instruction INSERT ou UPDATE.

> [!NOTE]
> Chaque ligne dans une table peut avoir des valeurs différentes pour les colonnes impliquées dans une colonne calculée ; par conséquent, il est possible que la colonne calculée n'ait pas la même valeur pour chaque ligne.

En fonction des expressions utilisées, la possibilité de valeurs NULL dans les colonnes calculées est déterminée automatiquement par le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Le résultat de la plupart des expressions est considéré comme pouvant avoir la valeur Null, même si seules des colonnes n'acceptant pas cette valeur sont présentes, car des dépassements négatifs ou positifs possibles produisent également des résultats Null. Utilisez la fonction `COLUMNPROPERTY` avec la propriété **AllowsNull** pour examiner la possibilité de valeur NULL pour chaque colonne calculée dans une table. Une expression qui accepte une valeur Null peut être transformée en expression qui n’accepte pas cette valeur, quand `ISNULL` est spécifié avec la constante *check_expression*, où la constante est une valeur non nulle substituée à n’importe quel résultat NULL. L'autorisation REFERENCES sur le type est nécessaire pour les colonnes calculées basées sur des expressions de type CLR (Common Language Runtime) défini par l'utilisateur.

PERSISTED spécifie que le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] stockera physiquement les valeurs calculées dans la table et qu’il les mettra à jour lorsque les autres colonnes dont dépend la colonne calculée seront actualisées. Notamment, une colonne calculée en tant que `PERSISTED` vous permet de créer un index sur une colonne calculée qui est déterministe, mais pas précise. Pour plus d'informations, consultez [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md). Les colonnes calculées utilisées comme colonnes de partitionnement d’une table partitionnée doivent être explicitement marquées comme `PERSISTED`. *computed_column_expression* doit être déterministe quand `PERSISTED` est spécifié.

ON { *partition_scheme* | *filegroup* |  **"default"** } : Spécifie le schéma ou le groupe de fichiers de partition sur lequel la table est stockée. Si *partition_scheme* est spécifié, la table est partitionnée avec des partitions stockées dans un ensemble d’un ou de plusieurs groupes de fichiers spécifié dans *partition_scheme*. Si *filegroup* est spécifié, la table est stockée dans le groupe de fichiers nommé. Le groupe de fichiers doit exister dans la base de données. Si **"default"** est spécifié, ou si ON n’est pas spécifié du tout, la table est stockée dans le groupe de fichiers par défaut. Le mécanisme de stockage d'une table tel que spécifié dans CREATE TABLE ne peut plus être modifié ultérieurement.

ON {*partition_scheme* | *filegroup* |  **"default"** } peut également être spécifié dans une contrainte PRIMARY KEY ou UNIQUE. Ces contraintes créent des index. Si *filegroup* est spécifié, l’index est stocké dans le groupe de fichiers nommé. Si **"default"** est spécifié, ou si ON n’est pas spécifié du tout, l’index est stocké dans le même groupe de fichiers que la table. Si la contrainte `PRIMARY KEY` ou `UNIQUE` crée un index cluster, les pages de données de la table sont stockées dans le même groupe de fichiers que l’index. Si `CLUSTERED` est spécifié ou si la contrainte crée un index cluster d’une autre manière, et que la valeur *partition_scheme* spécifiée est différente de la valeur *partition_scheme* ou *filegroup* de la définition de table, ou vice versa, seule la définition de la contrainte sera honorée et l’autre sera ignorée.

> [!NOTE]
> Dans ce contexte, *default* n’est pas un mot clé. Il s’agit de l’identificateur du groupe de fichiers par défaut et il doit être délimité, comme dans ON **"default"** ou ON **[** default **]** . Si **"default"** est spécifié, l’option `QUOTED_IDENTIFIER` doit être activée (ON) pour la session active. Il s'agit du paramètre par défaut. Pour plus d’informations, voir [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).
>
> Après avoir créé une table partitionnée, pensez à définir l’option `LOCK_ESCALATION` de la table sur la valeur `AUTO`. Cela peut améliorer la concurrence en permettant l'escalade des verrous au niveau de la partition (HoBT) plutôt que de la table. Pour plus d’informations, consultez [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).

TEXTIMAGE_ON { *filegroup*|  **"default"** } : Indique que les colonnes **text**, **ntext**, **image**, **xml**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** et de type CLR (Common Language Runtime) défini par l’utilisateur (y compris géométrie et géographie) sont stockées dans le groupe de fichiers spécifié.

`TEXTIMAGE_ON` n’est pas autorisé s’il n’y a pas de colonne de valeur élevée dans la table. `TEXTIMAGE_ON` ne peut pas être spécifié si *partition_scheme* est spécifié. Si **"default"** est spécifié, ou si `TEXTIMAGE_ON` n’est pas spécifié du tout, les colonnes de valeur élevée sont stockées dans le groupe de fichiers par défaut. Le stockage de données de colonnes de valeur élevée tel que spécifié dans `CREATE TABLE` ne peut plus être modifié ultérieurement.

> [!NOTE]
> Les valeurs varchar(max), nvarchar(max), varbinary(max), xml ainsi que les valeurs UDT élevées sont stockées directement dans la ligne de données, jusqu’à une limite de 8 000 octets et tant que la valeur peut être contenue dans l’enregistrement. Si la valeur ne tient pas dans l'enregistrement, un pointeur est stocké dans la ligne et le reste est stocké hors de la ligne dans l'espace de stockage LOB. 0 est la valeur par défaut, qui indique que toutes les valeurs sont stockées directement dans la ligne de données.
>
> `TEXTIMAGE_ON` change uniquement l’emplacement de l’espace de stockage LOB. Il n’a pas d’impact sur le moment où les données sont stockées dans la ligne. Utilisez l’option hors ligne des types valeur volumineux de sp_tableoption pour stocker la valeur LOB entière hors de la ligne.
>
> L'élément « default » n'est pas un mot clé dans ce contexte. Il s’agit de l’identificateur du groupe de fichiers par défaut et il doit être délimité, comme dans `TEXTIMAGE_ON "default"` ou `TEXTIMAGE_ON [default]`. Si **"default"** est spécifié, l’option `QUOTED_IDENTIFIER` doit être activée (ON) pour la session active. Il s'agit du paramètre par défaut. Pour plus d’informations, voir [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

FILESTREAM_ON { *partition_scheme_name* | filegroup | **"** default **"** } **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] et versions ultérieures). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ne prend pas en charge `FILESTREAM`.

Spécifie le groupe de fichiers pour les données FILESTREAM.

Si la table contient des données FILESTREAM et si elle est partitionnée, la clause FILESTREAM_ON doit être incluse et doit spécifier un schéma de partition de groupes de fichiers FILESTREAM. Ce schéma de partition doit utiliser la même fonction de partition et les mêmes colonnes de partition que le schéma de partition de la table, faute de quoi une erreur est générée.

Si la table n'est pas partitionnée, la colonne FILESTREAM ne peut pas être partitionnée. Les données FILESTREAM de la table doivent être stockées dans un groupe de fichiers unique. Ce groupe de fichiers est spécifié dans la clause FILESTREAM_ON.

Si la table n’est pas partitionnée et que la clause `FILESTREAM_ON` n’est pas spécifiée, c’est le groupe de fichiers FILESTREAM avec la propriété `DEFAULT`qui est utilisé. S'il n'y a aucun groupe de fichiers FILESTREAM, une erreur est générée.

Comme avec ON et `TEXTIMAGE_ON`, la valeur définie à l’aide de `CREATE TABLE` pour `FILESTREAM_ON` ne peut pas être changée, sauf dans les cas suivants :

- Une instruction [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) convertit un segment de mémoire en index cluster. Dans ce cas, il est possible de spécifier un autre groupe de fichiers FILESTREAM, un autre schéma de partition ou la valeur NULL.
- Une instruction [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md) convertit un index cluster en segment de mémoire. Dans ce cas, il est possible de spécifier un autre groupe de fichiers FILESTREAM, un autre schéma de partition ou la valeur **"default"** .

Le groupe de fichiers de la clause `FILESTREAM_ON <filegroup>` ou chaque groupe de fichiers FILESTREAM nommé dans le schéma de partition doit avoir un fichier défini pour le groupe de fichiers. Ce fichier doit être défini à l’aide d’une instruction [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) ou [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md), sinon une erreur est générée.

Pour accéder à des rubriques FILESTREAM connexes, consultez [Données Blob (Binary Large Object)](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).

[ _type\_schema\_name_ **.** ] *type_name* spécifie le type de données de la colonne et le schéma auquel il appartient. Pour les tables sur disque, le type de données peut être l'un des suivants :

- Type de données système
- Type d'alias basé sur un type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les types de données alias sont créés à l’aide de l’instruction `CREATE TYPE` avant de pouvoir être utilisés dans une définition de table. L’affectation NULL ou NOT NULL pour un type de données d’alias peut être ignorée au moment de l’exécution de l’instruction `CREATE TABLE`. Cependant, la spécification de longueur ne peut pas être changée ; la longueur d’un type de données d’alias ne peut pas être spécifiée dans une instruction `CREATE TABLE`.
- Un type CLR défini par l’utilisateur. Les types de données CLR définis par l’utilisateur sont créés avec l’instruction `CREATE TYPE` avant de pouvoir être utilisés dans une définition de table. Pour créer une colonne sur un type de données CLR défini par l'utilisateur, une autorisation REFERENCES est nécessaire pour le type.

Si *type_schema_name* n’est pas spécifié, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] référence *type_name* dans l’ordre suivant :

- Le type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
- Le schéma par défaut de l'utilisateur actuel dans la base de données active
- Schéma **dbo** dans la base de données active.

Pour les tables à mémoire optimisée, consultez [Types de données pris en charge pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md) afin d’obtenir une liste des types système pris en charge.

*precision* est la précision du type de données spécifié. Pour plus d’informations sur les valeurs de précision valides, consultez [Précision, échelle et longueur](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

*scale* est l’échelle du type de données spécifié. Pour plus d’informations sur les valeurs d’échelle valides, consultez [Précision, échelle et longueur](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

**max** s’applique uniquement aux types de données **varchar**, **nvarchar** et **varbinary** pour le stockage de 2^31 octets de données caractères et binaires et 2^30 octets de données Unicode.

CONTENT spécifie que chaque instance du type de données **xml** dans *column_name* peut contenir plusieurs éléments de niveau supérieur. CONTENT s’applique uniquement au type de données **xml** et ne peut être spécifié que si *xml_schema_collection* l’est également. En l'absence de toute spécification, CONTENT est le comportement par défaut.

DOCUMENT spécifie que chaque instance du type de données **xml** dans *column_name* ne peut contenir qu’un seul élément de niveau supérieur. DOCUMENT s’applique uniquement au type de données **xml** et ne peut être spécifié que si *xml_schema_collection* l’est également.

*xml_schema_collection* s’applique uniquement au type de données **xml** pour l’association d’une collection de schémas XML au type. Avant d’inclure une colonne **xml** dans un schéma, vous devez d’abord créer ce dernier dans la base de données à l’aide de [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).

DEFAULT spécifie la valeur fournie pour la colonne lorsque vous n’avez pas spécifié explicitement de valeur lors d’une insertion. Les définitions DEFAULT peuvent être appliquées à n’importe quelle colonne, sauf celles définies en tant que **timestamp** ou celles ayant la propriété `IDENTITY`. Si une valeur par défaut est spécifiée pour une colonne de type défini par l’utilisateur, le type doit prendre en charge une conversion implicite de *constant_expression* vers le type défini par l’utilisateur. Les définitions de valeurs par défaut sont supprimées lorsque la table est supprimée. Seule une valeur constante, telle qu'une chaîne de caractères, une fonction scalaire (fonction système, définie par l'utilisateur ou CLR) ou la valeur NULL peut être utilisée comme valeur par défaut. Pour maintenir la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un nom de contrainte peut être affecté à une définition DEFAULT.

*constant_expression* est une constante, valeur NULL ou fonction système utilisée comme valeur par défaut pour la colonne.

*memory_optimized_constant_expression* est une constante, valeur NULL ou fonction système prise en charge comme valeur par défaut pour la colonne. Doit être prise en charge dans les procédures stockées compilées en mode natif. Pour plus d’informations sur les fonctions intégrées dans les procédures stockées compilées en mode natif, consultez [Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).

IDENTITY indique que la nouvelle colonne est une colonne d’identité. Lorsqu'une ligne est ajoutée à la table, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] affecte une valeur incrémentée unique à la colonne. Les colonnes d'identité sont normalement utilisées avec les contraintes PRIMARY KEY comme identificateur de ligne unique pour la table. La propriété `IDENTITY` peut être affectée à des colonnes **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** ou **numeric(p,0)** . Une seule colonne d'identité peut être créée par table. Il n'est pas possible d'utiliser des valeurs par défaut liées et des contraintes DEFAULT avec une colonne d'identité. Vous devez spécifier à la fois la valeur initiale et l'incrément ou aucune de ces valeurs. Si vous n'en spécifiez aucun, la valeur par défaut est (1,1).

*seed* est la valeur utilisée pour la toute première ligne chargée dans la table.

*increment* est la valeur d’incrément ajoutée à la valeur d’identité de la ligne précédemment chargée.

NOT FOR REPLICATION Dans l’instruction `CREATE TABLE`, la clause `NOT FOR REPLICATION` peut être spécifiée pour la propriété IDENTITY, les contraintes FOREIGN KEY et les contraintes CHECK. Si la clause est spécifiée pour la propriété `IDENTITY`, les valeurs ne sont pas incrémentées dans les colonnes d’identité lorsque les agents de réplication effectuent des insertions. Si cette clause est spécifiée pour une contrainte, la contrainte n'est pas appliquée lorsque les agents de réplication effectuent des opérations d'insertion, de mise à jour ou de suppression.

TGENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] [ NOT NULL ] **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie qu’une colonne `datetime2` spécifiée sera utilisée par le système pour enregistrer l’heure de début ou l’heure de fin pour laquelle un enregistrement est valide. La colonne doit être définie comme `NOT NULL`. Si vous essayez de la spécifier comme `NULL`, le système génère une erreur. Si vous ne spécifiez pas explicitement NOT NULL pour une colonne de période, le système définit la colonne comme `NOT NULL` par défaut. Utilisez cet argument conjointement avec les arguments `PERIOD FOR SYSTEM_TIME` et `WITH SYSTEM_VERSIONING = ON` pour activer la gestion système des versions sur une table. Pour plus d’informations, voir [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

Vous pouvez marquer une colonne de période, ou les deux, avec l’indicateur **HIDDEN** afin de masquer implicitement ces colonnes pour que **SELECT \* FROM** _`<table>`_ ne retourne pas de valeur pour elles. Par défaut, les colonnes de période ne sont pas masquées. Pour pouvoir être utilisées, les colonnes masquées doivent être incluses explicitement dans toutes les requêtes qui référencent directement la table temporelle. Pour changer l’attribut **HIDDEN** pour une colonne de période existante, vous devez supprimer puis recréer **PERIOD** avec un indicateur HIDDEN différent.

INDEX *index_name* [ CLUSTERED | NONCLUSTERED ] (*column_name* [ ASC | DESC ] [ ,... *n* ] ) **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indique qu’il faut créer un index sur la table. Il peut s’agir d’un index cluster ou non-cluster. L’index contiendra les colonnes répertoriées et triera les données dans l’ordre croissant ou décroissant.

INDEX *index_name* CLUSTERED COLUMNSTORE **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indique qu’il faut stocker la table entière sous forme de colonnes avec un index cluster columnstore. Cela inclut toujours toutes les colonnes de la table. Les données ne sont pas triées par ordre alphabétique ou numérique, car les lignes sont organisées de manière à tirer parti de la compression columnstore.

INDEX *index_name* [ NONCLUSTERED ] COLUMNSTORE (*column_name* [ ,... *n* ] ) **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indique qu’il faut créer un index non-cluster columnstore sur la table. La table sous-jacente peut être un segment de mémoire rowstore ou un index cluster, ou il peut s’agir d’un index columnstore cluster. Dans tous les cas, la création d’un index columnstore non-cluster sur une table stocke une deuxième copie des données pour les colonnes dans l’index.

L’index columnstore non-cluster est stocké et géré en tant qu’index columnstore cluster. Il porte le nom d’index columnstore non-cluster car les colonnes peuvent être limitées et il existe en tant qu’index secondaire sur une table.

ON _partition\_scheme\_name_ **(** _column\_name_ **)** spécifie le schéma de partition qui définit les groupes de fichiers auxquels les partitions d’un index partitionné seront mappées. Le schéma de partition doit exister dans la base de données en exécutant soit [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md), soit [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). *column_name* spécifie la colonne par rapport à laquelle un index partitionné sera partitionné. Cette colonne doit correspondre au type de données, à la longueur et à la précision de l’argument de la fonction de partition que *partition_scheme_name* utilise. *column_name* n’est pas limité aux colonnes de la définition d’index. Toute colonne de la table de base peut être spécifiée, sauf lors du partitionnement d’un index UNIQUE ; le nom de colonne *column_name* doit être choisi parmi les noms de colonnes utilisés comme clés uniques. Cette restriction permet au [!INCLUDE[ssDE](../../includes/ssde-md.md)] de vérifier l'unicité des valeurs de clés dans une seule partition uniquement.

> [!NOTE]
> Lorsque vous partitionnez un index cluster non unique, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ajoute par défaut la colonne de partitionnement à la liste des clés d'index cluster, si elle n'est pas déjà spécifiée. Lorsque vous partitionnez un index non cluster non unique, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ajoute la colonne de partitionnement sous la forme d'une colonne (incluse) non clé de l'index, si elle n'est pas déjà spécifiée.

Si *partition_scheme_name* ou *filegroup* n’est pas spécifié et que la table est partitionnée, l’index est placé dans le même schéma de partition que la table sous-jacente, en utilisant la même colonne de partitionnement.

> [!NOTE]
> Vous ne pouvez pas spécifier un schéma de partitionnement dans un index XML. Si la table de base est partitionnée, l'index XML utilise le même schéma de partition que la table.

Pour plus d’informations sur le partitionnement d’index, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

ON *filegroup_name* crée l’index spécifié dans le groupe de fichiers spécifié. Si aucun emplacement n'est défini et que la table ou la vue n'est pas partitionnée, l'index utilise le même groupe de fichiers que la table ou la vue sous-jacente. Le groupe de fichiers doit déjà exister.

ON **"default"** Crée l’index spécifié sur le groupe de fichiers par défaut.

Le terme « default », dans ce contexte, n'est pas un mot clé. Il s’agit de l’identificateur du groupe de fichiers par défaut et il doit être délimité, comme dans ON **"default"** ou ON **[default]** . Si "default" est spécifié, l’option `QUOTED_IDENTIFIER` doit être activée (ON) pour la session active. Il s'agit du paramètre par défaut. Pour plus d’informations, voir [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

[ FILESTREAM_ON { *filestream_filegroup_name* | *partition_scheme_name* | "NULL" } ] **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] et versions ultérieures).

Spécifie le positionnement de données FILESTREAM pour la table lorsqu'un index cluster est créé. La clause FILESTREAM_ON permet le déplacement des données FILESTREAM vers un schéma de partition ou un groupe de fichiers FILESTREAM différent.

*filestream_filegroup_name* est le nom d’un groupe de fichiers FILESTREAM. Le groupe de fichiers doit avoir un fichier défini pour le groupe de fichiers à l’aide d’une instruction [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) ou [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) ; dans le cas contraire, une erreur est générée.

Si la table est partitionnée, la clause `FILESTREAM_ON` doit être incluse et doit spécifier un schéma de partition de groupes de fichiers FILESTREAM qui utilise la même fonction de partition et les mêmes colonnes de partition que le schéma de partition de la table. Dans le cas contraire, une erreur est générée.

Si la table n'est pas partitionnée, la colonne FILESTREAM ne peut pas être partitionnée. Les données FILESTREAM de la table doivent être stockées dans un groupe de fichiers unique spécifié dans la clause `FILESTREAM_ON`.

`FILESTREAM_ON NULL` peut être spécifié dans une instruction `CREATE INDEX` si un index cluster est créé et si la table ne contient pas de colonne FILESTREAM.

Pour plus d’informations, consultez [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md).

ROWGUIDCOL indique que la nouvelle colonne est une colonne d’identificateur global unique (GUID). Une seule colonne **uniqueidentifier** par table peut être désignée comme colonne ROWGUIDCOL. L’application de la propriété ROWGUIDCOL permet à la colonne d’être référencée à l’aide de `$ROWGUID`. La propriété ROWGUIDCOL ne peut être affectée qu’à une colonne **uniqueidentifier**. Les colonnes avec un type de données défini par l'utilisateur ne peuvent pas être conçues avec ROWGUIDCOL.

La propriété ROWGUIDCOL n'assure pas l'unicité des valeurs stockées dans la colonne. ROWGUIDCOL ne peut pas non plus générer automatiquement des valeurs pour les nouvelles lignes insérées dans la table. Pour générer des valeurs uniques pour chaque colonne, vous pouvez soit utiliser la fonction [NEWID](../../t-sql/functions/newid-transact-sql.md) ou [NEWSEQUENTIALID](../../t-sql/functions/newsequentialid-transact-sql.md) sur des instructions [INSERT](../../t-sql/statements/insert-transact-sql.md), soit utiliser ces fonctions comme fonctions par défaut pour la colonne.

ENCRYPTED WITH spécifie que les colonnes doivent être chiffrées à l’aide de la fonctionnalité [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

COLUMN_ENCRYPTION_KEY = *key_name* spécifie la clé de chiffrement de colonne. Pour plus d’informations, consultez [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md).

ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } : le **chiffrement déterministe** utilise une méthode qui génère toujours la même valeur chiffrée pour une valeur de texte brut donnée. L’utilisation du chiffrement déterministe permet d’effectuer des recherches à l’aide de la comparaison d’égalité, d’effectuer des regroupements et de joindre des tables à l’aide de jointures d’égalité sur la base de valeurs chiffrées, mais elle peut aussi permettre à des utilisateurs non autorisés de deviner des informations concernant les valeurs chiffrées en examinant les séquences dans la colonne chiffrée. Joindre deux tables sur des colonnes chiffrées de façon déterministe n’est possible que si les deux colonnes sont chiffrées à l’aide de la même clé de chiffrement de colonne. Le chiffrement déterministe doit utiliser un classement de colonne avec un ordre de tri binaire 2 pour les colonnes de type caractère.

Le **chiffrement aléatoire** utilise une méthode qui chiffre les données de manière moins prévisible. Le chiffrement aléatoire est plus sécurisé, mais il empêche tout calcul et indexation sur les colonnes chiffrées, sauf si votre instance de SQL Server prend en charge Always Encrypted avec des enclaves sécurisées. Consultez [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md) pour plus d’informations.

Si vous utilisez Always Encrypted (sans enclaves sécurisées), utilisez le chiffrement déterministe pour les colonnes dans lesquelles des recherches seront effectuées avec des paramètres ou des paramètres de regroupement, par exemple un numéro d’identification gouvernemental. Utilisez le chiffrement aléatoire pour les données telles qu’un numéro de carte de crédit, qui ne sont pas regroupées avec d’autres enregistrements, ou utilisées pour joindre des tables, et qui ne sont pas soumises à des recherches car vous utilisez d’autres colonnes (par exemple un numéro de transaction) pour rechercher la ligne qui contient la colonne chiffrée qui vous intéresse.

Si vous utilisez Always Encrypted avec enclaves sécurisées, le chiffrement aléatoire est un type de chiffrement recommandé.

Les colonnes doivent être d’un type de données qualifié.

ALGORITHM **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures).

Doit être **'AEAD_AES_256_CBC_HMAC_SHA_256'** .

Pour plus d’informations, notamment sur les contraintes de fonctionnalité, consultez [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

SPARSE Indique que la colonne est une colonne éparse. Le stockage des colonnes éparses est optimisé pour les valeurs Null. Les colonnes éparses ne peuvent pas être désignées comme NOT NULL. Pour connaître les restrictions supplémentaires et obtenir plus d’informations sur les colonnes éparses, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).

MASKED WITH ( FUNCTION = ' *mask_function* ') **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures).

Spécifie un masque dynamique des données. *mask_function* est le nom de la fonction de masquage avec les paramètres appropriés. Quatre fonctions sont disponibles :

- default()
- email()
- partial()
- random()

Pour les paramètres de fonction, consultez [Masquage dynamique des données](../../relational-databases/security/dynamic-data-masking.md).

FILESTREAM **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] et versions ultérieures)

Valide uniquement pour les colonnes **varbinary(max)** . Spécifie le stockage FILESTREAM pour les données BLOB **varbinary(max)** .

La table doit également comporter une colonne du type de données **uniqueidentifier** ayant l’attribut ROWGUIDCOL. Cette colonne ne doit pas autoriser les valeurs Null et doit avoir une contrainte de colonne unique de type UNIQUE ou PRIMARY KEY. La valeur GUID de la colonne doit être fournie par une application lors de l’insertion de données ou par une contrainte DEFAULT qui utilise la fonction NEWID ().

La colonne ROWGUIDCOL ne peut pas être supprimée et les contraintes liées ne peuvent pas être modifiées tant qu'une colonne FILESTREAM est définie pour la table. La colonne ROWGUIDCOL peut être supprimée uniquement lorsque la dernière colonne FILESTREAM a été supprimée.

Lorsque l'attribut de stockage FILESTREAM est spécifié pour une colonne, toutes les valeurs de cette colonne sont stockées dans un conteneur de données FILESTREAM sur le système de fichiers.

COLLATE *collation_name* spécifie le classement de la colonne. Le nom du classement peut être un nom de classement Windows ou un nom de classement SQL. *collation_name* s’applique uniquement aux colonnes des types de données **char**, **varchar**, **texte**, **nchar**,  **nvarchar** et **ntext**. Si cette valeur n'est pas spécifiée, la colonne reçoit le classement du type de données utilisateur, si son type de données est un type de données utilisateur, ou le classement par défaut de la base de données.

Pour plus d’informations sur les noms de classements Windows et SQL, consultez [Nom de classement Windows](../../t-sql/statements/windows-collation-name-transact-sql.md) et [Nom de classement SQL](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

Pour plus d’informations, consultez l’article [COLLATE](~/t-sql/statements/collations.md).

CONSTRAINT est un mot clé facultatif qui indique le début de la définition d’une contrainte PRIMARY KEY, NOT NULL, UNIQUE, FOREIGN KEY ou CHECK.

*constraint_name* correspond au nom d’une contrainte. Les noms de contraintes doivent être uniques au sein du schéma auquel appartient la table.

NULL | NOT NULL détermine si les valeurs Null sont autorisées dans la colonne. NULL n'est pas strictement une contrainte, mais peut être spécifié comme NOT NULL. Il est possible de spécifier NOT NULL pour des colonnes calculées seulement si PERSISTED est également spécifié.

PRIMARY KEY est une contrainte assurant l’intégrité d’entité d’une ou de plusieurs colonnes spécifiées au moyen d’un index unique. Une seule contrainte PRIMARY KEY peut être créée par table.

UNIQUE est une contrainte assurant l’intégrité de l’entité d’une colonne ou de plusieurs colonnes spécifiées au moyen d’un index unique. Une table peut comprendre plusieurs contraintes UNIQUE.

CLUSTERED | NONCLUSTERED indique qu’un index, cluster ou non cluster, est créé pour la contrainte PRIMARY KEY ou UNIQUE. Les contraintes PRIMARY KEY ont la valeur par défaut CLUSTERED et les contraintes UNIQUE la valeur par défaut NONCLUSTERED.

CLUSTERED peut être spécifié pour une seule contrainte dans une instruction `CREATE TABLE`. Si CLUSTERED est spécifié pour une contrainte UNIQUE et une contrainte PRIMARY KEY est également spécifiée, la contrainte PRIMARY KEY a la valeur par défaut NONCLUSTERED.

FOREIGN KEY REFERENCES est une contrainte qui assure l’intégrité référentielle des données des colonnes. Avec les contraintes FOREIGN KEY, il faut que chaque valeur de la colonne existe dans la ou les colonnes référencées correspondantes de la table référencée. Les contraintes FOREIGN KEY ne peuvent référencer que des colonnes qui sont des contraintes PRIMARY KEY ou UNIQUE dans la table référencée ou des colonnes référencées dans un UNIQUE INDEX sur la table référencée. Les clés étrangères des colonnes calculées doivent également être marquées comme PERSISTED.

[ _schema\_name_ **.** ] *referenced_table_name*] correspond au nom de la table référencée par la contrainte FOREIGN KEY, et le schéma à laquelle elle appartient.

**(** *ref_column* [ **,** ... *n* ] **)** est une colonne ou liste de colonnes provenant de la table référencée par la contrainte FOREIGN KEY.

ON DELETE { **NO ACTION** \| CASCADE \| SET NULL \| SET DEFAULT } spécifie l’action qui se produit dans les lignes de la table créée, si ces lignes comportent une relation référentielle et si la ligne référencée est supprimée de la table parent. La valeur par défaut est NO ACTION.

NO ACTION : le [!INCLUDE[ssDE](../../includes/ssde-md.md)] déclenche une erreur et la suppression de la ligne dans la table parent est annulée.

CASCADE : les lignes correspondantes sont supprimées de la table de référence pour celles supprimées de la table parent.

SET NULL : toutes les valeurs qui composent la clé étrangère sont NULL si la ligne correspondante dans la table parent est supprimée. Pour que cette contrainte s'applique, les colonnes clés étrangères doivent pouvoir cependant être définies sur NULL.

SET DEFAULT : toutes les valeurs qui composent la clé étrangère sont celles par défaut si la ligne correspondante dans la table parent est supprimée. Pour que cette contrainte s'applique, les colonnes clés étrangères doivent disposer cependant de valeur par défaut. Si une colonne peut être affectée de la valeur NULL et qu'aucune valeur par défaut n'est définie, NULL constitue alors la valeur par défaut de la colonne de façon implicite.

Ne spécifiez pas CASCADE si la table est incluse dans une publication de fusion qui utilise des enregistrements logiques. Pour plus d’informations sur les enregistrements logiques, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).

`ON DELETE CASCADE` ne peut pas être spécifié si un déclencheur `INSTEAD OF` défini sur `ON DELETE` existe déjà sur la table.

Par exemple, dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la table **ProductVendor** a une relation référentielle avec la table **Vendor**. La clé étrangère **ProductVendor.BusinessEntityID** référence la clé primaire **Vendor.BusinessEntityID**.

Si une instruction `DELETE` est exécutée sur une ligne de la table **Vendor** et qu’une action `ON DELETE CASCADE` est spécifiée pour **ProductVendor.BusinessEntityID**, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie la présence d’une ou de plusieurs lignes dépendantes dans la table **ProductVendor**. Le cas échéant, les lignes dépendantes détectées dans la table **ProductVendor** sont supprimées, ainsi que la ligne référencée dans la table **Vendor**.

En revanche, si la valeur `NO ACTION` est spécifiée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur et annule la suppression de la ligne dans la table **Vendor** si au moins une ligne y fait référence dans la table **ProductVendor**.

ON UPDATE { **NO ACTION** \| CASCADE \ SET NULL \| SET DEFAULT } spécifie l’action qui se produit sur les lignes de la table modifiée, si chacune de ces lignes possède une relation référentielle et que la ligne référencée correspondante est mise à jour dans la table parent. La valeur par défaut est NO ACTION.

NO ACTION : le [!INCLUDE[ssDE](../../includes/ssde-md.md)] déclenche une erreur et la mise à jour de la ligne dans la table parent est restaurée.

CASCADE : les lignes correspondantes sont mises à jour dans la table de référence si la ligne de la table parent est mise à jour.

SET NULL : toutes les valeurs composant la clé étrangère sont définies sur NULL si la ligne correspondante se trouvant à l’origine dans la table parent est mise à jour. Pour que cette contrainte s'applique, les colonnes clés étrangères doivent pouvoir cependant être définies sur NULL.

SET DEFAULT : toutes les valeurs composant la clé étrangère sont définies sur leur valeur par défaut si la ligne correspondante se trouvant à l’origine dans la table parent est mise à jour. Pour que cette contrainte s'applique, les colonnes clés étrangères doivent disposer cependant de valeur par défaut. Si une colonne peut être affectée de la valeur NULL et qu'aucune valeur par défaut n'est définie, NULL constitue alors la valeur par défaut de la colonne de façon implicite.

Ne spécifiez pas `CASCADE` si la table est incluse dans une publication de fusion qui utilise des enregistrements logiques. Pour plus d’informations sur les enregistrements logiques, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).

`ON UPDATE CASCADE`, `SET NULL` ou `SET DEFAULT` ne peut pas être spécifié si un déclencheur `INSTEAD OF` défini sur `ON UPDATE` existe déjà dans la table en cours de modification.

Par exemple, dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la table **ProductVendor** a une relation référentielle avec la table **Vendor** : la clé étrangère **ProductVendor.BusinessEntity** référence la clé primaire **Vendor.BusinessEntityID**.

Si une instruction UPDATE est exécutée sur une ligne de la table **Vendor** et qu’une action ON UPDATE CASCADE est spécifiée pour **ProductVendor.BusinessEntityID**, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie la présence de lignes dépendantes dans la table **ProductVendor**. Le cas échéant, les lignes dépendantes détectées dans la table **ProductVendor** sont mises à jour, ainsi que la ligne référencée dans la table **Vendor**.

En revanche, si la valeur NO ACTION est spécifiée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur et restaure la mise à jour de la ligne dans la table **Vendor** si au moins une ligne y fait référence dans la table **ProductVendor**.

CHECK est une contrainte qui assure l’intégrité du domaine en limitant les valeurs possibles pouvant être entrées dans une ou plusieurs colonnes. Les contraintes CHECK des colonnes calculées doivent également être marquées comme PERSISTED.

*logical_expression* est une expression logique qui retourne TRUE ou FALSE. Les types de données d'alias ne peuvent pas faire partie de l'expression.

*column* est une colonne, ou liste de colonnes, entre parenthèses, utilisée dans des contraintes de table pour indiquer les colonnes utilisées dans la définition de la contrainte.

[ **ASC** | DESC ] spécifie l’ordre de tri de la ou des colonnes impliquées dans les contraintes de table. La valeur par défaut est ASC.

*partition_scheme_name* correspond au nom du schéma de partition qui définit les groupes de fichiers vers lesquels les partitions d’une table partitionnée seront mappées. Le schéma de partition doit exister dans la base de données.

[ _partition\_colonne\_nom_ **.** ] spécifie la colonne selon laquelle une table partitionnée sera partitionnée. Cette colonne doit être identique en termes de type de données, de longueur et de précision à celle qui est spécifiée dans la fonction de partition utilisée par *partition_scheme_name*. Une colonne calculée qui participe à une fonction de partition doit être explicitement marquée comme PERSISTED.

> [!IMPORTANT]
> Nous vous conseillons de spécifier NOT NULL sur la colonne de partitionnement des tables partitionnées et également des tables non partitionnées qui sont sources ou cibles d'opérations ALTER TABLE...SWITCH. Vous êtes ainsi certain que les contraintes CHECK sur les colonnes de partitionnement ne doivent pas rechercher la présence de valeurs Null.

WITH FILLFACTOR **=** _fillfactor_ spécifie le remplissage par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] des pages d’index utilisées pour stocker les données d’index. Les valeurs *fillfactor* spécifiées par l’utilisateur doivent être comprises entre 1 et 100. Si aucune valeur n'est spécifiée, la valeur par défaut est 0. Les taux de remplissage 0 et 100 sont identiques en tous points.

> [!IMPORTANT]
> Dans la documentation, l’indication que WITH FILLFACTOR = *fillfactor* constitue l’unique option d’indexation s’appliquant aux contraintes PRIMARY KEY ou UNIQUE est maintenue dans un but de compatibilité ascendante, mais ne sera plus indiquée ainsi dans les versions à venir.

*column_set_name* XML COLUMN_SET FOR ALL_SPARSE_COLUMNS correspond au nom du jeu de colonnes. Un jeu de colonnes est une représentation XML non typée qui combine toutes les colonnes éparses d'une table dans une sortie structurée. Pour plus d’informations sur les jeux de colonnes, consultez [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).

PERIOD FOR SYSTEM_TIME (*system_start_time_column_name* , *system_end_time_column_name* ) **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie les noms des colonnes que le système utilisera pour enregistrer la période pour laquelle un enregistrement est valide. Utilisez cet argument conjointement avec les arguments GENERATED ALWAYS AS ROW { START | END } et WITH SYSTEM_VERSIONING = ON pour activer la gestion système des versions sur une table. Pour plus d’informations, voir [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

COMPRESSION_DELAY **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Pour une table à mémoire optimisée, le délai spécifie le nombre minimal de minutes pendant lesquelles une ligne doit rester dans la table, inchangée, avant d’être éligible pour la compression dans l’index columnstore. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sélectionne les lignes spécifiques à compresser en fonction de l’heure de leur dernière mise à jour. Par exemple, si les lignes changent fréquemment pendant une période de deux heures, vous pouvez définir `COMPRESSION_DELAY = 120 Minutes` pour empêcher SQL Server de compresser une ligne avant la fin des mises à jour.

Pour une table sur disque, le délai spécifie le nombre minimal de minutes pendant lesquelles un rowgroup delta à l’état CLOSED doit rester dans le rowgroup delta avant que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puisse le compresser dans le rowgroup compressé. Étant donné que les tables sur disque ne suivent pas les durées des opérations d’insertion et de mise à jour sur chaque ligne, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applique le délai aux rowgroups delta qui ont l’état CLOSED.

La valeur par défaut est 0 minute.

Pour obtenir des recommandations concernant l’utilisation de `COMPRESSION_DELAY`, consultez [Prise en main de Columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).

\< table_option> ::= spécifie une ou plusieurs options de table.

DATA_COMPRESSION spécifie l’option de compression de données pour la table, le numéro de partition ou la plage de partitions spécifiés. Les options disponibles sont les suivantes :

NONE : la table ou les partitions spécifiées ne sont pas compressées.

ROW La table ou les partitions spécifiées sont compressées au moyen de la compression de ligne.

PAGE La table ou les partitions spécifiées sont compressées au moyen de la compression de page.

COLUMNSTORE

**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

S'applique uniquement aux index columnstore, y compris aux index columnstore non cluster et cluster. COLUMNSTORE indique qu’il faut compresser avec la compression columnstore la plus performante. Il s’agit de l’option généralement choisie.

COLUMNSTORE_ARCHIVE **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

S'applique uniquement aux index columnstore, y compris aux index columnstore non cluster et cluster. COLUMNSTORE_ARCHIVE compressera davantage la partition ou la table en une plus petite taille. Peut être utilisé pour l'archivage, ou d'autres situations qui nécessitent moins de stockage et supportent plus de temps pour le stockage et la récupération.

Pour plus d’informations, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md).

ON PARTITIONS **(** { `<partition_number_expression>` | [ **,** ...*n* ] **)** spécifie les partitions auxquelles le paramètre DATA_COMPRESSION s’applique. Si la table n'est pas partitionnée, l'argument `ON PARTITIONS` génère une erreur. Si la clause `ON PARTITIONS` n’est pas fournie, l’option `DATA_COMPRESSION` s’applique à toutes les partitions d’une table partitionnée.

*partition_number_expression* peut être spécifié des manières suivantes :

- Spécifiez le numéro de partition d’une partition, par exemple : `ON PARTITIONS (2)`
- Spécifiez les numéros de plusieurs partitions individuelles séparés par des virgules, par exemple : `ON PARTITIONS (1, 5)`
- Spécifiez à la fois des plages et des partitions individuelles, par exemple : `ON PARTITIONS (2, 4, 6 TO 8)`

`<range>` peut être spécifié sous la forme de numéros de partitions séparés par le mot TO, par exemple : `ON PARTITIONS (6 TO 8)`.

Pour définir des types différents de compression de données pour des partitions différentes, spécifiez plusieurs fois l’option `DATA_COMPRESSION`, par exemple :

```sql
WITH
(
    DATA_COMPRESSION = NONE ON PARTITIONS (1),
    DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),
    DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)
)
```

\<index_option> ::= spécifie une ou plusieurs options d’index. Pour obtenir une description complète de ces options, consultez [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md).

PAD_INDEX = { ON | **OFF** } : lorsque ON est spécifié, le pourcentage d’espace disponible spécifié par FILLFACTOR est appliqué aux pages de niveau intermédiaire de l’index. Lorsque OFF ou une valeur FILLFACTOR n'est pas spécifié, les pages de niveau intermédiaire de l'index sont presque entièrement remplies, ce qui laisse un espace libre suffisant pour prendre en charge au moins une ligne de la taille maximale permise par l'index, en prenant en compte l'ensemble de clés sur les pages intermédiaires. La valeur par défaut est OFF.

FILLFACTOR **=** _fillfactor_ spécifie un pourcentage indiquant le taux de remplissage appliqué par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] au niveau feuille de chaque page d’index lors de la création ou de la modification de l’index. *fillfactor* doit être une valeur entière comprise entre 1 et 100. La valeur par défaut est 0. Les taux de remplissage 0 et 100 sont identiques en tous points.

IGNORE_DUP_KEY = { ON | **OFF** } spécifie la réponse d’erreur lorsqu’une opération d’insertion essaie d’insérer des valeurs de clés en double dans un index unique. L'option IGNORE_DUP_KEY s'applique uniquement aux opérations d'insertion après la création ou la régénération de l'index. Cette option n’a aucun effet lors de l’exécution de [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), d’[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) ou d’[UPDATE](../../t-sql/queries/update-transact-sql.md). La valeur par défaut est OFF.

ON : un message d’avertissement s’affiche lorsque des valeurs de clé en double sont insérées dans un index unique. Seules les lignes qui violent la contrainte d'unicité échouent.

OFF : un message d’erreur s’affiche lorsque des valeurs de clé en double sont insérées dans un index unique. L'intégralité de l'opération INSERT sera restaurée.

`IGNORE_DUP_KEY` ne peut pas être défini sur ON pour les index créés sur une vue, les index non uniques, les index XML, les index spatiaux et les index filtrés.

Pour voir `IGNORE_DUP_KEY`, utilisez [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

Dans la syntaxe à compatibilité descendante, `WITH IGNORE_DUP_KEY` équivaut à `WITH IGNORE_DUP_KEY = ON`.

STATISTICS_NORECOMPUTE **=** { ON | **OFF** } : lorsque la valeur spécifiée est ON, les statistiques d’index périmées ne sont pas recalculées automatiquement. Lorsque la valeur spécifiée est OFF, la mise à jour automatique des statistiques est activée. La valeur par défaut est OFF.

ALLOW_ROW_LOCKS **=** { **ON** | OFF } : lorsque la valeur spécifiée est ON, les verrous de ligne sont autorisés lorsque vous accédez à l’index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de ligne sont utilisés. Si la valeur est OFF, les verrous de ligne ne sont pas utilisés. La valeur par défaut est ON.

ALLOW_PAGE_LOCKS **=** { **ON** | OFF } : lorsque la valeur spécifiée est ON, les verrous de page sont autorisés lorsque vous accédez à l’index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de page sont utilisés. Si la valeur est OFF, les verrous de page ne sont pas utilisés. La valeur par défaut est ON.

OPTIMIZE_FOR_SEQUENTIAL_KEY = {ON | **OFF** } **s’applique à** : [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] et versions ultérieures. <BR>
Spécifie s’il faut optimiser ou pas la contention d’insertion de la dernière page. La valeur par défaut est OFF. Consultez la section [clés séquentielles](./create-index-transact-sql.md#sequential-keys) de la page CREATE INDEX pour plus d’informations.

FILETABLE_DIRECTORY = *directory_name*

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures).

Spécifie le nom de répertoire FileTable compatible Windows. Ce nom doit être unique parmi tous les noms de répertoire FileTable de la base de données. La comparaison d'unicité n'est pas sensible à la casse, indépendamment des paramètres de classement. Si cette valeur n'est pas spécifiée, le nom de la table de fichiers est utilisé.

FILETABLE_COLLATE_FILENAME = { *collation_name* | database_default }

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ne prend pas en charge `FILETABLE`.

Spécifie le nom du classement à appliquer à la colonne **Name** du FileTable. Le classement ne doit pas être sensible à la casse pour des raisons de conformité à la sémantique de nommage des fichiers du système d’exploitation Windows. Si cette valeur n'est pas spécifiée, le classement par défaut de la base de données est utilisé. Si le classement par défaut de la base de données respecte la casse, une erreur est générée et l'opération CREATE TABLE échoue.

*collation_name* correspond au nom d’un classement non sensible à la casse.

database_default spécifie que le classement par défaut de la base de données doit être utilisé. Ce classement ne doit pas être sensible à la casse.

FILETABLE_PRIMARY_KEY_CONSTRAINT_NAME = *constraint_name*
**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures).

Spécifie le nom à utiliser pour la contrainte de clé primaire qui est créée automatiquement sur le FileTable. Si cette valeur n'est pas spécifiée, le système génère un nom pour la contrainte.

FILETABLE_STREAMID_UNIQUE_CONSTRAINT_NAME = *constraint_name*
**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures).

Spécifie le nom à utiliser pour la contrainte unique qui est créée automatiquement sur la colonne **stream_id** dans le FileTable. Si cette valeur n'est pas spécifiée, le système génère un nom pour la contrainte.

FILETABLE_FULLPATH_UNIQUE_CONSTRAINT_NAME = *constraint_name*
**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures).

Spécifie le nom à utiliser pour la contrainte unique qui est créée automatiquement sur les colonnes **parent_path_locator** et **name** dans le FileTable. Si cette valeur n'est pas spécifiée, le système génère un nom pour la contrainte.

SYSTEM_VERSIONING **=** ON [ ( HISTORY_TABLE **=** *schema_name* .*history_table_name* [, DATA_CONSISTENCY_CHECK **=** { **ON** | OFF } ] ) ] **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]).

Active la gestion système des versions de la table si le type de données, les contraintes de possibilité de valeur Null et les spécifications de contrainte de clé primaire sont satisfaits. Si l’argument `HISTORY_TABLE` n’est pas utilisé, le système génère une nouvelle table d’historique qui correspond au schéma de la table actuelle dans le même groupe de fichiers que la table actuelle, créant un lien entre les deux tables. Ainsi, le système peut enregistrer l’historique de chaque enregistrement dans la table actuelle dans la table d’historique. Le nom de cette table d’historique sera `MSSQL_TemporalHistoryFor<primary_table_object_id>`. Par défaut, la table d’historique est **PAGE** compressée. Si l’argument `HISTORY_TABLE` est utilisé pour créer un lien vers une table d’historique existante et pour utiliser cette table, le lien est créé entre la table actuelle et la table spécifiée. Si la table actuelle est partitionnée, la table d’historique est créée sur le groupe de fichiers par défaut car la configuration du partitionnement n’est pas répliquée automatiquement de la table actuelle dans la table d’historique. Si le nom d’une table d’historique est spécifié lors de sa création, vous devez spécifier le nom du schéma et de la table. Lorsque vous créez un lien vers une table de l’historique existante, vous pouvez choisir d’effectuer une vérification de cohérence des données. Cette vérification de cohérence des données garantit que les enregistrements existants ne se chevauchent pas. La vérification de cohérence des données est effectuée par défaut. Utilisez cet argument conjointement avec les arguments `PERIOD FOR SYSTEM_TIME` et `GENERATED ALWAYS AS ROW { START | END }` pour activer la gestion système des versions sur une table. Pour plus d’informations, voir [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

REMOTE_DATA_ARCHIVE = { ON [ ( *table_stretch_options* [,...n] ) ] | OFF ( MIGRATION_STATE = PAUSED ) }   
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures).

Crée la nouvelle table avec Stretch Database activé ou désactivé. Pour plus d'informations, consultez [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

**Activation de Stretch Database pour une table**

Lorsque vous activez Stretch pour une table en spécifiant `ON`, vous pouvez éventuellement spécifier `MIGRATION_STATE = OUTBOUND` pour commencer à migrer les données immédiatement, ou `MIGRATION_STATE = PAUSED` pour reporter la migration des données. La valeur par défaut est `MIGRATION_STATE = OUTBOUND`. Pour plus d’informations sur l’activation de Stretch pour une table, consultez [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

**Conditions préalables**. Avant d’activer Stretch pour une table, vous devez l’activer sur le serveur et sur la base de données. Pour plus d'informations, consultez [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).

**Autorisations**. L’activation de Stretch pour une table ou une base de données nécessite les autorisations db_owner. L’activation de Stretch pour une table nécessite également des autorisations ALTER sur la table.

[ FILTER_PREDICATE = { null | *predicate* } ] **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures).

Spécifie éventuellement un prédicat de filtre pour sélectionner des lignes à migrer à partir d’une table qui contient des données historiques et actuelles. Le prédicat doit appeler une fonction table inline déterministe. Pour plus d’informations, consultez [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) et [Sélectionner les lignes à migrer à l’aide d’une fonction de filtre](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).

> [!IMPORTANT]
> Si vous fournissez un prédicat de filtre qui fonctionne mal, la migration des données fonctionne mal également. Stretch Database applique le prédicat de filtre à la table à l'aide de l'opérateur CROSS APPLY.

Si vous ne spécifiez aucun prédicat de filtre, la table entière est migrée.

Quand vous spécifiez un prédicat de filtre, vous devez également spécifier *MIGRATION_STATE*.

MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED } **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

- Spécifiez `OUTBOUND` pour migrer des données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
- Spécifiez `INBOUND` pour copier les données distantes pour la table de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et pour désactiver Stretch pour la table. Pour plus d’informations, consultez [Désactiver Stretch Database et récupérer les données distantes](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

   Cette opération entraîne des coûts de transfert de données et ne peut pas être annulée.

- Spécifiez `PAUSED` pour interrompre ou reporter la migration des données. Pour plus d’informations, consultez [Suspension et reprise de la migration de données - Stretch Database](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).

[DATA_DELETION = ON  
    {( FILTER_COLUMN = column_name,   
           RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS | MONTH | MONTHS | YEAR | YEARS } )}] **Applies to:** Azure SQL Edge *uniquement*

Active le nettoyage basé sur la stratégie de rétention des données anciennes dans les tables d’une base de données. Pour plus d’informations, consultez [Activer et désactiver la rétention des données](/azure/azure-sql-edge/data-retention-enable-disable). Les paramètres suivants doivent être spécifiés pour que la rétention des données soit activée. 

- FILTER_COLUMN = { column_name }  
Spécifie la colonne qui doit être utilisée pour déterminer si les lignes de la table sont obsolètes ou non. Les types de données suivants sont autorisés pour la colonne de filtre.
  - Date
  - DateTime
  - DateTime2
  - SmallDateTime
  - DateTimeOffset

- RETENTION_PERIOD = { INFINITE \| number {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS }}       
  Spécifie la stratégie de la période de rétention pour la table. La période de rétention est spécifiée sous forme de combinaison d’une valeur entière positive et de l’unité de la partie date.   

MEMORY_OPTIMIZED **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]). Azure SQL Managed Instance ne prend pas en charge les tables à mémoire optimisée.

La valeur ON indique que la table est à mémoire optimisée. Les tables à mémoire optimisée font partie de la fonctionnalité OLTP en mémoire, qui sert à optimisé les performances de traitement des transactions. Pour bien démarrer avec OLTP en mémoire, consultez [Démarrage rapide 1 : technologies OLTP en mémoire pour accélérer les performances Transact-SQL](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md). Pour plus d’informations sur les tables à mémoire optimisée, consultez [Tables optimisées en mémoire](../../relational-databases/in-memory-oltp/sample-database-for-in-memory-oltp.md).

La valeur par défaut OFF indique qu’il s’agit d’une table sur disque.

DURABILITY **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

La valeur de `SCHEMA_AND_DATA` indique que la table est durable, ce qui signifie que les modifications sont rendues persistantes sur disque et survivent au redémarrage ou au basculement. SCHEMA_AND_DATA est la valeur par défaut.

La valeur de `SCHEMA_ONLY` indique que la table est une table non durable. Le schéma de la table est conservé mais aucune mise à jour des données n’est conservée lors du redémarrage ou du basculement de la base de données. `DURABILITY = SCHEMA_ONLY` est uniquement autorisé avec `MEMORY_OPTIMIZED = ON`.

> [!WARNING]
> Quand une table est créée avec **DURABILITY = SCHEMA_ONLY** et que **READ_COMMITTED_SNAPSHOT** est changé par la suite à l’aide d’**ALTER DATABASE**, les données de la table sont perdues.

BUCKET_COUNT **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indique le nombre de compartiments qui doivent être créés dans l'index de hachage. La valeur maximale de BUCKET_COUNT dans les index de hachage est de 1 073 741 824. Pour plus d’informations sur le nombre de compartiments, consultez [Index sur des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).

Bucket_count est un argument obligatoire.

INDEX **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Vous pouvez spécifier les index de table et de colonne dans le cadre de l’instruction CREATE TABLE. Pour plus d’informations sur l’ajout et la suppression d’index sur des tables à mémoire optimisée, consultez : [Modification des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)

HASH **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indique qu'un index HASH est créé.

Les index de hachage sont pris en charge uniquement sur les tables mémoire optimisées.

## <a name="remarks"></a>Notes

Pour plus d’informations sur le nombre de tables, colonnes, contraintes et index autorisés, consultez [Spécifications des capacités maximales pour SQL Server](../../sql-server/maximum-capacity-specifications-for-sql-server.md).

L'espace est généralement alloué aux tables et aux index par incréments d'une valeur d'extension à la fois. Quand l’option `SET MIXED_PAGE_ALLOCATION` de `ALTER DATABASE` a la valeur TRUE, ou toujours avant [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], quand une table ou un index est créé, des pages lui sont allouées à partir d’extensions mixtes jusqu’à ce qu’il ou elle ait suffisamment de pages pour remplir une extension uniforme. Quand il y a assez de pages pour remplir une extension uniforme, une autre extension est allouée chaque fois que l'extension active est pleine. Pour obtenir des informations sur la quantité d’espace allouée et utilisée par une table, exécutez `sp_spaceused`.

Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne garantit pas l'application de l'ordre dans lequel des contraintes DEFAULT, IDENTITY, ROWGUIDCOL, ou des contraintes de colonne, sont spécifiées dans une définition de colonne.

Lors de la création d'une table, l'option QUOTED IDENTIFIER est toujours stockée avec la valeur ON dans les métadonnées de la table, même si elle a la valeur OFF au moment de sa création.

## <a name="temporary-tables"></a>Tables temporaires

Vous pouvez créer des tables temporaires locales et globales. Les tables temporaires locales ne peuvent être vues que dans la session active ; les tables temporaires globales sont accessibles dans toutes les sessions. Les tables temporaires ne peuvent pas être partitionnées.

Faites précéder les noms de tables temporaires locales d’un signe dièse (#*table_name*), et les noms de tables temporaires globales de deux signes dièse (##*table_name*).

Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] référencent une table temporaire à l’aide de la valeur spécifiée pour *table_name* dans l’instruction `CREATE TABLE`, par exemple :

```sql
CREATE TABLE #MyTempTable (
    col1 INT PRIMARY KEY
);

INSERT INTO #MyTempTable
VALUES (1);
```

Si plusieurs tables temporaires sont créées dans un lot ou une seule procédure stockée, elles doivent porter des noms différents.

Si vous incluez une valeur *schema_name* lorsque vous créez ou accédez à une table temporaire, cette valeur est ignorée. Toutes les tables temporaires sont créées dans le schéma dbo.

Si vous créez une table temporaire locale dans une procédure stockée ou dans une application qui peut être exécutée en même temps par plusieurs utilisateurs, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit être capable de distinguer les tables créées par les différents utilisateurs. Cela est effectué en interne par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] en ajoutant de manière interne un suffixe numérique à chaque nom de table temporaire locale. Le nom complet d’une table temporaire, tel qu’il est stocké dans la table **sysobjects** de **tempdb**, est constitué du nom de table spécifié dans l’instruction CREATE TABLE et du suffixe numérique généré par le système. Pour laisser assez de place au suffixe, le *table_name* spécifié pour un nom de table temporaire locale ne doit pas dépasser 116 caractères.

Les tables temporaires sont automatiquement supprimées lorsqu'elles passent hors de portée, sauf si elles sont supprimées explicitement à l'aide de DROP TABLE :

- Une table temporaire locale créée dans une procédure stockée est supprimée automatiquement lorsque la procédure stockée est terminée. La table peut être référencée par des procédures stockées imbriquées exécutées par la procédure stockée qui a créé la table. La table ne peut pas être référencée par le processus qui a appelé la procédure stockée ayant créé la table.
- Toutes les autres tables temporaires locales sont supprimées automatiquement à la fin de la session active.
- Les tables temporaires globales sont supprimées automatiquement lorsque la session qui a créé la table se termine, et que toutes les autres tâches n'y font plus référence. L'association entre une tâche et une table n'est assurée que pendant la durée d'une seule instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]. Cela signifie qu'une table temporaire globale est supprimée à la fin de la dernière instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui faisait activement référence à la table lorsque la session de création s'est terminée.

Une table temporaire locale créée au sein d'une procédure stockée ou d'un déclencheur peut avoir le même nom qu'une table temporaire créée avant l'appel de la procédure stockée ou du déclencheur. Cependant, si une requête fait référence à une table temporaire et si deux tables temporaires portent ce nom, la table par rapport à laquelle la requête est résolue n'est pas définie. Les procédures stockées imbriquées peuvent également créer des tables temporaires portant le même nom qu'une table temporaire créée par la procédure stockée qui l'a appelée. Cependant, pour que les modifications résolvent la table créée par la procédure imbriquée, la table doit avoir la même structure, avec les mêmes noms de colonnes, que la table créée dans la procédure d'appel. Cela est illustré par l'exemple suivant.

```sql
CREATE PROCEDURE dbo.Test2
AS
    CREATE TABLE #t (x INT PRIMARY KEY);
    INSERT INTO #t VALUES (2);
    SELECT Test2Col = x FROM #t;
GO

CREATE PROCEDURE dbo.Test1
AS
    CREATE TABLE #t (x INT PRIMARY KEY);
    INSERT INTO #t VALUES (1);
    SELECT Test1Col = x FROM #t;
    EXEC Test2;
GO

CREATE TABLE #t(x INT PRIMARY KEY);
INSERT INTO #t VALUES (99);
GO

EXEC Test1;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
(1 row(s) affected)
Test1Col
-----------
1

(1 row(s) affected)
 Test2Col
 -----------
 2
 ```

Quand vous créez des tables temporaires locales ou globales, la syntaxe `CREATE TABLE` prend en charge les définitions de contraintes à l’exception des contraintes FOREIGN KEY. Si vous spécifiez une contrainte FOREIGN KEY dans une table temporaire, l'instruction retourne un message d'avertissement précisant que la contrainte a été ignorée. La table est toujours créée mais sans les contraintes FOREIGN KEY. Les tables temporaires ne peuvent pas être référencées dans des contraintes FOREIGN KEY.

Si une table temporaire est créée avec une contrainte nommée et dans l'étendue d'une transaction définie par l'utilisateur, un seul utilisateur à la fois peut exécuter l'instruction qui crée la table temp. Par exemple, si une procédure stockée crée une table temporaire avec une contrainte nommée de clé primaire, elle ne peut pas être exécutée simultanément par plusieurs utilisateurs.

## <a name="database-scoped-global-temporary-tables-azure-sql-database"></a>Tables temporaires globales délimitées à la base de données (Azure SQL Database)

Les tables temporaires globales pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (dont le nom de table commence par ##) sont stockées dans tempdb et partagées entre les sessions de tous les utilisateurs dans toute l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur les types de tables SQL, consultez la section ci-dessus sur CREATE TABLE.

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] prend en charge les tables temporaires globales qui sont également stockées dans tempdb et dont l’étendue est limitée à la base de données. Cela signifie que les tables temporaires globales sont partagées pour les sessions de tous les utilisateurs au sein de la même [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Les sessions utilisateur d’autres instances de bases de données n’ont pas accès aux tables temporaires globales.

Les tables temporaires globales pour [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] suivent la même syntaxe et la même sémantique que celles utilisées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les tables temporaires. De même, les procédures stockées temporaires globales sont également délimitées à la base de données dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Les tables temporaires locales (dont le nom de table commence par #) sont également prises en charge pour [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et suivent la même syntaxe et la même sémantique que celles utilisées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Consultez la section ci-dessus sur les [Tables temporaires](#temporary-tables).  

> [!IMPORTANT]
> Cette fonctionnalité est disponible pour [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

### <a name="troubleshooting-global-temporary-tables-for-azure-sql-database"></a>Dépannage des tables temporaires globales pour Azure SQL Database

Pour résoudre des problèmes liés à tempdb, consultez [Comment analyser l’utilisation de tempdb](../../relational-databases/databases/tempdb-database.md#monitoring-tempdb-use).

> [!NOTE]
> Seul un administrateur de serveur peut accéder aux DMV de dépannage dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

### <a name="permissions"></a>Autorisations

Tout utilisateur peut créer des objets temporaires globaux. Les utilisateurs n'ont accès qu'aux objets qu'ils possèdent, sauf s'ils ont reçu des autorisations supplémentaires.

## <a name="partitioned-tables"></a>tables partitionnées ;

Avant de créer une table partitionnée à l'aide de CREATE TABLE, vous devez d'abord créer une fonction de partition pour spécifier la manière dont la table est partitionnée. Une fonction de partition est créée à l’aide de [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md). Ensuite, vous devez créer un schéma de partition pour spécifier les groupes de fichiers qui contiendront les partitions indiquées par la fonction de partition. Un schéma de partition est créé à l’aide de [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md). Le placement des contraintes PRIMARY KEY ou UNIQUE pour séparer les groupes de fichiers ne peut pas être spécifié pour les tables partitionnées. Pour plus d’informations, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

## <a name="primary-key-constraints"></a>Contraintes PRIMARY KEY

- Une table ne peut contenir qu'une seule contrainte PRIMARY KEY.
- L'index généré par une contrainte PRIMARY KEY ne peut avoir pour conséquence une augmentation du nombre d'index dans la table à plus de 999 index non cluster et un index cluster.
- Si vous ne spécifiez pas CLUSTERED ou NONCLUSTERED pour une contrainte PRIMARY KEY, CLUSTERED est utilisé s'il n'y a pas d'index cluster spécifiés pour les contraintes UNIQUE.
- Toutes les colonnes définies dans une contrainte PRIMARY KEY doivent avoir la valeur NOT NULL. Si vous ne spécifiez pas la possibilité ou non de valeurs NULL, toutes les colonnes participant à une contrainte PRIMARY KEY sont définies à NOT NULL.

    > [!NOTE]
    > Pour les tables à mémoire optimisée, la colonne clé autorisant la valeur NULL est autorisée.

- Si une clé primaire est définie sur une colonne avec le type de données CLR défini par l'utilisateur, l'implémentation du type doit prendre en charge le tri binaire. Pour plus d’informations, consultez [Types CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

## <a name="unique-constraints"></a>Contraintes UNIQUE

- Si vous ne spécifiez pas CLUSTERED ou NONCLUSTERED pour une contrainte UNIQUE, NONCLUSTERED est utilisé par défaut.
- Chaque contrainte UNIQUE génère un index. Le nombre de contraintes UNIQUE ne peut avoir pour conséquence une augmentation du nombre d'index dans la table à plus de 999 index non cluster et 1 index cluster.
- Si une contrainte unique est définie sur une colonne avec le type de données CLR défini par l'utilisateur, l'implémentation du type doit prendre en charge le tri binaire ou basé sur l'opérateur. Pour plus d’informations, consultez [Types CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

## <a name="foreign-key-constraints"></a>Contraintes FOREIGN KEY

- Lorsqu'une valeur différente de NULL est entrée dans la colonne d'une contrainte FOREIGN KEY, la valeur doit exister dans la colonne référencée. Dans le cas contraire, le système retourne un message d'erreur signalant une violation de clé étrangère.
- Les contraintes FOREIGN KEY sont appliquées à la colonne précédente, à moins que des colonnes sources ne soient spécifiées.
- Les contraintes FOREIGN KEY ne peuvent faire référence qu'à des tables au sein de la même base de données sur le même serveur. L'intégrité référentielle inter-base de données doit être implémentée via les déclencheurs. Pour plus d’informations, consultez [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md).
- Les contraintes FOREIGN KEY peuvent faire référence à une autre colonne dans la même table. On appelle habituellement ce mécanisme « auto-référence ».
- La clause REFERENCES d'une contrainte FOREIGN KEY au niveau des colonnes, ne peut lister qu'une colonne de référence. Cette colonne doit avoir le même type de données que la colonne pour laquelle la contrainte est définie.
- La clause REFERENCES d'une contrainte FOREIGN KEY de niveau table doit avoir le même nombre de colonnes de référence que le nombre de colonnes de la liste des colonnes de la contrainte. Le type de données de chaque colonne de référence doit également être identique à la colonne de référence correspondante dans la liste des colonnes. Les colonnes de référence doivent être spécifiées dans le même ordre que celui utilisé lors de la spécification des colonnes de la clé primaire ou de la contrainte unique sur la table référencée.
- La valeur CASCADE, SET NULL ou SET DEFAULT ne peut pas être spécifiée si une colonne de type **timestamp** fait partie de la clé étrangère ou de la clé référencée.
- Il est possible de combiner CASCADE, SET NULL, SET DEFAULT et NO ACTION pour des tables liées par des relations référentielles. Si le moteur [!INCLUDE[ssDE](../../includes/ssde-md.md)] rencontre NO ACTION, il s’interrompt et restaure les actions CASCADE, SET NULL et SET DEFAULT. Lorsqu'une instruction DELETE génère une combinaison d'actions CASCADE, SET NULL, SET DEFAULT et NO ACTION, les actions CASCADE, SET NULL et SET DEFAULT sont appliquées par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] avant toute recherche de NO ACTION.
- Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'a pas de limite prédéfinie du nombre de contraintes FOREIGN KEY qu'une table peut contenir et qui référencent d'autres tables ou du nombre de contraintes FOREIGN KEY possédées par d'autres tables qui font référence à une table spécifique.

  Cependant, le nombre réel de contraintes FOREIGN KEY qui peuvent être utilisées est limité par la configuration matérielle et par la conception de la base de données et de l'application. Nous vous recommandons de ne pas insérer plus de 253 contraintes FOREIGN KEY dans une table et qu'une même table ne soit pas référencée par plus de 253 contraintes FOREIGN KEY. La limite effective pour vous peut varier en fonction de l'application et du matériel. Prenez en compte le coût d'application des contraintes FOREIGN KEY avant de concevoir vos bases de données et applications.

- Les contraintes FOREIGN KEY ne sont pas appliquées dans les tables temporaires.
- Les contraintes FOREIGN KEY ne peuvent référencer que les colonnes dans des contraintes PRIMARY KEY ou UNIQUE dans la table référencée ou dans un index UNIQUE INDEX de la table référencée.
- Si une clé étrangère est définie sur une colonne avec le type de données CLR défini par l'utilisateur, l'implémentation du type doit prendre en charge le tri binaire. Pour plus d’informations, consultez [Types CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).
- Les colonnes participant à une relation de clé étrangère doivent être définies avec la même longueur et la même échelle.

## <a name="default-definitions"></a>définitions DEFAULT

- Une colonne ne peut avoir qu'une seule définition DEFAULT (valeur par défaut).
- Une définition DEFAULT peut contenir des valeurs constantes, des fonctions, des fonctions niladiques SQL standard ou des valeurs NULL. Le tableau suivant montre les fonctions niladiques et les valeurs qu'elles retournent pour la valeur par défaut, lors d'une instruction INSERT.

    |Fonction niladique SQL-92|Valeur retournée|
    |------------------------------|--------------------|
    |CURRENT_TIMESTAMP|Date et heure actuelles.|
    |CURRENT_USER|Nom de l'utilisateur effectuant une insertion.|
    |SESSION_USER|Nom de l'utilisateur effectuant une insertion.|
    |SYSTEM_USER|Nom de l'utilisateur effectuant une insertion.|
    |Utilisateur|Nom de l'utilisateur effectuant une insertion.|
- *constant_expression* dans une définition DEFAULT ne peut pas faire référence à une autre colonne de la table, ou à d’autres tables, vues ou procédures stockées.
- Les définitions DEFAULT ne peuvent pas être créées dans des colonnes ayant un type de données **timestamp** ou une propriété IDENTITY.
- Les définitions DEFAULT ne peuvent pas être créées pour des colonnes qui possèdent des types de données d'alias, si ces types de données sont liés à un objet par défaut.

## <a name="check-constraints"></a>Contraintes CHECK

- Une colonne peut posséder un nombre illimité de contraintes CHECK et la condition peut inclure plusieurs expressions logiques combinées par AND et OR. S'il existe plusieurs contraintes CHECK pour une même colonne, elles sont validées dans l'ordre de leur création.
- La condition de recherche doit correspondre à une expression booléenne et ne peut pas faire référence à une autre table.
- Une contrainte CHECK de niveau colonne ne peut faire référence qu'à la colonne contenant la contrainte, et une contrainte CHECK de niveau table ne peut faire référence qu'aux colonnes d'une même table.

  Les contraintes CHECK et les règles servent toutes les deux à valider les données lors des instructions INSERT et UPDATE.

- Quand il existe une règle et une ou plusieurs contraintes CHECK pour une colonne, toutes les restrictions sont évaluées.
- Les contraintes CHECK ne peuvent pas être définies sur des colonnes **text**, **ntext** ou **image**.

## <a name="additional-constraint-information"></a>Informations supplémentaires sur les contraintes

- Un index créé pour une contrainte ne peut pas être supprimé en utilisant `DROP INDEX` ; la contrainte doit être supprimée à l'aide de `ALTER TABLE`. Un index créé pour une contrainte et utilisé par elle peut être recréé en utilisant `ALTER INDEX ... REBUILD`. Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).
- Les noms de contrainte doivent suivre les règles des [identificateurs](../../relational-databases/databases/database-identifiers.md), sauf que le nom ne peut pas commencer par un signe dièse (#). Si *constraint_name* n’est pas spécifié, un nom généré par le système est affecté à la contrainte. Le nom de la contrainte apparaît dans tous les messages d'erreur relatifs aux violations de contraintes.
- Quand il y a une violation de contrainte dans une instruction `INSERT`, `UPDATE` ou `DELETE`, l’exécution de l’instruction est arrêtée. Cependant, si `SET XACT_ABORT` a la valeur OFF, et que l’instruction fait partie d’une transaction explicite, la transaction continue de s’exécuter. Si `SET XACT_ABORT` a la valeur ON, toute la transaction est annulée. Vous pouvez également utiliser l’instruction `ROLLBACK TRANSACTION` avec la définition de la transaction en vérifiant la fonction système `@@ERROR`.
- Si `ALLOW_ROW_LOCKS = ON` et `ALLOW_PAGE_LOCK = ON`, les verrous au niveau de la ligne, de la page et de la table sont autorisés au moment où vous accédez à l’index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] choisit le verrou approprié et peut promouvoir un verrou de ligne ou de page en verrou de table. Si `ALLOW_ROW_LOCKS = OFF` et `ALLOW_PAGE_LOCK = OFF`, seul un verrou au niveau de la table est autorisé au moment où vous accédez à l’index.
- Si une table contient des contraintes FOREIGN KEY ou CHECK, et des déclencheurs, les conditions de la contrainte sont évaluées avant l'exécution du déclencheur.

Pour obtenir des informations sur une table et ses colonnes, utilisez `sp_help` ou `sp_helpconstraint`. Pour renommer une table, utilisez `sp_rename`. Pour obtenir un rapport sur les vues et procédures stockées qui dépendent d’une table, utilisez [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) et [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md).

## <a name="nullability-rules-within-a-table-definition"></a>Règles des possibilités de valeurs Null dans une définition de table

La possibilité de valeurs Null pour une colonne détermine si cette colonne peut accepter une valeur Null (NULL) comme données dans la colonne. La valeur NULL n’est pas équivalente à la valeur zéro ou à un blanc : cela signifie qu’il n’y a pas eu d’entrée dans la colonne ou que la valeur NULL explicite a été spécifiée. Cela implique généralement que la valeur est inconnue ou non applicable.

Quand vous utilisez `CREATE TABLE` ou `ALTER TABLE` pour créer ou modifier une table, les paramètres de la base de données et de la session influencent et éventuellement modifient la possibilité de valeur NULL pour le type de données utilisé dans une définition de colonne. Il est recommandé de toujours définir explicitement une colonne comme NULL ou NOT NULL ou, si vous utilisez un type de données défini par l'utilisateur, d'autoriser la colonne à utiliser la possibilité de valeur NULL par défaut pour ce type de données. Les colonnes éparses doivent toujours autoriser les valeurs NULL.

Lorsque vous ne l'avez pas spécifiée explicitement, la possibilité de valeurs Null pour les colonnes respecte les règles récapitulées dans le tableau suivant :

|Type de données de colonne|Règle|
|----------------------|----------|
|Type de données d'alias|Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise la possibilité de valeurs Null spécifiée lors de la création du type de données. Utilisez **sp_help** pour obtenir la possibilité de valeurs Null par défaut du type de données.|
|type CLR défini par l'utilisateur|La possibilité de valeur NULL est déterminée en fonction de la définition de la colonne.|
|Type de données fourni par le système|Si le type de données fourni par le système ne possède qu'une option, il a priorité. Les types de données **timestamp** doivent être NOT NULL. Lorsque les paramètres de session ont pour valeur ON en utilisant SET :<br />**ANSI_NULL_DFLT_ON** = ON, NULL est affecté. <br />**ANSI_NULL_DFLT_OFF** = ON, NOT NULL est affecté.<br /><br /> Lorsque les paramètres de base de données sont configurés en utilisant ALTER DATABASE :<br />**ANSI_NULL_DEFAULT_ON** = ON, NULL est affecté. <br />**ANSI_NULL_DEFAULT_OFF** = ON, NOT NULL est affecté.<br /><br /> Pour voir le paramètre de la base de données pour ANSI_NULL_DEFAULT, utilisez la vue de catalogue **sys.databases**.|

Si aucune des options ANSI_NULL_DFLT n’est définie pour la session et si la base de données est définie avec les valeurs par défaut (ANSI_NULL_DEFAULT est OFF), la valeur par défaut, NOT NULL, est affectée.

La possibilité de valeurs NULL dans les colonnes calculées est déterminée automatiquement par le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Pour déterminer si ce type de colonne accepte ou non les valeurs NULL, utilisez la fonction `COLUMNPROPERTY` avec la propriété **AllowsNull**.

> [!NOTE]
> Que ce soit pour le pilote ODBC de SQL Server ou le pilote OLE DB de SQL Server, ANSI_NULL_DFLT_ON a la valeur ON par défaut. Les utilisateurs ODBC et OLE DB peuvent réaliser cette configuration dans les sources de données ODBC ou à l'aide d'attributs ou de propriétés de connexion définies par l'application.

## <a name="data-compression"></a>Data Compression

Les tables système ne peuvent pas être activées pour la compression. Lorsque vous créez une table ou un index, la compression de données est définie sur NONE, sauf indication contraire. Si vous spécifiez une liste de partitions ou une partition hors limites, une erreur est générée. Pour plus d’informations sur la compression de données, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).

Pour évaluer la façon dont la modification de l’état de compression affecte une table, un index ou une partition, utilisez la procédure stockée [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) .

## <a name="permissions"></a>Autorisations

Nécessite l’autorisation `CREATE TABLE` dans la base de données et l’autorisation `ALTER` sur le schéma dans lequel la table est créée.

Si des colonnes dans l’instruction `CREATE TABLE` sont définies avec un type de données défini par l’utilisateur, une autorisation `REFERENCES` est nécessaire sur ce type.

Si des colonnes dans l’instruction `CREATE TABLE` sont définies avec le type de données CLR défini par l’utilisateur, la propriété du type ou l’autorisation `REFERENCES` est nécessaire.

Si des colonnes dans l’instruction `CREATE TABLE` sont associées à une collection de schémas XML, la propriété de la collection de schémas XML ou l’autorisation `REFERENCES` est nécessaire.

Tous les utilisateurs peuvent créer des tables temporaires dans tempdb.

## <a name="examples"></a>Exemples

### <a name="a-create-a-primary-key-constraint-on-a-column"></a>R. Créer une contrainte PRIMARY KEY dans une colonne

L'exemple suivant affiche la définition de colonne pour une contrainte PRIMARY KEY avec un index cluster sur la colonne `EmployeeID` de la table `Employee`. Étant donné que le nom de la contrainte n'est pas spécifié, le système en fournit un.

```sql
CREATE TABLE dbo.Employee (
    EmployeeID INT PRIMARY KEY CLUSTERED
);
```

### <a name="b-using-foreign-key-constraints"></a>B. Utilisation des contraintes FOREIGN KEY

Une contrainte FOREIGN KEY sert à référencer une autre table. Les clés étrangères peuvent être des clés à colonne unique ou sur plusieurs colonnes. Cet exemple montre une contrainte FOREIGN KEY à colonne unique dans la table `SalesOrderHeader` qui fait référence à la table `SalesPerson`. Seule la clause REFERENCES est obligatoire pour une contrainte FOREIGN KEY à colonne unique.

```sql
SalesPersonID INT NULL REFERENCES SalesPerson(SalesPersonID)
```

Vous pouvez également utiliser de manière explicite la clause FOREIGN KEY et redéterminer l'attribut de la colonne. Notez que le nom de la colonne ne doit pas nécessairement être le même dans les deux tables.

```sql
FOREIGN KEY (SalesPersonID) REFERENCES SalesPerson(SalesPersonID)
```

Les contraintes de clés sur plusieurs colonnes sont créées comme des contraintes de table. Dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la table `SpecialOfferProduct` inclut une clause PRIMARY KEY sur plusieurs colonnes. L'exemple suivant montre comment faire référence à cette clé à partir d'une autre table ; un nom de contrainte explicite n'est pas obligatoire.

```sql
CONSTRAINT FK_SpecialOfferProduct_SalesOrderDetail
    FOREIGN KEY (ProductID, SpecialOfferID)
    REFERENCES SpecialOfferProduct (ProductID, SpecialOfferID)
```

### <a name="c-using-unique-constraints"></a>C. Utilisation des contraintes UNIQUE

Les contraintes UNIQUE servent à garantir l'unicité dans les colonnes qui n'ont pas de clés primaires. L'exemple suivant applique la restriction suivant laquelle la colonne `Name` de la table `Product` doit être unique.

```sql
Name NVARCHAR(100) NOT NULL
UNIQUE NONCLUSTERED
```

### <a name="d-using-default-definitions"></a>D. Utilisation des définitions DEFAULT

Les valeurs par défaut fournissent une valeur (avec les instructions INSERT et UPDATE) lorsqu'aucune valeur n'est fournie. Par exemple, la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] peut inclure une table de correspondance répertoriant les différents emplois que les employés peuvent occuper dans la société. Sous une colonne décrivant chaque emploi, une chaîne de caractères par défaut peut fournir une description lorsqu'aucune description réelle n'est entrée explicitement.

```sql
DEFAULT 'New Position - title not formalized yet'
```

Outre des constantes, les définitions DEFAULT peuvent inclure des fonctions. Utilisez l'exemple suivant pour obtenir la date actuelle d'une entrée.

```sql
DEFAULT (GETDATE())
```

Une fonction niladique peut également améliorer l'intégrité des données. Afin de garder une trace de l'utilisateur qui a inséré une ligne, utilisez la fonction niladique pour USER. N'entourez pas les fonctions niladiques de parenthèses.

```sql
DEFAULT USER
```

### <a name="e-using-check-constraints"></a>E. Utilisation des contraintes CHECK

L'exemple suivant affiche une restriction appliquée aux valeurs entrées dans la colonne `CreditRating` de la table `Vendor`. La contrainte n'a pas de nom.

```sql
CHECK (CreditRating >= 1 and CreditRating <= 5)
```

Cet exemple montre une contrainte nommée avec un modèle de restriction pour les données caractères entrées dans une colonne d'une table.

```sql
CONSTRAINT CK_emp_id CHECK (
    emp_id LIKE '[A-Z][A-Z][A-Z][1-9][0-9][0-9][0-9][0-9][FM]'
    OR emp_id LIKE '[A-Z]-[A-Z][1-9][0-9][0-9][0-9][0-9][FM]'
)
```

Cet exemple spécifie que les valeurs doivent figurer dans une liste spécifique ou suivre un modèle donné.

```sql
CHECK (
    emp_id IN ('1389', '0736', '0877', '1622', '1756')
    OR emp_id LIKE '99[0-9][0-9]'
)
```

### <a name="f-showing-the-complete-table-definition"></a>F. Affichage des définitions de tables complètes

Cet exemple montre des définitions complètes de tables avec toutes les définitions de contraintes pour la table `PurchaseOrderDetail` créée dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Notez que pour exécuter l'exemple, le schéma de table est modifié en `dbo`.

```sql
CREATE TABLE dbo.PurchaseOrderDetail
(
    PurchaseOrderID int NOT NULL
        REFERENCES Purchasing.PurchaseOrderHeader(PurchaseOrderID),
    LineNumber smallint NOT NULL,
    ProductID int NULL
        REFERENCES Production.Product(ProductID),
    UnitPrice money NULL,
    OrderQty smallint NULL,
    ReceivedQty float NULL,
    RejectedQty float NULL,
    DueDate datetime NULL,
    rowguid uniqueidentifier ROWGUIDCOL NOT NULL
        CONSTRAINT DF_PurchaseOrderDetail_rowguid DEFAULT (NEWID()),
    ModifiedDate datetime NOT NULL
        CONSTRAINT DF_PurchaseOrderDetail_ModifiedDate DEFAULT (GETDATE()),
    LineTotal AS ((UnitPrice*OrderQty)),
    StockedQty AS ((ReceivedQty-RejectedQty)),
    CONSTRAINT PK_PurchaseOrderDetail_PurchaseOrderID_LineNumber
               PRIMARY KEY CLUSTERED (PurchaseOrderID, LineNumber)
               WITH (IGNORE_DUP_KEY = OFF)
)
ON PRIMARY;
```

### <a name="g-creating-a-table-with-an-xml-column-typed-to-an-xml-schema-collection"></a>G. Création d'une table avec une colonne XML de type collection de schémas XML

L'exemple suivant crée une table avec une colonne `xml` de type collection de schémas XML `HRResumeSchemaCollection`. Le mot clé `DOCUMENT` spécifie que chaque instance du type de données `xml` dans *column_name* ne peut contenir qu’un seul élément de niveau supérieur.

```sql
CREATE TABLE HumanResources.EmployeeResumes
(
    LName nvarchar(25),
    FName nvarchar(25),
    Resume xml(DOCUMENT HumanResources.HRResumeSchemaCollection)
);
```

### <a name="h-creating-a-partitioned-table"></a>H. Création d'une table partitionnée

L'exemple suivant crée une fonction de partition pour partitionner une table ou un index en quatre partitions. L'exemple crée ensuite un schéma de partition pour spécifier les groupes de fichiers qui contiendront chacune des quatre partitions. Enfin, l'exemple crée une table qui utilise le schéma de partition. Cet exemple suppose que les groupes de fichiers existent déjà dans la base de données.

```sql
CREATE PARTITION FUNCTION myRangePF1 (int)
    AS RANGE LEFT FOR VALUES (1, 100, 1000);
GO

CREATE PARTITION SCHEME myRangePS1
    AS PARTITION myRangePF1
    TO (test1fg, test2fg, test3fg, test4fg);
GO  
  
CREATE TABLE PartitionTable (col1 int, col2 char(10))
    ON myRangePS1 (col1);
GO
```

En fonction des valeurs de la colonne `col1` de `PartitionTable`, les partitions sont affectées des manières suivantes.

|Groupe de fichiers|test1fg|test2fg|test3fg|test4fg|
|---------------|-------------|-------------|-------------|-------------|
|**Partition**|1|2|3|4|
|**Valeurs**|col 1 \<= 1|col1 > 1 AND col1 \<= 100|col1 > 100 AND col1 \<= 1,000|col1 > 1000|

### <a name="i-using-the-uniqueidentifier-data-type-in-a-column"></a>I. Utilisation du type de données uniqueidentifier dans une colonne

L'exemple suivant crée une table avec une colonne `uniqueidentifier`. Il utilise une contrainte PRIMARY KEY pour empêcher les utilisateurs de la table d'insérer des doublons, et la fonction `NEWSEQUENTIALID()` dans la contrainte `DEFAULT` pour fournir des valeurs aux nouvelles lignes. La propriété ROWGUIDCOL est appliquée à la colonne `uniqueidentifier` de sorte qu'elle peut être référencée en utilisant le mot clé $ROWGUID.

```sql
CREATE TABLE dbo.Globally_Unique_Data
(
    GUID UNIQUEIDENTIFIER
        CONSTRAINT Guid_Default DEFAULT
        NEWSEQUENTIALID() ROWGUIDCOL,
    Employee_Name VARCHAR(60)
    CONSTRAINT Guid_PK PRIMARY KEY (GUID)
);
```

### <a name="j-using-an-expression-for-a-computed-column"></a>J. Utilisation d'une expression pour une colonne calculée

L'exemple suivant illustre l'utilisation d'une expression (`(low + high)/2`) pour le calcul de la colonne calculée `myavg`.

```sql
CREATE TABLE dbo.mytable
(
    low INT,
    high INT,
    myavg AS (low + high)/2
);
```

### <a name="k-creating-a-computed-column-based-on-a-user-defined-type-column"></a>K. Création d'une colonne calculée basée sur une colonne de type défini par l'utilisateur

L'exemple suivant crée une table, avec une colonne définie comme `utf8string` dont le type de données est défini par l'utilisateur, en supposant que l'assembly du type et le type lui-même ont déjà été créés dans la base de données active. Une seconde colonne est définie d’après `utf8string` et utilise la méthode `ToString()` de **type(class)** `utf8string` pour calculer une valeur pour la colonne.

```sql
CREATE TABLE UDTypeTable
(
    u UTF8STRING,
    ustr AS u.ToString() PERSISTED
);
```

### <a name="l-using-the-user_name-function-for-a-computed-column"></a>L. Utilisation de la fonction USER_NAME pour une colonne calculée

L'exemple suivant utilise la fonction `USER_NAME()` dans la colonne `myuser_name`.

```sql
CREATE TABLE dbo.mylogintable
(
    date_in DATETIME,
    user_id INT,
    myuser_name AS USER_NAME()
);
```

### <a name="m-creating-a-table-that-has-a-filestream-column"></a>M. Création d'une table qui comporte une colonne FILESTREAM

L'exemple suivant crée une table qui comporte une colonne `FILESTREAM``Photo`. Si une table comporte une un ou plusieurs colonnes `FILESTREAM`, elle doit aussi comporter une colonne `ROWGUIDCOL`.

```sql
CREATE TABLE dbo.EmployeePhoto
(
    EmployeeId INT NOT NULL PRIMARY KEY,
    Photo VARBINARY(MAX) FILESTREAM NULL,
    MyRowGuidColumn UNIQUEIDENTIFIER NOT NULL ROWGUIDCOL UNIQUE DEFAULT NEWID()
);
```

### <a name="n-creating-a-table-that-uses-row-compression"></a>N. Création d'une table qui utilise la compression de ligne

L'exemple suivant crée une table qui utilise la compression de ligne.

```sql
CREATE TABLE dbo.T1
(
    c1 INT,
    c2 NVARCHAR(200)
)
WITH (DATA_COMPRESSION = ROW);
```

Pour obtenir d’autres exemples de compression de données, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md).

### <a name="o-creating-a-table-that-has-sparse-columns-and-a-column-set"></a>O. Création d'une table qui comporte des colonnes éparses et un jeu de colonnes

Les exemples suivants montrent comment créer une table qui comporte une colonne éparse et une table qui comporte deux colonnes éparses et un jeu de colonnes. Ces exemples utilisent la syntaxe de base. Pour obtenir des exemples plus complexes, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md) et [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).

Cet exemple crée une table qui comporte une colonne éparse.

```sql
CREATE TABLE dbo.T1
(
    c1 INT PRIMARY KEY,
    c2 VARCHAR(50) SPARSE NULL
);
```

Cet exemple crée une table qui comporte deux colonnes éparses et un jeu de colonnes nommé `CSet`.

```sql
CREATE TABLE T1
(
    c1 INT PRIMARY KEY,
    c2 VARCHAR(50) SPARSE NULL,
    c3 INT SPARSE NULL,
    CSet XML COLUMN_SET FOR ALL_SPARSE_COLUMNS
);
```

### <a name="p-creating-a-system-versioned-disk-based-temporal-table"></a>P. Création d’une table temporelle sur disque avec version système

**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Les exemples suivants montrent comment créer une table temporelle liée à une nouvelle table d’historique, et comment créer une table temporelle liée à une table d’historique existante. Notez que la table temporelle doit avoir une clé primaire définie pour que la table puisse être activée pour la gestion système des versions. Pour obtenir des exemples montrant comment ajouter ou supprimer la gestion système des versions sur une table existante, consultez Gestion système des versions dans [Exemples](../../t-sql/statements/alter-table-transact-sql.md#Example_Top). Pour plus d’informations sur les cas d’usage, consultez [Tables temporelles](../../relational-databases/tables/temporal-tables.md).

Cet exemple crée une table temporelle liée à une nouvelle table d’historique.

```sql
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON);
```

Cet exemple crée une table temporelle liée à une table d’historique existante.

```sql
-- Existing table
CREATE TABLE Department_History
(
    DepartmentNumber CHAR(10) NOT NULL,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 NOT NULL,
    SysEndTime DATETIME2 NOT NULL
);

-- Temporal table
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Department_History, DATA_CONSISTENCY_CHECK = ON));
```

### <a name="q-creating-a-system-versioned-memory-optimized-temporal-table"></a>Q. Création d’une table temporelle à mémoire optimisée avec version système

**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

L’exemple suivant montre comment créer une table temporelle à mémoire optimisée avec version système liée à une nouvelle table d’historique sur disque.

Cet exemple crée une table temporelle liée à une nouvelle table d’historique.

```sql
CREATE SCHEMA History;
GO

CREATE TABLE dbo.Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY NONCLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH
(
    MEMORY_OPTIMIZED = ON,
    DURABILITY = SCHEMA_AND_DATA,
    SYSTEM_VERSIONING = ON (HISTORY_TABLE = History.DepartmentHistory)
);
```

Cet exemple crée une table temporelle liée à une table d’historique existante.

```sql
-- Existing table
CREATE TABLE Department_History
(
    DepartmentNumber CHAR(10) NOT NULL,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 NOT NULL,
    SysEndTime DATETIME2 NOT NULL
);

-- Temporal table
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH
(
    SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Department_History, DATA_CONSISTENCY_CHECK = ON)
);
```

### <a name="r-creating-a-table-with-encrypted-columns"></a>R. Création d’une table avec colonnes chiffrées

L’exemple suivant crée une table avec deux colonnes chiffrées. Pour plus d’informations, consultez [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

```sql
CREATE TABLE Customers (
    CustName NVARCHAR(60)
        ENCRYPTED WITH (
            COLUMN_ENCRYPTION_KEY = MyCEK,
            ENCRYPTION_TYPE = RANDOMIZED,
            ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ),
    SSN VARCHAR(11) COLLATE Latin1_General_BIN2
        ENCRYPTED WITH (
            COLUMN_ENCRYPTION_KEY = MyCEK,
            ENCRYPTION_TYPE = DETERMINISTIC ,
            ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ),
    Age INT NULL
);
```

### <a name="s-create-an-inline-filtered-index"></a>S. Créer un index filtré inline

Crée une table avec un index filtré inline.

```sql
CREATE TABLE t1
(
    c1 INT,
    index IX1 (c1) WHERE c1 > 0
);
```

### <a name="t-create-an-inline-index"></a>T. Créer un index inline

L’exemple suivant montre comment utiliser un index inline NONCLUSTERED pour les tables sur disque :

```sql
CREATE TABLE t1
(
    c1 INT,
    INDEX ix_1 NONCLUSTERED (c1)
);

CREATE TABLE t2
(
    c1 INT,
    c2 INT INDEX ix_1 NONCLUSTERED
);

CREATE TABLE t3
(
    c1 INT,
    c2 INT,
    INDEX ix_1 NONCLUSTERED (c1,c2)
);
```

### <a name="u-create-a-temporary-table-with-an-anonymously-named-compound-primary-key"></a>U. Créer une table temporaire avec une clé primaire composée nommée de manière anonyme

Crée une table avec une clé primaire composée nommée de manière anonyme. Cela est utile pour éviter les conflits de temps d’exécution, où deux tables temporaires dans l’étendue de session, chacune d’elles dans une session distincte, utilisent le même nom pour une contrainte.

```sql
CREATE TABLE #tmp
(
    c1 INT,
    c2 INT,
    PRIMARY KEY CLUSTERED ([c1], [c2])
);
GO
```

Si vous nommez explicitement la contrainte, la deuxième session générera une erreur telle que :

```
Msg 2714, Level 16, State 5, Line 1
There is already an object named 'PK_#tmp' in the database.
Msg 1750, Level 16, State 1, Line 1
Could not create constraint or index. See previous errors.
```

Le problème est dû au fait que, bien que le nom de table temporaire soit unique, les noms des contraintes ne le sont pas.

### <a name="v-using-global-temporary-tables-in-azure-sql-database"></a>V. Utiliser des tables temporaires globales dans Azure SQL Database

La session A crée une table temporaire globale ##test dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb1 et ajoute une ligne.

```sql
CREATE TABLE ##test (
    a INT,
    b INT
);

INSERT INTO ##test
VALUES (1, 1);

-- Obtain object ID for temp table ##test
SELECT OBJECT_ID('tempdb.dbo.##test') AS 'Object ID';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
1253579504
```

Obtenir le nom de la table temporaire globale pour un objet ayant l’ID 1253579504 dans tempdb (2)

```sql
SELECT name FROM tempdb.sys.objects WHERE object_id = 1253579504;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
##test
```

La session B se connecte à [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb1 et peut accéder à la table ##test créée par la session A.

```sql
SELECT * FROM ##test;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
1, 1
```

La session C se connecte à une autre base de données dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] testdb2 et veut accéder à la base de données ##test créée dans testdb1. Cette instruction select échoue à cause de l’étendue de la base de données pour les tables temporaires globales.

```sql
SELECT * FROM ##test
```

Ce qui génère l’erreur suivante :

```
Msg 208, Level 16, State 0, Line 1
Invalid object name '##test'
```

Adressage des objets système dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] tempdb à partir de la base de données utilisateur active testdb1

```sql
SELECT * FROM tempdb.sys.objects;
SELECT * FROM tempdb.sys.columns;
SELECT * FROM tempdb.sys.database_files;
```   

### <a name="w-enable-data-retention-policy-on-a-table"></a>Heure standard Activer la stratégie de rétention des données sur une table

L’exemple suivant crée une table avec la rétention des données activée et une période de rétention d’une semaine. Cet exemple s’applique à **Azure SQL Edge** uniquement.

```sql
CREATE TABLE [dbo].[data_retention_table] 
(
  [dbdatetime2] datetime2(7), 
  [product_code] int, 
  [value] char(10)
) 
WITH (DATA_DELETION = ON ( FILTER_COLUMN = [dbdatetime2], RETENTION_PERIOD = 1 WEEKS ))
```

## <a name="next-steps"></a>Étapes suivantes

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md)
- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)
- [CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)
- [Data types](../../t-sql/data-types/data-types-transact-sql.md)
- [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)
- [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)
- [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)
- [DROP TABLE](../../t-sql/statements/drop-table-transact-sql.md)
- [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md)
- [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md)
- [CREATE TYPE](../../t-sql/statements/create-type-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_help](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)
- [sp_helpconstraint](../../relational-databases/system-stored-procedures/sp-helpconstraint-transact-sql.md)
- [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
