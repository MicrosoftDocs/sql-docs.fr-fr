---
title: Expression CASE dans une procédure stockée compilée nativement
description: Les modules T-SQL compilés en nativement prennent en charge les expressions CASE dans certaines versions de SQL Server. Cet exemple implémente l’expression CASE dans une requête.
ms.custom: seo-dt-2019
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2f82db01-da7e-4a7d-8bc0-48b245e6f768
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9711cebcaab12cd0c665591612b56aefd8f0cfbb
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351262"
---
# <a name="implementing-a-case-expression-in-a-natively-compiled-stored-procedure"></a>Implémentation d’une expression CASE dans une procédure stockée compilée en mode natif
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

**S’applique à :** [!INCLUDE[ssSDSFull_md](../../includes/sssdsfull-md.md)] et SQL Server à partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]

Les expressions CASE sont prises en charge dans les modules T-SQL compilés en mode natif. L’exemple suivant montre une façon d’utiliser l’expression CASE dans une requête. 

``` 
-- Query using a CASE expression in a natively compiled stored procedure.
CREATE PROCEDURE dbo.usp_SOHOnlineOrderResult  
   WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
   AS BEGIN ATOMIC WITH  (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE=N'us_english')  
   SELECT   
      SalesOrderID,   
      CASE (OnlineOrderFlag)   
      WHEN 1 THEN N'Order placed online by customer'  
      ELSE N'Order placed by sales person'  
      END  
   FROM Sales.SalesOrderHeader_inmem
END  
GO  
  
EXEC dbo.usp_SOHOnlineOrderResult  
GO  
``` 

**S’applique à :** [!INCLUDE[ssSQL14-md](../../includes/ssSQL14-md.md)] et SQL Server à partir de [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]

  Les expressions CASE *ne sont pas* prises en charge dans les modules T-SQL compilés en mode natif. L’exemple suivant illustre une méthode permettant d’implémenter les fonctionnalités d’une expression CASE dans une procédure stockée compilée en mode natif.  
  
 Les exemples de code utilisent une variable de table pour construire un jeu de résultats unique. Cela est uniquement adapté en cas de traitement d’un nombre limité de lignes, puisque cela implique la création d’une copie supplémentaire des lignes de données.  
  
 Vous devez tester les performances de cette solution de contournement.  
  
```  
-- original query  
SELECT   
   SalesOrderID,   
   CASE (OnlineOrderFlag)   
   WHEN 1 THEN N'Order placed online by customer'  
   ELSE N'Order placed by sales person'  
   END  
FROM Sales.SalesOrderHeader_inmem  
  
--  workaround for CASE in natively compiled stored procedures  
--  use a table for the single resultset  
CREATE TYPE dbo.SOHOnlineOrderResult AS TABLE  
(  
   SalesOrderID uniqueidentifier not null index ix_SalesOrderID,  
     OrderFlag nvarchar(100) not null  
) with (memory_optimized=on)  
go  
  
-- natively compiled stored procedure that includes the query  
CREATE PROCEDURE dbo.usp_SOHOnlineOrderResult  
   WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
   AS BEGIN ATOMIC WITH  
      (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE=N'us_english')  
  
   -- table variable for creating the single resultset  
   DECLARE @result dbo.SOHOnlineOrderResult  
  
   -- CASE OnlineOrderFlag=1  
   INSERT @result   
   SELECT SalesOrderID, N'Order placed online by customer'  
      FROM Sales.SalesOrderHeader_inmem  
      WHERE OnlineOrderFlag=1  
  
   -- ELSE  
   INSERT @result   
   SELECT SalesOrderID, N'Order placed by sales person'  
      FROM Sales.SalesOrderHeader_inmem  
      WHERE OnlineOrderFlag!=1  
  
   -- return single resultset  
   SELECT SalesOrderID, OrderFlag FROM @result  
END  
GO  
  
EXEC dbo.usp_SOHOnlineOrderResult  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Problèmes de migration pour les procédures stockées compilées en mode natif](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [Constructions Transact-SQL non prises en charge par In-Memory OLTP](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  
