---
description: 'Requêtes distribuées Microsoft SQL Server : connectivité OLE DB'
title: Créer un fournisseur de serveurs liés
ms.date: 07/01/2019
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: MikeRayMSFT
ms.topic: conceptual
author: pmasl
ms.author: pelopes
manager: rothj
ms.custom: seo-dt-2019
ms.openlocfilehash: 75134031d7567dcc5a653d28ecb1c92a26891d23
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080410"
---
# <a name="microsoft-sql-server-distributed-queries-ole-db-connectivity"></a>Requêtes distribuées Microsoft SQL Server : connectivité OLE DB

Cet article décrit comment le processeur de requêtes Microsoft SQL Server interagit avec un fournisseur OLE DB pour activer des requêtes distribuées et hétérogènes. Il concerne principalement les développeurs de fournisseurs OLE DB et suppose de parfaitement comprendre la spécification OLE DB. L’accent est mis sur l’interface OLE DB entre le processeur de requêtes SQL Server et le fournisseur OLE DB, et non sur la fonctionnalité de requête distribuée elle-même. Pour obtenir une description complète de la fonctionnalité de requête distribuée, consultez [Serveurs liés](../../relational-databases/linked-servers/linked-servers-database-engine.md).

## <a name="overview-and-terminology"></a>Vue d’ensemble et terminologie

 Dans Microsoft SQL Server, les requêtes distribuées permettent à des utilisateurs SQL Server d’accéder à des données situées hors d’un serveur SQL Server, au sein d’autres serveurs exécutant SQL Server ou d’autres sources de données qui exposent une interface OLE DB. OLE DB offre un moyen d’accéder uniformément aux données tabulaires à partir de sources de données hétérogènes.

Une requête distribuée, dans le cadre de cet article, correspond à toute instruction `SELECT`, `INSERT`, `UPDATE` ou `DELETE` qui fait référence à des tables et des ensembles de lignes à partir d’une ou de plusieurs sources de données OLE DB externes.

Une table distante est une table qui est stockée dans une source de données OLE DB et qui est externe au serveur fonctionnant sous SQL Server exécutant la requête. Une requête distribuée accède à une ou plusieurs tables distantes.

### <a name="ole-db-provider-categories"></a>Catégories de fournisseurs OLE DB

Voici une catégorisation des fournisseurs OLE DB en fonction de leurs fonctionnalités d’un point de vue des requêtes distribuées SQL Server. Tels qu’ils sont définis, ceux-ci ne s’excluent pas mutuellement ; un fournisseur donné peut appartenir à plusieurs des catégories suivantes :

- Fournisseurs de commandes SQL

- Fournisseurs d’index

- Fournisseurs de tables simples

- Fournisseurs de commandes non-SQL

#### <a name="sql-command-providers"></a>Fournisseurs de commandes SQL

Les fournisseurs qui prennent en charge l’objet `Command` avec un dialecte standard SQL reconnu par SQL Server appartiennent à cette catégorie. Les exigences spécifiques pour un fournisseur OLE DB donné à traiter en tant que fournisseur de commandes SQL par SQL Server sont les suivantes :

- Le fournisseur doit prendre en charge l’objet `Command` et toutes ses interfaces OLE DB obligatoires : `ICommand`, `ICommandText`, `IColumnsInfo`, `ICommandProperties` et `IAccessor`.

- Le dialecte SQL pris en charge par le fournisseur doit être au moins SQL Subminimum. Le dialecte doit être signalé par le fournisseur par le biais de la propriété `DBPROP_SQLSUPPORT`.

Le fournisseur Microsoft OLE DB pour SQL Server et le fournisseur Microsoft OLE DB pour ODBC sont des exemples de fournisseurs de commandes SQL.

#### <a name="index-providers"></a>Fournisseurs d’index

Les fournisseurs d’index sont ceux qui prennent en charge et exposent des index en fonction d’OLE DB et qui permettent une recherche basée sur les index des tables de base. Les exigences spécifiques pour un fournisseur OLE DB donné à traiter en tant que fournisseur d’index par SQL Server sont les suivantes :

- Le fournisseur doit prendre en charge l’interface `IDBSchemaRowset` avec les ensembles de lignes de schémas TABLES, COLUMNS et INDEXES.

- Le fournisseur doit prendre en charge l’ouverture d’un ensemble de lignes sur un index via `IOpenRowset` en spécifiant le nom d’index et le nom de la table de base correspondante.

- L’objet `Index` doit prendre en charge toutes ses interfaces obligatoires : `IRowset`, `IRowsetIndex`, `IAccessor`, `IColumnsInfo`, `IRowsetInfo` et `IConvertTypes`.

- Les ensembles de lignes ouverts dans la table de base indexée (via `IOpenRowset`) doivent prendre en charge l’interface `IRowsetLocate` pour le positionnement sur une ligne en fonction d’un signet.

Si le fournisseur OLE DB répond aux exigences ci-dessus, les utilisateurs peuvent définir l’option de fournisseur `Index As Access Path` pour permettre à SQL Server d’utiliser les index du fournisseur pour évaluer des requêtes. Par défaut, SQL Server n’essaie pas d’utiliser les index du fournisseur à moins que cette option ne soit définie.

>[!NOTE]
>SQL Server prend en charge différentes options qui influencent la manière dont SQL Server accède à un fournisseur OLE DB. La boîte de dialogue `Linked Server Properties` dans SQL Server Enterprise Manager peut être utilisée pour définir ces options.

#### <a name="simple-table-providers"></a>Fournisseurs de tables simples

Il s’agit de fournisseurs qui exposent l’ouverture d’un ensemble de lignes dans une table de base par le biais de l’interface `IOpenRowset`. Ces fournisseurs ne sont ni des fournisseurs de commandes SQL ni des fournisseurs d’index ; au lieu de cela, il s’agit de la classe la plus simple de fournisseurs avec lesquels les requêtes distribuées SQL Server peuvent fonctionner.

Avec ces fournisseurs, SQL Server peut uniquement effectuer des analyses de table pendant l’évaluation de requêtes distribuées.

#### <a name="non-sql-command-providers"></a>Fournisseurs de commandes non-SQL

Les fournisseurs qui prennent en charge l’objet `Command` et ses interfaces obligatoires, mais qui ne prennent pas en charge un dialecte standard SQL reconnu par SQL Server, appartiennent à cette catégorie.

Le fournisseur Microsoft OLE DB pour le service d’indexation et le [fournisseur Microsoft OLE DB pour le service Microsoft Active Directory](../../ado/guide/appendixes/microsoft-ole-db-provider-for-microsoft-active-directory-service.md) sont deux exemples de fournisseurs de commandes non-SQL.

### <a name="transact-sql-subset"></a>Sous-ensemble Transact-SQL

Chacune des classes d’instructions Transact-SQL suivantes est prise en charge pour les requêtes distribuées si le fournisseur prend en charge les interfaces OLE DB requises.

- Toutes les instructions `SELECT` sont autorisées à l’exception des instructions `SELECT` INTO avec une table distante comme table de destination.

- Les instructions `INSERT` sont autorisées pour les tables distantes si le fournisseur prend en charge les interfaces requises pour INSERT. Pour plus d’informations sur les exigences OLE DB applicables à INSERT, consultez \"Instruction INSERT\", plus loin dans cet article.

- Les instructions `UPDATE` et DELETE sont autorisées pour les tables distantes si le fournisseur respecte les exigences de l’interface OLE DB applicables à la table spécifiée. Pour connaître les exigences de l’interface OLE DB et les conditions sous lesquelles une table distante peut être mise à jour ou supprimée, consultez \"Instructions UPDATE et DELETE\", plus loin dans cet article.

### <a name="cursor-support"></a>Prise en charge des curseurs

Les curseurs d’instantané et de jeu de clés sont pris en charge sur les requêtes distribuées si le fournisseur prend en charge les fonctionnalités OLE DB nécessaires. Les curseurs dynamiques ne sont pas pris en charge sur les requêtes distribuées. Une demande d’un utilisateur pour un curseur dynamique sur une requête distribuée est rétrogradée en curseur de jeu de clés.

Les curseurs d’instantané sont remplis au moment de l’ouverture du curseur et le jeu de résultats reste inchangé. Les mises à jour, insertions et suppressions effectuées dans les tables sous-jacentes ne sont pas reflétées dans le curseur.

Les curseurs de jeu de clés sont remplis au moment de l’ouverture du curseur et le jeu de résultats reste inchangé pendant toute la durée de vie du curseur. Toutefois, les mises à jour et les suppressions effectuées dans les tables sous-jacentes sont visibles dans le curseur à mesure que les lignes sont examinées. Les insertions dans les tables sous-jacentes qui peuvent affecter l’appartenance du curseur ne sont pas visibles.

Une table distante peut être mise à jour ou supprimée par l’intermédiaire d’un curseur défini sur une requête distribuée et qui référence la table distante si le fournisseur remplit les conditions nécessaires aux mises à jour et suppressions de la table distante, par exemple table `UPDATE` \| DELETE `<remote-table>` `WHERE` CURRENT OF `<cursor-name>`. Pour plus d’informations, consultez \"Instructions UPDATE et DELETE\", plus loin dans cet article.

#### <a name="keyset-cursor-support-requirements"></a>Exigences de prise en charge des curseurs de jeu de clés

Un curseur de jeu de clés est pris en charge sur une requête distribuée si toutes les exigences de la syntaxe Transact-SQL sont satisfaites et que l’une des conditions suivantes est remplie :

- Le fournisseur OLE DB prend en charge les signets réutilisables sur toutes les tables distantes dans la requête. Les signets réutilisables peuvent être consommés à partir d’un ensemble de lignes sur une table donnée et utilisés sur un autre ensemble de lignes de la même table. La prise en charge des signets réutilisables est indiquée par l’ensemble de lignes de schéma TABLES_INFO d’`IDBSchemaRowset` en affectant à la colonne BOOKMARK_DURABILITY la valeur BMK_DURABILITY_INTRANSACTION ou une durabilité supérieure.

- Toutes les tables distantes exposent une clé unique par le biais de l’ensemble de lignes INDEXES de l’interface `IDBSchemaRowset`. Il doit y avoir une entrée d’index avec la colonne UNIQUE définie sur VARIANT_TRUE.

Les curseurs de jeu de clés ne sont pas pris en charge sur les requêtes distribuées qui impliquent la fonction *OpenQuery*.

#### <a name="updatable-keyset-cursor-requirements"></a>Éléments requis pour la mise à jour des curseurs pilotés par jeu de clés

Une table distante peut être mise à jour ou supprimée au moyen d’un curseur de jeu de clés défini sur une requête distribuée, par exemple `UPDATE` \| DELETE `<remote-table>` `WHERE` CURRENT OF `<cursor-name>`. Voici les conditions dans lesquelles les curseurs pouvant être mis à jour sur des requêtes distribuées sont autorisés :

- Les curseurs pouvant être mis à jour sont autorisés si le fournisseur remplit également les conditions pour les mises à jour et les suppressions sur la table distante. Pour plus d’informations, consultez \"Instructions UPDATE et DELETE\", plus loin dans cet article.

- Toutes les opérations des curseurs de jeu de clés pouvant être mis à jour doivent avoir lieu dans une transaction définie par l’utilisateur avec un niveau d’isolement renouvelable en lecture ou supérieur. En outre, le fournisseur doit prendre en charge les transactions distribuées avec l’interface `ITransactionJoin`.

## <a name="ole-db-provider-interaction-phases"></a>Phases d’interaction du fournisseur OLE DB

 Six opérations sont communes à tous les scénarios d’exécution de requêtes distribuées :

- Les opérations d’établissement de la connexion et de récupération de la propriété indiquent comment SQL Server se connecte à un fournisseur OLE DB et quelles propriétés du fournisseur sont utilisées.

- Les opérations de résolution de noms de table et de récupération de métadonnées indiquent comment SQL Server résout le nom de la table distante (qui est spécifié de l’une des deux façons suivantes : un nom basé sur un serveur lié ou un nom ad hoc) dans l’objet de données approprié du fournisseur. Cela comprend également les métadonnées de table que SQL Server récupère à partir du fournisseur afin de compiler et d’optimiser une requête distribuée.

- Les opérations de gestion des transactions spécifient toutes les interactions relatives aux transactions avec le fournisseur OLE DB.

- Les opérations de gestion des types de données indiquent la façon dont les types de données OLE DB sont gérés par SQL Server quand il utilise des données du fournisseur OLE DB ou en exporte vers celui-ci lors du traitement d’une requête distribuée.

- Les opérations de gestion des erreurs indiquent comment SQL Server utilise les informations détaillées sur les erreurs du fournisseur.

- Les opérations de sécurité spécifient comment la sécurité de SQL Server interagit avec la sécurité du fournisseur.

### <a name="connection-establishment-and-property-retrieval"></a>Établissement de connexion et récupération de propriété

SQL Server prend en charge deux conventions de nommage d’objets de données distants : les noms en quatre parties basés sur un serveur lié et les noms ad hoc avec la fonction `OPENROWSET`.

#### <a name="linked-server-based-names"></a>Noms basés sur un serveur lié

Un serveur lié sert d’abstraction à une source de données OLE DB. Un nom basé sur un serveur lié est un nom en quatre parties sous la forme `<linked-server>.<catalog>`. `<schema>.<object>`, où `<linked-server>` est le nom du serveur lié. SQL Server interprète `<linked-server>` pour dériver le fournisseur OLE DB et les attributs de connexion qui identifient la source de données auprès du fournisseur. Les trois autres parties du nom sont interprétées par la source de données OLE DB pour identifier la table distante spécifique. :::

#### <a name="ad-hoc-names"></a>Noms ad hoc

Un nom ad hoc est un nom basé sur la fonction `OPENROWSET` ou `OPENDATASOURCE`. Il comprend toutes les informations de connexion (autrement dit, le fournisseur OLE DB à utiliser, les attributs nécessaires pour identifier la source de données, l’ID d’utilisateur et le mot de passe) chaque fois que la table distante est référencée dans une requête distribuée.

L’utilisation de noms ad hoc n’est pas autorisée par défaut, à l’exception des membres du rôle sysadmin. Pour pouvoir utiliser des noms ad hoc sur un fournisseur OLE DB, l’option de fournisseur `DisallowAdhocAccess` doit avoir la valeur `0`.

Si un nom de serveur lié est utilisé, SQL Server extrait de la définition du serveur lié le nom du fournisseur OLE DB et les propriétés d’initialisation du fournisseur. Si un nom ad hoc est utilisé, SQL Server extrait les mêmes informations à partir des arguments de la fonction `OPENROWSET`.

Pour obtenir des instructions détaillées sur la configuration d’un serveur lié à l’aide d’une syntaxe basée sur un nom en quatre parties et sur un nom ad hoc, consultez [Créer des serveurs liés](create-linked-servers-sql-server-database-engine.md).

### <a name="connecting-to-an-ole-db-provider"></a>Connexion à un fournisseur OLE DB

Voici les principales étapes que SQL Server effectue lorsqu’il se connecte à un fournisseur OLE DB :

1. SQL Server crée un objet source de données.

   SQL Server utilise l’identificateur `ProgID` du fournisseur pour instancier son objet source de données. L’identificateur ProgID est spécifié en tant que paramètre `provider_name` d’une configuration de serveur lié ou en tant que premier argument de la fonction `OPENROWSET` dans le cas d’un nom ad hoc.

   SQL Server instancie l’objet source de données du fournisseur par le biais de l’interface du composant de service OLE DB `IDataInitialize`. Cela permet au gestionnaire des composants du service d’agréger ses services, tels que le défilement et la prise en charge des mises à jour, au-delà des fonctionnalités natives du fournisseur. En outre, l’instanciation du fournisseur via `IDataInitialize` permet au composant de service OLE DB de regrouper les connexions au niveau du fournisseur, réduisant ainsi une partie de la surcharge de connexion et d’initialisation.

   Un fournisseur donné peut être configuré pour être instancié dans le même processus que SQL Server ou dans son propre processus. L’instanciation dans un processus séparé protège le processus SQL Server des défaillances du fournisseur. En même temps, une surcharge au niveau des performances est associée au marshaling des appels OLE DB out-of-process à partir de SQL Server. Un fournisseur peut être configuré pour être instancié in-process ou out-of-process en définissant l’option de fournisseur `Allow In Process`. Pour plus d’informations, consultez [Définition des options du fournisseur](../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md).

   Pour en savoir plus sur les composants du service OLE DB et le regroupement de sessions, consultez la documentation OLE DB afin de connaître la configuration requise pour le fournisseur.

2. La source de données est initialisée.

   Une fois l’objet source de données créé, l’interface `IDBProperties` définit la propriété d’initialisation DBPROP_INIT_TIMEOUT si l’option de configuration de serveur `remote login timeout` est supérieure à 0. il s’agit d’une propriété obligatoire.

   Ces propriétés sont définies si elles sont spécifiées ou implicites dans la définition du serveur lié ou dans le deuxième argument de la fonction `OPENROWSET` :

   - `DBPROP_INIT_PROVIDERSTRING`

   - `DBPROP_INIT_DATASOURCE`

   - `DBPROP_INIT_LOCATION`

   - `DBPROP_INIT_CATALOG`

   - `DBPROP_AUTH_USERID`

   - `DBPROP_AUTH_PASSWORD`

   Une fois ces propriétés définies, `IDBInitialize::Initialize` est appelée pour initialiser l’objet source de données avec les propriétés spécifiées.

3. SQL Server collecte des informations spécifiques au fournisseur.

   SQL Server collecte plusieurs propriétés de fournisseur à utiliser dans l’évaluation de requêtes distribuées ; ces propriétés sont récupérées en appelant `IDBProperties::GetProperties`. Toutes ces propriétés sont facultatives. Toutefois, la prise en charge de toutes les propriétés pertinentes permet à SQL Server de tirer pleinement parti des fonctionnalités du fournisseur. Par exemple, `DBPROP_SQLSUPPORT` est nécessaire pour déterminer si SQL Server peut envoyer des requêtes au fournisseur. Si cette propriété n’est pas prise en charge, SQL Server n’utilise pas le fournisseur distant en tant que fournisseur de commandes SQL, même s’il s’agit d’un fournisseur de ce type. Dans le tableau suivant, la colonne Valeur par défaut indique la valeur que SQL Server suppose si le fournisseur ne prend pas en charge la propriété.

Propriété| Valeur par défaut| Utilisation |
|:----|:----|:----|
|`DBPROP_DBMSNAME`|Aucun|Utilisée pour les messages d’erreur.|
|`DBPROP_DBMSVER` |Aucun|Utilisée pour les messages d’erreur.|
|`DBPROP_PROVIDERNAME`|Aucun|Utilisée pour les messages d’erreur.|
|`DBPROP_PROVIDEROLEDBVER1`|1.5|Utilisée pour déterminer la disponibilité des fonctionnalités 2.0.
|`DBPROP_CONCATNULLBEHAVIOR`|Aucun|Utilisée pour déterminer si le comportement de concaténation `NULL` du fournisseur est le même que celui de SQL Server.|
|`DBPROP_NULLCOLLATION`|Aucun|Autorise l’utilisation du tri/de l’index uniquement si `NULLCOLLATION` correspond au comportement de classement null de l’instance SQL Server.|
|`DBPROP_OLEOBJECTS`|Aucun|Détermine si le fournisseur prend en charge les interfaces de stockage structuré pour les colonnes d’objets de données volumineux.|
|`DBPROP_STRUCTUREDSTORAGE`|Aucun|Identifie les interfaces de stockage structuré prises en charge pour les types d’objets volumineux (parmi `ILockBytes`, `Istream` et `ISequentialStream`).|
|`DBPROP_MULTIPLESTORAGEOBJECTS`|Faux|Détermine si plusieurs colonnes d’objets volumineux peuvent être ouvertes en même temps.|
|`DBPROP_SQLSUPPORT`|Aucun|Détermine si les requêtes SQL peuvent être envoyées au fournisseur.|
|`DBPROP_CATALOGLOCATION`|`DBPROPVAL_CL_START`|Utilisée pour construire des noms de table en plusieurs parties.
|`SQLPROP_DYNAMICSQL`|Faux|Propriété spécifique à SQL Server : si elle retourne `VARIANT_TRUE`, elle indique que les marqueurs de paramètres `?` sont pris en charge pour l’exécution des requêtes paramétrables.
|`SQLPROP_NESTEDQUERIES`|Faux|Propriété spécifique à SQL Server : si elle retourne `VARIANT_TRUE`, elle indique que le fournisseur prend en charge les instructions `SELECT` imbriquées dans la clause `FROM`.
|`SQLPROP_GROUPBY`|Faux|Propriété spécifique à SQL Server : si elle retourne `VARIANT_TRUE`, elle indique que le fournisseur prend en charge la clause GROUP BY dans l’instruction `SELECT` comme indiqué par la norme SQL-92.
|`SQLPROP_DATELITERALS `|Faux|Propriété spécifique à SQL Server : si elle retourne `VARIANT_TRUE`, elle indique que le fournisseur prend en charge les littéraux datetime conformément à la syntaxe SQL Server Transact-SQL.
|`SQLPROP_ANSILIKE `|Faux|Propriété spécifique à SQL Server : cette propriété présente un intérêt pour un fournisseur qui prend en charge le niveau SQL-Minimum et l’opérateur `LIKE` conformément au niveau d’entrée SQL-92 (\'%\' et \'_\' comme caractères génériques).
|`SQLPROP_SUBQUERIES `|Faux|Propriété SQL Server : Cette propriété présente un intérêt dans un fournisseur qui prend en charge le niveau SQL-Minimum. Cette propriété indique que le fournisseur prend en charge les sous-requêtes comme indiqué par le niveau d’entrée SQL-92. Cela comprend les sous-requêtes dans la liste `SELECT` et dans la clause `WHERE` avec prise en charge des sous-requêtes corrélées ainsi que des opérateurs `IN`, `EXISTS`, `ALL` et `ANY`.
|`SQLPROP_INNERJOIN`|Faux|Propriété spécifique à SQL Server : Cette propriété présente un intérêt pour les fournisseurs qui prennent en charge le niveau SQL-Minimum. Cette propriété indique la prise en charge des jointures avec plusieurs tables dans la clause `FROM`. ------ ---

Les trois littéraux suivants sont récupérés d’`IDBInfo::GetLiteralInfo` : `DBLITERAL_CATALOG_SEPARATOR`, `DBLITERAL_SCHEMA_SEPARATOR` (pour construire un nom d’objet complet en fonction de ses parties catalogue, schéma et nom d’objet) et `DBLITERAL_QUOTE` (pour délimiter les noms d’identificateur dans une requête SQL envoyée au fournisseur).

Si le fournisseur ne prend pas en charge les littéraux de séparateur, SQL Server utilise un point (.) comme caractère de séparation par défaut. Si le fournisseur prend en charge uniquement le caractère de séparation de catalogue, mais pas le caractère de séparation de schéma, SQL Server utilise également le caractère de séparation de catalogue comme caractère de séparation de schéma. Si le fournisseur ne prend pas en charge `DBLITERAL_QUOTE`, SQL Server utilise un guillemet simple (`'`) comme caractère de délimitation.

>[!NOTE]
>Si les littéraux de séparateur de nom du fournisseur ne correspondent pas à ces valeurs par défaut, le fournisseur doit les exposer via `IDBInfo` pour que SQL Server accède à ses tables par le biais de noms en quatre parties. Si ces littéraux ne sont pas exposés, seules les requêtes directes peuvent être utilisées avec ce type de fournisseur.

Pour plus d’informations sur l’exposition des propriétés `SQLPROP_DYNAMICSQL` et `SQLPROP_NESTEDQUERIES`, consultez [Propriétés spécifiques à SQL Server](#appendixc).

### <a name="table-name-resolution-and-meta-data-retrieval"></a>Résolution de noms de table et récupération de métadonnées

SQL Server résout un nom de table distante donné dans une requête distribuée en une table ou une vue spécifique dans une source de données OLE DB. Les schémas de nommage ad hoc et basé sur un serveur lié entraînent l’interprétation d’un nom en trois parties par le fournisseur. Dans le cas du nom basé sur un serveur lié, les trois dernières parties du nom en quatre parties forment le catalogue, le schéma et les noms d’objet. Dans le cas du nom ad hoc, le troisième argument de la fonction `OPENROWSET` spécifie un nom en trois parties qui décrit le catalogue, le schéma et les noms d’objet. Un des noms de catalogue et de schéma (ou les deux) peut être vide. (Un nom en quatre parties avec un nom de schéma et un nom de catalogue vide doit ressembler à `<server-name>...<object-name>`.) Dans ce cas, SQL Server utilise `NULL` comme valeur correspondante à rechercher dans les tables d’ensembles de lignes de schémas.

Les règles de résolution de noms et les étapes de récupération de métadonnées employées par SQL Server varient selon que le fournisseur prend en charge l’interface `IDBSchemaRowset` sur l’objet `Session`.

Si l’interface `IDBSchemaRowset` est prise en charge, les ensembles de lignes de schémas `TABLES`, `COLUMNS`, `INDEXES` et `TABLES_INFO` sont utilisés à partir de l’interface `IDBSchemaRowset`. (L’ensemble de lignes de schéma `TABLES_INFO` est défini dans OLE DB 2.0.) SQL Server empêche les ensembles de lignes de schémas retournés par l’interface `IDBSchemaRowset` de rechercher les lignes de schémas qui correspondent aux parties du nom de la table distante spécifiée. La section suivante présente les règles relatives aux restrictions prises en charge par le fournisseur sur les ensembles de lignes de schémas et la façon dont SQL Server les utilise pour récupérer les métadonnées d’une table distante :

- Les restrictions sur les colonnes `TABLE_NAME` et `COLUMN_NAME` sont toujours requises.

- Si le fournisseur prend en charge une restriction sur `TABLE_CATALOG` (ou `TABLE_SCHEMA`), SQL Server utilise cette restriction sur `TABLE_CATALOG` (ou `TABLE_SCHEMA`). Si le nom du catalogue (ou schéma) n’est pas spécifié dans le nom de la table distante, une valeur `NULL` est utilisée comme valeur de restriction correspondante. Si un nom de catalogue (ou schéma) est spécifié, le fournisseur doit prendre en charge la restriction correspondante sur `TABLE_CATALOG` (ou `TABLE_SCHEMA`).

- Le fournisseur doit prendre en charge la restriction sur la colonne `TABLE_SCHEMA` à la fois dans `TABLES` et `COLUMNS`, ou dans aucun des deux. Le fournisseur doit prendre en charge la restriction de nom de catalogue sur les deux ensembles de lignes `TABLES` et `COLUMNS` ou sur aucun des deux.

- Si des restrictions sont prises en charge sur INDEXES, le fournisseur doit prendre en charge la restriction de schéma sur les deux ensembles de lignes `TABLES` et INDE`XES or support them on neither. The provider must either support catalog name restriction on both `TABLES` and `INDEXES, ou sur aucun des deux.

À partir de l’ensemble de lignes de schéma `TABLES`, SQL Server récupère les colonnes `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `TABLE_TYPE`, `TABLE_GUID` en définissant des restrictions selon les règles ci-dessus.

À partir de l’ensemble de lignes de schéma `COLUMNS`, SQL Server récupère les colonnes `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `COLUMN_NAME`, `COLUMN_GUID`, `ORDINAL_POSITION`, `COLUMN_FLAGS`, `IS_NULLABLE`, `DATA_TYPE`, `TYPE_GUID`, `CHARACTER_MAXIMUM_LENGTH`, `NUMERIC_PRECISION` et `NUMERIC_SCALE`. `COLUMN_NAME`, `DATA_TYPE` et `ORDINAL_POSITION` doivent retourner des valeurs non Null valides. Si `DATA_TYPE` est `DBTYPE_NUMERIC` ou `DBTYPE_DECIMAL`, les valeurs `NUMERIC_PRECISION` et `NUMERIC_SCALE` correspondantes doivent être des valeurs non Null valides.

À partir de l’ensemble de lignes de schéma `INDEXES` facultatif, SQL Server recherche des index sur la table distante spécifiée en définissant des restrictions conformément aux règles précédentes. À partir des entrées d’index correspondantes ainsi trouvées, SQL Server récupère les colonnes `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `INDEX_CATALOG`, `INDEX_SCHEMA`, `INDEX_NAME`, `PRIMARY_KEY`, `UNIQUE`, `CLUSTERED`, `FILL_FACTOR`, `ORDINAL_POSITION`, `COLUMN_NAME`, `COLLATION`, `CARDINALITY` et `PAGES`.

À partir de l’ensemble de lignes `TABLES_INFO` facultatif, SQL Server recherche des informations supplémentaires sur la table distante spécifiée, telles que la prise en charge des signets, le type et la longueur du signet. Toutes les colonnes à l’exception de la colonne `DESCRIPTION` de l’ensemble de lignes `TABLES_INFO` sont utilisées. Les informations de l’ensemble de lignes `TABLES_INFO` sont utilisées comme suit :

- La colonne `BOOKMARK_DURABILITY` est utilisée pour implémenter des curseurs de jeu de clés plus efficaces. Si cette colonne a la valeur `BMK_DURABILITY_INTRANSACTION` ou une valeur de durabilité supérieure, SQL Server utilise la récupération basée sur les signets et les mises à jour des lignes de la table distante pour l’implémentation d’un curseur de jeu de clés.

- Les colonnes `BOOKMARK_TYPE`, `BOOKMARK_DATA TYPE` et `BOOKMARK_MAXIMUM_LENGTH` sont utilisées pour déterminer les métadonnées de signet au moment de la compilation de la requête. Si ces colonnes ne sont pas prises en charge, SQL Server ouvre l’ensemble de lignes de la table de base via `IOpenRowset` pendant la compilation pour récupérer les informations de signet.

Si l’interface `IDBSchemaRowset` n’est pas prise en charge et que le nom de la table distante contient un nom de catalogue ou de schéma, SQL Server requiert `IDBSchemaRowset` et retourne une erreur. Toutefois, si ni le nom de catalogue ni le nom de schéma ne sont fournis, SQL Server ouvre l’ensemble de lignes qui correspond à la table distante et récupère les métadonnées de colonne à partir de l’interface `IColumnsInfo` obligatoire de l’objet rowset.

SQL Server ouvre l’ensemble de lignes correspondant à la table en appelant `IOpenRowset::OpenRowset`. Le nom de table fourni à `OPENROWSET` est construit à partir des parties catalogue, schéma et nom d’objet.

- Chacune des parties du nom (`catalog`, `schema`, `object name`) est délimitée par le caractère de délimitation du fournisseur (`DBLITERAL_QUOTE`), puis concaténée avec le caractère `DBLITERAL_CATALOG_SEPARATOR` et le caractère `DBLITERAL_SCHEMA_SEPARATOR` incorporé entre eux. La construction de nom suit les règles OLE DB dans `IOpenRowset`.

- Les métadonnées de colonne pour la table sont récupérées via `IColumnsInfo::GetColumnInfo` à l’issue de l’ouverture de l’objet rowset.

Si l’interface `IDBSchemaRowset` n’est pas prise en charge avec les ensembles de lignes TABLES, COLUMNS et TABLES_INFO, SQL Server ouvre l’ensemble de lignes dans la table de base à deux reprises : une fois pendant la compilation de la requête pour récupérer les métadonnées et une fois pendant l’exécution de la requête. Les fournisseurs qui occasionnent des effets secondaires suite à l’ouverture de l’ensemble de lignes (par exemple, l’exécution de code qui modifie l’état d’un appareil en temps réel, l’envoi d’un e-mail, l’exécution de code arbitraire fourni par l’utilisateur) doivent être conscients de ce comportement.

### <a name="statistics-retrieval"></a>Récupération des statistiques

Si le fournisseur prend en charge les statistiques de distribution sur les tables de base, SQL Server utilise ces statistiques. Il existe deux types de statistiques qui présentent un intérêt pour le processeur de requêtes SQL Server :

- **Cardinalités de colonne (ou tuple)**. Il s’agit du nombre de valeurs uniques qui se trouvent dans une colonne (ou une combinaison de colonnes) d’une table. Cela peut être utilisé pour estimer la sélectivité des prédicats par rapport aux colonnes. Un fournisseur qui prend en charge les statistiques de distribution doit prendre en charge au moins un type de cardinalité.

- **Histogrammes**. Si la distribution des valeurs n’est pas uniforme, alors le nombre de valeurs uniques n’est pas suffisant pour estimer avec précision la sélectivité des prédicats. Dans ce cas, vous pouvez fournir un histogramme qui donne des informations plus précises sur la distribution des valeurs de colonne dans une table.

La disponibilité des statistiques permet à l’optimiseur de requête SQL Server de mieux estimer les cardinalités des opérations intermédiaires dans une requête, ce qui lui permet de générer de meilleurs plans d’exécution.

Le fournisseur OLE DB doit prendre en charge les statistiques de distribution comme suit :

- **Obligatoire**. Prise en charge des propriétés (1) `DBPROP_TABLESTATISTICS` qui indique si les cardinalités de colonne ou de tuple sont prises en charge et si les histogrammes sont pris en charge, et (2) `DBPROP_OPENROWSETSUPPORT` qui indique l’utilisation du bit `DBPROPVAL_ORS_HISTOGRAM`, si les histogrammes sont pris en charge.

- **Obligatoire**. Ensemble de lignes de schéma `TABLE_STATISTICS`. L’ensemble de lignes de schéma `TABLE_STATISTICS` liste les statistiques disponibles dans une base de données donnée. Il comprend également les cardinalités de colonne et de tuple dans l’ensemble de lignes de schéma lui-même et indique si les histogrammes sont pris en charge sur les colonnes spécifiques. Pour que SQL Server utilise des statistiques, les colonnes `TABLE_NAME`, `STATISTICS_NAME`, `STATISTICS_TYPE`, `COLUMN_NAME`et `ORDINAL_POSITION` sont obligatoires dans cet ensemble de lignes de schéma. Au moins l’une des cardinalités `COLUMN_CARDINALITY` ou `TUPLE_CARDINALITY` est obligatoire. Si les histogrammes sont pris en charge, `NO_OF_RANGES` est également obligatoire.

- **Facultatif**. Éventuellement, si le fournisseur prend en charge les histogrammes, il doit prendre en charge une amélioration de la méthode `IOpenRowset::OpenRowset` qui permet d’ouvrir un ensemble de lignes d’histogramme en spécifiant le `DBID` de la statistique correspondante.

Pour obtenir des informations complètes sur les interfaces de statistiques, reportez-vous à la spécification OLE DB 2.6.

### <a name="constraints"></a>Contraintes

L’optimiseur de requête SQL Server utilise également les contraintes `CHECK` définies sur les tables de base dans une source de données distante, si le fournisseur OLE DB prend en charge l’ensemble de lignes de schéma OLE DB 2.6 `CHECK_CONSTRAINTS_BY_TABLE`. La colonne `CHECK_CLAUSE` de l’ensemble de lignes de schéma doit retourner le prédicat de clause `CHECK` dans la syntaxe compatible SQL-92. L’optimiseur de requête utilise les informations de contraintes afin d’éliminer ou de simplifier les prédicats connus comme ayant toujours la valeur False ou True en raison de la présence d’une contrainte CHECK sur la table.

### <a name="transaction-management"></a>Gestion des transactions

SQL Server prend en charge l’accès par transaction aux données distribuées par l’intermédiaire des interfaces OLE DB `ITransactionLocal` (transaction locale) et `ITransactionJoin` (transactions distribuées) du fournisseur. En démarrant une transaction locale sur le fournisseur, SQL Server garantit des opérations d’écriture atomiques. En utilisant des transactions distribuées, SQL Server garantit qu’une transaction impliquant plusieurs nœuds a le même résultat (validation ou abandon) dans tous les nœuds. Si le fournisseur ne prend pas en charge les interfaces relatives aux transactions OLE DB requises, les opérations de mise à jour sur ce fournisseur ne sont pas autorisées en fonction du contexte de la transaction locale.

Le tableau suivant décrit ce qui se produit lorsque l’utilisateur exécute une requête distribuée en fonction des capacités du fournisseur et du contexte d’une transaction locale. Une opération de lecture sur un fournisseur fait référence à une instruction `SELECT` ou à la table distante quand elle est lue côté entrée d’une instruction `SELECT INTO`, `INSERT`, `UPDATE` ou `DELETE`. Une opération d’écriture sur un fournisseur fait référence à une instruction `INSERT`, `UPDATE` ou `DELETE`, avec une table distante comme table de destination.

Résultats d’une requête distribuée en fonction des capacités du fournisseur et du contexte de transaction :

|La requête distribuée se produit|Le fournisseur ne prend pas en charge `ITransactionLocal`|Le fournisseur prend en charge `ITransactionLocal`, mais pas `ITransactionJoin`|Le fournisseur prend en charge `ITransactionLocal` et `ITransactionJoin`|
|:-----|:-----|:-----|:-----|
| Dans une transaction seule (aucune transaction utilisateur).|Par défaut, seules les opérations de lecture sont autorisées. Lorsque l’option de niveau fournisseur `Nontransacted Updates` est activée, les opérations d’écriture sont autorisées. (Quand cette option est activée, SQL Server ne peut pas garantir l’atomicité et la cohérence sur les données du fournisseur. Cela peut entraîner la répercussion des effets partiels d’une opération d’écriture dans la source de données distante sans possibilité de les annuler.)| Toutes les instructions sont autorisées sur les données distantes. Les curseurs de jeu de clés sont en lecture seule. La transaction locale est démarrée sur le fournisseur avec le niveau d’isolement de la session SQL Server actuelle et est validée à la fin de l’évaluation de la réussite de l’instruction. (Le niveau d’isolement par défaut pour une session SQL Server est `READ COMMITTED`, sauf s’il est modifié avec l’instruction `SET TRANSACTION ISOLATION LEVEL`. Le fournisseur doit prendre en charge le niveau d’isolement demandé.) | Toutes les instructions sont autorisées. Les curseurs de jeu de clés sont en lecture seule. La transaction locale est démarrée sur le fournisseur avec le niveau d’isolement de la session SQL Server actuelle et est validée à la fin de l’évaluation de la réussite de l’instruction. |
| Dans une transaction utilisateur (autrement dit, entre `BEGIN TRAN` ou `BEGIN DISTRIBUTED TRAN` et `COMMIT`). | Si le niveau d’isolement de la transaction est `READ COMMITTED` (valeur par défaut) ou une valeur inférieure, les opérations de lecture sont autorisées. Si le niveau d’isolement est supérieur, aucune requête distribuée n’est autorisée. | Seules les opérations de lecture sont autorisées. Les nouvelles transactions distribuées sont lancées sur le fournisseur avec le niveau d’isolement de la session SQL Server actuelle. |Toutes les instructions sont autorisées. La nouvelle transaction distribuée est démarrée sur le fournisseur avec le niveau d’isolement de la session SQL Server actuelle et est validée quand la transaction utilisateur est validée. Pour les instructions de modification de données, SQL Server démarre par défaut une transaction imbriquée sous la transaction distribuée, de sorte que l’instruction de modification de données peut être arrêtée sous certaines conditions d’erreur sans arrêter la transaction environnante. Si l’option `XACT_ABORT SET` est activée, SQL Server ne requiert pas la prise en charge des transactions imbriquées et arrête la transaction environnante en cas d’erreur au cours de l’exécution de l’instruction de modification de données. |
|  |  |  |

### <a name="data-type-handling-in-distributed-queries"></a>Gestion des types de données dans les requêtes distribuées

Les fournisseurs OLE DB exposent leurs données en termes de types de données définis par OLE DB (indiqués par `DBTYPE` dans OLE DB). SQL Server traite les données externes à l’intérieur du serveur en tant que types SQL Server natifs. Cela aboutit à un mappage de types de données OLE DB aux types natifs SQL Server, et vice versa, à mesure que les données sont consommées par SQL Server ou exportées par SQL Server, respectivement. Ce mappage est effectué implicitement, sauf indication contraire.

Les types de données dans les requêtes distribuées sont gérés à l’aide de l’une de deux méthodes de mappage :

- Le mappage côté consommation mappe les types des types de données OLE DB aux types de données natifs SQL Server, quand les tables distantes apparaissent dans des instructions `SELECT` et côté entrée des instructions INSERT, UPDATE et DELETE.

- Le mappage côté exportation mappe les types des types de données SQL Server aux types de données OLE DB, quand une table distante apparaît comme table de destination d’une instruction `INSERT` ou `UPDATE`.

Table de mappage des types de données SQL Server et OLE DB.

| Type OLE DB | `DBCOLUMNFLAG` | Type de données SQL Server |
|-----|-----|-----|
|`DBTYPE_I1`*| |`numeric(3, 0)`|
|`DBTYPE_I2`| |`smallint`|
|`DBTYPE_I4`| |`int`|
|`DBTYPE_I8`| |`numeric(19,0)`|
|`DBTYPE_UI1`| |`tinyint`|
|`DBTYPE_UI2`*| |`numeric(5,0)`|
|`DBTYPE_UI4`*| |`numeric(10,0)`|
|`DBTYPE_UI8`*| |`numeric(20,0)`|
|`DBTYPE_R4`| |`float`|
|`DBTYPE_R8`| |`real`|
|`DBTYPE_NUMERIC`| |`numeric`|
|`DBTYPE_DECIMAL`| |`decimal`|
|`DBTYPE_CY`| |`money`|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`<br>ou<br> Longueur max. > 4 000 caractères|ntext|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`|NCHAR|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=false`|NVARCHAR|
|`DBTYPE_IDISPATCH`| |Error|
|`DBTYPE_ERROR`| |Error|
|`DBTYPE_BOOL`| |`bit`|
|`DBTYPE_VARIANT`*| |NVARCHAR|
|`DBTYPE_IUNKNOWN`| |Erreur|
|`DBTYPE_GUID`| |`uniqueidentifier`|
|`DBTYPE_BYTES`|`DBCOLUMNFLAGS_ISLONG=true` <br>ou<br> Longueur max. > 8 000|`image`|
|`DBTYPE_BYTES`|`DBCOLUMNFLAGS_ISROWVER=true`, `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`, taille de colonne = 8 <br>ou<br> Longueur maximale non indiquée. | `timestamp` |
|`DBTYPE_BYTES`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `binary` |
|`DBTYPE_BYTES`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `varbinary`|
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `char`|
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `varchar` |
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISLONG=true` <br>ou<br> Longueur max. > 8 000 caractères <br>ou<br>   Longueur maximale non indiquée. | `text`|
|`DBTYPE_WSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` |`nchar`|
|`DBTYPE_WSTR` | `DBCOLUMNFLAGS_ISFIXEDLENGTH=false`|`nvarchar`|
|`DBTYPE_WSTR`| `DBCOLUMNFLAGS_ISLONG=true` <br>ou<br> Longueur max. > 4 000 caractères <br>ou<br>   Longueur maximale non indiquée. | `ntext`|
|`DBTYPE_UDT`| |Erreur|
|`DBTYPE_DATE`* | | `datetime` |
|`DBTYPE_DBDATE` | | `datetime` (conversion explicite requise)|
|`DBTYPE_DBTIME`| | `datetime` (conversion explicite requise)|
|`DBTYPE_DBTIMESTAMP`* | | `datetime`|
|`DBTYPE_ARRAY` | |Erreur|
|`DBTYPE_BYREF` | | Ignoré |
|`DBTYPE_VECTOR` | |Error|
|`DBTYPE_RESERVED`| |Error|

\* Indiquez une traduction approximative de la représentation du type SQL Server, car il n’existe pas de type de données équivalent exact dans SQL Server. Ces conversions peuvent aboutir à une perte de précision, un dépassement de capacité ou un dépassement négatif. Les mappages implicites par défaut peuvent être modifiés à l’avenir si les types de données correspondants sont pris en charge par les futures versions de SQL Server.

>[!NOTE]
>`numeric(p,s)` indique le type de données SQL Server `numeric` avec la précision `p` et l’échelle `s`. La précision maximale autorisée pour `DBTYPE_NUMERIC` et `DBTYPE_DECIMAL` est 38. Le fournisseur doit prendre en charge la liaison à la colonne `DBTYPE_BSTR` en tant que `DBTYPE_WSTR` lors de la création d’un accesseur. Les colonnes `DBTYPE_VARIANT` sont consommées en tant que chaînes de caractères Unicode `nvarchar`. Cela nécessite la prise en charge de la conversion de `DBTYPE_VARIANT` en `DBTYPE_WSTR` à partir du fournisseur. Le fournisseur est censé implémenter cette conversion comme défini dans OLE DB. Pour plus d’informations, consultez [Types de données de la spécification OLE DB](#appendixa).

#### <a name="interpreting-data-type-mapping"></a>Interprétation du mappage de types de données

Le mappage à un type SQL Server est déterminé par le type de données OLE DB et les valeurs DBCOLUMNFLAGS qui décrivent la colonne ou la valeur scalaire. Dans le cas de l’ensemble de lignes de schéma `COLUMNS`, les colonnes `DATA_TYPE` et `COLUMN_FLAGS` représentent ces valeurs. Dans le cas de l’interface `IColumnsInfo::GetColumnInfo`, les membres `wType` et `dwFlags` de la structure `DBCOLUMNINFO` représentent ces informations.

Pour utiliser le mappage côté consommation pour une colonne donnée avec des valeurs `DBTYPE` et `DBCOLUMNFLAG` spécifiques, recherchez le type SQL Server correspondant dans la table. Les règles de type pour les colonnes de tables distantes dans les expressions peuvent être décrites par la simple règle suivante :

>Une valeur de colonne distante donnée est légale dans une expression Transact-SQL si le type SQL Server mappé correspondant dans la table est légal dans le même contexte.

La table et la règle définissent les éléments suivants :

- Comparaisons et expressions.

En général, `X <op> <remote-column>` est une expression valide si `<op>` est un opérateur valide sur le type de données `X` et le type de données auquel `<remote-column>` est mappé.

- Conversions explicites.

`Convert(X, <remote-column>)` est autorisé si le `DBTYPE` de `<remote-column>` est mappé au type de données natif `Y` (conformément à la table ci- dessus) et que la conversion explicite de `Y` en `X` est autorisée.

Si les utilisateurs souhaitent que les données distantes soient converties en un type de données natif non défini par défaut, ils doivent utiliser une conversion explicite.

Pour utiliser le mappage côté exportation dans le cas des instructions `UPDATE` et `INSERT` sur des tables distantes, mappez les types de données SQL Server natifs à des types de données OLE DB à l’aide de la même table. Un mappage d’un type SQL Server `S1` à un type OLE DB donné `T` est autorisé si l’une des conditions suivantes est respectée :

- Le mappage correspondant se trouve directement dans la table de mappage.

- Il existe une conversion implicite autorisée de `S1` vers un autre type SQL Server `S2`, de sorte que `S2` est mappé au type `T` dans la table de mappage.

#### <a name="large-object-lob-handling"></a>Traitement des objets volumineux (LOB)

Comme indiqué dans la table de mappage, si les colonnes du type `DBTYPE_STR``DBTYPE_WSTR` ou `DBTYPE_BSTR` signalent également `DBCOLUMNFLAGS_ISLONG`, ou si leur longueur maximale dépasse 4 000 caractères (ou si aucune longueur maximale n’est indiquée), SQL Server les traite en tant que colonne `text` ou `ntext` selon le cas. De même, pour les colonnes `DBTYPE_BYTES`, si `DBCOLUMNFLAGS_ISLONG` est défini ou si la longueur maximale est supérieure à 8 000 octets (ou si la longueur maximale n’est pas indiquée), les colonnes sont traitées comme des colonnes `image`. Les colonnes `Text`, `ntext` et `image` sont appelées colonnes LOB.

SQL Server n’expose pas la fonctionnalité de texte intégral et d’image sur les objets LOB à partir d’un fournisseur OLE DB. Les `TEXTPTRS` ne sont pas pris en charge sur les objets volumineux à partir d’un fournisseur OLE DB ; par conséquent, aucune des fonctionnalités associées n’est prise en charge, par exemple la fonction système `TEXTPTR` ainsi que les instructions `READTEXT`, `WRITETEXT` et `UPDATETEXT`. Les instructions `SELECT` qui récupèrent des colonnes LOB entières sont prises en charge, comme les instructions `UPDATE` et `INSERT` pour les colonnes d’objets volumineux dans les tables distantes.

SQL Server utilise les interfaces de stockage structuré sur les colonnes LOB si le fournisseur les prend en charge. Les interfaces de stockage structuré dans l’ordre croissant de préférence et des fonctionnalités sont les suivantes : `ISequentialStream`, `Istream` ou `ILockBytes`. Si une ou plusieurs de ces interfaces sont prises en charge, le fournisseur doit retourner DBPROPVAL_OO_BLOB comme valeur de la propriété `DBPROP_OLEOBJECTS` lorsqu’elle est interrogée par le biais de l’interface `IDBProperties`. En outre, le fournisseur doit indiquer les interfaces qu’il prend en charge dans la propriété `DBPROP_STRUCTUREDSTORAGE`.

Si le fournisseur ne prend en charge aucune des interfaces de stockage structuré sur les colonnes LOB, SQL Server matérialise cette interface seule et les expose néanmoins en tant que colonnes `text`, `ntext` ou `image`.

#### <a name="accessing-lob-columns"></a>Accès aux colonnes LOB

Si le fournisseur prend en charge l’une des interfaces de stockage structuré, SQL Server effectue les étapes suivantes pour récupérer les colonnes LOB lors de l’exécution de la requête :

1. Avant d’ouvrir l’ensemble de lignes via `IOpenRowset::OpenRowset`, SQL Server demande la prise en charge d’une ou de plusieurs interfaces de stockage structuré (`ISequentialStream`, `Istream` et `ILockBytes`) sur les colonnes d’objets volumineux. La première interface prise en charge par le fournisseur est requise ; des interfaces supplémentaires sont demandées comme \"définies si bon marché\" en affectant à l’élément *dwOptions* de la structure DBPROP correspondante la valeur DBPROPOPTIONS_SETIFCHEAP. Par exemple, si un fournisseur prend en charge à la fois `ISequentialStream` et `ILockBytes`, `ISequentialStream` est requise et `ILockBytes` est demandée comme \"définie si bon marché\".

4. Une fois l’ensemble de lignes ouvert, SQL Server utilise `IRowsetInfo::GetProperties` pour identifier les interfaces réelles disponibles dans l’ensemble de lignes. L’interface la plus souhaitable ou la plus récente que le fournisseur a retournée est utilisée. Lorsque SQL Server crée un accesseur sur la colonne d’objets volumineux, la colonne est liée en tant que DBTYPE_IUNKNOWN avec l’élément *iid* de la structure DBOBJECT dans la liaison définie sur l’interface.

#### <a name="reading-from-lob-columns"></a>Lecture de colonnes LOB

Utilisez le pointeur d’interface pour l’interface de stockage structuré demandée retournée dans la mémoire tampon de ligne à partir d’`IRowset::GetData` pour lire la colonne d’objets volumineux. Si le fournisseur ne prend pas en charge plusieurs objets LOB ouverts en même temps (autrement dit, s’il ne prend pas en charge `DBPROP_MULTIPLE_STORAGEOBJECTS`) et si la ligne contient plusieurs colonnes d’objets volumineux, SQL Server copie les colonnes LOB dans une table de travail locale.

#### <a name="update-and-insert-statements-on-lob-columns"></a>Instructions `UPDATE` et `INSERT` sur les colonnes LOB

SQL Server passe au fournisseur un pointeur vers un nouvel objet de stockage au lieu d’utiliser l’interface fournie par le fournisseur pour modifier l’objet de stockage. Pour chaque colonne LOB, la valeur mise à jour ou insérée sur un objet de stockage est créée à l’aide de l’interface de stockage structuré choisie. Selon qu’il s’agit d’une opération `UPDATE` ou `INSERT`, un pointeur vers l’objet de stockage est passé au fournisseur via `IRowsetChange::SetData` ou `IRowsetChange::InsertRow`, respectivement.

### <a name="error-handling"></a>Gestion des erreurs

Lorsqu’un appel de méthode spécifique à un fournisseur OLE DB retourne un code d’erreur, SQL Server recherche les informations détaillées sur les erreurs du fournisseur avant de retourner des informations sur la condition d’erreur à l’utilisateur.

SQL Server utilise l’objet d’erreur OLE DB tel que spécifié par OLE DB. Voici quelques-unes des principales étapes :

1. Quand un appel de méthode retourne un code d’erreur du fournisseur, SQL Server recherche l’interface `ISupportErrorInfo`. Si cette interface est prise en charge, SQL Server appelle `ISupportErrorInfo::InterfaceSupportsErrorInfo` pour vérifier si les objets d’erreur sont pris en charge par l’interface qui a produit le code d’erreur.

<!-- -->

5. Si les objets d’erreur sont pris en charge par l’interface, SQL Server appelle la fonction `GetErrorInfo` pour recevoir un pointeur d’interface `IErrorInfo` sur l’objet d’erreur actuel.

6. SQL Server utilise l’interface `IErrorInfo` pour obtenir un pointeur vers l’interface `IErrorRecords`.

7. SQL Server utilise `IErrorRecords` pour parcourir tous les enregistrements d’erreur dans l’objet et recevoir le texte du message d’erreur correspondant à chaque enregistrement.

Pour plus d’informations sur l’utilisation de l’objet d’erreur du fournisseur, consultez la documentation OLE DB.

### <a name="security"></a>Sécurité

Lorsqu’un consommateur se connecte à un fournisseur OLE DB, le fournisseur requiert généralement un ID d’utilisateur et un mot de passe, sauf si le consommateur souhaite être authentifié en tant qu’utilisateur de sécurité intégrée. Dans le cas de requêtes distribuées, SQL Server agit en tant que consommateur du fournisseur OLE DB au nom du compte de connexion SQL Server qui exécute la requête distribuée. SQL Server mappe le compte de connexion SQL Server actuel à un ID d’utilisateur et un mot de passe sur le serveur lié.

Ces mappages peuvent être spécifiés par l’utilisateur pour un serveur lié donné et peuvent être configurés et gérés par les procédures stockées système `sp_addlinkedsrvlogin` et `sp_droplinkedsrvlogin`. En définissant les propriétés de groupe d’initialisation DBPROP_AUTH_USERID et DBPROP_AUTH_PASSWORD via `IDBProperties::SetProperties`, l’ID d’utilisateur et le mot de passe déterminés par le mappage sont passés au fournisseur lors de l’établissement de la connexion.

Quand un client se connecte à SQL Server par le biais de l’authentification Windows et si le mappage `self` du compte de connexion est configuré à l’aide de `sp_addlinkedsrvlogin`, SQL Server tente d’emprunter l’identité du contexte de sécurité du client et définit la propriété `DBPROP_AUTH_INTEGRATED` sur le fournisseur lors de l’établissement de la connexion. Ce processus est appelé *délégation*.

Une fois le contexte de sécurité utilisé pour la connexion déterminé, l’authentification de ce contexte de sécurité et la vérification des autorisations pour ce contexte par rapport aux objets de données de la source de données incombent entièrement au fournisseur OLE DB.

Pour plus d’informations, consultez [`sp_addlinkedsrvlogin`](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) et [`sp_droplinkedsrvlogin`](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md).

## <a name="query-execution-scenarios"></a>Scénarios d’exécution de requêtes

 Lors de l’évaluation d’une requête distribuée, SQL Server interagit avec le fournisseur OLE DB dans un ou plusieurs de ces scénarios :

- Requête distante

- Accès indexé

- Analyses de table pures

- Instructions `UPDATE` et DELETE

- Instruction `INSERT`

- Requêtes directes

### <a name="remote-query"></a>Remote Query

SQL Server génère une requête SQL qui évalue une partie de la requête d’origine qui peut être évaluée dans son intégralité par le fournisseur. Ce scénario est possible uniquement avec les fournisseurs de commandes SQL. La mesure dans laquelle SQL Server envoie des opérations au fournisseur en générant une requête SQL dépend de la grammaire SQL que le fournisseur prend en charge. Le fournisseur doit indiquer son niveau de prise en charge SQL par le biais des éléments suivants :

1. En indiquant la prise en charge de niveau SQL-Minimum, de niveau principal ODBC ou de niveau d’entrée SQL-92 par le biais de la propriété `DBPROP_SQLSUPPORT`. Le niveau de syntaxe SQL-Minimum est un nouveau niveau pris en charge dans SQL Server qui permet à SQL Server d’envoyer des requêtes distantes à des fournisseurs simples qui prennent en charge un sous-ensemble simple de SQL. Ce niveau englobe une instruction `SELECT` de base qui n’inclut pas de sous-requêtes, plusieurs tables dans la clause `FROM` (par conséquent, aucune jointure) et GROUP BY. Pour obtenir le sous-ensemble de la grammaire SQL utilisé par SQL Server pour la génération de requêtes distantes sur des fournisseurs de chacun de ces niveaux de syntaxe, consultez [Sous-ensemble SQL utilisé pour la génération de requêtes distantes](#appendixb).

1. En prenant en charge différentes propriétés spécifiques à SQL Server pour indiquer la prise en charge des fonctionnalités SQL individuelles qui ne sont autrement pas incluses dans le niveau de syntaxe comme indiqué par DBPROP_SQLSUPPORT. La liste des propriétés et la façon dont elles sont utilisées par SQL Server sont décrites plus loin dans cette section.

SQL Server utilise l’exécution des requêtes paramétrables avec un point d’interrogation (?) comme marqueur de paramètre dans la chaîne Transact-SQL. L’exécution des requêtes paramétrables est utilisée sur les fournisseurs SQL Server, Microsoft Jet et OLE DB Oracle. Pour les autres fournisseurs, l’exécution des requêtes paramétrables est utilisée si le fournisseur prend en charge `ICommandWithParameters` sur l’objet `Command` et qu’au moins l’une des conditions suivantes est remplie :

- Le fournisseur indique le niveau principal ODBC de prise en charge de SQL Server par le biais de la propriété `DBPROP_SQLSUPPORT`.

- Le fournisseur indique la prise en charge du marqueur de paramètre point d’interrogation (?) en prenant en charge la propriété SQLPROP_DYNCMICSQL spécifique à SQL Server via `IDBPProperties`. Pour plus d’informations, consultez la section suivante sur les propriétés du fournisseur.

- L’administrateur définit l’option de fournisseur `Dynamic Parameters` sur le fournisseur pour que SQL Server génère des requêtes paramétrables.

Quand SQL Server génère le texte SQL à exécuter à distance, les noms de table et de colonne sont délimités par le caractère de délimitation du fournisseur tel qu’indiqué par le littéral `DBLITERAL_QUOTE` de l’interface `IDBInfo`. Si ce littéral n’est pas pris en charge, les noms de table et de colonne ne sont pas délimités.

Si le fournisseur prend en charge l’exécution des requêtes paramétrables, SQL Server considère une stratégie d’exécution de requêtes paramétrables pour évaluer une jointure d’une table distante avec une table locale. La requête paramétrable est exécutée à plusieurs reprises pour les valeurs de paramètres générées à partir de chaque ligne de la table locale. Cette stratégie réduit le nombre de lignes récupérées du fournisseur et est utile quand une table locale avec un petit nombre de lignes est jointe à une table distante avec un grand nombre de lignes. Cette stratégie de jointure distante peut être appliquée par l’indicateur d’optimiseur de jointure `REMOTE`. Pour plus d’informations sur l’exécution des requêtes paramétrables, consultez [Guide pratique pour exécuter des requêtes paramétrables](../../connect/php/how-to-perform-parameterized-queries.md).

Voici les principales étapes pour le fournisseur dans le scénario de requête distante.

1. SQL Server crée un objet `Command` à partir de l’objet `Session` en utilisant `IDBCreateCommand::CreateCommand`.

9. Si l’option de configuration de serveur `Remote Query Timeout` a une valeur > 0, SQL Server affecte à la propriété `DBPROP_COMMANDTIMEOUT` de l’objet `Command` la même valeur en utilisant `ICommandProperties::SetProperties`. `ICommand::SetCommandText` doit être appelé pour définir le texte de la commande sur la chaîne Transact-SQL générée.

10. SQL Server appelle `ICommandPrepare::Prepare` pour préparer la commande. Si le fournisseur ne prend pas en charge cette interface, SQL Server passe à l’étape 4.

11. Si la requête générée est paramétrable, SQL Server utilise `ICommandWithParameters::SetParameterInfo` pour décrire les paramètres et `IAccessor::CreateAccessor` pour créer des accesseurs pour les paramètres.

12. SQL Server appelle `ICommand::Execute` pour exécuter la commande et créer l’ensemble de lignes.

13. SQL Server utilise l’interface `IRowset` pour naviguer et consommer des lignes dans la table. Utilisez `IRowset::GetNextRows` pour extraire des lignes, `IRowset::RestartPosition` pour un repositionnement au début de l’ensemble de lignes et `IRowset::ReleaseRows` pour libérer des lignes.

#### <a name="provider-properties-of-interest-for-remote-query-execution"></a>Propriétés du fournisseur intéressantes pour l’exécution de la requête distante

Si le fournisseur prend en charge des fonctionnalités SQL qui ne sont pas couvertes par le niveau de syntaxe indiqué dans DBPROP_SQLSUPPORT, il peut les indiquer à l’aide de différentes propriétés spécifiques au fournisseur.

- SQLPROP_GROUPBY. Cette propriété présente un intérêt pour un fournisseur qui prend en charge le niveau SQL-Minimum. Cette propriété indique que le fournisseur prend en charge les clauses GROUP BY et HAVING dans l’instruction `SELECT`. De plus, cette propriété indique également que le fournisseur prend en charge les cinq fonctions d’agrégation suivantes : MIN, MAX, SUM, COUNT et AVG. Le fournisseur peut ne pas prendre en charge DISTINCT sur l’argument de ces fonctions d’agrégation.

- SQLPROP_SUBQUERIES. Cette propriété présente un intérêt dans un fournisseur qui prend en charge le niveau SQL-Minimum. Il indique que le fournisseur prend en charge les sous-requêtes comme indiqué par le niveau d’entrée SQL-92. Cela comprend les sous-requêtes dans la liste `SELECT` et dans la clause `WHERE` avec prise en charge des sous-requêtes corrélées ainsi que des opérateurs `IN`, `EXISTS`, `ALL` et `ANY`.

- SQLPROP_DATELITERALS. Cette propriété présente un intérêt pour tous les fournisseurs (notamment ceux qui prennent en charge le niveau d’entrée SQL-92). La prise en charge de la syntaxe des littéraux standard pour les littéraux datetime ne fait pas partie du niveau d’entrée SQL-92. Cette propriété spécifique à SQL Server indique que le fournisseur prend en charge la syntaxe des littéraux datetime comme spécifié par la norme SQL-92.

- SQLPROP_ANSILIKE. Présente un intérêt pour un fournisseur qui prend en charge le niveau SQL-Minimum. Cette propriété indique que le fournisseur prend en charge l’opérateur `LIKE` conformément au niveau d’entrée SQL-92 (\'%\' et \'_\' comme caractères génériques). Cela sera utile avec un fournisseur qui prend en charge le niveau SQL-Minimum, car le niveau SQL-Minimum n’inclut pas la prise en charge de `LIKE`.

- SQLPROP_INNERJOIN. Cette propriété présente un intérêt pour les fournisseurs qui prennent en charge le niveau SQL-Minimum. Elle indique la prise en charge de plusieurs tables dans la clause `FROM`. Cela sera utile avec un fournisseur qui prend en charge uniquement le niveau SQL-Minimum, car le niveau SQL-Minimum n’inclut pas la prise en charge de jointures. Cela n’indique pas la prise en charge de mots clés JOIN explicites ni celle de jointures OUTER. Elle indique uniquement la prise en charge de jointures implicites via une liste de tables dans la clause `FROM`.

- SQLPROP_DYNAMICSQL. Indique la prise en charge de `?` en tant que marqueur de paramètre. Le marqueur de paramètre doit être pris en charge à la place d’un élément scalaire dans une clause `WHERE` ou dans la liste `SELECT`. La prise en charge des marqueurs de paramètres `?` permet à SQL Server d’envoyer des requêtes paramétrables au fournisseur.

- SQLPROP_NESTEDQUERIES. Indique la prise en charge des instructions SELECT imbriquées dans la clause `FROM` (par exemple, `SELECT * FROM (SELECT * FROM T))`. Dans de nombreux cas, SQL Server utilise des instructions `SELECT` imbriquées dans la clause `FROM` d’une requête lors de la génération des chaînes de requête à exécuter à distance. Étant donné que la prise en charge de `SELECT` imbriquée n’est pas requise par le niveau d’entrée SQL-92, SQL Server ne délègue pas de requêtes avec des instructions `SELECT` imbriquées au fournisseur, sauf si le fournisseur définit également cette propriété. Sinon, l’administrateur peut également définir l’option de fournisseur `Nested Queries` pour que SQL Server génère des requêtes imbriquées sur le fournisseur.

Le fournisseur peut prendre en charge ces propriétés à l’aide d’un jeu de propriétés spécifique à SQL Server appelé `SQLPROPSET_OPTHINTS` et avoir des valeurs `PROPID` définies. Le jeu de propriétés `SQLPROPSET_OPTHINTS` et les deux propriétés sont définis à l’aide des constantes suivantes :

```
extern const GUID `SQLPROPSET_OPTHINTS` = { 0x2344480c, 0x33a7, 0x11d1, { 0x9b, 0x1a, 0x0, 0x60, 0x8, 0x26, 0x8b, 0x9e } };
enum SQLPROPERTIES {
SQLPROP_NESTEDQUERIES = 0x4,
SQLPROP_DYNAMICSQL = 0x5,
SQLPROP_GROUPBY = 0x6,
SQLPROP_DATELITERALS = 0x7,
SQLPROP_ANSILIKE = 0x8,
SQLPROP_INNERJOIN = 0x9,
SQLPROP_SUBQUERIES = 0x10
};
```

#### <a name="character-set-and-sort-order-implications"></a>Jeu de caractères et implications sur l’ordre de tri

SQL Server prend en charge la spécification d’un classement pour les données caractères au niveau de chaque colonne. Le classement comprend à la fois le jeu de caractères et la spécification de l’ordre de tri pour les données caractères non-Unicode (colonnes `char` et `varchar`). Pour les données Unicode (colonnes `nchar` et `nvarchar`), le classement spécifie uniquement l’ordre de tri.

SQL Server délègue les comparaisons de chaînes au fournisseur uniquement si le jeu de caractères (pour les données non-Unicode), l’ordre de tri et la sémantique de comparaison de chaînes utilisés par le serveur lié sont les mêmes que ceux utilisés par le serveur local.

Dans le cas de serveurs liés SQL Server, SQL Server détermine automatiquement la compatibilité du classement. Pour les autres fournisseurs, l’administrateur doit indiquer à SQL Server le classement des données caractères à partir d’un serveur lié donné. Dans SQL Server, une nouvelle option de serveur lié appelée `Collation Name` est prise en charge. Si l’administrateur détermine que la sémantique de classement adoptée par le serveur lié est identique à l’un des classements SQL Server standard, il peut définir l’option `Collation Name` sur ce nom de classement. L’option `Collation Name` peut être définie en utilisant la procédure stockée système `sp_serveroption`. Cette option ne doit être définie que si les deux conditions suivantes sont réunies :

- L’ordre de tri et le jeu de caractères distants sont les mêmes que le classement SQL Server spécifié.

- La sémantique de comparaison de chaînes utilisée par le fournisseur OLE DB suit celle des spécifications de la norme SQL-92 ou, de manière équivalente, la sémantique de comparaison de SQL Server.

L’option Compatible avec le classement prise en charge dans SQL Server 7.0 est toujours prise en charge, pour des raisons de compatibilité descendante. Si vous lui affectez la valeur True, cela revient à définir l’option Nom du classement sur le classement par défaut de la base de données master de SQL Server. Les nouvelles applications doivent utiliser l’option Nom du classement à la place de l’option Compatible avec le classement.

### <a name="indexed-access"></a>Accès indexé

SQL Server utilise un index exposé par le fournisseur pour évaluer certains prédicats de la requête distribuée. Ce scénario est possible uniquement pour les fournisseurs d’index et lorsque l’utilisateur définit l’option de fournisseur `Index as Access Path`. Voici les principales étapes que SQL Server effectue sur le fournisseur lors de l’utilisation d’un index pour exécuter une requête :

1. Ouvre l’ensemble de lignes d’index via `IOpenRowset::OpenRowset` avec les noms complets de la table et de l’index. Les noms complets de la table et de l’index sont générés comme décrit précédemment dans le scénario de requête distante.

1. Ouvre l’ensemble de lignes de la table de base via `IOpenRowset::OpenRowset` avec le nom complet de la table.

1. Définit des plages sur l’ensemble de lignes d’index selon le prédicat de requête via `IRowsetIndex::SetRange`.

1. Analyse des lignes de l’ensemble de lignes d’index via `IRowset` sur l’ensemble de lignes d’index.

1. Utilise la colonne de signets des lignes d’index récupérées pour extraire les lignes correspondantes de l’ensemble de lignes de la table de base via `IRowsetLocate::GetRowsByBookmark`.

Les propriétés de l’ensemble de lignes `DBPROP_IRowsetLocate` et `DBPROP_BOOKMARKS` sont requises sur l’ensemble de lignes ouvert dans la table de base.

### <a name="pure-table-scans"></a>Analyses de table pures

SQL Server analyse l’intégralité de la table distante à partir du fournisseur et effectue l’évaluation de toutes les requêtes localement. L’ensemble de lignes correspondant à la table est ouvert en appelant `IOpenRowset::OpenRowset`. SQL Server construit le nom de table fourni à `OPENROWSET` à partir des parties catalogue, schéma et nom d’objet comme suit :

1. Chacune des parties du nom est délimitée par le caractère de délimitation du fournisseur (`DBLITERAL_QUOTE`), puis concaténée avec le caractère `DBLITERAL_CATALOG_SEPARATOR` incorporé entre eux.

1. Une fois l’objet rowset ouvert, SQL Server utilise l’interface `IColumnsInfo` pour vérifier que les métadonnées au moment de l’exécution sont identiques aux métadonnées au moment de la compilation pour la table.

1. SQL Server utilise l’interface `IRowset` pour naviguer et consommer des lignes dans la table. Utilisez `IRowset::GetNextRows` pour extraire des lignes, `IRowset::RestartPosition` pour un repositionnement au début de l’ensemble de lignes et `IRowset::ReleaseRows` pour libérer des lignes.

### <a name="update-and-delete-statements"></a>Instructions `UPDATE` et `DELETE`

Les conditions suivantes doivent être satisfaites pour qu’une table distante soit mise à jour ou supprimée à partir d’une requête distribuée SQL Server :

- Le fournisseur doit prendre en charge les signets pour l’ensemble de lignes ouvert via `IOpenRowset` sur la table en cours de mise à jour ou de suppression.

- Le fournisseur doit prendre en charge les interfaces `IRowsetLocate` et `IRowsetChange` sur l’ensemble de lignes ouvert via `IOpenRowset` pour la table en cours de mise à jour ou de suppression.

- L’interface `IRowsetChange` doit prendre en charge les méthodes de mise à jour (`SetData`) et de suppression (`DeleteRows`).

- Si le fournisseur ne prend pas en charge `ITransactionLocal`, les instructions `UPDATE` et `DELETE` sont autorisées uniquement si l’option `Non-transacted` est définie pour ce fournisseur et si l’instruction ne se trouve pas dans une transaction utilisateur.

- Si le fournisseur ne prend pas en charge `ITransactionJoin`, une instruction `UPDATE` ou `DELETE` est autorisée uniquement si elle ne se trouve pas dans une transaction utilisateur.

Les propriétés de l’ensemble de lignes suivantes sont requises sur l’ensemble de lignes ouvert dans la table mise à jour : `DBPROP_IRowsetLocate`, `DBPROP_IRowsetChange` et `DBPROP_BOOKMARKS`. La propriété de l’ensemble de lignes `DBPROP_UPDATABILITY` a la valeur `DBPROPVAL_UP_CHANGE` ou `DBPROPVAL_UP_DELETE`, selon que l’opération exécutée est `UPDATE` ou `DELETE`, respectivement.

Les principales étapes suivantes sur le fournisseur pour le traitement d’une opération `UPDATE` ou `DELETE` sont effectuées :

1. SQL Server ouvre l’ensemble de lignes de la table de base à l’aide de l’interface `IOpenRowset`. SQL Server requiert les propriétés mentionnées ci-dessus sur l’ensemble de lignes.

1. SQL Server détermine l’ensemble des lignes éligibles à mettre à jour ou à supprimer.

1. SQL Server utilise les signets à positionner sur les lignes éligibles par le biais de l’interface `IRowsetLocate`.

1. Utilisez `IRowsetChange::SetData` pour les opérations `UPDATE` ou `IRowsetChange::DeleteRows` pour les opérations de suppression afin d’effectuer les modifications requises sur les lignes éligibles.

### <a name="insert-statement"></a>Instruction `INSERT`

Les conditions de prise en charge des instructions `INSERT` sur une table distante sont moins strictes que pour les instructions `UPDATE` et `DELETE` :

- Le fournisseur doit prendre en charge `IRowsetChange::InsertRow` sur l’ensemble de lignes ouvert dans la table de base qui est actuellement insérée.

- Si le fournisseur ne prend pas en charge `ITransactionLocal`, les instructions `INSERT` sont autorisées uniquement si l’option `Non-transacted updates` est définie pour ce serveur lié et si l’instruction ne se trouve pas dans une transaction utilisateur.

- Si le fournisseur ne prend pas en charge `ITransactionJoin`, les instructions `INSERT` sont autorisées uniquement si elles ne se trouvent pas dans une transaction utilisateur.

SQL Server utilise `IOpenRowset::OpenRowset` pour ouvrir un ensemble de lignes dans la table de base et appelle `IRowsetChange::InsertRow` pour insérer de nouvelles lignes dans l’ensemble de lignes de base.

### <a name="pass-through-queries"></a>Requêtes directes

Ce scénario est similaire au scénario dans la requête distante, à ceci près que le texte de la commande donné à `ICommand` est une chaîne de commande soumise par l’utilisateur et qui n’est pas interprétée par SQL Server. SQL Server utilise `DBGUID_DEFAULT` comme identificateur de dialecte quand il appelle `ICommandText::SetCommandText`. `DBGUID_DEFAULT` indique que le fournisseur doit utiliser son dialecte par défaut. Si le texte de cette commande retourne plusieurs jeux de résultats, par exemple si la commande appelle une procédure stockée qui retourne plusieurs jeux de résultats, SQL Server utilise uniquement le premier jeu de résultats de la commande.

Pour obtenir la liste de toutes les interfaces OLE DB que SQL Server utilise, consultez [Interfaces OLE DB consommées par SQL Server](#appendixa).

### <a name="conclusion"></a>Conclusion

Microsoft SQL Server offre l’ensemble d’outils le plus robuste pour accéder aux données à partir de sources de données hétérogènes. En maîtrisant les interfaces OLE DB exposées par SQL Server, les développeurs peuvent exercer un degré élevé de contrôle et de sophistication dans les requêtes distribuées.

## <a name="ole-db-interfaces-consumed-by-sql-server"></a><a name="appendixa"></a> Interfaces OLE DB consommées par SQL Server

Le tableau suivant répertorie toutes les interfaces OLE DB utilisées par SQL Server. La colonne Obligatoire indique si l’interface fait partie du strict minimum des fonctionnalités OLE DB dont SQL Server a besoin ou si elle est facultative. Si une interface donnée n’est pas marquée comme étant obligatoire, SQL Server peut toujours accéder au fournisseur, mais certaines fonctionnalités SQL Server spécifiques ou l’optimisation ne sont pas possibles avec le fournisseur.

Dans le cas des interfaces facultatives, la colonne Scénarios indique un ou plusieurs des six scénarios qui utilisent l’interface spécifiée. Par exemple, l’interface `IRowsetChange` sur les ensembles de lignes de la table de base est une interface facultative ; cette interface est utilisée dans les instructions `UPDATE` et DELETE ainsi que dans les scénarios de l’instruction `INSERT`. Si cette interface n’est pas prise en charge, les instructions UPDATE, DELETE et `INSERT` ne peuvent pas être prises en charge avec ce fournisseur. Certaines des autres interfaces facultatives sont marquées \"performances\" dans la colonne Scénarios, ce qui indique que l’interface produit de meilleures performances générales. Par exemple, si l’interface `IDBSchemaRowset` n’est pas prise en charge, SQL Server doit ouvrir l’ensemble de lignes deux fois : une fois pour ses métadonnées et une fois pour l’exécution de la requête. En prenant en charge `IDBSchemaRowset`, les performances SQL Server sont améliorées.

|Object|Interface|Obligatoire|Commentaires|Scénarios|
|:-----|:-----|:-----|:-----|:-----|
|Objet source de données|`IDBInitialize`|Oui|Initialisez et configurez les données et le contexte de sécurité.| |
| |`IDBCreateSession`|Oui|Créez un objet de session de base de données.| |
| |`IDBProperties`|Oui|Obtenez des informations sur les capacités du fournisseur, définissez des propriétés d’initialisation, propriété obligatoire : DBPROP_INIT_TIMEOUT.| |
| |`IDBInfo`|Non|Obtenez le littéral de délimitation, le catalogue, le nom, la partie, le séparateur, le caractère et ainsi de suite.|Requête distante.|
|Objet de session de base de données|`IDBSchemaRowset`|Non|Obtenez les métadonnées de table/colonne. Ensembles de lignes nécessaires : `TABLES`, `COLUMNS`, `PROVIDER_TYPES` ; autres ensembles de lignes qui sont utilisés s’ils sont disponibles : `INDEXES`, `TABLE_STATISTICS`.|Performances, accès indexé.|
| |`IOpenRowset`|Oui|Ouvrez un ensemble de lignes dans une table, un index ou un histogramme.| |
| |`IGetDataSource`|Oui|À utiliser pour revenir à l’objet source de données à partir d’un objet de session de base de données.| |
| |`IDBCreateCommand`|Non|À utiliser afin de créer un objet de commande (requête) pour les fournisseurs qui prennent en charge l’interrogation.|Requête distante, requête directe.|
| |`ITransactionLocal`|Non|À utiliser pour les mises à jour transactionnelles.|Instructions `UPDATE` et `DELETE`, `INSERT`.|
| |`ITransactionJoin`|Non|À utiliser pour la prise en charge des transactions distribuées.|Instructions `UPDATE` et `DELETE`, `INSERT` dans une transaction utilisateur.|
|Objet rowset|IRowset|Oui|Analysez les lignes.| |
| |`IAccessor`|Oui|Établissez des liaisons avec des colonnes d’un ensemble de lignes.| |
| |`IColumnsInfo`|Oui|Obtenez des informations sur les colonnes d’un ensemble de lignes.| |
| |`IRowsetInfo`|Oui|Obtenez des informations sur les propriétés d’un ensemble de lignes.| |
| |`IRowsetLocate`|Non|Nécessaire pour les opérations `UPDATE`/`DELETE` et pour effectuer des recherches basées sur l’index ; permet de rechercher des lignes par signets.|Accès indexé, instructions `UPDATE` et `DELETE`.|
| |`IRowsetChange`|Non|Nécessaire pour `INSERTS`/`UPDATES`/ `DELETES` sur un ensemble de lignes. Les ensembles de lignes des tables de base doivent prendre en charge cette interface pour les instructions `INSERT`, `UPDATE` et `DELETE`.|Instructions `UPDATE` et `DELETE`, `INSERT`.|
| |`IConvertType`|Oui|À utiliser pour vérifier si l’ensemble de lignes prend en charge les conversions de types de données spécifiques sur ses colonnes.| |
|Index|`IRowset`|Oui|Analysez les lignes.|Accès indexé, performances.|
| |`IAccessor`|Oui|Établissez des liaisons avec des colonnes d’un ensemble de lignes.|Accès indexé, performances.|
| |`IColumnsInfo`|Oui|Obtenez des informations sur les colonnes d’un ensemble de lignes.|Accès indexé, performances.|
| |`IRowsetInfo`|Oui|Obtenez des informations sur les propriétés d’un ensemble de lignes.|Accès indexé, performances.|
| |`IRowsetIndex`|Oui|Nécessaire uniquement pour les ensembles de lignes d’un index ; utilisation avec la fonctionnalité d’indexation (définition de plages, recherche).|Accès indexé, performances.|
|Commande|`ICommand`|Oui| |Requête distante, requête directe.|
| |`ICommandText`|Oui|À utiliser pour la définition du texte de la requête.|Requête distante, requête directe.|
| |`IColumnsInfo`|Oui|À utiliser afin d’obtenir les métadonnées de colonne pour les résultats de la requête.|Requête distante, requête directe.|
| |`ICommandProperties`|Oui|À utiliser pour spécifier les propriétés requises sur des ensembles de lignes retournés par la commande.|Requête distante, requête directe.|
| |`ICommandWithParameters`|Non|À utiliser pour l’exécution des requêtes paramétrables.|Requête distante, performances.|
| |`ICommandPrepare`|Non|À utiliser pour la préparation d’une commande permettant d’obtenir des métadonnées (utilisation dans les requêtes directes, si elles sont disponibles).|Requête distante, performances.|
|Objet d’erreur|`IErrorRecords`|Oui|À utiliser afin d’obtenir un pointeur vers une interface `IErrorInfo` correspondant à un enregistrement d’erreur unique.| |
| |`IErrorInfo`|Oui|À utiliser afin d’obtenir un pointeur vers une interface `IErrorInfo` correspondant à un enregistrement d’erreur unique.| |
|Tout objet|`ISupportErrorInfo`|Non|À utiliser pour vérifier si une interface donnée prend en charge les objets d’erreur.| |
|  |  |  |  |  |

>[!NOTE]
>L’objet `Index`, l’objet `Command` et l’objet `Error` ne sont pas obligatoires. Toutefois, s’ils sont pris en charge, les interfaces répertoriées sont obligatoires comme indiqué dans la colonne Obligatoire.

## <a name="sql-subset-used-for-generating-remote-queries"></a><a name="appendixb"></a>Sous-ensemble SQL utilisé pour la génération de requêtes distantes

Le sous-ensemble SQL que le processeur de requêtes SQL Server génère sur un fournisseur de commandes SQL dépend du niveau de syntaxe que le fournisseur prend en charge, comme indiqué par la propriété `DBPROP_SQLSUPPORT`.

Fournisseurs de commandes SQL qui prennent en charge le niveau d’entrée SQL ou le niveau principal ODBC

SQL Server utilise le sous-ensemble suivant du langage SQL pour les requêtes évaluées par les fournisseurs de commandes SQL qui prennent en charge le niveau d’entrée SQL-92 ou le niveau principal ODBC :

1. Instructions `SELECT` avec les clauses `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `UNION`, `UNION ALL`, `ORDER BY DESC`, `ASC` et `HAVING`.

1. Les clauses `UNION` et `UNION ALL` sont générées uniquement sur les fournisseurs qui prennent en charge le niveau d’entrée SQL-92, et non sur ceux qui prennent en charge le niveau principal ODBC.

1. Clause `SELECT` :

   - Sous-requêtes scalaires dans la liste `SELECT`.

   - Alias de colonne sans le mot clé `AS`.

1. Clause `FROM` :

   - Les mots clés de jointures explicites ne sont pas utilisés, les noms de tables séparés par des virgules sont utilisés pour spécifier des jointures internes, et les jointures externes ne sont pas spécifiées dans les requêtes distantes.

   - Requêtes imbriquées de la forme `FROM` ( `<nested query>` ) `<alias>`.

   - Alias de table sans le mot clé AS.

1. La clause `WHERE` utilise des sous-requêtes avec `NOT` `EXISTS`, `ANY`, `ALL`.

1. Expressions :

   - Fonctions d’agrégation utilisées : `MIN([DISTINCT])`, `MAX([DISTINCT])`, `COUNT([DISTINCT])`, `SUM([DISTINCT])`, `AVG([DISTINCT])` et `COUNT(*)`.

   - Opérateurs de comparaison : `<`, `=`, `<=`, `>`, `<>`, `>=`, `IS NULL` et `IS NOT NULL`.

   - Opérateurs booléens : `AND`, `OR` et `NOT`.

 - Opérateurs arithmétiques: `+`, `-`, `*` et `/`.

1. Constantes :

- Les littéraux numériques et monétaires sont toujours entourés de `( )`.

- Les littéraux de caractère sont délimités par `' '`.

Fournisseurs de commandes SQL qui prennent en charge le niveau SQL-Minimum

Pour les fournisseurs de commandes SQL qui prennent en charge le niveau SQL-Minimum, SQL Server génère SQL à l’aide de la grammaire suivante.

Cette grammaire est dérivée de la grammaire SQL-Minimum décrite dans ODBC 3.0. Toutes les différences par rapport à cette grammaire sont mises en surbrillance. Les éléments affichés en `*bold italics`* sont ceux ajoutés à la grammaire SQL-Minimum décrite dans ODBC 3.0. Les éléments affichés comme supprimés en vert sont ceux qui ont été retirés de cette grammaire.

*select-statement* ::=

`SELECT [ALL | DISTINCT] *select-list* FROM *table-reference-list*[WHERE *search-condition*] [order-by-clause]`

Clause `SELECT`

select-list ::= `*` | `select-sublist [, select-sublist]...`

select-sublist ::= expression * [`alias`]*

`*alias ::= user-defined-name`*

`FROM clause`

table-reference-list ::= table-reference

table-identifier ::= user-defined-name

table-name ::= table-identifier

table-reference ::= table-name

`WHERE clause`

search-condition ::= boolean-term \[OR search-condition\]

boolean-term ::= boolean-factor \[AND boolean-term\]

boolean-factor ::= \[NOT\] boolean-primary

boolean-primary ::= comparison-predicate \| ( search-condition )

comparison-predicate ::= expression comparison-operator expression

*\| `expression IS \[NOT\] NULL`*

comparison-operator ::= `< \| >` \| `<= \| >`= \| = \| `<>`

`ORDER BY clause`

order-by-clause ::= ORDER BY sort-specification \[, sort-specification\]\..

sort-specification ::= { \| column-name } \[ASC \| DESC\]

`Common syntactic elements`

expression ::= term \| expression {+\|--} term

term ::= factor \| term {\*\|/} factor

factor ::= \[+\|--\] primary

primary ::= column-name

\| literal

\| ( expression )

column-name ::= \[table-name.\]column-identifier

literal ::= character-string-literal

*\| integer-literal*

*\| exact-numeric-literal*

character-string-literal ::= \'{character}...\'

(character est un caractère du jeu de caractères du pilote/de la source de données. Pour inclure un caractère guillemet littéral simple (\') dans un character-string-literal, utilisez deux guillemets littéraux (\'\').)

`*integer-literal ::=* \[*+ \| -*\] *unsigned-integer`*

`*exact-numeric-literal::=* \[*+ \| -*\] *unsigned-integer* \[*period unsigned-integer*\]`

`*\| period unsigned-integer`*

base-table-name ::= base-table-identifier

base-table-identifier ::= user-defined-name

column-identifier ::= user-defined-name

user-defined-name ::= letter\[digit \| letter \| _\]\..

unsigned-integer ::= {digit}...

digit ::= 0 \| 1 \| 2 \| 3 \| 4 \| 5 \| 6 \| 7 \| 8 \| 9

period ::= . 

## <a name="sql-server-specific-properties"></a><a name="appendixc"></a>Propriétés spécifiques à SQL Server

```
enum SQLPROPERTIES
       {
       SQLPROP_NOHPNEEDED = 0x1,
       SQLPROP_FREETHREADED = 0x2,
       SQLPROP_UMSENABLED = 0x3,
       SQLPROP_NESTEDQUERIES = 0x4,
       SQLPROP_DYNAMICSQL = 0x5,
       SQLPROP_GROUPBY = 0x6,
       SQLPROP_DATELITERALS = 0x7,
       SQLPROP_ANSILIKE = 0x8,
       SQLPROP_INNERJOIN = 0x9,
       SQLPROP_SUBQUERIES = 0x10, 
       SQLPROP_PARALLELSCAN = 0x11,
       SQLPROP_COLUMNCOLLATION = 0x12,
       SQLPROP_CARDINALITY = 0x13,
       SQLPROP_SIMPLEUPDATES = 0x14,
       SQLPROP_SQLLIKE = 0x15,
       SQLPROP_BITREMOTING = 0x16,
       SQLPROP_UNICODELITERALS = 0x17,
       SQLPROP_USELATESTCOLLATIONVERSION = 0x18
       };

```
