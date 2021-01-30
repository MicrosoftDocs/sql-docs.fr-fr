---
description: sp_executesql (Transact-SQL)
title: sp_executesql (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_executesql
- sp_executesql_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_executesql
- dynamic SQL
ms.assetid: a8d68d72-0f4d-4ecb-ae86-1235b962f646
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 333bc510861d769a8b9dbb22272918e65982ff2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188019"
---
# <a name="sp_executesql-transact-sql"></a>sp_executesql (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Exécute un lot ou une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)], réutilisable plusieurs fois ou créé dynamiquement. L'instruction ou le traitement d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] peut contenir des paramètres incorporés.  
  
> [!IMPORTANT]  
>  Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] compilées à l'exécution peuvent exposer les applications à des attaques malveillantes.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_executesql [ @stmt = ] statement  
[   
  { , [ @params = ] N'@parameter_name data_type [ OUT | OUTPUT ][ ,...n ]' }   
     { , [ @param1 = ] 'value1' [ ,...n ] }  
]  
```  
  
## <a name="arguments"></a>Arguments  
 [ \@ stmt =] *instruction*  
 Chaîne Unicode qui contient une [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction ou un lot. \@stmt doit être une constante Unicode ou une variable Unicode. L'utilisation d'expressions Unicode plus complexes (comme la concaténation de deux chaînes avec l'opérateur +) n'est pas autorisée. L'utilisation de constantes de caractères n'est pas autorisée. Si une constante Unicode est spécifiée, elle doit être précédée de **N**. Par exemple, la constante Unicode **N’sp_who'** est valide, mais la constante caractère **'sp_who'** n’est pas. La taille de la chaîne n'est limitée que par la quantité de mémoire disponible sur le serveur de base de données. Sur les serveurs 64 bits, la taille de la chaîne est limitée à 2 Go, la taille maximale de **nvarchar (max)**.  
  
> [!NOTE]  
>  \@stmt peut contenir des paramètres ayant la même forme qu’un nom de variable, par exemple : `N'SELECT * FROM HumanResources.Employee WHERE EmployeeID = @IDParameter'`  
  
 Chaque paramètre inclus dans \@ stmt doit avoir une entrée correspondante dans la \@ liste de définitions de paramètres params et dans la liste des valeurs de paramètre.  
  
 [ \@ params =] N' \@ *parameter_name* *data_type* [,... *n* ] '  
 Est une chaîne qui contient les définitions de tous les paramètres qui ont été incorporés dans \@ stmt. La chaîne doit être une constante Unicode ou une variable Unicode. Chaque définition de paramètre se compose d'un nom de paramètre et d'un type de données. *n* est un espace réservé qui indique des définitions de paramètres supplémentaires. Chaque paramètre spécifié dans \@ stmt doit être défini dans \@ params. Si l' [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction ou le lot dans \@ stmt ne contient pas de paramètres, \@ params n’est pas requis. La valeur par défaut de ce paramètre est NULL.  
  
 [ \@ param1 =] '*valeur1*'  
 Valeur du premier paramètre qui est défini dans la chaîne de paramètres. Cette valeur peut être une constante ou une variable Unicode. Une valeur de paramètre doit être fournie pour chaque paramètre inclus dans \@ stmt. Les valeurs ne sont pas obligatoires lorsque l' [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction ou le lot dans \@ stmt n’a pas de paramètres.  
  
 [ OUT | OUTPUT ]  
 Indique que le paramètre est un paramètre de sortie. les paramètres **Text**, **ntext** et **image** peuvent être utilisés comme paramètres de sortie, sauf si la procédure est une procédure Common Language Runtime (CLR). Un paramètre de sortie qui utilise le mot clé OUTPUT peut être un espace réservé de curseur, sauf si la procédure est une procédure CLR (Common Language Runtime).  
  
 *n*  
 Représente un espace réservé destiné aux valeurs de paramètres supplémentaires. Ces valeurs doivent être des constantes ou des variables. Leur degré de complexité ne doit pas dépasser celui d'expressions telles que les fonctions ou expressions créées à l'aide d'opérateurs.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou autre que zéro (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne les jeux de résultats de toutes les instructions SQL de la chaîne SQL.  
  
## <a name="remarks"></a>Notes  
 sp_executesql paramètres doivent être entrés dans l’ordre spécifique, comme décrit dans la section « syntaxe », plus haut dans cette rubrique. Si les paramètres sont entrés dans le désordre, un message d'erreur se produira.  
  
 La procédure sp_executesql a le même comportement vis-à-vis des traitements d'instructions, de l'étendue des noms et du contexte de base de données que l'instruction EXECUTE. L' [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction ou le lot dans le \@ paramètre sp_executesql stmt n’est pas compilé tant que l’instruction sp_executesql n’est pas exécutée. Le contenu de \@ stmt est ensuite compilé et exécuté en tant que plan d’exécution distinct du plan d’exécution du lot qui a appelé sp_executesql. Le traitement sp_executesql ne peut pas faire référence à des variables déclarées dans le traitement qui a appelé sp_executesql. Les curseurs ou les variables locaux du traitement sp_executesql ne sont pas visibles pour le traitement qui appelle sp_executesql. Les modifications apportées au contexte de base de données ne durent que jusqu'à la fin de l'exécution de l'instruction sp_executesql.  
  
 La procédure sp_executesql peut être utilisée à la place de procédures stockées afin d'exécuter une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] plusieurs fois lorsque la modification des valeurs de paramètres de l'instruction constitue l'unique changement. L'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] même demeurant constante, seules les valeurs de paramètre changent. Par conséquent, l'optimiseur de requête de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut réutiliser le plan d'exécution généré pour la première exécution.  
  
> [!NOTE]  
>  Pour améliorer les performances, utilisez des noms d'objets complets dans la chaîne d'instruction.  
  
 La procédure sp_executesql prend en charge la définition des valeurs de paramètres en dehors de la chaîne [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
```sql  
DECLARE @IntVariable INT;  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
  
/* Build the SQL string one time.*/  
SET @SQLString =  
     N'SELECT BusinessEntityID, NationalIDNumber, JobTitle, LoginID  
       FROM AdventureWorks2012.HumanResources.Employee   
       WHERE BusinessEntityID = @BusinessEntityID';  
SET @ParmDefinition = N'@BusinessEntityID tinyint';  
/* Execute the string with the first parameter value. */  
SET @IntVariable = 197;  
EXECUTE sp_executesql @SQLString, @ParmDefinition,  
                      @BusinessEntityID = @IntVariable;  
/* Execute the same string with the second parameter value. */  
SET @IntVariable = 109;  
EXECUTE sp_executesql @SQLString, @ParmDefinition,  
                      @BusinessEntityID = @IntVariable;  
```  
  
 Les paramètres de sortie peuvent également être utilisés avec sp_executesql. L'exemple suivant récupère un poste dans la table `AdventureWorks2012.HumanResources.Employee` et le retourne dans le paramètre de sortie `@max_title`.  
  
```sql  
DECLARE @IntVariable INT;  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
DECLARE @max_title VARCHAR(30);  
  
SET @IntVariable = 197;  
SET @SQLString = N'SELECT @max_titleOUT = max(JobTitle)   
   FROM AdventureWorks2012.HumanResources.Employee  
   WHERE BusinessEntityID = @level';  
SET @ParmDefinition = N'@level TINYINT, @max_titleOUT VARCHAR(30) OUTPUT';  
  
EXECUTE sp_executesql @SQLString, @ParmDefinition, @level = @IntVariable, @max_titleOUT=@max_title OUTPUT;  
SELECT @max_title;  
```  
  
 La possibilité de substitution de paramètres dans sp_executesql présente les avantages suivants lors de l'utilisation de l'instruction EXECUTE pour exécuter une chaîne :  
  
-   Le texte de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] contenu dans la chaîne sp_executesql ne changeant pas entre les différentes exécutions, il est probable que l'optimiseur de requête calque dans ce cas l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] de la deuxième exécution sur le plan d'exécution généré pour la première exécution. Cela évite donc à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de devoir compiler la deuxième instruction.  
  
-   La chaîne [!INCLUDE[tsql](../../includes/tsql-md.md)] est créée une seule fois.  
  
-   Le paramètre de type entier est spécifié dans son format d'origine. La conversion en Unicode n'est pas nécessaire.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-executing-a-simple-select-statement"></a>R. Exécution d'une instruction SELECT simple  
 Cet exemple illustre la création et l'exécution d'une instruction `SELECT` simple contenant un paramètre incorporé appelé `@level`.  
  
```sql  
EXECUTE sp_executesql   
          N'SELECT * FROM AdventureWorks2012.HumanResources.Employee   
          WHERE BusinessEntityID = @level',  
          N'@level TINYINT',  
          @level = 109;  
```  
  
### <a name="b-executing-a-dynamically-built-string"></a>B. Exécution d'une chaîne créée dynamiquement  
 L'exemple suivant illustre l'utilisation de `sp_executesql` pour exécuter une chaîne créée dynamiquement. La procédure stockée proposée sert à l'insertion de données dans un ensemble de tables utilisées pour partitionner les données commerciales d'une année. Il existe une table par mois de l'année, d'après le format suivant :  
  
```sql  
CREATE TABLE May1998Sales  
    (OrderID INT PRIMARY KEY,  
    CustomerID INT NOT NULL,  
    OrderDate  DATETIME NULL  
        CHECK (DATEPART(yy, OrderDate) = 1998),  
    OrderMonth INT  
        CHECK (OrderMonth = 5),  
    DeliveryDate DATETIME NULL,  
        CHECK (DATEPART(mm, OrderDate) = OrderMonth)  
    )  
```  
  
 Cet exemple de procédure stockée permet de créer et d'exécuter dynamiquement une instruction `INSERT` destinée à insérer les nouvelles commandes dans la table appropriée. L'exemple utilise la date de commande pour générer le nom de la table devant contenir les données, puis incorpore ce nom dans une instruction `INSERT`.  
  
> [!NOTE]  
>  Il s'agit d'un exemple simple illustrant l'utilisation de sp_executesql. L'exemple ne prévoit pas de détection d'erreur et n'inclut aucun contrôle des règles d'entreprise, telles que la recherche de numéros de commande en double dans les différentes tables.  
  
```sql  
CREATE PROCEDURE InsertSales @PrmOrderID INT, @PrmCustomerID INT,  
                 @PrmOrderDate DATETIME, @PrmDeliveryDate DATETIME  
AS  
DECLARE @InsertString NVARCHAR(500)  
DECLARE @OrderMonth INT  
  
-- Build the INSERT statement.  
SET @InsertString = 'INSERT INTO ' +  
       /* Build the name of the table. */  
       SUBSTRING( DATENAME(mm, @PrmOrderDate), 1, 3) +  
       CAST(DATEPART(yy, @PrmOrderDate) AS CHAR(4) ) +  
       'Sales' +  
       /* Build a VALUES clause. */  
       ' VALUES (@InsOrderID, @InsCustID, @InsOrdDate,' +  
       ' @InsOrdMonth, @InsDelDate)'  
  
/* Set the value to use for the order month because  
   functions are not allowed in the sp_executesql parameter  
   list. */  
SET @OrderMonth = DATEPART(mm, @PrmOrderDate)  
  
EXEC sp_executesql @InsertString,  
     N'@InsOrderID INT, @InsCustID INT, @InsOrdDate DATETIME,  
       @InsOrdMonth INT, @InsDelDate DATETIME',  
     @PrmOrderID, @PrmCustomerID, @PrmOrderDate,  
     @OrderMonth, @PrmDeliveryDate  
  
GO  
```  
  
 Pour cette procédure, l'utilisation de sp_executesql est plus efficace que l'utilisation d'EXECUTE pour exécuter une chaîne. Si vous utilisez sp_executesql, seules 12 versions de la chaîne INSERT sont générées (une par table mensuelle). Avec EXECUTE, chaque chaîne INSERT est unique car les valeurs de paramètres diffèrent. Bien que ces deux méthodes génèrent le même nombre de traitements d'instructions, la similitude des chaînes INSERT générées par sp_executesql renforce la probabilité de réutilisation des plans d'exécution par l'optimiseur de requête.  
  
### <a name="c-using-the-output-parameter"></a>C. Utilisation du paramètre OUTPUT  
 L’exemple suivant utilise un `OUTPUT` paramètre pour stocker le jeu de résultats généré par l' `SELECT` instruction dans le `@SQLString` paramètre. Deux `SELECT` instructions sont ensuite exécutées qui utilisent la valeur du `OUTPUT` paramètre.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
DECLARE @SalesOrderNumber NVARCHAR(25);  
DECLARE @IntVariable INT;  
SET @SQLString = N'SELECT @SalesOrderOUT = MAX(SalesOrderNumber)  
    FROM Sales.SalesOrderHeader  
    WHERE CustomerID = @CustomerID';  
SET @ParmDefinition = N'@CustomerID INT,  
    @SalesOrderOUT NVARCHAR(25) OUTPUT';  
SET @IntVariable = 22276;  
EXECUTE sp_executesql  
    @SQLString  
    ,@ParmDefinition  
    ,@CustomerID = @IntVariable  
    ,@SalesOrderOUT = @SalesOrderNumber OUTPUT;  
-- This SELECT statement returns the value of the OUTPUT parameter.  
SELECT @SalesOrderNumber;  
-- This SELECT statement uses the value of the OUTPUT parameter in  
-- the WHERE clause.  
SELECT OrderDate, TotalDue  
FROM Sales.SalesOrderHeader  
WHERE SalesOrderNumber = @SalesOrderNumber;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-executing-a-simple-select-statement"></a>D. Exécution d'une instruction SELECT simple  
 Cet exemple illustre la création et l'exécution d'une instruction `SELECT` simple contenant un paramètre incorporé appelé `@level`.  
  
```sql  
-- Uses AdventureWorks  
  
EXECUTE sp_executesql   
          N'SELECT * FROM AdventureWorksPDW2012.dbo.DimEmployee   
          WHERE EmployeeKey = @level',  
          N'@level TINYINT',  
          @level = 109;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
