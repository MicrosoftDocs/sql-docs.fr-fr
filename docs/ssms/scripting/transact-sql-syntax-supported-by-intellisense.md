---
title: Syntaxe Transact-SQL prise en charge par IntelliSense
description: Découvrez les instructions Transact-SQL et les éléments de syntaxe pris en charge par SQL Server Management Studio IntelliSense dans SQL Server 2019 (15.x).
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- Transact-SQL IntelliSense
- IntelliSense [SQL Server], Transact-SQL syntax
ms.assetid: 194e8f4f-fd7e-4f32-a169-f23531128004
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/16/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 38c0ce9079dddb67e274fccb071aabbb778fb050
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352911"
---
# <a name="transact-sql-syntax-supported-by-intellisense"></a>Syntaxe Transact-SQL prise en charge par IntelliSense
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Cette rubrique décrit les instructions et les éléments syntaxiques [!INCLUDE[tsql](../../includes/tsql-md.md)] pris en charge par IntelliSense dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
## <a name="statements-supported-by-intellisense"></a>Instructions prises en charge par IntelliSense  
 Dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], IntelliSense prend uniquement en charge les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] les plus couramment utilisées. Certaines conditions générales de l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] peuvent nuire au bon fonctionnement d’IntelliSense. Pour plus d’informations, consultez [Résolution des problèmes liés à IntelliSense &#40;SQL Server Management Studio&#41;](./troubleshooting-intellisense.md).  
  
> [!NOTE]  
>  IntelliSense n'est pas disponible pour les objets de base de données chiffrés, tels que les procédures stockées ou les fonctions définies par l'utilisateur chiffrées. L'aide et les infos express sur les paramètres ne sont pas disponibles pour les paramètres de procédures stockées étendues et les types définis par l'utilisateur de l'intégration du CLR.  
  
### <a name="select-statement"></a>Instruction SELECT  
 L’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] fournit la prise en charge IntelliSense pour les éléments syntaxiques suivants de l’instruction SELECT :  
  
:::row:::
    :::column:::
        SELECT
    :::column-end:::
    :::column:::
        WHERE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        FROM
    :::column-end:::
    :::column:::
        ORDER BY
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        HAVING
    :::column-end:::
    :::column:::
        UNION
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        FOR
    :::column-end:::
    :::column:::
        GROUP BY
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        Haut de la page
    :::column-end:::
    :::column:::
        OPTION (conseil)
    :::column-end:::
:::row-end:::

### <a name="additional-transact-sql-statements-that-are-supported"></a>Instructions Transact-SQL supplémentaires prises en charge  
 L’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] fournit également la prise en charge IntelliSense pour les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] répertoriées dans le tableau suivant.  
  
|Instruction Transact-SQL|Syntaxe prise en charge|Exceptions|  
|-----------------------------|----------------------|----------------|  
|[INSERT](../../t-sql/statements/insert-transact-sql.md)|Toute la syntaxe, sauf la clause *execute_statement* .|None|  
|[UPDATE](../../t-sql/queries/update-transact-sql.md)|Toute la syntaxe.|None|  
|[DELETE](../../t-sql/statements/delete-transact-sql.md)|Toute la syntaxe.|None|  
|[DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)|Toute la syntaxe.|None|  
|[SET @local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md)|Toute la syntaxe.|None|  
|[EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)|Exécution des procédures stockées définies par l'utilisateur, des procédures stockées système, des fonctions définies par l'utilisateur et des fonctions système.|None|  
|[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)|Toute la syntaxe.|None|  
|[CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)|Toute la syntaxe.|None|  
|[CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)|Toute la syntaxe.|Il n'existe aucune prise en charge IntelliSense pour la clause EXTERNAL NAME.<br /><br /> Dans la clause AS, IntelliSense prend uniquement en charge les instructions et la syntaxe répertoriées dans cette rubrique.|  
|[ALTER PROCEDURE](../../t-sql/statements/alter-procedure-transact-sql.md)|Toute la syntaxe.|Il n'existe aucune prise en charge IntelliSense pour la clause EXTERNAL NAME.<br /><br /> Dans la clause AS, IntelliSense prend uniquement en charge les instructions et la syntaxe répertoriées dans cette rubrique.|  
|[USE](../../t-sql/language-elements/use-transact-sql.md)|Toute la syntaxe.|None|  
  
## <a name="intellisense-in-supported-statements"></a>IntelliSense dans les instructions prises en charge  
 IntelliSense dans l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] prend en charge les éléments syntaxiques suivants quand ils sont utilisés dans l’une des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] prises en charge :  
  
-   Tous les types de jointure, y compris APPLY.  
  
-   PIVOT et UNPIVOT.  
  
-   Références aux objets de base de données suivants :  
  
    -   Bases de données et schémas  
  
    -   Tables, vues, fonctions table et expressions de table  
  
    -   Colonnes  
  
    -   Procédures et paramètres de procédure  
  
    -   Fonctions scalaires et expressions scalaires  
  
    -   Variables locales  
  
    -   Expressions de table communes  
  
-   Objets de base de données référencés uniquement dans une instruction CREATE ou ALTER du script ou du lot, mais qui n'existent pas dans la base de données parce que le script ou le lot n'a pas encore été exécuté. Ces objets sont les suivants :  
  
    -   Tables et procédures spécifiées dans une instruction CREATE TABLE ou CREATE PROCEDURE du script ou du lot.  
  
    -   Modifications des tables et procédures spécifiées dans une instruction ALTER TABLE ou ALTER PROCEDURE du script ou lot.  
  
    > [!NOTE]  
    >  IntelliSense n'est pas disponible pour les colonnes d'une instruction CREATE VIEW tant que l'instruction CREATE VIEW n'a pas été exécutée.  
  
 IntelliSense n'est pas fourni pour les éléments précédemment répertoriés lorsqu'ils sont utilisés dans d'autres instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . Par exemple, il existe une prise en charge IntelliSense pour les noms de colonne utilisés dans une instruction SELECT, mais pas pour les colonnes utilisées dans l'instruction CREATE FUNCTION.  
  
## <a name="examples"></a>Exemples  
 Dans un script ou lot [!INCLUDE[tsql](../../includes/tsql-md.md)] , IntelliSense dans l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] prend uniquement en charge les instructions et la syntaxe répertoriées dans cette rubrique. Les exemples de code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivants indiquent les instructions et les éléments syntaxiques pris en charge par IntelliSense. Par exemple, dans le lot suivant, IntelliSense est disponible pour l'instruction `SELECT` lorsqu'elle est codée seule, mais pas lorsque `SELECT` est contenue dans une instruction `CREATE FUNCTION`  
  
```  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE Name LIKE N'Road-250%' and Color = N'Red';  
GO  
CREATE FUNCTION Production.ufn_Red250 ()  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT Name  
    FROM AdventureWorks2012.Production.Product  
    WHERE Name LIKE N'Road-250%'  
      AND Color = N'Red'  
);GO  
```  
  
 Ces fonctionnalités s'appliquent également aux jeux d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] de la clause AS d'une instruction CREATE PROCEDURE ou ALTER PROCEDURE.  
  
 Dans un script ou lot [!INCLUDE[tsql](../../includes/tsql-md.md)] , IntelliSense prend en charge les objets spécifiés dans une instruction CREATE ou ALTER ; toutefois, ces objets n'existent pas dans la base de données parce que les instructions n'ont pas été exécutées. Par exemple, vous pouvez entrer le code suivant dans l'éditeur de requête :  
  
```  
USE MyTestDB;  
GO  
CREATE TABLE MyTable  
    (PrimaryKeyCol   INT PRIMARY KEY,  
    FirstNameCol      NVARCHAR(50),  
   LastNameCol       NVARCHAR(50));  
GO  
SELECT   
```  
  
 Après que vous avez tapé `SELECT`, IntelliSense répertorie **PrimaryKeyCol**, **FirstNameCol** et **LastNameCol** comme éléments possibles dans la liste de sélection, même si le script n'a pas été exécuté et que `MyTable` n'existe pas encore dans `MyTestDB`.  
  
