---
description: CREATE COLUMNSTORE INDEX (Transact-SQL)
title: CREATE COLUMNSTORE INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE INDEX
- COLUMNSTORE_INDEX_TSQL
- CREATE CLUSTERED COLUMNSTORE INDEX
- COLUMNSTORE_TSQL
- CREATE NONCLUSTERED COLUMNSTORE INDEX
- CREATE_NONCLUSTERED_COLUMNSTORE_INDEX_TSQL
- CREATE COLUMNSTORE INDEX
- CREATE_CLUSTERED_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE
dev_langs:
- TSQL
helpviewer_keywords:
- index creation [SQL Server], columnstore indexes
- columnstore index, creating
- CREATE COLUMNSTORE INDEX statement
- CREATE INDEX statement
ms.assetid: 7e1793b3-5383-4e3d-8cef-027c0c8cb5b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b650e0fa1d1d6c02e657d3d5860908164ed5cd91
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099556"
---
# <a name="create-columnstore-index-transact-sql"></a>CREATE COLUMNSTORE INDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Convertissez une table rowstore en index cluster columnstore ou créez un index columnstore non-cluster. Utilisez un index columnstore pour procéder efficacement à une analytique opérationnelle en temps réel sur une charge de travail OLTP ou pour améliorer les performances de compression et de requête de données pour les charges de travail d’entreposage de données.  
  
> [!NOTE]
> Depuis [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], vous pouvez créer la table en tant qu’index cluster columnstore.   Il n’est plus nécessaire de créer d’abord une table rowstore, puis de la convertir en index cluster columnstore.  

> [!TIP]
> Pour obtenir des informations sur les règles de conception d’index, consultez le [Guide de conception d’index SQL Server](../../relational-databases/sql-server-index-design-guide.md).

Passez aux exemples :  
-   [Exemples de conversion d’une table rowstore en columnstore](../../t-sql/statements/create-columnstore-index-transact-sql.md#convert)  
-   [Exemples d’index columnstore non-cluster](../../t-sql/statements/create-columnstore-index-transact-sql.md#nonclustered)  
  
Passez aux scénarios :  
-   [Index columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
-   [Index columnstore pour l’entreposage des données](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)  
  
En savoir plus :  
-   [Guide des index columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md)  
-   [Synthèse des fonctionnalités des index columnstore](../../relational-databases/indexes/columnstore-indexes-what-s-new.md)  
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create a clustered columnstore index on disk-based table.  
CREATE CLUSTERED COLUMNSTORE INDEX index_name  
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }  
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ] 
[ ; ]  
  
--Create a nonclustered columnstore index on a disk-based table.  
CREATE [NONCLUSTERED]  COLUMNSTORE INDEX index_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
        ( column  [ ,...n ] )  
    [ WHERE <filter_expression> [ AND <filter_expression> ] ]
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ]
[ ; ]  
  
<with_option> ::=  
      DROP_EXISTING = { ON | OFF } -- default is OFF  
    | MAXDOP = max_degree_of_parallelism 
    | ONLINE = { ON | OFF } 
    | COMPRESSION_DELAY  = { 0 | delay [ Minutes ] }  
    | DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
      [ ON PARTITIONS ( { partition_number_expression | range } [ ,...n ] ) ]  
  
<on_option>::=  
      partition_scheme_name ( column_name )
    | filegroup_name
    | "default"
  
<filter_expression> ::=  
      column_name IN ( constant [ ,...n ]  
    | column_name { IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !< } constant  
  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE CLUSTERED COLUMNSTORE INDEX index_name
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name } 
    [ORDER (column [,...n] ) ]  
    [ WITH ( DROP_EXISTING = { ON | OFF } ) ] --default is OFF  
[;]  

```
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

Certaines options ne sont pas disponibles dans toutes les versions du moteur de base de données. Le tableau suivant présente les versions quand les options sont introduites dans les index CLUSTERED COLUMNSTORE et NONCLUSTERED COLUMNSTORE :

|Option| CLUSTERED | NONCLUSTERED |
|---|---|---|
| COMPRESSION_DELAY | [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] | [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] |
| DATA_COMPRESSION | [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] | [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] | 
| ONLINE | [!INCLUDE[ssSQLv15_md](../../includes/sssqlv15-md.md)] | [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] |
| WHERE (clause) | N/A | [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] |

Toutes les options sont disponibles dans Azure SQL Database.

### <a name="create-clustered-columnstore-index"></a>CREATE CLUSTERED COLUMNSTORE INDEX

Crée un index cluster columnstore dans lequel toutes les données sont compressées et stockées en colonne. L'index inclut toutes les colonnes de la table et stocke la table entière. Si la table existante est un segment ou un index cluster, elle est convertie en index cluster columnstore. Si la table est déjà stockée en tant qu’index cluster columnstore, l’index existant est supprimé et recréé.  
  
*index_name*  
Spécifie le nom du nouvel index.  
  
Si la table a déjà un index cluster columnstore, vous pouvez spécifier le même nom que l’index existant, ou vous pouvez utiliser l’option DROP EXISTING pour spécifier un nouveau nom.  
  
ON [*database_name*. [*schema_name* ] . | *schema_name* . ] *table_name*

Spécifie le nom à une, deux ou trois parties de la table à stocker en tant qu'index columnstore cluster. Si la table est un segment ou un index cluster, elle est convertie de rowstore en columnstore. Si la table est déjà un index columnstore, cette instruction reconstruit l’index cluster columnstore. Pour convertir en index columnstore en cluster ordonné l’index existant doit être un index cluster columnstore en cluster.
  
#### <a name="with-options"></a>Options WITH

##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON

   `DROP_EXISTING = ON` spécifie de supprimer l’index existant et de créer un index columnstore.  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (DROP_EXISTING = ON);
```
   La valeur par défaut, DROP_EXISTING = OFF attend que le nom de l’index soit le même que le nom existant. Une erreur se produit si le nom d’index spécifié existe déjà.  
  
##### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = *max_degree_of_parallelism*  
   Remplace la configuration de serveur « max degree of parallelism » existante pendant la durée de l'opération d'index. Utilisez MAXDOP pour limiter le nombre de processeurs utilisés dans une exécution de plan parallèle. Le nombre maximal de processeurs est égal à 64.  
  
   Les valeurs de *max_degree_of_parallelism* peuvent être :  
   - 1 - Supprime la génération de plan parallèle.  
   - \>1 - Limite le nombre maximal de processeurs utilisés dans l’indexation parallèle au nombre défini ou à un nombre inférieur en fonction de la charge de travail actuelle du système. Par exemple, si MAXDOP = 4, le nombre de processeurs utilisés est inférieur ou égal à 4.  
   - 0 (valeur par défaut) - Utilise le nombre réel de processeurs ou un nombre de processeurs inférieur en fonction de la charge de travail actuelle du système.  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (MAXDOP = 2);
```

   Pour plus d’informations, consultez [Configurer l’option de configuration du serveur Degré maximal de parallélisme](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) et [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
 
###### <a name="compression_delay--0--delay--minutes-"></a>COMPRESSION_DELAY = **0** | *délai* [ minutes ]  
   Pour une table sur disque, le *délai* spécifie le nombre minimal de minutes pendant lesquelles un rowgroup delta à l’état CLOSED doit rester dans le rowgroup delta avant que SQL Server puisse le compresser dans le rowgroup compressé. Étant donné que les tables sur disque ne surveillent pas les durées d’insertion et de mise à jour sur chaque ligne, SQL Server applique le délai aux rowgroups delta à l’état CLOSED.  
   La valeur par défaut est 0 minute.  
   
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( COMPRESSION_DELAY = 10 Minutes );
```

   Pour obtenir des recommandations concernant l’utilisation de COMPRESSION_DELAY, consultez [Prise en main de Columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
##### <a name="data_compression--columnstore--columnstore_archive"></a>DATA_COMPRESSION = COLUMNSTORE | COLUMNSTORE_ARCHIVE  
   Spécifie l'option de compression de données pour la table, le numéro de partition ou la plage de partitions spécifiés. Les options disponibles sont les suivantes :   
- `COLUMNSTORE` est la valeur par défaut et indique qu’il faut compresser avec la compression columnstore la plus performante. Il s’agit de l’option généralement choisie.  
- `COLUMNSTORE_ARCHIVE` compresse la partition ou la table en une taille encore plus petite. Utilisez cette option dans des cas où l’archivage nécessite un stockage plus petit et peut supporter une durée de stockage et de récupération plus longue.  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( DATA_COMPRESSION = COLUMNSTORE_ARCHIVE );
```
   Pour plus d’informations sur la compression, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).  

###### <a name="online--on--off"></a>ONLINE = [ON | OFF]
- `ON` indique que l’index columnstore reste en ligne et disponible pendant que la nouvelle copie de l’index est en cours de création.
- `OFF` indique que l’index n’est pas disponible pendant que la nouvelle copie est en cours de création.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( ONLINE = ON );
```

#### <a name="on-options"></a>Options ON 
   Avec les options ON, spécifiez des options pour le stockage des données, par exemple, un schéma de partition, un groupe de fichiers spécifique, ou le groupe de fichiers par défaut. Si l'option ON n'est pas spécifiée, l’index utilise les paramètres de la partition ou du groupe de fichiers de la table existante.  
  
   *partition_scheme_name* **(** _column_name_ **)**  
   Spécifie le schéma de partition de la table. Le schéma de partition doit déjà exister dans la base de données. Pour créer le schéma de partition, consultez [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md).  
 
   *column_name* désigne la colonne sur laquelle un index partitionné est partitionné. Cette colonne doit correspondre au type de données, à la longueur et à la précision de l’argument de la fonction de partition que *partition_scheme_name* utilise.  

   *filegroup_name*  
   Spécifie le groupe de fichiers pour le stockage de l'index columnstore cluster. Si aucun emplacement n'est défini et que la table n'est pas partitionnée, l'index utilise le même groupe de fichiers que la table ou vue sous-jacente. Le groupe de fichiers doit déjà exister.  

   **"** default **"**  
   Pour créer l’index sur le groupe de fichiers par défaut, utilisez la valeur "default" ou [ default ].  
  
   Si "default" est spécifié, l'option QUOTED_IDENTIFIER doit être activée (ON) pour la session active. QUOTED_IDENTIFIER est activé par défaut. Pour plus d’informations, consultez [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
### <a name="create-nonclustered-columnstore-index"></a>CREATE [NONCLUSTERED] COLUMNSTORE INDEX  
Créez un index columnstore non-cluster en mémoire sur une table rowstore stockée sous forme de segment ou d’index cluster. L’index peut avoir une condition filtrée et n’a pas besoin d’inclure toutes les colonnes de la table sous-jacente. L’index columnstore nécessite suffisamment d’espace pour stocker une copie des données. Il peut être mis à jour, et il l’est dès que la table sous-jacente est modifiée. L’index columnstore non-cluster sur un index cluster permet une analytique en temps réel.  
  
*index_name*  
   Spécifie le nom de l'index. *index_name* doit être unique dans la table, mais ne doit pas nécessairement l’être dans la base de données. Les noms d’index doivent se conformer aux règles régissant les [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 **(** _column_  [ **,** ...*n* ] **)**  
    Spécifie les colonnes à stocker. Un index columnstore non-cluster est limité à 1 024 colonnes.  
   Chaque colonne doit appartenir à un type de données pris en charge pour les index columnstore. Pour obtenir la liste des types de données pris en charge, consultez [Limitations et Restrictions](../../t-sql/statements/create-columnstore-index-transact-sql.md#LimitRest).  

ON [*database_name*. [*schema_name* ] . | *schema_name* . ] *table_name*  
   Spécifie un nom en une, deux ou trois parties pour la table qui contient l’index.  

#### <a name="with-options"></a>Options WITH
##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON  
   DROP_EXISTING = ON L’index existant est supprimé et recréé. Le nom d'index défini doit être identique à celui de l'index existant. Toutefois, la définition de l'index peut être modifiée. Par exemple, vous pouvez spécifier différentes colonnes, ou options d'index.
  
   DROP_EXISTING = OFF Une erreur s’affiche si le nom d’index spécifié existe déjà. Le type d'index ne peut pas être modifié à l'aide de DROP_EXISTING. Dans la syntaxe de compatibilité descendante, WITH DROP_EXISTING est équivalent à WITH DROP_EXISTING = ON.  

###### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = *max_degree_of_parallelism*  
   Remplace l’option de configuration [Configurer l’option de configuration du serveur Degré maximal de parallélisme](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) pendant la durée de l’opération d’index. Utilisez MAXDOP pour limiter le nombre de processeurs utilisés dans une exécution de plan parallèle. Le nombre maximal de processeurs est égal à 64.  
  
   Les valeurs de *max_degree_of_parallelism* peuvent être :  
   - 1 - Supprime la génération de plan parallèle.  
   - \>1 - Limite le nombre maximal de processeurs utilisés dans l’indexation parallèle au nombre défini ou à un nombre inférieur en fonction de la charge de travail actuelle du système. Par exemple, si MAXDOP = 4, le nombre de processeurs utilisés est inférieur ou égal à 4.  
   - 0 (valeur par défaut) - Utilise le nombre réel de processeurs ou un nombre de processeurs inférieur en fonction de la charge de travail actuelle du système.  
  
   Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Les opérations d’index parallèles ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msC](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
###### <a name="online--on--off"></a>ONLINE = [ON | OFF]   
- `ON` indique que l’index columnstore reste en ligne et disponible pendant que la nouvelle copie de l’index est en cours de création.
- `OFF` indique que l’index n’est pas disponible pendant que la nouvelle copie est en cours de création. Dans un index non-cluster, la table de base reste disponible. Seul l’index columnstore non-cluster n’est pas utilisé pour satisfaire les requêtes tant que le nouvel index n’est pas terminé. 

```sql
CREATE COLUMNSTORE INDEX ncci ON Sales.OrderLines (StockItemID, Quantity, UnitPrice, TaxRate) WITH ( ONLINE = ON );
```

##### <a name="compression_delay--0--delayminutes"></a>COMPRESSION_DELAY = **0** | \<delay>[Minutes]  
   Spécifie une durée minimale pendant laquelle une ligne doit rester dans un rowgroup delta avant qu’elle ne soit éligible pour la migration vers un rowgroup compressé. Par exemple, un client peut décider que si une ligne reste inchangée pendant 120 minutes, elle est éligible à la compression dans le format de stockage en colonnes. Pour les index columnstore des tables sur disque, nous ne suivons pas l’heure à laquelle une ligne a été insérée ou mise à jour, mais nous utilisons au lieu de cela l’heure de fermeture du rowgroup delta comme proxy pour la ligne. La durée par défaut est de 0 minute. Une ligne est migrée vers un stockage en colonnes après l’accumulation de 1 million de lignes dans un rowgroup delta et après avoir été marquée comme fermée.  
  
###### <a name="data_compression"></a>DATA_COMPRESSION  
   Spécifie l'option de compression de données pour la table, le numéro de partition ou la plage de partitions spécifiés. S'applique uniquement aux index columnstore, y compris aux index columnstore non cluster et cluster. Les options disponibles sont les suivantes :
   
- `COLUMNSTORE` : valeur par défaut qui indique qu’il faut compresser avec la compression columnstore la plus performante. Il s’agit de l’option généralement choisie.  
- `COLUMNSTORE_ARCHIVE` : compresse davantage la partition ou la table dans une taille plus petite. Peut être utilisé pour l'archivage, ou d'autres situations qui nécessitent moins de stockage et supportent plus de temps pour le stockage et la récupération.  
  
 Pour plus d’informations sur la compression, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).  
  
##### <a name="where-filter_expression--and-filter_expression-"></a>WHERE \<filter_expression> [ AND \<filter_expression> ]
  
   Appelé prédicat de filtre, spécifie les lignes à inclure dans l’index. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée des statistiques filtrées sur les lignes de données de l’index filtré.  
  
   Le prédicat de filtre utilise une logique de comparaison simple. Les comparaisons à l'aide de littéraux NULL ne sont pas autorisées avec les opérateurs de comparaison. Utilisez les opérateurs IS NULL et IS NOT NULL à la place.  
  
   Voici quelques exemples de prédicats de filtre pour la table `Production.BillOfMaterials` :  
   `WHERE StartDate > '20000101' AND EndDate <= '20000630'`    
   `WHERE ComponentID IN (533, 324, 753)`  
   `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
   
   Pour des conseils sur les index filtrés, consultez [Créer des index filtrés](../../relational-databases/indexes/create-filtered-indexes.md).  
  
#### <a name="on-options"></a>Options ON  
   Ces options spécifient les groupes de fichiers sur lesquels l’index est créé.  
  
*partition_scheme_name* **(** _column_name_ **)**  
   Spécifie le schéma de partition qui définit les groupes de fichiers auxquels les partitions d’un index partitionné sont mappées. Le schéma de partition doit exister dans la base de données en exécutant [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md). 
   *column_name* désigne la colonne sur laquelle un index partitionné est partitionné. Cette colonne doit correspondre au type de données, à la longueur et à la précision de l’argument de la fonction de partition que *partition_scheme_name* utilise. *column_name* n’est pas limité aux colonnes de la définition d’index. Lors du partitionnement d'un index columnstore, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ajoute la colonne de partitionnement en tant que colonne de l'index, si elle n'a pas déjà été spécifiée.  
   Si *partition_scheme_name* ou *filegroup* n’est pas spécifié et que la table est partitionnée, l’index est placé dans le même schéma de partition que la table sous-jacente, en utilisant la même colonne de partitionnement.  
   Un index columnstore sur une table partitionnée doit être aligné sur les partitions.  
   Pour plus d’informations sur les index de partitionnement, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  

*filegroup_name*  
   Spécifie un nom de groupe de fichiers sur lequel créer l'index. Si *flegroup_name* n’est pas défini et que la table n’est pas partitionnée, l’index utilise le même groupe de fichiers que la table sous-jacente. Le groupe de fichiers doit déjà exister.  
 
**"** default **"**  
Crée l'index spécifié sur le groupe de fichiers par défaut.  
  
Le terme « default », dans ce contexte, n'est pas un mot clé. Il s’agit de l’identificateur du groupe de fichiers par défaut et il doit être délimité, comme dans ON **"** default **"** ou ON **[** default **]** . Si "default" est spécifié, l'option QUOTED_IDENTIFIER doit être activée (ON) pour la session active. Il s'agit du paramètre par défaut. Pour plus d’informations, consultez [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert une autorisation ALTER sur la table.  
  
##  <a name="general-remarks"></a><a name="GenRemarks"></a> Remarques d’ordre général  
Créez un index columnstore sur une table temporaire. Lorsque la table est supprimée ou que la session prend fin, l'index est également supprimé.  

Un index Columnstore en cluster ordonné peut être créé sur les colonnes de tout type de données pris en charge dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] à l’exception des colonnes de type chaîne.  
 
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
  
     *L'affectation de la valeur ON à ANSI_WARNINGS affecte de manière implicite la valeur ON à ARITHABORT, lorsque le niveau de compatibilité de la base de données est d'au moins 90. Si le niveau de compatibilité de la base de données est au maximum de 80, la valeur ON doit être affectée de manière explicite à l'option ARITHABORT.  
  
 Si les options SET sont incorrectes, les conditions suivantes peuvent se vérifier :  
  
-   L'index filtré n'est pas créé.  
  
-   Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur et restaure les instructions INSERT, UPDATE DELETE ou MERGE qui modifient les données incluses dans l'index.  
  
-   L'optimiseur de requête ne prend en compte l'index dans le plan d'exécution pour aucune instruction Transact-SQL.  
  
 Pour plus d’informations sur les index filtrés, consultez [Créer des index filtrés](../../relational-databases/indexes/create-filtered-indexes.md). 
  
##  <a name="limitations-and-restrictions"></a><a name="LimitRest"></a> Limitations et restrictions  

**Chaque colonne dans un index columnstore doit appartenir à l’un des types de données métier courants suivants :** 
-   datetimeoffset [ ( *n* ) ]  
-   datetime2 [ ( *n* ) ]  
-   DATETIME  
-   smalldatetime  
-   Date  
-   time [ ( *n* ) ]  
-   float [ ( *n* ) ]  
-   real [ ( *n* ) ]  
-   decimal [ ( *precision* [ *, scale* ] **)** ]
-   numeric [ ( *precision* [ *, scale* ] **)** ]    
-   money  
-   SMALLMONEY  
-   bigint  
-   int  
-   SMALLINT  
-   TINYINT  
-   bit  
-   nvarchar [ ( *n* ) ] 
-   nvarchar(max) (s’applique à [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et au niveau Premium, au niveau Standard (S3 et ultérieur) et à tous les niveaux d’offres VCore, dans les index columnstore cluster uniquement)   
-   nchar [ ( *n* ) ]  
-   varchar [ ( *n* ) ]  
-   varchar(max) (s’applique à [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et au niveau Premium, au niveau Standard (S3 et ultérieur) et à tous les niveaux d’offres VCore, dans les index columnstore cluster uniquement)
-   char [ ( *n* ) ]  
-   varbinary [ ( *n* ) ] 
-   varbinary (max) (s’applique à [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et à Azure SQL Database au niveau Premium, au niveau Standard (S3 et ultérieur) et à tous les niveaux d’offres VCore, dans les index columnstore cluster uniquement)
-   binary [ ( *n* ) ]  
-   uniqueidentifier  (S’applique à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et ultérieur)
  
Si la table sous-jacente a une colonne d’un type de données non pris en charge pour les index columnstore, vous devez omettre cette colonne dans l’index columnstore non-cluster.  
  
**Les colonnes qui utilisent l’un des types de données suivants ne peuvent pas être incluses dans un index columnstore :**
-   ntext, text et image  
-   nvarchar(max), varchar(max) et varbinary(max) (S’applique à [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et antérieur, ainsi qu’aux index columnstore non-cluster) 
-   rowversion (et timestamp)  
-   sql_variant  
-   Types CLR (hierarchyid et types spatiaux)  
-   Xml  
-   uniqueidentifier (S’applique à [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])  

**Index columnstore non-cluster :**
-   Ne peut pas avoir plus de 1 024 colonnes.
-   Ne peut pas être créé comme un index basé sur les contraintes. Une table avec un index columnstore peut avoir des contraintes uniques, des contraintes de clé primaire et des contraintes de clé étrangère. Les contraintes sont toujours appliquées avec un index rowstore. Les contraintes ne peuvent pas être appliquées avec un index columnstore (en cluster ou non).
-   Ne peut pas inclure de colonne éparse.  
-   Ne peut pas être modifié en utilisant l’instruction **ALTER INDEX**. Pour modifier l'index non cluster, vous devez plutôt supprimer et recréer l'index columnstore. Vous pouvez utiliser **ALTER INDEX** pour désactiver et reconstruire un index columnstore.  
-   Ne peut pas être créé en utilisant le mot clé **INCLUDE**.  
-   Ne peut pas inclure les mots clés **ASC** ou **DESC** pour le tri de l’index. Les index columnstore sont triés en fonction des algorithmes de compression. Le tri éliminerait beaucoup des avantages en termes de performances.  
-   Ne peut pas inclure les colonnes LOB (large object) de type nvarchar(max), varchar(max) et varbinary(max) dans les index columnstore non-cluster. Seuls les index columnstore cluster prennent en charge les types LOB, depuis la version [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et Azure SQL Database configuré au niveau Premium, au niveau Standard (S3 et ultérieur) et à tous les niveaux d’offres VCore. Notez que les versions antérieures ne prennent pas en charge les types LOB dans des index columnstore cluster et non-cluster.


> [!NOTE]  
> À partir de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], vous pouvez créer un index columnstore non-cluster dans une vue indexée.  


 **Les index columnstore ne peuvent pas être combinés avec les fonctionnalités suivantes :**  
-   Colonnes calculées Depuis SQL Server 2017, un index cluster columnstore peut contenir une colonne calculée non persistante. Toutefois, dans SQL Server 2017, les index cluster columnstore ne peuvent pas contenir de colonnes calculées persistantes, et vous ne pouvez pas créer d’index non-cluster sur des colonnes calculées. 
-   Compression de pages et de lignes et format de stockage **vardecimal** (Un index columnstore est déjà compressé dans un autre format.)  
-   Réplication  
-   Filestream

Vous ne pouvez pas utiliser de curseurs ou de déclencheurs sur une table avec un index cluster columnstore. Cette restriction ne s’applique pas aux index columnstore non-cluster. En effet, vous pouvez utiliser des curseurs et des déclencheurs sur une table avec un index columnstore non-cluster.

**[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] limitations spécifiques**  
Ces limitations s’appliquent uniquement à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Dans cette version, nous avions introduit des index cluster columnstore qui pouvaient être mis à jour. Seuls les index columnstore non-cluster étaient toujours en lecture seule.  

-   Suivi des modifications. Vous ne pouvez pas utiliser le suivi des modifications avec des index columnstore.  
-   Capture des modifications de données. La capture des changements de données ne peut pas être activée sur les tables avec un index columnstore cluster. À compter de SQL Server 2016, elle peut être activée sur les tables avec un index columnstore non cluster.  
-   Secondaire accessible en lecture. Vous ne pouvez pas accéder à un index cluster columnstore (CCI) à partir d’un secondaire accessible en lecture d’un groupe de disponibilité Always OnReadable.  Par contre, vous pouvez accéder à un index columnstore non-cluster (NCCI) à partir d’un secondaire accessible en lecture.  
-   MARS (Multiple Active Result Sets). [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] utilise MARS pour les connexions en lecture seule à des tables avec un index columnstore. Toutefois, SQL [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ne prend pas en charge MARS pour les opérations de langage de manipulation de données (DML) simultanées sur une table avec un index columnstore. Le cas échéant, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] met fin aux connexions et abandonne les transactions.  
-  Les index columnstore non-cluster ne peuvent pas être créés dans une vue ou une vue indexée.
  
 Pour des informations sur les avantages au niveau des performances et sur les limitations des index columnstore, consultez [Vue d’ensemble des index columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md).
  
##  <a name="metadata"></a><a name="Metadata"></a> Métadonnées  
 Toutes les colonnes dans un index columnstore sont stockées dans les métadonnées en tant que colonnes incluses. L'index columnstore n'a pas de colonnes clés. Ces vues système fournissent des informations sur les index columnstore.  
  
-   [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
-   [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
-   [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)  
-   [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
-   [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)  
-   [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)  

##  <a name="examples-for-converting-a-rowstore-table-to-columnstore"></a><a name="convert"></a> Exemples de conversion d’une table rowstore en columnstore  
  
### <a name="a-convert-a-heap-to-a-clustered-columnstore-index"></a>R. Convertir un segment dans un index cluster columnstore  
 Cet exemple crée une table en tant que segment puis la convertit en un index columnstore cluster nommé cci_Simple. Cela modifie le stockage de la table entière qui change de rowstore en columnstore.  
  
```sql  
CREATE TABLE SimpleTable(  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cci_Simple ON SimpleTable;  
GO  
```  
  
### <a name="b-convert-a-clustered-index-to-a-clustered-columnstore-index-with-the-same-name"></a>B. Convertir un index cluster en un index columnstore cluster ayant le même nom.  
 Cet exemple suivant crée une table avec un index cluster, puis illustre la syntaxe de conversion de l'index cluster en un index columnstore cluster. Cela modifie le stockage de la table entière qui change de rowstore en columnstore.  
  
```sql  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cl_simple ON SimpleTable  
WITH (DROP_EXISTING = ON);  
GO  
```  
  
### <a name="c-handle-nonclustered-indexes-when-converting-a-rowstore-table-to-a-columnstore-index"></a>C. Gérer les index non-cluster lors de la conversion d’une table rowstore en index columnstore.  
 Cet exemple montre comment gérer les index non-cluster lors de la conversion d’une table rowstore en index columnstore. En fait, depuis [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], aucune action n’est requise en particulier ; [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit et reconstruit automatiquement les index non-cluster sur le nouvel index cluster columnstore.  
  
 Si vous souhaitez supprimer les index non-cluster, utilisez l’instruction DROP INDEX avant de créer l’index columnstore. L’option DROP EXISTING supprime uniquement l’index cluster en cours de conversion. Elle ne supprime pas les index non-cluster.  
  
 Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], vous ne pouviez pas créer un index non-cluster sur un index columnstore. Cet exemple montre comment vous devez supprimer les index non-cluster avant de créer l’index columnstore dans les versions précédentes.  
  
```sql  
--Create the table for use with this example.  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
  
--Create two nonclustered indexes for use with this example  
CREATE INDEX nc1_simple ON SimpleTable (OrderDateKey);  
CREATE INDEX nc2_simple ON SimpleTable (DueDateKey);   
GO  
  
--SQL Server 2012 and SQL Server 2014: you need to drop the nonclustered indexes  
--in order to create the columnstore index.   
  
DROP INDEX SimpleTable.nc1_simple;  
DROP INDEX SimpleTable.nc2_simple;  
  
--Convert the rowstore table to a columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_simple ON SimpleTable;   
GO  
```  
  
### <a name="d-convert-a-large-fact-table-from-rowstore-to-columnstore"></a>D. Convertir une table de faits volumineuse de rowstore en columnstore  
 Cet exemple explique comment convertir une table de faits volumineuse d'une table rowstore en une table columnstore.  
  
 Pour convertir une table rowstore en une table columnstore  
  
1.  Tout d'abord, créez une petite table pour cet exemple.  
  
    ```sql  
    --Create a rowstore table with a clustered index and a nonclustered index.  
    CREATE TABLE MyFactTable (  
        ProductKey [INT] NOT NULL,  
        OrderDateKey [INT] NOT NULL,  
         DueDateKey [INT] NOT NULL,  
         ShipDateKey [INT] NOT NULL )  
    )  
    WITH (  
        CLUSTERED INDEX ( ProductKey )  
    );  
  
    --Add a nonclustered index.  
    CREATE INDEX my_index ON MyFactTable ( ProductKey, OrderDateKey );  
    ```  
  
2.  Supprimez tous les index non-cluster de la table rowstore.  
  
    ```sql  
    --Drop all nonclustered indexes  
    DROP INDEX my_index ON MyFactTable;  
    ```  
  
3.  supprimer l'index cluster ;  
  
    -   Effectuez cette opération uniquement si vous souhaitez spécifier un nouveau pour l'index lorsqu'il est converti en un index columnstore cluster. Si vous ne supprimez pas l’index cluster, le nouvel index cluster columnstore porte le même nom.  
  
        > [!NOTE]  
        > Le nom de l'index peut être plus facile à mémoriser si vous utilisez votre propre nom. Tous les index cluster rowstore utilisent le nom par défaut qui est 'ClusteredIndex_\<GUID>'.  
  
    ```sql  
    --Process for dropping a clustered index.  
    --First, look up the name of the clustered rowstore index.  
    --Clustered rowstore indexes always use the DEFAULT name 'ClusteredIndex_<GUID>'.  
    SELECT i.name   
    FROM sys.indexes i   
    JOIN sys.tables t  
    ON ( i.type_desc = 'CLUSTERED' ) WHERE t.name = 'MyFactTable';  
  
    --Drop the clustered rowstore index.  
    DROP INDEX ClusteredIndex_d473567f7ea04d7aafcac5364c241e09 ON MyDimTable;  
    ```  
  
4.  Convertissez la table rowstore en une table columnstore avec un index columnstore cluster.  
  
    ```sql  
    --Option 1: Convert to columnstore and name the new clustered columnstore index MyCCI.  
    CREATE CLUSTERED COLUMNSTORE INDEX MyCCI ON MyFactTable;  
  
    --Option 2: Convert to columnstore and use the rowstore clustered   
    --index name for the columnstore clustered index name.  
    --First, look up the name of the clustered rowstore index.  
    SELECT i.name   
    FROM sys.indexes i  
    JOIN sys.tables t   
    ON ( i.type_desc = 'CLUSTERED' )  
    WHERE t.name = 'MyFactTable';  
  
    --Second, create the clustered columnstore index and   
    --Replace ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
    --with the name of your clustered index.  
    CREATE CLUSTERED COLUMNSTORE INDEX   
    ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
     ON MyFactTable  
    WITH DROP_EXISTING = ON;  
    ```  
  
### <a name="e-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>E. Convertir une table columnstore en une table rowstore avec un index cluster  
 Pour convertir une table columnstore en une table rowstore avec un index cluster, utilisez l'instruction CREATE INDEX avec l'option DROP_EXISTING.  
  
```sql  
CREATE CLUSTERED INDEX ci_MyTable   
ON MyFactTable  
WITH ( DROP EXISTING = ON );  
```  
  
### <a name="f-convert-a-columnstore-table-to-a-rowstore-heap"></a>F. Convertir une table columnstore en un segment de mémoire rowstore  
 Pour convertir une table columnstore en un segment de mémoire, supprimez simplement l'index columnstore cluster.  
  
```sql  
DROP INDEX MyCCI   
ON MyFactTable;  
```  
  

### <a name="g-defragment-by-rebuilding-the-entire-clustered-columnstore-index"></a>G. Défragmenter en reconstruisant l’index cluster columnstore en entier  
   S’applique à : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
 Il existe deux façons de reconstruire l'index cluster columnstore complet. Vous pouvez utiliser CREATE CLUSTERED COLUMNSTORE INDEX ou [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) et l’option REBUILD. Les deux méthodes permettent d'obtenir le même résultat.  
  
> [!NOTE]  
> En commençant par [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], utilisez `ALTER INDEX...REORGANIZE` au lieu de reconstruire avec les méthodes décrites dans cet exemple.  
  
```sql  
--Determine the Clustered Columnstore Index name of MyDimTable.  
SELECT i.object_id, i.name, t.object_id, t.name   
FROM sys.indexes i   
JOIN sys.tables t  
ON (i.type_desc = 'CLUSTERED COLUMNSTORE')  
WHERE t.name = 'RowstoreDimTable';  
  
--Rebuild the entire index by using CREATE CLUSTERED INDEX.  
CREATE CLUSTERED COLUMNSTORE INDEX my_CCI   
ON MyFactTable  
WITH ( DROP_EXISTING = ON );  
  
--Rebuild the entire index by using ALTER INDEX and the REBUILD option.  
ALTER INDEX my_CCI  
ON MyFactTable  
REBUILD PARTITION = ALL  
WITH ( DROP_EXISTING = ON );  
```  
  
##  <a name="examples-for-nonclustered-columnstore-indexes"></a><a name="nonclustered"></a> Exemples d’index columnstore non-cluster  
  
### <a name="a-create-a-columnstore-index-as-a-secondary-index-on-a-rowstore-table"></a>R. Créer un index columnstore comme index secondaire sur une table rowstore  
 Cet exemple crée un index columnstore non-cluster sur une table rowstore. Un seul index columnstore peut être créé dans ce cas de figure. L’index columnstore nécessite un stockage supplémentaire, car il contient une copie des données dans la table rowstore. Cet exemple crée une table simple et un index cluster, puis montre la syntaxe de création d'un index columnstore non-cluster.  
  
```sql  
CREATE TABLE SimpleTable  
(ProductKey [INT] NOT NULL,   
OrderDateKey [INT] NOT NULL,   
DueDateKey [INT] NOT NULL,   
ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey);  
GO  
```  
  
### <a name="b-create-a-simple-nonclustered-columnstore-index-using-all-options"></a>B. Créer un index columnstore non-cluster simple à l’aide de toutes les options  
 L'exemple suivant illustre la syntaxe de création d'un index columnstore non cluster à l'aide de toutes les options.  
  
```sql  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey)  
WITH (DROP_EXISTING =  ON,   
    MAXDOP = 2)  
ON "default"  
GO  
```  
  
 Pour un exemple plus complexe avec des tables partitionnées, consultez [Vue d’ensemble des index columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md).  
  
### <a name="c-create-a-nonclustered-columnstore-index-with-a-filtered-predicate"></a>C. Créer un index columnstore non-cluster avec un prédicat filtré  
 L’exemple suivant crée un index columnstore non-cluster filtré sur la table Production.BillOfMaterials de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Le prédicat de filtre peut inclure des colonnes qui ne sont pas des colonnes clés dans l'index filtré. Dans cet exemple, le prédicat sélectionne uniquement les lignes où EndDate n'est pas NULL.  
  
```sql  
IF EXISTS (SELECT name FROM sys.indexes  
    WHERE name = N'FIBillOfMaterialsWithEndDate'   
    AND object_id = OBJECT_ID(N'Production.BillOfMaterials'))  
DROP INDEX FIBillOfMaterialsWithEndDate  
    ON Production.BillOfMaterials;  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX "FIBillOfMaterialsWithEndDate"  
    ON Production.BillOfMaterials (ComponentID, StartDate)  
    WHERE EndDate IS NOT NULL;  
```  
  
###  <a name="d-change-the-data-in-a-nonclustered-columnstore-index"></a><a name="ncDML"></a> D. Changer les données dans un index columnstore non-cluster  
   S’applique à : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].
  
 Une fois que vous avez créé un index columnstore non cluster sur une table, vous ne pouvez pas modifier directement les données dans cette table. Une requête avec INSERT, UPDATE, DELETE ou MERGE échoue et retourne un message d’erreur. Pour ajouter ou modifier les données de la table, effectuez les tâches suivantes :  
  
-   Désactivez ou supprimez l'index columnstore. Vous pourrez ensuite mettre à jour les données de la table. Si vous désactivez l'index columnstore, vous pouvez le reconstruire lorsque vous avez fini de mettre à jour les données. Par exemple,  
  
    ```sql  
    ALTER INDEX mycolumnstoreindex ON mytable DISABLE;  
    -- update mytable --  
    ALTER INDEX mycolumnstoreindex on mytable REBUILD  
    ```  
  
-   Chargez les données dans une table intermédiaire qui n'a pas d'index columnstore. Créez un index columnstore sur la table intermédiaire. Basculez la table intermédiaire dans une partition vide de la table principale.  
  
-   Basculez une partition de la table avec l'index columnstore dans une table intermédiaire vide. S'il existe un index columnstore sur la table intermédiaire, désactivez-le. Effectuez toutes les mises à jour. Créez (ou reconstruisez) l'index columnstore. Rebasculez la table intermédiaire dans la partition (maintenant vide) de la table principale.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-change-a-clustered-index-to-a-clustered-columnstore-index"></a>R. Changer un index cluster en index cluster columnstore  
 En utilisant l’instruction CREATE CLUSTERED COLUMNSTORE INDEX avec DROP_EXISTING = ON, vous pouvez :  
  
-   Changer un index cluster en index cluster columnstore.  
  
-   Reconstruire un index cluster columnstore.  
  
 Cet exemple crée la table xDimProduct comme table rowstore avec un index cluster, puis utilise CREATE CLUSTERED COLUMNSTORE INDEX pour changer la table d’une table rowstore en table columnstore.  
  
```sql  
-- Uses AdventureWorks  
  
IF EXISTS (SELECT name FROM sys.tables  
    WHERE name = N'xDimProduct'  
    AND object_id = OBJECT_ID (N'xDimProduct'))  
DROP TABLE xDimProduct;  
  
--Create a distributed table with a clustered index.  
CREATE TABLE xDimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey)  
WITH ( DISTRIBUTION = HASH(ProductKey),  
    CLUSTERED INDEX (ProductKey) )  
AS SELECT ProductKey, ProductAlternateKey, ProductSubcategoryKey FROM DimProduct;  
  
--Change the existing clustered index   
--to a clustered columnstore index with the same name.  
--Look up the name of the index before running this statement.  
CREATE CLUSTERED COLUMNSTORE INDEX <index_name>   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="b-rebuild-a-clustered-columnstore-index"></a>B. Reconstruire un index cluster columnstore  
 S’appuyant sur l’exemple précédent, cet exemple utilise CREATE CLUSTERED COLUMNSTORE INDEX pour reconstruire l’index cluster columnstore existant appelé cci_xDimProduct.  
  
```sql  
--Rebuild the existing clustered columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_xDimProduct   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="c-change-the-name-of-a-clustered-columnstore-index"></a>C. Changer le nom d’un index cluster columnstore  
 Pour changer le nom d’un index cluster columnstore, supprimez l’index cluster columnstore existant, puis recréez l’index avec un nouveau nom.  
  
 Nous vous recommandons d’effectuer cette opération avec une petite table ou une table vide uniquement. La suppression d’un index cluster columnstore volumineux et la reconstruction avec un autre nom prennent beaucoup de temps.  
  
 En prenant l’index cluster columnstore cci_xDimProduct de l’exemple précédent, cet exemple supprime l’index cluster columnstore cci_xDimProduct et recrée l’index cluster columnstore avec le nom mycci_xDimProduct.  
  
```sql  
--For illustration purposes, drop the clustered columnstore index.   
--The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xDimProduct;  
  
--Create a clustered index with a new name, mycci_xDimProduct.  
CREATE CLUSTERED COLUMNSTORE INDEX mycci_xDimProduct  
ON xdimProduct  
WITH ( DROP_EXISTING = OFF );  
```  
  
### <a name="d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>D. Convertir une table columnstore en une table rowstore avec un index cluster  
 Il peut arriver que vous souhaitiez supprimer un index cluster columnstore et créer un index cluster. Dans ce cas, la table est stockée au format rowstore. Cet exemple convertit une table columnstore en table rowstore avec un index cluster du même nom. Aucune des données n’est perdue. Les données vont toutes dans la table rowstore, tandis que les colonnes listées deviennent les colonnes clés de l’index cluster.  
  
```sql  
--Drop the clustered columnstore index and create a clustered rowstore index.   
--All of the columns are stored in the rowstore clustered index.   
--The columns listed are the included columns in the index.  
CREATE CLUSTERED INDEX cci_xDimProduct    
ON xdimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey, WeightUnitMeasureCode)  
WITH ( DROP_EXISTING = ON);  
```  
  
### <a name="e-convert-a-columnstore-table-back-to-a-rowstore-heap"></a>E. Reconvertir une table columnstore en segment rowstore  
 Utilisez [DROP INDEX (SQL Server PDW)](drop-index-transact-sql.md) pour supprimer l’index cluster columnstore et convertir la table en segment rowstore. Cet exemple convertit la table cci_xDimProduct en segment rowstore. La table continue d’être distribuée, mais est stockée en tant que segment.  
  
```sql  
--Drop the clustered columnstore index. The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xdimProduct;  
```  

### <a name="f-create-an-ordered-clustered-columnstore-index-on-a-table-with-no-index"></a>F. Créer un index columnstore en cluster ordonné sur une table sans index

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
```

### <a name="g-convert-a-clustered-columnstore-index-to-an-ordered-clustered-columnstore-index"></a>G. Convertir un index columnstore en cluster en index columnstore en cluster ordonné

```sql  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
WITH (DROP_EXISTING = ON)
```

### <a name="h-add-a-column-to-the-ordering-of-an-ordered-clustered-columnstore-index"></a>H. Ajouter une colonne au classement d’un index columnstore en cluster ordonné

```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE column only.  Add PRODUCTKEY column to the ordering.
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE, PRODUCTKEY );
WITH (DROP_EXISTING = ON)
```
### <a name="i-change-the-ordinal-of-ordered-columns"></a>I. Modifier l’ordinal de colonnes ordonnées  
```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE, PRODUCTKEY.  Change the ordering to PRODUCTKEY, SHIPDATE.  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( PRODUCTKEY,SHIPDATE );
WITH (DROP_EXISTING = ON)
```