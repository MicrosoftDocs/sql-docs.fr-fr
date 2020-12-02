---
title: Amélioration du niveau de performance - OLTP en mémoire
description: Cet exemple de code illustre les performances rapides des tables à mémoire optimisée avec le langage Transact-SQL interprété et une procédure stockée compilée en mode natif.
ms.custom: seo-dt-2019
ms.date: 08/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: c6def45d-d2d4-4d24-8068-fab4cd94d8cc
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ab153d8d28e2f8005f0b0a370ffa4def42f81c58
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96125220"
---
# <a name="demonstration-performance-improvement-of-in-memory-oltp"></a>Démonstration : Optimisation des performances de l'OLTP en mémoire
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  l’exemple de code de cette rubrique illustre les performances rapides des tables optimisées en mémoire. L’optimisation des performances est évidente quand l’accès aux données dans une table optimisée en mémoire s’effectue à partir d’un [!INCLUDE[tsql](../../includes/tsql-md.md)]traditionnel et interprété. Cette amélioration des performances est encore supérieure lorsque les données dans les tables optimisées en mémoire sont accédées au moyen d’une procédure stockée compilée en mode natif (NCSProc).  
 
Pour voir une démonstration plus complète des améliorations des performances potentielles d’OLTP en mémoire, voir [Démo v1.0 des performances de la fonction OLTP en mémoire SQL Server](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0). 
  
 Dans cette rubrique, l’exemple de code est monothread et ne tire pas profit des avantages de concurrence de l’OLTP en mémoire. Une charge de travail utilisant la concurrence aura des gains de performance supérieurs. Cet exemple de code illustre un seul aspect de l’amélioration des performances, l’efficacité de l’accès aux données pour INSERT.  
  
 L’amélioration des performances offerte par les tables mémoire optimisées est totale lorsque l’accès aux données s’effectue au moyen d’une NCSProc.  
  
## <a name="code-example"></a>Exemple de code  
 Les sous-sections suivantes décrivent chaque étape.  
  
### <a name="step-1a-prerequisite-if-using-ssnoversion"></a>Étape 1 a : Condition préalable requise si vous utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Les étapes de cette première sous-section s’appliquent uniquement si vous exécutez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; elles ne s’appliquent pas si vous exécutez [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. Effectuez les actions suivantes :  
  
1.  Utilisez SQL Server Management Studio (SSMS.exe) pour vous connecter à votre serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sinon, tout autre outil similaire à SSMS.exe convient.  
  
2.  Créez manuellement un répertoire nommé **C:\data\\** . L’exemple de code Transact-SQL exige que le répertoire existe déjà.  
  
3.  Exécutez le T-SQL court pour créer la base de données et son groupe de fichiers mémoire optimisé.  
  
```sql  
go  
CREATE DATABASE imoltp;    --  Transact-SQL  
go  
  
ALTER DATABASE imoltp ADD FILEGROUP [imoltp_mod]  
    CONTAINS MEMORY_OPTIMIZED_DATA;  
  
ALTER DATABASE imoltp ADD FILE  
    (name = [imoltp_dir], filename= 'c:\data\imoltp_dir')  
    TO FILEGROUP imoltp_mod;  
go  
  
USE imoltp;  
go  
```  
  
### <a name="step-1b-prerequisite-if-using-sssdsfull"></a>Étape 1b : Condition préalable requise si vous utilisez [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]  
 Cette sous-section s’applique uniquement si vous utilisez [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. Effectuez les actions suivantes :  
  
1.  Décidez quelle base de données test existante vous allez utiliser pour l’exemple de code.  
  
2.  Si vous décidez de créer une nouvelle base de données de test, utilisez le [portail Azure](https://portal.azure.com) pour créer une base de données nommée **imoltp**.  
  
 Pour des instructions concernant l’utilisation du portail Azure, consultez [Prise en main de la base de données SQL Azure](/azure/azure-sql/database/single-database-create-quickstart).  
  
### <a name="step-2-create-memory-optimized-tables-and-ncsproc"></a>Étape 2 : Créer des tables optimisées en mémoire et une NSCProc  
 Cette étape permet de créer des tables optimisées en mémoire et une procédure stockée compilée en mode natif (NCSProc). Effectuez les actions suivantes :  
  
1.  Utilisez SSMS.exe pour vous connecter à votre nouvelle base de données.  
  
2.  Exécutez le T-SQL suivant dans votre base de données.  
  
```sql  
go  
DROP PROCEDURE IF EXISTS ncsp;  
DROP TABLE IF EXISTS sql;  
DROP TABLE IF EXISTS hash_i;  
DROP TABLE IF EXISTS hash_c;  
go  
  
CREATE TABLE [dbo].[sql] (  
  c1 INT NOT NULL PRIMARY KEY,  
  c2 NCHAR(48) NOT NULL  
);  
go  
  
CREATE TABLE [dbo].[hash_i] (  
  c1 INT NOT NULL PRIMARY KEY NONCLUSTERED HASH WITH (BUCKET_COUNT=1000000),  
  c2 NCHAR(48) NOT NULL  
) WITH (MEMORY_OPTIMIZED=ON, DURABILITY = SCHEMA_AND_DATA);  
go  
  
CREATE TABLE [dbo].[hash_c] (  
  c1 INT NOT NULL PRIMARY KEY NONCLUSTERED HASH WITH (BUCKET_COUNT=1000000),  
  c2 NCHAR(48) NOT NULL  
) WITH (MEMORY_OPTIMIZED=ON, DURABILITY = SCHEMA_AND_DATA);  
go  
  
CREATE PROCEDURE ncsp  
    @rowcount INT,  
    @c NCHAR(48)  
  WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
  AS   
  BEGIN ATOMIC   
  WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
  DECLARE @i INT = 1;  
  WHILE @i <= @rowcount  
  BEGIN;  
    INSERT INTO [dbo].[hash_c] VALUES (@i, @c);  
    SET @i += 1;  
  END;  
END;  
go  
```  
  
### <a name="step-3-run-the-code"></a>Étape 3 : Exécuter le code  
 Vous pouvez maintenant exécuter les requêtes pour démontrer les performances des tables optimisées en mémoire. Effectuez les actions suivantes :  
  
1.  Utilisez SSMS.exe pour exécuter le T-SQL suivant dans votre base de données.  
  
     Ignorez les données de vitesse ou autres données de performance générées par cette première exécution. La première exécution effectue plusieurs opérations uniques, telles que les allocations initiales de mémoire.  
  
2.  Là encore, utilisez SSMS.exe pour exécuter à nouveau le T-SQL suivant dans votre base de données.  
  
```sql  
go  
SET STATISTICS TIME OFF;  
SET NOCOUNT ON;  
  
-- Inserts, one at a time.  
  
DECLARE @starttime DATETIME2 = sysdatetime();  
DECLARE @timems INT;  
DECLARE @i INT = 1;  
DECLARE @rowcount INT = 100000;  
DECLARE @c NCHAR(48) = N'12345678901234567890123456789012345678';  
  
-- Harddrive-based table and interpreted Transact-SQL.  
  
BEGIN TRAN;  
  WHILE @i <= @rowcount  
  BEGIN;  
    INSERT INTO [dbo].[sql] VALUES (@i, @c);  
    SET @i += 1;  
  END;  
COMMIT;  
  
SET @timems = datediff(ms, @starttime, sysdatetime());  
SELECT 'A: Disk-based table and interpreted Transact-SQL: '  
    + cast(@timems AS VARCHAR(10)) + ' ms';  
  
-- Interop Hash.  
  
SET @i = 1;  
SET @starttime = sysdatetime();  
  
BEGIN TRAN;  
  WHILE @i <= @rowcount  
    BEGIN;  
      INSERT INTO [dbo].[hash_i] VALUES (@i, @c);  
      SET @i += 1;  
    END;  
COMMIT;  
  
SET @timems = datediff(ms, @starttime, sysdatetime());  
SELECT 'B: memory-optimized table with hash index and interpreted Transact-SQL: '  
    + cast(@timems as VARCHAR(10)) + ' ms';  
  
-- Compiled Hash.  
  
SET @starttime = sysdatetime();  
  
EXECUTE ncsp @rowcount, @c;  
  
SET @timems = datediff(ms, @starttime, sysdatetime());  
SELECT 'C: memory-optimized table with hash index and native SP:'  
    + cast(@timems as varchar(10)) + ' ms';  
go  
  
DELETE sql;  
DELETE hash_i;  
DELETE hash_c;  
go  
```  
  
 Les statistiques de temps de sortie générées par la seconde exécution test sont les suivantes.  
  
```sql  
10453 ms , A: Disk-based table and interpreted Transact-SQL.  
5626 ms , B: memory-optimized table with hash index and interpreted Transact-SQL.  
3937 ms , C: memory-optimized table with hash index and native SP.  
```  
  
## <a name="see-also"></a>Voir aussi  
 [OLTP en mémoire &#40;Optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
