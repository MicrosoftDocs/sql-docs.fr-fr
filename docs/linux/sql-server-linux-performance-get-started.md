---
title: Bien démarrer avec les fonctionnalités relatives aux performances de SQL Server sur Linux
description: Cet article sert d’introduction aux fonctionnalités relatives aux performances de SQL Server pour les utilisateurs Linux qui découvrent SQL Server. Beaucoup de ces exemples fonctionnent sur toutes les plateformes, mais le contexte de cet article est Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 60036d26-4797-4872-9a9e-3552841c61be
ms.openlocfilehash: 24794910e53f730b16975714c514eb717b41ce49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350963"
---
# <a name="walkthrough-for-the-performance-features-of-sql-server-on-linux"></a>Procédure pas à pas pour les fonctionnalités relatives aux performances de SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Si vous êtes un utilisateur de Linux nouveau dans SQL Server, les tâches suivantes vous guident tout au long de certaines tâches relatives aux performances. Celles-ci ne sont pas uniques ni spécifiques à Linux, mais elles vous permettent de vous faire une idée des domaines à examiner en détail. Dans chaque exemple, un lien est fourni vers la documentation détaillée de cette zone.

> [!NOTE]
> Les exemples suivants utilisent l’exemple de base de données AdventureWorks. Pour obtenir des instructions sur l’obtention et l’installation de cet exemple de base de données, consultez [Restaurer une base de données SQL Server à partir de Windows vers Linux](sql-server-linux-migrate-restore-database.md).

## <a name="create-a-columnstore-index"></a>Créer un index columnstore
Un index columnstore est une technologie permettant de stocker et d’interroger de grands magasins de données dans un format de données en colonnes, appelé columnstore.  

1. Ajoutez un index columnstore à la table SalesOrderDetail en exécutant les commandes Transact-SQL suivantes :

   ```sql
   CREATE NONCLUSTERED COLUMNSTORE INDEX [IX_SalesOrderDetail_ColumnStore]
      ON Sales.SalesOrderDetail
      (UnitPrice, OrderQty, ProductID)
   GO
   ```

2. Exécutez la requête suivante qui utilise l'index columnstore pour scanner la table :

   ```sql
   SELECT ProductID, SUM(UnitPrice) SumUnitPrice, AVG(UnitPrice) AvgUnitPrice,
      SUM(OrderQty) SumOrderQty, AVG(OrderQty) AvgOrderQty
   FROM Sales.SalesOrderDetail
      GROUP BY ProductID
      ORDER BY ProductID
   ```

3. Vérifiez que l'index columnstore a été utilisé en recherchant la valeur object_id pour l'index Columnstore, puis en vérifiant que cette valeur apparaît dans les statistiques d'utilisation de la table SalesOrderDetail :

   ```sql
   SELECT * FROM sys.indexes WHERE name = 'IX_SalesOrderDetail_ColumnStore'
   GO

   SELECT * 
   FROM sys.dm_db_index_usage_stats
      WHERE database_id = DB_ID('AdventureWorks')
      AND object_id = OBJECT_ID('AdventureWorks.Sales.SalesOrderDetail');
   ```
   
## <a name="use-in-memory-oltp"></a>Utiliser OLTP en mémoire
SQL Server fournit des fonctionnalités OLTP en mémoire qui peuvent améliorer considérablement les performances des systèmes d’applications.  Cette section du guide d'évaluation vous guidera à travers les étapes de création d'une table à mémoire optimisée et d'une procédure stockée compilée nativement capable d’accéder à la table sans avoir besoin d'être compilée ou interprétée.

### <a name="configure-database-for-in-memory-oltp"></a>Configurer la base de données pour OLTP en mémoire
1. Il est recommandé de définir la base de données à un niveau de compatibilité d'au moins 130 pour utiliser OLTP en mémoire.  Utilisez la requête suivante pour vérifier le niveau de compatibilité actuel d'AdventureWorks :  

   ```sql
   USE AdventureWorks
   GO
   SELECT d.compatibility_level
   FROM sys.databases as d
     WHERE d.name = Db_Name();
   GO
   ```
   
   Si nécessaire, réglez le niveau à 130 :

   ```sql
   ALTER DATABASE CURRENT
   SET COMPATIBILITY_LEVEL = 130;
   GO
   ```

2. Lorsqu’une transaction implique à la fois une table sur disque et une table à mémoire optimisée, il est essentiel que la partie optimisation en mémoire de la transaction fonctionne au niveau d’isolation de la transaction, nommé SNAPSHOT.  Pour appliquer de manière fiable ce niveau aux tables optimisées en mémoire dans une transaction entre conteneurs, exécutez ce qui suit :

   ```sql
   ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON
   GO
   ```

3. Avant de créer une table optimisée en mémoire, vous devez créer un groupe de fichiers Memory Optimized FILEGROUP et un conteneur pour les fichiers de données :

   ```sql
   ALTER DATABASE AdventureWorks ADD FILEGROUP AdventureWorks_mod CONTAINS memory_optimized_data
   GO  
   ALTER DATABASE AdventureWorks ADD FILE (NAME='AdventureWorks_mod', FILENAME='/var/opt/mssql/data/AdventureWorks_mod') TO FILEGROUP AdventureWorks_mod
   GO
   ```

### <a name="create-a-memory-optimized-table"></a>Créer une table optimisée en mémoire
La banque principale des tables à mémoire optimisée est la mémoire principale et, contrairement aux tables sur disque, les données n'ont pas besoin d'être lues à partir du disque dans les tampons de mémoire.  Pour créer une table optimisée en mémoire, utilisez la clause MEMORY_OPTIMIZED = ON.

1. Exécutez la requête suivante pour créer la table optimisée en mémoire dbo.ShoppingCart.  Par défaut, les données seront conservées sur le disque à des fins de durabilité (notez que le niveau DURABILITY peut également être défini pour ne conserver que le schéma). 

   ```sql
   CREATE TABLE dbo.ShoppingCart ( 
   ShoppingCartId INT IDENTITY(1,1) PRIMARY KEY NONCLUSTERED,
   UserId INT NOT NULL INDEX ix_UserId NONCLUSTERED HASH WITH (BUCKET_COUNT=1000000), 
   CreatedDate DATETIME2 NOT NULL, 
   TotalPrice MONEY
   ) WITH (MEMORY_OPTIMIZED=ON) 
   GO
   ```

2. Insérez quelques enregistrements dans la table :

   ```sql
   INSERT dbo.ShoppingCart VALUES (8798, SYSDATETIME(), NULL) 
   INSERT dbo.ShoppingCart VALUES (23, SYSDATETIME(), 45.4) 
   INSERT dbo.ShoppingCart VALUES (80, SYSDATETIME(), NULL) 
   INSERT dbo.ShoppingCart VALUES (342, SYSDATETIME(), 65.4) 
   ```

### <a name="natively-compiled-stored-procedure"></a>Procédure stockée compilée en mode natif
SQL Server prend en charge les procédures stockées compilées en mode natif qui accèdent aux tables mémoire optimisées. Les instructions T-SQL sont compilées en code machine et stockées sous forme de DLL natives, ce qui permet un accès plus rapide aux données et une exécution des requêtes plus efficace qu’avec des requêtes T-SQL traditionnelles.   Les procédures stockées qui sont identifiées par NATIVE_COMPILATION sont compilées en mode natif. 

1. Exécutez le script suivant pour créer une procédure stockée compilée en mode natif qui insère un grand nombre d'enregistrements dans la table ShoppingCart :


   ```sql
   CREATE PROCEDURE dbo.usp_InsertSampleCarts @InsertCount int 
    WITH NATIVE_COMPILATION, SCHEMABINDING AS 
   BEGIN ATOMIC 
   WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')

   DECLARE @i int = 0

   WHILE @i < @InsertCount 
    BEGIN 
     INSERT INTO dbo.ShoppingCart VALUES (1, SYSDATETIME() , NULL) 
     SET @i += 1 
    END
   END 
   ```
2. Insérez 1 000 000 de lignes :

   ```sql
   EXEC usp_InsertSampleCarts 1000000 
   ```

3. Vérifiez que les lignes ont été insérées :

   ```sql
   SELECT COUNT(*) FROM dbo.ShoppingCart 
   ```

### <a name="learn-more-about-in-memory-oltp"></a>En savoir plus sur OLTP en mémoire
Pour plus d'informations sur OLTP en mémoire, consultez les rubriques suivantes :

- [Démarrage rapide 1 : technologies OLTP en mémoire pour accélérer les performances Transact-SQL](../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)
- [Migration vers OLTP en mémoire](../relational-databases/in-memory-oltp/plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)
- [Table temporaire et variable de table plus rapides à l’aide de l’optimisation en mémoire](../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md)
- [Surveiller l’utilisation de la mémoire et résoudre les problèmes connexes](../relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage.md)
- [In-Memory OLTP (optimisation en mémoire)](../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)

## <a name="use-query-store"></a>Utiliser Magasin des requêtes
Magasin des requêtes collecte des informations détaillées sur les performances des requêtes, les plans d'exécution et les statistiques d'exécution.

Magasin des requêtes n'est pas actif par défaut et peut être activé avec ALTER DATABASE :

   ```sql
   ALTER DATABASE AdventureWorks SET QUERY_STORE = ON;
   ```

Exécutez la requête suivante pour retourner des informations sur les requêtes et les plans dans le magasin des requêtes : 

   ```sql
   SELECT Txt.query_text_id, Txt.query_sql_text, Pl.plan_id, Qry.*
   FROM sys.query_store_plan AS Pl
      JOIN sys.query_store_query AS Qry
         ON Pl.query_id = Qry.query_id
      JOIN sys.query_store_query_text AS Txt
         ON Qry.query_text_id = Txt.query_text_id ;
   ```

## <a name="query-dynamic-management-views"></a>Interroger les vues de gestion dynamique
Les vues de gestion dynamique renvoient des informations sur l'état du serveur qu'il est possible d'utiliser pour surveiller l'état d'une instance du serveur, diagnostiquer des problèmes et améliorer les performances.

Pour interroger la vue de gestion dynamique dm_os_wait stats :

   ```sql
   SELECT wait_type, wait_time_ms
   FROM sys.dm_os_wait_stats;
   ```