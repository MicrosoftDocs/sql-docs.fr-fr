---
title: Table à mémoire optimisée et procédure stockée compilée nativement
description: Cet exemple vous présente la syntaxe de l'OLTP en mémoire pour créer un tableau optimisé pour la mémoire et une procédure stockée compilée nativement.
ms.custom: seo-dt-2019
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 48a9a0a3-930f-477b-bd0f-e82e77999ecc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 10c344ac156a59dda89e678709e6b846bf3c33a0
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89541175"
---
# <a name="creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure"></a>Création d'une table mémoire optimisée et d'une procédure stockée compilée en mode natif

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette rubrique contient un exemple qui présente la syntaxe de l'OLTP en mémoire.  

Pour permettre à une application d'utiliser l'OLTP en mémoire, effectuez les tâches suivantes :  

-   Créez un groupe de fichiers de données mémoire optimisé et ajoutez un conteneur au groupe de fichiers.  
  
-   Créez des tables et des index mémoire optimisés. Pour plus d’informations, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
-   Chargez les données dans la table mémoire optimisée et mettez à jour les statistiques après le chargement des données et avant de créer les procédures stockées compilées. Pour plus d’informations, consultez [Statistiques pour les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/statistics-for-memory-optimized-tables.md).  
  
-   Créez des procédures stockées compilées en mode natif pour accéder aux données des tables mémoire optimisées. Pour plus d’informations, consultez [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md). Vous pouvez également utiliser un [!INCLUDE[tsql](../../includes/tsql-md.md)] traditionnel et interprète pour accéder aux données dans les tables mémoire optimisées.  
  
-   Le cas échéant, effectuez la migration des données de tables existantes vers des tables mémoire optimisées.  

## <a name="background-on-in-memory-objects"></a>Informations générales sur les objets en mémoire

Pour plus d’informations sur l’utilisation de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour créer des tables optimisées en mémoire, consultez [Prise en charge de SQL Server Management Studio pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/sql-server-management-studio-support-for-in-memory-oltp.md).  

### <a name="natively-compiled-stored-procedures"></a>Procédures stockées compilées en mode natif

Les procédures stockées compilées en mode natif sont des procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] compilées en code natif et qui accèdent aux tables optimisées en mémoire. Elles permettent aux requêtes et à la logique métier qu'elles contiennent de s'exécuter rapidement. Pour plus d'informations sur le processus de compilation en mode natif, consultez [Native Compilation of Tables and Stored Procedures](../../relational-databases/in-memory-oltp/native-compilation-of-tables-and-stored-procedures.md). Pour plus d’informations sur la migration des procédures stockées sur disque vers des procédures stockées compilées en mode natif, consultez [Problèmes de migration pour les procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/migration-issues-for-natively-compiled-stored-procedures.md).

> [!NOTE]
> La différence entre les procédures stockées (sur disque) interprétées et les procédures stockées compilées en mode natif réside dans le fait qu’une procédure stockée interprétée est compilée lors de la première exécution, tandis qu’une procédure stockée compilée en mode natif l’est au moment de sa création. Avec des procédures stockées compilées en mode natif, de nombreuses conditions d’erreur peuvent être détectées lors de la création et entraîner l’échec de la création de la procédure stockée compilée en mode natif (dépassement arithmétique, conversion de type et certaines conditions de division par zéro). Avec des procédures stockées interprétées, ces conditions d’erreur ne provoquent généralement pas d’échec quand la procédure stockée est créée, mais toutes les exécutions échouent.

## <a name="code-example-in-t-sql"></a>Exemple de code en T-SQL

L’exemple de code suivant nécessite un répertoire nommé c:\Data\.

```sql  
CREATE DATABASE imoltp   
GO  
  
--------------------------------------  
-- create database with a memory-optimized
-- filegroup and a container.

ALTER DATABASE imoltp ADD FILEGROUP imoltp_mod
    CONTAINS MEMORY_OPTIMIZED_DATA;

ALTER DATABASE imoltp ADD FILE (
    name='imoltp_mod1', filename='c:\data\imoltp_mod1')
    TO FILEGROUP imoltp_mod;

ALTER DATABASE imoltp
    SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
GO  
  
USE imoltp  
GO  
  
-- Create a durable (data will be persisted) memory-optimized table
-- two of the columns are indexed.

CREATE TABLE dbo.ShoppingCart (   
    ShoppingCartId INT IDENTITY(1,1) PRIMARY KEY NONCLUSTERED,  
    UserId INT NOT NULL INDEX ix_UserId NONCLUSTERED
        HASH WITH (BUCKET_COUNT=1000000),
    CreatedDate DATETIME2 NOT NULL,   
    TotalPrice MONEY  
    ) WITH (MEMORY_OPTIMIZED=ON)   
GO  

-- Create a non-durable table. Data will not be persisted,
-- data loss if the server turns off unexpectedly.

CREATE TABLE dbo.UserSession (   
   SessionId INT IDENTITY(1,1) PRIMARY KEY NONCLUSTERED
        HASH WITH (BUCKET_COUNT=400000),
   UserId int NOT NULL,   
   CreatedDate DATETIME2 NOT NULL,  
   ShoppingCartId INT,  
   INDEX ix_UserId NONCLUSTERED
        HASH (UserId) WITH (BUCKET_COUNT=400000)   
    )   
    WITH (MEMORY_OPTIMIZED=ON, DURABILITY=SCHEMA_ONLY)
GO  
  
-- insert data into the tables  
INSERT dbo.UserSession VALUES (342, SYSDATETIME(), 4);
INSERT dbo.UserSession VALUES (65, SYSDATETIME(), NULL)   
INSERT dbo.UserSession VALUES (8798, SYSDATETIME(), 1)   
INSERT dbo.UserSession VALUES (80, SYSDATETIME(), NULL)   
INSERT dbo.UserSession VALUES (4321, SYSDATETIME(), NULL)   
INSERT dbo.UserSession VALUES (8578, SYSDATETIME(), NULL)   
  
INSERT dbo.ShoppingCart VALUES (8798, SYSDATETIME(), NULL)   
INSERT dbo.ShoppingCart VALUES (23, SYSDATETIME(), 45.4)   
INSERT dbo.ShoppingCart VALUES (80, SYSDATETIME(), NULL)   
INSERT dbo.ShoppingCart VALUES (342, SYSDATETIME(), 65.4)   
GO  
  
-- Verify table contents.

SELECT * FROM dbo.UserSession;
SELECT * FROM dbo.ShoppingCart;
GO  
  
-- Update statistics on memory-optimized tables;

UPDATE STATISTICS dbo.UserSession  WITH FULLSCAN, NORECOMPUTE;
UPDATE STATISTICS dbo.ShoppingCart WITH FULLSCAN, NORECOMPUTE;
GO  
  
-- in an explicit transaction, assign a cart to a session
-- and update the total price.
-- SELECT/UPDATE/DELETE statements in explicit transactions.

BEGIN TRAN;
   UPDATE dbo.UserSession SET ShoppingCartId=3 WHERE SessionId=4;
   UPDATE dbo.ShoppingCart SET TotalPrice=65.84 WHERE ShoppingCartId=3;
COMMIT;
GO   
  
-- Verify table contents.

SELECT *   
    FROM dbo.UserSession u
        JOIN dbo.ShoppingCart s on u.ShoppingCartId=s.ShoppingCartId
    WHERE u.SessionId=4;
 GO  
  
-- Natively compiled stored procedure for assigning
-- a shopping cart to a session.

CREATE PROCEDURE dbo.usp_AssignCart @SessionId int
    WITH NATIVE_COMPILATION, SCHEMABINDING
AS
BEGIN ATOMIC
    WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
        LANGUAGE = N'us_english');
  
  DECLARE @UserId INT,  
          @ShoppingCartId INT;
  
  SELECT @UserId=UserId, @ShoppingCartId=ShoppingCartId
  FROM dbo.UserSession WHERE SessionId=@SessionId;
  
  IF @UserId IS NULL   
    THROW 51000, N'The session or shopping cart does not exist.', 1;
  
  UPDATE dbo.UserSession
    SET ShoppingCartId=@ShoppingCartId WHERE SessionId=@SessionId;
 END   
 GO  
  
 EXEC usp_AssignCart 1;
 GO  
  
-- natively compiled stored procedure for inserting
-- a large number of rows this demonstrates the
-- performance of native procs   
CREATE PROCEDURE dbo.usp_InsertSampleCarts @InsertCount int
    WITH NATIVE_COMPILATION, SCHEMABINDING   
AS
BEGIN ATOMIC   
    WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
        LANGUAGE = N'us_english');
  
  DECLARE @i int = 0;
  
  WHILE @i < @InsertCount   
  BEGIN   
    INSERT INTO dbo.ShoppingCart VALUES (1, SYSDATETIME() , NULL)
    SET @i += 1   
  END  
  
END   
GO  
  
-- insert 1,000,000 rows   
 EXEC usp_InsertSampleCarts 1000000   
 GO  
  
---- verify the rows have been inserted   
 SELECT COUNT(*) FROM dbo.ShoppingCart   
 GO  
  
-- Sample memory-optimized tables for
-- sales orders and sales order details.
CREATE TABLE dbo.SalesOrders   
(  
   so_id INT NOT NULL PRIMARY KEY NONCLUSTERED,  
   cust_id INT NOT NULL,  
   so_date DATE NOT NULL INDEX ix_date NONCLUSTERED,  
   so_total MONEY NOT NULL,  
   INDEX ix_date_total NONCLUSTERED
        (so_date DESC, so_total DESC)  
) WITH (MEMORY_OPTIMIZED=ON);
GO  
  
CREATE TABLE dbo.SalesOrderDetails  
(  
   so_id INT NOT NULL,  
   lineitem_id INT NOT NULL,  
   product_id INT NOT NULL,  
   unitprice MONEY NOT NULL,  
  
   CONSTRAINT PK_SOD PRIMARY KEY NONCLUSTERED
        (so_id,lineitem_id)
) WITH (MEMORY_OPTIMIZED=ON)  
GO  

-- Memory-optimized table type for collecting
-- sales order details.

CREATE TYPE dbo.SalesOrderDetailsType AS TABLE  
(  
   so_id INT NOT NULL,  
   lineitem_id INT NOT NULL,  
   product_id INT NOT NULL,  
   unitprice MONEY NOT NULL,  
  
   PRIMARY KEY NONCLUSTERED (so_id,lineitem_id)  
) WITH (MEMORY_OPTIMIZED=ON)  
GO  
  
-- stored procedure that inserts a sales order,
-- along with its details.

CREATE PROCEDURE dbo.InsertSalesOrder
    @so_id INT, @cust_id INT,
    @items dbo.SalesOrderDetailsType READONLY  
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS BEGIN ATOMIC WITH   
(  
   TRANSACTION ISOLATION LEVEL = SNAPSHOT,  
   LANGUAGE = N'us_english'  
)  
   DECLARE @total MONEY  
   SELECT @total = SUM(unitprice) FROM @items  

   INSERT dbo.SalesOrders
        VALUES (@so_id, @cust_id, getdate(), @total)

   INSERT dbo.SalesOrderDetails
        SELECT so_id, lineitem_id, product_id, unitprice
        FROM @items  
END  
GO  
  
-- Insert a sample sales order.
DECLARE @so_id INT = 18,  
       @cust_id INT = 8,  
       @items dbo.SalesOrderDetailsType;

INSERT @items  VALUES   
       (@so_id, 1, 4, 43),   
       (@so_id, 2, 3, 3),   
       (@so_id, 3, 8, 453),   
       (@so_id, 4, 5, 76),   
       (@so_id, 5, 4, 43);

EXEC dbo.InsertSalesOrder @so_id, @cust_id, @items;
GO  
  
-- verify the content of the tables  
SELECT   
       so.so_id,  
       so.so_date,  
       sod.lineitem_id,  
       sod.product_id,  
       sod.unitprice  
FROM dbo.SalesOrders so
    JOIN dbo.SalesOrderDetails sod on so.so_id=sod.so_id  
ORDER BY so.so_id, sod.lineitem_id  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Exemples de code OLTP en mémoire](../../relational-databases/in-memory-oltp/in-memory-oltp-code-samples.md)  
  
  
