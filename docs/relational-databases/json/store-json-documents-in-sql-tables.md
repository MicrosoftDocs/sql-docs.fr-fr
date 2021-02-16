---
title: Stocker des documents JSON dans SQL Server ou SQL Database
description: Cet article décrit les raisons pour lesquelles stocker et indexer des documents JSON dans SQL Server ou SQL Database et la procédure à suivre, ainsi que l’optimisation des requêtes sur les documents JSON.
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 30d51162b36d787e8976b2c6aa48e533fd476f8e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015059"
---
# <a name="store-json-documents-in-sql-server-or-sql-database"></a>Stocker des documents JSON dans SQL Server ou SQL Database
SQL Server et Azure SQL Database ont des fonctions JSON natives qui vous permettent d’analyser des documents JSON à l’aide du langage SQL standard. Vous pouvez stocker des documents JSON dans SQL Server ou SQL Database et interroger les données JSON comme dans une base de données NoSQL. Cet article explique comment stocker des documents JSON dans SQL Server ou SQL Database.

## <a name="json-storage-format"></a>Format de stockage JSON

La première décision de conception du stockage concerne la manière de stocker des documents JSON dans les tables. Deux options sont disponibles :
- **Stockage LOB** - Les documents JSON peuvent être stockés tels quels dans des colonnes `NVARCHAR`. Il s’agit de la meilleure façon de charger et d’ingérer rapidement les données, car la vitesse de chargement correspond au chargement des colonnes de chaînes. Cette approche peut entraîner une dégradation supplémentaire des performances au moment de la requête/analyse si l’indexation des valeurs JSON n’est pas effectuée, car les documents JSON bruts doivent être analysés pendant l’exécution des requêtes. 
- **Stockage relationnel** - Les documents JSON peuvent être analysés pendant qu’ils sont insérés dans la table à l’aide de fonctions `OPENJSON`, `JSON_VALUE` ou `JSON_QUERY`. Les fragments des documents JSON d’entrée peuvent être stockés dans les colonnes de type de données SQL ou dans des colonnes NVARCHAR contenant des sous-éléments JSON. Cette approche augmente le temps de chargement, car l’analyse JSON est effectuée pendant le chargement. Toutefois, les performances des requêtes sont identiques à celles des requêtes classiques sur les données relationnelles.

## <a name="classic-tables"></a>Tables classiques

La façon la plus simple de stocker des documents JSON dans SQL Server ou SQL Database consiste à créer une table de deux colonnes qui contient l’ID et le contenu du document. Par exemple :

```sql
create table WebSite.Logs (
    _id bigint primary key identity,
    log nvarchar(max)
);
```

Cette structure est équivalente aux collections que vous pouvez trouver dans les bases de données de document classiques. La clé primaire `_id` est une valeur à incrémentation automatique qui fournit un identificateur unique pour chaque document et permet d’effectuer des recherches rapides. Cette structure est un bon choix pour les scénarios NoSQL classiques dans lesquels vous souhaitez récupérer un document par son ID ou mettre à jour un document stocké par son ID.

Le type de données nvarchar(max) vous permet de stocker des documents JSON ayant une taille maximale de 2 Go. Toutefois, si vous êtes sûr que la taille de vos documents JSON n’est pas supérieure à 8 Ko, nous vous recommandons d’utiliser NVARCHAR(4000) au lieu de NVARCHAR(max) pour des raisons de performances.

L’exemple de table créé dans l’exemple précédent suppose que des documents JSON valides sont stockés dans la colonne `log`. Si vous souhaitez que soit enregistré du JSON valide dans la colonne `log`, vous pouvez ajouter une contrainte CHECK sur cette colonne. Par exemple :

```sql
ALTER TABLE WebSite.Logs
    ADD CONSTRAINT [Log record should be formatted as JSON]
                   CHECK (ISJSON(log)=1)
```

Chaque fois qu’un utilisateur insère ou met à jour un document dans la table, cette contrainte vérifie que le document JSON est correctement mis en forme. Sans la contrainte, la table est optimisée pour les insertions, car tout document JSON est ajouté directement à la colonne sans aucun traitement.

Quand vous stockez vos documents JSON dans la table, vous pouvez utiliser le langage Transact-SQL standard pour les interroger. Par exemple :

```sql
SELECT TOP 100 JSON_VALUE(log, '$.severity'), AVG( CAST( JSON_VALUE(log,'$.duration') as float))
 FROM WebSite.Logs
 WHERE CAST( JSON_VALUE(log,'$.date') as datetime) > @datetime
 GROUP BY JSON_VALUE(log, '$.severity')
 HAVING AVG( CAST( JSON_VALUE(log,'$.duration') as float) ) > 100
 ORDER BY AVG( CAST( JSON_VALUE(log,'$.duration') as float) ) DESC
```

La possibilité d’utiliser *n’importe quelle* clause de requête et fonction T-SQL pour interroger des documents JSON est un réel avantage. SQL Server et SQL Database n’introduisent aucune contrainte dans les requêtes que vous pouvez utiliser pour analyser des documents JSON. Vous pouvez extraire des valeurs d’un document JSON avec la fonction `JSON_VALUE` et les utiliser dans la requête comme toute autre valeur.

Cette possibilité d’utiliser une syntaxe de requête T-SQL étendue est la principale différence entre d’une part SQL Server et SQL Database, et d’autre part les bases de données NoSQL classiques : dans Transact-SQL, vous avez probablement toutes les fonctions dont vous avez besoin pour traiter les données JSON.

## <a name="indexes"></a>Index

Si vous constatez que vos requêtes parcourent souvent les documents en fonction d’une propriété (par exemple, une propriété `severity` dans un document JSON), vous pouvez ajouter un index NONCLUSTERED classique sur la propriété pour accélérer les requêtes.

Vous pouvez créer une colonne calculée qui expose des valeurs JSON à partir des colonnes JSON sur le chemin spécifié (autrement dit, sur le chemin `$.severity`) et créer un index standard sur cette colonne calculée. Par exemple :

```sql
create table WebSite.Logs (
    _id bigint primary key identity,
    log nvarchar(max),

    severity AS JSON_VALUE(log, '$.severity'),
    index ix_severity (severity)
);
```

La colonne calculée utilisée dans cet exemple est une colonne non persistante ou virtuelle qui n’entraîne aucune augmentation de l’espace occupé par la table. Elle est utilisée par l’index `ix_severity` pour améliorer les performances des requêtes, comme dans l’exemple suivant :

```sql
SELECT log
FROM Website.Logs
WHERE JSON_VALUE(log, '$.severity') = 'P4'
```

Une caractéristique importante de cet index est qu’il prend en charge les classements. Si votre colonne NVARCHAR d’origine a une propriété COLLATION (par exemple, un classement respectant la casse ou en fonction de la langue japonaise), l’index est organisé selon les règles de la langue ou du respect de la casse associées à la colonne NVARCHAR. Cette prise en compte du classement peut être une fonctionnalité importante si vous développez des applications à l’échelle internationale qui doivent utiliser des règles linguistiques personnalisées lors du traitement de documents JSON.

## <a name="large-tables--columnstore-format"></a>Tables de grande taille et mise en forme de columnstore

Si vous prévoyez un grand nombre de documents JSON dans votre collection, nous vous recommandons d’ajouter un index CLUSTERED COLUMNSTORE dans la collection, comme indiqué dans l’exemple suivant :

```sql
create sequence WebSite.LogID as bigint;
go
create table WebSite.Logs (
    _id bigint default(next value for WebSite.LogID),
    log nvarchar(max),

    INDEX cci CLUSTERED COLUMNSTORE
);
```

Un index CLUSTERED COLUMNSTORE fournit un taux de compression de données élevé (jusqu’à 25 fois plus important) qui peut considérablement réduire vos besoins en espace de stockage, baisser les coûts de stockage et améliorer les performances d’E/S de votre charge de travail. De plus, les index CLUSTERED COLUMNSTORE étant optimisés pour l’analytique et les analyses de table sur vos documents JSON, ils constituent probablement la meilleure option pour l’analytique des journaux.

L’exemple précédent utilise un objet de séquence pour attribuer des valeurs à la colonne `_id`. Les séquences et les identités sont des options valides pour la colonne d’ID.

## <a name="frequently-changing-documents--memory-optimized-tables"></a>Changement fréquent de documents et de tables à mémoire optimisée

Si vous prévoyez un grand nombre d’opérations de suppression, d’insertion et de mise à jour dans vos collections, vous pouvez stocker vos documents JSON dans des tables optimisées en mémoire. Les collections JSON à mémoire optimisée conservant toujours les données en mémoire, il n’existe aucune surcharge d’E/S pour le stockage. De plus, les collections JSON à mémoire optimisée sont complètement dépourvues de verrou ; autrement dit, les actions sur les documents ne bloquent aucune autre opération.

La seule chose à faire pour convertir une collection classique en collection optimisée en mémoire est de spécifier l’option **with (memory_optimized=on)** après la définition de table, comme indiqué dans l’exemple suivant. Vous disposez alors d’une version à mémoire optimisée de la collection JSON.

```sql
create table WebSite.Logs (
  _id bigint identity primary key nonclustered,
  log nvarchar(4000)
) with (memory_optimized=on)
```

Une table à mémoire optimisée est la meilleure option pour les documents qui changent fréquemment. Quand vous envisagez des tables à mémoire optimisée, prenez également en considération le niveau de performance. Si possible, utilisez NVARCHAR(4000) au lieu de NVARCHAR(max) pour les documents JSON dans vos collections à mémoire optimisée, car cela peut améliorer considérablement le niveau de performance.

Comme avec les tables classiques, vous pouvez ajouter des index sur les champs que vous exposez dans des tables optimisées en mémoire avec des colonnes calculées. Par exemple :

```sql
create table WebSite.Logs (

  _id bigint identity primary key nonclustered,
  log nvarchar(4000),

  severity AS cast(JSON_VALUE(log, '$.severity') as tinyint) persisted,
  index ix_severity (severity)

) with (memory_optimized=on)
```

Pour optimiser les performances, effectuez un cast de la valeur JSON vers le plus petit type possible pouvant contenir la valeur de la propriété. Dans l’exemple précédent, **tinyint** est utilisé.

Vous pouvez également placer les requêtes SQL qui mettent à jour les documents JSON dans des procédures stockées pour tirer parti de la compilation native. Par exemple :

```sql
CREATE PROCEDURE WebSite.UpdateData(@Id int, @Property nvarchar(100), @Value nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION

AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    UPDATE WebSite.Logs
    SET log = JSON_MODIFY(log, @Property, @Value)
    WHERE _id = @Id;

END
```

Cette procédure compilée en mode natif prend la requête et crée un code .DLL qui exécute la requête. Une procédure compilée en mode natif est l’approche la plus rapide pour interroger et mettre à jour des données.

## <a name="conclusion"></a>Conclusion

Les fonctions JSON natives dans SQL Server et SQL Database vous permettent de traiter des documents JSON comme dans les bases de données NoSQL. Chaque base de données - relationnelle ou NoSQL - présente des avantages et des inconvénients pour le traitement des données JSON. Le principal avantage du stockage des documents JSON dans SQL Server ou SQL Database est la prise en charge complète du langage SQL. Vous pouvez utiliser toute la palette du langage Transact-SQL pour traiter les données et configurer de nombreuses options de stockage (des index columnstore pour la compression à un taux élevé et l’analytique rapide aux tables à mémoire optimisée pour le traitement sans verrou). Parallèlement, vous tirez parti de fonctionnalités de sécurité et d’internationalisation matures, que vous pouvez facilement réutiliser dans votre scénario NoSQL. Les raisons décrites dans cet article justifient pleinement d’envisager de stocker les documents JSON dans SQL Server ou SQL Database.

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>En savoir plus sur JSON dans SQL Server et Azure SQL Database  
  
### <a name="microsoft-videos"></a>Vidéos Microsoft

Pour obtenir une présentation visuelle de la prise en charge intégrée de JSON dans SQL Server et Azure SQL Database, consultez les vidéos suivantes :

-   [SQL Server 2016 et prise en charge de JSON](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [Utilisation de JSON dans SQL Server 2016 et Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON : un pont entre les mondes relationnel et NoSQL](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
