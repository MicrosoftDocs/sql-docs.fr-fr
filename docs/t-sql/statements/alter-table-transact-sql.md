---
description: ALTER TABLE (Transact-SQL)
title: ALTER TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WAIT_AT_LOW_PRIORITY
- ABORT_AFTER_WAIT
- ABORT_AFTER_WAIT_TSQL
- ALTER_TABLE_TSQL
- ALTER TABLE
- WAIT_AT_LOW_PRIORITY_TSQL
- ALTER_COLUMN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- columns [SQL Server], resizing
- changing column size
- MAXDOP index option, ALTER TABLE statement
- table modifications [SQL Server], ALTER TABLE
- ALTER TABLE statement
- modifying tables
- partitioned tables [SQL Server], lock escalation
- resizing columns
- removing columns
- switching partitions
- reassigning partitions
- removing constraints
- triggers [SQL Server], disabling
- columns [SQL Server], adding
- LOCK_ESCALATION option of ALTER TABLE
- constraints [SQL Server], deleting
- constraints [SQL Server], disabling
- triggers [SQL Server], enabling
- re-enabling constraints
- index modifications [SQL Server]
- disabling constraints
- columns [SQL Server], removing
- max degree of parallelism option
- locking [SQL Server], tables
- ONLINE option
- disabling triggers
- constraints [SQL Server], adding
- deleting constraints
- adding constraints
- adding columns
- SWITCH partitions
- partitioned tables [SQL Server], switching
- lock escalation [SQL Server], option of ALTER TABLE
- constraints [SQL Server], enabling
- dropping constraints
- dropping columns
- data retention policy
- table changes [SQL Server]
ms.assetid: f1745145-182d-4301-a334-18f799d361d1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 14c663264dc05f403f12e9cad390cd0ef52f17c1
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100348499"
---
# <a name="alter-table-transact-sql"></a>ALTER TABLE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Modifie une définition de table en modifiant, ajoutant ou déposant des colonnes et des contraintes. ALTER TABLE réaffecte et reconstruit également des partitions, ou désactive et active des contraintes et des déclencheurs.

Pour plus d’informations sur les conventions de la syntaxe, consultez [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

> [!IMPORTANT]
> La syntaxe ALTER TABLE est différente pour les tables basées sur disque et les tables à mémoire optimisée. Utilisez les liens suivants pour accéder directement au bloc de syntaxe approprié pour vos types de tables et aux exemples de syntaxe appropriés :
>
> - Tables basées sur disque :
>
>   - [Syntaxe](#syntax-for-disk-based-tables)
>   - [Exemples](#Example_Top)
> - Tables optimisées en mémoire
>
>   - [Syntaxe](#syntax-for-memory-optimized-tables)
>   - [Exemples](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)

## <a name="syntax-for-disk-based-tables"></a>Syntaxe des tables basées sur disque

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                 | max
                 | xml_schema_collection
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ] [ SPARSE ]
      | { ADD | DROP }
          { ROWGUIDCOL | PERSISTED | NOT FOR REPLICATION | SPARSE | HIDDEN }
      | { ADD | DROP } MASKED [ WITH ( FUNCTION = ' mask_function ') ]
    }
    [ WITH ( ONLINE = ON | OFF ) ]
    | [ WITH { CHECK | NOCHECK } ]
  
    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <column_set_definition>
    } [ ,...n ]
      | [ system_start_time_column_name datetime2 GENERATED ALWAYS AS ROW START
                [ HIDDEN ] [ NOT NULL ] [ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
                system_end_time_column_name datetime2 GENERATED ALWAYS AS ROW END
                   [ HIDDEN ] [ NOT NULL ][ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
        ]
       PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )
    | DROP
     [ {
         [ CONSTRAINT ][ IF EXISTS ]
         {
              constraint_name
              [ WITH
               ( <drop_clustered_constraint_option> [ ,...n ] )
              ]
          } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }
  
    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | { ENABLE | DISABLE } CHANGE_TRACKING
        [ WITH ( TRACK_COLUMNS_UPDATED = { ON | OFF } ) ]
  
    | SWITCH [ PARTITION source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]

    | SET
        (
            [ FILESTREAM_ON =
                { partition_scheme_name | filegroup | "default" | "NULL" } ]
            | SYSTEM_VERSIONING =
                  {
                      OFF
                  | ON
                      [ ( HISTORY_TABLE = schema_name . history_table_name
                          [, DATA_CONSISTENCY_CHECK = { ON | OFF } ]
                          [, HISTORY_RETENTION_PERIOD =
                          {
                              INFINITE | number {DAY | DAYS | WEEK | WEEKS
                  | MONTH | MONTHS | YEAR | YEARS }
                          }
                          ]
                        )
                      ]
                  }
            | DATA_DELETION =  
                {
                      OFF 
                    | ON  
                        [(  [ FILTER_COLUMN = column_name ]   
                            [, RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS 
                                    | MONTH | MONTHS | YEAR | YEARS }}]   
                        )]
                   }
    | REBUILD
      [ [PARTITION = ALL]
        [ WITH ( <rebuild_option> [ ,...n ] ) ]
      | [ PARTITION = partition_number
           [ WITH ( <single_partition_rebuild_option> [ ,...n ] ) ]
        ]
      ]
  
    | <table_option>
    | <filetable_option>
    | <stretch_configuration>
}
[ ; ]
  
-- ALTER TABLE options
  
<column_set_definition> ::=
    column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS

<drop_clustered_constraint_option> ::=
    {
        MAXDOP = max_degree_of_parallelism
      | ONLINE = { ON | OFF }
      | MOVE TO
         { partition_scheme_name ( column_name ) | filegroup | "default" }
    }
<table_option> ::=
    {
        SET ( LOCK_ESCALATION = { AUTO | TABLE | DISABLE } )
    }
  
<filetable_option> ::=
    {
       [ { ENABLE | DISABLE } FILETABLE_NAMESPACE ]
       [ SET ( FILETABLE_DIRECTORY = directory_name ) ]
    }
  
<stretch_configuration> ::=
    {
      SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options>)
          | = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED )
          | ( <table_stretch_options> [, ...n] )
        }
            )
    }
  
<table_stretch_options> ::=
    {
     [ FILTER_PREDICATE = { null | table_predicate_function } , ]
       MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }
    }
  
<single_partition_rebuild__option> ::=
{
      SORT_IN_TEMPDB = { ON | OFF }
    | MAXDOP = max_degree_of_parallelism
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE} }
    | ONLINE = { ON [( <low_priority_lock_wait> ) ] | OFF }
}
  
<low_priority_lock_wait>::=
{
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ],
        ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )
}
```

> [!NOTE]
> Si vous souhaitez en savoir plus, veuillez consulter :
>
> - [ALTER TABLE column_constraint](alter-table-column-constraint-transact-sql.md)
> - [ALTER TABLE column_definition](alter-table-column-definition-transact-sql.md)
> - [ALTER TABLE computed_column_definition](alter-table-computed-column-definition-transact-sql.md)
> - [ALTER TABLE index_option](alter-table-index-option-transact-sql.md)
> - [ALTER TABLE table_constraints](alter-table-table-constraint-transact-sql.md)

## <a name="syntax-for-memory-optimized-tables"></a>Syntaxe des tables à mémoire optimisée

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ]
    }

    | ALTER INDEX index_name
    {
        [ type_schema_name. ] type_name
        REBUILD
        [ [ NONCLUSTERED ] WITH ( BUCKET_COUNT = bucket_count )
        ]
    }

    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <table_index>
      | <column_index>
    } [ ,...n ]
  
    | DROP
     [ {
         CONSTRAINT [ IF EXISTS ]
         {
              constraint_name
          } [ ,...n ]
        | INDEX [ IF EXISTS ]
      {
         index_name
       } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }

    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | SWITCH [ [ PARTITION ] source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]
    
}
[ ; ]

-- ALTER TABLE options

< table_constraint > ::=
 [ CONSTRAINT constraint_name ]
{
   {PRIMARY KEY | UNIQUE }
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
{[ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count)
  | [ NONCLUSTERED ] (column [ ASC | DESC ] [ ,... n ] )
      [ ON filegroup_name | default ]
  | CLUSTERED COLUMNSTORE [WITH ( COMPRESSION_DELAY = {0 | delay [Minutes]})]
      [ ON filegroup_name | default ]
}

```

## <a name="syntax-for-azure-synapse-analytics-and-parallel-data-warehouse"></a>Syntaxe pour Azure Synapse Analytics et Parallel Data Warehouse

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

ALTER TABLE { database_name.schema_name.source_table_name | schema_name.source_table_name | source_table_name }
{
    ALTER COLUMN column_name
        {
            type_name [ ( precision [ , scale ] ) ]
            [ COLLATE Windows_collation_name ]
            [ NULL | NOT NULL ]
        }
    | ADD { <column_definition> | <column_constraint> FOR column_name} [ ,...n ]
    | DROP { COLUMN column_name | [CONSTRAINT] constraint_name } [ ,...n ]
    | REBUILD {
            [ PARTITION = ALL [ WITH ( <rebuild_option> ) ] ]
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_option> ] ]
      }
    | { SPLIT | MERGE } RANGE (boundary_value)
    | SWITCH [ PARTITION source_partition_number
        TO target_table_name [ PARTITION target_partition_number ] [ WITH ( TRUNCATE_TARGET = ON | OFF )
}
[;]

<column_definition>::=
{
    column_name
    type_name [ ( precision [ , scale ] ) ]
    [ <column_constraint> ]
    [ COLLATE Windows_collation_name ]
    [ NULL | NOT NULL ]
}

<column_constraint>::=
    [ CONSTRAINT constraint_name ] 
    {
        DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }
<rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]
}

<single_partition_rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*database_name*  
Nom de la base de données dans laquelle la table a été créée.

*schema_name*  
Nom du schéma auquel appartient la table.

*table_name*  
Nom de la table à modifier. Si la table ne se trouve pas dans la base de données active ou si elle n'est pas contenue dans le schéma appartenant à l'utilisateur actif, vous devez spécifier explicitement la base de données et le schéma.

ALTER COLUMN  
Spécifie que la colonne nommée doit être modifiée.

La colonne modifiée ne peut pas être :

- Une colonne avec un type de données **timestamp**
- la colonne ROWGUIDCOL de la table ;
- une colonne calculée ou utilisée dans une colonne calculée ;
- utilisée dans les statistiques générées par l’instruction CREATE STATISTICS. Les utilisateurs doivent exécuter DROP STATISTICS pour supprimer les statistiques ; sinon, ALTER COLUMN n’aboutit pas.  Exécutez cette requête pour obtenir toutes les statistiques et les colonnes de statistiques créées par l’utilisateur pour une table.

``` sql

SELECT s.name AS statistics_name  
      ,c.name AS column_name  
      ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('<table_name>'); 

```
   > [!NOTE]
   > Les statistiques créées automatiquement par l'optimiseur de requête sont automatiquement supprimées par ALTER COLUMN.

- une colonne utilisée dans une contrainte PRIMARY KEY ou [FOREIGN KEY] REFERENCES ;
- une colonne utilisée dans une contrainte CHECK ou UNIQUE. Mais la modification de la longueur d'une colonne de longueur variable utilisée dans une contrainte CHECK ou UNIQUE est autorisée.
- une colonne associée à une définition par défaut. Cependant, il est possible de modifier la longueur, la précision ou l'échelle d'une colonne si le type de données n'est pas modifié.

Vous ne pouvez modifier le type de données de colonnes **text**, **ntext** et **image** que de l’une des manières suivantes :

- **text** en **varchar(max)** , **nvarchar(max)** ou **xml**
- **ntext** en **varchar(max)** , **nvarchar(max)** ou **xml**
- **image** en **varbinary(max)**

Certaines modifications de type de données peuvent entraîner une modification des données. Par exemple, la conversion d’une colonne de type **nchar** ou **nvarchar** en type **char** ou **varchar** peut entraîner la conversion de caractères étendus. Pour plus d’informations, consultez l’article [CAST et CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md). La réduction de la précision ou de l'échelle d'une colonne peut tronquer les données.

> [!NOTE]
> Vous ne pouvez pas modifier le type de données d'une colonne d'une table partitionnée.
>
> Le type de données des colonnes inclus dans un index ne peut pas être modifié, sauf quand la colonne est du type **varchar**, **nvarchar** ou **varbinary** et que la nouvelle taille est supérieure ou égale à l’ancienne taille.
>
> Les colonnes incluses dans une contrainte de clé primaire ne peuvent pas être modifiées de **NOT NULL** en **NULL**.

Lorsqu’Always Encrypted est utilisé (sans enclaves sécurisées), si la colonne en cours de modification est chiffrée avec 'ENCRYPTED WITH', vous pouvez changer le type de données en un type de données compatible (par ex., INT en BIGINT), mais vous ne pouvez pas changer les paramètres de chiffrement.

Lorsque vous utilisez Always Encrypted avec des enclaves sécurisées, vous pouvez modifier les paramètres de chiffrement si la clé de chiffrement de colonne qui protège la colonne (et la nouvelle clé de chiffrement de colonne si vous modifiez la clé) prennent en charge les calculs d’enclave (chiffrés avec des clés principales de la colonne prenant en charge l’enclave). Pour plus d’informations, consultez [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

*column_name*  
Nom de la colonne à ajouter, modifier ou supprimer. Le maximum pour *column_name* est de 128 caractères. Pour les nouvelles colonnes, vous pouvez omettre *column_name* pour les colonnes créées avec un type de données **timestamp**. Le nom **timestamp** est utilisé si vous ne spécifiez pas de *column_name* pour une colonne du type de données **timestamp**.

> [!NOTE]
> Les nouvelles colonnes sont ajoutées après toutes les colonnes existantes de la table à modifier.

[ _type\_schema\_name_ **.** ] _type\_name_  
Nouveau type de données de la colonne modifiée ou type de données de la colonne ajoutée. Vous ne pouvez pas spécifier *type_name* pour les colonnes existantes de tables partitionnées. *type_name* peut être l’un des types suivants :

- type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;
- type de données alias dérivé d'un type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous créez les types de données alias à l'aide de l'instruction CREATE TYPE avant de pouvoir les utiliser dans une définition de table.
- type [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] défini par l'utilisateur et schéma auquel il appartient. Vous créez des types alias à l'aide de l'instruction CREATE TYPE avant de pouvoir les utiliser dans une définition de table.

Les critères suivants s’appliquent à l’argument *type_name* d’une colonne modifiée :

- Le type de données précédent doit pouvoir être implicitement converti vers le nouveau type de données.
- *type_name* ne peut pas être **timestamp**.
- Les valeurs par défaut ANSI_NULL sont toujours activées pour ALTER COLUMN ; si l'option n'est pas spécifiée, la colonne accepte les valeurs NULL.
- Le remplissage ANSI_PADDING est toujours activé (ON) pour ALTER COLUMN.
- Si la colonne modifiée est une colonne d’identité, *new_data_type* doit être un type de données qui prend en charge la propriété d’identité.
- La configuration actuelle de SET ARITHABORT est ignorée. ALTER TABLE fonctionne comme si l'option ARITHABORT était activée (ON).

> [!NOTE]
> Si la clause COLLATE n'est pas spécifiée, la modification du type de données d'une colonne entraîne une modification du classement, qui est remplacé par le classement par défaut de la base de données.

*precision*  
Précision du type de données spécifié. Pour plus d’informations sur les valeurs de précision valides, consultez [Précision, échelle et longueur](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

*scale*  
Échelle du type de données spécifié. Pour plus d’informations sur les valeurs d’échelle valides, consultez [Précision, échelle et longueur](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

**max**  
S’applique uniquement aux types de données **varchar**, **nvarchar** et **varbinary** pour le stockage de 2^31-1 octets de données caractères, binaires et Unicode.

*xml_schema_collection*  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

S’applique seulement au type de données **xml**, pour associer un schéma XML au type. Avant de définir une colonne de type **xml** dans une collection de schémas, vous commencez par créer la collection de schémas avec [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).

COLLATE \< *collation_name* >  
Spécifie le nouveau classement pour la colonne modifiée. Si l'argument n'est pas spécifié, c'est le classement par défaut de la base de données qui est affecté à la colonne. Le nom du classement peut être un nom de classement Windows ou SQL. Pour en obtenir la liste et des informations supplémentaires, consultez les articles [Nom de classement Windows](../../t-sql/statements/windows-collation-name-transact-sql.md) et [Nom du classement SQL Server](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

La clause COLLATE modifie uniquement les classements des colonnes ayant les types de données **char**, **varchar**, **nchar** et **nvarchar**. Pour modifier le classement d’une colonne de type de données alias définie par l’utilisateur, utilisez des instructions ALTER TABLE distinctes afin de transformer la colonne en type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ensuite, modifiez son classement et transformez à nouveau la colonne en type de données alias.

ALTER COLUMN ne peut pas modifier un classement si une ou plusieurs des conditions suivantes sont remplies :

- Une contrainte CHECK, une contrainte FOREIGN KEY ou des colonnes calculées font référence à la colonne modifiée.
- Un index, des statistiques ou un index de recherche en texte intégral sont créés sur la colonne. Les statistiques créées automatiquement sur la colonne modifiée sont supprimées si le classement de la colonne est modifié.
- Une vue ou une fonction liée à un schéma fait référence à la colonne.

Pour plus d’informations, consultez l’article [COLLATE](~/t-sql/statements/collations.md).

NULL | NOT NULL  
Spécifie si la colonne accepte les valeurs NULL. Les colonnes qui n'acceptent pas les valeurs NULL ne peuvent être ajoutées à l'aide de l'instruction ALTER TABLE que si une valeur par défaut a été définie elles ou si la table est vide. Vous pouvez spécifier NOT NULL pour des colonnes calculées seulement si vous avez également spécifié PERSISTED. Si la nouvelle colonne accepte les valeurs NULL et que vous ne spécifiez aucune valeur par défaut, la nouvelle colonne contient une valeur NULL pour chaque ligne de la table. Si la nouvelle colonne accepte les valeurs NULL et que vous ajoutez une définition de valeur par défaut avec la nouvelle colonne, vous pouvez utiliser WITH VALUES pour stocker la valeur par défaut dans la nouvelle colonne pour chaque ligne existante de la table.

Si la nouvelle colonne n’autorise pas les valeurs NULL et que la table n’est pas vide, vous devez ajouter une définition PAR DÉFAUT avec la nouvelle colonne. La nouvelle colonne est alors automatiquement chargée avec la valeur par défaut dans les nouvelles colonnes de chaque ligne existante.

Vous pouvez spécifier l'option NULL dans ALTER COLUMN pour forcer une colonne NOT NULL à accepter des valeurs NULL, excepté pour les colonnes soumises à des contraintes PRIMARY KEY. Vous ne pouvez spécifier l'option NOT NULL dans ALTER COLUMN que si la colonne ne contient pas de valeurs NULL. Les valeurs NULL doivent être mises à jour avec une valeur quelconque avant que ALTER COLUMN NOT NULL soit autorisé. Par exemple :

```sql
UPDATE MyTable SET NullCol = N'some_value' WHERE NullCol IS NULL;
ALTER TABLE MyTable ALTER COLUMN NullCOl NVARCHAR(20) NOT NULL;
```

Lorsque vous créez ou modifiez une table à l'aide des instructions CREATE TABLE ou ALTER TABLE, les paramètres de la base de données et de la session influencent et, éventuellement. modifient la possibilité de valeurs NULL pour le type de données utilisé dans la définition d'une colonne. Veillez à toujours définir explicitement une colonne comme NULL ou NOT NULL dans le cas de colonnes non calculées.

Si vous ajoutez une colonne avec un type de données défini par l'utilisateur, veillez à définir la colonne avec la même possibilité de valeur Null que le type de données défini par l'utilisateur. Spécifiez également une valeur par défaut pour la colonne. Pour plus d’informations, consultez [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md).

> [!NOTE]
> Si NULL ou NOT NULL est spécifié avec ALTER COLUMN, vous devez également spécifier *new_data_type* [(*precision* [, *scale* ])]. Si vous ne modifiez pas le type de données, la précision ou l'échelle, spécifiez les valeurs actuelles de la colonne.

[ {ADD | DROP} ROWGUIDCOL ]  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie que la propriété ROWGUIDCOL est supprimée de la colonne spécifiée ou y est ajoutée. ROWGUIDCOL indique que la colonne est une colonne d'identificateur global unique (GUID). Vous ne pouvez définir qu’une seule colonne **uniqueidentifier** par table comme colonne ROWGUIDCOL. Et vous ne pouvez affecter la propriété ROWGUIDCOL qu’à une colonne **uniqueidentifier**. Vous ne pouvez pas affecter ROWGUIDCOL à une colonne dont le type de données est défini par l'utilisateur.

ROWGUIDCOL n'impose pas l'unicité des valeurs stockées dans la colonne et ne génère pas automatiquement des valeurs pour les nouvelles lignes insérées dans la table. Pour générer des valeurs uniques pour chaque colonne, utilisez la fonction NEWID ou NEWSEQUENTIALID dans les instructions INSERT. Vous pouvez également spécifier la fonction NEWID ou NEWSEQUENTIALID comme valeur par défaut pour la colonne.

[ {ADD | DROP} PERSISTED ]  
Spécifie que la propriété PERSISTED est ajoutée à ou supprimée de la colonne spécifiée. La colonne doit être une colonne calculée définie avec une expression déterministe. Pour les colonnes spécifiées avec la propriété PERSISTED, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] stocke physiquement les valeurs calculées dans la table et met à jour les valeurs lorsque d'autres colonnes dont dépend la colonne calculée sont mises à jour. Si vous marquez une colonne calculée comme PERSISTED, vous pouvez créer des index sur des colonnes calculées définies sur des expressions qui sont déterministes mais pas précises. Pour plus d'informations, consultez [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).

Toute colonne calculée utilisée comme colonne de partitionnement d'une table partitionnée doit être explicitement marquée comme PERSISTED.

DROP NOT FOR REPLICATION  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie que les valeurs sont incrémentées dans les colonnes d'identité lorsque les agents de réplication effectuent des opérations d'insertion. Vous ne pouvez spécifier cette clause que si *column_name* est une colonne d’identité.

SPARSE  
Indique que la nouvelle colonne est une colonne éparse. Le stockage des colonnes éparses est optimisé pour les valeurs Null. Vous ne pouvez pas définir des colonnes éparses comme NOT NULL. Le fait de convertir une colonne éparse en colonne non éparse ou inversement a pour effet de verrouiller la table pendant la durée de l'exécution de la commande. Vous devrez peut-être utiliser la clause REBUILD pour récupérer de l'espace. Pour connaître les restrictions supplémentaires et obtenir plus d’informations sur les colonnes éparses, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).

ADD MASKED WITH ( FUNCTION = ' *mask_function* ')  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie un masque dynamique des données. *mask_function* est le nom de la fonction de masquage avec les paramètres appropriés. Quatre fonctions sont disponibles :

- default()
- email()
- partial()
- random()

Pour supprimer un masque, utilisez `DROP MASKED`. Pour les paramètres de fonction, consultez [Masquage dynamique des données](../../relational-databases/security/dynamic-data-masking.md).

WITH (ONLINE = ON/OFF) \<as applies to altering a column>  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Permet d'effectuer de nombreuses actions de modification de colonne pendant que la table reste disponible. La valeur par défaut est OFF. Vous pouvez exécuter la modification de colonne en ligne pour les modifications de colonne liées à un type de données, à la longueur de colonne ou à la précision, à la possibilité de valeur null, au caractère épars et au classement.

La modification de colonne en ligne permet aux statistiques créées par l'utilisateur et automatiques de faire référence à la colonne modifiée pendant la durée de l'opération ALTER COLUMN, ce qui permet d’exécuter les requêtes de la manière habituelle. À la fin de l'opération, les statistiques automatiques qui font référence à la colonne sont supprimées et les statistiques créées par l'utilisateur sont invalidées. L'utilisateur doit mettre à jour manuellement les statistiques générées par l'utilisateur une fois l'opération terminée. Si la colonne fait partie d’une expression de filtre pour des index ou les statistiques, vous ne pouvez pas effectuer une opération ALTER COLUMN.

- Pendant l'exécution de l'opération de modification de colonne en ligne, toutes les opérations qui peuvent établir une dépendance sur la colonne (index, vues, etc.) sont bloquées ou échouent avec une erreur appropriée. Ce comportement garantit que l'opération de modification de colonne en ligne n'échouera pas en raison des dépendances introduites pendant son exécution.
- Le remplacement de la valeur NOT NULL par NULL dans une colonne n'est pas pris en charge en tant qu'opération en ligne quand la colonne modifiée est référencée par les index non-cluster.
- La modification en ligne n'est pas prise en charge quand la colonne est référencée par une contrainte de validation et que l'opération de modification limite la précision de la colonne (numérique ou datetime).
- L'option `WAIT_AT_LOW_PRIORITY` ne peut pas être utilisée avec la modification de colonne en ligne.
- `ALTER COLUMN ... ADD/DROP PERSISTED` n’est pas pris en charge pour modifier la colonne en ligne.
- `ALTER COLUMN ... ADD/DROP ROWGUIDCOL/NOT FOR REPLICATION` n’est pas affecté par la modification de colonne en ligne.
- La modification de colonne en ligne ne prend pas en charge la modification d'une table où le suivi des modifications est activé ou qui est un serveur de publication de la réplication de fusion.
- La modification de colonne en ligne ne prend pas en charge la modification depuis ou vers des types de données CLR.
- La modification de colonne en ligne ne prend pas en charge la modification d'un type de données XML qui possède une collection de schémas différente de la collection de schémas active.
- La modification de colonne en ligne ne réduit pas les restrictions relatives aux périodes de modification possibles d'une colonne. Les références par index/statistiques, etc. peuvent entraîner l’échec de la modification.
- La modification de colonne en ligne ne prend pas en charge la modification simultanée de plusieurs colonnes.
- La modification de colonne en ligne n’a aucun effet dans une table temporelle dont la version est contrôlée par le système. L’opération ALTER COLUMN n’est pas effectuée en ligne, quelle que soit la valeur spécifiée pour l’option ONLINE.

La modification de colonne en ligne a des exigences, restrictions et fonctionnalités similaires à la reconstruction d'index en ligne, notamment les suivantes :

- La reconstruction d'index en ligne n'est pas prise en charge quand la table contient des colonnes LOB ou filestream héritées, ou quand la table possède un index columnstore. Les mêmes limitations s'appliquent à la modification de colonne en ligne.
- Une colonne existante modifiée nécessite deux fois plus d'allocation d'espace : pour la colonne d'origine et la colonne masquée nouvellement créée.
- La stratégie de verrouillage lors d'une opération de modification de colonne en ligne suit le même modèle de verrouillage utilisé pour la construction d'index en ligne.

WITH CHECK | WITH NOCHECK  
Spécifie si les données de la table sont ou non validées par sur une contrainte FOREIGN KEY ou CHECK nouvellement ajoutée ou réactivée. Si vous ne spécifiez rien, l'option WITH CHECK est utilisée pour les nouvelles contraintes et l'option WITH NOCHECK pour les contraintes réactivées.

Utilisez WITH NOCHECK si vous ne voulez pas vérifier les nouvelles contraintes CHECK ou FOREIGN KEY sur les données existantes. Ceci n'est pas recommandé, sauf dans quelques cas rares. La nouvelle contrainte est évaluée dans toutes les mises à jour ultérieures. Toute violation de contrainte supprimée par WITH NOCHECK quand la contrainte est ajoutée risque de provoquer l’échec des mises à jour ultérieures si elles mettent à jour des lignes avec des données qui ne respectent pas la contrainte.

> [!NOTE]
> L'optimiseur de requête ne prend pas en compte les contraintes définies avec WITH NOCHECK. De telles contraintes sont ignorées tant qu'elles n'ont pas été réactivées à l'aide d'`ALTER TABLE table WITH CHECK CHECK CONSTRAINT ALL`.

ALTER INDEX *index_name*  
Spécifie que le nombre de compartiments pour *index_name* doit être modifié.

La syntaxe ALTER TABLE… ADD/DROP/ALTER INDEX est uniquement prise en charge pour les tables optimisées en mémoire.

> [!IMPORTANT]
> En l’absence d’instruction ALTER TABLE, les instructions [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) et [PAD_INDEX](alter-table-index-option-transact-sql.md) ne sont pas prises en charge pour les index sur les tables à mémoire optimisée.

ADD  
Spécifie qu'une ou plusieurs définitions de colonnes, définitions de colonnes calculées ou contraintes de tables sont ajoutées. Ou bien les colonnes utilisées par le système pour le contrôle de version sont ajoutées. Pour les tables optimisées en mémoire, vous pouvez ajouter un index.

> [!NOTE]
> Les nouvelles colonnes sont ajoutées après toutes les colonnes existantes de la table à modifier.

> [!IMPORTANT]
> En l’absence d’une instruction ALTER TABLE, les instructions [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) et [PAD_INDEX](alter-table-index-option-transact-sql.md) ne sont pas prises en charge pour les index sur les tables à mémoire optimisée.

PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie les noms des colonnes que le système utilise pour enregistrer la période pendant laquelle un enregistrement est valide. Vous pouvez spécifier des colonnes existantes ou créer des colonnes dans le cadre de l’argument ADD PERIOD FOR SYSTEM_TIME. Configurez les colonnes avec le type de données datetime2 et définissez-les comme NOT NULL. Si vous définissez une colonne de période avec la valeur NULL, une erreur se produit. Vous pouvez définir un élément [column_constraint](../../t-sql/statements/alter-table-column-constraint-transact-sql.md) et/ou [spécifier des valeurs par défaut pour les colonnes](../../relational-databases/tables/specify-default-values-for-columns.md) dans le cas des colonnes system_start_time et system_end_time. Consultez l’Exemple A, dans les exemples suivants de [Gestion système des versions](#system_versioning) ; il illustre l’utilisation d’une valeur par défaut pour la colonne system_end_time.

Utilisez cet argument avec l’argument SYSTEM_VERSIONING pour activer la gestion système des versions sur une table existante. Pour plus d’informations, consultez [Tables temporelles](../../relational-databases/tables/temporal-tables.md) et [Prise en main des tables temporelles dans Azure SQL Database](/azure/azure-sql/temporal-tables).

À compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], les utilisateurs peuvent marquer l’une des colonnes de période, ou les deux, avec l’indicateur **HIDDEN** afin de masquer implicitement ces colonnes pour que **SELECT \* FROM \<table_name>** ne renvoie pas de valeur pour elles. Par défaut, les colonnes de période ne sont pas masquées. Pour pouvoir être utilisées, les colonnes masquées doivent être incluses explicitement dans toutes les requêtes qui référencent directement la table temporelle.

DROP  
Spécifie qu’une ou plusieurs définitions de colonnes, définitions de colonnes calculées ou contraintes de tables sont supprimées, ou qu’il faut supprimer la spécification pour les colonnes que le système utilise pour la gestion système des versions.

CONSTRAINT *constraint_name*  
Spécifie que *constraint_name* est supprimé de la table. Vous pouvez spécifier plusieurs contraintes.

Vous pouvez déterminer le nom de la contrainte défini par l’utilisateur ou fourni par le système en effectuant une requête dans les vues de catalogue **sys.check_constraint**, **sys.default_constraints**, **sys.key_constraints** et **sys.foreign_keys**.

Il n'est pas possible de supprimer une contrainte PRIMARY KEY s'il existe un index XML sur la table.

INDEX *index_name*  
Spécifie que *nom_index* est supprimé de la table.

La syntaxe ALTER TABLE… ADD/DROP/ALTER INDEX est uniquement prise en charge pour les tables optimisées en mémoire.

> [!IMPORTANT]
> En l’absence d’instruction ALTER TABLE, les instructions [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) et [PAD_INDEX](alter-table-index-option-transact-sql.md) ne sont pas prises en charge pour les index sur les tables à mémoire optimisée.

COLUMN *column_name*  
Spécifie que *constraint_name* ou *column_name* est supprimé de la table. Vous pouvez spécifier plusieurs colonnes.

 Une colonne ne peut pas être supprimée lorsqu'elle est :

- utilisée dans un index, sous la forme d’une colonne clé ou un INCLUDE ;
- utilisée dans une contrainte CHECK, FOREIGN KEY, UNIQUE ou PRIMARY KEY ;
- associée à une valeur par défaut définie à l'aide du mot clé DEFAULT ou liée à un objet par défaut ;
- liée à une règle.

> [!NOTE]
> La suppression d'une colonne ne permet pas de récupérer l'espace disque de la colonne. Vous pouvez être amené à récupérer l'espace disque d'une colonne supprimée lorsque la taille des lignes d'une table est proche de sa limite ou l'a dépassée. Récupérez de l’espace en créant un index cluster sur la table ou en reconstruisant un index cluster existant à l’aide de l’instruction [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md). Pour plus d’informations sur l’impact de la suppression de types de données LOB (Large Object), consultez l’[entrée de blog CSS](/archive/blogs/psssql/how-it-works-gotcha-varcharmax-caused-my-queries-to-be-slower).

PERIOD FOR SYSTEM_TIME  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Supprime la spécification pour les colonnes que le système utilisera pour la gestion système des versions.

WITH \<drop_clustered_constraint_option>  
Spécifie qu'une ou plusieurs options de suppression de contrainte cluster sont définies.

MAXDOP = *max_degree_of_parallelism*  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Remplace l’option de configuration **max degree of parallelism** seulement pendant la durée de l’opération. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

Utilisez l'option MAXDOP pour limiter le nombre de processeurs utilisés dans une exécution de plans parallèles. Le nombre maximal de processeurs est égal à 64.

*max_degree_of_parallelism* peut prendre l’une des valeurs suivantes :

1  
Supprime la création de plans parallèles.

\>1  
Limite au nombre spécifié le nombre maximal de processeurs utilisés dans le traitement en parallèle des index.

0 (valeur par défaut)  
Utilise le nombre réel de processeurs ou un nombre de processeurs inférieur en fonction de la charge de travail actuelle du système.

Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).

> [!NOTE]
> Les opérations d’index parallèles ne sont pas disponibles dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Éditions et fonctionnalités prises en charge de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) et [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

ONLINE **=** { ON | **OFF** } \<as applies to drop_clustered_constraint_option>  
Indique si les tables sous-jacentes et les index associés sont disponibles pour les requêtes et la modification de données pendant l'opération d'index. La valeur par défaut est OFF. Vous pouvez exécuter REBUILD en tant qu’opération ONLINE.

ACTIVÉ  
Les verrous de table à long terme ne sont pas maintenus pendant la durée de l'opération d'index. Lors de la principale phase de l'indexation, seul le verrou de partage intentionnel (IS, Intent Share) est maintenu sur la table source. Ceci permet d'effectuer des requêtes ou des mises à jour dans la table sous-jacente et à l'opération sur les index de continuer. Au début de l'opération, un verrou partagé (S) est maintenu sur l'objet source pendant une période de temps courte. À la fin de l’opération, pendant une courte période, un verrou S (partagé) est acquis sur la source si un index non-cluster est créé. Ou bien, un verrou SCH-M (modification du schéma) est acquis lorsqu’un index cluster est créé ou supprimé en ligne et lorsqu’un index cluster ou non-cluster est reconstruit. ONLINE ne peut pas prendre la valeur ON si un index est en cours de création sur une table locale temporaire. Seule l'opération de reconstruction de segment monothread est autorisée.

Pour exécuter l’instruction DDL pour **SWITCH** ou une reconstruction d’index en ligne, toutes les transactions bloquantes actives qui s’exécutent sur une table particulière doivent être terminées. Lors de l’exécution, **SWITCH** ou l’opération de reconstruction empêche de nouvelles transactions de commencer et peut affecter de manière significative le débit de la charge de travail et différer temporairement l’accès à la table sous-jacente.

OFF  
Des verrous de table s’appliquent pendant l'opération d'indexation. Une opération d'indexation hors ligne qui crée, régénère ou supprime un index cluster, ou régénère ou supprime un index non cluster, acquiert un verrou de modification de schéma (Sch-M) sur la table. Ce verrou empêche tous les utilisateurs d'accéder à la table sous-jacente pendant la durée de l'opération. Une opération d'indexation hors ligne qui crée un index non cluster acquiert un verrou partagé (S, Shared) sur la table. Ce verrou empêche la mise à jour de la table sous-jacente, mais autorise les opérations de lecture, telles que des instructions SELECT. Les opérations de reconstruction de segment multithread sont autorisées.

Pour plus d’informations, consultez [Fonctionnement des opérations d’index en ligne](../../relational-databases/indexes/how-online-index-operations-work.md).

> [!NOTE]
> Les opérations d'index en ligne ne sont pas disponibles dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Éditions et fonctionnalités prises en charge de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) et [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

MOVE TO { _partition\_scheme\_name_ **(** _column\_name_ [ 1 **,** ... *n*] **)**  | *filegroup* |  **"** default **"** }  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie un emplacement où déplacer les lignes de données actuellement au niveau feuille de l'index cluster. La table est déplacée au nouvel emplacement. Cette option s'applique uniquement aux contraintes qui créent un index cluster.

> [!NOTE]
> « default » n'est pas un mot clé dans ce contexte. Il s’agit d’un identificateur du groupe de fichiers par défaut qui doit être délimité, comme dans MOVE TO **"** default **"** or MOVE TO **[** default **]** . Si **"** default **"** est spécifié, l’option QUOTED_IDENTIFIER doit être ON pour la session active. Il s'agit du paramètre par défaut. Pour plus d’informations, voir [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

{ CHECK | NOCHECK } CONSTRAINT  
Spécifie que *constraint_name* est activé ou désactivé. Cette option peut être utilisée seulement avec les contraintes FOREIGN KEY et CHECK. Lorsque NOCHECK est spécifié, la contrainte est désactivée ; les insertions et les mises à jour ultérieures de la colonne ne sont pas validées par rapport aux conditions de la contrainte. Il n'est pas possible de désactiver les contraintes DEFAULT, PRIMARY KEY et UNIQUE.

ALL  
Spécifie que toutes les contraintes sont désactivées à l'aide de l'option NOCHECK, ou bien activées à l'aide de l'option CHECK.

{ ENABLE | DISABLE } TRIGGER  
Spécifie que *trigger_name* est activé ou désactivé. Lorsqu’un déclencheur est désactivé, il reste défini pour la table. Toutefois, lorsque les instructions INSERT, UPDATE ou DELETE s’exécutent sur la table, aucune action n’est effectuée dans le déclencheur tant que ce dernier n’est pas réactivé.

ALL  
Spécifie que tous les déclencheurs de la table sont activés ou désactivés.

*trigger_name*  
Spécifie le nom du déclencheur à activer ou à désactiver.

{ ENABLE | DISABLE } CHANGE_TRACKING  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie si le suivi des modifications est activé ou désactivé pour la table. Par défaut, le suivi des modifications est désactivé.

Cette option est disponible uniquement lorsque le suivi des modifications est activé pour la base de données. Pour plus d’informations, consultez l’article [Options SET d’ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md).

Pour activer le suivi des modifications, la table doit avoir une clé primaire.

WITH **(** TRACK_COLUMNS_UPDATED **=** { ON | **OFF** } **)**  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie, si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] effectue un suivi des modifications, quelles colonnes de suivi des modifications ont été mises à jour. La valeur par défaut est OFF.

SWITCH [ PARTITION *source_partition_number_expression* ] TO [ _schema\_name_ **.** ] *target_table* [PARTITION *target_partition_number_expression* ]  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Insère un bloc de données de l'une des manières suivantes :

- Réaffecte toutes les données d'une table en tant que partition d'une table partitionnée déjà existante.
- Bascule une partition d'une table partitionnée vers une autre.
- Réaffecte toutes les données d'une partition d'une table partitionnée à une table non partitionnée existante.

Si *table* est une table partitionnée, vous devez spécifier *source_partition_number_expression*. Si *targe_table* est une table partitionnée, vous devez spécifier *source_partition_number_expression*. Lors de la réaffectation des données d'une table en tant que partition à une table partitionnée déjà existante ou de basculement d'une partition d'une table partitionnée vers une autre, la partition cible doit exister et être vide.

Lors de la réaffectation des données d'une partition pour constituer une seule table, la table cible doit déjà exister et être vide. La table ou la partition source ainsi que la table ou la partition cible doivent se trouver dans le même groupe de fichiers. Les index ou les partitions d'index correspondants doivent également se trouver dans le même groupe de fichiers. De nombreuses autres restrictions s'appliquent au basculement des partitions. *table* et *target_table* ne peuvent pas être identiques. *target_table* peut être un identificateur en plusieurs parties.

*source_partition_number_expression* et *target_partition_number_expression* sont des expressions constantes qui peuvent référencer des variables et des fonctions. y compris les variables et les fonctions définies par l'utilisateur. Ces arguments ne peuvent pas référencer des expressions [!INCLUDE[tsql](../../includes/tsql-md.md)].

Une table partitionnée avec un index columstore cluster se comporte comme un segment partitionné :

- La clé primaire doit inclure la clé de partition.
- Un index unique doit inclure la clé de partition. Le fait d’inclure la clé de partition dans un index unique existant peut cependant changer l’unicité.
- Pour changer de partition, tous les index non-cluster doivent inclure la clé de partition.

Pour plus d’informations sur les restrictions relatives à **SWITCH** lors de l’utilisation de la réplication, consultez [Répliquer des tables et des index partitionnés](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).

Les index columnstore non-clusters générés pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016 CTP1 et pour SQL Database avant la version V12 présentaient un format en lecture seule. Vous devez reconstruire les index columnstore non-cluster au format actuel (qui peut être mis à jour) avant de pouvoir exécuter une opération PARTITION.

SET **(** FILESTREAM_ON = { *nom_schéma_partitions* \| *nom_groupe_de_fichiers_flux_de_fichier* \| **"** default **"** \| **"** NULL **"** } **)**  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ne prend pas en charge `FILESTREAM`.

Spécifie où les données FILESTREAM sont stockées.

ALTER TABLE avec la clause SET FILESTREAM_ON réussit uniquement si la table n'a pas de colonnes FILESTREAM. Vous pouvez ajouter des colonnes FILESTREAM en utilisant une deuxième instruction ALTER TABLE.

Si vous spécifiez *partition_scheme_name*, les règles en vigueur pour [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) s’appliquent. Assurez-vous que la table est déjà partitionnée pour les données de lignes et que son schéma de partition utilise les mêmes fonction et colonnes de partition que le schéma de partition FILESTREAM.

*filestream_filegroup_name* spécifie le nom d’un groupe de fichiers FILESTREAM. Le groupe de fichiers doit avoir un fichier qui est défini pour le groupe de fichiers à l’aide d’une instruction [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) ou [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) ; sinon, une erreur se produit.

**"** default **"** spécifie le groupe de fichiers FILESTREAM avec l’ensemble de propriétés DEFAULT. S'il n'y a aucun groupe de fichiers FILESTREAM, une erreur se produit.

**"** NULL **"** spécifie que toutes les références aux groupes de fichiers FILESTREAM pour la table sont supprimées. Toutes les colonnes FILESTREAM doivent être supprimées en premier. Utilisez SET FILESTREAM_ON = "**NULL**" pour supprimer toutes les données FILESTREAM associées à une table.

SET **(** SYSTEM_VERSIONING **=** { OFF | ON [ ( HISTORY_TABLE = schema_name . history_table_name [ , DATA_CONSISTENCY_CHECK = { **ON** | OFF } ] ) ] } **)**  
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Active ou désactive la gestion système des versions d’une table. Pour activer la gestion système des versions d’une table, le système vérifie que le type de données, la contrainte de possibilité de valeur null et les spécifications de contrainte de clé primaire pour la gestion système des versions sont satisfaits. Si vous n’utilisez pas l’argument HISTORY_TABLE, le système génère une nouvelle table d’historique qui correspond au schéma de la table actuelle, crée un lien entre les deux tables et permet au système d’enregistrer l’historique de chaque enregistrement dans la table actuelle de la table d’historique. Le nom de cette table d’historique sera `MSSQL_TemporalHistoryFor<primary_table_object_id>`. Si vous utilisez l’argument HISTORY_TABLE pour créer un lien vers une table d’historique existante et utiliser cette table, le système crée un lien entre la table actuelle et la table spécifiée. Lorsque vous créez un lien vers une table de l’historique existante, vous pouvez choisir d’effectuer une vérification de cohérence des données. Cette vérification de cohérence des données garantit que les enregistrements existants ne se chevauchent pas. La vérification de cohérence des données est effectuée par défaut. Pour plus d’informations, voir [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

HISTORY_RETENTION_PERIOD = { **INFINITE** \| nombre {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS} }  
**S’applique à**  : [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie la rétention finie ou infinie des données d’historique dans une table temporelle. Si vous l’omettez, la rétention infinie est appliquée.

ENSEMBLE (DATA_DELETION =  
      {OFF | ON  
                [(  [ FILTER_COLUMN = column_name ]   
                    [, RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS | MONTH | MONTHS | YEAR | YEARS }}]   
                )] }   
**S’applique à :** Azure SQL Edge *uniquement*

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

SET **(** LOCK_ESCALATION = { AUTO \| TABLE \| DISABLE } **)**  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie les méthodes autorisées d'escalade de verrous pour une table.

AUTO  
Cette option permet au [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] de sélectionner la granularité d'escalade de verrous appropriée pour le schéma de la table.

- Si la table est partitionnée, l'escalade de verrous sera autorisée jusqu'à la granularité de segment de mémoire ou d'arbre B (B-tree) (HoBT, Heap or B-tree). En d’autres termes, l’escalade est autorisée au niveau de la partition. Une fois que le verrou a atteint le niveau HoBT, il n'est plus escaladé jusqu'à la granularité TABLE.
- Si la table n'est pas partitionnée, l'escalade de verrous continue jusqu'à la granularité TABLE.

TABLE  
L'escalade de verrous continue jusqu'à la granularité TABLE, que la table soit ou non partitionnée. TABLE est la valeur par défaut.

DISABLE  
Empêche l'escalade de verrous dans la plupart des cas. Les verrous de niveau table ne sont pas totalement interdits. Par exemple, lorsque vous analysez une table ne contenant aucun index cluster sous le niveau d'isolation sérialisable, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit prendre un verrou de table pour protéger l'intégrité des données.

REBUILD  
Utilisez la syntaxe REBUILD WITH pour reconstruire une table entière qui inclut toutes les partitions dans une table partitionnée. Si la table a un index cluster, l'option REBUILD reconstruit l'index cluster. REBUILD peut être effectué en tant qu'opération ONLINE.

Utilisez la syntaxe REBUILD PARTITION pour reconstruire une partition unique dans une table partitionnée.

PARTITION = ALL  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Reconstruit toutes les partitions lors de la modification des paramètres de compression de la partition.

REBUILD WITH ( \<rebuild_option> )  
Toutes les options s'appliquent à une table pourvue d'un index cluster Si la table n'a pas d'index cluster, la structure de segment n'est affectée que par certaines options.

Lorsqu'un paramètre de compression spécifique n'est pas spécifié avec l'opération REBUILD, le paramètre de compression actuel est utilisé pour la partition. Pour retourner la valeur actuelle, interrogez la colonne **data_compression** dans la vue de catalogue **sys.partitions**.

Pour une description complète des options de reconstruction, consultez l’article [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md).

DATA_COMPRESSION  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie l'option de compression de données pour la table, le numéro de partition ou la plage de partitions spécifiés. Les options disponibles sont les suivantes :

NONE La table ou les partitions spécifiées ne sont pas compressées. Cette option ne s'applique pas aux tables columnstore.

ROW La table ou les partitions spécifiées sont compressées au moyen de la compression de ligne. Cette option ne s'applique pas aux tables columnstore.

PAGE La table ou les partitions spécifiées sont compressées au moyen de la compression de page. Cette option ne s'applique pas aux tables columnstore.

COLUMNSTORE  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

S'applique uniquement aux tables columnstore. COLUMNSTORE spécifie qu'il faut décompresser une partition compressée à l'aide de l'option COLUMNSTORE_ARCHIVE. Lorsque les données sont restaurées, elles continuent à être compressées à l'aide de la compression columnstore utilisée pour toutes les tables columnstore.

COLUMNSTORE_ARCHIVE  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

S'applique uniquement aux tables columnstore, qui sont des tables stockées avec un index cluster columnstore. COLUMNSTORE_ARCHIVE compressera davantage la partition spécifiée en une plus petite taille. Utilisez cette option pour l'archivage, ou pour d'autres situations qui nécessitent moins de stockage et supportent plus de temps pour le stockage et la récupération.

Pour reconstruire plusieurs partitions en même temps, consultez l’article [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md). Si la table n'a pas d'index cluster, la modification de la compression de données reconstruit le segment de mémoire et les index non-cluster. Pour plus d’informations sur la compression, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).

ONLINE **=** { ON | **OFF** } \<as applies to single_partition_rebuild_option>  
Spécifie si une seule partition des tables sous-jacentes et les index associés sont disponibles pour modifier des requêtes et des données pendant l'opération d'index. La valeur par défaut est OFF. Vous pouvez exécuter REBUILD en tant qu’opération ONLINE.

ACTIVÉ  
Les verrous de table à long terme ne sont pas maintenus pendant la durée de l'opération d'index. Un verrou S sur la table est requis au début de la reconstruction de l'index, et un verrou Sch-M sur la table à la fin de la reconstruction de l'index en ligne. Bien que les deux verrous soient des verrous de métadonnées courtes, le verrou Sch-M doit attendre que toutes les transactions bloquantes soient terminées. Pendant le temps d'attente, le verrou Sch-M bloque toutes les autres transactions qui attendent derrière ce verrou en cas d'accès à la même table.

> [!NOTE]
> La reconstruction d’index en ligne peut définir les options *low_priority_lock_wait* décrites plus loin dans cette section.

OFF  
Des verrous de table sont appliqués pendant l'opération d'indexation. Cela empêche tous les utilisateurs d'accéder à la table sous-jacente pendant la durée de l'opération.

*column_set_name* XML COLUMN_SET FOR ALL_SPARSE_COLUMNS  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Nom du jeu de colonnes. Un jeu de colonnes est une représentation XML non typée qui combine toutes les colonnes éparses d'une table dans une sortie structurée. Un jeu de colonnes ne peut pas être ajouté à une table qui contient des colonnes éparses. Pour plus d’informations sur les jeux de colonnes, consultez [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).

{ ENABLE | DISABLE } FILETABLE_NAMESPACE  
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures).

Active ou désactive les contraintes définies par le système sur un FileTable. Peut être utilisé uniquement avec un FileTable.

SET ( FILETABLE_DIRECTORY = *directory_name* )  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ne prend pas en charge `FILETABLE`.

Spécifie le nom de répertoire FileTable compatible Windows. Ce nom doit être unique parmi tous les noms de répertoire FileTable de la base de données. La comparaison d'unicité n'est pas sensible à la casse, malgré les paramètres de classement SQL. Peut être utilisé uniquement avec un FileTable.

```syntaxsql
 SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options> )
          | = OFF_WITHOUT_DATA_RECOVERY
          ( MIGRATION_STATE = PAUSED ) | ( <table_stretch_options> [, ...n] )
        } )
```

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et versions ultérieures).

Active ou désactive Stretch Database pour une table. Pour plus d'informations, consultez [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

**Activation de Stretch Database pour une table**

Quand vous activez Stretch pour une table en spécifiant `ON`, vous devez aussi spécifier `MIGRATION_STATE = OUTBOUND` pour commencer à migrer les données immédiatement, ou `MIGRATION_STATE = PAUSED` pour reporter la migration des données. La valeur par défaut est `MIGRATION_STATE = OUTBOUND`. Pour plus d’informations sur l’activation de Stretch pour une table, consultez [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

**Conditions préalables**. Avant d’activer Stretch pour une table, vous devez l’activer sur le serveur et sur la base de données. Pour plus d'informations, consultez [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).

**Autorisations**. L’activation de Stretch pour une table ou une base de données nécessite les autorisations db_owner. L’activation de Stretch pour une table nécessite également des autorisations ALTER sur la table.

**Désactivation de Stretch Database pour une table**

Quand vous désactivez Stretch pour une table, vous avez deux options pour les données distantes qui ont déjà été migrées vers Azure. Pour plus d’informations, consultez [Désactiver Stretch Database et récupérer les données distantes](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

- Pour désactiver Stretch Database pour une table et copier les données distantes de la table à partir d’Azure vers SQL Server, exécutez la commande suivante. Cette commande ne peut pas être annulée.

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    ```

Cette opération entraîne des coûts de transfert de données et ne peut pas être annulée. Pour plus d'informations, consultez la rubrique [Détails de la tarification des transferts de données](https://azure.microsoft.com/pricing/details/data-transfers/).

Une fois que toutes les données distantes ont été copiées d'Azure vers SQL Server, Stretch est désactivée pour la table.

- Pour désactiver Stretch pour une table et abandonner les données distantes, exécutez la commande suivante.

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

Une fois que vous avez désactivé Stretch Database pour une table, la migration des données s’arrête et les résultats de requête n’incluent plus les résultats de la table distante.

La désactivation de Stretch ne supprime pas la table distante. Si vous souhaitez supprimer la table distante, vous devez utiliser le Portail Azure.

[ FILTER_PREDICATE = { null | *predicate* } ]  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et versions ultérieures).

Spécifie éventuellement un prédicat de filtre pour sélectionner des lignes à migrer à partir d’une table qui contient des données historiques et actuelles. Le prédicat doit appeler une fonction table inline déterministe. Pour plus d’informations, consultez les articles [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) et [Sélectionner les lignes à migrer à l’aide d’une fonction de filtre(Stretch Database)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).

> [!IMPORTANT]
> Si vous fournissez un prédicat de filtre qui fonctionne mal, la migration des données fonctionne mal également. Stretch Database applique le prédicat de filtre à la table à l'aide de l'opérateur CROSS APPLY.

Si vous ne spécifiez aucun prédicat de filtre, la table entière est migrée.

Quand vous spécifiez un prédicat de filtre, vous devez également spécifier *MIGRATION_STATE*.

MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et versions ultérieures).

- Spécifiez `OUTBOUND` pour migrer des données de SQL Server vers Azure.
- Spécifiez `INBOUND` pour copier les données distantes pour la table d’Azure vers SQL Server, et pour désactiver Stretch pour la table. Pour plus d’informations, consultez [Désactiver Stretch Database et récupérer les données distantes](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

    Cette opération entraîne des coûts de transfert de données et ne peut pas être annulée.

- Spécifiez `PAUSED` pour interrompre ou reporter la migration des données. Pour plus d’informations, consultez l’article [Suspension et reprise de la migration de données (Stretch Database)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).

WAIT_AT_LOW_PRIORITY  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Une reconstruction d'index en ligne doit attendre les opérations de blocage sur cette table. **WAIT_AT_LOW_PRIORITY** indique que l’opération de reconstruction de l’index en ligne attend les verrous de faible priorité, permettant à d’autres opérations de continuer pendant que l’opération de construction de l’index en ligne patiente. L’omission de l’option **WAIT AT LOW PRIORITY** est identique à `WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)`.

MAX_DURATION = *time* [**MINUTES** ]  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Le temps d’attente, qui est une valeur entière spécifiée en minutes, pendant lequel le **SWITCH** ou les verrous de reconstruction d’index en ligne attendent avec une basse priorité lors de l’exécution de la commande DDL. Si l’opération est bloquée pendant le temps **MAX_DURATION**, l’une des actions **ABORT_AFTER_WAIT** est exécutée. La durée **MAX_DURATION** est toujours indiquée en minutes et vous pouvez omettre le mot **MINUTES**.

ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Aucune  
Continuez à attendre le verrou avec la priorité normale.

SELF  
Quitter le **SWITCH** ou l’opération DDL de reconstruction de l’index en ligne actuellement exécutée sans effectuer aucune action.

BLOCKERS  
Annuler toutes les transactions utilisateur qui bloquent actuellement le **SWITCH** ou l’opération DDL de reconstruction de l’index en ligne afin que l’opération puisse continuer.

Nécessite l’autorisation **ALTER ANY CONNECTION**.

IF EXISTS  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Supprime de manière conditionnelle la colonne ou contrainte uniquement si elle existe déjà.

## <a name="remarks"></a>Notes

Pour ajouter de nouvelles lignes de données, utilisez l’instruction [INSERT](../../t-sql/statements/insert-transact-sql.md). Pour supprimer des lignes de données, utilisez les instructions [DELETE](../../t-sql/statements/delete-transact-sql.md) ou [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md). Pour modifier des valeurs sur les lignes existantes, utilisez l’instruction [UPDATE](../../t-sql/queries/update-transact-sql.md).

Si le cache de procédures contient des plans d'exécution qui référencent la table, ALTER TABLE les marque de façon à les recompiler lors de leur prochaine exécution.

## <a name="changing-the-size-of-a-column"></a>Modification de la taille d'une colonne

Vous pouvez modifier la longueur, l'échelle ou la précision d'une colonne en spécifiant une nouvelle taille pour le type de données de la colonne. Utilisez la clause ALTER COLUMN. Si des données existent dans la colonne, la nouvelle taille ne peut pas être inférieure à la taille maximale des données. De même, vous ne pouvez pas définir la colonne dans un index, sauf si la colonne est un type de données **varchar**, **nvarchar** ou **varbinary** et que l’index n’est pas le résultat d’une contrainte PRIMARY KEY. Consultez l’exemple dans la courte section intitulée [Modification d’une définition de colonne](#alter_column).

## <a name="locks-and-alter-table"></a>Verrous et ALTER TABLE

Les modifications que vous spécifiez dans l’instruction ALTER TABLE sont implémentées immédiatement. Si elles nécessitent une modification des lignes de la table, ALTER TABLE met les lignes à jour. ALTER TABLE acquiert un verrou (SCH-M) de modification du schéma sur la table pour garantir qu'aucune autre connexion ne référence même les métadonnées de la table pendant la modification, à l'exception des opérations d'index en ligne qui nécessitent un verrouillage court de type SCH-M à la fin. Dans une opération `ALTER TABLE...SWITCH`, le verrou est acquis à la fois sur la table source et sur la table cible. Les modifications effectuées sur la table sont consignées dans un journal et peuvent être récupérées entièrement. Les modifications qui affectent toutes les lignes d’une grande table de dimension, telles que la suppression d’une colonne ou, dans certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l’ajout d’une colonne NOT NULL avec une valeur par défaut, peuvent demander beaucoup de temps, tant pour s’exécuter que pour générer un grand nombre d’enregistrements dans le journal des transactions. Exécutez ces instructions ALTER TABLE avec le même soin que toute instruction INSERT, UPDATE ou DELETE qui affecte un grand nombre de lignes.

### <a name="adding-not-null-columns-as-an-online-operation"></a>Ajout de colonnes NOT NULL en tant qu'opération en ligne

À partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Enterprise Edition, l’ajout d’une colonne NOT NULL avec une valeur par défaut est une opération en ligne quand la valeur par défaut est une *constante d’exécution*. Cela signifie que l'opération est terminée presque instantanément malgré le nombre de lignes dans la table. Parce que les lignes existantes dans la table ne sont pas mises à jour pendant l’opération. À la place, la valeur par défaut est stockée uniquement dans les métadonnées de la table et la valeur se trouve autant que nécessaire dans les requêtes qui accèdent à ces lignes. Ce comportement est automatique. Aucune syntaxe supplémentaire n'est nécessaire pour implémenter l'opération en ligne au-delà de la syntaxe COLUMN ADD. Une constante d'exécution est une expression qui produit la même valeur au moment de l'exécution pour chaque ligne dans la table malgré son déterminisme. Par exemple, l'expression constante « mes données temporaires », ou la fonction système GETUTCDATETIME () sont des constantes d'exécution. Par opposition, les fonctions `NEWID()` ou `NEWSEQUENTIALID()` ne sont pas des constantes d’exécution car une valeur unique est produite pour chaque ligne de la table. L'ajout d'une colonne NOT NULL avec une valeur par défaut qui n'est pas une constante de runtime est toujours exécuté hors connexion et un verrou (SCH-M) exclusif est acquis pour la durée de l'opération.

Alors que les lignes existantes référencent la valeur stockée dans les métadonnées, la valeur par défaut est stockée dans la ligne pour toutes les nouvelles lignes qui sont insérées et ne spécifient pas d’autre valeur pour la colonne. La valeur par défaut stockée dans les métadonnées se déplace vers une ligne existante lorsque la ligne est mise à jour (même si la colonne réelle n'est pas spécifiée dans l'instruction UPDATE), ou si la table ou l'index cluster est régénéré.

Les colonnes de type **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml**, **text**, **ntext**, **image**, **hierarchyid**, **geometry**, **geography** ou de types CLR définis par l’utilisateur ne peuvent pas être ajoutées dans une opération en ligne. Une colonne ne peut pas être ajoutée en ligne si cela entraîne le dépassement de la limite de 8 060 octets pour la taille de la ligne. Dans ce cas, la colonne est ajoutée en tant que traitement en différé.

## <a name="parallel-plan-execution"></a>Exécution d'un plan en parallèle

Dans [!INCLUDE[ssEnterpriseEd11](../../includes/ssenterpriseed11-md.md)] et versions ultérieures, le nombre de processeurs utilisés pour exécuter une instruction ALTER TABLE ADD (basée sur un index) CONSTRAINT ou DROP (index cluster) CONSTRAINT est déterminé par l’option de configuration **Degré maximal de parallélisme** et par la charge de travail en cours. Si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détecte que le système est occupé, le degré de parallélisme de l'opération est automatiquement diminué avant le démarrage de l'exécution de l'instruction. Vous pouvez configurer manuellement le nombre de processeurs utilisés pour exécuter l'instruction en spécifiant l'option MAXDOP. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

## <a name="partitioned-tables"></a>Tables partitionnées

Outre les opérations SWITCH qui impliquent des tables partitionnées, utilisez ALTER TABLE pour modifier l'état des colonnes, des contraintes et des déclencheurs d'une table partitionnée, de la même manière que pour les tables non partitionnées. Cependant, cette instruction ne peut pas être utilisée pour modifier la façon dont la table elle-même est partitionnée. Pour repartitionner une table partitionnée, utilisez les instructions [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md) et [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md). De plus, vous ne pouvez pas modifier le type de données d'une colonne d'une table partitionnée.

## <a name="restrictions-on-tables-with-schema-bound-views"></a>Restrictions sur les tables comportant des vues liées au schéma

Les restrictions applicables aux instructions ALTER TABLE dans les tables comportant des vues liées au schéma sont identiques à celles qui s'appliquent à la modification de tables comportant un index simple. L'ajout d'une colonne est autorisé. Cependant, la suppression ou la modification d'une colonne intervenant dans une vue associée à un schéma n'est pas autorisée. Si l'instruction ALTER TABLE requiert la modification d'une colonne utilisée dans une vue liée au schéma, ALTER TABLE échoue et le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère un message d'erreur. Pour plus d’informations sur la liaison aux schémas et sur les vues indexées, consultez l’article [CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md).

L'ajout ou la suppression de déclencheurs sur les tables de base n'est pas affectée par la création d'une vue liée au schéma comportant des références aux tables.

## <a name="indexes-and-alter-table"></a>Index et ALTER TABLE

Tout index créé dans le cadre d'une contrainte est supprimé lorsque cette dernière est supprimée. Un index créé au moyen de l'instruction CREATE INDEX doit être supprimé à l'aide de l'instruction DROP INDEX. Utilisez l'instruction ALTER INDEX pour reconstruire un index faisant partie de la définition d'une contrainte ; il n'est pas nécessaire de supprimer la contrainte et de l'ajouter à nouveau à l'aide de l'instruction ALTER TABLE.

Tous les index et contraintes basés sur une colonne doivent être supprimés avant que la colonne puisse être supprimée.

Lorsque vous supprimez une contrainte qui a créé un index cluster, les lignes de données stockées au niveau feuille de l'index cluster sont stockées dans une table non-cluster. Vous pouvez supprimer l'index cluster et déplacer la table résultante vers un autre groupe de fichiers ou schéma de partition dans une transaction unique en spécifiant l'option MOVE TO. Cette option est soumise aux restrictions suivantes :

- MOVE TO n'est pas valide pour les vues non indexées ou les index non-cluster.
- Le schéma de partition ou le groupe de fichiers doit déjà exister.
- Si MOVE TO n'est pas spécifié, la table est placée dans le même schéma de partition ou groupe de fichiers qui a été défini pour l'index cluster.

Quand vous supprimez un index cluster, spécifiez l’option ONLINE **=** ON afin que la transaction DROP INDEX ne bloque pas les requêtes et les modifications des données sous-jacentes et des index non-cluster associés.

L’option ONLINE **=** ON est soumise aux restrictions suivantes :

- ONLINE **=** ON n’est pas valide pour les index cluster qui sont également désactivés. Les index désactivés doivent être supprimés au moyen de ONLINE **=** OFF.
- Un seul index peut être supprimé à la fois.
- ONLINE **=** ON n’est pas valide pour les vues indexées, les index non-cluster ou les index sur des tables temporaires locales.
- ONLINE **=** ON n’est pas valide pour les index columnstore.

Pour supprimer un index cluster, l'espace disque temporaire doit être égal à la taille de l'index cluster existant. Cet espace supplémentaire est libéré dès que l'opération est terminée.

> [!NOTE]
> Les options listés sous *\<drop_clustered_constraint_option>* s'appliquent aux index cluster des tables et elles ne s'appliquent pas aux index cluster des vues ou aux index non cluster.

## <a name="replicating-schema-changes"></a>Réplication des modifications de schéma

Lorsque vous exécutez l'instruction ALTER TABLE sur une table publiée d'un serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par défaut, cette modification est propagée à tous les Abonnés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette fonctionnalité comporte des restrictions. Vous pouvez la désactiver. Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).

## <a name="data-compression"></a>Data Compression

Les tables système ne peuvent pas être activées pour la compression. Si la table est un segment de mémoire, l'opération de reconstruction pour le mode ONLINE sera monothread. Utilisez le mode OFFLINE pour une opération de reconstruction de segment de mémoire multithread. Pour plus d’informations sur la compression de données, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).

Pour évaluer la façon dont la modification de l’état de compression affecte une table, un index ou une partition, utilisez la procédure stockée [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) .

Les restrictions suivantes s'appliquent aux tables partitionnées :

- Vous ne pouvez pas modifier le paramètre de compression d'une partition unique si la table possède des index non alignés.
- La syntaxe ALTER TABLE \<table> REBUILD PARTITION ... reconstruit la partition spécifiée.
- La syntaxe ALTER TABLE \<table> REBUILD WITH ... reconstruit toutes les partitions.

## <a name="dropping-ntext-columns"></a>Suppression de colonnes NTEXT

Lors de la suppression de colonnes NTEXT, le nettoyage des données supprimées se produit en tant qu'opération sérialisée sur toutes les lignes. Le nettoyage peut prendre beaucoup de temps. Lorsque vous supprimez une colonne NTEXT dans une table contenant beaucoup de lignes, mettez à jour la colonne NTEXT avec la valeur NULL au préalable, puis supprimez la colonne. Vous pouvez exécuter cette option avec des opérations parallèles et la rendre beaucoup plus rapide.

## <a name="online-index-rebuild"></a>Reconstruction d'index en ligne

Pour exécuter l'instruction DDL pour une reconstruction d'index en ligne, toutes les transactions bloquantes actives qui s'exécutent sur une table particulière doivent être terminées. Lorsque la reconstruction d'index en ligne est lancée, elle bloque toutes les nouvelles transactions qui sont prêtes à s'exécuter sur cette table. Bien que la durée du verrou pour la reconstruction de l'index en ligne soit courte, le fait d'attendre que toutes les transactions ouvertes sur une table spécifique soient exécutées, et le fait de bloquer les nouvelles transactions qui doivent démarrer, peuvent avoir un impact important sur le débit. Ceci peut entraîner un ralentissement ou un délai d'expiration de la charge de travail et limiter significativement l’accès à la table sous-jacente. L’option **WAIT_AT_LOW_PRIORITY** permet aux administrateurs de base de données de gérer les verrous S et Sch-M nécessaires pour les reconstructions d’index en ligne, et de sélectionner l’une des trois options. Dans les trois cas, si aucune activité n’est bloquante pendant le temps d’attente (`(MAX_DURATION =n [minutes])`), la reconstruction d’index en ligne est exécutée immédiatement, sans attendre, et l’instruction DDL est effectuée.

## <a name="compatibility-support"></a>Prise en charge de la compatibilité

L'instruction ALTER TABLE prend en charge uniquement les noms de tables en deux parties (schema.object). Dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], la spécification d'un nom de table à l'aide des formats suivants échoue au moment de la compilation, avec l'erreur 117.

- server.database.schema.table
- .database.schema.table
- ..schema.table

Dans les versions antérieures, la spécification du format server.database.schema.table retournait l'erreur 4902. La spécification du format .database.schema.table ou .schema.table aboutissait.

Pour résoudre le problème, supprimez l'utilisation d'un préfixe en quatre parties.

## <a name="permissions"></a>Autorisations

Requiert une autorisation ALTER sur la table.

Les autorisations ALTER TABLE s'appliquent aux tables mises en œuvre dans une instruction ALTER TABLE SWITCH. Toute donnée basculée hérite de la sécurité de la table cible.

Si vous avez défini des colonnes dans l'instruction ALTER TABLE avec un type CLR défini par l'utilisateur ou un type de données alias, l'autorisation REFERENCES sur le type est requise.

L’ajout ou la modification d’une colonne qui met à jour les lignes de la table requiert l’autorisation **METTRE À JOUR** sur la table. Par exemple, l’ajout d’une colonne **NOT NULL** avec une valeur par défaut ou l’ajout d’une colonne d’identité quand la table n’est pas vide.

## <a name="examples"></a><a name="Example_Top"></a> Exemples

|Category|Éléments syntaxiques proposés|
|--------------|------------------------------|
|[Ajout de colonnes et de contraintes](#add)|ADD * PRIMARY KEY avec des options d’index * colonnes éparses et jeux de colonnes *|
|[Suppression de colonnes et de contraintes](#Drop)|DROP|
|[Modification d’une définition de colonne](#alter_column)|changement de type de données * changement de taille de colonne * classement|
|[Modification d’une définition de table](#alter_table)|DATA_COMPRESSION * SWITCH PARTITION * LOCK ESCALATION * suivi des modifications|
|[Désactivation et activation des contraintes et des déclencheurs](#disable_enable)|CHECK * NO CHECK * ENABLE TRIGGER * DISABLE TRIGGER|
| &nbsp; | &nbsp; |

### <a name="adding-columns-and-constraints"></a><a name="add"></a>Ajout de colonnes et de contraintes

Les exemples fournis dans cette section expliquent comment ajouter des colonnes et des contraintes à une table.

#### <a name="a-adding-a-new-column"></a>R. Ajout d'une nouvelle colonne

L'exemple suivant ajoute une colonne qui accepte les valeurs NULL et pour laquelle aucune valeur n'est spécifiée via une définition DEFAULT. Dans la nouvelle colonne, chaque ligne aura la valeur `NULL`.

```sql
CREATE TABLE dbo.doc_exa (column_a INT) ;
GO
ALTER TABLE dbo.doc_exa ADD column_b VARCHAR(20) NULL ;
GO
```

#### <a name="b-adding-a-column-with-a-constraint"></a>B. Ajout d'une colonne avec une contrainte

L'exemple suivant ajoute une nouvelle colonne avec une contrainte `UNIQUE`.

```sql
CREATE TABLE dbo.doc_exc (column_a INT) ;
GO
ALTER TABLE dbo.doc_exc ADD column_b VARCHAR(20) NULL
    CONSTRAINT exb_unique UNIQUE ;
GO
EXEC sp_help doc_exc ;
GO
DROP TABLE dbo.doc_exc ;
GO
```

#### <a name="c-adding-an-unverified-check-constraint-to-an-existing-column"></a>C. Ajout d'une contrainte CHECK non vérifiée à une colonne existante

L'exemple suivant ajoute une contrainte à une colonne existante de la table. La colonne comporte une valeur qui ne respecte pas la contrainte. Par conséquent, `WITH NOCHECK` empêche la validation de la contrainte sur les lignes existantes, et permet l'ajout de la contrainte.

```sql
CREATE TABLE dbo.doc_exd (column_a INT) ;
GO
INSERT INTO dbo.doc_exd VALUES (-1) ;
GO
ALTER TABLE dbo.doc_exd WITH NOCHECK
ADD CONSTRAINT exd_check CHECK (column_a > 1) ;
GO
EXEC sp_help doc_exd ;
GO
DROP TABLE dbo.doc_exd ;
GO
```

#### <a name="d-adding-a-default-constraint-to-an-existing-column"></a>D. Ajout d'une contrainte DEFAULT à une colonne existante

L'exemple suivant crée une table de deux colonnes et insère une valeur dans la première ; l'autre colonne conserve la valeur NULL. Une contrainte `DEFAULT` est alors ajoutée à la deuxième colonne. Pour vérifier que la valeur par défaut est appliquée, une autre valeur est insérée dans la première colonne et la table fait l'objet d'une requête.

```sql
CREATE TABLE dbo.doc_exz (column_a INT, column_b INT) ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (7) ;
GO
ALTER TABLE dbo.doc_exz
  ADD CONSTRAINT col_b_def
  DEFAULT 50 FOR column_b ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (10) ;
GO
SELECT * FROM dbo.doc_exz ;
GO
DROP TABLE dbo.doc_exz ;
GO
```

#### <a name="e-adding-several-columns-with-constraints"></a>E. Ajout de plusieurs colonnes avec des contraintes

L'exemple suivant ajoute plusieurs colonnes avec des contraintes définies. La première colonne a la propriété `IDENTITY`. Chaque ligne de la table a de nouvelles valeurs incrémentielles dans la colonne d'identité.

```sql
CREATE TABLE dbo.doc_exe (column_a INT CONSTRAINT column_a_un UNIQUE) ;
GO
ALTER TABLE dbo.doc_exe ADD

-- Add a PRIMARY KEY identity column.
column_b INT IDENTITY
CONSTRAINT column_b_pk PRIMARY KEY,

-- Add a column that references another column in the same table.
column_c INT NULL
CONSTRAINT column_c_fk
REFERENCES doc_exe(column_a),

-- Add a column with a constraint to enforce that
-- nonnull data is in a valid telephone number format.
column_d VARCHAR(16) NULL
CONSTRAINT column_d_chk
CHECK
(column_d LIKE '[0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]' OR
column_d LIKE
'([0-9][0-9][0-9]) [0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]'),

-- Add a nonnull column with a default.
column_e DECIMAL(3,3)
CONSTRAINT column_e_default
DEFAULT .081 ;
GO
EXEC sp_help doc_exe ;
GO
DROP TABLE dbo.doc_exe ;
GO
```

#### <a name="f-adding-a-nullable-column-with-default-values"></a>F. Ajout d'une colonne acceptant les valeurs NULL, avec des valeurs par défaut

L'exemple suivant ajoute une colonne qui accepte les valeurs NULL, avec une définition `DEFAULT`. Il utilise l'option `WITH VALUES` pour spécifier des valeurs pour chaque ligne existante de la table. Si l'option WITH VALUES n'est pas utilisée, chaque ligne a la valeur NULL dans la nouvelle colonne.

```sql
CREATE TABLE dbo.doc_exf (column_a INT) ;
GO
INSERT INTO dbo.doc_exf VALUES (1) ;
GO
ALTER TABLE dbo.doc_exf
ADD AddDate smalldatetime NULL
CONSTRAINT AddDateDflt
DEFAULT GETDATE() WITH VALUES ;
GO
DROP TABLE dbo.doc_exf ;
GO
```

#### <a name="g-creating-a-primary-key-constraint-with-index-or-data-compression-options"></a>G. Création d’une contrainte PRIMARY KEY avec des options d’index ou de compression des données

L'exemple suivant crée la contrainte PRIMARY KEY `PK_TransactionHistoryArchive_TransactionID` et définit les options `FILLFACTOR`, `ONLINE` et `PAD_INDEX`. L'index cluster généré portera le même nom que la contrainte.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (FILLFACTOR = 75, ONLINE = ON, PAD_INDEX = ON);
GO
```

Cet exemple similaire applique la compression de page lors de l’application de la clé primaire en cluster.

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (DATA_COMPRESSION = PAGE);
GO
```

#### <a name="h-adding-a-sparse-column"></a>H. Ajout d'une colonne éparse

Les exemples suivants illustrent l'ajout et la modification des colonnes éparses dans la table T1. Le code pour créer la table `T1` est comme suit.

```sql
CREATE TABLE T1 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) SPARSE NULL,
  C3 INT SPARSE NULL,
  C4 INT) ;
GO
```

Pour ajouter une colonne éparse supplémentaire `C5`, exécutez l'instruction suivante.

```sql
ALTER TABLE T1
ADD C5 CHAR(100) SPARSE NULL ;
GO
```

Pour convertir la colonne non éparse `C4` en colonne éparse, exécutez l'instruction suivante.

```sql
ALTER TABLE T1
ALTER COLUMN C4 ADD SPARSE ;
GO
```

Pour convertir la colonne éparse `C4` en colonne non éparse, exécutez l’instruction suivante.

```sql
ALTER TABLE T1
ALTER COLUMN C4 DROP SPARSE ;
GO
```

#### <a name="i-adding-a-column-set"></a>I. Ajout d'un jeu de colonnes

Les exemples suivants montrent comment ajouter une colonne à la table `T2`. Un jeu de colonnes ne peut pas être ajouté à une table qui contient déjà des colonnes éparses. Le code pour créer la table `T2` est comme suit.

```sql
CREATE TABLE T2 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Les trois instructions suivantes ajoutent un jeu de colonnes nommé `CS`, puis changent les colonnes `C2` et `C3` en `SPARSE`.

```sql
ALTER TABLE T2
ADD CS XML COLUMN_SET FOR ALL_SPARSE_COLUMNS ;
GO

ALTER TABLE T2
ALTER COLUMN C2 ADD SPARSE ;
GO

ALTER TABLE T2
ALTER COLUMN C3 ADD SPARSE ;
GO
```

#### <a name="j-adding-an-encrypted-column"></a>J. Ajout d’une colonne chiffrée

L’instruction suivante ajoute une colonne chiffrée nommée `PromotionCode`.

```sql
ALTER TABLE Customers ADD
    PromotionCode nvarchar(100)
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,
    ENCRYPTION_TYPE = RANDOMIZED,
    ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') ;
```

### <a name="dropping-columns-and-constraints"></a><a name="Drop"></a>Suppression de colonnes et de contraintes

Les exemples fournis dans cette section expliquent comme supprimer des colonnes et des contraintes.

#### <a name="a-dropping-a-column-or-columns"></a>R. Suppression d'une ou plusieurs colonnes

Le premier exemple supprime une colonne dans une table. Le second exemple supprime plusieurs colonnes.

```sql
CREATE TABLE dbo.doc_exb (
     column_a INT,
     column_b VARCHAR(20) NULL,
     column_c DATETIME,
     column_d INT) ;
GO  
-- Remove a single column.
ALTER TABLE dbo.doc_exb DROP COLUMN column_b ;
GO
-- Remove multiple columns.
ALTER TABLE dbo.doc_exb DROP COLUMN column_c, column_d;
```

#### <a name="b-dropping-constraints-and-columns"></a>B. Suppression de contraintes et de colonnes

Le premier exemple supprime une contrainte `UNIQUE` d'une table. Le second exemple supprime deux contraintes et une seule colonne.

```sql
CREATE TABLE dbo.doc_exc (column_a INT NOT NULL CONSTRAINT my_constraint UNIQUE) ;
GO

-- Example 1. Remove a single constraint.
ALTER TABLE dbo.doc_exc DROP my_constraint ;
GO

DROP TABLE dbo.doc_exc;
GO

CREATE TABLE dbo.doc_exc ( column_a INT
                          NOT NULL CONSTRAINT my_constraint UNIQUE
                          ,column_b INT
                          NOT NULL CONSTRAINT my_pk_constraint PRIMARY KEY) ;
GO

-- Example 2. Remove two constraints and one column
-- The keyword CONSTRAINT is optional. The keyword COLUMN is required.
ALTER TABLE dbo.doc_exc
DROP CONSTRAINT my_constraint, my_pk_constraint, COLUMN column_b ;
GO
```

#### <a name="c-dropping-a-primary-key-constraint-in-the-online-mode"></a>C. Suppression d'une contrainte PRIMARY KEY en mode ONLINE

L'exemple suivant supprime une contrainte PRIMARY KEY avec l'option `ONLINE` ayant pour valeur `ON`.

```sql
ALTER TABLE Production.TransactionHistoryArchive
DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID
WITH (ONLINE = ON) ;
GO
```

#### <a name="d-adding-and-dropping-a-foreign-key-constraint"></a>D. Ajout et suppression d'une contrainte FOREIGN KEY

L'exemple suivant crée la table `ContactBackup`, puis la modifie en ajoutant d'abord une contrainte `FOREIGN KEY` qui référence la table `Person.Person`, puis en supprimant la contrainte `FOREIGN KEY`.

```sql
CREATE TABLE Person.ContactBackup
    (ContactID INT) ;
GO

ALTER TABLE Person.ContactBackup
ADD CONSTRAINT FK_ContactBackup_Contact FOREIGN KEY (ContactID)
    REFERENCES Person.Person (BusinessEntityID) ;
GO

ALTER TABLE Person.ContactBackup
DROP CONSTRAINT FK_ContactBackup_Contact ;
GO

DROP TABLE Person.ContactBackup ;
```

![Icône de flèche utilisée avec le lien Retour en haut](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour en haut") [Exemples](#Example_Top)

### <a name="altering-a-column-definition"></a><a name="alter_column"></a> Modification d’une définition de colonne

#### <a name="a-changing-the-data-type-of-a-column"></a>R. Modification du type de données d'une colonne

L'exemple suivant modifie le type d'une colonne d'une table de `INT` en `DECIMAL`.

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy ALTER COLUMN column_a DECIMAL (5, 2) ;
GO
DROP TABLE dbo.doc_exy ;
GO
```

#### <a name="b-changing-the-size-of-a-column"></a>B. Modification de la taille d’une colonne

L’exemple suivant augmente la taille d’une colonne **varchar** ainsi que la précision et l’échelle d’une colonne **decimal**. Dans la mesure où les colonnes contiennent des données, la taille de colonne peut uniquement être augmentée. Remarquez aussi que `col_a` est défini dans un index unique. La taille de `col_a` peut encore être augmentée car le type de données est un **varchar** et l’index n’est pas le résultat d’une contrainte PRIMARY KEY.

```sql
-- Create a two-column table with a unique index on the varchar column.
CREATE TABLE dbo.doc_exy (col_a varchar(5) UNIQUE NOT NULL, col_b decimal (4,2)) ;
GO
INSERT INTO dbo.doc_exy VALUES ('Test', 99.99) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
GO
-- Increase the size of the varchar column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_a varchar(25) ;
GO
-- Increase the scale and precision of the decimal column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_b decimal (10,4) ;
GO
-- Insert a new row.
INSERT INTO dbo.doc_exy VALUES ('MyNewColumnSize', 99999.9999) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
```

#### <a name="c-changing-column-collation"></a>C. Modification du classement des colonnes

L'exemple suivant indique comment modifier le classement d'une colonne. En premier lieu, une table est créée avec le classement de l’utilisateur par défaut.

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Ensuite, le classement `C2` de la colonne est modifié en Latin1_General_BIN. Le type de données est obligatoire, bien qu'il ne soit pas modifié.

```sql
ALTER TABLE T3
ALTER COLUMN C2 varchar(50) COLLATE Latin1_General_BIN ;
GO
```

#### <a name="d-encrypting-a-column"></a>D. Chiffrement d’une colonne

L’exemple suivant montre comment chiffrer une colonne à l’aide d’[Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

Tout d’abord, une table est créée sans aucune colonne chiffrée.

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Ensuite, la colonne « C2 » est chiffrée avec une clé de chiffrement de colonne, nommée CEK1, et un chiffrement aléatoire. Pour que l’instruction suivante réussisse :

- la clé de chiffrement de colonne doit prendre en charge les enclaves. Ceci signifie qu’elle doit être chiffrée avec une clé principale de colonne qui permet des calculs d’enclave.
- L’instance SQL Server cible doit prendre en charge Always Encrypted avec des enclaves sécurisées.
- L’instruction doit être émise via une connexion définie pour Always Encrypted avec des enclaves sécurisées et à l’aide d’un pilote de client pris en charge.
- L’application appelante doit avoir accès à la clé principale de colonne protégeant CEK1.

```sql
ALTER TABLE T3
ALTER COLUMN C2 VARCHAR(50) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NULL;
GO
```

### <a name="altering-a-table-definition"></a><a name="alter_table"></a> Modification d’une définition de table

Les exemples présentés dans cette section montrent comment modifier la définition d'une table.

#### <a name="a-modifying-a-table-to-change-the-compression"></a>R. Modification d'une table pour modifier la compression

L'exemple suivant modifie la compression d'une table non partitionnée. Le segment de mémoire ou l'index cluster sera reconstruit. Si la table est un segment, tous les index non cluster associés à la table sont reconstruits.

```sql
ALTER TABLE T1
REBUILD WITH (DATA_COMPRESSION = PAGE) ;
```

L'exemple suivant modifie la compression d'une table partitionnée. La syntaxe `REBUILD PARTITION = 1` provoque uniquement la reconstruction de la partition numéro `1`.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =NONE) ;
GO
```

La même opération utilisant la syntaxe suivante provoque la reconstruction de toutes les partitions dans la table.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = ALL
WITH (DATA_COMPRESSION = PAGE ON PARTITIONS(1)) ;
```

Pour obtenir d’autres exemples de compression de données, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md).

#### <a name="b-modifying-a-columnstore-table-to-change-archival-compression"></a>B. Modification d'une table columnstore pour modifier la compression d'archivage

L'exemple suivant compresse davantage une partition de table columnstore en appliquant un algorithme de compression supplémentaire. Cette compression réduit la taille de la table, mais augmente également le temps nécessaire pour le stockage et la récupération. Cela est utile pour l'archivage, ou d'autres situations qui nécessitent moins d'espace de stockage et supportent plus de temps pour le stockage et la récupération.

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE) ;
GO
```

L’exemple ci-après décompresse une partition de table columnstore compressée à l’aide de l’option COLUMNSTORE_ARCHIVE. Lorsque les données sont restaurées, elles continuent à être compressées à l'aide de la compression columnstore utilisée pour toutes les tables columnstore.

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE) ;
GO
```

#### <a name="c-switching-partitions-between-tables"></a>C. Basculement de partitions entre des tables

L'exemple suivant crée une table partitionnée, en partant du principe que le schéma de partition `myRangePS1` est déjà créé dans la base de données. Ensuite, une table non partitionnée est créée avec la même structure que la table partitionnée et sur le même groupe de fichiers que `PARTITION 2` de la table `PartitionTable`. Les données de `PARTITION 2` de la table `PartitionTable` sont ensuite basculées dans la table `NonPartitionTable`.

```sql
CREATE TABLE PartitionTable (col1 INT, col2 CHAR(10))
ON myRangePS1 (col1) ;
GO
CREATE TABLE NonPartitionTable (col1 INT, col2 CHAR(10))
ON test2fg ;
GO
ALTER TABLE PartitionTable SWITCH PARTITION 2 TO NonPartitionTable ;
GO
```

#### <a name="d-allowing-lock-escalation-on-partitioned-tables"></a>D. Autorisation de l'escalade de verrous sur les tables partitionnées

L'exemple suivant autorise l'escalade de verrous au niveau de la partition sur une table partitionnée. Si la table n'est pas partitionnée, l'escalade de verrous est définie au niveau TABLE.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE dbo.T1 SET (LOCK_ESCALATION = AUTO) ;
GO
```

#### <a name="e-configuring-change-tracking-on-a-table"></a>E. Configuration du suivi des modifications sur une table

L'exemple suivant active le suivi des modifications sur la table `Person.Person`.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING ;
```

L'exemple ci-dessous active le suivi des modifications ainsi que le suivi des colonnes qui sont mises à jour lors d'une modification.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING
WITH (TRACK_COLUMNS_UPDATED = ON)
```

L'exemple suivant désactive le suivi des modifications sur la table `Person.Person`.

**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
DISABLE CHANGE_TRACKING ;
```

### <a name="disabling-and-enabling-constraints-and-triggers"></a><a name="disable_enable"></a>Désactivation et activation des contraintes et des déclencheurs

#### <a name="a-disabling-and-re-enabling-a-constraint"></a>R. Désactivation et réactivation d'une contrainte

L'exemple suivant désactive la contrainte définissant les salaires pouvant être inclus dans les données. L'option `NOCHECK CONSTRAINT` est utilisée avec `ALTER TABLE` pour désactiver la contrainte et permettre une insertion qui entraîne généralement une violation de la contrainte. `CHECK CONSTRAINT` réactive la contrainte.

```sql
CREATE TABLE dbo.cnst_example (
  id INT NOT NULL,
  name VARCHAR(10) NOT NULL,
  salary MONEY NOT NULL
  CONSTRAINT salary_cap CHECK (salary < 100000)) ;

-- Valid inserts
INSERT INTO dbo.cnst_example VALUES (1,'Joe Brown',65000) ;
INSERT INTO dbo.cnst_example VALUES (2,'Mary Smith',75000) ;

-- This insert violates the constraint.
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Disable the constraint and try again.
ALTER TABLE dbo.cnst_example NOCHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Re-enable the constraint and try another insert; this will fail.
ALTER TABLE dbo.cnst_example CHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (4,'Eric James',110000) ;
```

#### <a name="b-disabling-and-re-enabling-a-trigger"></a>B. Désactivation et réactivation d'un déclencheur

L'exemple suivant utilise l'option `DISABLE TRIGGER` de l'instruction `ALTER TABLE` pour désactiver le déclencheur et autoriser une insertion qui ne respecte normalement pas le déclencheur. `ENABLE TRIGGER` est ensuite utilisée pour réactiver le déclencheur.

```sql
CREATE TABLE dbo.trig_example (
  id INT,
  name VARCHAR(12),
  salary MONEY) ;
GO
-- Create the trigger.
CREATE TRIGGER dbo.trig1 ON dbo.trig_example FOR INSERT
AS
IF (SELECT COUNT(*) FROM INSERTED
WHERE salary > 100000) > 0
BEGIN
    print 'TRIG1 Error: you attempted to insert a salary > $100,000'
    ROLLBACK TRANSACTION
END ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (1,'Pat Smith',100001) ;
GO
-- Disable the trigger.
ALTER TABLE dbo.trig_example DISABLE TRIGGER trig1 ;
GO
-- Try an insert that would typically violate the trigger.
INSERT INTO dbo.trig_example VALUES (2,'Chuck Jones',100001) ;
GO
-- Re-enable the trigger.
ALTER TABLE dbo.trig_example ENABLE TRIGGER trig1 ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (3,'Mary Booth',100001) ;
GO
```

### <a name="online-operations"></a><a name="online"></a>Opérations en ligne

#### <a name="a-online-index-rebuild-using-low-priority-wait-options"></a>R. Reconstruction d'index en ligne à l'aide d'options d'attente à basse priorité

L'exemple suivant montre comment effectuer une reconstruction d'index en ligne qui spécifie les options d'attente à basse priorité.

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE T1
REBUILD WITH
(
    PAD_INDEX = ON,
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES,
                                         ABORT_AFTER_WAIT = BLOCKERS ) )
) ;
```

#### <a name="b-online-alter-column"></a>B. Modification de colonne en ligne

L'exemple suivant montre comment exécuter une opération de modification de colonne avec l'option ONLINE.

**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy
    ALTER COLUMN column_a DECIMAL (5, 2) WITH (ONLINE = ON) ;
GO
sp_help doc_exy;
DROP TABLE dbo.doc_exy ;
GO
```

### <a name="system-versioning"></a><a name="system_versioning"></a> Gestion système des versions

Les quatre exemples ci-dessous vous aideront à vous familiariser avec la syntaxe d’utilisation de la gestion système des versions. Pour obtenir une assistance supplémentaire, consultez [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md).

**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

#### <a name="a-add-system-versioning-to-existing-tables"></a>R. Ajouter la gestion système des versions à des tables existantes

L’exemple suivant montre comment ajouter la gestion système des versions à une table existante, et comment créer une table d’historique future. Cet exemple part du principe qu’il existe une table nommée `InsurancePolicy` avec une clé primaire définie. Cet exemple remplit les colonnes de période nouvellement créées pour la gestion système des versions à l’aide des valeurs par défaut pour les heures de début et de fin, car ces valeurs ne peuvent pas être Null. Cet exemple utilise la clause HIDDEN pour s’assurer qu’il n’y a aucun impact sur les applications existantes interagissant avec la table active. Il utilise également HISTORY_RETENTION_PERIOD, qui est disponible uniquement sur [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

```sql
--Alter non-temporal table to define periods for system versioning
ALTER TABLE InsurancePolicy
ADD PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime),
SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL
    DEFAULT SYSUTCDATETIME(),
SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL
    DEFAULT CONVERT(DATETIME2, '9999-12-31 23:59:59.99999999') ;

--Enable system versioning with 1 year retention for historical data
ALTER TABLE InsurancePolicy
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 1 YEAR)) ;
```

#### <a name="b-migrate-an-existing-solution-to-use-system-versioning"></a>B. Migrer une solution existante pour utiliser la gestion système des versions

L’exemple suivant montre comment migrer vers la gestion système des versions à partir d’une solution qui utilise des déclencheurs pour reproduire la prise en charge temporelle. Il part du principe qu’il existe une solution qui utilise une table `ProjectTask` et une table `ProjectTaskHistory`, qui utilise les colonnes `Changed Date` et `Revised Date` comme périodes, que ces colonnes de période n’utilisent pas le type de données `datetime2` et que la table `ProjectTask` a une clé primaire définie.

```sql
-- Drop existing trigger
DROP TRIGGER ProjectTask_HistoryTrigger;

-- Adjust the schema for current and history table
-- Change data types for existing period columns
ALTER TABLE ProjectTask ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTask ALTER COLUMN [Revised Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Revised Date] datetime2 NOT NULL ;

-- Add SYSTEM_TIME period and set system versioning with linking two existing tables
-- (a certain set of data checks happen in the background)
ALTER TABLE ProjectTask
ADD PERIOD FOR SYSTEM_TIME ([Changed Date], [Revised Date])

ALTER TABLE ProjectTask
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProjectTaskHistory, DATA_CONSISTENCY_CHECK = ON))
```

#### <a name="c-disabling-and-re-enabling-system-versioning-to-change-table-schema"></a>C. Désactivation et réactivation de la gestion système des versions pour changer le schéma de table

Cet exemple montre comment désactiver la gestion système des versions sur la table `Department`, ajouter une colonne et réactiver la gestion système des versions. La désactivation de la gestion système des versions est nécessaire pour modifier le schéma de table. Effectuez ces étapes dans une transaction pour empêcher les mises à jour des deux tables lors de la mise à jour du schéma de table, ce qui permet à l’administrateur de base de données d’ignorer la vérification de cohérence des données pendant la réactivation de la gestion système des versions et d’obtenir un gain de performances. Les tâches telles que la création de statistiques, le changement de partition ou la compression de l’une ou des deux tables ne nécessitent pas la désactivation de la gestion système des versions.

```sql
BEGIN TRAN
/* Takes schema lock on both tables */
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
/* expand table schema for temporal table */
ALTER TABLE Department  
     ADD Col5 int NOT NULL DEFAULT 0 ;
/* Expand table schema for history table */
ALTER TABLE DepartmentHistory
    ADD Col5 int NOT NULL DEFAULT 0 ;
/* Re-establish versioning again*/
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE=dbo.DepartmentHistory,
                                 DATA_CONSISTENCY_CHECK = OFF)) ;
COMMIT
```

#### <a name="d-removing-system-versioning"></a>D. Suppression de la gestion système des versions

Cet exemple montre comment supprimer complètement la gestion système des versions de la table Department, et comment supprimer la table `DepartmentHistory`. Si vous le souhaitez, vous pouvez aussi supprimer les colonnes de période utilisées par le système pour enregistrer les informations de gestion système des versions. Vous ne pouvez pas supprimer les tables `Department` ou `DepartmentHistory` pendant que la gestion système des versions est activée.

```sql
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
ALTER TABLE Department
DROP PERIOD FOR SYSTEM_TIME ;
DROP TABLE DepartmentHistory ;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Les exemples suivants A à C utilisent la table `FactResellerSales` dans la base de données [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].

### <a name="a-determining-if-a-table-is-partitioned"></a>R. Déterminer si une table est partitionnée

La requête suivante renvoie une ou plusieurs lignes si la table `FactResellerSales` est partitionnée. Si la table n'est pas partitionnée, aucune ligne n'est retournée.

```sql
SELECT * FROM sys.partitions AS p
JOIN sys.tables AS t
    ON p.object_id = t.object_id
WHERE p.partition_id IS NOT NULL
    AND t.name = 'FactResellerSales' ;
```

### <a name="b-determining-boundary-values-for-a-partitioned-table"></a>B. Déterminer les valeurs limites pour une table partitionnée

La requête suivante renvoie les valeurs limites pour chaque partition de la table `FactResellerSales` .

```sql
SELECT t.name AS TableName, i.name AS IndexName, p.partition_number,
    p.partition_id, i.data_space_id, f.function_id, f.type_desc,
    r.boundary_id, r.value AS BoundaryValue
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.partitions AS p
    ON i.object_id = p.object_id AND i.index_id = p.index_id
JOIN  sys.partition_schemes AS s
    ON i.data_space_id = s.data_space_id
JOIN sys.partition_functions AS f
    ON s.function_id = f.function_id
LEFT JOIN sys.partition_range_values AS r
    ON f.function_id = r.function_id and r.boundary_id = p.partition_number
WHERE t.name = 'FactResellerSales' AND i.type <= 1
ORDER BY p.partition_number ;
```

### <a name="c-determining-the-partition-column-for-a-partitioned-table"></a>C. Déterminer la colonne de partition pour une table partitionnée

La requête suivante renvoie le nom de la colonne de partitionnement pour une table. `FactResellerSales`.

```sql
SELECT t.object_id AS Object_ID, t.name AS TableName,
    ic.column_id as PartitioningColumnID, c.name AS PartitioningColumnName
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.columns AS c
    ON t.object_id = c.object_id
JOIN sys.partition_schemes AS ps
    ON ps.data_space_id = i.data_space_id
JOIN sys.index_columns AS ic
    ON ic.object_id = i.object_id
    AND ic.index_id = i.index_id AND ic.partition_ordinal > 0
WHERE t.name = 'FactResellerSales'
AND i.type <= 1
AND c.column_id = ic.column_id ;
```

### <a name="d-merging-two-partitions"></a>D. Fusionner deux partitions

L’exemple suivant fusionne deux partitions sur une table.

La table `Customer` a la définition suivante :

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100))) ;
```

La commande suivante combine les limites de 10 et 25 partitions.

```sql
ALTER TABLE Customer MERGE RANGE (10);
```

La nouvelle DDL pour la table est la suivante :

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 25, 50, 100))) ;
```

### <a name="e-splitting-a-partition"></a>E. Fractionner une partition

L’exemple suivant fractionne une partition sur une table.

La table `Customer` a la DDL suivante :

```sql
DROP TABLE Customer;

CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100 ))) ;
```

La commande suivante crée une partition liée par la valeur 75, entre 50 et 100.

```sql
ALTER TABLE Customer SPLIT RANGE (75);
```

La nouvelle DDL pour la table est la suivante :

```sql
CREATE TABLE Customer (
   id INT NOT NULL,
   lastName VARCHAR(20),
   orderCount INT,
   orderDate DATE)
   WITH DISTRIBUTION = HASH(id),
   PARTITION ( orderCount (RANGE LEFT
      FOR VALUES (1, 5, 10, 25, 50, 75, 100))) ;
```

### <a name="f-using-switch-to-move-a-partition-to-a-history-table"></a>F. Utiliser SWITCH pour déplacer une partition vers une table d’historique

L’exemple suivant déplace les données dans une partition de la table `Orders` vers une partition dans la table `OrdersHistory`.

La table `Orders` a la DDL suivante :

```sql
CREATE TABLE Orders (
    id INT,
    city VARCHAR (25),
    lastUpdateDate DATE,
    orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01', '2005-01-01', '2006-01-01', '2007-01-01'))) ;
```

Dans cet exemple, la table `Orders` a les partitions suivantes. Chaque partition contient des données.

|Partition|Contient des données ?|Plage de limite|
|---------------|---------------|--------------------|
|1|Oui|OrderDate < '2004-01-01'|
|2|Oui|'2004-01-01' <= OrderDate < '2005-01-01'|
|3|Oui|'2005-01-01' <= OrderDate< '2006-01-01'|
|4|Oui|'2006-01-01'<= OrderDate < '2007-01-01'|
|5|Oui|'2007-01-01' <= OrderDate|
| &nbsp; | &nbsp; | &nbsp; |

- Partition 1 (a des données) : OrderDate < '2004-01-01'
- Partition 2 (a des données) : '2004-01-01' <= OrderDate < '2005-01-01'
- Partition 3 (a des données) : '2005-01-01' <= OrderDate< '2006-01-01'
- Partition 4 (a des données) : '2006-01-01'<= OrderDate < '2007-01-01'
- Partition 5 (a des données) : '2007-01-01' <= OrderDate

La table `OrdersHistory` a la DDL suivante, qui a des colonnes et des noms de colonnes identiques à ceux de la table `Orders`. Toute deux sont distribuées par hachage sur la colonne `id`.

```sql
CREATE TABLE OrdersHistory (
   id INT,
   city VARCHAR (25),
   lastUpdateDate DATE,
   orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01'))) ;
```

Bien que les colonnes et les noms de colonnes doivent être identiques, les limites de partition ne doivent pas obligatoirement être les mêmes. Dans cet exemple, la table `OrdersHistory` a les deux partitions suivantes, toutes deux vides :

- Partition 1 (aucune donnée) : OrderDate < '2004-01-01'
- Partition 2 (vide) : '2004-01-01' <= OrderDate

Pour les deux tables précédentes, la commande suivante déplace toutes les lignes avec `OrderDate < '2004-01-01'` de la table `Orders` vers la table `OrdersHistory`.

```sql
ALTER TABLE Orders SWITCH PARTITION 1 TO OrdersHistory PARTITION 1;
```

En conséquence, la première partition dans `Orders` est vide et la première partition dans `OrdersHistory` contient des données. Les tables apparaissent maintenant comme suit :

 Table `Orders`

- Partition 1 (vide) : OrderDate < '2004-01-01'
- Partition 2 (a des données) : '2004-01-01' <= OrderDate < '2005-01-01'
- Partition 3 (a des données) : '2005-01-01' <= OrderDate< '2006-01-01'
- Partition 4 (a des données) : '2006-01-01'<= OrderDate < '2007-01-01'
- Partition 5 (a des données) : '2007-01-01' <= OrderDate

Table `OrdersHistory`

- Partition 1 (a des données) : OrderDate < '2004-01-01'
- Partition 2 (vide) : '2004-01-01' <= OrderDate

Pour nettoyer la table `Orders`, vous pouvez supprimer la partition vide en fusionnant les partitions 1 et 2 comme suit :

```sql
ALTER TABLE Orders MERGE RANGE ('2004-01-01');
```

Après la fusion, la table `Orders` a les partitions suivantes :

Table `Orders`

- Partition 1 (a des données) : OrderDate < '2005-01-01'
- Partition 2 (a des données) : '2005-01-01' <= OrderDate< '2006-01-01'
- Partition 3 (a des données) : '2006-01-01'<= OrderDate < '2007-01-01'
- Partition 4 (a des données) : '2007-01-01' <= OrderDate

Supposez qu’une autre année s’écoule et que vous êtes prêt à archiver l’année 2005. Vous pouvez allouer une partition vide pour l’année 2005 dans la table `OrdersHistory` en fractionnant la partition vide comme suit :

```sql
ALTER TABLE OrdersHistory SPLIT RANGE ('2005-01-01');
```

Après le fractionnement, la table `OrdersHistory` a les partitions suivantes :

 Table `OrdersHistory`

- Partition 1 (a des données) : OrderDate < '2004-01-01'
- Partition 2 (vide) : '2004-01-01' < '2005-01-01'
- Partition 3 (vide) : '2005-01-01' <= OrderDate

## <a name="see-also"></a>Voir aussi

- [sys.tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)
- [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DROP TABLE](../../t-sql/statements/drop-table-transact-sql.md)
- [sp_help](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)
- [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md)
- [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
