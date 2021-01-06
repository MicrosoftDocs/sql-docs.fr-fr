---
description: CREATE EXTERNAL TABLE (Transact-SQL)
title: CREATE EXTERNAL TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/27/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_EXTERNAL_TABLE
- CREATE EXTERNAL TABLE
- PolyBase, T-SQL
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, table create
- PolyBase, external table
ms.assetid: 6a6fd8fe-73f5-4639-9908-2279031abdec
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49eb63fb4e90f20a7c63778efdd017b7ca32439a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643147"
---
# <a name="create-external-table-transact-sql"></a>CREATE EXTERNAL TABLE (Transact-SQL)

Crée une table externe

Cet article fournit la syntaxe, les arguments, les notes, les autorisations et des exemples associés au produit SQL que vous choisissez.

Pour plus d’informations sur les conventions de la syntaxe, consultez [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Base de données SQL](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-server"></a>Présentation : SQL Server

Cette commande crée une table externe pour PolyBase, afin d’accéder à des données stockées dans un cluster Hadoop, dans un stockage d’objets blob Azure ou dans une table externe PolyBase qui fait référence à des données stockées dans ces derniers.

**S’APPLIQUE À** : SQL Server 2016 (ou ultérieur)

Utilisez une table externe avec une source de données externe pour les requêtes PolyBase. Des sources de données externes sont utilisées pour établir la connectivité et prendre en charge ces principaux cas d’utilisation :

- Virtualisation des données et chargement des données à l’aide de [PolyBase](../../relational-databases/polybase/polybase-guide.md)
- Opérations de chargement en masse à l’aide de SQL Server ou SQL Database avec `BULK INSERT` ou `OPENROWSET`

Voir aussi [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) et [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Create a new external table
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )
    WITH (
        LOCATION = 'folder_or_filepath',
        DATA_SOURCE = external_data_source_name,
        FILE_FORMAT = external_file_format_name
        [ , <reject_options> [ ,...n ] ]
    )
[;]

<reject_options> ::=
{
    | REJECT_TYPE = value | percentage
    | REJECT_VALUE = reject_value
    | REJECT_SAMPLE_VALUE = reject_sample_value
}

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
```

## <a name="arguments"></a>Arguments

*{database_name.schema_name.table_name | nom_schéma.nom_table | nom_table}*  Nom (composé d’une à trois parties) de la table à créer. Pour une table externe, SQL stocke uniquement les métadonnées de la table avec des statistiques de base sur le fichier ou le dossier qui est référencé dans Hadoop ou le Stockage Blob Azure. Aucune donnée n’est déplacée ni stockée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!IMPORTANT]
> Pour des performances optimales, si le pilote de source de données externe prend en charge un nom en trois parties, il est fortement recommandé de fournir ce nom en trois parties.  

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE prend en charge la possibilité de configurer le nom de colonne, le type de données, la possibilité d’une valeur Null et le classement. Vous ne pouvez pas utiliser DEFAULT CONSTRAINT sur des tables externes.

Les définitions de colonne, notamment les types de données et le nombre de colonnes, doivent correspondre aux données des fichiers externes. En cas de non-correspondance, les lignes du fichier sont rejetées lors de l’interrogation des données réelles.

LOCATION = '*folder_or_filepath*' Spécifie le dossier, ou le chemin et le nom du fichier, où se trouvent les données Hadoop ou Azure Blob Storage. L’emplacement commence au dossier racine. Le dossier racine est l’emplacement de données qui est spécifié dans la source de données externe.

Dans SQL Server, l’instruction CREATE EXTERNAL TABLE crée le chemin et le dossier s’ils n’existent pas déjà. Vous pouvez ensuite utiliser INSERT INTO pour exporter les données d’une table SQL Server locale dans la source de données externe. Pour plus d’informations, consultez [Requêtes PolyBase](../../relational-databases/polybase/polybase-queries.md).

Si vous spécifiez LOCATION comme étant un dossier, une requête PolyBase qui sélectionne des données dans la table externe récupère les fichiers dans le dossier et dans tous ses sous-dossiers. Tout comme Hadoop, PolyBase ne retourne pas les dossiers masqués. Il ne retourne pas non plus les fichiers dont le nom commence par un trait de soulignement (_) ou un point (.).

Dans cet exemple, si LOCATION='/webdata/', une requête PolyBase retourne des lignes à partir de mydata.txt et mydata2.txt. Il ne retourne pas mydata3.txt, car il s’agit d’un fichier qui se trouve dans un dossier masqué. Il ne retourne pas non plus _hidden.txt car il s’agit d’un fichier masqué.

![Données récursives pour les tables externes](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Données récursives pour les tables externes")

Pour modifier la valeur par défaut et uniquement lire les données du dossier racine, définissez l’attribut \<polybase.recursive.traversal>sur 'false' dans le fichier de configuration core-site.xml. Ce fichier se trouve sous `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Par exemple : `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *external_data_source_name* Spécifie le nom de la source de données externe qui contient l’emplacement des données externes. Cet emplacement est un système de fichiers Hadoop (HDFS), un conteneur d’objets blob de stockage Azure ou Azure Data Lake Store. Pour créer une source de données externe, utilisez [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *external_file_format_name* Spécifie le nom de l’objet de format de fichier externe qui stocke le type de fichier et la méthode de compression des données externes. Pour créer un format de fichier externe, utilisez [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Options REJECT Vous pouvez spécifier les paramètres REJECT qui déterminent la façon dont PolyBase traite les enregistrements *incorrects* qu’il récupère à partir de la source de données externe. Un enregistrement de données est considéré comme « incorrect » si les types de données ou le nombre de colonnes ne correspondent pas aux définitions de colonnes de la table externe.

Si vous ne spécifiez pas ou ne changez pas les valeurs REJECT, PolyBase utilise les valeurs par défaut. Ces informations sur les paramètres REJECT sont stockées en tant que métadonnées supplémentaires lorsque vous créez une table externe avec l’instruction CREATE EXTERNAL TABLE. Quand une prochaine instruction SELECT ou SELECT INTO SELECT sélectionne des données dans la table externe, PolyBase utilise les options REJECT pour déterminer le nombre ou le pourcentage de lignes pouvant être rejetées avant de provoquer l’échec de la requête. La requête retourne des résultats (partiels) jusqu’à ce que le seuil de rejet soit dépassé. Ensuite, elle échoue avec le message d’erreur correspondant.

REJECT_TYPE = **value** | percentage Précise si l’option REJECT_VALUE est spécifiée comme une valeur littérale ou un pourcentage.

La valeur REJECT_VALUE est une valeur littérale, et non un pourcentage. La requête PolyBase échoue lorsque le nombre de lignes rejetées dépasse la valeur *reject_value*.

Par exemple, si REJECT_VALUE = 5 et REJECT_TYPE = value, la requête PolyBase SELECT échoue après le rejet de cinq lignes.

Le pourcentage REJECT_VALUE est un pourcentage, et non une valeur littérale. Une requête PolyBase échoue lorsque le *pourcentage* de lignes ayant échoué dépasse la valeur *reject_value*. Le pourcentage de lignes ayant échoué est calculé à intervalles.

REJECT_VALUE = *reject_value* Spécifie la valeur ou le pourcentage de lignes pouvant être rejetées avant de provoquer l’échec de la requête.

Pour REJECT_TYPE = value, *reject_value* doit être un entier compris entre 0 et 2 147 483 647.

Pour REJECT_TYPE = percentage, *reject_value* doit être une valeur float comprise entre 0 et 100.

REJECT_SAMPLE_VALUE = *reject_sample_value* Cet attribut est nécessaire lorsque vous spécifiez REJECT_TYPE = percentage. Il détermine le nombre de lignes à tenter de récupérer avant que PolyBase ne recalcule le pourcentage de lignes rejetées.

Le paramètre *reject_sample_value* doit être un entier compris entre 0 et 2 147 483 647.

Par exemple, si REJECT_SAMPLE_VALUE = 1000, PolyBase calcule le pourcentage de lignes ayant échoué après avoir tenté d’importer 1000 lignes à partir du fichier de données externe. Si le pourcentage de lignes ayant échoué est inférieur à la valeur de *reject_value*, PolyBase tente de récupérer 1000 autres lignes. Il continue de recalculer le pourcentage de lignes ayant échoué après avoir tenté d’importer chacune des 1000 lignes supplémentaires.

> [!NOTE]
> Étant donné que PolyBase calcule le pourcentage de lignes ayant échoué à intervalles, le pourcentage de lignes ayant échoué peut dépasser la valeur de *reject_value*.

Exemple :

Cet exemple montre comment les trois options REJECT interagissent les unes avec les autres. Par exemple, si REJECT_TYPE = percentage, REJECT_VALUE = 30 et REJECT_SAMPLE_VALUE = 100, le scénario suivant peut se produire :

- PolyBase tente de récupérer les 100 premières lignes : la récupération échoue pour 25 d’entre elles, et réussit pour les 75 autres.
- Le pourcentage de lignes ayant échoué qui est obtenu est de 25 %, ce qui est inférieur à la valeur de rejet de 30 %. Par conséquent, PolyBase va continuer de récupérer les données à partir de la source de données externe.
- PolyBase tente de charger les 100 lignes suivantes. Cette fois-ci, le chargement réussit pour 25 lignes et échoue pour les 75 autres.
- Le pourcentage de lignes ayant échoué est recalculé et on obtient 50 %. Le pourcentage de lignes ayant échoué a donc dépassé la valeur de rejet de 30 %.
- La requête PolyBase échoue après le rejet de 50 % des 200 premières lignes qu’elle a tenté de retourner. Notez que les lignes correspondantes sont retournées avant que la requête PolyBase ne détecte que le seuil de rejet a été dépassé.

SCHEMA_NAME La clause SCHEMA_NAME offre la possibilité de mapper la définition de table externe sur une table d’un autre schéma dans la base de données distante. Cette clause permet de lever l’ambiguïté pour les schémas qui existent à la fois sur des bases de données locales et des bases de données distantes.

OBJECT_NAME La clause OBJECT_NAME offre la possibilité de mapper la définition de table externe sur une table portant un nom différent dans la base de données distante. Cette clause permet de lever l’ambiguïté pour les noms d’objets qui existent à la fois sur des bases de données locales et des bases de données distantes.

DISTRIBUTION Ce paramètre est facultatif. Cet argument est obligatoire uniquement pour les bases de données de type SHARD_MAP_MANAGER. Cet argument contrôle si une table est traitée comme une table shardée ou une table répliquée. Avec les tables **SHARDED** (*nom de colonne*), les données des différentes tables ne se chevauchent pas. **REPLICATED** spécifie que les tables contiennent les mêmes données sur chaque partition. **ROUND_ROBIN** indique qu’une méthode spécifique à l’application est utilisée pour distribuer les données.

## <a name="permissions"></a>Autorisations

Nécessite les autorisations utilisateur suivantes :

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- **CONTROL DATABASE**

Notez que la connexion qui crée la source de données externe doit être autorisée à lire et à écrire dans la source de données externe, qui est située dans Hadoop ou Azure Blob Storage.

> [!IMPORTANT]
> L’autorisation ALTER ANY EXTERNAL DATA SOURCE accorde à n’importe quel principal la possibilité de créer et de modifier tout objet de source de données externe. Par conséquent, elle permet également d’accéder à toutes les informations d’identification délimitées à la base de données. Cette autorisation doit être considérée comme fournissant des privilèges très élevés, et doit donc être accordée uniquement aux principaux de confiance du système.

## <a name="error-handling"></a>Gestion des erreurs

Lors de l’exécution de l’instruction CREATE EXTERNAL TABLE, PolyBase tente de se connecter à la source de données externe. Si la tentative de connexion échoue, l’instruction échoue et la table externe n’est pas créée. L’échec de la commande peut prendre plusieurs minutes, car PolyBase tente plusieurs connexions avant que la requête n’échoue.

## <a name="general-remarks"></a>Remarques d'ordre général

Dans les scénarios de requête ad hoc, comme avec SELECT FROM EXTERNAL TABLE, PolyBase stocke les lignes qui sont extraites de la source de données externe dans une table temporaire. Une fois la requête terminée, PolyBase supprime la table temporaire. Aucune donnée permanente n’est stockée dans les tables SQL.

En revanche, dans le scénario d’importation, comme avec SELECT INTO FROM EXTERNAL TABLE, PolyBase stocke les lignes qui sont extraites de la source de données externe sous forme de données permanentes dans la table SQL. La nouvelle table est créée lors de l’exécution de la requête, au moment où PolyBase récupère les données externes.

PolyBase peut envoyer (push) une partie du calcul des requêtes vers Hadoop pour améliorer les performances des requêtes. Cette action est appelée « poussée de prédicats ». Pour l’activer, spécifiez l’option d’emplacement du Gestionnaire de ressources Hadoop dans [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

Vous pouvez créer plusieurs tables externes qui référencent les mêmes sources de données externes ou des sources différentes.

## <a name="limitations-and-restrictions"></a>Limitations et restrictions

Dans la mesure où les données d’une table externe ne sont pas sous le contrôle de gestion direct de SQL Server, elles peuvent être changées ou supprimées à tout moment par un processus externe. Par conséquent, il n’est pas garanti que les résultats d’une requête exécutée sur une table externe soient déterministes. La même requête peut retourner des résultats différents chaque fois qu’elle est exécutée sur une table externe. De même, une requête peut échouer si les données externes sont déplacées ou supprimées.

Vous pouvez créer plusieurs tables externes qui référencent chacune des sources de données externes différentes. Si vous exécutez simultanément plusieurs requêtes sur des sources de données Hadoop différentes, chaque source Hadoop doit utiliser le même paramètre de configuration de serveur « hadoop connectivity » (connectivité Hadoop). Par exemple, vous ne pouvez pas exécuter simultanément une requête sur un cluster Hadoop Cloudera et sur un cluster Hadoop Hortonworks, puisqu’ils utilisent des paramètres de configuration différents. Pour connaître les paramètres de configuration et les combinaisons prises en charge, consultez [Configuration de la connectivité PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

Seules les instructions DDL suivantes sont autorisées avec les tables externes :

- CREATE TABLE et DROP TABLE
- CREATE STATISTICS et DROP STATISTICS
- CREATE VIEW et DROP VIEW

Les constructions et les opérations suivantes ne sont pas prises en charge :

- La contrainte DEFAULT sur les colonnes de table externe
- Les opérations DML delete, insert et update

### <a name="query-limitations"></a>Limitations des requêtes

PolyBase peut consommer un maximum de 33 000 fichiers par dossier lors de l’exécution simultanée de 32 requêtes PolyBase. Ce nombre maximal inclut les fichiers et les sous-dossiers de chaque dossier HDFS. Si le degré de concurrence est inférieur à 32, un utilisateur peut exécuter des requêtes PolyBase sur des dossiers dans des systèmes HDFS contenant plus de 33 000 fichiers. Il est recommandé de raccourcir au maximum les chemins de fichiers externes et de ne pas utiliser plus de 30 000 fichiers par dossier HDFS. Lorsque trop de fichiers sont référencés, une exception d’insuffisance de mémoire Java Virtual Machine (JVM) peut être levée.

### <a name="table-width-limitations"></a>Limitations concernant la largeur des tables

Dans SQL Server 2016, PolyBase a une limite de largeur de ligne de 32 Ko, basée sur la taille maximale d’une ligne valide par définition de table. Si la somme du schéma de colonne est supérieure à 32 Ko, PolyBase ne peut pas interroger les données.

### <a name="data-type-limitations"></a>Limitations des types de données

Les types de données suivants ne peuvent pas être utilisés dans des tables externes Polybase :

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Tout type défini par l’utilisateur

## <a name="locking"></a>Verrouillage

Verrou partagé sur l’objet SCHEMARESOLUTION.

## <a name="security"></a>Sécurité

Les fichiers de données d’une table externe sont stockés dans Hadoop ou le Stockage Blob Azure. Ces fichiers de données sont créés et gérés par vos propres processus. Il vous incombe de gérer la sécurité des données externes.

## <a name="examples"></a>Exemples

### <a name="a-create-an-external-table-with-data-in-text-delimited-format"></a>R. Créer une table externe avec des données au format texte délimité

Cet exemple montre toutes les étapes nécessaires à la création d’une table externe dont les données sont des fichiers de texte délimité. Il définit la source de données externe *mydatasource* et le format de fichier externe *myfileformat*. Ces objets de niveau base de données sont ensuite référencés dans l’instruction CREATE EXTERNAL TABLE. Pour plus d’informations, consultez [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) et [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat
WITH (
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS (FIELD_TERMINATOR ='|')
);

CREATE EXTERNAL TABLE ClickStream (
    url varchar(50),
    event_date date,
    user_IP varchar(50)
)
WITH (
        LOCATION='/webdata/employee.tbl',
        DATA_SOURCE = mydatasource,
        FILE_FORMAT = myfileformat
    )
;
```

### <a name="b-create-an-external-table-with-data-in-rcfile-format"></a>B. Créer une table externe avec des données au format RCFile

Cet exemple montre toutes les étapes nécessaires à la création d’une table externe contenant des données au format RCFile. Il définit la source de données externe *mydatasource_rc* et le format de fichier externe *myfileformat_rc*. Ces objets de niveau base de données sont ensuite référencés dans l’instruction CREATE EXTERNAL TABLE. Pour plus d’informations, consultez [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) et [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource_rc
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat_rc
WITH (
    FORMAT_TYPE = RCFILE,
    SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'
)
;

CREATE EXTERNAL TABLE ClickStream_rc (
    url varchar(50),
    event_date date,
    user_ip varchar(50)
)
WITH (
        LOCATION='/webdata/employee_rc.tbl',
        DATA_SOURCE = mydatasource_rc,
        FILE_FORMAT = myfileformat_rc
    )
;
```

### <a name="c-create-an-external-table-with-data-in-orc-format"></a>C. Créer une table externe avec des données au format ORC

Cet exemple montre toutes les étapes nécessaires à la création d’une table externe contenant des données au format ORC. Il définit la source de données externe mydatasource_orc et le format de fichier externe myfileformat_orc. Ces objets de niveau base de données sont ensuite référencés dans l’instruction CREATE EXTERNAL TABLE. Pour plus d’informations, consultez [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) et [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource_orc
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat_orc
WITH (
    FORMAT = ORC,
    COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
)
;

CREATE EXTERNAL TABLE ClickStream_orc (
    url varchar(50),
    event_date date,
    user_ip varchar(50)
)
WITH (
        LOCATION='/webdata/',
        DATA_SOURCE = mydatasource_orc,
        FILE_FORMAT = myfileformat_orc
    )
;
```

### <a name="d-querying-hadoop-data"></a>D. Interrogation de données Hadoop

Clickstream est une table externe qui se connecte au fichier texte délimité employee.tbl dans un cluster Hadoop. La requête suivante ressemble à une requête exécutée sur une table standard. Toutefois, cette requête récupère les données dans Hadoop, puis calcule les résultats.

```sql
SELECT TOP 10 (url) FROM ClickStream WHERE user_ip = 'xxx.xxx.xxx.xxx'
;
```

### <a name="e-join-hadoop-data-with-sql-data"></a>E. Joindre des données Hadoop à des données SQL

Cette requête ressemble à une requête JOIN standard exécutée sur deux tables SQL. La différence est que PolyBase récupère les données Clickstream dans Hadoop, puis les joint à la table UrlDescription. L’une des tables est une table externe, et l’autre est une table SQL standard.

```sql
SELECT url.description
FROM ClickStream cs
JOIN UrlDescription url ON cs.url = url.name
WHERE cs.url = 'msdn.microsoft.com'
;
```

### <a name="f-import-data-from-hadoop-into-a-sql-table"></a>F. Importer des données Hadoop dans une table SQL

Cet exemple crée la table ms_user SQL qui stocke de façon permanente le résultat d’une jointure entre la table SQL standard *user* et la table externe *ClickStream*.

```sql
SELECT DISTINCT user.FirstName, user.LastName
INTO ms_user
FROM user INNER JOIN (
    SELECT * FROM ClickStream WHERE cs.url = 'www.microsoft.com'
    ) AS ms
ON user.user_ip = ms.user_ip
;
```

### <a name="g-create-an-external-table-for-a-sharded-data-source"></a>G. Créer une table externe pour une source de données partitionnée

Cet exemple remappe une vue de gestion dynamique à distance vers une table externe à l’aide des clauses SCHEMA_NAME et OBJECT_NAME.

```sql
CREATE EXTERNAL TABLE [dbo].[all_dm_exec_requests]([session_id] smallint NOT NULL,
  [request_id] int NOT NULL,
  [start_time] datetime NOT NULL,
  [status] nvarchar(30) NOT NULL,
  [command] nvarchar(32) NOT NULL,
  [sql_handle] varbinary(64),
  [statement_start_offset] int,
  [statement_end_offset] int,
  [cpu_time] int NOT NULL)
WITH
(
  DATA_SOURCE = MyExtSrc,
  SCHEMA_NAME = 'sys',
  OBJECT_NAME = 'dm_exec_requests',
  DISTRIBUTION=ROUND_ROBIN
);
```

### <a name="h-create-an-external-table-for-sql-server"></a>H. Créer une table externe pour SQL Server

```sql
     -- Create a Master Key
      CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';
    GO
     /*  specify credentials to external data source
     *  IDENTITY: user name for external source.
     *  SECRET: password for external source.
     */
     CREATE DATABASE SCOPED CREDENTIAL SqlServerCredentials
     WITH IDENTITY = 'username', Secret = 'password';
    GO

    /* LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE SQLServerInstance
    WITH (
    LOCATION = 'sqlserver://SqlServer',
    -- PUSHDOWN = ON | OFF,
      CREDENTIAL = SQLServerCredentials
    );
    GO

    CREATE SCHEMA sqlserver;
    GO

     /* LOCATION: sql server table/view in 'database_name.schema_name.object_name' format
     * DATA_SOURCE: the external data source, created above.
     */
     CREATE EXTERNAL TABLE sqlserver.customer(
     C_CUSTKEY INT NOT NULL,
     C_NAME VARCHAR(25) NOT NULL,
     C_ADDRESS VARCHAR(40) NOT NULL,
     C_NATIONKEY INT NOT NULL,
     C_PHONE CHAR(15) NOT NULL,
     C_ACCTBAL DECIMAL(15,2) NOT NULL,
     C_MKTSEGMENT CHAR(10) NOT NULL,
     C_COMMENT VARCHAR(117) NOT NULL
      )
      WITH (
      LOCATION='tpch_10.dbo.customer',
      DATA_SOURCE=SqlServerInstance
     );
```

### <a name="i-create-an-external-table-for-oracle"></a>I. Créer une table externe pour Oracle

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

   /*
   * LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
   * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
   * CONNECTION_OPTIONS: Specify driver location
   * CREDENTIAL: the database scoped credential, created above.
   */
   CREATE EXTERNAL DATA SOURCE external_data_source_name
   WITH (
     LOCATION = 'oracle://<server address>[:<port>]',
     -- PUSHDOWN = ON | OFF,
     CREDENTIAL = credential_name)

   /*
   * LOCATION: Oracle table/view in '.<schema_name>.<object_name>' format
   * DATA_SOURCE: the external data source, created above.
   */
   CREATE EXTERNAL TABLE customers(
   [O_ORDERKEY] DECIMAL(38) NOT NULL,
   [O_CUSTKEY] DECIMAL(38) NOT NULL,
   [O_ORDERSTATUS] CHAR COLLATE Latin1_General_BIN NOT NULL,
   [O_TOTALPRICE] DECIMAL(15,2) NOT NULL,
   [O_ORDERDATE] DATETIME2(0) NOT NULL,
   [O_ORDERPRIORITY] CHAR(15) COLLATE Latin1_General_BIN NOT NULL,
   [O_CLERK] CHAR(15) COLLATE Latin1_General_BIN NOT NULL,
   [O_SHIPPRIORITY] DECIMAL(38) NOT NULL,
   [O_COMMENT] VARCHAR(79) COLLATE Latin1_General_BIN NOT NULL
   )
   WITH (
    LOCATION='.mySchema.customer',
    DATA_SOURCE= external_data_source_name
   );
```

### <a name="j-create-an-external-table-for-teradata"></a>J. Créer une table externe pour Teradata

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';

   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

    /* LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (
    LOCATION = teradata://<server address>[:<port>],
   -- PUSHDOWN = ON | OFF,
    CREDENTIAL =credential_name
    );


     /* LOCATION: Teradata table/view in '<database_name>.<object_name>' format
      * DATA_SOURCE: the external data source, created above.
      */
     CREATE EXTERNAL TABLE customer(
      L_ORDERKEY INT NOT NULL,
      L_PARTKEY INT NOT NULL,
     L_SUPPKEY INT NOT NULL,
     L_LINENUMBER INT NOT NULL,
     L_QUANTITY DECIMAL(15,2) NOT NULL,
     L_EXTENDEDPRICE DECIMAL(15,2) NOT NULL,
     L_DISCOUNT DECIMAL(15,2) NOT NULL,
     L_TAX DECIMAL(15,2) NOT NULL,
     L_RETURNFLAG CHAR NOT NULL,
     L_LINESTATUS CHAR NOT NULL,
     L_SHIPDATE DATE NOT NULL,
     L_COMMITDATE DATE NOT NULL,
     L_RECEIPTDATE DATE NOT NULL,
     L_SHIPINSTRUCT CHAR(25) NOT NULL,
     L_SHIPMODE CHAR(10) NOT NULL,
     L_COMMENT VARCHAR(44) NOT NULL
     )
     WITH (
     LOCATION='customer',
     DATA_SOURCE= external_data_source_name
     );
```

### <a name="k-create-an-external-table-for-mongodb"></a>K. Créer une table externe pour MongoDB

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';

   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

     /* LOCATION: Location string should be of format '<type>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (
    LOCATION = mongodb://<server>[:<port>],
    -- PUSHDOWN = ON | OFF,
      CREDENTIAL = credential_name
    );

     /* LOCATION: MongoDB table/view in '<database_name>.<schema_name>.<object_name>' format
     * DATA_SOURCE: the external data source, created above.
     */
     CREATE EXTERNAL TABLE customers(
     [O_ORDERKEY] DECIMAL(38) NOT NULL,
     [O_CUSTKEY] DECIMAL(38) NOT NULL,
     [O_ORDERSTATUS] CHAR COLLATE Latin1_General_BIN NOT NULL,
     [O_TOTALPRICE] DECIMAL(15,2) NOT NULL,
     [O_ORDERDATE] DATETIME2(0) NOT NULL,
     [O_COMMENT] VARCHAR(79) COLLATE Latin1_General_BIN NOT NULL
     )
     WITH (
     LOCATION='customer',
     DATA_SOURCE= external_data_source_name
     );
```

## <a name="see-also"></a>Voir aussi

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Database \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-sql-database"></a>Présentation : Azure SQL Database

Dans Azure SQL Database, une table externe est créée pour les [requêtes élastiques (en préversion)](/azure/sql-database/sql-database-elastic-query-overview/).


Voir également [CRÉER UNE SOURCE DE DONNÉES EXTERNES](../../t-sql/statements/create-external-data-source-transact-sql.md).

## <a name="syntax"></a>Syntaxe

```syntaxsql
-- Create a table for use with elastic query  
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH ( <sharded_external_table_options> )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
  
<sharded_external_table_options> ::=  
        DATA_SOURCE = external_data_source_name,
        SCHEMA_NAME = N'nonescaped_schema_name',  
        OBJECT_NAME = N'nonescaped_object_name',  
        [DISTRIBUTION  = SHARDED(sharding_column_name) | REPLICATED | ROUND_ROBIN]]  
    )  
[;]  
```

## <a name="arguments"></a>Arguments

*{database_name.schema_name.table_name | nom_schéma.nom_table | nom_table}*  Nom (composé d’une à trois parties) de la table à créer. Pour une table externe, SQL stocke uniquement les métadonnées de la table avec des statistiques de base sur le fichier ou le dossier qui est référencé dans Azure SQL Database. Aucune donnée n’est déplacée ni stockée dans Azure SQL Database.

> [!IMPORTANT]
> Pour des performances optimales, si le pilote de source de données externe prend en charge un nom en trois parties, il est fortement recommandé de fournir ce nom en trois parties.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE prend en charge la possibilité de configurer le nom de colonne, le type de données, la possibilité d’une valeur Null et le classement. Vous ne pouvez pas utiliser DEFAULT CONSTRAINT sur des tables externes.

> [!NOTE]
> `Text`, `nText` et`XML` ne sont pas des types de données pris en charge pour les colonnes de tables externes dans Azure SQL Database.

Les définitions de colonne, notamment les types de données et le nombre de colonnes, doivent correspondre aux données des fichiers externes. En cas de non-correspondance, les lignes du fichier sont rejetées lors de l’interrogation des données réelles.

Options de table externe partitionnée

Spécifie la source de données externe (source de données non-SQL Server) et une méthode de distribution pour la [requête élastique](/azure/azure-sql/database/elastic-query-overview).

DATA_SOURCE La clause DATA_SOURCE définit la source de données externe (une carte de partitions dans le cas d’un partitionnement horizontal) qui est utilisée pour la table externe. Pour obtenir un exemple, consultez [Créer des tables externes](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning#13-create-external-tables).

SCHEMA_NAME et OBJECT_NAME Les clauses SCHEMA_NAME et OBJECT_NAME mappent la définition de table externe à une table située dans un schéma différent. En cas d’omission, le schéma de l’objet distant est supposé être « dbo » et son nom est supposé être identique au nom de la table externe en cours de définition. Ceci est particulièrement utile si le nom de votre table distante est déjà utilisé dans la base de données dans laquelle vous souhaitez créer la table externe. Par exemple, vous souhaitez définir une table externe pour obtenir une vue agrégée des affichages de catalogue ou de vues de gestion dynamiques sur la couche des données mise à l’échelle. Dans la mesure où les affichages catalogue et les vues de gestion dynamique existent déjà localement, vous ne pouvez pas utiliser leur nom pour la définition de la table externe. Vous devez utiliser un autre nom ainsi que le nom de la vue de catalogue ou de la vue de gestion dynamique dans les clauses SCHEMA_NAME et/ou OBJECT_NAME. Pour obtenir un exemple, consultez [Créer des tables externes](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning#13-create-external-tables).

DISTRIBUTION La clause DISTRIBUTION spécifie la distribution des données utilisée pour cette table. Le processeur de requêtes utilise les informations fournies dans la clause DISTRIBUTION pour créer les plans de requête les plus efficaces.

- SHARDED signifie que les données sont partitionnées horizontalement entre les bases de données. La clé de partitionnement pour la distribution des données figure dans le paramètre <nom_colonne_partitionnement>.
- REPLICATED signifie que des copies identiques de la table sont présentes sur chaque base de données. La responsabilité de vous assurer que les réplicas sont identiques d’une base de données à l’autre vous incombe.
- ROUND_ROBIN signifie que la table est partitionnée horizontalement à l’aide d’une méthode de distribution liée à l’application.

## <a name="permissions"></a>Autorisations

Les utilisateurs ayant accès à la table externe acquièrent un accès automatique aux tables distantes sous-jacentes avec les informations d’identification fournies dans la définition de source de données externe. Évitez une élévation de privilèges non souhaitée par le biais d’informations d'identification de la source de données externe. Utilisez GRANT ou REVOKE pour une table externe, comme s'il s'agissait d'une table standard. Une fois votre table externe et votre source de données externe définies, vous pouvez utiliser l’ensemble T-SQL complet sur vos tables externes.

## <a name="error-handling"></a>Gestion des erreurs

Lors de l’exécution de l’instruction CREATE EXTERNAL TABLE, si la tentative de connexion échoue, l’instruction échoue et la table externe n’est pas créée. L’échec de la commande peut prendre plusieurs minutes, car SQL Database tente plusieurs connexions avant que la requête n’échoue.

## <a name="general-remarks"></a>Remarques d'ordre général

Dans les scénarios de requête ad hoc, comme avec SELECT FROM EXTERNAL TABLE, SQL Database stocke les lignes qui sont extraites de la source de données externe dans une table temporaire. Une fois la requête terminée, SQL Database supprime la table temporaire. Aucune donnée permanente n’est stockée dans les tables SQL.

En revanche, dans le scénario d’importation, comme avec SELECT INTO FROM EXTERNAL TABLE, SQL Database stocke les lignes qui sont extraites de la source de données externe sous forme de données permanentes dans la table SQL. La nouvelle table est créée lors de l’exécution de la requête, au moment où SQL Database récupère les données externes.

Vous pouvez créer plusieurs tables externes qui référencent les mêmes sources de données externes ou des sources différentes.

## <a name="limitations-and-restrictions"></a>Limitations et restrictions

L’accès aux données par le biais d’une table externe n’est pas conforme à la sémantique d’isolation dans SQL Server. Cela signifie que l’interrogation d’une table externe n’impose pas de verrouillage ni d’isolation des instantanés et que, par conséquent, les données retournées peuvent changer si les données dans la source de données externe sont modifiées.  La même requête peut retourner des résultats différents chaque fois qu’elle est exécutée sur une table externe. De même, une requête peut échouer si les données externes sont déplacées ou supprimées.

Vous pouvez créer plusieurs tables externes qui référencent chacune des sources de données externes différentes.

Seules les instructions DDL suivantes sont autorisées avec les tables externes :

- CREATE TABLE et DROP TABLE
- CREATE VIEW et DROP VIEW

Les constructions et les opérations suivantes ne sont pas prises en charge :

- La contrainte DEFAULT sur les colonnes de table externe
- Les opérations DML delete, insert et update

Seuls les prédicats littéraux définis dans une requête peuvent être envoyés (push down) vers la source de données externe. Cela diffère des serveurs liés et de l’accès où les prédicats définis au moment de l’exécution de la requête peuvent être utilisés, par exemple, en combinaison avec une boucle imbriquée dans un plan de requête. Le plus souvent, il en résulte que toute la table externe est copiée localement avant d’être jointe.

```sql
  \\ Assuming External.Orders is an external table and Customer is a local table.
  \\ This query  will copy the whole of the external locally as the predicate needed
  \\ to filter isn't known at compile time. Its only known during execution of the query
  
  SELECT Orders.OrderId, Orders.OrderTotal
    FROM External.Orders
   WHERE CustomerId in (SELECT TOP 1 CustomerId
                          FROM Customer
                          WHERE CustomerName = 'MyCompany')
```

Avec les tables externes, vous ne pouvez pas utiliser le parallélisme dans le plan de requête.

Les tables externes étant implémentées sous forme de requête sur source distante, le nombre estimé de lignes retournées est généralement de 1 000, et d’autres règles basées sur le type de prédicat sont utilisées pour filtrer la table externe. Il s’agit d’estimations basées sur des règles au lieu d’estimations basées sur les données réelles contenues dans la table externe. L’optimiseur n’accède pas à la source de données distante pour obtenir une estimation plus précise.

### <a name="data-type-limitations"></a>Limitations des types de données

Les types de données suivants ne peuvent pas être utilisés dans des tables externes Polybase :

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Tout type défini par l’utilisateur

## <a name="locking"></a>Verrouillage

Verrou partagé sur l’objet SCHEMARESOLUTION.

## <a name="examples"></a>Exemples

### <a name="a-create-external-table-for-azure-sql-database"></a>R. Créer une table externe pour Azure SQL Database

```sql
CREATE EXTERNAL TABLE [dbo].[CustomerInformation]
( [CustomerID] [int] NOT NULL,
  [CustomerName] [varchar](50) NOT NULL,
  [Company] [varchar](50) NOT NULL)
WITH
( DATA_SOURCE = MyElasticDBQueryDataSrc)
```

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble de la requête élastique Azure SQL Database](/azure/sql-database/sql-database-elastic-query-overview)
- [Créer des rapports sur des bases de données cloud mises à l’échelle](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning)
- [Bien démarrer avec les requêtes de bases de données croisées (partitionnement vertical)](/azure/sql-database/sql-database-elastic-query-getting-started-vertical)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-synapse-analytics"></a>Présentation : Azure Synapse Analytics

Utilisez une table externe pour :

- Interroger des données Hadoop ou Azure Blob Storage avec des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]
- Importer et stocker des données depuis Hadoop ou le stockage Blob Azure.
- Importer et stocker des données depuis Azure Data Lake Store

Voir aussi [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) et [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).  

## <a name="syntax"></a>Syntaxe

### [[!INCLUDE[sss-dedicated-pool-md.md](../../includes/sss-dedicated-pool-md.md)]](#tab/dedicated)
```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'hdfs_folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
  
<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage,  
    | REJECT_VALUE = reject_value,  
    | REJECT_SAMPLE_VALUE = reject_sample_value,
    | REJECTED_ROW_LOCATION = '/REJECT_Directory'
}  
```
### [[!INCLUDE[sssod-md.md](../../includes/sssod-md.md)]](#tab/serverless)
```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name } 
    ( <column_definition> [ ,...n ] )   
    WITH ( 
        LOCATION = 'folder_or_filepath',   
        DATA_SOURCE = external_data_source_name,   
        FILE_FORMAT = external_file_format_name   
    )   
[;]   
<column_definition> ::= 
column_name <data_type> 
    [ COLLATE collation_name ] 
```
---

## <a name="arguments"></a>Arguments

*{database_name.schema_name.table_name | nom_schéma.nom_table | nom_table}*  Nom (composé d’une à trois parties) de la table à créer. Pour une table externe, seules les métadonnées de la table avec des statistiques de base sur le fichier ou le dossier qui sont référencées dans Azure Data Lake, Hadoop ou le stockage Blob Azure. Aucune donnée réelle n’est déplacée ni stockée lors de la création de tables externes.

> [!IMPORTANT]
> Pour des performances optimales, si le pilote de source de données externe prend en charge un nom en trois parties, il est fortement recommandé de fournir ce nom en trois parties.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE prend en charge la possibilité de configurer le nom de colonne, le type de données, la possibilité d’une valeur Null et le classement. Vous ne pouvez pas utiliser DEFAULT CONSTRAINT sur des tables externes.

> [!NOTE]
> `Text`, `nText` et`XML` ne sont pas des types de données pris en charge pour les colonnes de tables externes dans Azure SQL Warehouse.

Les définitions de colonne, notamment les types de données et le nombre de colonnes, doivent correspondre aux données des fichiers externes. En cas de non-correspondance, les lignes du fichier sont rejetées lors de l’interrogation des données réelles.

LOCATION = '*folder_or_filepath*' Spécifie le dossier, ou le chemin et le nom du fichier, où se trouvent les données dans Azure Data Lake, Hadoop ou le Stockage Blob Azure. L’emplacement commence au dossier racine. Le dossier racine est l’emplacement de données qui est spécifié dans la source de données externe. L’instruction [CREATE EXTERNAL TABLE AS SELECT](create-external-table-as-select-transact-sql.md) crée le chemin et le dossier s’ils n’existent pas. `CREATE EXTERNAL TABLE` ne crée pas le chemin et le dossier.

Si vous spécifiez LOCATION comme étant un dossier, une requête PolyBase qui sélectionne des données dans la table externe récupère les fichiers dans le dossier et dans tous ses sous-dossiers. Tout comme Hadoop, PolyBase ne retourne pas les dossiers masqués. Il ne retourne pas non plus les fichiers dont le nom commence par un trait de soulignement (_) ou un point (.).

Dans cet exemple, si LOCATION='/webdata/', une requête PolyBase retourne des lignes à partir de mydata.txt et mydata2.txt. Il ne retourne pas les données de mydata3.txt, car il se trouve dans un sous-dossier d’un dossier masqué. Il ne retourne pas non plus _hidden.txt car il s’agit d’un fichier masqué.

![Données récursives pour les tables externes](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Données récursives pour les tables externes")

Pour modifier la valeur par défaut et uniquement lire les données du dossier racine, définissez l’attribut \<polybase.recursive.traversal>sur 'false' dans le fichier de configuration core-site.xml. Ce fichier se trouve sous `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Par exemple : `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *external_data_source_name* Spécifie le nom de la source de données externe qui contient l’emplacement des données externes. Cet emplacement figure dans Azure Data Lake. Pour créer une source de données externe, utilisez [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *external_file_format_name* Spécifie le nom de l’objet de format de fichier externe qui stocke le type de fichier et la méthode de compression des données externes. Pour créer un format de fichier externe, utilisez [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Options REJECT Vous pouvez spécifier les paramètres REJECT qui déterminent la façon dont PolyBase traite les enregistrements *incorrects* qu’il récupère à partir de la source de données externe. Un enregistrement de données est considéré comme « incorrect » si les types de données ou le nombre de colonnes ne correspondent pas aux définitions de colonnes de la table externe.

Si vous ne spécifiez pas ou ne changez pas les valeurs REJECT, PolyBase utilise les valeurs par défaut. Ces informations sur les paramètres REJECT sont stockées en tant que métadonnées supplémentaires lorsque vous créez une table externe avec l’instruction CREATE EXTERNAL TABLE. Quand une prochaine instruction SELECT ou SELECT INTO SELECT sélectionne des données dans la table externe, PolyBase utilise les options REJECT pour déterminer le nombre ou le pourcentage de lignes pouvant être rejetées avant de provoquer l’échec de la requête. La requête retourne des résultats (partiels) jusqu’à ce que le seuil de rejet soit dépassé. Ensuite, elle échoue avec le message d’erreur correspondant.

REJECT_TYPE = **value** | percentage Précise si l’option REJECT_VALUE est spécifiée comme une valeur littérale ou un pourcentage.

La valeur REJECT_VALUE est une valeur littérale, et non un pourcentage. La requête PolyBase échoue lorsque le nombre de lignes rejetées dépasse la valeur *reject_value*.

Par exemple, si REJECT_VALUE = 5 et REJECT_TYPE = value, la requête PolyBase SELECT échoue après le rejet de cinq lignes.

Le pourcentage REJECT_VALUE est un pourcentage, et non une valeur littérale. Une requête PolyBase échoue lorsque le *pourcentage* de lignes ayant échoué dépasse la valeur *reject_value*. Le pourcentage de lignes ayant échoué est calculé à intervalles.

REJECT_VALUE = *reject_value* Spécifie la valeur ou le pourcentage de lignes pouvant être rejetées avant de provoquer l’échec de la requête.

Pour REJECT_TYPE = value, *reject_value* doit être un entier compris entre 0 et 2 147 483 647.

Pour REJECT_TYPE = percentage, *reject_value* doit être une valeur float comprise entre 0 et 100.

REJECT_SAMPLE_VALUE = *reject_sample_value* Cet attribut est nécessaire lorsque vous spécifiez REJECT_TYPE = percentage. Il détermine le nombre de lignes à tenter de récupérer avant que PolyBase ne recalcule le pourcentage de lignes rejetées.

Le paramètre *reject_sample_value* doit être un entier compris entre 0 et 2 147 483 647.

Par exemple, si REJECT_SAMPLE_VALUE = 1000, PolyBase calcule le pourcentage de lignes ayant échoué après avoir tenté d’importer 1000 lignes à partir du fichier de données externe. Si le pourcentage de lignes ayant échoué est inférieur à la valeur de *reject_value*, PolyBase tente de récupérer 1000 autres lignes. Il continue de recalculer le pourcentage de lignes ayant échoué après avoir tenté d’importer chacune des 1000 lignes supplémentaires.

> [!NOTE]
> Étant donné que PolyBase calcule le pourcentage de lignes ayant échoué à intervalles, le pourcentage de lignes ayant échoué peut dépasser la valeur de *reject_value*.

Exemple :

Cet exemple montre comment les trois options REJECT interagissent les unes avec les autres. Par exemple, si REJECT_TYPE = percentage, REJECT_VALUE = 30 et REJECT_SAMPLE_VALUE = 100, le scénario suivant peut se produire :

- PolyBase tente de récupérer les 100 premières lignes : la récupération échoue pour 25 d’entre elles, et réussit pour les 75 autres.
- Le pourcentage de lignes ayant échoué qui est obtenu est de 25 %, ce qui est inférieur à la valeur de rejet de 30 %. Par conséquent, PolyBase va continuer de récupérer les données à partir de la source de données externe.
- PolyBase tente de charger les 100 lignes suivantes. Cette fois-ci, le chargement réussit pour 25 lignes et échoue pour les 75 autres.
- Le pourcentage de lignes ayant échoué est recalculé et on obtient 50 %. Le pourcentage de lignes ayant échoué a donc dépassé la valeur de rejet de 30 %.
- La requête PolyBase échoue après le rejet de 50 % des 200 premières lignes qu’elle a tenté de retourner. Notez que les lignes correspondantes sont retournées avant que la requête PolyBase ne détecte que le seuil de rejet a été dépassé.

REJECTED_ROW_LOCATION = *Emplacement de répertoire*

Spécifie le répertoire dans la Source de données externe dans lequel les lignes rejetées et le fichier d’erreur correspondant doivent être écrits.
Si le chemin spécifié n’existe pas, PolyBase en crée un en votre nom. Un répertoire enfant est créé sous le nom « \_rejectedrows ». Le caractère « \_ » garantit que le répertoire est placé dans une séquence d’échappement pour le traitement d’autres données, sauf s’il est explicitement nommé dans le paramètre d’emplacement. Dans ce répertoire figure un dossier qui est créé d’après l’heure de soumission du chargement au format AnnéeMoisJour - HeureMinuteSeconde (par exemple, 20180330-173205). Dans ce dossier, deux types de fichiers sont écrits : le fichier _reason (raison) et le fichier de données.

Les fichiers de raison et les fichiers de données ont tous deux le queryID associé à l’instruction CTAS. Comme les données et la raison se trouvent dans des fichiers distincts, les fichiers correspondants ont un suffixe analogue.

## <a name="permissions"></a>Autorisations

Nécessite les autorisations utilisateur suivantes :

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**

> [!NOTE]
> Les autorisations de BASE DE DONNÉES DE CONTRÔLE sont requises pour créer uniquement la CLÉ PRINCIPALE, LES INFORMATIONS D’IDENTIFICATION DÉLIMITÉES À LA BASE DE DONNÉES et LA SOURCE DE DONNÉES EXTERNE

Notez que la connexion qui crée la source de données externe doit être autorisée à lire et à écrire dans la source de données externe, qui est située dans Hadoop ou Azure Blob Storage.

> [!IMPORTANT]
> L’autorisation ALTER ANY EXTERNAL DATA SOURCE accorde à n’importe quel principal la possibilité de créer et de modifier tout objet de source de données externe. Par conséquent, elle permet également d’accéder à toutes les informations d’identification délimitées à la base de données. Cette autorisation doit être considérée comme fournissant des privilèges très élevés, et doit donc être accordée uniquement aux principaux de confiance du système.

## <a name="error-handling"></a>Gestion des erreurs

Lors de l’exécution de l’instruction CREATE EXTERNAL TABLE, PolyBase tente de se connecter à la source de données externe. Si la tentative de connexion échoue, l’instruction échoue et la table externe n’est pas créée. L’échec de la commande peut prendre plusieurs minutes, car PolyBase tente plusieurs connexions avant que la requête n’échoue.

## <a name="general-remarks"></a>Remarques d'ordre général

Dans les scénarios de requête ad hoc, comme avec SELECT FROM EXTERNAL TABLE, PolyBase stocke les lignes qui sont extraites de la source de données externe dans une table temporaire. Une fois la requête terminée, PolyBase supprime la table temporaire. Aucune donnée permanente n’est stockée dans les tables SQL.

En revanche, dans le scénario d’importation, comme avec SELECT INTO FROM EXTERNAL TABLE, PolyBase stocke les lignes qui sont extraites de la source de données externe sous forme de données permanentes dans la table SQL. La nouvelle table est créée lors de l’exécution de la requête, au moment où PolyBase récupère les données externes.

PolyBase peut envoyer (push) une partie du calcul des requêtes vers Hadoop pour améliorer les performances des requêtes. Cette action est appelée « poussée de prédicats ». Pour l’activer, spécifiez l’option d’emplacement du Gestionnaire de ressources Hadoop dans [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

Vous pouvez créer plusieurs tables externes qui référencent les mêmes sources de données externes ou des sources différentes.

## <a name="limitations-and-restrictions"></a>Limitations et restrictions

Dans la mesure où les données d’une table externe ne sont pas sous le contrôle de gestion direct d’Azure Synapse, elles peuvent être changées ou supprimées à tout moment par un processus externe. Par conséquent, il n’est pas garanti que les résultats d’une requête exécutée sur une table externe soient déterministes. La même requête peut retourner des résultats différents chaque fois qu’elle est exécutée sur une table externe. De même, une requête peut échouer si les données externes sont déplacées ou supprimées.

Vous pouvez créer plusieurs tables externes qui référencent chacune des sources de données externes différentes.

Seules les instructions DDL suivantes sont autorisées avec les tables externes :

- CREATE TABLE et DROP TABLE
- CREATE STATISTICS et DROP STATISTICS
- CREATE VIEW et DROP VIEW

Les constructions et les opérations suivantes ne sont pas prises en charge :

- La contrainte DEFAULT sur les colonnes de table externe
- Les opérations DML delete, insert et update

### <a name="query-limitations"></a>Limitations des requêtes

Il est recommandé de ne pas dépasser plus de 30 000 fichiers par dossier. Lorsque trop de fichiers sont référencés, une exception d’insuffisance de mémoire de machine virtuelle Java (JVM) peut être levée ou les performances peuvent se dégrader.

### <a name="table-width-limitations"></a>Limitations concernant la largeur des tables

Dans Azure Data Warehouse, PolyBase a une limite de largeur de ligne de 1 Mo, basée sur la taille maximale d’une ligne valide par définition de table. Si la somme du schéma de colonne est supérieure à 1 Mo, PolyBase ne peut pas interroger les données.

### <a name="data-type-limitations"></a>Limitations des types de données

Les types de données suivants ne peuvent pas être utilisés dans des tables externes Polybase :

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Tout type défini par l’utilisateur

## <a name="locking"></a>Verrouillage

Verrou partagé sur l’objet SCHEMARESOLUTION.

## <a name="examples"></a>Exemples

### <a name="a-importing-data-from-adls-gen-2-into-azure-ssdw"></a>R. Importation de données ADLS Gen 2 dans Azure [!INCLUDE[ssDW](../../includes/ssdw-md.md)]. 

Pour obtenir des exemples de Gen ADLS Gen 1, consultez [Créer une source de données externe](create-external-data-source-transact-sql.md).

```sql

-- These values come from your Azure Active Directory Application used to authenticate to ADLS Gen 2. 
CREATE DATABASE SCOPED CREDENTIAL ADLUser
WITH IDENTITY = '<clientID>@\<OAuth2.0TokenEndPoint>',
SECRET = '<KEY>' ;

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (TYPE = HADOOP,
      LOCATION = 'abfss://data@pbasetr.azuredatalakestore.net'
)

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(
    FORMAT_TYPE = DELIMITEDTEXT
    , FORMAT_OPTIONS ( FIELD_TERMINATOR = '|'
       , STRING_DELIMITER = ''
      , DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fff'
      , USE_TYPE_DEFAULT = FALSE
      )
)

CREATE EXTERNAL TABLE [dbo].[DimProductexternal]
( [ProductKey] [int] NOT NULL,
  [ProductLabel] nvarchar NULL,
  [ProductName] nvarchar NULL )
WITH
(
    LOCATION='/DimProduct/' ,
    DATA_SOURCE = AzureDataLakeStore ,
    FILE_FORMAT = TextFileFormat ,
    REJECT_TYPE = VALUE ,
    REJECT_VALUE = 0
) ;

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey] ) )
AS SELECT * FROM
[dbo].[DimProduct_external] ;
```

## <a name="see-also"></a>Voir aussi

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-analytics-platform-system"></a>Présentation : Système de la plateforme d'analyse

Utilisez une table externe pour :

- Interroger des données Hadoop ou Azure Blob Storage avec des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]
- Importer des données Hadoop ou Azure Blob Storage, et les stocker dans le système de la plateforme d’analyse.

Voir aussi [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) et [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).

## <a name="syntax"></a>Syntaxe

```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'hdfs_folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]

<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage,  
    | REJECT_VALUE = reject_value,  
    | REJECT_SAMPLE_VALUE = reject_sample_value,
  
}  
```

## <a name="arguments"></a>Arguments

*{database_name.schema_name.table_name | nom_schéma.nom_table | nom_table}*  Nom (composé d’une à trois parties) de la table à créer. Pour une table externe, le système de la plateforme d’analyse stocke uniquement les métadonnées de la table avec des statistiques de base sur le fichier ou le dossier qui est référencé dans Hadoop ou le Stockage Blob Azure. Aucune donnée réelle est déplacée ou stockée dans le système de la plateforme d’analyse.

> [!IMPORTANT]
> Pour des performances optimales, si le pilote de source de données externe prend en charge un nom en trois parties, il est fortement recommandé de fournir ce nom en trois parties.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE prend en charge la possibilité de configurer le nom de colonne, le type de données, la possibilité d’une valeur Null et le classement. Vous ne pouvez pas utiliser DEFAULT CONSTRAINT sur des tables externes.

Les définitions de colonne, notamment les types de données et le nombre de colonnes, doivent correspondre aux données des fichiers externes. En cas de non-correspondance, les lignes du fichier sont rejetées lors de l’interrogation des données réelles.

LOCATION = '*folder_or_filepath*' Spécifie le dossier, ou le chemin et le nom du fichier, où se trouvent les données Hadoop ou Azure Blob Storage. L’emplacement commence au dossier racine. Le dossier racine est l’emplacement de données qui est spécifié dans la source de données externe.

Dans le système de la plateforme d’analyse, l’instruction [CREATE EXTERNAL TABLE AS SELECT](create-external-table-as-select-transact-sql.md) crée le chemin et le dossier s’ils n’existent pas. `CREATE EXTERNAL TABLE` ne crée pas le chemin et le dossier.

Si vous spécifiez LOCATION comme étant un dossier, une requête PolyBase qui sélectionne des données dans la table externe récupère les fichiers dans le dossier et dans tous ses sous-dossiers. Tout comme Hadoop, PolyBase ne retourne pas les dossiers masqués. Il ne retourne pas non plus les fichiers dont le nom commence par un trait de soulignement (_) ou un point (.).

Dans cet exemple, si LOCATION='/webdata/', une requête PolyBase retourne des lignes à partir de mydata.txt et mydata2.txt. Il ne retourne pas les données de mydata3.txt, car il se trouve dans un sous-dossier d’un dossier masqué. Il ne retourne pas non plus _hidden.txt car il s’agit d’un fichier masqué.

![Données récursives pour les tables externes](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Données récursives pour les tables externes")

Pour modifier la valeur par défaut et uniquement lire les données du dossier racine, définissez l’attribut \<polybase.recursive.traversal>sur 'false' dans le fichier de configuration core-site.xml. Ce fichier se trouve sous `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Par exemple : `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *external_data_source_name* Spécifie le nom de la source de données externe qui contient l’emplacement des données externes. Cet emplacement est soit Hadoop, soit Azure Blob Storage. Pour créer une source de données externe, utilisez [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *external_file_format_name* Spécifie le nom de l’objet de format de fichier externe qui stocke le type de fichier et la méthode de compression des données externes. Pour créer un format de fichier externe, utilisez [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Options REJECT Vous pouvez spécifier les paramètres REJECT qui déterminent la façon dont PolyBase traite les enregistrements *incorrects* qu’il récupère à partir de la source de données externe. Un enregistrement de données est considéré comme « incorrect » si les types de données ou le nombre de colonnes ne correspondent pas aux définitions de colonnes de la table externe.

Si vous ne spécifiez pas ou ne changez pas les valeurs REJECT, PolyBase utilise les valeurs par défaut. Ces informations sur les paramètres REJECT sont stockées en tant que métadonnées supplémentaires lorsque vous créez une table externe avec l’instruction CREATE EXTERNAL TABLE. Quand une prochaine instruction SELECT ou SELECT INTO SELECT sélectionne des données dans la table externe, PolyBase utilise les options REJECT pour déterminer le nombre ou le pourcentage de lignes pouvant être rejetées avant de provoquer l’échec de la requête. La requête retourne des résultats (partiels) jusqu’à ce que le seuil de rejet soit dépassé. Ensuite, elle échoue avec le message d’erreur correspondant.

REJECT_TYPE = **value** | percentage Précise si l’option REJECT_VALUE est spécifiée comme une valeur littérale ou un pourcentage.

La valeur REJECT_VALUE est une valeur littérale, et non un pourcentage. La requête PolyBase échoue lorsque le nombre de lignes rejetées dépasse la valeur *reject_value*.

Par exemple, si REJECT_VALUE = 5 et REJECT_TYPE = value, la requête PolyBase SELECT échoue après le rejet de cinq lignes.

Le pourcentage REJECT_VALUE est un pourcentage, et non une valeur littérale. Une requête PolyBase échoue lorsque le *pourcentage* de lignes ayant échoué dépasse la valeur *reject_value*. Le pourcentage de lignes ayant échoué est calculé à intervalles.

REJECT_VALUE = *reject_value* Spécifie la valeur ou le pourcentage de lignes pouvant être rejetées avant de provoquer l’échec de la requête.

Pour REJECT_TYPE = value, *reject_value* doit être un entier compris entre 0 et 2 147 483 647.

Pour REJECT_TYPE = percentage, *reject_value* doit être une valeur float comprise entre 0 et 100.

REJECT_SAMPLE_VALUE = *reject_sample_value* Cet attribut est nécessaire lorsque vous spécifiez REJECT_TYPE = percentage. Il détermine le nombre de lignes à tenter de récupérer avant que PolyBase ne recalcule le pourcentage de lignes rejetées.

Le paramètre *reject_sample_value* doit être un entier compris entre 0 et 2 147 483 647.

Par exemple, si REJECT_SAMPLE_VALUE = 1000, PolyBase calcule le pourcentage de lignes ayant échoué après avoir tenté d’importer 1000 lignes à partir du fichier de données externe. Si le pourcentage de lignes ayant échoué est inférieur à la valeur de *reject_value*, PolyBase tente de récupérer 1000 autres lignes. Il continue de recalculer le pourcentage de lignes ayant échoué après avoir tenté d’importer chacune des 1000 lignes supplémentaires.

> [!NOTE]
> Étant donné que PolyBase calcule le pourcentage de lignes ayant échoué à intervalles, le pourcentage de lignes ayant échoué peut dépasser la valeur de *reject_value*.

Exemple :

Cet exemple montre comment les trois options REJECT interagissent les unes avec les autres. Par exemple, si REJECT_TYPE = percentage, REJECT_VALUE = 30 et REJECT_SAMPLE_VALUE = 100, le scénario suivant peut se produire :

- PolyBase tente de récupérer les 100 premières lignes : la récupération échoue pour 25 d’entre elles, et réussit pour les 75 autres.
- Le pourcentage de lignes ayant échoué qui est obtenu est de 25 %, ce qui est inférieur à la valeur de rejet de 30 %. Par conséquent, PolyBase va continuer de récupérer les données à partir de la source de données externe.
- PolyBase tente de charger les 100 lignes suivantes. Cette fois-ci, le chargement réussit pour 25 lignes et échoue pour les 75 autres.
- Le pourcentage de lignes ayant échoué est recalculé et on obtient 50 %. Le pourcentage de lignes ayant échoué a donc dépassé la valeur de rejet de 30 %.
- La requête PolyBase échoue après le rejet de 50 % des 200 premières lignes qu’elle a tenté de retourner. Notez que les lignes correspondantes sont retournées avant que la requête PolyBase ne détecte que le seuil de rejet a été dépassé.

## <a name="permissions"></a>Autorisations

Nécessite les autorisations utilisateur suivantes :

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- **CONTROL DATABASE**

Notez que la connexion qui crée la source de données externe doit être autorisée à lire et à écrire dans la source de données externe, qui est située dans Hadoop ou Azure Blob Storage.

> [!IMPORTANT]
> L’autorisation ALTER ANY EXTERNAL DATA SOURCE accorde à n’importe quel principal la possibilité de créer et de modifier tout objet de source de données externe. Par conséquent, elle permet également d’accéder à toutes les informations d’identification délimitées à la base de données. Cette autorisation doit être considérée comme fournissant des privilèges très élevés, et doit donc être accordée uniquement aux principaux de confiance du système.

## <a name="error-handling"></a>Gestion des erreurs

Lors de l’exécution de l’instruction CREATE EXTERNAL TABLE, PolyBase tente de se connecter à la source de données externe. Si la tentative de connexion échoue, l’instruction échoue et la table externe n’est pas créée. L’échec de la commande peut prendre plusieurs minutes, car PolyBase tente plusieurs connexions avant que la requête n’échoue.

## <a name="general-remarks"></a>Remarques d'ordre général

Dans les scénarios de requête ad hoc, comme avec SELECT FROM EXTERNAL TABLE, PolyBase stocke les lignes qui sont extraites de la source de données externe dans une table temporaire. Une fois la requête terminée, PolyBase supprime la table temporaire. Aucune donnée permanente n’est stockée dans les tables SQL.

En revanche, dans le scénario d’importation, comme avec SELECT INTO FROM EXTERNAL TABLE, PolyBase stocke les lignes qui sont extraites de la source de données externe sous forme de données permanentes dans la table SQL. La nouvelle table est créée lors de l’exécution de la requête, au moment où PolyBase récupère les données externes.

PolyBase peut envoyer (push) une partie du calcul des requêtes vers Hadoop pour améliorer les performances des requêtes. Cette action est appelée « poussée de prédicats ». Pour l’activer, spécifiez l’option d’emplacement du Gestionnaire de ressources Hadoop dans [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

Vous pouvez créer plusieurs tables externes qui référencent les mêmes sources de données externes ou des sources différentes.

## <a name="limitations-and-restrictions"></a>Limitations et restrictions

Dans la mesure où les données d’une table externe ne sont pas sous le contrôle de gestion direct de l’appliance, elles peuvent être changées ou supprimées à tout moment par un processus externe. Par conséquent, il n’est pas garanti que les résultats d’une requête exécutée sur une table externe soient déterministes. La même requête peut retourner des résultats différents chaque fois qu’elle est exécutée sur une table externe. De même, une requête peut échouer si les données externes sont déplacées ou supprimées.

Vous pouvez créer plusieurs tables externes qui référencent chacune des sources de données externes différentes. Si vous exécutez simultanément plusieurs requêtes sur des sources de données Hadoop différentes, chaque source Hadoop doit utiliser le même paramètre de configuration de serveur « hadoop connectivity » (connectivité Hadoop). Par exemple, vous ne pouvez pas exécuter simultanément une requête sur un cluster Hadoop Cloudera et sur un cluster Hadoop Hortonworks, puisqu’ils utilisent des paramètres de configuration différents. Pour connaître les paramètres de configuration et les combinaisons prises en charge, consultez [Configuration de la connectivité PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

Seules les instructions DDL suivantes sont autorisées avec les tables externes :

- CREATE TABLE et DROP TABLE
- CREATE STATISTICS et DROP STATISTICS
- CREATE VIEW et DROP VIEW

Les constructions et les opérations suivantes ne sont pas prises en charge :

- La contrainte DEFAULT sur les colonnes de table externe
- Les opérations DML delete, insert et update

### <a name="query-limitations"></a>Limitations des requêtes

PolyBase peut consommer un maximum de 33 000 fichiers par dossier lors de l’exécution simultanée de 32 requêtes PolyBase. Ce nombre maximal inclut les fichiers et les sous-dossiers de chaque dossier HDFS. Si le degré de concurrence est inférieur à 32, un utilisateur peut exécuter des requêtes PolyBase sur des dossiers dans des systèmes HDFS contenant plus de 33 000 fichiers. Il est recommandé de raccourcir au maximum les chemins de fichiers externes et de ne pas utiliser plus de 30 000 fichiers par dossier HDFS. Lorsque trop de fichiers sont référencés, une exception d’insuffisance de mémoire Java Virtual Machine (JVM) peut être levée.

### <a name="table-width-limitations"></a>Limitations concernant la largeur des tables

Dans SQL Server 2016, PolyBase a une limite de largeur de ligne de 32 Ko, basée sur la taille maximale d’une ligne valide par définition de table. Si la somme du schéma de colonne est supérieure à 32 Ko, PolyBase ne peut pas interroger les données.

Dans Azure Synapse Analytics, cette limitation a été augmentée à 1 Mo.

### <a name="data-type-limitations"></a>Limitations des types de données

Les types de données suivants ne peuvent pas être utilisés dans des tables externes Polybase :

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Tout type défini par l’utilisateur

## <a name="locking"></a>Verrouillage

Verrou partagé sur l’objet SCHEMARESOLUTION.

## <a name="security"></a>Sécurité

Les fichiers de données d’une table externe sont stockés dans Hadoop ou le Stockage Blob Azure. Ces fichiers de données sont créés et gérés par vos propres processus. Il vous incombe de gérer la sécurité des données externes.

## <a name="examples"></a>Exemples

### <a name="a-join-hdfs-data-with-analytics-platform-system-data"></a>R. Joindre des données HDFS avec des données du système de la plateforme d’analyse

```sql
SELECT cs.user_ip FROM ClickStream cs
JOIN User u ON cs.user_ip = u.user_ip
WHERE cs.url = 'www.microsoft.com'
;
```

### <a name="b-import-row-data-from-hdfs-into-a-distributed-analytics-platform-system-table"></a>B. Importer des données de ligne à partir de HDFS dans une table du système de la plateforme d’analyse distribuée

```sql
CREATE TABLE ClickStream_PDW
WITH ( DISTRIBUTION = HASH (url) )
AS SELECT url, event_date, user_ip FROM ClickStream
;
```

### <a name="c-import-row-data-from-hdfs-into-a-replicated-analytics-platform-system-table"></a>C. Importer des données de ligne à partir de HDFS dans une table du système de la plateforme d’analyse répliquée

```sql
CREATE TABLE ClickStream_PDW
WITH ( DISTRIBUTION = REPLICATE )
AS SELECT url, event_date, user_ip
FROM ClickStream
;
```

## <a name="see-also"></a>Voir aussi

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)

::: moniker-end
