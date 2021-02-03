---
description: CREATE TABLE (SQL Graph)
title: CREATE TABLE (SQL Graph) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL_GRAPH_TSQL
- TABLE
- CREATE_TABLE_TSQL
- NODE
- NODE_TSQL
- AS_NODE
- AS_NODE_TSQL
- EDGE
- EDGE_TSQL
- AS_EDGE
- AS_EDGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- graph
- SQL graph
- CREATE TABLE SQL graph
- NODE
- EDGE
- SQL graph, CREATE TABLE statement
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d946a2c834fa910a0a7b76b321a55bfbdd91198
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177899"
---
# <a name="create-table-sql-graph"></a>CREATE TABLE (SQL Graph)
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

Crée une table graphique SQL en tant que table `NODE` ou `EDGE`. 
  
> [!NOTE]   
>  Pour plus d’informations sur les instructions Transact-SQL standard, consultez [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE TABLE   
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition> } 
       | <computed_column_definition>
       | <column_set_definition>
       | [ <table_constraint> ] [ ,... n ]
       | [ <table_index> ] }
          [ ,...n ]
    )   
    AS [ NODE | EDGE ]
    [ ON { partition_scheme_name ( partition_column_name )
           | filegroup
           | "default" } ]
[ ; ] 

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
    | CONNECTION
        ( { node_table TO node_table } 
          [ , {node_table TO node_table }]
          [ , ...n ]
        )
        [ ON DELETE { NO ACTION | CASCADE } ]
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
```  
  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
Ce document répertorie uniquement les arguments appartenant à un graphe SQL. Pour obtenir la liste complète des arguments pris en charge et leur description, consultez [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).

 *database_name*    
 Nom de la base de données dans laquelle la table est créée. *database_name* doit spécifier le nom d’une base de données existante. Si aucun nom n’est spécifié, *database_name* correspond par défaut à la base de données actuelle. Le nom d’accès de la connexion actuelle doit être associé à un ID d’utilisateur existant dans la base de données spécifiée par *database_name*, et cet ID d’utilisateur doit disposer des autorisations CREATE TABLE.  
  
 *schema_name*    
 Nom du schéma auquel appartient la nouvelle table.  
  
 *table_name*      
 Est le nom de la table de nœuds ou d’arêtes. Les noms de tables doivent respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md). *table_name* peut comprendre un maximum de 128 caractères, à l’exception des noms de tables temporaires locales (noms précédés du signe #) qui ne peuvent pas dépasser 116 caractères.  
  
 NODE   
 Crée une table de nœuds.

 EDGE  
 Crée une table d’arêtes.  
 
 *table_constraint*   
 Spécifie les propriétés d’une contrainte PRIMARY KEY, UNIQUE, FOREIGN KEY, CONNECTION ou CHECK, ou d’une définition DEFAULT ajoutée à une table.
 
 > [!NOTE]   
 > La contrainte CONNECTION ne s’applique qu’à un type de table d’arêtes.
 
 ON { partition_scheme | filegroup | "default" }    
 Spécifie le schéma de partition ou groupe de fichiers dans lequel la table est stockée. Si partition_scheme est spécifié, la table est partitionnée avec des partitions stockées dans un ensemble de fichiers ou groupes de fichiers spécifié dans partition_scheme. Si le groupe de fichiers est spécifié, la table est stockée dans le groupe de fichiers nommé. Le groupe de fichiers doit exister dans la base de données. Si "default" est spécifié, ou si ON n’est pas spécifié du tout, la table est stockée dans le groupe de fichiers par défaut. Le mécanisme de stockage d'une table tel que spécifié dans CREATE TABLE ne peut plus être modifié ultérieurement.

 ON {partition_scheme | filegroup | "default"}    
 Peut également être spécifié dans une contrainte PRIMARY KEY ou UNIQUE. Ces contraintes créent des index. Si le groupe de fichiers est spécifié, l’index est stocké dans le groupe de fichiers nommé. Si "default" est spécifié, ou si ON n’est pas spécifié du tout, l’index est stocké dans le même groupe de fichiers que la table. Si la contrainte PRIMARY KEY ou UNIQUE crée un index cluster, les pages de données de la table sont stockées dans le même groupe de fichiers que l'index. Si CLUSTERED est spécifié ou la contrainte crée un index cluster d’une autre manière, et qu’une valeur partition_scheme est spécifiée qui diffère des valeurs partition_scheme ou le groupe de fichiers de la définition de table, ou vice versa, seule la définition de la contrainte sera honorée et l’autre sera ignorée.
  
## <a name="remarks"></a>Remarques

Le création d’une table temporaire en tant que nœud ou table d’arêtes n’est pas prise en charge.  

Le création d’une table de nœuds ou d’arêtes temporelle n’est pas prise en charge.

L’utilisation de Stretch Database n’est pas prise en charge pour les tables de nœuds et d’arêtes.

Les tables de nœuds et d’arêtes ne peuvent pas être des tables externes (aucune prise en charge PolyBase pour les tables de graphe). 

Une table de nœuds/arêtes de graphique non partitionnée ne peut pas devenir une table de nœuds/arêtes de graphique partitionnée. 
  
 
## <a name="examples"></a>Exemples  
  
### <a name="a-create-a-node-table"></a>R. Créer une table `NODE`
 L’exemple suivant montre comment créer une table `NODE`.

```sql
 CREATE TABLE Person (
        ID INTEGER PRIMARY KEY, 
        name VARCHAR(100), 
        email VARCHAR(100)
 ) AS NODE;
```

### <a name="b-create-an-edge-table"></a>B. Créer une table `EDGE`
Les exemples suivants montrent comment créer des tables `EDGE`.

```sql
 CREATE TABLE friends (
    id INTEGER PRIMARY KEY,
    start_date DATe
 ) AS EDGE;
```

```sql
 -- Create a likes edge table, this table does not have any user defined attributes   
 CREATE TABLE likes AS EDGE;
```

L’exemple suivant modélise la règle selon laquelle **seules** des personnes peuvent être amies avec d’autres personnes. En d’autres termes, cette arête n’autorise pas les références à des nœuds autres que Person.

```
/* Create friend edge table with CONSTRAINT, restricts for nodes and it direction */
CREATE TABLE dbo.FriendOf(
  CONSTRAINT cnt_Person_FriendOf_Person
    CONNECTION (dbo.Person TO dbo.Person) 
)AS EDGE;
```


## <a name="see-also"></a>Voir aussi 
 [ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [INSERT (SQL Graph)](../../t-sql/statements/insert-sql-graph.md)]  
 [Traitement des graphes avec SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md)

