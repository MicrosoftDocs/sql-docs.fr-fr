---
description: sp_prepare (Transact SQL)
title: sp_prepare (Transact SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/28/2018
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepare_TSQL
- sp_cursor_prepare
dev_langs:
- TSQL
helpviewer_keywords:
- sp_prepare
ms.assetid: f328c9eb-8211-4863-bafa-347e1bf7bb3f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fe4da6ff8d3cdcbeb7ba809bf198c44373b99c9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196030"
---
# <a name="sp_prepare-transact-sql"></a>sp_prepare (Transact SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

Prépare une instruction paramétrable [!INCLUDE[tsql](../../includes/tsql-md.md)] et retourne un *handle* d’instruction pour l’exécution.  `sp_prepare` est appelé en spécifiant ID = 11 dans un paquet TDS (Tabular Data Stream).  
  
 ![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
sp_prepare handle OUTPUT, params, stmt, options  
```  
  
## <a name="arguments"></a>Arguments  
 *traitée*  
 Est un identificateur de *handle préparé* généré par SQL Server. *handle* est un paramètre obligatoire avec une valeur de retour **int** .  
  
 *params*  
 Identifie des instructions paramétrables. La définition *params* des variables est substituée aux marqueurs de paramètres dans l’instruction. *params* est un paramètre obligatoire qui requiert une valeur d’entrée **ntext**, **nchar** ou **nvarchar** . Entrez une valeur NULL si l'instruction n'est pas paramétrable.  
  
 *Insert*  
 Définit le jeu de résultats de curseur. Le paramètre *stmt* est requis et appelle une valeur d’entrée **ntext**, **nchar** ou **nvarchar** .  
  
 *options*  
 Paramètre optionnel qui retourne une description des colonnes du jeu de résultats du curseur. les *options* requièrent la valeur d’entrée int suivante :  
  
|Valeur|Description|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
## <a name="examples"></a>Exemples  
R. L'exemple suivant prépare et exécute une instruction simple.  
  
```sql  
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@P1 NVARCHAR(128), @P2 NVARCHAR(100)',  
    N'SELECT database_id, name FROM sys.databases WHERE name=@P1 AND state_desc = @P2';  
EXEC sp_execute @P1, N'tempdb', N'ONLINE';  
EXEC sp_unprepare @P1;  
```

B. L’exemple suivant prépare une instruction dans la base de données AdventureWorks2016, puis l’exécute par la suite à l’aide du descripteur.

```sql
-- Prepare query
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@Param INT',  
    N'SELECT *
FROM Sales.SalesOrderDetail AS sod
INNER JOIN Production.Product AS p ON sod.ProductID = p.ProductID
WHERE SalesOrderID = @Param
ORDER BY Style DESC;';  

-- Return handle for calling application
SELECT @P1;
GO
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
-----------
1

(1 row affected)
```

L’application exécute ensuite la requête à deux reprises à l’aide de la valeur de handle 1, avant d’ignorer le plan préparé.

```sql
EXEC sp_execute 1, 49879;  
GO

EXEC sp_execute 1, 48766;
GO

EXEC sp_unprepare 1; 
GO
```
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  

