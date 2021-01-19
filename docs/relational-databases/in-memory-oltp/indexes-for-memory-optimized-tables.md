---
title: Index pour les tables optimisées en mémoire | Microsoft Docs
description: Découvrez comment un index sur une table à mémoire optimisée diffère d'un index traditionnel sur une table sur disque dans SQL Server et Azure SQL Database.
ms.custom: ''
ms.date: 09/16/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: eecc5821-152b-4ed5-888f-7c0e6beffed9
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4721f3ae9add5e62aa95820b0b40f0ddd20a6d81
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172551"
---
# <a name="indexes-on-memory-optimized-tables"></a>Index sur des tables optimisées en mémoire

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Chaque table à mémoire optimisée doit avoir au moins un index, car l’index permet de lier les lignes de la table entre elles. Sur une table optimisée en mémoire, chaque index est également optimisé en mémoire. Il existe plusieurs différences entre un index sur une table à mémoire optimisée et un index classique sur une table sur disque :  

- Les lignes de données ne sont pas stockées dans des pages. Il n’y a donc pas de collection de pages ou d’étendues, ni de partitions ou d’unités d’allocation qui peuvent être référencées pour obtenir toutes les pages d’une table. Le concept de pages d’index existe pour l’un des types d’index disponibles, mais ils sont stockés différemment des index des tables sur disque. Ne contribuant pas au type traditionnel de fragmentation dans une page, ils ne présentent aucun facteur de remplissage.
- Les modifications effectuées dans les index de tables à mémoire optimisée pendant la manipulation des données ne sont jamais écrites sur le disque. Seules les lignes de données et les modifications apportées aux données sont écrites dans le journal des transactions. 
- Les index optimisés en mémoire sont reconstruits quand la base de données est remise en ligne. 

Tous les index des tables à mémoire optimisée sont créés sur la base des définitions d’index au moment de la récupération de la base de données.

Le type d’index doit être l’un des suivants :  
  
- Index de hachage  
- Index non-cluster à mémoire optimisée, désignant la structure interne par défaut d’un arbre B (B-tree) 
  
Les index de *hachage* sont présentés plus en détail dans [Index de hachage pour les tables à mémoire optimisée](../../relational-databases/sql-server-index-design-guide.md#hash_index).  
Les index *non-cluster* sont présentés plus en détail dans [Index non-cluster pour les tables à mémoire optimisée](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index).  
Les index *columnstore* sont abordés dans un [autre article](../../relational-databases/indexes/columnstore-indexes-overview.md).  

## <a name="syntax-for-memory-optimized-indexes"></a>Syntaxe des index optimisés en mémoire  
  
Chaque instruction CREATE TABLE d’une table à mémoire optimisée doit inclure un index, soit explicitement à l’aide d’une contrainte INDEX, soit implicitement à l’aide d’une contrainte PRIMAY KEY ou UNIQUE.
  
Pour être déclarée avec DURABILITY = SCHEMA\_AND_DATA (paramètre par défaut), la table à mémoire optimisée doit avoir une clé primaire. La clause PRIMARY KEY NONCLUSTERED dans l’instruction CREATE TABLE suivante répond à deux conditions :  
  
- Elle fournit un index pour satisfaire à la condition minimale d’un index dans l’instruction CREATE TABLE.  
- Elle fournit la clé primaire qui est requise pour la clause SCHEMA\_AND_DATA.  

    ```sql
    CREATE TABLE SupportEvent  
    (  
        SupportEventId   int NOT NULL  
            PRIMARY KEY NONCLUSTERED,  
        ...  
    )  
        WITH (  
            MEMORY_OPTIMIZED = ON,  
            DURABILITY = SCHEMA_AND_DATA);  
    ```

> [!NOTE]  
> Dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], le nombre d’index par table à mémoire optimisée ou type de table est limité à 8. À compter de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le nombre d’index n’est plus limité pour les tables à mémoire optimisée et les types de tables.
  
### <a name="code-sample-for-syntax"></a>Exemple de code pour la syntaxe  
  
Cette sous-section contient un bloc de code Transact-SQL qui illustre la syntaxe pour créer plusieurs index sur une table optimisée en mémoire. Le code illustre les opérations suivantes :  
  
1. Créer une table optimisée en mémoire.  
2. Utiliser des instructions ALTER TABLE pour ajouter deux index.  
3. Ajouter quelques lignes de données avec INSERT.  
   
    ```sql
    DROP TABLE IF EXISTS SupportEvent;  
    go  

    CREATE TABLE SupportEvent  
    (  
        SupportEventId   int               not null   identity(1,1)  
        PRIMARY KEY NONCLUSTERED,  

        StartDateTime        datetime2     not null,  
        CustomerName         nvarchar(16)  not null,  
        SupportEngineerName  nvarchar(16)      null,  
        Priority             int               null,  
        Description          nvarchar(64)      null  
    )  
        WITH (  
        MEMORY_OPTIMIZED = ON,  
        DURABILITY = SCHEMA_AND_DATA);  
    go  
        
        --------------------  
        
    ALTER TABLE SupportEvent  
        ADD CONSTRAINT constraintUnique_SDT_CN  
        UNIQUE NONCLUSTERED (StartDateTime DESC, CustomerName);  
    go  

    ALTER TABLE SupportEvent  
        ADD INDEX idx_hash_SupportEngineerName  
        HASH (SupportEngineerName) WITH (BUCKET_COUNT = 64);  -- Nonunique.  
    go  
        
        --------------------  
        
    INSERT INTO SupportEvent  
        (StartDateTime, CustomerName, SupportEngineerName, Priority, Description)  
        VALUES  
        ('2016-02-23 13:40:41:123', 'Abby', 'Zeke', 2, 'Display problem.'     ),  
        ('2016-02-24 13:40:41:323', 'Ben' , null  , 1, 'Cannot find help.'    ),  
        ('2016-02-25 13:40:41:523', 'Carl', 'Liz' , 2, 'Button is gray.'      ),  
        ('2016-02-26 13:40:41:723', 'Dave', 'Zeke', 2, 'Cannot unhide column.');  
    go 
    ``` 
  
## <a name="duplicate-index-key-values"></a>Valeurs de clé d’index dupliquées

Les valeurs dupliquées pour une clé d’index peuvent réduire les performances des tables à mémoire optimisée. Les doublons pour le système traversent les chaînes d’entrée pour la plupart des opérations de lecture et d’écriture d’index. Quand une chaîne d’entrées dupliquées dépasse 100 entrées, la dégradation des performances peut devenir mesurable.

### <a name="duplicate-hash-values"></a>Valeurs de hachage dupliquées

Ce problème est plus visible dans le cas des index de hachage. Les index de hachage souffrent davantage en raison des considérations suivantes :

- Le plus faible coût par opération pour les index de hachage
- L’interférence des chaînes dupliquées volumineuses avec la chaîne de collision de hachage

Pour réduire la duplication dans un index, essayez les ajustements suivants :

- Utilisez un index non-cluster
- Ajoutez des colonnes supplémentaires à la fin de la clé d’index pour réduire le nombre de doublons
  - Par exemple, vous pouvez ajouter des colonnes qui figurent également dans la clé primaire.

Pour plus d’informations sur les collisions de hachage, consultez [Index de hachage pour les tables à mémoire optimisée](../../relational-databases/sql-server-index-design-guide.md#hash_index).

### <a name="example-improvement"></a>Exemple d’amélioration

Voici un exemple illustrant comment éviter la dégradation des performances de votre index.

Prenez une table `Customers` ayant une clé primaire sur `CustomerId` et un index sur la colonne `CustomerCategoryID`. En général, il y aura de nombreux clients dans une catégorie donnée. Ainsi, il y aura de nombreuses valeurs dupliquées pour CustomerCategoryID à l’intérieur d’une clé donnée de l’index.

Dans ce scénario, la bonne pratique consiste à utiliser un index non-cluster sur `(CustomerCategoryID, CustomerId)`. Cet index peut être utilisé pour les requêtes qui utilisent un prédicat impliquant `CustomerCategoryID`, mais la clé d’index ne contient pas de doublon. Par conséquent, aucune dégradation dans la maintenance d’index n’est provoquée par les valeurs CustomerCategoryID dupliquées ou par la colonne supplémentaire dans l’index.

La requête suivante affiche le nombre moyen d’index de doublons de valeurs de clé d’index sur `CustomerCategoryID` dans la table `Sales.Customers`, dans l’exemple de base de données [WideWorldImporters](../../samples/wide-world-importers-what-is.md).

```sql
SELECT AVG(row_count) FROM
    (SELECT COUNT(*) AS row_count 
        FROM Sales.Customers
        GROUP BY CustomerCategoryID) a
```

Pour évaluer le nombre moyen de doublons de clé d’index de votre propre table et de votre propre index, remplacez `Sales.Customers` par votre nom de table et remplacez `CustomerCategoryID` par la liste de colonnes de clé d’index.

## <a name="comparing-when-to-use-each-index-type"></a>Comparaison des utilisations de chaque type d’index  
  
La nature de vos requêtes particulières détermine le type d’index le mieux approprié.  

Quand vous implémentez des tables optimisées en mémoire dans une application existante, la recommandation générale consiste à commencer avec des index non cluster, dont les fonctionnalités rappellent celles des index cluster et non cluster traditionnels sur les tables sur disque. 
  
### <a name="recommendations-for-nonclustered-index-use"></a>Recommandations d’utilisation d’un index non-cluster  
  
Un index non cluster est préférable à un index de hachage dans les cas suivants :  
  
- Les requêtes ont une clause `ORDER BY` sur la colonne indexée.  
- Requêtes où seule(s) la (les) colonne(s) de début d’un index sur plusieurs colonnes est (sont) testée(s).  
- Les requêtes testent la colonne indexée à l’aide d’une clause `WHERE` avec :  
  - Une inégalité : `WHERE StatusCode != 'Done'`  
  - Une analyse de plage de valeurs : `WHERE Quantity >= 100`  
  
Dans toutes les instructions SELECT suivantes, un index non cluster est préférable à un index de hachage :  

```sql
SELECT CustomerName, Priority, Description 
FROM SupportEvent  
WHERE StartDateTime > DateAdd(day, -7, GetUtcDate());  

SELECT StartDateTime, CustomerName  
FROM SupportEvent  
ORDER BY StartDateTime DESC; -- ASC would cause a scan.

SELECT CustomerName  
FROM SupportEvent  
WHERE StartDateTime = '2016-02-26';  
```
  
### <a name="recommendations-for-hash-index-use"></a>Recommandations d’utilisation d’un index de hachage   
  
Les [index de hachage](../../relational-databases/sql-server-index-design-guide.md#hash_index) sont principalement utilisés pour les recherches de points, mais pas pour les analyses de plage.

Un index de hachage est préférable à un index non-cluster quand les requêtes utilisent des prédicats d’égalité et que la clause `WHERE` correspond à toutes les colonnes clés d’index, comme dans l’exemple suivant :  
  
```sql
SELECT CustomerName 
FROM SupportEvent  
WHERE SupportEngineerName = 'Liz';
```  

### <a name="multi-column-index"></a>Index multicolonnes  
  
Un index multicolonnes peut être un index non-cluster ou un index de hachage. Supposons que les colonnes d’index sont col1 et col2. D’après l’instruction `SELECT` suivante, seul l’index non-cluster peut être utile à l’optimiseur de requête :  
  
```sql
SELECT col1, col3  
FROM MyTable_memop  
WHERE col1 = 'dn';  
```

L’index de hachage doit utiliser une clause `WHERE` pour spécifier un test d’égalité pour chacune des colonnes dans sa clé. Sinon, l’index de hachage n’est pas utile à l’optimiseur de requête.  
  
Aucun de ces types d’index n’est utile si la clause `WHERE` spécifie uniquement la deuxième colonne de la clé d’index.  

## <a name="summary-table-to-compare-index-use-scenarios"></a>Tableau récapitulant les scénarios d’usage des différents index  
  
Le tableau suivant répertorie toutes les opérations qui sont prises en charge par les différents types d’index. *Oui* signifie que l’index peut traiter efficacement la demande et *Non* signifie que l’index ne peut pas la traiter efficacement. 
  
| Opération | Index optimisé en mémoire, <br/> Hachage | Index optimisé en mémoire, <br/> non-cluster | Index sur disque, <br/> (non) cluster |  
| :-------- | :--------------------------- | :----------------------------------- | :------------------------------------ |  
| Analyse d'index, récupère toutes les lignes de la table. | Oui | Oui | Oui |  
| Recherche d’index sur les prédicats d’égalité (=). | Oui <br/> (Une clé complète est requise.) | Oui  | Oui |  
| Recherche d’index sur les prédicats d’inégalité et de plage <br/> (>, <, <=, >=, `BETWEEN`). | Non <br/> (Résultats dans une analyse d’index) | Oui <sup>1</sup> | Oui |  
| Récupérer les lignes selon un ordre de tri qui correspond à la définition d’index. | Non | Oui | Oui |  
| Récupérer les lignes selon un ordre de tri inverse par rapport à la définition d’index. | Non | Non | Oui |  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> Pour un index non-cluster à mémoire optimisée, la clé complète n’est pas nécessaire pour effectuer une recherche d’index.  

## <a name="automatic-index-and-statistics-management"></a>Gestion automatique des index et des statistiques

Tirez parti de solutions comme [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) pour gérer automatiquement la défragmentation des index et les mises à jour des statistiques pour une ou plusieurs bases de données. Cette procédure choisit automatiquement s’il faut reconstruire ou réorganiser un index en fonction de son niveau de fragmentation, entre autres, et mettre à jour les statistiques avec un seuil linéaire.

## <a name="see-also"></a><a name="Additional_Reading"></a> Voir aussi   
 [Guide de conception d’index SQL Server](../../relational-databases/sql-server-index-design-guide.md)   
 [Index de hachage pour les tables à mémoire optimisée](../../relational-databases/sql-server-index-design-guide.md#hash_index)   
 [Index non-cluster pour les tables à mémoire optimisée](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index)    
 [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)