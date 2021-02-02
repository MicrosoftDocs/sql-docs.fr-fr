---
description: CREATE TABLE (Azure Synapse Analytics)
title: CREATE TABLE (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 07/03/2019
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: ea21c73c-40e8-4c54-83d4-46ca36b2cf73
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 336073c272094bb7ef5549aafd9f6e3b09211561
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99156736"
---
# <a name="create-table-azure-synapse-analytics"></a>CREATE TABLE (Azure Synapse Analytics)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Crée une table dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  

Pour comprendre les tables et savoir comment les utiliser, consultez [Tables dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-overview/).

> [!NOTE]
>  Les discussions sur les [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] dans cet article s’appliquent à la fois à [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , sauf indication contraire.

 ![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien d’article") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

<a name="Syntax"></a>

## <a name="syntax"></a>Syntaxe
  
```syntaxsql
-- Create a new table.
CREATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( 
      { column_name <data_type>  [ <column_options> ] } [ ,...n ]
    )  
    [ WITH ( <table_option> [ ,...n ] ) ]  
[;]  

<column_options> ::=
    [ COLLATE Windows_collation_name ]  
    [ NULL | NOT NULL ] -- default is NULL  
    [ <column_constraint> ]

<column_constraint>::=
    {
        DEFAULT DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED  NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }

<table_option> ::=
    {
       CLUSTERED COLUMNSTORE INDEX --default for Azure Synapse Analytics 
      | CLUSTERED COLUMNSTORE INDEX ORDER (column [,...n])  
      | HEAP --default for Parallel Data Warehouse
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) -- default is ASC
    }  
    {
        DISTRIBUTION = HASH ( distribution_column_name )
      | DISTRIBUTION = ROUND_ROBIN -- default for Azure Synapse Analytics
      | DISTRIBUTION = REPLICATE -- default for Parallel Data Warehouse
    }
    | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] -- default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) )

<data type> ::=
      datetimeoffset [ ( n ) ]  
    | datetime2 [ ( n ) ]  
    | datetime  
    | smalldatetime  
    | date  
    | time [ ( n ) ]  
    | float [ ( n ) ]  
    | real [ ( n ) ]  
    | decimal [ ( precision [ , scale ] ) ]   
    | numeric [ ( precision [ , scale ] ) ]   
    | money  
    | smallmoney  
    | bigint  
    | int   
    | smallint  
    | tinyint  
    | bit  
    | nvarchar [ ( n | max ) ]  -- max applies only to Azure Synapse Analytics 
    | nchar [ ( n ) ]  
    | varchar [ ( n | max )  ] -- max applies only to Azure Synapse Analytics  
    | char [ ( n ) ]  
    | varbinary [ ( n | max ) ] -- max applies only to Azure Synapse Analytics  
    | binary [ ( n ) ]  
    | uniqueidentifier  
```  

<a name="Arguments"></a>
## <a name="arguments"></a>Arguments

 *database_name*  
 Nom de la base de données qui contiendra la nouvelle table. La valeur par défaut est la base de données active.  
  
 *schema_name*  
 Schéma de la table. La définition du *schéma* est facultative. Si cet argument est vide, le schéma par défaut est utilisé.  
  
 *table_name*  
 Nom de la nouvelle table. Pour créer une table temporaire locale, faites précéder le nom de la table de #.  Pour obtenir des explications et des aides sur les tables temporaires, consultez [des tables temporaires dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-temporary/). 

 *column_name*  
 Nom d’une colonne de table.

### <a name="column-options"></a><a name="ColumnOptions"></a> Options de colonne

 `COLLATE` *Windows_collation_name*  
 Spécifie le classement de l’expression. Le classement doit correspondre à l’un des classements Windows pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des classements Windows pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Nom de classement Windows (Transact-SQL)](windows-collation-name-transact-sql.md)/).  
  
 `NULL` | `NOT NULL`  
 Indique si les valeurs `NULL` sont autorisées dans la colonne. Par défaut, il s’agit de `NULL`.  
  
 [ `CONSTRAINT` *constraint_name* ] `DEFAULT` *constant_expression*  
 Spécifie la valeur de colonne par défaut.  
  
 | Argument | Explication |
 | -------- | ----------- |
 | *constraint_name* | Nom facultatif de la contrainte. Le nom de contrainte est unique dans la base de données. Le nom peut être réutilisé dans d’autres bases de données. |
 | *constant_expression* | Valeur par défaut de la colonne. L’expression doit être une valeur littérale ou une constante. Par exemple, ces expressions constantes sont autorisées : `'CA'`, `4`. Ces expressions constantes sont autorisées : `2+3`, `CURRENT_TIMESTAMP`. |
  
### <a name="table-structure-options"></a><a name="TableOptions"></a> Options de structure de table

Pour obtenir de l’aide sur le choix du type de table, consultez [Indexation de tables dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index/).
  
 `CLUSTERED COLUMNSTORE INDEX` 
 
Stocke la table sous forme d’index cluster columnstore. L’index cluster columnstore s’applique à toutes les données de table. Il s'agit du comportement par défaut pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].
 
 `HEAP` Stocke la table sous forme de segment de mémoire. Il s'agit du comportement par défaut pour [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 `CLUSTERED INDEX` ( *index_column_name* [ ,...*n* ] )  
 Stocke le tableau sous forme d’index cluster avec une ou plusieurs colonnes clés. Ce comportement stocke les données par ligne. Utilisez *index_column_name* pour spécifier le nom d’une ou plusieurs colonnes clés dans l’index.  Pour plus d’informations, consultez Tables Rowstore dans la section Remarques d’ordre général.
 
 `LOCATION = USER_DB` Cette fonction est déconseillée. Bien qu’elle soit acceptée du point de vue de la syntaxe, elle n’est plus nécessaire et n’a plus d’effet sur le comportement.   
  
### <a name="table-distribution-options"></a><a name="TableDistributionOptions"></a> Options de distribution de table

Pour comprendre comment choisir la meilleure méthode de distribution et utiliser des tables distribuées, consultez [Distribution de tables dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-distribute/).

`DISTRIBUTION = HASH` ( *distribution_column_name* ) Affecte chaque ligne à une distribution en hachant la valeur stockée dans *distribution_column_name*. L’algorithme est déterministe, ce qui signifie qu’il hache toujours la même valeur pour la même distribution.  La colonne de distribution doit être définie comme étant une valeur NOT NULL, car toutes les lignes de valeur NULL sont affectées à la même distribution.

`DISTRIBUTION = ROUND_ROBIN` Distribue les lignes uniformément entre toutes les distributions selon le principe du tourniquet (round robin). Il s'agit du comportement par défaut pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].

`DISTRIBUTION = REPLICATE` Stocke une copie de la table sur chaque nœud de calcul. Pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], la table est stockée dans une base de données de distribution sur chaque nœud de calcul. Pour [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], la table est stockée dans un groupe de fichiers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui englobe le nœud de calcul. Il s'agit du comportement par défaut pour [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
  
### <a name="table-partition-options"></a><a name="TablePartitionOptions"></a> Options de partitions de table
Pour obtenir de l’aide sur les partitions de table, consultez [Partitionnement de tables dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/).

 `PARTITION` ( *partition_column_name* `RANGE` [ `LEFT` | `RIGHT` ] `FOR VALUES` ( [ *boundary_value* [,...*n*] ] ))   
Crée une ou plusieurs partitions de table. Ces partitions sont des coupes de table horizontales qui vous permettent d’effectuer des opérations sur des sous-ensembles de lignes, que la table soit stockée sous forme de segment de mémoire, d’index cluster ou d’index cluster columnstore. Contrairement aux colonnes de distribution, les partitions de table ne déterminent dans quelle distribution chaque ligne est stockée. En revanche, elles déterminent la façon dont les lignes sont regroupées et stockées dans chaque distribution.  

| Argument | Explication |
| -------- | ----------- |
|*partition_column_name*| Indique la colonne que [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] utilisera pour partitionner les lignes. Cette colonne peut être de n’importe quel type de données. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] trie les valeurs de colonne de partition par ordre croissant. L’ordre croissant va de `LEFT` à `RIGHT` dans la spécification `RANGE`. |  
| `RANGE LEFT` | Indique que la valeur limite fait partie de la partition de gauche (valeurs inférieures). La valeur par défaut est LEFT. |
| `RANGE RIGHT` | Indique que la valeur limite fait partie de la partition de droite (valeurs supérieures). | 
| `FOR VALUES` ( *boundary_value* [,...*n*] ) | Spécifie les valeurs limites de la partition. *boundary_value* est une expression constante. Ne peut pas être NULL. Son type de données doit correspondre à celui de *partition_column_name* ou être implicitement convertible dans celui-ci. Elle ne peut pas être tronquée lors d’une conversion implicite de telle sorte que la taille et l’échelle de la valeur ne correspondent pas au type de données de *partition_column_name*<br></br><br></br>Si vous spécifiez la clause `PARTITION`, mais que vous ne spécifiez pas de valeur limite, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] crée une table partitionnée à une partition. Le cas échéant, vous pouvez par la suite scinder la table en deux partitions.<br></br><br></br>Si vous spécifiez une valeur limite, la table obtenue comprend deux partitions : l’une contenant les valeurs inférieures à la valeur limite et l’autre contenant les valeurs supérieures à la valeur limite. Si vous déplacez une partition dans une table non partitionnée, celle-ci reçoit les données, mais les limites de partition en figurent pas dans ses métadonnées.| 

 Consultez [Créer une table partitionnée](#PartitionedTable) dans la section Exemples.

### <a name="ordered-clustered-columnstore-index-option"></a>Option Index columnstore cluster ordonné 

Index columnstore en cluster (CCI) est la valeur par défaut pour la création de tables dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  Les données d’un index columnstore cluster ne sont pas triées avant d’être compressées dans des segments columnstore.  Lors de la création d’un index columnstore cluster avec ORDER, les données sont triées avant d’être ajoutées aux segments d’index et les performances des requêtes peuvent être améliorées. Pour plus d’informations, consultez [Réglage des performances avec un index columstore cluster ordonné](/azure/sql-data-warehouse/performance-tuning-ordered-cci?view=azure-sqldw-latest&preserve-view=true).  

Une CCI ordonnée peut être créée sur les colonnes de tous les types de données pris en charge dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] à l’exception des colonnes de type chaîne.  

Les utilisateurs peuvent interroger la colonne **column_store_order_ordinal** dans **sys.index_columns** pour obtenir la ou les colonnes sur lesquelles une table est ordonnée et la séquence dans le classement.  

Pour plus d’informations, consultez [Optimisation des performances avec un index columnstore cluster trié](/azure/sql-data-warehouse/performance-tuning-ordered-cci).   

### <a name="data-type"></a><a name="DataTypes"></a> Type de données

[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] prend en charge les types de données les plus couramment utilisés. Vous trouverez ci-dessous la liste des types de données pris en charge, les détails les concernant, ainsi que leur taille de stockage (en octets). Pour mieux comprendre les types de données et savoir comment les utiliser, consultez [Types de données pour les tables dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-data-types).

Pour obtenir un tableau des conversions des types de données, consultez la section Conversions implicites de la rubrique [CAST et CONVERT (Transact-SQL)](../functions/cast-and-convert-transact-sql.md).

>[!NOTE]
>Consultez [Types de données et fonctions de la Date et heure &#40;Transact-SQL&#41;](../functions/date-and-time-data-types-and-functions-transact-sql.md) pour plus d’informations.

`datetimeoffset` [ ( *n* ) ]  
 La valeur par défaut pour *n* est 7.  
  
 `datetime2` [ ( *n* ) ]  
Identique à `datetime`, sauf que vous pouvez spécifier le nombre de fractions de seconde. La valeur par défaut pour *n* est `7`.  
  
|Valeur *n*|Precision|Scale|  
|--:|--:|-:|  
|`0`|19|0|  
|`1`|21|1|  
|`2`|22|2|  
|`3`|23|3|  
|`4`|24|4|  
|`5`|25|5|  
|`6`|26|6|  
|`7`|27|7|  
  
 `datetime`  
 Stocke la date et l’heure du jour avec entre 19 et 23 caractères selon le calendrier grégorien. La date peut contenir l’année, le mois et le jour. L’heure contient l’heure, les minutes et les secondes. Vous pouvez éventuellement afficher trois chiffres pour les fractions de seconde. La taille de stockage est de 8 octets.  
  
 `smalldatetime`  
 Stocke une date et une heure. La taille de stockage est de 4 octets.  
  
 `date`  
 Stocke une date en utilisant au maximum 10 caractères pour l’année, le mois et le jour selon le calendrier grégorien. La taille de stockage est de 3 octets. La date est stockée sous forme d’entier.  
  
 `time` [ ( *n* ) ]  
 La valeur par défaut pour *n* est `7`.  
  
 `float` [ ( *n* ) ]  
 Type de données numériques approximatives à utiliser avec des données numériques à virgule flottante. Les données à virgule flottante sont approximatives, ce qui signifie que certaines valeurs de ce type de données ne peuvent pas être représentées de manière précise. *n* spécifie le nombre de bits utilisés pour stocker la mantisse de `float` en notation scientifique. *n* détermine la précision et la taille du stockage. Si *n* est spécifié, sa valeur doit être comprise entre `1` et `53`. La valeur par défaut de *n* est `53`.  
  
| Valeur *n* | Precision | Taille de stockage |  
| --------: | --------: | -----------: |  
| 1-24   | 7 chiffres  | 4 octets      |  
| 25-53  | 15 chiffres | 8 octets      |  
  
 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] considère *n* comme l’une des deux valeurs possibles. Si `1`<= *n* <= `24`, *n* est considéré comme égal à `24`. Si `25` <= *n* <= `53`, *n* est considéré comme égal à `53`.  
  
 Le type de données [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] `float` est conforme à la norme ISO pour toutes les valeurs de *n* entre `1` et `53`. Le synonyme de double précision est `float(53)`.  
  
 `real` [ ( *n* ) ]  
 La définition de real est identique à celle de float. Le synonyme ISO de `real` est `float(24)`.  
  
 `decimal` [ ( *precision* [ *, scale* ] ) ] | `numeric` [ ( *precision* [ *, scale* ] ) ]  
 Stocke les valeurs de précision et d’échelle fixes.  
  
 *precision*  
 Nombre total maximal de chiffres décimaux qui peuvent être stockés, aussi bien à gauche qu’à droite de la décimale. La précision doit être une valeur comprise entre `1` et la précision maximale de `38`. La précision par défaut est `18`.  
  
 *scale*  
 Nombre maximal de chiffres décimaux à droite de la virgule. La valeur de *scale* doit être comprise entre `0` et la valeur de *precision*. Vous ne pouvez spécifier *scale* que si *precision* est spécifié. La valeur par défaut de scale est `0` ; par conséquent, `0` <= *scale* <= *precision*. Les tailles de stockage maximales varient en fonction de la précision.  
  
| Precision | Taille de stockage (octets)  |  
| ---------: |-------------: |  
|  1-9       |             5 |  
| 10-19      |             9 |  
| 20-28      |            13 |  
| 29-38      |            17 |  
  
 `money` | `smallmoney`  
 Types de données représentant les valeurs monétaires.  
  
| Type de données | Taille de stockage (octets) |  
| --------- | ------------: |  
| `money`|8|  
| `smallmoney` |4|  
  
 `bigint` \| `int` \| `smallint` \| `tinyint`  
 Types de données représentant des valeurs numériques exactes qui utilisent des entiers. Le stockage est présenté dans le tableau suivant.  
  
| Type de données | Taille de stockage (octets) |  
| --------- | ------------: |  
| `bigint`|8|  
| `int` |4|  
| `smallint` |2|  
| `tinyint` |1|  
  
 `bit`  
 Type de données entier qui peut prendre la valeur `1`, `0` ou NULL. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] optimise le stockage des colonnes de bits. Si une table contient 8 colonnes de bits ou moins, celles-ci sont stockées comme 1 octet. Si elle contient entre 9 et 16 colonnes de bits, celles-ci sont stockées comme 2 octets, etc.  
  
 `nvarchar` [ ( *n* | `max` ) ]  -- `max` s’applique uniquement à [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
 Données de type caractères Unicode de longueur variable. *n* peut être une valeur comprise entre 1 et 4000. `max` indique que la taille de stockage maximale occupée est de 2^31-1 octets (2 Go). La taille de stockage en octets correspond à deux fois le nombre de caractères entrés plus 2 octets. Les données saisies peuvent avoir une longueur de zéro caractère.  
  
 `nchar` [ ( *n* ) ]  
 Données caractères Unicode d’une longueur fixe de *n* caractères. *n* doit être une valeur comprise entre `1` et `4000`. La taille de stockage est le double de *n* octets.  
  
 `varchar` [ ( *n*  | `max` ) ]  -- `max` s’applique uniquement à [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].   
 Données caractères non-Unicode d’une longueur variable de *n* octets. *n* doit être une valeur comprise entre `1` et `8000`. `max` indique que la taille de stockage maximale est de 2^31-1 octets (2 Go). La taille de stockage correspond à la longueur réelle des données entrées + 2 octets.  
  
 `char` [ ( *n* ) ]  
 Données caractères non-Unicode d’une longueur fixe de *n* octets. *n* doit être une valeur comprise entre `1` et `8000`. La taille de stockage est égale à *n* octets. La valeur par défaut de *n* est `1`.  
  
 `varbinary` [ ( *n*  | `max` ) ]  -- `max` s’applique uniquement à [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
 Données binaires de longueur variable. *n* peut être une valeur comprise entre `1` et `8000`. `max` indique que la taille de stockage maximale occupée est de 2^31-1 octets (2 Go). La taille de stockage est la longueur réelle des données entrées + 2 octets. La valeur par défaut pour *n* est 7.  
  
 `binary` [ ( *n* ) ]  
 Données binaires d’une longueur fixe de *n* octets. *n* peut être une valeur comprise entre `1` et `8000`. La taille de stockage est égale à *n* octets. La valeur par défaut pour *n* est `7`.  
  
 `uniqueidentifier`  
 GUID sur 16 octets.  
   
<a name="Permissions"></a>  
## <a name="permissions"></a>Autorisations  
 La création d’une table nécessite une autorisation dans le rôle de base de données fixe `db_ddladmin` ou :
 - Une autorisation `CREATE TABLE` au niveau de la base de données
 - Une autorisation `ALTER SCHEMA` au niveau du schéma qui contient la table. 

La création d’une table partitionnée nécessite une autorisation dans le rôle de base de données fixe `db_ddladmin` ou

- Une autorisation `ALTER ANY DATASPACE`
  
 La connexion qui crée une table temporaire locale bénéficie des autorisations `CONTROL`, `INSERT`, `SELECT` et `UPDATE` au niveau de la table.  
 
<a name="GeneralRemarks"></a>  
## <a name="general-remarks"></a>Remarques d'ordre général  
 
Pour les limites minimales et maximales, consultez [[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] limites de capacité](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-service-capacity-limits/). 
 
### <a name="determining-the-number-of-table-partitions"></a>Détermination du nombre de partitions dans une table
Chaque table définie par l’utilisateur est divisée en plusieurs tables de plus petite taille qui sont stockées dans des emplacements distincts appelés distributions. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] utilise 60 distributions. Dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], le nombre de distributions varie en fonction du nombre de nœuds de calcul.
 
Chaque distribution contient toutes les partitions de table. Par exemple, s’il existe 60 distributions et quatre partitions de table plus une partition vide, il y aura 300 partitions (5 x 60 = 300). Si la table est un index cluster columnstore, il n’y aura qu’un seul index columnstore par partition, ce qui signifie qu’il y aura 300 index columnstore.

Nous vous recommandons d’utiliser moins de partitions de table de sorte que chaque index columnstore contienne suffisamment de lignes pour pouvoir profiter des avantages liés aux index columnstore. Pour plus d’informations, consultez [Partitionnement des tables dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/) et [Indexation des tables dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/)  

### <a name="rowstore-table-heap-or-clustered-index"></a>Table rowstore (segment de mémoire ou index cluster)

Une table rowstore est une table stockée ligne par ligne. Il s’agit d’un segment de mémoire ou d’un index cluster. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] crée toutes les tables rowstore avec la compression de page ; ce comportement ne peut pas être configuré par l’utilisateur.

### <a name="columnstore-table-columnstore-index"></a>Table columnstore (index columnstore)

Une table columnstore est une table stockée colonne par colonne. L’index columnstore est la technologie qui gère les données stockées dans une table columnstore.  L’index cluster columnstore n’affecte pas la façon dont les données sont distribuées. En revanche, il affecte la façon dont les données sont stockées dans chaque distribution.

Pour changer une table rowstore en table columnstore, supprimez tous les index existants de la table et créez un index cluster columnstore. Pour obtenir un exemple, consultez [CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md).

Pour plus d’informations, voir les articles suivants :
- [Synthèse des fonctionnalités des index columnstore en fonction des versions](../../relational-databases/indexes/columnstore-indexes-what-s-new.md)
- [Indexation des tables dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/)
- [Description des index columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md) 

<a name="LimitationsRestrictions"></a>  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Vous ne pouvez pas définir une contrainte DEFAULT au niveau d’une colonne de distribution.  
  
### <a name="partitions"></a>Partitions
Quand des partitions sont utilisées, la colonne de partition ne peut pas avoir de classement Unicode uniquement. Par exemple, l’instruction suivante échoue.  
  
 ```sql
CREATE TABLE t1 ( c1 varchar(20) COLLATE Divehi_90_CI_AS_KS_WS) WITH (PARTITION (c1 RANGE FOR VALUES (N'')))
```  
 
 Si *boundary_value* est une valeur littérale qui doit être convertie implicitement dans le type de données de *partition_column_name*, cela produit un écart. La valeur littérale s’affiche via les vues système [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], mais la valeur convertie est utilisée pour des opérations [!INCLUDE[tsql](../../includes/tsql-md.md)]. 

### <a name="temporary-tables"></a>tables temporaires ;

 Les tables temporaires globales qui commencent par ## ne sont pas prises en charge.  
  
 Les tables temporaires locales sont soumises aux limitations et restrictions suivantes :  
  
-   Elles sont visibles uniquement dans la session active. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] les supprime automatiquement à la fin de la session. Pour les supprimer de façon explicite, utilisez l’instruction DROP TABLE.   
-   Elles ne peuvent pas être renommées. 
-   Elles ne peuvent pas contenir de partitions ou de vues.  
-   Leurs autorisations ne peuvent pas être modifiées. Les instructions `GRANT`, `DENY` et `REVOKE` ne peuvent pas être utilisées avec des tables temporaires locales.   
-   Les commandes DBCC (Database Console Command) sont bloquées pour les tables temporaires.   
-   Si plusieurs tables temporaires locales sont utilisées dans un traitement, chacune doit avoir un nom unique. Si plusieurs sessions exécutent le même traitement et créent la même table temporaire locale, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ajoute en interne un suffixe numérique au nom de table temporaire locale de sorte que chaque table temporaire locale conserve un nom unique.  
    
<a name="LockingBehavior"></a>   
## <a name="locking-behavior"></a>Comportement du verrouillage  
 Applique un verrou exclusif sur la table. Applique un verrou partagé sur les objets DATABASE, SCHEMA et SCHEMARESOLUTION.  
 

<a name="ExamplesColumn"></a>   
## <a name="examples-for-columns"></a>Exemples de colonnes

### <a name="a-specify-a-column-collation"></a><a name="ColumnCollation"></a> A. Spécifier un classement de colonne 
 Dans l’exemple suivant, la table `MyTable` est créée avec deux classements de colonne différents. Par défaut, la colonne `mycolumn1` possède le classement par défaut Latin1_General_100_CI_AS_KS_WS. La colonne `mycolumn2` possède le classement Frisian_100_CS_AS.  
  
```sql
CREATE TABLE MyTable   
  (  
    mycolumnnn1 nvarchar,  
    mycolumn2 nvarchar COLLATE Frisian_100_CS_AS )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```  
  
### <a name="b-specify-a-default-constraint-for-a-column"></a><a name="DefaultConstraint"></a> B. Spécifier une contrainte DEFAULT pour une colonne

 L’exemple suivant illustre la syntaxe à utiliser pour spécifier la valeur par défaut d’une colonne. La colonne colA est assortie d’une contrainte par défaut nommée constraint_colA et d’une valeur par défaut de 0.  
  
```sql
CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```

<a name="ExamplesTemporaryTables"></a> 
## <a name="examples-for-temporary-tables"></a>Exemples de tables temporaires

### <a name="c-create-a-local-temporary-table"></a><a name="TemporaryTable"></a> C. Créer une table temporaire locale  
 Dans l’exemple ci-dessous, une table temporaire locale est créée sous le nom #myTable. La table est spécifiée avec un nom en trois parties, qui commence par #.
  
```sql
CREATE TABLE AdventureWorks.dbo.#myTable
  (  
   id int NOT NULL,  
   lastName varchar(20),  
   zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```

<a name="ExTableStructure"></a>  
## <a name="examples-for-table-structure"></a>Exemples de structure de table

### <a name="d-create-a-table-with-a-clustered-columnstore-index"></a><a name="ClusteredColumnstoreIndex"></a> D. Créer une table avec un index cluster columnstore  
 L’exemple suivant crée une table distribuée avec un index cluster columnstore. Chaque distribution est stockée sous forme de columnstore.  
  
 L’index cluster columnstore n’affecte pas la façon dont les données sont distribuées ; les données sont toujours distribuées par ligne. L’index cluster columnstore affecte la façon dont les données sont stockées dans chaque distribution.  
  
```sql
  CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH   
  (   
    DISTRIBUTION = HASH ( colB ),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  

### <a name="e-create-an-ordered-clustered-columnstore-index"></a><a name="OrderedClusteredColumnstoreIndex"></a> E. Créer un index columnstore en cluster ordonné

L’exemple suivant montre comment créer un index columnstore en cluster ordonné. L’index est ordonné sur SHIPDATE.

```sql
CREATE TABLE Lineitem  
WITH (DISTRIBUTION = ROUND_ROBIN, CLUSTERED COLUMNSTORE INDEX ORDER(SHIPDATE))  
AS  
SELECT * FROM ext_Lineitem
```

<a name="ExTableDistribution"></a> 
## <a name="examples-for-table-distribution"></a>Exemples de distribution de table

### <a name="f-create-a-round_robin-table"></a><a name="RoundRobin"></a> F. Créer une table ROUND_ROBIN  
 L’exemple suivant crée une table ROUND_ROBIN à trois colonnes et aucune partition. Les données sont réparties entre toutes les distributions. La table est créée avec un index cluster columnstore (CLUSTERED COLUMNSTORE INDEX), qui offre de meilleures performances et une meilleure compression de données qu’un segment de mémoire ou un index cluster rowstore.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );  
```  
  
### <a name="g-create-a-hash-distributed-table"></a><a name="HashDistributed"></a> G. Créer une table distribuée avec hachage

 L’exemple ci-dessous crée la même table que l’exemple précédent. Cependant, pour cette table, les lignes sont distribuées (dans la colonne `id`) et non réparties de façon aléatoire comme une table ROUND_ROBIN. La table est créée avec un index cluster columnstore (CLUSTERED COLUMNSTORE INDEX), qui offre de meilleures performances et une meilleure compression de données qu’un segment de mémoire ou un index cluster rowstore.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),   
    CLUSTERED COLUMNSTORE INDEX  
  );  
```  
  
### <a name="h-create-a-replicated-table"></a><a name="Replicated"></a> H. Créer une table répliquée  
 L’exemple suivant crée une table répliquée semblable à celles des exemples précédents. Les tables répliquées sont copiées en totalité dans chaque nœud de calcul. De ce fait, le déplacement de données est réduit pour les requêtes. Cet exemple est créé avec un CLUSTERED INDEX, qui offre une meilleure compression des données qu’un segment de mémoire. Un segment de mémoire peut ne pas contenir suffisamment de lignes pour obtenir une bonne compression CLUSTERED COLUMNSTORE INDEX.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = REPLICATE,
    CLUSTERED INDEX (lastName)  
  );  
```  

<a name="ExTablePartitions"></a> 
## <a name="examples-for-table-partitions"></a>Exemples de partitions de table

###  <a name="i-create-a-partitioned-table"></a><a name="PartitionedTable"></a> I. Créer une table partitionnée

 L’exemple suivant crée la même table que dans l’exemple A, mais avec en plus un partitionnement RANGE LEFT dans la colonne `id`. Quatre valeurs limites de partition y sont spécifiées, ce qui donne au total cinq partitions.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
  (
  
    PARTITION ( id RANGE LEFT FOR VALUES (10, 20, 30, 40 )),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  
  
 Dans cet exemple, les données seront triées dans les partitions suivantes :  
  
- Partition 1 : col <= 10
- Partition 2 : 10 < col <= 20
- Partition 3 : 20 < col <= 30
- Partition 4 : 30 < col <= 40
- Partition 5 : 40 < col  
  
 Si cette même table était partitionnée avec RANGE RIGHT au lieu de RANGE LEFT (par défaut), les données seraient triées dans les partitions suivantes :  
  
- Partition 1 : col < 10  
- Partition 2 : 10 <= col < 20
- Partition 3 : 20 <= col < 30
- Partition 4 : 30 <= col < 40
- Partition 5 : 40 <= col  
  
### <a name="j-create-a-partitioned-table-with-one-partition"></a><a name="OnePartition"></a> J. Créer une table partitionnée à une partition

 L’exemple suivant crée une table partitionnée à une partition. Aucune valeur limite n’y étant spécifiée, elle contient une seule partition.  
  
```sql
CREATE TABLE myTable (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
    (
      PARTITION ( id RANGE LEFT FOR VALUES ( )),  
      CLUSTERED COLUMNSTORE INDEX  
    )  
;  
```  
  
### <a name="k-create-a-table-with-date-partitioning"></a><a name="DatePartition"></a> K. Créer une table avec un partitionnement par date

 L’exemple suivant crée une table sous le nom `myTable`, avec un partitionnement dans une colonne `date`. Comme RANGE RIGHT est utilisé et que les valeurs limites sont des dates, il place un mois de données dans chaque partition.  
  
```sql
CREATE TABLE myTable (  
    l_orderkey      bigint,
    l_partkey       bigint,
    l_suppkey       bigint,
    l_linenumber    bigint,
    l_quantity      decimal(15,2),  
    l_extendedprice decimal(15,2),  
    l_discount      decimal(15,2),  
    l_tax           decimal(15,2),  
    l_returnflag    char(1),  
    l_linestatus    char(1),  
    l_shipdate      date,  
    l_commitdate    date,  
    l_receiptdate   date,  
    l_shipinstruct  char(25),  
    l_shipmode      char(10),  
    l_comment       varchar(44))  
WITH
  (
    DISTRIBUTION = HASH (l_orderkey),  
    CLUSTERED COLUMNSTORE INDEX,  
    PARTITION ( l_shipdate  RANGE RIGHT FOR VALUES
      (  
        '1992-01-01','1992-02-01','1992-03-01','1992-04-01','1992-05-01',
        '1992-06-01','1992-07-01','1992-08-01','1992-09-01','1992-10-01',
        '1992-11-01','1992-12-01','1993-01-01','1993-02-01','1993-03-01',
        '1993-04-01','1993-05-01','1993-06-01','1993-07-01','1993-08-01',
        '1993-09-01','1993-10-01','1993-11-01','1993-12-01','1994-01-01',
        '1994-02-01','1994-03-01','1994-04-01','1994-05-01','1994-06-01',
        '1994-07-01','1994-08-01','1994-09-01','1994-10-01','1994-11-01',
        '1994-12-01'  
      ))
  );  
```  
  
<a name="SeeAlso"></a>
## <a name="see-also"></a>Voir aussi
 
[CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)   
[DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
[sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md) 
