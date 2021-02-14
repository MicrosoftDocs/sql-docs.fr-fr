---
description: SHORTEST_PATH (Transact-SQL)
title: Chemin d’accès le plus rapide (SQL Graph) | Microsoft Docs
ms.custom: ''
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
f1_keywords:
- SHORTEST PATH
- SHORTEST_PATH
dev_langs:
- TSQL
helpviewer_keywords:
- MATCH statement [SQL Server], SQL graph
- SQL graph, MATCH statement
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-ver15||=azuresqldb-mi-current
ms.openlocfilehash: 9e5a6f6506e4ce40b3b89f32dd58329eb4667760
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351388"
---
# <a name="shortest_path-transact-sql"></a>SHORTEST_PATH (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

  Spécifie une condition de recherche pour un graphique, qui est recherchée de manière récursive ou répétée. SHORTEST_PATH peut être utilisé dans une correspondance avec le nœud de graphique et les tables de bord, dans l’instruction SELECT. 
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="shortest-path"></a>Chemin d’accès le plus rapide
La fonction SHORTEST_PATH vous permet de trouver :    
* Chemin d’accès le plus bref entre deux nœuds/entités donnés
* Chemin (s) d’accès le plus courts à la source unique.
* Chemin le plus bref entre plusieurs nœuds sources et plusieurs nœuds cibles.

Elle prend un modèle de longueur arbitraire comme entrée et retourne un chemin d’accès le plus rapide qui existe entre deux nœuds. Cette fonction ne peut être utilisée qu’à l’intérieur de MATCH. La fonction ne retourne qu’un seul chemin d’accès le plus rapide entre deux nœuds donnés. S’il existe, deux chemins d’accès ou plus courts de la même longueur entre une paire de nœuds source et de destination, la fonction retourne un seul chemin d’accès qui a été trouvé en premier lors du parcours. Notez qu’un modèle de longueur arbitraire ne peut être spécifié qu’à l’intérieur d’une fonction SHORTEST_PATH. 

Reportez-vous à la syntaxe [(graphique SQL) correspondante](../../t-sql/queries/match-sql-graph.md) . 

## <a name="for-path"></a>POUR LE CHEMIN D’ACCÈS
POUR PATH doit être utilisé avec n’importe quel nom de table de nœud ou d’arête dans la clause FROM, qui participera à un modèle de longueur arbitraire. POUR chemin d’accès indique au moteur que la table de nœuds ou d’arêtes retourne une collection ordonnée représentant la liste des nœuds ou des arêtes trouvés le long du chemin parcouru. Les attributs de ces tables ne peuvent pas être préprojetés directement dans la clause SELECT. Pour projeter des attributs à partir de ces tables, vous devez utiliser les fonctions d’agrégation du chemin d’accès Graph.  

## <a name="arbitrary-length-pattern"></a>Modèle de longueur arbitraire
Ce modèle comprend les nœuds et les bords qui doivent être parcourus à plusieurs reprises jusqu’à ce que le nœud souhaité soit atteint ou jusqu’à ce que le nombre maximal d’itérations spécifié dans le modèle soit respecté. Chaque fois que la requête est exécutée, le résultat de l’exécution de ce modèle est une collection ordonnée des nœuds et des bords parcourus le long du nœud de départ jusqu’au nœud de fin. Il s’agit d’un modèle de syntaxe de style d’expression régulière et les quantificateurs de deux modèles suivants sont pris en charge :

* **' + '**: Répéter le modèle 1 ou plusieurs fois. Arrêtez dès qu’un chemin d’accès le plus court est trouvé.
* **{1,n}**  : Répétez le modèle une à « n » fois. Terminer dès qu’un plus petit est trouvé.

## <a name="last_node"></a>LAST_NODE
La fonction LAST_NODE () autorise le chaînage de deux modèles de traversée de longueur arbitraire. Il peut être utilisé dans les scénarios où :    
* Plusieurs modèles de chemin d’accès les plus courts sont utilisés dans une requête et un modèle commence au dernier nœud du modèle précédent.
* Deux modèles Path les plus courts fusionnent au même LAST_NODE ().

## <a name="graph-path-order"></a>Ordre des chemins du graphique
L’ordre des chemins d’accès des graphiques fait référence à l’ordre des données dans le chemin de sortie. L’ordre du chemin de sortie démarre toujours au niveau de la partie non récursive du modèle suivi des nœuds/bords qui apparaissent dans la partie récursive. L’ordre dans lequel le graphique est parcouru pendant l’optimisation et l’exécution de la requête n’a rien à faire avec la commande imprimée dans la sortie. De même, la direction de la flèche dans le modèle récursif n’a pas d’incidence sur l’ordre des chemins du graphique. 

## <a name="graph-path-aggregate-functions"></a>Fonctions d’agrégation du chemin Graph
Étant donné que les nœuds et les bords impliqués dans un modèle de longueur arbitraire retournent une collection (de nœuds et d’arêtes parcourues dans ce chemin d’accès), les utilisateurs ne peuvent pas projeter les attributs directement à l’aide de la syntaxe TableName. AttributeName conventionnelle. Pour les requêtes dans lesquelles il est nécessaire de projeter des valeurs d’attribut à partir des tables de nœuds ou d’arêtes intermédiaires, dans le chemin parcouru, utilisez les fonctions d’agrégation de chemin de graphique suivantes : STRING_AGG, LAST_VALUE, SUM, AVG, MIN, MAX et COUNT. La syntaxe générale permettant d’utiliser ces fonctions d’agrégation dans la clause SELECT est la suivante :

```syntaxsql
<GRAPH_PATH_AGGREGATE_FUNCTION>(<expression> , <separator>)  <order_clause>

    <order_clause> ::=
        { WITHIN GROUP (GRAPH PATH) }

    <GRAPH_PATH_AGGREGATE_FUNCTION> ::=
          STRING_AGG
        | LAST_VALUE
        | SUM
        | COUNT
        | AVG
        | MIN
        | MAX

```

### <a name="string_agg"></a>STRING_AGG
La fonction STRING_AGG prend une expression et un séparateur comme entrée et retourne une chaîne. Les utilisateurs peuvent utiliser cette fonction dans la clause SELECT pour projeter des attributs à partir des nœuds intermédiaires ou des bords dans le chemin parcouru. 

### <a name="last_value"></a>LAST_VALUE
Pour projeter les attributs du dernier nœud du chemin parcouru, LAST_VALUE fonction d’agrégation peut être utilisée. Le fait de fournir un alias de table Edge comme entrée à cette fonction est une erreur, seuls les noms de tables de nœuds ou les alias peuvent être utilisés.

**Dernier nœud**: le dernier nœud fait référence au nœud qui apparaît en dernier dans le chemin parcouru, quelle que soit la direction de la flèche dans le PRÉDICAT de correspondance. Par exemple : `MATCH(SHORTEST_PATH(n(-(e)->p)+) )`. Ici, le dernier nœud du chemin d’accès est le dernier nœud P visité. 

Tandis que le dernier nœud est le dernier nième nœud dans le chemin d’accès du graphique de sortie pour ce modèle : `MATCH(SHORTEST_PATH((n<-(e)-)+p))`    

### <a name="sum"></a>SUM
Cette fonction retourne la somme des valeurs d’attribut node/Edge fournies ou de l’expression qui apparaissait dans le chemin parcouru.

### <a name="count"></a>COUNT
Cette fonction retourne le nombre de valeurs non null de l’attribut node/Edge souhaité dans le chemin d’accès. La fonction COUNT prend en charge l' \* opérateur «» avec un nœud ou un alias de table Edge. Sans l’alias de la table node ou Edge, l’utilisation de \* est ambiguë et génère une erreur.

```syntaxsql
{  COUNT( <expression> | <node_or_edge_alias>.* )  <order_clause>  }
```

### <a name="avg"></a>AVG
Retourne la moyenne des valeurs d’attribut node/Edge fournies ou de l’expression qui apparaissait dans le chemin parcouru.

### <a name="min"></a>MIN
Retourne la valeur minimale des valeurs d’attribut de nœud/de bord fournies ou de l’expression qui apparaissaient dans le chemin parcouru.

### <a name="max"></a>MAX
Retourne la valeur maximale des valeurs d’attribut de nœud/de bord fournies ou de l’expression qui apparaissaient dans le chemin parcouru.

## <a name="remarks"></a>Notes  
shortest_path fonction ne peut être utilisée qu’à l’intérieur de MATCH.     
LAST_NODE est pris en charge uniquement dans les shortest_path.     
Recherche du chemin d’accès le plus faible pondéré, tous les chemins d’accès ou tous les chemins les plus courts ne sont pas pris en charge.         
Dans certains cas, des plans incorrects peuvent être générés pour les requêtes avec un plus grand nombre de tronçons, ce qui entraîne des temps d’exécution de requête plus élevés. L’utilisation d’un indicateur de jointure de hachage peut être utile.    


## <a name="examples"></a>Exemples 
Pour les exemples de requêtes indiqués ici, nous allons utiliser les tables node et Edge créées dans l' [exemple SQL Graph](./sql-graph-sample.md) .

### <a name="a--find-shortest-path-between-2-people"></a>R.  Rechercher le chemin le plus rapide entre 2 personnes
 Dans l’exemple suivant, nous trouvons le chemin le plus rapide entre Jacob et Alice. Nous aurons besoin du nœud Person et du Edge FriendOf créé à partir de l’exemple de script Graph. 

```sql
SELECT PersonName, Friends
FROM (  
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        LAST_VALUE(Person2.name) WITHIN GROUP (GRAPH PATH) AS LastNode
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
    AND Person1.name = 'Jacob'
) AS Q
WHERE Q.LastNode = 'Alice'
```

 ### <a name="b--find-shortest-path-from-a-given-node-to-all-other-nodes-in-the-graph"></a>B.  Recherche le chemin le plus rapide d’un nœud donné vers tous les autres nœuds du graphique. 
 L’exemple suivant recherche toutes les personnes auxquelles Jacob est connectée dans le graphique et le chemin le plus bref, qui commence par Jacob pour toutes ces personnes. 

```sql
SELECT
    Person1.name AS PersonName, 
    STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends
FROM
    Person AS Person1,
    friendOf FOR PATH AS fo,
    Person FOR PATH  AS Person2
WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
AND Person1.name = 'Jacob'
```

### <a name="c--count-the-number-of-hopslevels-traversed-to-go-from-one-person-to-another-in-the-graph"></a>C.  Comptez le nombre de tronçons/niveaux traversés pour passer d’une personne à une autre dans le graphique.
 L’exemple suivant recherche le chemin le plus rapide entre Jacob et Alice et imprime le nombre de tronçons qu’il prend pour passer de Jacob à Alice. 

```sql
 SELECT PersonName, Friends, levels
FROM (  
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        LAST_VALUE(Person2.name) WITHIN GROUP (GRAPH PATH) AS LastNode,
        COUNT(Person2.name) WITHIN GROUP (GRAPH PATH) AS levels
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
    AND Person1.name = 'Jacob'
) AS Q
WHERE Q.LastNode = 'Alice'
```

### <a name="d-find-people-1-3-hops-away-from-a-given-person"></a>D. Rechercher des personnes 1-3 sauts éloignés d’une personne donnée
L’exemple suivant recherche le chemin le plus rapide entre Jacob et toutes les personnes auxquelles il est connecté dans le graphique 1-3 sauts. 

```sql
SELECT
    Person1.name AS PersonName, 
    STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends
FROM
    Person AS Person1,
    friendOf FOR PATH AS fo,
    Person FOR PATH  AS Person2
WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2){1,3}))
AND Person1.name = 'Jacob'
```

### <a name="e-find-people-exactly-2-hops-away-from-a-given-person"></a>E. Rechercher des personnes exactement 2 sauts à partir d’une personne donnée
L’exemple suivant recherche le chemin le plus rapide entre Jacob et les personnes qui sont exactement 2 sauts à l’extérieur du graphique. 

```sql
SELECT PersonName, Friends
FROM (
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        COUNT(Person2.name) WITHIN GROUP (GRAPH PATH) AS levels
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2){1,3}))
    AND Person1.name = 'Jacob'
) Q
WHERE Q.levels = 2
```

## <a name="see-also"></a>Voir aussi  
 [MATCH (graphique SQL)](../../t-sql/queries/match-sql-graph.md)    
 [CREATE TABLE &#40;SQL Graph&#41;](../../t-sql/statements/create-table-sql-graph.md)   
 [INSERT (SQL Graph)](../../t-sql/statements/insert-sql-graph.md)]  
 [Traitement des graphes avec SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md)     
 
