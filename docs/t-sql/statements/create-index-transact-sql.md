---
description: CREATE INDEX (Transact-SQL)
title: CREATE INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE INDEX
- INDEX
- INDEX_TSQL
- CREATE_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML INDEX statement
- PRIMARY XML INDEX statement
- indexes [SQL Server], creating
- online index operations
- index keys [SQL Server]
- PROPERTY INDEX statement
- computed columns, index creation
- clustered indexes, creating
- indexed views [SQL Server], index creation
- partitioned indexes [SQL Server], creating
- VALUE INDEX statement
- index creation [SQL Server], CREATE INDEX statement
- DROP_EXISTING clause
- row locks [SQL Server]
- composite indexes
- MAXDOP index option, CREATE INDEX statement
- filtered indexes [SQL Server], creating
- PATH INDEX statement
- locking [SQL Server], indexes
- unique indexes, creating
- primary indexes [SQL Server]
- CREATE PRIMARY XML INDEX statement
- SET statement, index creation
- index options [SQL Server]
- included columns
- ONLINE option
- nonclustered indexes [SQL Server], creating
- CREATE INDEX statement
- IGNORE_DUP_KEY option
- deterministic functions
- keys [SQL Server], index
- SECONDARY XML INDEX statement
- page locks [SQL Server]
- secondary indexes [SQL Server]
- XML indexes [SQL Server], creating
ms.assetid: d2297805-412b-47b5-aeeb-53388349a5b9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 01a80dd71397a4528c1d56882cec5934d750bb8f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093504"
---
# <a name="create-index-transact-sql"></a>CREATE INDEX (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Crée un index relationnel sur une table ou une vue. Également appelé index rowstore, car il s’agit d’un index B-Tree cluster ou non-cluster. Vous pouvez créer un index rowstore avant que la table soit remplie de données. Utilisez un index rowstore pour améliorer les performances des requêtes, en particulier quand les requêtes effectuent une sélection dans des colonnes spécifiques ou qu’elles exigent que les valeurs soient triées dans un ordre particulier.

> [!NOTE]
> [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ne prennent actuellement pas en charge les contraintes Unique. Les exemples faisant référence à des contraintes Unique sont applicables uniquement à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et à [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
> [!TIP]
> Pour obtenir des informations sur les règles de conception d’index, consultez le [Guide de conception d’index SQL Server](../../relational-databases/sql-server-index-design-guide.md).

 **Exemples simples :**

```sql
-- Create a nonclustered index on a table or view
CREATE INDEX i1 ON t1 (col1);

-- Create a clustered index on a table and use a 3-part name for the table
CREATE CLUSTERED INDEX i1 ON d1.s1.t1 (col1);

-- Syntax for SQL Server and Azure SQL Database
-- Create a nonclustered index with a unique constraint
-- on 3 columns and specify the sort order for each column
CREATE UNIQUE INDEX i1 ON t1 (col1 DESC, col2 ASC, col3 DESC);
```

**Scénario clé :**

Depuis [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)], utilisez un index non-cluster sur un index columnstore pour améliorer les performances des requêtes d’entreposage de données. Pour plus d’informations, consultez [Index columnstore - Entrepôt de données](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md).

Pour d’autres types d’index, consultez :

- [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md)
- [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)
- [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md)

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

### <a name="syntax-for-sql-server-and-azure-sql-database"></a>Syntaxe pour SQL Server et Azure SQL Database

```syntaxsql
CREATE [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON <object> ( column [ ASC | DESC ] [ ,...n ] )
    [ INCLUDE ( column_name [ ,...n ] ) ]
    [ WHERE <filter_predicate> ]
    [ WITH ( <relational_index_option> [ ,...n ] ) ]
    [ ON { partition_scheme_name ( column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
[ ; ]
  
<object> ::=
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }

<relational_index_option> ::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | SORT_IN_TEMPDB = { ON | OFF }
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | STATISTICS_INCREMENTAL = { ON | OFF }
  | DROP_EXISTING = { ON | OFF }
  | ONLINE = { ON | OFF }
  | RESUMABLE = { ON | OFF }
  | MAX_DURATION = <time> [MINUTES]
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS = { ON | OFF }
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
  | MAXDOP = max_degree_of_parallelism
  | DATA_COMPRESSION = { NONE | ROW | PAGE }
     [ ON PARTITIONS ( { <partition_number_expression> | <range> }
     [ , ...n ] ) ]
}

<filter_predicate> ::=
    <conjunct> [ AND <conjunct> ]

<conjunct> ::=
    <disjunct> | <comparison>

<disjunct> ::=
        column_name IN (constant ,...n)

<comparison> ::=
        column_name <comparison_op> constant

<comparison_op> ::=
    { IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !< }

<range> ::=
<partition_number_expression> TO <partition_number_expression>
```

### <a name="backward-compatible-relational-index"></a>Index relationnel à compatibilité descendante

> [!IMPORTANT]
> La structure de la syntaxe de l’index relationnel à compatibilité descendante sera supprimée dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
> Évitez d'utiliser cette structure de syntaxe dans tout nouveau travail de développement et envisagez de modifier les applications qui l'utilisent actuellement.
> Utilisez la structure de syntaxe spécifiée dans <relational_index_option> à la place.

```syntaxsql
CREATE [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON <object> ( column_name [ ASC | DESC ] [ ,...n ] )
    [ WITH <backward_compatible_index_option> [ ,...n ] ]
    [ ON { filegroup_name | "default" } ]

<object> ::=
{
    [ database_name. [ owner_name ] . | owner_name. ]
    table_or_view_name
}

<backward_compatible_index_option> ::=
{
    PAD_INDEX
  | FILLFACTOR = fillfactor
  | SORT_IN_TEMPDB
  | IGNORE_DUP_KEY
  | STATISTICS_NORECOMPUTE
  | DROP_EXISTING
}
```

### <a name="syntax-for-azure-synapse-analytics-and-parallel-data-warehouse"></a>Syntaxe pour Azure Synapse Analytics et Parallel Data Warehouse

```syntaxsql

CREATE CLUSTERED COLUMNSTORE INDEX index_name
    ON [ database_name . [ schema ] . | schema . ] table_name
    [ORDER (column[,...n])]
    [WITH ( DROP_EXISTING = { ON | OFF } )]
[;]


CREATE [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON [ database_name . [ schema ] . | schema . ] table_name
        ( { column [ ASC | DESC ] } [ ,...n ] )
    WITH ( DROP_EXISTING = { ON | OFF } )
[;]


```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

UNIQUE      
Crée un index unique sur une table ou une vue. Un index unique est un index dans lequel deux lignes ne peuvent pas avoir la même valeur de clé d'index. Un index cluster d'une vue doit être unique.

Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne permet pas de créer un index unique sur des colonnes qui contiennent déjà des valeurs dupliquées, qu'IGNORE_DUP_KEY soit ou non activé (ON). Si vous tentez de le faire, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] affiche un message d'erreur. Les valeurs dupliquées doivent être supprimées pour qu'un index unique puisse être créé sur la ou les colonnes. Les colonnes utilisées dans un index unique doivent être définies avec la valeur NOT NULL, car plusieurs valeurs Null sont considérées comme des valeurs dupliquées lors de la création d'un index unique.

CLUSTERED      
Crée un index dans lequel l'ordre logique des valeurs de clés détermine l'ordre physique des lignes correspondantes dans une table. Le niveau inférieur (ou feuille) de l'index cluster contient les lignes de données réelles de la table. Une table ou une vue ne peut avoir qu'un seul index cluster à la fois.

Une vue avec un index cluster unique est appelée une vue indexée. La création d'un index cluster unique sur une vue matérialise physiquement la vue. Un index cluster unique doit être créé sur une vue avant la définition de tout autre index sur cette même vue. Pour plus d’informations, consultez [Créer des vues indexées](../../relational-databases/views/create-indexed-views.md).

Créez l'index cluster avant les index non cluster. Les index non cluster déjà existants sur les tables sont reconstruits lors de la création d'un index cluster.

Si `CLUSTERED` n’est pas spécifié, un index non cluster est créé.

> [!NOTE]
> Le niveau feuille d’un index cluster et les pages de données étant, par définition, identiques, la création d’un index cluster et l’utilisation de la clause ON *partition_scheme_name* ou ON *filegroup_name* déplace en fait une table du groupe de fichiers dans lequel la table a été créée vers le nouveau schéma de partition ou le nouveau groupe de fichiers. Avant de créer des tables ou des index sur des groupes de fichiers spécifiques, vérifiez quels sont les groupes de fichiers disponibles et s'ils disposent de suffisamment d'espace vide pour l'index.

Dans certains cas, la création d'un index cluster peut activer les index précédemment désactivés. Pour plus d’informations, consultez [Activer les index et contraintes](../../relational-databases/indexes/enable-indexes-and-constraints.md) et [Désactiver les index et contraintes](../../relational-databases/indexes/disable-indexes-and-constraints.md).

NONCLUSTERED      
Crée un index qui spécifie l'ordre logique d'une table. Avec un index non cluster, l'ordre physique des lignes de données est indépendant de l'ordre indexé.

Chaque table peut comporter jusqu'à 999 index non cluster, indépendamment de la façon dont les index sont créés : implicitement avec des contraintes PRIMARY KEY et UNIQUE ou explicitement avec CREATE INDEX.

Pour les vues indexées, les index non cluster peuvent être créés uniquement dans une vue ayant un index cluster unique déjà défini.

Sauf indication contraire, le type d’index par défaut est NONCLUSTERED.

*index_name*      
 Nom de l'index. Les noms d'index doivent être uniques dans une table ou une vue, mais ne doivent pas être nécessairement uniques dans une base de données. Les noms d’index doivent se conformer aux règles régissant les [identificateurs](../../relational-databases/databases/database-identifiers.md).

*column*      
 Colonnes sur lesquelles l'index est basé. Spécifiez deux ou plusieurs noms de colonnes pour créer un index composite sur les valeurs combinées des colonnes spécifiées. Répertoriez les colonnes à inclure dans l’index composite, suivant l’ordre de priorité de tri, dans les parenthèses après *table_or_view_name*.

Vous pouvez combiner jusqu’à 32 colonnes dans une même clé d’index composite. Toutes les colonnes d'une clé d'index composite doivent se trouver dans la même table ou la même vue. La taille maximale autorisée pour les valeurs d’index combinées est de 900 octets pour un index cluster, ou de 1 700 pour un index non-cluster. Les limites sont de 16 colonnes et de 900 octets pour les versions antérieures à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] et à [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)].

Les colonnes ayant les types de données LOB (Large OBject) **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml** ou **image** ne peuvent pas être spécifiées comme colonnes clés pour un index. De plus, une définition de vue ne peut pas contenir des colonnes **ntext**, **text** ou **image**, même si elles ne sont pas référencées dans l’instruction CREATE INDEX.

Vous pouvez créer des index sur des colonnes de type CLR défini par l'utilisateur si le type prend en charge le tri binaire. Vous pouvez également créer des index sur des colonnes calculées définies comme appels de méthodes d'une colonne de type défini par l'utilisateur, dès lors que les méthodes sont déterministes et n'exécutent pas des opérations d'accès aux données. Pour plus d’informations sur l’indexation des colonnes de types CLR définis par l’utilisateur, consultez [Types CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

[ **ASC** | DESC ]      
Détermine le sens croissant ou décroissant du tri d'une colonne d'index particulière. La valeur par défaut est ASC.

INCLUDE **(** _colonne_ [ **,** ... *n* ] **)**       
Spécifie les colonnes non clés à ajouter au niveau feuille de l'index non cluster. L'index non cluster peut être unique ou non.

Les noms de colonne ne peuvent pas être répétés dans la liste INCLUDE et ne peuvent pas être utilisés simultanément comme colonnes clés et colonnes non clés. Les index non cluster contiennent toujours les colonnes de l'index cluster si un index cluster est défini sur la table. Pour plus d’informations, consultez [Créer des index avec colonnes incluses](../../relational-databases/indexes/create-indexes-with-included-columns.md).

Tous les types de données sont autorisés, à l'exception de **text**, **ntext** et **image**. L’index doit être créé ou reconstruit hors connexion (ONLINE = OFF) si l’une des colonnes non-clés spécifiées est du type de données **varchar(max)** , **nvarchar(max)** ou **varbinary(max)** .

Les colonnes calculées déterministes et précises ou imprécises peuvent être des colonnes incluses. Les colonnes calculées dérivées des types de données **image**, **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** et **xml** peuvent être incluses dans des colonnes non-clés dès lors que le type de données de la colonne calculée est autorisé comme colonne incluse. Pour plus d'informations, consultez [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).

Pour plus d’informations sur la création d’un index XML, consultez [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md).

WHERE \<filter_predicate>      
Crée un index filtré en spécifiant les lignes à inclure dans l'index. L'index filtré doit être un index non cluster sur une table. Crée des statistiques filtrées pour les lignes de données dans l'index filtré.

Le prédicat de filtre utilise une logique de comparaison simple et ne peut pas référencer une colonne calculée, une colonne UDT, une colonne de type de données spatiales ou une colonne de type de données hierarchyID. Les comparaisons à l'aide de littéraux NULL ne sont pas autorisées avec les opérateurs de comparaison. Utilisez les opérateurs IS NULL et IS NOT NULL à la place.

Voici quelques exemples de prédicats de filtre pour la table `Production.BillOfMaterials` :

```sql
WHERE StartDate > '20000101' AND EndDate <= '20000630'

WHERE ComponentID IN (533, 324, 753)

WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL
```

Les index filtrés ne s'appliquent pas aux index XML ni aux index de recherche en texte intégral. Pour les index UNIQUES, seules les lignes sélectionnées doivent avoir des valeurs d'index unique. Les index filtrés ne permettent pas d'utiliser l'option IGNORE_DUP_KEY.

ON *partition_scheme_name* **( _column_name_ )**      

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Spécifie le schéma de partition qui définit les groupes de fichiers auxquels les partitions d'un index partitionné seront mappées. Le schéma de partition doit exister dans la base de données en exécutant soit [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md), soit [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). *column_name* spécifie la colonne par rapport à laquelle un index partitionné sera partitionné. Cette colonne doit correspondre au type de données, à la longueur et à la précision de l’argument de la fonction de partition que *partition_scheme_name* utilise. *column_name* n’est pas limité aux colonnes de la définition d’index. Toute colonne de la table de base peut être spécifiée, sauf lors du partitionnement d’un index UNIQUE ; le nom de colonne *column_name* doit être choisi parmi les noms de colonnes utilisés comme clés uniques. Cette restriction permet au [!INCLUDE[ssDE](../../includes/ssde-md.md)] de vérifier l'unicité des valeurs de clés dans une seule partition uniquement.

> [!NOTE]
> Lorsque vous partitionnez un index cluster non unique, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ajoute par défaut la colonne de partitionnement à la liste des clés d'index cluster, si elle n'est pas déjà spécifiée. Lorsque vous partitionnez un index non cluster non unique, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ajoute la colonne de partitionnement sous la forme d'une colonne (incluse) non clé de l'index, si elle n'est pas déjà spécifiée.

Si _partition_scheme_name_ ou _filegroup_ n’est pas spécifié et que la table est partitionnée, l’index est placé dans le même schéma de partition que la table sous-jacente, en utilisant la même colonne de partitionnement.

> [!NOTE]
> Vous ne pouvez pas spécifier un schéma de partitionnement dans un index XML. Si la table de base est partitionnée, l'index XML utilise le même schéma de partition que la table.

Pour plus d’informations sur le partitionnement d’index, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

ON _filegroup_name_      

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures)

Crée l'index spécifié dans le groupe de fichiers spécifié. Si aucun emplacement n'est défini et que la table ou la vue n'est pas partitionnée, l'index utilise le même groupe de fichiers que la table ou la vue sous-jacente. Le groupe de fichiers doit déjà exister.

ON **"** default **"**      

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Crée l’index spécifié dans le même groupe de fichiers ou schéma de partition que la table ou la vue.

Le terme « default », dans ce contexte, n'est pas un mot clé. Il s’agit de l’identificateur du groupe de fichiers par défaut et il doit être délimité, comme dans ON **"** default **"** ou ON **[** default **]** . Si "default" est spécifié, l'option QUOTED_IDENTIFIER doit être activée (ON) pour la session active. Il s'agit du paramètre par défaut. Pour plus d’informations, voir [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

> [!NOTE]
> « default » n’indique pas le groupe de fichiers de base de données par défaut dans le contexte de CREATE INDEX. Cela diffère de CREATE TABLE, où « default » localise la table dans le groupe de fichiers de base de données par défaut.

[ FILESTREAM_ON { *filestream_filegroup_name* | *partition_scheme_name* | "NULL" } ]      

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures)

Spécifie le positionnement de données FILESTREAM pour la table lorsqu'un index cluster est créé. La clause FILESTREAM_ON permet le déplacement des données FILESTREAM vers un schéma de partition ou un groupe de fichiers FILESTREAM différent.

_filestream_filegroup_name_ est le nom d’un groupe de fichiers FILESTREAM. Le groupe de fichiers doit avoir un fichier défini pour le groupe de fichiers à l’aide d’une instruction [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) ou [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) ; dans le cas contraire, une erreur est générée.

Si la table est partitionnée, la clause FILESTREAM_ON doit être incluse et doit spécifier un schéma de partition de groupes de fichiers FILESTREAM qui utilise les mêmes fonctions de partition et colonnes de partition que le schéma de partition pour la table. Dans le cas contraire, une erreur est générée.

Si la table n'est pas partitionnée, la colonne FILESTREAM ne peut pas être partitionnée. Les données FILESTREAM pour la table doivent être stockées dans un groupe de fichiers unique spécifié dans la clause FILESTREAM_ON.

`FILESTREAM_ON NULL` peut être spécifié dans une instruction CREATE INDEX si un index cluster est créé et si la table ne contient pas de colonne FILESTREAM.

Pour plus d’informations, consultez [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).

**\<object>::=**      

Objet qualifié complet ou partiel à indexer.

_database_name_      
Nom de la base de données.

*schema_name*      
Nom du schéma auquel la table ou la vue appartient.

*table_or_view_name*      
Nom de la table ou de la vue à indexer.

La vue doit être définie avec SCHEMABINDING pour pouvoir créer un index sur celle-ci. Un index cluster unique doit être créé sur une vue avant la création de tout index non cluster. Pour plus d'informations sur les vues indexées, consultez la section Remarques.

Depuis [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], l’objet peut être une table stockée avec un index columnstore cluster.

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] prend en charge le format de nom en trois parties _database_name_.[_schema_name_]._object_name_ quand *database_name* est la base de données active ou quand _database_name_ est `tempdb` et que _object_name_ commence par #.

**\<relational_index_option\>::=**       
Spécifie les options à utiliser lorsque vous créez l'index.

PAD_INDEX = { ON | **OFF** }      

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Spécifie le remplissage de l'index. La valeur par défaut est OFF.

ACTIVÉ      
Le pourcentage d’espace libre indiqué par *fillfactor* est appliqué aux pages de niveau intermédiaire de l’index.

OFF ou _fillfactor_ n’est pas spécifié      
Les pages de niveau intermédiaire sont presque entièrement remplies, ce qui laisse suffisamment d'espace libre pour au moins une ligne de la taille maximale permise par l'index, en prenant en compte l'ensemble de clés sur les pages intermédiaires.

L'option PAD_INDEX est utile seulement si FILLFACTOR est spécifié, car PAD_INDEX utilise le pourcentage spécifié par FILLFACTOR. Si le pourcentage défini pour FILLFACTOR n'est pas suffisamment élevé pour autoriser une ligne, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] remplace en interne le pourcentage de façon à ce qu'il autorise le minimum. Le nombre de lignes dans une page d’index intermédiaire n’est jamais inférieur à deux, quelle que soit la faiblesse de la valeur de _fillfactor_.

Dans la syntaxe de compatibilité descendante, WITH PAD_INDEX est équivalent à WITH PAD_INDEX = ON.

FILLFACTOR **=** _fillfactor_      

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Spécifie un pourcentage indiquant le taux de remplissage appliqué par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] au niveau feuille de chaque page d'index lors de la création ou de la reconstruction de l'index. *fillfactor* doit être une valeur entière comprise entre 1 et 100. Si *fillfactor* a la valeur 100, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée des index avec des pages de niveau feuille intégralement remplies.

La valeur FILLFACTOR s'applique uniquement lors de la création ou de la reconstruction de l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne conserve pas dynamiquement dans les pages le pourcentage d'espace libre défini. Pour afficher le facteur de remplissage, utilisez la vue de catalogue [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

> [!IMPORTANT]
> La création d'un index cluster avec un facteur de remplissage FILLFACTOR inférieur à 100 affecte la quantité d'espace de stockage qu'occupent les données, car le [!INCLUDE[ssDE](../../includes/ssde-md.md)] redistribue les données lorsqu'il crée l'index cluster.

Pour plus d’informations, consultez [Spécifier un facteur de remplissage pour un index](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).

SORT_IN_TEMPDB = { ON | **OFF** }      

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Spécifie s’il faut stocker les résultats temporaires du tri dans **tempdb**. La valeur par défaut est OFF, sauf pour Azure SQL Database hyperscale. Pour toutes les opérations de création d’index dans Hyperscale, SORT_IN_TEMPDB est toujours ON quelle que soit l’option spécifiée, sauf si la regénération de l’index pouvant être reprise est utilisée.

ACTIVÉ      
Les résultats intermédiaires du tri utilisés pour créer l’index sont stockés dans **tempdb**. Cela peut réduire le temps nécessaire pour créer un index si **tempdb** ne se trouve pas sur le même groupe de disques que la base de données utilisateur. Toutefois, une plus grande quantité d'espace disque est alors utilisée lors de la création de l'index.

OFF      
Les résultats de tri intermédiaires sont stockés dans la même base de données que l'index.

En plus de l’espace nécessaire dans la base de données utilisateur pour créer l’index, **tempdb** doit disposer à peu près d’autant d’espace supplémentaire pour stocker les résultats intermédiaires du tri. Pour plus d’informations, consultez [Option SORT_IN_TEMPDB pour les index](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).

Dans la syntaxe de compatibilité descendante, WITH SORT_IN_TEMPDB est équivalent à WITH SORT_IN_TEMPDB = ON.

IGNORE_DUP_KEY = { ON | **OFF** }      
Spécifie la réponse d'erreur lorsqu'une opération d'insertion essaie d'insérer des valeurs de clés en double dans un index unique. L'option IGNORE_DUP_KEY s'applique uniquement aux opérations d'insertion après la création ou la régénération de l'index. Cette option n’a aucun effet lors de l’exécution de [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), d’[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) ou d’[UPDATE](../../t-sql/queries/update-transact-sql.md). La valeur par défaut est OFF.

ACTIVÉ      
Un message d'avertissement s'affichera lorsque des valeurs de clé en double sont insérées dans un index unique. Seules les lignes qui violent la contrainte d'unicité échouent.

OFF      
Un message d'erreur s'affichera lorsque des valeurs de clé en double sont insérées dans un index unique. L'intégralité de l'opération INSERT sera restaurée.

IGNORE_DUP_KEY ne peut pas être activé (ON) dans le cas d'index créés sur une vue, d'index non uniques, d'index XML, d'index spatiaux et d'index filtrés.

Pour afficher IGNORE_DUP_KEY, utilisez [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

Dans la syntaxe de compatibilité descendante, WITH IGNORE_DUP_KEY est équivalent à WITH IGNORE_DUP_KEY = ON.

STATISTICS_NORECOMPUTE = { ON | **OFF**}      
Spécifie si les statistiques de distribution sont recalculées. La valeur par défaut est OFF.

ACTIVÉ      
Les statistiques obsolètes ne sont pas recalculées automatiquement.

OFF      
La mise à jour automatique des statistiques est activée.

Pour restaurer la mise à jour automatique des statistiques, affectez la valeur OFF à STATISTICS_NORECOMPUTE ou exécutez UPDATE STATISTICS sans la clause NORECOMPUTE.

> [!IMPORTANT]
> La désactivation du recalcul automatique des statistiques de distribution peut empêcher l'optimiseur de requête de sélectionner des plans d'exécution optimaux pour les requêtes qui impliquent la table.

Dans la syntaxe de compatibilité descendante, WITH STATISTICS_NORECOMPUTE est équivalent à WITH STATISTICS_NORECOMPUTE = ON.

STATISTICS_INCREMENTAL = { ON | **OFF** }     

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Si la valeur **ON** est définie, les statistiques sont créées par partition. Si la valeur est **OFF**, l’arborescence des statistiques est supprimée et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recalcule les statistiques. La valeur par défaut est **OFF**.

Si les statistiques par partition ne sont pas prises en charge, l'option est ignorée et un avertissement est généré. Les statistiques incrémentielles ne sont pas prises en charge pour les types de statistiques suivants :

- statistiques créées avec des index qui ne sont pas alignés sur les partitions avec la table de base ;
- statistiques créées sur les bases de données secondaires lisibles Always On ;
- statistiques créées sur les bases de données en lecture seule ;
- statistiques créées sur les index filtrés ;
- statistiques créées sur les vues ;
- statistiques créées sur les tables internes ;
- Statistiques créées avec les index spatiaux ou les index XML.

DROP_EXISTING = { ON | **OFF** }      
Option permettant de supprimer et de reconstruire l’index cluster ou non-cluster existant avec des spécifications de colonne modifiées, tout en conservant le même nom pour l’index. La valeur par défaut est OFF.

ACTIVÉ      
Spécifie de supprimer et de reconstruire l’index existant, qui doit avoir le même nom que le paramètre *index_name*.

OFF      
Spécifie de ne pas supprimer et recréer l’index existant. SQL Server affiche une erreur si le nom d’index spécifié existe déjà.

DROP_EXISTING vous permet de transformer :

- Un index rowstore non-cluster en index rowstore cluster.

DROP_EXISTING ne vous permet pas de modifier :

- Un index rowstore cluster en index rowstore non-cluster.
- Un index columnstore cluster en n’importe quel type d’index rowstore.

Dans la syntaxe de compatibilité descendante, WITH DROP_EXISTING est équivalent à WITH DROP_EXISTING = ON.

ONLINE = { ON | **OFF** }      
Indique si les tables sous-jacentes et les index associés sont disponibles pour les requêtes et la modification de données pendant l'opération d'index. La valeur par défaut est OFF.

> [!IMPORTANT]
> Les opérations d’index en ligne ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).

ACTIVÉ      
Les verrous de table à long terme ne sont pas maintenus pendant la durée de l'opération d'index. Lors de la principale phase de l'indexation, seul le verrou de partage intentionnel (IS, Intent Share) est maintenu sur la table source. Ceci permet d'exécuter les requêtes ou les mises à jour dans la table sous-jacente et ses index. Au début de l'opération, un verrou partagé (S, Shared) est placé sur l'objet source pendant une période de temps très courte. À la fin de l'opération, pendant une période de temps très courte, un verrou partagé (S, Shared) est placé sur la source si un index non cluster est créé, ou bien un verrou de SCH-M (Modification du schéma) est placé lorsqu'un index cluster est créé ou supprimé en ligne et lorsqu'un index cluster ou non cluster est régénéré. ONLINE ne peut pas prendre la valeur ON si un index est en cours de création sur une table locale temporaire.

OFF      
Des verrous de table sont appliqués pendant l'opération d'indexation. Une opération d'indexation hors ligne qui crée, régénère ou supprime un index cluster, ou régénère ou supprime un index non cluster, acquiert un verrou de modification de schéma (Sch-M) sur la table. Cela empêche tous les utilisateurs d'accéder à la table sous-jacente pendant la durée de l'opération. Une opération d'indexation hors ligne qui crée un index non cluster acquiert un verrou partagé (S, Shared) sur la table. Cela empêche la mise à jour de la table sous-jacente, mais autorise les opérations de lecture, telles que des instructions SELECT.

Pour plus d'informations, consultez [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  

Les index, notamment les index portant sur des tables temporaires globales, ne peuvent pas être créés en ligne, à l’exception des cas suivants :

- Index XML
- Index de table temporaire locale
- Index cluster unique de départ sur une vue
- Index cluster désactivés
- Index columnstore
- Idex cluster si la table sous-jacente contient des types de données LOB (**image**, **ntext**, **text**) et des types de données spatiales
- Les colonnes **varchar(max)** et **varbinary(max)** ne peuvent pas faire partie d’un index. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], quand une table contient des colonnes **varchar(max)** ou **varbinary(max)** , un index cluster contenant d’autres colonnes peut être généré ou régénéré à l’aide de l’option **ONLINE**. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] n’autorise pas l’option **ONLINE** quand la table de base contient des colonnes **varchar(max)** ou **varbinary(max)**

Pour plus d’informations, consultez [Fonctionnement des opérations d’index en ligne](../../relational-databases/indexes/how-online-index-operations-work.md).

RESUMABLE **=** { ON | **OFF**}      

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

 Spécifie si une opération d’index en ligne peut être reprise.

 ACTIVÉ      
L’opération d’index peut être reprise.

 OFF      
L’opération de l’index ne peut pas être reprise.

MAX_DURATION **=** *heure* [**MINUTES**] utilisé avec **RESUMABLE = ON** (nécessite **ONLINE = ON**)   

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Indique le temps (valeur entière spécifiée en minutes) pendant lequel une opération d’index en ligne pouvant être reprise est exécutée avant d’être mise en pause.

> [!IMPORTANT]
> Pour plus d’informations sur les opérations d’index pouvant être effectuées en ligne, consultez [Instructions pour les opérations d’index en ligne](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

> [!NOTE] 
> Les régénérations d’index en ligne pouvant être reprise ne sont pas prises en charge sur les index columnstore.

ALLOW_ROW_LOCKS = { **ON** | OFF }      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Indique si les verrous de ligne sont autorisés ou non. La valeur par défaut est ON.

ACTIVÉ      
Les verrous de ligne sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de ligne sont utilisés.

OFF      
Les verrous de ligne ne sont pas utilisés.

ALLOW_PAGE_LOCKS = { **ON** | OFF }      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Indique si les verrous de page sont autorisés. La valeur par défaut est ON.

ACTIVÉ      
Les verrous de page sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de page sont utilisés.

OFF      
Les verrous de page ne sont pas utilisés.

OPTIMIZE_FOR_SEQUENTIAL_KEY = {ON | **OFF** }      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Spécifie s’il faut optimiser ou pas la contention d’insertion de la dernière page. La valeur par défaut est OFF. Pour plus d'informations, consultez la section [Clés séquentielles](#sequential-keys) .

MAXDOP = *max_degree_of_parallelism*      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Remplace l’option de configuration **max degree of parallelism** pendant la durée de l’opération d’index. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilisez MAXDOP pour limiter le nombre de processeurs utilisés dans une exécution de plan parallèle. Le nombre maximal de processeurs est égal à 64.

*max_degree_of_parallelism* peut avoir la valeur :

1      
Supprime la création de plans parallèles.

\>1      
Limite le nombre maximal de processeurs utilisés dans l'indexation parallèle au nombre défini ou à un nombre inférieur en fonction de la charge de travail actuelle du système.

0 (valeur par défaut)      
Utilise le nombre réel de processeurs ou un nombre de processeurs inférieur en fonction de la charge de travail actuelle du système.

 Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).

> [!NOTE]
> Les opérations d’index parallèles ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Éditions et fonctionnalités prises en charge de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) et [Éditions et fonctionnalités prises en charge pour SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

DATA_COMPRESSION      
Spécifie l'option de compression de données pour l'index, le numéro de partition ou la plage de partitions spécifiés. Les options disponibles sont les suivantes :

Aucune      
L'index ou les partitions spécifiées ne sont pas compressés.

ROW      
L'index ou les partitions spécifiées sont compressés au moyen de la compression de ligne.

PAGE      
L'index ou les partitions spécifiées sont compressés au moyen de la compression de page.

Pour plus d’informations sur la compression, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).

ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ..._n_ ] **)**       
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Spécifie les partitions auxquelles le paramètre DATA_COMPRESSION s'applique. Si l'index n'est pas partitionné, l'argument ON PARTITIONS générera une erreur. Si la clause ON PARTITIONS n'est pas fournie, l'option DATA_COMPRESSION s'applique à toutes les partitions d'un index partitionné.

\<partition_number_expression> peut être spécifié des manières suivantes :

- Spécifiez le numéro de partition, par exemple : ON PARTITIONS (2).
- Spécifiez des numéros de partition pour plusieurs partitions individuelles séparées par des virgules, par exemple : ON PARTITIONS (1, 5).
- Spécifiez à la fois des plages et des partitions individuelles, par exemple : ON PARTITIONS (2, 4, 6 TO 8).

\<range> peut être spécifié sous la forme de numéros de partitions séparés par le mot TO, par exemple : `ON PARTITIONS (6 TO 8)`.

 Pour définir des types différents de compression de données pour des partitions différentes, spécifiez plusieurs fois l'option DATA_COMPRESSION, par exemple :

```sql
REBUILD WITH
(
  DATA_COMPRESSION = NONE ON PARTITIONS (1),
  DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),
  DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)
);
```

## <a name="remarks"></a>Notes
L'instruction CREATE INDEX est optimisée comme toute autre requête. Pour consommer moins de ressources sur les opérations d'E/S, le processeur de requêtes peut choisir d'analyser un autre index au lieu d'effectuer une analyse de la table. L'opération de tri peut être éliminée dans certains cas. Sur des ordinateurs multiprocesseurs, CREATE INDEX peut utiliser plusieurs processeurs pour exécuter les opérations d'analyse et de tri associées à la création de l'index, à l'instar des autres requêtes. Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).

L'opération de création d'index peut être consignée de manière minimale si le mode de récupération de base de données correspond au mode de journalisation en bloc ou au mode simple.

Vous pouvez créer des index sur une table temporaire. Lorsque la table est supprimée ou que la session prend fin, les index sont supprimés.

Un index cluster peut être généré sur une variable de table lors de la création d’une clé primaire. Lorsque la requête est terminée ou que la session prend fin, l'index est déposé.

Les index prennent en charge les propriétés étendues.

## <a name="clustered-indexes"></a>Index cluster
La création d'un index cluster sur une table (segment de mémoire) ou la suppression et la recréation d'un index nécessite un espace de travail supplémentaire dans la base de données pour pouvoir y placer le tri des données et une copie temporaire de la table d'origine ou les données d'index cluster existantes. Pour plus d’informations sur les index cluster, consultez [Créer un index cluster](../../relational-databases/indexes/create-clustered-indexes.md) et le [Guide de conception et d’architecture d’Index SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="nonclustered-indexes"></a>Index non-cluster
Depuis [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], vous pouvez créer un index non-cluster sur une table stockée sous la forme d’un index columnstore cluster. Si vous créez tout d’abord un index non-cluster sur une table stockée sous la forme d’un segment de mémoire ou d’un index cluster, l’index est conservé si, par la suite, vous convertissez la table en un index columnstore cluster. Il n’est pas non plus nécessaire de supprimer l’index non-cluster quand vous reconstruisez l’index columnstore cluster.

Limitations et restrictions :

- L’option `FILESTREAM_ON` n’est pas valide quand vous créez un index non-cluster sur une table stockée sous la forme d’un index columnstore cluster.

## <a name="unique-indexes"></a>Index uniques
Lorsqu'un index unique existe, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] recherche les valeurs dupliquées chaque fois que des données sont ajoutées par des opérations d'insertion. Les opérations d'insertion qui génèrent des valeurs de clés dupliquées sont restaurées, et le [!INCLUDE[ssDE](../../includes/ssde-md.md)] affiche un message d'erreur. Ceci s'applique même si l'insertion modifie un grand nombre de lignes et ne génère qu'une seule valeur dupliquée. Si vous tentez d'entrer des données pour lesquelles il existe un index unique et que la clause IGNORE_DUP_KEY a la valeur ON, seules les lignes qui violent l'index UNIQUE échouent.

## <a name="partitioned-indexes"></a>Index partitionnés
Les index partitionnés sont créés et gérés pratiquement comme des tables partitionnées, mais comme les index ordinaires, ils sont gérés sous forme d'objets de base de données distincts. Un index partitionné peut être créé sur une table non partitionnée, et une table partitionnée peut avoir un index non partitionné.

Si vous créez un index sur une table partitionnée et que ne spécifiez pas un groupe de fichiers pour y placer l'index, l'index est partitionné de la même manière que la table sous-jacente. Ceci s'explique par le fait que les index sont placés par défaut dans les mêmes groupes de fichiers que leurs tables sous-jacentes et, pour une table partitionnée, dans le même schéma de partition qui utilise les mêmes colonnes de partitionnement. Quand l’index utilise le même schéma de partition et la même colonne de partitionnement que la table, il est *aligné* sur la table.

> [!WARNING]
> La création et la reconstruction des index non alignés sur une table contenant plus de 1 000 partitions sont possibles, mais ne sont pas prises en charge. Ces opérations peuvent entraîner une dégradation des performances ou une consommation de mémoire excessive. Nous vous recommandons d'utiliser uniquement des index alignés lorsque le nombre de partitions est supérieur à 1000.

Lorsque vous partitionnez un index cluster non unique, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ajoute par défaut des colonnes de partitionnement à la liste des clés d'index cluster, si elles ne sont pas déjà spécifiées.

Vous pouvez créer des vues indexées sur des tables partitionnées, en appliquant la même procédure que celle utilisée pour les index sur des tables. Pour plus d’informations sur les index cluster, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md) et le [Guide de conception et d’architecture d’Index SQL Server](../../relational-databases/sql-server-index-design-guide.md).

Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], les statistiques ne sont pas créées en analysant toutes les lignes de la table quand un index partitionné est créé ou reconstruit. Au lieu de cela, l'optimiseur de requête utilise l'algorithme d'échantillonnage par défaut pour générer des statistiques. Pour obtenir des statistiques sur les index partitionnés en analysant toutes les lignes de la table, utilisez `CREATE STATISTICS` ou `UPDATE STATISTICS` avec la clause `FULLSCAN`.

## <a name="filtered-indexes"></a>Index filtrés
Un index filtré est un index non cluster optimisé, approprié pour les requêtes qui sélectionnent un faible pourcentage de lignes d'une table. Il utilise un prédicat de filtre pour indexer une partie des données de la table. Un index filtré bien conçu peut améliorer les performances des requêtes, réduire les coûts de stockage et réduire les coûts de maintenance.

### <a name="required-set-options-for-filtered-indexes"></a>Options SET requises pour les index filtrés

Les options SET figurant dans la colonne Valeur requise sont requises chaque fois qu'une des conditions suivantes est vérifiée :

- Vous créez un index filtré.
- L'opération INSERT, UPDATE, DELETE ou MERGE modifie les données dans un index filtré.
- L’index filtré est utilisé par l’optimiseur de requête pour générer le plan de requête.

    |Options définies|Valeur requise|Valeur de serveur par défaut|Default<br /><br /> Valeur OLE DB et ODBC|Default<br /><br /> Valeur DB-Library|
    |-----------------|--------------------|--------------------------|---------------------------------------|-----------------------------------|
    |ANSI_NULLS|ACTIVÉ|ACTIVÉ|ACTIVÉ|OFF|
    |ANSI_PADDING|ACTIVÉ|ACTIVÉ|ACTIVÉ|OFF|
    |ANSI_WARNINGS*|ACTIVÉ|ACTIVÉ|ACTIVÉ|OFF|
    |ARITHABORT|ACTIVÉ|ACTIVÉ|OFF|OFF|
    |CONCAT_NULL_YIELDS_NULL|ACTIVÉ|ACTIVÉ|ACTIVÉ|OFF|
    |NUMERIC_ROUNDABORT|OFF|OFF|OFF|OFF|
    |QUOTED_IDENTIFIER|ACTIVÉ|ACTIVÉ|ACTIVÉ|OFF|
  
     * L'affectation de la valeur ON à ANSI_WARNINGS affecte de manière implicite la valeur ON à ARITHABORT, lorsque le niveau de compatibilité de la base de données est d'au moins 90. Si le niveau de compatibilité de la base de données est au maximum de 80, la valeur ON doit être affectée de manière explicite à l'option ARITHABORT.

Si les options SET sont incorrectes, les conditions suivantes peuvent se vérifier :

- L'index filtré n'est pas créé.
- Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur et restaure les instructions INSERT, UPDATE DELETE ou MERGE qui modifient les données incluses dans l'index.
- L'optimiseur de requête ne prend en compte l'index dans le plan d'exécution pour aucune instruction Transact-SQL.

 Pour plus d’informations sur les index filtrés, consultez [Créer des index filtrés](../../relational-databases/indexes/create-filtered-indexes.md) et le [Guide de conception et d’architecture d’Index SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="spatial-indexes"></a>Index spatiaux
Pour plus d’informations sur les index spatiaux, consultez [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md) et [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md).

## <a name="xml-indexes"></a>Index XML
Pour plus d’informations sur les index XML, consultez [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md) et [Index XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md).

## <a name="index-key-size"></a>Taille de clé d'index
La taille maximale d’une clé d’index est de 900 octets pour un index cluster et de 1 700 octets pour un index non-cluster. (Avant [!INCLUDE[ssSDS](../../includes/sssds-md.md)] et [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], la limite était toujours de 900 octets.) Vous pouvez créer des index qui dépassent la limite en octets sur des colonnes **varchar** si les données existantes des colonnes ne dépassent pas cette limite lors de la création de l’index. Cependant, les actions d’insertion ou de mise à jour suivantes sur les colonnes aboutissant à une taille totale supérieure à la limite échouent. La clé d’un index cluster ne peut pas contenir de colonnes **varchar** qui possèdent des données dans l’unité d’allocation ROW_OVERFLOW_DATA. Si un index cluster est créé sur une colonne **varchar** et que les données existantes se trouvent dans l’unité d’allocation IN_ROW_DATA, les actions d’insertion ou de mise à jour réalisées ultérieurement sur la colonne et susceptibles d’envoyer les données hors ligne sont vouées à l’échec.

Les index non cluster peuvent contenir des colonnes non-clés au niveau feuille de l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne tient pas compte de ces colonnes lors du calcul de la taille de la clé d’index. Pour plus d’informations, consultez [Créer des index avec colonnes incluses](../../relational-databases/indexes/create-indexes-with-included-columns.md) et le [Guide de conception et d’architecture d’Index SQL Server](../../relational-databases/sql-server-index-design-guide.md).

> [!NOTE]
> Lorsque des tables sont partitionnées, si les colonnes de clé de partitionnement ne sont pas déjà présentes dans un index cluster non unique, elles sont ajoutées à l'index par le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. La taille combinée des colonnes indexées (sans compter les colonnes incluses), plus toutes les colonnes de partitionnement ajoutées, ne peut pas dépasser 1800 octets dans un index cluster non unique.

## <a name="computed-columns"></a>Colonnes calculées
Des index peuvent être créés sur des colonnes calculées. En outre, les colonnes calculées peuvent avoir la propriété PERSISTED. Cela signifie que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] stocke les valeurs calculées dans la table et qu'il les met à jour lorsque les autres colonnes dont dépendent les colonnes calculées sont mises à jour. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise ces valeurs persistantes pour créer un index sur la colonne et lorsqu'une requête fait référence à l'index.

Pour qu'il soit possible de créer un index d'une colonne calculée, celle-ci doit être déterministe et précise. Cependant, l'utilisation de la propriété PERSISTED permet d'étendre le type des colonnes calculées indexables pour inclure :

- les colonnes calculées basées sur les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] et CLR, et les méthodes de type CLR définies par l'utilisateur que l'utilisateur marque comme étant déterministes ;
- les colonnes calculées basées sur des expressions déterministes, comme défini par le [!INCLUDE[ssDE](../../includes/ssde-md.md)], mais qui ne sont pas précises.

Les colonnes calculées persistantes nécessitent de définir les options SET ci-dessous, comme indiqué dans la section précédente [Options SET requises pour les index filtrés](#required-set-options-for-filtered-indexes).

La contrainte UNIQUE ou PRIMARY KEY peut contenir une colonne calculée dès lors qu'elle satisfait à toutes les conditions d'indexation. En particulier, la colonne calculée doit être déterministe et précise, ou déterministe et permanente. Pour plus d’informations sur le déterminisme, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

Les colonnes calculées dérivées des types de données **image**, **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** et **xml** peuvent être indexées comme colonnes clés ou comme colonnes non-clés incluses dès lors que le type de données de la colonne calculée est autorisé comme colonne clé d’index ou comme colonne non-clé incluse. Par exemple, vous ne pouvez pas créer un index XML primaire sur une colonne **xml** calculée. Si la taille de la clé d'index est supérieure à 900 octets, un message d'avertissement est affiché.

La création d'un index sur une colonne calculée peut provoquer l'échec d'une opération d'insertion ou de mise à jour qui fonctionnait auparavant. Ce type d'échec peut survenir lorsque la colonne calculée génère une erreur arithmétique. Par exemple, dans la table suivante, bien que la colonne calculée `c` retourne une erreur arithmétique, l’instruction INSERT fonctionne.

```sql
CREATE TABLE t1 (a INT, b INT, c AS a/b);
INSERT INTO t1 VALUES (1, 0);
```

Par contre, si après avoir créé la table, vous créez un index sur la colonne calculée `c`, la même instruction `INSERT` va échouer.

```sql
CREATE TABLE t1 (a INT, b INT, c AS a/b);
CREATE UNIQUE CLUSTERED INDEX Idx1 ON t1(c);
INSERT INTO t1 VALUES (1, 0);
```

Pour plus d'informations, consultez [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).

## <a name="included-columns-in-indexes"></a>Colonnes incluses dans les index
Des colonnes non clés, appelées colonnes incluses, peuvent être ajoutées au niveau feuille d'un index non cluster pour améliorer les performances d'une requête en couvrant la requête. En l'occurrence, toutes les colonnes référencées dans la requête sont incluses dans l'index sous forme de colonnes clés ou de colonnes non clés. Ainsi, l'optimiseur de requête peut rechercher toutes les informations nécessaires via une analyse de l'index ; il n'accède pas à la table, ni aux données de l'index cluster. Pour plus d’informations, consultez [Créer des index avec colonnes incluses](../../relational-databases/indexes/create-indexes-with-included-columns.md) et le [Guide de conception et d’architecture d’Index SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="specifying-index-options"></a>Définition des options d'index
[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a introduit de nouvelles options d'index et modifié également la façon dont les options sont spécifiées. Dans la syntaxe de compatibilité descendante, WITH *nom_option* est équivalent à WITH **(** \<option_name>  **= ON )** . Lorsque vous définissez les options d'index, les règles suivantes s'appliquent :

- Les nouvelles options d’index peuvent être spécifiées uniquement en utilisant WITH (**_option\_name_ = ON | OFF**).
- Vous ne pouvez pas définir les options en utilisant la syntaxe de compatibilité descendante et la nouvelle syntaxe dans une même instruction. Par exemple, si vous définissez WITH (**DROP_EXISTING, ONLINE = ON**), l’instruction échoue.
- Quand vous créez un index XML, les options doivent être spécifiées en utilisant WITH (**_option_name_= ON | OFF**).

## <a name="drop_existing-clause"></a>Clause DROP_EXISTING
Vous pouvez utiliser la clause DROP_EXISTING pour régénérer l'index, ajouter ou supprimer des colonnes, modifier des options, modifier l'ordre de tri des colonnes ou modifier le schéma de partition ou le groupe de fichiers.

Si l'index applique une contrainte PRIMARY KEY ou UNIQUE et que sa définition n'est pas modifiée, l'index est supprimé et recréé en conservant la contrainte existante. Toutefois, si la définition de l'index est modifiée, l'instruction échoue. Pour changer la définition d'une contrainte PRIMARY KEY ou UNIQUE, supprimez la contrainte et ajoutez une contrainte avec la nouvelle définition.

DROP_EXISTING améliore les performances lorsque vous recréez un index cluster, avec le même groupe de clés ou un groupe de clés différent, sur une table qui contient également des index non cluster. DROP_EXISTING remplace l'exécution d'une instruction DROP INDEX dans l'ancien index cluster, suivie de l'exécution d'une instruction CREATE INDEX pour le nouvel index cluster. Les index non cluster sont régénérés une fois, et ensuite seulement si la définition d'index est modifiée. La clause DROP_EXISTING ne régénère pas les index non cluster lorsque la définition d'index porte le même nom d'index, a les mêmes colonnes clé et de partition, le même attribut d'unicité et le même ordre de tri que l'index d'origine.

Que les index non cluster soient régénérés ou non, ils restent toujours dans leur groupes de fichiers ou schémas de partition d'origine et utilisent les fonctions de partition d'origine. Si un index cluster est régénéré dans un groupe de fichiers ou un schéma de partition différent, les index non cluster ne sont pas déplacés pour coïncider avec le nouvel emplacement de l'index cluster. Par conséquent, même si les index non cluster ont été alignés sur l'index cluster, ils peuvent ne plus être alignés avec celui-ci. Pour plus d’informations sur les index partitionnés, consultez [Index et tables partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

La clause DROP_EXISTING ne retrie pas les données si les mêmes colonnes de clé d'index sont utilisées dans le même ordre et avec le même ordre croissant ou descendant, sauf si l'instruction d'indexation spécifie un index non cluster et que l'option ONLINE est désactivée (OFF). Si l'index cluster est désactivé, l'opération CREATE INDEX WITH DROP_EXISTING doit être exécutée avec l'option ONLINE désactivée (OFF). Si un index non cluster est désactivé et n'est pas associé à un index cluster désactivé, l'opération CREATE INDEX WITH DROP_EXISTING peut être exécutée avec l'option ONLINE désactivée (OFF) ou activée (ON).

> [!NOTE]
> Lorsque des index avec 128 extensions ou plus sont supprimés ou régénérés, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] diffère les désallocations de page réelles et leurs verrous associés jusqu'à la validation de la transaction.

## <a name="online-option"></a>Option ONLINE
Les instructions suivantes s'appliquent aux opérations d'indexation en ligne :

- La table sous-jacente ne peut pas être modifiée, tronquée ou supprimée tant qu'une opération d'indexation en ligne est en cours.
- Un espace disque temporaire supplémentaire est nécessaire au cours de l'opération d'indexation.
- Des opérations en ligne peuvent être exécutées sur les index partitionnés et sur les index qui contiennent des colonnes calculées persistantes ou des colonnes incluses.

Pour plus d'informations, consultez [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).

### <a name="resumable-index-operations"></a><a name="resumable-indexes"></a> Opérations d’index pouvant être reprises
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Les instructions suivantes s’appliquent aux opérations d’index pouvant être reprises :

- La création d’index en ligne est spécifiée comme pouvant être reprise avec l’option `RESUMABLE = ON`.
- L’option RESUMABLE n’est pas persistante dans les métadonnées d’un index donné et s’applique uniquement à la durée d’une instruction DDL actuelle. C’est pourquoi la clause `RESUMABLE = ON` doit être spécifiée explicitement pour permettre la reprise.
- L’option MAX_DURATION n’est prise en charge que pour l’option `RESUMABLE = ON`.
- MAX_DURATION pour l’option RESUMABLE spécifie l’intervalle de temps d’un index en cours de génération. Une fois ce délai écoulé, la génération d’index est mise en pause ou termine son exécution. L’utilisateur choisit quand reprendre la génération d’un index en pause. La **durée** en minutes de MAX_DURATION doit être supérieure à 0 minute et inférieur ou égale à une semaine (7 \* 24 \* 60 = 10080 minutes). Si la mise en pause d’une opération d’index est longue, les performances DML d’une table spécifique et la capacité de disque de la base de données risquent d’être impactées parce que les deux index, l’original et celui nouvellement créé, ont besoin d’espace disque et doivent être mis à jour au cours des opérations DML. Si l’option MAX_DURATION est omise, l’opération d’index se poursuit jusqu'à ce qu’elle se termine ou qu’une défaillance se produise.
- Pour interrompre immédiatement l’opération d’index, vous pouvez arrêter (Ctrl-C) la commande en cours ou bien exécuter la commande [ALTER INDEX](alter-index-transact-sql.md) PAUSE ou la command `KILL <session_id>`. Une commande en pause peut être reprise avec la commande [ALTER INDEX](alter-index-transact-sql.md).
- Si l’instruction CREATE INDEX d’origine est réexécutée sur un index pouvant être repris, l’opération de création d’index en pause est automatiquement reprise.
- L’option `SORT_IN_TEMPDB = ON` n’est pas prise en charge sur l’index pouvant être repris.
- La commande DDL avec `RESUMABLE = ON` ne peut pas être exécutée dans une transaction explicite (ne peut pas faire partie du block begin TRAN ... COMMIT).
- Pour reprendre/abandonner une création/regénération d’index, utilisez la syntaxe T-SQL [ALTER INDEX](alter-index-transact-sql.md).

> [!NOTE]
> La commande DDL s’exécute tant qu’elle n’a pas fini, n’est pas mise en pause ou n’a pas échoué. Si la commande est en pause, une erreur s’affiche indiquant que l’opération a été mise en pause et que la création d’index n’a pas été effectuée. Vous trouverez plus d’informations sur l’état d’index actuel dans [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md). Comme avant, en cas de défaillance, une erreur s’affiche également.

Pour indiquer qu’une création d’index est exécutée sous la forme d’une opération pouvant être reprise et pour connaître son état d’exécution actuel, voir [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md).

#### <a name="resources"></a>Ressources
Les ressources suivantes sont requises pour l’opération de création d’index en ligne pouvant être reprise :

- De l’espace supplémentaire nécessaire pour conserver l’index en cours de construction, y compris le temps lorsque l’index est en pause
- Du débit de journal supplémentaire pendant la phase de tri. L’utilisation globale d’espace journal d’un index pouvant être repris est inférieure à celle de la création d’un index en ligne classique ; la troncature du journal est permise pendant cette opération.
- Un état DDL empêchant toute modification DDL
- Le nettoyage des éléments fantômes est bloqué sur l’index en génération pendant toute la durée de l’opération, qu’elle soit en pause ou en cours d’exécution.

#### <a name="current-functional-limitations"></a>Limitations fonctionnelles actuelles
Les fonctionnalités suivantes sont désactivées pour les opérations de création d’index pouvant être reprises :

- Une fois l’opération de création d’index en ligne pouvant être reprise mise en pause, la valeur initiale de MAXDOP n’est plus modifiable.
- Créez un index contenant :

  - la ou les colonnes calculées ou TIMESTAMP en tant que colonnes clés ;
  - la colonne LOB en tant que colonne incluse pour la création d’index pouvant être reprise.
  - Index filtré

## <a name="row-and-page-locks-options"></a>Options de verrous de ligne et de page
Lorsque `ALLOW_ROW_LOCKS = ON` et `ALLOW_PAGE_LOCK = ON`, les verrous au niveau de la ligne, de la page et de la table sont autorisés au moment où vous accédez à l’index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] choisit le verrou approprié et peut promouvoir un verrou de ligne ou de page en verrou de table.

Lorsque `ALLOW_ROW_LOCKS = OFF` et `ALLOW_PAGE_LOCK = OFF`, seul un verrou au niveau de la table est autorisé au moment où vous accédez à l’index.

## <a name="sequential-keys"></a>Clés séquentielles
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 

La contention de l’insertion de la dernière page est un problème de performances courant qui se produit lorsqu’un grand nombre de threads simultanés tentent d’insérer des lignes dans un index avec une clé séquentielle. Un index est considéré comme séquentiel lorsque la colonne clé principale contient des valeurs qui augmentent (ou diminuent) toujours, par exemple une colonne d’identité ou une date correspondant par défaut à la date/heure actuelle. Étant donné que les clés insérées sont séquentielles, toutes les nouvelles lignes seront insérées à la fin de la structure d’index - en d’autres termes, sur la même page. Cela conduit à une contention de la page en mémoire qui peut être observée comme plusieurs threads en attente sur PAGELATCH_EX de la page en question.

L’activation de l’option d’index OPTIMIZE_FOR_SEQUENTIAL_KEY permet une optimisation dans le moteur de base de données qui vous aide à améliorer le débit pour les insertions de haute concurrence dans l’index. Elle est conçue pour les index qui ont une clé séquentielle et sont susceptibles d’engendrer des contentions d’insertion de la dernière page, mais elle peut être également utile avec les index qui ont des zones réactives dans d’autres zones de la structure d’index B-Tree.

## <a name="viewing-index-information"></a>Affichage des informations sur les index
Pour retourner des informations sur les index, vous pouvez utiliser des affichages catalogue, des fonctions système et des procédures stockées système.

## <a name="data-compression"></a>Data Compression
La compression de données est décrite dans la rubrique [Compression de données](../../relational-databases/data-compression/data-compression.md). Voici les points clés à prendre en compte :

- La compression permet de stocker un plus grand nombre de lignes dans une page, mais ne modifie pas la taille maximale des lignes.
- Les pages non-feuille d'un index ne sont pas compressées par le biais de la compression de page, mais par le biais de la compression de ligne.
- Chaque index non cluster dispose d'un paramètre de compression individuel et n'hérite pas du paramètre de compression de la table sous-jacente.
- Lorsqu'un index cluster est créé sur un segment de mémoire, l'index cluster hérite de l'état de compression du segment, à moins qu'un autre état de compression soit spécifié.

Les restrictions suivantes s'appliquent aux index partitionnés :

- Vous ne pouvez pas modifier le paramètre de compression d'une partition unique si la table possède des index non alignés.
- L’instruction ALTER INDEX \<index> ... REBUILD PARTITION ... reconstruit la partition spécifiée de l’index.
- L’instruction ALTER INDEX \<index> ... REBUILD WITH ... reconstruit toutes les partitions de l’index.

Pour évaluer la façon dont la modification de l’état de compression affecte une table, un index ou une partition, utilisez la procédure stockée [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) .

## <a name="permissions"></a>Autorisations
Nécessite l’autorisation `ALTER` sur la table ou la vue. L’utilisateur doit être membre du rôle serveur fixe `sysadmin` ou des rôles de base de données fixes `db_ddladmin` ou `db_owner`.

## <a name="limitations-and-restrictions"></a>Limitations et restrictions
Dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], vous ne pouvez pas créer :

- Un index rowstore cluster ou non-cluster sur une table d’entrepôt de données quand il existe déjà un index columnstore. Ce comportement est différent de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SMP qui permet à des index rowstore et columnstore de coexister sur la même table.
- Vous ne pouvez pas créer un index sur une vue.

## <a name="metadata"></a>Métadonnées
Pour afficher des informations sur les index existants, vous pouvez interroger la vue de catalogue [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

## <a name="version-notes"></a>Notes de version

[!INCLUDE[ssSDS](../../includes/sssds-md.md)] ne prend pas en charge les options filegroup et filestream.

## <a name="examples-all-versions-uses-the-adventureworks-database"></a>Exemples : Toutes les versions. Utilise la base de données AdventureWorks

### <a name="a-create-a-simple-nonclustered-rowstore-index"></a>R. Créer un index rowstore non-cluster simple
Les exemples suivants créent un index non-cluster sur la colonne `VendorID` de la table `Purchasing.ProductVendor`.

```sql
CREATE INDEX IX_VendorID ON ProductVendor (VendorID);
CREATE INDEX IX_VendorID ON dbo.ProductVendor (VendorID DESC, Name ASC, Address DESC);
CREATE INDEX IX_VendorID ON Purchasing..ProductVendor (VendorID);
```

### <a name="b-create-a-simple-nonclustered-rowstore-composite-index"></a>B. Créer un index composite rowstore non-cluster simple
L’exemple suivant crée un index composite non-cluster sur les colonnes `SalesQuota` et `SalesYTD` de la table `Sales.SalesPerson`.

```sql
CREATE NONCLUSTERED INDEX IX_SalesPerson_SalesQuota_SalesYTD ON Sales.SalesPerson (SalesQuota, SalesYTD);
```

### <a name="c-create-an-index-on-a-table-in-another-database"></a>C. Créer un index sur une table dans une autre base de données
L’exemple suivant crée un index cluster sur la colonne `VendorID` de la table `ProductVendor` dans la base de données `Purchasing`.

```sql
CREATE CLUSTERED INDEX IX_ProductVendor_VendorID ON Purchasing..ProductVendor (VendorID);
```

### <a name="d-add-a-column-to-an-index"></a>D. Ajouter une colonne à un index
L’exemple suivant crée un index IX_FF avec deux colonnes à partir de la table dbo.FactFinance. L’instruction suivante reconstruit l’index avec une colonne supplémentaire et conserve le nom existant.

```sql
CREATE INDEX IX_FF ON dbo.FactFinance (FinanceKey ASC, DateKey ASC);

-- Rebuild and add the OrganizationKey
CREATE INDEX IX_FF ON dbo.FactFinance (FinanceKey, DateKey, OrganizationKey DESC)
  WITH (DROP_EXISTING = ON);
```

## <a name="examples-sql-server-azure-sql-database"></a>Exemples : SQL Server, Azure SQL Database

### <a name="e-create-a-unique-nonclustered-index"></a>E. Créer un index non-cluster unique
L'exemple suivant crée un index non cluster unique sur la colonne `Name` de la table `Production.UnitMeasure` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. L'index applique la contrainte d'unicité sur les données insérées dans la colonne `Name`.

```sql
CREATE UNIQUE INDEX AK_UnitMeasure_Name
  ON Production.UnitMeasure(Name);
```

La requête suivante teste la contrainte d'unicité en tentant d'insérer une ligne avec une valeur existant dans une autre ligne.

```sql
-- Verify the existing value.
SELECT Name FROM Production.UnitMeasure WHERE Name = N'Ounces';
GO

INSERT INTO Production.UnitMeasure (UnitMeasureCode, Name, ModifiedDate)
  VALUES ('OC', 'Ounces', GETDATE());
```

Le message d'erreur retourné est :

```
Server: Msg 2601, Level 14, State 1, Line 1
Cannot insert duplicate key row in object 'UnitMeasure' with unique index 'AK_UnitMeasure_Name'. The statement has been terminated.
```

### <a name="f-use-the-ignore_dup_key-option"></a>F. Utiliser l’option IGNORE_DUP_KEY
L'exemple suivant montre l'effet de l'option `IGNORE_DUP_KEY` en insérant plusieurs lignes dans une table temporaire avec cette option d'abord définie sur `ON`, puis sur `OFF`. Une ligne est insérée dans la table `#Test` pour créer intentionnellement une valeur dupliquée lorsque la deuxième instruction `INSERT`, qui va insérer plusieurs lignes, est exécutée. Un compteur de lignes de la table retourne le nombre de lignes insérées.

```sql
CREATE TABLE #Test (C1 NVARCHAR(10), C2 NVARCHAR(50), C3 DATETIME);
GO

CREATE UNIQUE INDEX AK_Index ON #Test (C2)
  WITH (IGNORE_DUP_KEY = ON);
GO

INSERT INTO #Test VALUES (N'OC', N'Ounces', GETDATE());
INSERT INTO #Test SELECT * FROM Production.UnitMeasure;
GO

SELECT COUNT(*) AS [Number of rows] FROM #Test;
GO

DROP TABLE #Test;
GO
```

Voici les résultats de la deuxième instruction `INSERT`.

```cmd
Server: Msg 3604, Level 16, State 1, Line 5 Duplicate key was ignored.

Number of rows
--------------
38
```

Notez que les lignes insérées depuis la table `Production.UnitMeasure` qui ne violent pas la contrainte d'unicité ont été correctement insérées. Un avertissement est émis, et la ligne dupliquée est ignorée ; l'ensemble de la transaction n'a pas été restauré.

Les mêmes instructions sont exécutées de nouveau, mais avec `IGNORE_DUP_KEY` défini sur `OFF`.

```sql
CREATE TABLE #Test (C1 NVARCHAR(10), C2 NVARCHAR(50), C3 DATETIME);
GO

CREATE UNIQUE INDEX AK_Index ON #Test (C2)
  WITH (IGNORE_DUP_KEY = OFF);
GO

INSERT INTO #Test VALUES (N'OC', N'Ounces', GETDATE());
INSERT INTO #Test SELECT * FROM Production.UnitMeasure;
GO

SELECT COUNT(*) AS [Number of rows] FROM #Test;
GO

DROP TABLE #Test;
GO
```

Voici les résultats de la deuxième instruction `INSERT`.

```
Server: Msg 2601, Level 14, State 1, Line 5
Cannot insert duplicate key row in object '#Test' with unique index
'AK_Index'. The statement has been terminated.

Number of rows
--------------
1
```

Notez qu'aucune des lignes de la table `Production.UnitMeasure` n'a été insérée dans la table, alors qu'une seule ligne de la table a violé la contrainte `UNIQUE` de l'index.

### <a name="g-using-drop_existing-to-drop-and-re-create-an-index"></a>G. Utilisation de DROP_EXISTING pour supprimer et recréer un index
L'exemple suivant supprime et recrée un index existant sur la colonne `ProductID` de la table `Production.WorkOrder` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] avec l'option `DROP_EXISTING`. Les options `FILLFACTOR` et `PAD_INDEX` sont également définies.

```sql
CREATE NONCLUSTERED INDEX IX_WorkOrder_ProductID
  ON Production.WorkOrder(ProductID)
    WITH (FILLFACTOR = 80,
      PAD_INDEX = ON,
      DROP_EXISTING = ON);
GO
```

### <a name="h-create-an-index-on-a-view"></a>H. Créer un index sur une vue
L'exemple suivant crée une vue et un index sur cette vue. Deux requêtes sont incluses, lesquelles utilisent la vue indexée.

```sql
-- Set the options to support indexed views
SET NUMERIC_ROUNDABORT OFF;
SET ANSI_PADDING, ANSI_WARNINGS, CONCAT_NULL_YIELDS_NULL, ARITHABORT,
  QUOTED_IDENTIFIER, ANSI_NULLS ON;
GO

-- Create view with schemabinding
IF OBJECT_ID ('Sales.vOrders', 'view') IS NOT NULL
  DROP VIEW Sales.vOrders;
GO

CREATE VIEW Sales.vOrders
  WITH SCHEMABINDING
AS
  SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Revenue,
    OrderDate, ProductID, COUNT_BIG(*) AS COUNT
  FROM Sales.SalesOrderDetail AS od, Sales.SalesOrderHeader AS o
  WHERE od.SalesOrderID = o.SalesOrderID
  GROUP BY OrderDate, ProductID;
GO

-- Create an index on the view
CREATE UNIQUE CLUSTERED INDEX IDX_V1
  ON Sales.vOrders (OrderDate, ProductID);
GO

-- This query can use the indexed view even though the view is
-- not specified in the FROM clause.
SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev,
  OrderDate, ProductID
FROM Sales.SalesOrderDetail AS od
  JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID = o.SalesOrderID
    AND ProductID BETWEEN 700 AND 800
    AND OrderDate >= CONVERT(DATETIME, '05/01/2002', 101)
GROUP BY OrderDate, ProductID
ORDER BY Rev DESC;
GO

-- This query can use the above indexed view
SELECT OrderDate, SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev
FROM Sales.SalesOrderDetail AS od
  JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID = o.SalesOrderID
    AND DATEPART(mm, OrderDate) = 3
  AND DATEPART(yy, OrderDate) = 2002
GROUP BY OrderDate
ORDER BY OrderDate ASC;
GO
```

### <a name="i-create-an-index-with-included-non-key-columns"></a>I. Créer un index avec des colonnes (non-clés) incluses
L'exemple suivant crée un index non cluster avec une colonne clé (`PostalCode`) et quatre colonnes non clés (`AddressLine1`, `AddressLine2`, `City`, `StateProvinceID`). Une requête couverte par l'index suit. Pour afficher l’index sélectionné par l’optimiseur de requête, dans le menu **Requête** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], sélectionnez **Inclure le plan d’exécution réel** avant d’exécuter la requête.

```sql
CREATE NONCLUSTERED INDEX IX_Address_PostalCode
  ON Person.Address (PostalCode)
  INCLUDE (AddressLine1, AddressLine2, City, StateProvinceID);
GO

SELECT AddressLine1, AddressLine2, City, StateProvinceID, PostalCode
FROM Person.Address
WHERE PostalCode BETWEEN N'98000' and N'99999';
GO
```

### <a name="j-create-a-partitioned-index"></a>J. Créer un index partitionné
L'exemple suivant crée un index partitionné non cluster sur `TransactionsPS1`, un schéma de partition existant dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Cet exemple suppose que l'exemple d'index partitionné a été installé.

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
CREATE NONCLUSTERED INDEX IX_TransactionHistory_ReferenceOrderID
  ON Production.TransactionHistory (ReferenceOrderID)
  ON TransactionsPS1 (TransactionDate);
GO
```

### <a name="k-creating-a-filtered-index"></a>K. Création d'un index filtré
L'exemple suivant crée un index filtré sur la table Production.BillOfMaterials dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Le prédicat de filtre peut inclure des colonnes qui ne sont pas des colonnes clés dans l'index filtré. Dans cet exemple, le prédicat sélectionne uniquement les lignes où EndDate n'est pas NULL.

```sql
CREATE NONCLUSTERED INDEX "FIBillOfMaterialsWithEndDate"
  ON Production.BillOfMaterials (ComponentID, StartDate)
  WHERE EndDate IS NOT NULL;
```

### <a name="l-create-a-compressed-index"></a>L. Créer un index compressé
L'exemple ci-dessous illustre la création d'un index sur une table non partitionnée à l'aide de la compression de ligne.

```sql
CREATE NONCLUSTERED INDEX IX_INDEX_1
  ON T1 (C2)
  WITH (DATA_COMPRESSION = ROW);
GO
```

L'exemple ci-dessous illustre la création d'un index sur une table partitionnée à l'aide de la compression de ligne sur toutes les partitions de l'index.

```sql
CREATE CLUSTERED INDEX IX_PartTab2Col1
  ON PartitionTable1 (Col1)
  WITH (DATA_COMPRESSION = ROW);
GO
```

 L'exemple ci-dessous illustre la création d'un index sur une table partitionnée à l'aide de la compression de page sur la partition `1` de l'index et à l'aide de la compression de ligne sur les partitions `2` à `4` de l'index.

```sql
CREATE CLUSTERED INDEX IX_PartTab2Col1
  ON PartitionTable1 (Col1)
  WITH (
    DATA_COMPRESSION = PAGE ON PARTITIONS(1),
    DATA_COMPRESSION = ROW ON PARTITIONS (2 TO 4)
  );
GO
```

### <a name="m-create-resume-pause-and-abort-resumable-index-operations"></a>M. Créer, reprendre, suspendre et abandonner des opérations d’index pouvant être reprises
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
-- Execute a resumable online index create statement with MAXDOP=1
CREATE INDEX test_idx1 ON test_table (col1) WITH (ONLINE = ON, MAXDOP = 1, RESUMABLE = ON);

-- Executing the same command again (see above) after an index operation was paused, resumes automatically the index create operation.

-- Execute a resumable online index creates operation with MAX_DURATION set to 240 minutes. After the time expires, the resumable index create operation is paused.
CREATE INDEX test_idx2 ON test_table (col2) WITH (ONLINE = ON, RESUMABLE = ON, MAX_DURATION = 240);

-- Pause a running resumable online index creation
ALTER INDEX test_idx1 ON test_table PAUSE;
ALTER INDEX test_idx2 ON test_table PAUSE;

-- Resume a paused online index creation
ALTER INDEX test_idx1 ON test_table RESUME;
ALTER INDEX test_idx2 ON test_table RESUME;

-- Abort resumable index create operation which is running or paused
ALTER INDEX test_idx1 ON test_table ABORT;
ALTER INDEX test_idx2 ON test_table ABORT;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="n-basic-syntax"></a>N. Syntaxe de base
Créer, reprendre, suspendre et abandonner des opérations d’index pouvant être reprises       

**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
-- Execute a resumable online index create statement with MAXDOP=1
CREATE INDEX test_idx ON test_table WITH (ONLINE = ON, MAXDOP = 1, RESUMABLE = ON);

-- Executing the same command again (see above) after an index operation was paused, resumes automatically the index create operation.

-- Execute a resumable online index creates operation with MAX_DURATION set to 240 minutes. After the time expires, the resumable index create operation is paused.
CREATE INDEX test_idx ON test_table WITH (ONLINE = ON, RESUMABLE = ON, MAX_DURATION = 240);

-- Pause a running resumable online index creation
ALTER INDEX test_idx ON test_table PAUSE;

-- Resume a paused online index creation
ALTER INDEX test_idx ON test_table RESUME;

-- Abort resumable index create operation which is running or paused
ALTER INDEX test_idx ON test_table ABORT;
```

### <a name="o-create-a-nonclustered-index-on-a-table-in-the-current-database"></a>O. Créer un index non-cluster sur une table de la base de données active
L’exemple suivant crée un index non-cluster sur la colonne `VendorID` de la table `ProductVendor`.

```sql
CREATE INDEX IX_ProductVendor_VendorID
  ON ProductVendor (VendorID);
```

### <a name="p-create-a-clustered-index-on-a-table-in-another-database"></a>P. Créer un index cluster sur une table dans une autre base de données
L’exemple suivant crée un index non-cluster sur la colonne `VendorID` de la table `ProductVendor` dans la base de données `Purchasing`.

```sql
CREATE CLUSTERED INDEX IX_ProductVendor_VendorID
  ON Purchasing..ProductVendor (VendorID);
```
### <a name="q-create-an-ordered-clustered-index-on-a-table"></a>Q. Créer un index cluster ordonné sur une table  
L’exemple suivant crée un index composite cluster ordonné sur les colonnes `c1` et `c2` de la table `T1` dans la base de données `MyDB`.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX MyOrderedCCI ON MyDB.dbo.T1 
ORDER (c1, c2);

```

### <a name="r-convert-a-cci-to-an-ordered-clustered-index-on-a-table"></a>R. Convertir un index columnstore cluster en index cluster ordonné sur une table  
L’exemple suivant convertit l’index columnstore cluster existant en index columnstore cluster ordonné appelé `MyOrderedCCI` sur les colonnes `c1` et `c2` de la table `T2` dans la base de données `MyDB`.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX MyOrderedCCI ON MyDB.dbo.T2
ORDER (c1, c2)
WITH (DROP_EXISTING = ON);

```

## <a name="see-also"></a>Voir aussi
[Guide de conception et d’architecture d’index SQL Server](../../relational-databases/sql-server-index-design-guide.md)     
[Exécuter des opérations en ligne sur les index](../../relational-databases/indexes/perform-index-operations-online.md)  
[Index et ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md#indexes-and-alter-table)     
[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)     
[CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md)     
[CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md)     
[CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)      
[CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md)     
[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)    
[CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md)     
[Types de données](../../t-sql/data-types/data-types-transact-sql.md)    
[DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)    
[DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)    
[Index XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)     
[sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)     
[sys.index_columns](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)    
[sys.xml_indexes](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)     
[EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)