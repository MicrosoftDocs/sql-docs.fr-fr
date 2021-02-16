---
description: OPENROWSET (Transact-SQL)
title: OPENROWSET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/30/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENROWSET_TSQL
- OPENROWSET
dev_langs:
- TSQL
helpviewer_keywords:
- data sources [SQL Server]
- OPENROWSET function
- remote data access [SQL Server], OPENROWSET
- ad hoc distributed queries
- OPENROWSET function, Transact-SQL
- OPENROWSET statement
- OLE DB data sources [SQL Server]
- ad hoc connection information
ms.assetid: f47eda43-33aa-454d-840a-bb15a031ca17
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: a2c7af1aaa1840b3c3468a00541f96325d8a5c52
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100352403"
---
# <a name="openrowset-transact-sql"></a>OPENROWSET (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Inclut toutes les informations de connexion exigées pour accéder à des données distantes à partir d'une source de données OLE DB. Cette méthode est une autre façon d'accéder à des tables dans un serveur lié et constitue une méthode efficace pour vous connecter et accéder à des données distantes en utilisant OLE DB. Pour faire des références plus fréquentes à des sources de données OLE DB, utilisez plutôt des serveurs liés. Pour plus d’informations, consultez [Serveurs liés &#40;moteur de base de données&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md). Il est possible de référencer la fonction `OPENROWSET` dans la clause FROM d’une requête comme s’il s’agissait du nom d’une table. La fonction `OPENROWSET` peut également être référencée comme table cible d’une instruction `INSERT`, `UPDATE` ou `DELETE`, en fonction des capacités du fournisseur OLE DB. Bien que la requête puisse retourner plusieurs jeux de résultats, `OPENROWSET` ne retourne que le premier.

`OPENROWSET` prend également en charge les opérations de chargement en masse par l’intermédiaire d’un fournisseur BULK intégré qui permet de lire les données d’un fichier et de les retourner comme un ensemble de lignes.

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
OPENROWSET
( { 'provider_name' 
    , { 'datasource' ; 'user_id' ; 'password' | 'provider_string' }
    , {   <table_or_view> | 'query' }
   | BULK 'data_file' ,
       { FORMATFILE = 'format_file_path' [ <bulk_options> ]
       | SINGLE_BLOB | SINGLE_CLOB | SINGLE_NCLOB }
} )

<table_or_view> ::= [ catalog. ] [ schema. ] object

<bulk_options> ::=

   [ , DATASOURCE = 'data_source_name' ]

   [ , ERRORFILE = 'file_name' ]
   [ , ERRORFILE_DATA_SOURCE = 'data_source_name' ]
   [ , MAXERRORS = maximum_errors ]

   [ , FIRSTROW = first_row ]
   [ , LASTROW = last_row ]
   [ , ROWS_PER_BATCH = rows_per_batch ]
   [ , ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) [ UNIQUE ] ]
  
   -- bulk_options related to input file format
   [ , CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]
   [ , FORMAT = 'CSV' ]
   [ , FIELDQUOTE = 'quote_characters']
   [ , FORMATFILE = 'format_file_path' ]
   [ , FORMATFILE_DATA_SOURCE = 'data_source_name' ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

### <a name="provider_name"></a>'*provider_name*'
Chaîne de caractères représentant le nom convivial (ou PROGID) du fournisseur OLE DB tel que spécifié dans le Registre. *provider_name* n’a aucune valeur par défaut. Des exemples de noms de fournisseurs sont `Microsoft.Jet.OLEDB.4.0`, `SQLNCLI` ou `MSDASQL`.

### <a name="datasource"></a>'*datasource*'
Constante de chaîne correspondant à une source de données OLE DB spécifique. *datasource* est la propriété DBPROP_INIT_DATASOURCE à transmettre à l’interface IDBProperties du fournisseur pour initialiser ce dernier. En général, cette chaîne comporte le nom du fichier de base de données, le nom d'un serveur de base de données ou un nom que comprend le fournisseur pour retrouver la ou les bases de données.
La source de données peut être le chemin de fichier `C:\SAMPLES\Northwind.mdb'` pour le fournisseur `Microsoft.Jet.OLEDB.4.0` ou la chaîne de connexion `Server=Seattle1;Trusted_Connection=yes;` pour le fournisseur `SQLNCLI`.

### <a name="user_id"></a>'*user_id*'
Constante de chaîne représentant le nom d'utilisateur passé au fournisseur OLE DB spécifié. *user_id* spécifie le contexte de sécurité de la connexion et est transmis en tant que propriété DBPROP_AUTH_USERID pour initialiser le fournisseur. *user_id* ne peut pas être un ID de connexion Microsoft Windows.

### <a name="password"></a>'*password*'
Constante de chaîne représentant le mot de passe utilisateur à transmettre au fournisseur OLE DB. *password* est transmis en tant que propriété DBPROP_AUTH_PASSWORD au moment de l’initialisation du fournisseur. *password* ne peut pas être un mot de passe Microsoft Windows.

```sql
SELECT a.*
   FROM OPENROWSET('Microsoft.Jet.OLEDB.4.0',
                   'C:\SAMPLES\Northwind.mdb';
                   'admin';
                   'password',
                   Customers) AS a;
```

### <a name="provider_string"></a>'*provider_string*'
Chaîne de connexion spécifique au fournisseur qui est passée en tant que propriété DBPROP_INIT_PROVIDERSTRING pour initialiser le fournisseur OLE DB. En général, *provider_string* encapsule toutes les informations de connexion nécessaires à l’initialisation du fournisseur. Pour obtenir la liste des mots clés reconnus par le fournisseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB, consultez [Propriétés d’initialisation et d’autorisation](../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md).

```sql
SELECT d.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
                            Department) AS d;
```

### <a name="table_or_view"></a><table_or_view>
Table ou vue distante contenant les données devant être lues par `OPENROWSET`. Il peut s’agir d’un objet au nom en trois parties avec les composants suivants :
- *catalog* (facultatif) : nom du catalogue ou de la base de données où réside l’objet spécifié.
- *schema* (facultatif) : nom du propriétaire du schéma ou de l’objet pour l’objet spécifié.
- *object* : nom d’objet qui identifie de façon unique l’objet à manipuler.

```sql
SELECT d.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
                 AdventureWorks2012.HumanResources.Department) AS d;
```

### <a name="query"></a>'*query*'
Constante de chaîne envoyée au fournisseur en vue de son exécution. L'instance locale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne traite pas cette requête, mais traite les résultats de la requête retournés par le fournisseur (requête directe). Les requêtes directes sont utiles lorsque les fournisseurs mettent leurs données tabulaires à disposition non pas par l'intermédiaire de noms de tables, mais uniquement au moyen d'un langage de commande. Les requêtes directes sont prises en charge sur le serveur distant à condition que le fournisseur de requêtes prenne en charge l’objet OLE DB Command et ses interfaces obligatoires. Pour plus d’informations, consultez [Informations de référence sur SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/sql-server-native-client-ole-db-interfaces.md).

```sql
SELECT a.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
     'SELECT TOP 10 GroupName, Name
     FROM AdventureWorks2012.HumanResources.Department') AS a;
```

### <a name="bulk"></a>BULK
Utilise le fournisseur d'ensembles de lignes BULK pour que OPENROWSET lise les données dans un fichier. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], OPENROWSET peut lire un fichier de données sans charger les données dans une table cible. Cela permet d’utiliser OPENROWSET avec une instruction SELECT simple.

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du Stockage Blob Azure.

Les arguments de l'option BULK permettent un contrôle significatif sur le début et la fin de la lecture des données, sur le traitement des erreurs et sur l'interprétation des données. Vous pouvez par exemple spécifier que le fichier de données soit lu comme un ensemble d’une seule ligne et deune seule colonne du type **varbinary**, **varchar** ou **nvarchar**. Le comportement par défaut est indiqué dans les descriptions des arguments ci-dessous.

 Pour plus d'informations sur l'utilisation de l'option BULK, consultez la section « Remarques » plus loin dans cette rubrique. Pour plus d'informations sur les autorisations requises par l'option BULK, consultez la section « Autorisations » plus loin dans cette rubrique.

> [!NOTE]
> Lorsqu'elle est utilisée pour importer des données avec le mode de récupération complète, la fonction OPENROWSET (BULK ...) n'optimise pas la journalisation.

Pour plus d’informations sur la préparation des données en vue d’une importation en bloc, consultez [Préparer des données en vue d’une exportation ou d’une importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

#### <a name="bulk-data_file"></a>BULK '*data_file*'
Chemin d'accès complet au fichier dont les données doivent être copiées dans la table cible.

```sql
SELECT * FROM OPENROWSET(
   BULK 'C:\DATA\inv-2017-01-19.csv',
   SINGLE_CLOB) AS DATA;
```

**S’applique à :** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
À partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1, data_file peut se trouver dans le Stockage Blob Azure. Pour obtenir des exemples, consultez [Exemples d’accès en bloc à des données dans Stockage Blob Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du Stockage Blob Azure.

#### <a name="bulk-error-handling-options"></a>Options de gestion des erreurs BULK

##### <a name="errorfile"></a>ERRORFILE
`ERRORFILE` ='*file_name*' spécifie le fichier utilisé pour collecter les lignes comportant des erreurs de mise en forme et impossibles à convertir en un ensemble de lignes OLE DB. Ces lignes sont copiées « en l'état » du fichier de données vers ce fichier d'erreur.

Le fichier d'erreur est créé au début de l'exécution de la commande. Une erreur est signalée si le fichier existe déjà. De plus, un fichier de contrôle portant l'extension .ERROR.txt est créé. Il fait référence à chacune des lignes du fichier d’erreur et propose un diagnostic. Lorsque les erreurs sont corrigées, les données peuvent être chargées.
**S’applique à :** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
À partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)], `error_file_path` peut se trouver dans Stockage Blob Azure.

##### <a name="errorfile_data_source_name"></a>ERRORFILE_DATA_SOURCE_NAME
**S’applique à :** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Source de données externe nommée pointant vers l’emplacement de Stockage Blob Azure du fichier d’erreur contenant les erreurs détectées lors de l’importation. La source de données externe doit être créée à l’aide de l’option `TYPE = BLOB_STORAGE` ajoutée dans [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1. Pour plus d’informations, consultez [CRÉER UNE SOURCE DE DONNÉES EXTERNES](../../t-sql/statements/create-external-data-source-transact-sql.md).

##### <a name="maxerrors"></a>MAXERRORS
`MAXERRORS` =*maximum_errors* spécifie le nombre maximal d’erreurs de syntaxe ou de lignes non conformes (défini dans le fichier de format) qui peuvent se produire avant qu’OPENROWSET ne lève une exception. Tant que la valeur de MAXERRORS n'est pas atteinte, OPENROWSET ignore les lignes incorrectes, ne les charge pas, et les compte comme des erreurs.

La valeur par défaut de *maximum_errors* est 10.

> [!NOTE]
> `MAX_ERRORS` ne s’applique pas aux contraintes CHECK ni à la conversion des types de données **money** et **bigint**.

#### <a name="bulk-data-processing-options"></a>Options de traitement des données BULK

##### <a name="firstrow"></a>FIRSTROW
`FIRSTROW` =*first_row* Spécifie le numéro de la première ligne à charger. La valeur par défaut est 1. Cela indique la première ligne du fichier de données spécifié. Les numéros des lignes sont déterminés en comptant les indicateurs de fin de ligne. FIRSTROW commence à 1.

##### <a name="lastrow"></a>LASTROW
`LASTROW` =*last_row* Spécifie le numéro de la dernière ligne à charger. La valeur par défaut est 0. Cela indique la dernière ligne du fichier de données spécifié.

##### <a name="rows_per_batch"></a>ROWS_PER_BATCH
`ROWS_PER_BATCH` =*rows_per_batch* Spécifie le nombre approximatif de lignes de données contenues dans le fichier de données. Cette valeur doit être du même ordre que le nombre réel de lignes.

`OPENROWSET` importe toujours un fichier de données en un seul lot. Toutefois, si vous spécifiez une valeur > 0 pour *rows_per_batch*, le processeur de requêtes se base sur la valeur de *rows_per_batch* pour allouer les ressources dans le plan de requête.

Par défaut, ROWS_PER_BATCH est inconnu. Si vous spécifiez ROWS_PER_BATCH = 0, le résultat est le même que si vous omettez ROWS_PER_BATCH.

##### <a name="order"></a>ORDER
`ORDER` ( { *column* [ ASC | DESC ] } [ ,... *n* ] [ UNIQUE ] ) Indicateur facultatif qui spécifie le mode de tri des données dans le fichier de données. Par défaut, le processus de chargement en masse considère que le fichier de données n'est pas trié. Il est possible que les performances soient améliorées si l'optimiseur de requête peut exploiter l'ordre spécifié pour générer un plan de requête plus efficace. Voici quelques exemples de situations dans lesquelles il peut être intéressant de spécifier un tri :

- Insertion de lignes dans une table qui a un index cluster, où les données d'un ensemble de lignes sont triées sur la clé d'index cluster.
- Jointure de l'ensemble de lignes avec une autre table, où les colonnes de tri et de jointure correspondent.
- Agrégation des données de l'ensemble de lignes en fonction des colonnes de tri.
- Utilisation de l'ensemble de lignes en tant que table source dans la clause FROM d'une requête, où les colonnes de tri et de jointure correspondent.

##### <a name="unique"></a>UNIQUE
`UNIQUE` spécifie que le fichier de données n’a pas d’entrées en double.

Si les lignes réelles du fichier de données ne sont pas triées d'après l'ordre spécifié ou si l'indicateur UNIQUE est spécifié et si des clés en double sont présentes, une erreur est retournée.

Les alias de colonnes sont requis lorsque ORDER est utilisé. La liste d'alias de colonnes doit référencer la table dérivée à laquelle accède la clause BULK. Les noms de colonnes qui sont spécifiés dans la clause ORDER font référence à cette liste d'alias de colonnes. Il n’est pas possible de spécifier des colonnes de types de valeur élevée (**varchar(max)** , **nvarchar(max)** , **varbinary(max)** et **xml**) et de types LOB (**text**, **ntext** et **image**).

##### <a name="single_blob"></a>SINGLE_BLOB
Retourne le contenu de *data_file* sous la forme d’un ensemble de lignes à une seule ligne et une seule colonne de type **varbinary(max)** .

> [!IMPORTANT]
> Nous vous recommandons d'importer des données XML seulement au moyen de l'option SINGLE_BLOB, au lieu de SINGLE_CLOB et SINGLE_NCLOB, parce que seule l'option SINGLE_BLOB prend en charge toutes les conversions d'encodage de Windows.

##### <a name="single_clob"></a>SINGLE_CLOB
La lecture de *data_file* au format ASCII retourne son contenu sous la forme d’un ensemble de lignes à une seule ligne et une seule colonne du type **varchar(max)** en utilisant le classement de la base de données active.

##### <a name="single_nclob"></a>SINGLE_NCLOB
La lecture de *data_file* au format UNICODE retourne son contenu sous la forme d’un ensemble de lignes à une seule ligne et une seule colonne du type **nvarchar(max)** en utilisant le classement de la base de données active.

```sql
SELECT *
   FROM OPENROWSET(BULK N'C:\Text1.txt', SINGLE_NCLOB) AS Document;
```

#### <a name="bulk-input-file-format-options"></a>Options de format de fichier d’entrée BULK

##### <a name="codepage"></a>CODEPAGE
`CODEPAGE` = { 'ACP' \| 'OEM' \| 'RAW' \| '*code_page*' } Spécifie la page de codes des données dans le fichier de données. CODEPAGE n’est justifié que si les données contiennent des colonnes de type **char**, **varchar** ou **text** dont les valeurs de caractères sont supérieures à 127 ou inférieures à 32.

> [!IMPORTANT]
> `CODEPAGE` n’est pas une option prise en charge sur Linux.

> [!NOTE]
> Nous vous recommandons de spécifier un nom de classement pour chaque colonne dans un fichier de format, sauf lorsque vous souhaitez que l’option 65001 soit prioritaire sur la spécification de page de codes/classement.

|Valeur CODEPAGE|Description|
|--------------------|-----------------|
|ACP|Convertit les colonnes de type de données **char**, **varchar** ou **text** de la page de codes ANSI/[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows (ISO 1252) à la page de codes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|OEM (valeur par défaut)|Convertit les colonnes de type de données **char**, **varchar** ou **text** de la page de codes du système OEM à la page de codes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|RAW|Aucune conversion n'a lieu d'une page de codes à une autre. Il s'agit de l'option la plus rapide.|
|*code_page*|Indique la page de codes source sur laquelle est basé l'encodage des données caractères du fichier de données, par exemple 850.<br /><br /> **Important** Les versions antérieures à la version [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ne prennent pas en charge la page de codes 65001 (encodage UTF-8).|

##### <a name="format"></a>FORMAT
`FORMAT` **=** 'CSV' **S’applique à :** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Spécifie un fichier de valeurs séparées par des virgules conforme à la norme [RFC 4180](https://tools.ietf.org/html/rfc4180).

```sql
SELECT *
FROM OPENROWSET(BULK N'D:\XChange\test-csv.csv',
    FORMATFILE = N'D:\XChange\test-csv.fmt',
    FIRSTROW=2,
    FORMAT='CSV') AS cars;
```

##### <a name="formatfile"></a>FORMATFILE
`FORMATFILE` ='*format_file_path*' Spécifie le chemin complet d’un fichier de format. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge deux types de fichiers de format : XML et non-XML.

Un fichier de format est requis pour définir les types des colonnes dans le jeu de résultats, excepté lorsque SINGLE_CLOB, SINGLE_BLOB ou SINGLE_NCLOB est spécifié ; dans ce cas, le fichier de format n'est pas requis.

Pour plus d’informations sur les fichiers de format, consultez [Utiliser un fichier de format pour importer des données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).

**S’applique à :** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
À partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1, format_file_path peut être dans Stockage Blob Azure. Pour obtenir des exemples, consultez [Exemples d’accès en bloc à des données dans Stockage Blob Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

##### <a name="fieldquote"></a>FIELDQUOTE
`FIELDQUOTE` **=** 'field_quote' **S’applique à :** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Spécifie un caractère qui sera utilisé comme caractère de guillemet dans le fichier CSV. Si vous ne spécifiez pas cet argument, le caractère guillemet (") servira de caractère guillemet tel que défini dans la norme [RFC 4180](https://tools.ietf.org/html/rfc4180).

## <a name="remarks"></a>Notes

`OPENROWSET` ne peut être utilisé pour accéder à des données distantes à partir de sources de données OLE DB uniquement si l’option de Registre **DisallowAdhocAccess** est explicitement définie sur 0 pour le fournisseur spécifié et que l’option de configuration avancée Ad Hoc Distributed Queries est activée. Lorsque ces options ne sont pas définies, le comportement par défaut n'autorise pas l'accès d'égal à égal.

Lors de l'accès à des sources de données OLE DB distantes, l'identité des connexions approuvées n'est pas automatiquement déléguée du serveur auquel le client est connecté au serveur qui est interrogé. Il est nécessaire de configurer la délégation de l'authentification.

Les noms de catalogues et de schémas sont requis si le fournisseur OLE DB prend en charge plusieurs catalogues et schémas dans la source de données spécifiée. Les valeurs de _catalog_ et )_schema_ peuvent être omises si le fournisseur OLE DB ne les prend pas en charge. Si le fournisseur prend en charge uniquement les noms de schémas, il est nécessaire de spécifier un nom en deux parties, sous la forme _schéma_ **.** _objet_. Si le fournisseur prend en charge uniquement les noms de catalogues, il est nécessaire de spécifier un nom en trois parties, sous la forme _catalogue_ **.** _schéma_ **.** _objet_. Vous devez spécifier des noms en trois parties pour les requêtes directes qui utilisent le fournisseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB. Pour plus d’informations, consultez [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

`OPENROWSET` n’accepte pas de variables pour ses arguments.

Tout appel à `OPENDATASOURCE`, `OPENQUERY` ou `OPENROWSET` dans la clause `FROM` est évalué séparément et indépendamment de tout appel à ces fonctions utilisé comme cible de la mise à jour, même si des arguments identiques sont fournis aux deux appels. En particulier, les conditions de filtre ou de jointure appliquées sur le résultat de l'un de ces appels n'ont aucun effet sur les résultats de l'autre.

## <a name="using-openrowset-with-the-bulk-option"></a>Utilisation de OPENROWSET avec l'option BULK

Les améliorations [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes prennent en charge la fonction OPENROWSET(BULK…) :

- Une clause FROM utilisée avec `SELECT` peut appeler `OPENROWSET(BULK...)` au lieu d’un nom de table, avec des fonctionnalités `SELECT` complètes.

   `OPENROWSET` utilisée avec l’option `BULK` nécessite un nom de corrélation, également baptisé variable de plage ou alias, dans la clause `FROM`. Vous pouvez définir des alias de colonnes. Si une liste d'alias de colonnes n'est pas spécifiée, le fichier de format doit comporter les noms des colonnes. La spécification des alias de colonnes remplace les noms de colonnes dans le fichier de format, par exemple :

  - `FROM OPENROWSET(BULK...) AS table_alias`
  - `FROM OPENROWSET(BULK...) AS table_alias(column_alias,...n)`

  > [!IMPORTANT]
  > Un échec d’ajout de `AS <table_alias>` génère l’erreur : Msg 491, Niveau 16, État 1, Ligne 20 Un nom de corrélation doit être spécifié pour l’ensemble de lignes en bloc dans la clause FROM.

- Une instruction `SELECT...FROM OPENROWSET(BULK...)` interroge directement les données d’un fichier, sans les importer dans une table. Les instructions `SELECT...FROM OPENROWSET(BULK...)` peuvent également énumérer les alias de colonnes en bloc en utilisant un fichier de format pour spécifier les noms de colonnes ainsi que les types de données.
- L’utilisation de `OPENROWSET(BULK...)` en tant que table source dans une instruction `INSERT` ou `MERGE` importe en bloc les données d’un fichier de données dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Importer des données en bloc à l’aide de BULK INSERT ou OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).
- Lorsque l’option `OPENROWSET BULK` est utilisée avec une instruction `INSERT`, la clause BULK prend en charge les indicateurs de table. En plus des indicateurs de table standard, comme `TABLOCK`, la clause `BULK` peut accepter les indicateurs de table spécialisés suivants : `IGNORE_CONSTRAINTS` (ignore uniquement les contraintes `CHECK` et `FOREIGN KEY`), `IGNORE_TRIGGERS`, `KEEPDEFAULTS` et `KEEPIDENTITY`. Pour plus d’informations, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).

  Pour plus d’informations sur la manière d’utiliser `INSERT...SELECT * FROM OPENROWSET(BULK...)`, consultez [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md). Pour savoir à quel moment les opérations d’insertion de ligne effectuées par l’importation en bloc sont consignées dans le journal des transactions, consultez [Conditions requises pour une journalisation minimale dans l’importation en bloc](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).

> [!NOTE]
> Lorsque vous utilisez `OPENROWSET`, il est important que vous compreniez la manière dont [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère l’emprunt d’identité. Pour plus d’informations sur la sécurité, consultez [Importer des données en bloc à l’aide de BULK INSERT ou OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

### <a name="bulk-importing-sqlchar-sqlnchar-or-sqlbinary-data"></a>L'importation en bloc de données SQLCHAR, SQLNCHAR ou SQLBINARY

OPENROWSET(BULK...) suppose que, si elle n'est pas spécifiée, la longueur maximale des données SQLCHAR, SQLNCHAR ou SQLBINARY ne dépasse pas 8 000 octets. Si les données importées figurent dans un champ de données LOB qui contient des objets **varchar(max)** , **nvarchar(max)** ou **varbinary(max)** qui dépassent 8 000 octets, vous devez utiliser un fichier de format XML qui définit la longueur maximale du champ de données. Pour spécifier la longueur maximale, modifiez le fichier de format et déclarez l'attribut MAX_LENGTH.

> [!NOTE]
> Un fichier de format généré automatiquement ne spécifie pas la longueur ou la longueur maximale d'un champ LOB. Toutefois, vous pouvez modifier un fichier de format et spécifier manuellement la longueur ou la longueur maximale.

### <a name="bulk-exporting-or-importing-sqlxml-documents"></a>Exportation et importation en bloc de documents SQLXML

Pour exporter ou importer en bloc des données SQLXML, utilisez l'un des types de données ci-dessous dans votre fichier de format.

|Type de données|Résultat|
|---------------|------------|
|SQLCHAR ou SQLVARYCHAR|Les données sont envoyées dans la page de codes du client ou dans la page de codes impliquée par le classement.|
|SQLNCHAR ou SQLNVARCHAR|Les données sont envoyées au format Unicode.|
|SQLBINARY ou SQLVARYBIN|Les données sont envoyées sans être converties.|

## <a name="permissions"></a>Autorisations

Les autorisations `OPENROWSET` sont conditionnées par les autorisations associées au nom d’utilisateur passé au fournisseur OLE DB. L’utilisation de l’option `BULK` nécessite l’autorisation `ADMINISTER BULK OPERATIONS` ou `ADMINISTER DATABASE BULK OPERATIONS`.

## <a name="examples"></a>Exemples

### <a name="a-using-openrowset-with-select-and-the-sql-server-native-client-ole-db-provider"></a>R. Utilisation de OPENROWSET avec SELECT et le fournisseur SQL Server Native Client OLE DB

L’exemple suivant utilise le fournisseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB pour accéder à la table `HumanResources.Department` de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] sur le serveur distant `Seattle1`. (L'utilisation de SQLNCLI et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vous redirigera vers la version la plus récente du fournisseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB.) Une instruction `SELECT` définit l’ensemble de lignes retourné. La chaîne de caractères du fournisseur contient les mots clés `Server` et `Trusted_Connection`. Ces mots clés sont reconnus par le fournisseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB.

```sql
SELECT a.*
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',
     'SELECT GroupName, Name, DepartmentID
      FROM AdventureWorks2012.HumanResources.Department
      ORDER BY GroupName, Name') AS a;
```

### <a name="b-using-the-microsoft-ole-db-provider-for-jet"></a>B. Utilisation du fournisseur Microsoft OLE DB pour Jet

L'exemple suivant accède à la table `Customers` de la base de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access `Northwind` via le fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour Jet.

> [!NOTE]
> L'exécution de ce code exemple suppose que Microsoft Access est installé. Pour exécuter ce code exemple, vous devez installer la base de données Northwind.

```sql
SELECT CustomerID, CompanyName
   FROM OPENROWSET('Microsoft.Jet.OLEDB.4.0',
      'C:\Program Files\Microsoft Office\OFFICE11\SAMPLES\Northwind.mdb';
      'admin';'',Customers);
```

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du Stockage Blob Azure.

### <a name="c-using-openrowset-and-another-table-in-an-inner-join"></a>C. Utilisation de OPENROWSET avec une autre table dans une jointure interne INNER JOIN

L’exemple de code suivant sélectionne toutes les données de la table `Customers` dans la base de données `Northwind` installée sur l’instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et dans la table `Orders` de la base de données `Northwind` Access stockée sur le même ordinateur.

> [!NOTE]
> L'exécution de ce code exemple suppose que Microsoft Access est installé. Pour exécuter ce code exemple, vous devez installer la base de données Northwind.

```sql
USE Northwind;
GO
SELECT c.*, o.*
FROM Northwind.dbo.Customers AS c
   INNER JOIN OPENROWSET('Microsoft.Jet.OLEDB.4.0',
   'C:\Program Files\Microsoft Office\OFFICE11\SAMPLES\Northwind.mdb';'admin';'', Orders)
   AS o
   ON c.CustomerID = o.CustomerID ;
```

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] prend uniquement en charge la lecture à partir du Stockage Blob Azure.

### <a name="d-using-openrowset-to-bulk-insert-file-data-into-a-varbinarymax-column"></a>D. Utilisation de OPENROWSET pour insérer en bloc un fichier de données dans une colonne de type varbinary(max)

 Le code exemple suivant crée une petite table aux fins de démonstration et insère dans une colonne `Text1.txt` les données du fichier `C:` situé dans le répertoire racine `varbinary(max)`.

```sql
CREATE TABLE myTable(FileName NVARCHAR(60),
  FileType NVARCHAR(60), Document VARBINARY(max));
GO

INSERT INTO myTable(FileName, FileType, Document)
   SELECT
      'Text1.txt' AS FileName
      , '.txt' AS FileType
      , *
   FROM OPENROWSET(BULK N'C:\Text1.txt', SINGLE_BLOB) AS Document;
GO
```

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du Stockage Blob Azure.

### <a name="e-using-the-openrowset-bulk-provider-with-a-format-file-to-retrieve-rows-from-a-text-file"></a>E. Utilisation du fournisseur OPENROWSET BULK avec un fichier de format pour récupérer des lignes dans un fichier texte

Le code exemple suivant utilise un fichier de format pour extraire des lignes d'un fichier texte dont les données sont délimitées par des tabulations, `values.txt`, qui contient les données suivantes :

```
1     Data Item 1
2     Data Item 2
3     Data Item 3
```

Le fichier de format, `values.fmt`, décrit les colonnes du fichier `values.txt` :

```
9.0
2  
1  SQLCHAR  0  10 "\t"        1  ID                      SQL_Latin1_General_Cp437_BIN
2  SQLCHAR  0  40 "\r\n"      2  Description        SQL_Latin1_General_Cp437_BIN
```

Voici la requête qui récupère ces données :

```sql
SELECT a.* FROM OPENROWSET( BULK 'c:\test\values.txt',
   FORMATFILE = 'c:\test\values.fmt') AS a;
```

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du Stockage Blob Azure.

### <a name="f-specifying-a-format-file-and-code-page"></a>F. Spécification d’un fichier de format et d’une page de codes

L’exemple suivant montre comment utiliser à la fois les options de fichier de format et de page de codes en même temps.

```sql
INSERT INTO MyTable SELECT a.* FROM
OPENROWSET (BULK N'D:\data.csv', FORMATFILE =
    'D:\format_no_collation.txt', CODEPAGE = '65001') AS a;
```

### <a name="g-accessing-data-from-a-csv-file-with-a-format-file"></a>G. Accès aux données d’un fichier CSV avec un fichier de format

**S’applique à :** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.

```sql
SELECT *
FROM OPENROWSET(BULK N'D:\XChange\test-csv.csv',
    FORMATFILE = N'D:\XChange\test-csv.fmt',
    FIRSTROW=2,
    FORMAT='CSV') AS cars;
```

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] prend uniquement en charge la lecture à partir du Stockage Blob Azure.

### <a name="h-accessing-data-from-a-csv-file-without-a-format-file"></a>H. Accès aux données d’un fichier CSV sans fichier de format

```sql
SELECT * FROM OPENROWSET(
   BULK 'C:\Program Files\Microsoft SQL Server\MSSQL14.CTP1_1\MSSQL\DATA\inv-2017-01-19.csv',
   SINGLE_CLOB) AS DATA;
```

```sql
SELECT *
FROM OPENROWSET
   (  'MSDASQL'
     ,'Driver={Microsoft Access Text Driver (*.txt, *.csv)}'
     ,'select * from E:\Tlog\TerritoryData.csv')
;
```

> [!IMPORTANT]
>
> - Le pilote ODBC doit être 64 bits. Ouvrez l’onglet **Pilotes** de l’application [Sources de données ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md) dans Windows pour vérifier cela. Il y a un `Microsoft Text Driver (*.txt, *.csv)` 32 bits qui ne fonctionne pas avec une version 64 bits de sqlservr.exe.
> - Azure SQL Database prend uniquement en charge la lecture à partir du Stockage Blob Azure.

### <a name="i-accessing-data-from-a-file-stored-on-azure-blob-storage"></a>I. Accès aux données d’un fichier stocké sur Stockage Blob Azure

**S’applique à :** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
L’exemple suivant utilise une source de données externe qui pointe vers un conteneur dans un compte de stockage Azure et des informations d’identification délimitées à la base de données créées pour une signature d’accès partagé.

```sql
SELECT * FROM OPENROWSET(
   BULK 'inv-2017-01-19.csv',
   DATA_SOURCE = 'MyAzureInvoices',
   SINGLE_CLOB) AS DataFile;
```

Pour obtenir des exemples `OPENROWSET` complets, illustrant notamment la configuration des informations d’identification et de la source de données externe, consultez [Exemples d’accès en bloc à des données dans Stockage Blob Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

### <a name="j-importing-into-a-table-from-a-file-stored-on-azure-blob-storage"></a>J. Importation dans une table à partir d’un fichier stocké dans le stockage Blob Azure

L’exemple suivant montre comment utiliser la commande OPENROWSET pour charger des données à partir d’un fichier CSV dans un emplacement de stockage Blob Azure sur lequel vous avez créé une clé SAS. L’emplacement du stockage Blob Azure est configuré comme source de données externe. Ceci nécessite des informations d’identification délimitées à la base de données avec une signature d’accès partagé chiffrée à l’aide d’une clé principale dans la base de données utilisateur.

```sql
--> Optional - a MASTER KEY is not required if a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'YourStrongPassword1';
GO
--> Optional - a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE DATABASE SCOPED CREDENTIAL MyAzureBlobStorageCredential
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************';

 -- NOTE: Make sure that you don't have a leading ? in SAS token, and
 -- that you have at least read permission on the object that should be loaded srt=o&sp=r, and
 -- that expiration period is valid (all dates are in UTC time)

CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
WITH ( TYPE = BLOB_STORAGE,
          LOCATION = 'https://****************.blob.core.windows.net/curriculum'
          , CREDENTIAL= MyAzureBlobStorageCredential --> CREDENTIAL is not required if a blob is configured for public (anonymous) access!
);

INSERT INTO achievements with (TABLOCK) (id, description)
SELECT * FROM OPENROWSET(
   BULK  'csv/achievements.csv',
   DATA_SOURCE = 'MyAzureBlobStorage',
   FORMAT ='CSV',
   FORMATFILE='csv/achievements-c.xml',
   FORMATFILE_DATA_SOURCE = 'MyAzureBlobStorage'
    ) AS DataFile;
```

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du Stockage Blob Azure à l’aide d’un jeton SAS.

### <a name="additional-examples"></a>Autres exemples

Pour obtenir des exemples supplémentaires illustrant l’utilisation de `INSERT...SELECT * FROM OPENROWSET(BULK...)`, consultez les rubriques suivantes :

- [Exemples d’importation et d’exportation en bloc de documents XML &#40;SQL Server&#41;](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [Conserver des valeurs d’identité lors de l’importation de données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)
- [Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l’importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)
- [Utiliser un fichier de format pour importer des données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)
- [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)
- [Utiliser un fichier de format pour mapper les colonnes d’une table sur les champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>Voir aussi

- [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)
- [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)
- [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md)
- [OPENQUERY &#40;Transact-SQL&#41;](../../t-sql/functions/openquery-transact-sql.md)
- [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)
- [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)
- [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)
- [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)
- [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)
