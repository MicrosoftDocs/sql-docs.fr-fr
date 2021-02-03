---
description: MATCH (Transact-SQL)
title: MATCH (SQL Graph) | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- MATCH
- MATCH_TSQL
- SHORTEST_PATH
dev_langs:
- TSQL
helpviewer_keywords:
- MATCH statement [SQL Server], SQL graph
- SQL graph, MATCH statement
- Shortest Path, shortest_path
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7c55e6dc78a479952d1622f7d95ee85a3296dc41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207646"
---
# <a name="match-transact-sql"></a>MATCH (Transact-SQL)
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

  Indique une condition de recherche pour un graphique. L’indicateur MATCH peut être utilisé uniquement avec des tables d’arêtes et de nœuds d’un graphique, avec la clause WHERE dans l’instruction SELECT. 
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
MATCH (<graph_search_pattern>)

<graph_search_pattern>::=
  {  
      <simple_match_pattern> 
    | <arbitrary_length_match_pattern>  
    | <arbitrary_length_match_last_node_predicate> 
  }

<simple_match_pattern>::=
  {
      LAST_NODE(<node_alias>) | <node_alias>   { 
          { <-( <edge_alias> )- } 
        | { -( <edge_alias> )-> }
        <node_alias> | LAST_NODE(<node_alias>)
        } 
  }
  [ { AND } { ( <simple_match_pattern> ) } ]
  [ ,...n ]

<node_alias> ::=
  node_table_name | node_table_alias 

<edge_alias> ::=
  edge_table_name | edge_table_alias


<arbitrary_length_match_pattern>  ::=
  { 
    SHORTEST_PATH( 
      <arbitrary_length_pattern> 
      [ { AND } { <arbitrary_length_pattern> } ] 
      [ ,…n] 
    )
  } 

<arbitrary_length_match_last_node_predicate> ::=
  {  LAST_NODE( <node_alias> ) = LAST_NODE( <node_alias> ) }


<arbitrary_length_pattern> ::=
    {  LAST_NODE( <node_alias> )   | <node_alias>
     ( <edge_first_al_pattern> [<edge_first_al_pattern>…,n] )
     <al_pattern_quantifier> 
  }
    |  ( {<node_first_al_pattern> [<node_first_al_pattern> …,n] )
        <al_pattern_quantifier> 
        LAST_NODE( <node_alias> ) | <node_alias> 
 }
    
<edge_first_al_pattern> ::=
  { (  
        { -( <edge_alias> )->   } 
      | { <-( <edge_alias> )- } 
      <node_alias>
      ) 
  } 

<node_first_al_pattern> ::=
  { ( 
      <node_alias> 
        { <-( <edge_alias> )- } 
      | { -( <edge_alias> )-> }
      ) 
  } 


<al_pattern_quantifier> ::=
  {
        +
      | { 1 , n }
  }

n -  positive integer only.
 
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*graph_search_pattern*  
Spécifie le modèle à rechercher ou le chemin à parcourir dans le graphique. Ce modèle utilise la syntaxe d’art ASCII pour parcourir un chemin dans le graphique. Le modèle passe d’un nœud à un autre par une arête, dans la direction de la flèche fournie. Les noms ou alias des arêtes sont spécifiés entre parenthèses. Les noms ou alias des nœuds sont spécifiés à chaque extrémité de la flèche. La flèche peut aller dans les deux directions dans le modèle.

*node_alias*  
Nom ou alias d’une table de nœuds fourni dans la clause FROM.

*edge_alias*  
Nom ou alias d’une table d’arêtes fourni dans la clause FROM.

*SHORTEST_PATH*   
La fonction de chemin la plus courte est utilisée pour rechercher le chemin le plus court entre deux nœuds donnés d’un graphique, ou entre un nœud donnée et tous les autres nœuds d’un graphique. Il prend un modèle de longueur arbitraire en tant qu’entrée, qui est recherchée de manière répétée dans un graphique. 

*arbitrary_length_match_pattern*  
Spécifie les nœuds et les arêtes qui doivent être parcourus de manière répétée jusqu'à ce que le nœud souhaité ou le nombre maximal d’itérations spécifié dans le modèle soient atteints. 

*al_pattern_quantifier*   
Le modèle de longueur arbitraire prend des quantificateurs de modèles de style d’expressions régulières pour spécifier le nombre de répétitions d’un modèle de recherche donné. Les quantificateurs de modèle de recherche pris en charge sont :   
* **+**  : Répétez le modèle une ou plusieurs fois. Arrêtez dès qu’un chemin d’accès le plus court est trouvé.    
* **{1,n}**  : Répétez le modèle une à « n » fois. Arrêtez dès qu’un chemin d’accès le plus court est trouvé.     

## <a name="remarks"></a>Notes  
Un nom de nœud peut être répété dans un indicateur MATCH.  En d’autres termes, un nœud peut être parcouru un nombre de fois arbitraire dans la même requête.  
Un nom d’arête ne peut pas être répété dans un indicateur MATCH.  
Une arête peut pointer dans les deux directions, mais elle doit avoir une direction explicite.  
Les opérateurs OR et NOT ne sont pas pris en charge dans le modèle MATCH. Vous pouvez combiner MATCH avec d’autres expressions en ajoutant l’opérateur AND dans la clause WHERE. En revanche, il n’est pas possible de combiner MATCH avec d’autres expressions à l’aide des opérateurs OR et NOT. 

## <a name="examples"></a>Exemples  
### <a name="a--find-a-friend"></a>R.  Rechercher un ami 
 L’exemple suivant crée une table de nœuds Person et une table d’arêtes friends, insère des données, puis utilise MATCH pour rechercher des amis d’Alice, une personne figurant dans le graphique.

 ```sql
 -- Create person node table
 CREATE TABLE dbo.Person (ID INTEGER PRIMARY KEY, name VARCHAR(50)) AS NODE;
 CREATE TABLE dbo.friend (start_date DATE) AS EDGE;

 -- Insert into node table
 INSERT INTO dbo.Person VALUES (1, 'Alice');
 INSERT INTO dbo.Person VALUES (2,'John');
 INSERT INTO dbo.Person VALUES (3, 'Jacob');

-- Insert into edge table
INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'John'), '9/15/2011');

INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'Jacob'), '10/15/2011');

INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'John'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'Jacob'), '10/15/2012');

-- use MATCH in SELECT to find friends of Alice
SELECT Person2.name AS FriendName
FROM Person Person1, friend, Person Person2
WHERE MATCH(Person1-(friend)->Person2)
AND Person1.name = 'Alice';
 ```

 ### <a name="b--find-friend-of-a-friend"></a>B.  Rechercher l’ami d’un ami
 L’exemple suivant recherche l’ami d’un ami d’Alice. 

 ```sql
SELECT Person3.name AS FriendName 
FROM Person Person1, friend, Person Person2, friend friend2, Person Person3
WHERE MATCH(Person1-(friend)->Person2-(friend2)->Person3)
AND Person1.name = 'Alice';
 ```

### <a name="c--more-match-patterns"></a>C.  Autres modèles `MATCH`
 Voici d’autres façons de spécifier un modèle dans MATCH.

 ```sql
 -- Find a friend
    SELECT Person2.name AS FriendName
    FROM Person Person1, friend, Person Person2
    WHERE MATCH(Person1-(friend)->Person2);
    
-- The pattern can also be expressed as below

    SELECT Person2.name AS FriendName
    FROM Person Person1, friend, Person Person2 
    WHERE MATCH(Person2<-(friend)-Person1);


-- Find 2 people who are both friends with same person
    SELECT Person1.name AS Friend1, Person2.name AS Friend2
    FROM Person Person1, friend friend1, Person Person2, 
         friend friend2, Person Person0
    WHERE MATCH(Person1-(friend1)->Person0<-(friend2)-Person2);
    
-- this pattern can also be expressed as below

    SELECT Person1.name AS Friend1, Person2.name AS Friend2
    FROM Person Person1, friend friend1, Person Person2, 
         friend friend2, Person Person0
    WHERE MATCH(Person1-(friend1)->Person0 AND Person2-(friend2)->Person0);
 ```
 

## <a name="see-also"></a>Voir aussi  
 [CREATE TABLE &#40;SQL Graph&#41;](../../t-sql/statements/create-table-sql-graph.md)   
 [INSERT (SQL Graph)](../../t-sql/statements/insert-sql-graph.md)]  
 [Traitement des graphes avec SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md)  
