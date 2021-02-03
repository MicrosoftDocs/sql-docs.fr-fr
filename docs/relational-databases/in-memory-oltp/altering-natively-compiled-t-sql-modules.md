---
title: Modification de modules T-SQL compilés en mode natif | Microsoft Docs
description: Découvrez comment effectuer des opérations ALTER sur les procédures stockées compilées en mode natif et les modules Transact-SQL compilés en mode natif dans SQL Server et Azure SQL Database.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 010318a0-6807-47c3-8ecc-bb7cb60513f0
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dbbdb928a72525586ede01df5bfd7618fa66af51
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237606"
---
# <a name="altering-natively-compiled-t-sql-modules"></a>Altering Natively Compiled T-SQL Modules
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], vous pouvez effectuer des opérations `ALTER` sur les procédures stockées compilées en mode natif et d’autres modules [!INCLUDE[tsql](../../includes/tsql-md.md)] compilés en mode natif comme des fonctions scalaires définies par l’utilisateur et des déclencheurs à l’aide de l’instruction `ALTER`.  
  
Lors de l’exécution de l’instruction `ALTER` sur un module [!INCLUDE[tsql](../../includes/tsql-md.md)] compilé en mode natif, le module est recompilé à l’aide d’une nouvelle définition. Quand la recompilation est en cours, l’ancienne version du module reste disponible pour l’exécution. Une fois la compilation terminée, les exécutions de module sont purgées et la nouvelle version du module est installée. Quand vous modifiez un module [!INCLUDE[tsql](../../includes/tsql-md.md)] compilé en mode natif, vous pouvez modifier les options suivantes.  
  
-   Paramètres  
-   EXECUTE AS  
-   TRANSACTION ISOLATION LEVEL  
-   LANGUAGE  
-   DATEFIRST  
-   DATEFORMAT  
-   DELAYED_DURABILITY  
  
> [!NOTE]  
> Les modules [!INCLUDE[tsql](../../includes/tsql-md.md)] compilés en mode natif ne peuvent pas être convertis en modules non compilés dans ce mode. Les modules T-SQL non compilés en mode natif ne peuvent pas être convertis en modules compilés dans ce mode.  
  
Pour en savoir plus sur la fonctionnalité `ALTER PROCEDURE` et sur sa syntaxe, consultez [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md).  
  
Vous pouvez exécuter [sp_recompile](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md) sur des modules [!INCLUDE[tsql](../../includes/tsql-md.md)] compilés en mode natif, ce qui entraîne leur recompilation lors de la prochaine exécution.  
  
## <a name="example"></a>Exemple  
L’exemple suivant représente la création d’une table optimisée en mémoire (T1) et d’une procédure stockée compilée en mode natif (usp_1) qui sélectionne toutes les colonnes de la table T1. Ensuite, usp_1 est modifiée pour supprimer la clause `EXECUTE AS`, changer `LANGUAGE` et sélectionner une seule colonne (C1) à partir de T1.  
  
```sql  
CREATE TABLE [dbo].[T1] (  
  [c1] [int] NOT NULL,  
  [c2] [float] NOT NULL,  
  CONSTRAINT [PK_T1] PRIMARY KEY NONCLUSTERED ([c1])  
  ) WITH ( MEMORY_OPTIMIZED = ON , DURABILITY = SCHEMA_AND_DATA )  
GO  
  
CREATE PROCEDURE [dbo].[usp_1]  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english'  
)  
   SELECT c1, c2 FROM dbo.T1  
END  
GO  
  
ALTER PROCEDURE [dbo].[usp_1]  
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'Dutch'  
)  
   SELECT c1 FROM dbo.T1  
END  
GO    
```   
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées compilées en mode natif](./a-guide-to-query-processing-for-memory-optimized-tables.md)