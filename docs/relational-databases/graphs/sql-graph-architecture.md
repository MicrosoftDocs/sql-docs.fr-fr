---
description: Architecture du graphique SQL
title: Architecture SQL Graph | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
helpviewer_keywords:
- SQL graph
- SQL graph, architecture
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ab08692b1e221e589cc6283e2800a695044f79b7
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351413"
---
# <a name="sql-graph-architecture"></a>Architecture du graphique SQL  
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

Découvrez comment SQL Graph est conçu. Le fait de connaître les principes de base facilitera la compréhension des autres Articles de SQL Graph.
 
## <a name="sql-graph-database"></a>Base de données SQL Graph
Les utilisateurs peuvent créer un graphique par base de données. Un graphique est une collection de tables de nœuds et d’arêtes. Les tables de nœuds ou d’arêtes peuvent être créées sous n’importe quel schéma de la base de données, mais elles appartiennent toutes à un graphique logique. Une table de nœuds est une collection de nœuds de type similaire. Par exemple, une table de nœuds Person contient tous les nœuds person appartenant à un graphique. De même, une table Edge est une collection de bords similaires. Par exemple, une table d’amis contient tous les bords qui connectent une personne à une autre personne. Étant donné que les nœuds et les bords sont stockés dans des tables, la plupart des opérations prises en charge sur les tables standard sont prises en charge sur les tables de nœuds ou d’arêtes. 
 
 
![Diagramme montrant l’architecture de la base de données SQL Graph.](../../relational-databases/graphs/media/sql-graph-architecture.png "Architecture de la base de données SQL Graph")   

Figure 1 : architecture de la base de données SQL Graph
 
## <a name="node-table"></a>Table de nœuds
Une table de nœuds représente une entité dans un schéma de graphique. Chaque fois qu’une table de nœuds est créée, avec les colonnes définies par l’utilisateur, une colonne implicite `$node_id` est créée, ce qui identifie de façon unique un nœud donné dans la base de données. Les valeurs dans `$node_id` sont générées automatiquement et sont une combinaison de `object_id` cette table de nœuds et d’une valeur bigint générée en interne. Toutefois, lorsque la `$node_id` colonne est sélectionnée, une valeur calculée sous la forme d’une chaîne JSON s’affiche. En outre, `$node_id` est une pseudo-colonne qui est mappée à un nom interne avec une chaîne hexadécimale. Lorsque vous sélectionnez `$node_id` dans la table, le nom de la colonne s’affiche sous la forme `$node_id_<hex_string>` . L’utilisation de noms de pseudo-colonnes dans les requêtes est la méthode recommandée pour interroger la `$node_id` colonne interne et l’utilisation d’un nom interne avec une chaîne hexadécimale doit être évitée.

Il est recommandé que les utilisateurs créent une contrainte ou un index unique sur la `$node_id` colonne au moment de la création de la table de nœuds. Toutefois, si aucun index n’est créé, un index non-cluster unique par défaut est créé automatiquement. Toutefois, tout index sur une pseudo-colonne de graphique est créé sur les colonnes internes sous-jacentes. Autrement dit, un index créé sur la `$node_id` colonne apparaît sur la `graph_id_<hex_string>` colonne interne.   


## <a name="edge-table"></a>Table Edge
Une table Edge représente une relation dans un graphique. Les bords sont toujours dirigés et connectent deux nœuds. Une table Edge permet aux utilisateurs de modéliser des relations plusieurs-à-plusieurs dans le graphique. Une table Edge peut ou ne peut pas contenir d’attributs définis par l’utilisateur. Chaque fois qu’une table Edge est créée, avec les attributs définis par l’utilisateur, trois colonnes implicites sont créées dans la table Edge :

|Nom de la colonne    |Description  |
|---   |---  |
|`$edge_id`   |Identifie de façon unique une arête donnée dans la base de données. Il s’agit d’une colonne générée et la valeur est une combinaison de object_id de la table Edge et d’une valeur bigint générée en interne. Toutefois, lorsque la `$edge_id` colonne est sélectionnée, une valeur calculée sous la forme d’une chaîne JSON s’affiche. `$edge_id` est une pseudo-colonne qui est mappée à un nom interne avec une chaîne hexadécimale. Lorsque vous sélectionnez `$edge_id` dans la table, le nom de la colonne s’affiche sous la forme `$edge_id_\<hex_string>` . L’utilisation de noms de pseudo-colonnes dans les requêtes est la méthode recommandée pour interroger la `$edge_id` colonne interne et l’utilisation d’un nom interne avec une chaîne hexadécimale doit être évitée. |
|`$from_id`   |Stocke le `$node_id` du nœud à partir duquel le bord provient.  |
|`$to_id`   |Stocke le `$node_id` du nœud, à partir duquel le bord s’arrête. |

Les nœuds qu’un bord donné peut connecter sont régis par les données insérées dans les `$from_id` `$to_id` colonnes et. Dans la première version, il n’est pas possible de définir des contraintes sur la table Edge, afin de la limiter à la connexion de deux types de nœuds. Autrement dit, un bord peut connecter deux nœuds quelconques dans le graphique, quels que soient leurs types.

Comme pour la `$node_id` colonne, il est recommandé que les utilisateurs créent un index unique ou une contrainte sur la `$edge_id` colonne au moment de la création de la table du bord, mais si aucun index non cluster par défaut n’est créé, un index non-cluster unique est automatiquement créé sur cette colonne. Toutefois, tout index sur une pseudo-colonne de graphique est créé sur les colonnes internes sous-jacentes. Autrement dit, un index créé sur la `$edge_id` colonne apparaît sur la `graph_id_<hex_string>` colonne interne. Il est également recommandé, pour les scénarios OLTP, que les utilisateurs créent un index sur les `$from_id` colonnes (, `$to_id` ), afin d’accélérer les recherches dans le sens de la périphérie.  

La figure 2 montre comment les tables de nœuds et de périphérie sont stockées dans la base de données. 

![Diagramme montrant la représentation de la table de nœuds et du bord.](../../relational-databases/graphs/media/person-friends-tables.png "Nœud Person et tables de bord des amis")   

Figure 2 : représentation d’une table de nœuds et d’un bord



## <a name="metadata"></a>Métadonnées
Utilisez ces vues de métadonnées pour afficher les attributs d’une table de nœuds ou d’arêtes.
 
### <a name="systables"></a>sys.tables
Les nouvelles colonnes suivantes, de type bit, seront ajoutées à SYS. Elles. Si `is_node` a la valeur 1, cela indique que la table est une table de nœuds et si `is_edge` a la valeur 1, ce qui indique que la table est une table d’arêtes.
 
|Nom de la colonne |Type de données |Description |
|--- |---|--- |
|is_node |bit |1 = il s’agit d’une table de nœuds |
|is_edge |bit |1 = il s’agit d’une table Edge |
 
### <a name="syscolumns"></a>sys.columns
La `sys.columns` vue contient des colonnes supplémentaires `graph_type` et `graph_type_desc` , qui indiquent le type de la colonne dans les tables de nœuds et de périphérie.
 
|Nom de la colonne |Type de données |Description |
|--- |---|--- |
|graph_type |int |Colonne interne avec un ensemble de valeurs. Les valeurs sont comprises entre 1-8 pour les colonnes de graphique et NULL pour d’autres.  |
|graph_type_desc |nvarchar(60)  |colonne interne avec un ensemble de valeurs |
 
Le tableau suivant répertorie les valeurs valides pour la `graph_type` colonne

|Valeur de la colonne  |Description  |
|---   |---   |
|1  |GRAPH_ID  |
|2  |GRAPH_ID_COMPUTED  |
|3  |GRAPH_FROM_ID  |
|4  |GRAPH_FROM_OBJ_ID  |
|5  |GRAPH_FROM_ID_COMPUTED  |
|6  |GRAPH_TO_ID  |
|7  |GRAPH_TO_OBJ_ID  |
|8  |GRAPH_TO_ID_COMPUTED  |


`sys.columns` stocke également des informations sur les colonnes implicites créées dans des tables de nœuds ou d’arêtes. Les informations suivantes peuvent être récupérées à partir de sys. Columns, mais les utilisateurs ne peuvent pas sélectionner ces colonnes dans une table de nœuds ou d’arêtes. 

Colonnes implicites dans une table de nœuds

|Nom de la colonne    |Type de données  |is_hidden  |Comment  |
|---  |---|---|---  |
|graph_id_\<hex_string> |bigint |1  |`graph_id`colonne interne  |
|$node _id_\<hex_string> |NVARCHAR   |0  |Colonne de nœud externe `node_id`  |

Colonnes implicites dans une table Edge

|Nom de la colonne    |Type de données  |is_hidden  |Comment  |
|---  |---|---|---  |
|graph_id_\<hex_string> |bigint |1  |`graph_id`colonne interne  |
|$edge _id_\<hex_string> |NVARCHAR   |0  |`edge_id`colonne externe  |
|from_obj_id_\<hex_string>  |INT    |1  |interne à partir du nœud `object_id`  |
|from_id_\<hex_string>  |bigint |1  |Interne à partir du nœud `graph_id`  |
|$from _id_\<hex_string> |NVARCHAR   |0  |externe à partir du nœud `node_id`  |
|to_obj_id_\<hex_string>    |INT    |1  |interne au nœud `object_id`  |
|to_id_\<hex_string>    |bigint |1  |Interne au nœud `graph_id`  |
|$to _id_\<hex_string>   |NVARCHAR   |0  |externe au nœud `node_id`  |
 
### <a name="system-functions"></a>Fonctions système
Les fonctions intégrées suivantes sont ajoutées. Celles-ci permettent aux utilisateurs d’extraire des informations à partir des colonnes générées. Notez que ces méthodes ne valident pas l’entrée de l’utilisateur. Si l’utilisateur spécifie un non valide `sys.node_id` , la méthode extrait le composant approprié et le retourne. Par exemple, OBJECT_ID_FROM_NODE_ID prend `$node_id` comme entrée et retourne la object_id de la table, ce nœud appartient à. 
 
|Intégré   |Description  |
|---  |---  |
|OBJECT_ID_FROM_NODE_ID |Extraire le object_id à partir d’un `node_id`  |
|GRAPH_ID_FROM_NODE_ID  |Extraire le graph_id à partir d’un `node_id`  |
|NODE_ID_FROM_PARTS |Construit un node_id à partir d’un `object_id` et d’un `graph_id`  |
|OBJECT_ID_FROM_EDGE_ID |Extraire `object_id` à partir de `edge_id`  |
|GRAPH_ID_FROM_EDGE_ID  |Extraire l’identité de `edge_id`  |
|EDGE_ID_FROM_PARTS |Construire `edge_id` à partir de `object_id` et Identity  |



## <a name="transact-sql-reference"></a>Informations de référence sur Transact-SQL 
Découvrez les [!INCLUDE[tsql-md](../../includes/tsql-md.md)] extensions introduites dans SQL Server et Azure SQL Database, qui permettent de créer et d’interroger des objets graphiques. Les extensions de langage de requête aident à interroger et à parcourir le graphique à l’aide de la syntaxe ASCII art.
 
### <a name="data-definition-language-ddl-statements"></a>Instructions du langage de définition de données (DDL)

|Tâche   |Article connexe  |Remarques
|---  |---  |---  |
|CREATE TABLE |[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-sql-graph.md)|`CREATE TABLE` est maintenant étendu pour prendre en charge la création d’une table en tant que nœud ou EDGE. Notez qu’une table Edge peut ou non avoir des attributs définis par l’utilisateur.  |
|ALTER TABLE    |[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)|Les tables de nœuds et de périphérie peuvent être modifiées de la même façon qu’une table relationnelle, à l’aide de `ALTER TABLE` . Les utilisateurs peuvent ajouter ou modifier des colonnes, des index ou des contraintes définis par l’utilisateur. Toutefois, la modification de colonnes graphiques internes, comme `$node_id` ou `$edge_id` , génère une erreur.  |
|CREATE INDEX   |[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  |Les utilisateurs peuvent créer des index sur des pseudo-colonnes et des colonnes définies par l’utilisateur dans des tables de nœuds et de périphérie. Tous les types d’index sont pris en charge, y compris les index ColumnStore cluster et non cluster.  |
|CRÉER DES CONTRAINTES DE PÉRIPHÉRIE    |[CONTRAINTES EDGE &#40;&#41;Transact-SQL ](../../relational-databases/tables/graph-edge-constraints.md)  |Les utilisateurs peuvent désormais créer des contraintes Edge sur les tables Edge pour appliquer une sémantique spécifique et maintenir l’intégrité des données.  |
|DROP TABLE |[DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)  |Les tables de nœuds et de périphérie peuvent être supprimées de la même façon qu’une table relationnelle, à l’aide de `DROP TABLE` . Toutefois, dans cette version, il n’existe aucune contrainte pour garantir qu’aucun bord ne pointe vers un nœud supprimé et que la suppression en cascade des bords n’est pas prise en charge lors de la suppression d’une table de nœuds ou de nœuds. Si une table de nœuds est supprimée, les utilisateurs déposent manuellement les bords connectés aux nœuds de cette table de nœuds afin de préserver l’intégrité du graphique.  |


### <a name="data-manipulation-language-dml-statements"></a>Instructions du langage de manipulation de données

|Tâche   |Article connexe  |Remarques
|---  |---  |---  |
|INSERT |[INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-sql-graph.md)|L’insertion dans une table de nœuds n’est pas différente de l’insertion dans une table relationnelle. Les valeurs de la `$node_id` colonne sont générées automatiquement. Toute tentative d’insertion d’une valeur dans `$node_id` ou de `$edge_id` colonne génère une erreur. Les utilisateurs doivent fournir des valeurs pour les `$from_id` `$to_id` colonnes et lors de l’insertion dans une table Edge. `$from_id` et `$to_id` sont les `$node_id` valeurs des nœuds auxquels se connecte un bord donné.  |
|Suppression | [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|Les données des tables de nœuds ou d’arêtes peuvent être supprimées de la même façon qu’elles sont supprimées des tables relationnelles. Toutefois, dans cette version, il n’existe aucune contrainte pour garantir qu’aucun bord ne pointe vers un nœud supprimé et que la suppression en cascade des bords n’est pas prise en charge lors de la suppression d’un nœud. Quand un nœud est supprimé, tous les bords de connexion à ce nœud sont également supprimés, afin de préserver l’intégrité du graphique.  |
|UPDATE |[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  |Les valeurs des colonnes définies par l’utilisateur peuvent être mises à jour à l’aide de l’instruction UPDATE. La mise à jour des colonnes graphiques internes, `$node_id` , `$edge_id` `$from_id` et `$to_id` n’est pas autorisée.  |
|MERGE |[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)  |`MERGE` l’instruction est prise en charge sur une table de nœuds ou d’arêtes.  |


### <a name="query-statements"></a>Instructions de requête

|Tâche   |Article connexe  |Notes
|---  |---  |---  |
|SELECT |[SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)|Les nœuds et les bords sont stockés en interne en tant que tables. par conséquent, la plupart des opérations prises en charge sur une table dans SQL Server ou Azure SQL Database sont prises en charge sur les tables de nœuds et de périphérie  |
|MATCH  | [Faire correspondre &#40;&#41;Transact-SQL ](../../t-sql/queries/match-sql-graph.md)|La fonction intégrée de correspondance est introduite pour prendre en charge les critères spéciaux et traverser le graphique.  |



## <a name="limitations-and-known-issues"></a>Limitations et problèmes connus  
Il existe certaines limitations sur les tables de nœuds et d’arêtes dans cette version :
* Les tables temporaires locales ou globales ne peuvent pas être des tables de nœuds ou de périphérie.
* Les types de table et les variables de table ne peuvent pas être déclarés en tant que table de nœuds ou d’arêtes. 
* Les tables de nœuds et de périphérie ne peuvent pas être créées en tant que tables temporelles avec version gérée par le système.   
* Les tables de nœuds et de périphérie ne peuvent pas être des tables optimisées en mémoire.  
* Les utilisateurs ne peuvent pas mettre à jour les `$from_id` `$to_id` colonnes et d’un bord à l’aide de l’instruction Update. Pour mettre à jour les nœuds qu’un bord connecte, les utilisateurs doivent insérer le nouveau bord pointant vers de nouveaux nœuds et supprimer le précédent.
* Les requêtes entre bases de données sur les objets Graph ne sont pas prises en charge. 


## <a name="next-steps"></a>Étapes suivantes
Pour commencer à utiliser la nouvelle syntaxe, consultez [base de données SQL Graph-exemple](./sql-graph-sample.md)
 

