---
description: SET @local_variable (Transact-SQL)
title: SET @local_variable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- SET @local_variable
- variables [SQL Server], assigning
- SET statement, @local_variable
- local variables [SQL Server]
ms.assetid: d410e06e-061b-4c25-9973-b2dc9b60bd85
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0cf40a6b16c1a2750180a5646e17ad317fe3c1bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194288"
---
# <a name="set-local_variable-transact-sql"></a>SET @local_variable (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Affecte la valeur indiquée à la variable locale spécifiée, précédemment créée en utilisant l’instruction DECLARE @*local_variable*.  
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
Syntaxe pour SQL Server et Azure SQL Database :

```syntaxsql
SET   
{ @local_variable  
    [ . { property_name | field_name } ] = { expression | udt_name { . | :: } method_name }  
}  
|  
{ @SQLCLR_local_variable.mutator_method  
}  
|  
{ @local_variable  
    {+= | -= | *= | /= | %= | &= | ^= | |= } expression  
}  
|   
  { @cursor_variable =   
    { @cursor_variable | cursor_name   
    | { CURSOR [ FORWARD_ONLY | SCROLL ]   
        [ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]   
        [ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]   
        [ TYPE_WARNING ]   
    FOR select_statement   
        [ FOR { READ ONLY | UPDATE [ OF column_name [ ,...n ] ] } ]   
      }   
    }  
}   
```  
Syntaxe pour Azure Synapse Analytics et Parallel Data Warehouse :  
```syntaxsql
SET @local_variable { = | += | -= | *= | /= | %= | &= | ^= | |= } expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
**@** _local_variable_  
Nom d’une variable de tout type sauf **cursor**, **text**, **ntext**, **image** ou **table**. Les noms de variables doivent commencer par une arobase ( **@** ). Les noms de variable doivent respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
*property_name*  
Propriété d’un type défini par l’utilisateur.  
  
*field_name*  
Champ public d’un type défini par l’utilisateur.  
  
*udt_name*  
Nom d’un type CLR (Common Language Runtime) défini par l’utilisateur.  
  
`{ . | :: }`  
Définit la méthode d'un type CLR défini par l'utilisateur. Pour une méthode d’instance (non statique), utilisez un point (**.**). Pour une méthode statique, utilisez deux fois un deux-points (**::**). Pour appeler une méthode, une propriété ou un champ de type CLR défini par l'utilisateur, vous devez avoir l'autorisation EXECUTE sur le type.  
  
_method_name_ **(** _argument_ [ **,** ... *n* ] **)**  
Méthode d’un type défini par l’utilisateur qui utilise un ou plusieurs arguments pour modifier l’état d’une instance d’un type. Les méthodes statiques doivent être publiques.  
  
**@** _SQLCLR_local_variable_  
Variable dont le type se trouve dans un assembly. Pour plus d’informations, consultez [Concepts de programmation pour l’intégration du CLR &#40;Common Language Runtime&#41;](../../relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts.md).  
  
*mutator_method*  
Méthode de l’assembly qui peut modifier l’état de l’objet. SQLMethodAttribute.IsMutator est appliqué à cette méthode.  
  
`{ += | -= | *= | /= | %= | &= | ^= | |= }`  
Opérateur d'assignation composé :  
  
 +=              Additionner et assigner  
  
 -=              Soustraire et assigner  
  
 *=               Multiplier et assigner  
  
 /=              Diviser et assigner  
  
 %=              Modulo et assigner  
  
 &=               AND au niveau du bit et assigner  
  
 ^=               XOR au niveau du bit et assigner  
  
 |=              OR au niveau du bit et assigner  
  
*expression*  
Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide.  
  
*cursor_variable*  
Nom d'une variable curseur. Si la variable curseur cible référençait un autre curseur, cette ancienne référence est supprimée.  
  
*cursor_name*  
Nom d’un curseur déclaré à l’aide de l’instruction DECLARE CURSOR.  
  
CURSOR  
Spécifie que l'instruction SET contient une déclaration de curseur.  
  
SCROLL  
Indique que le curseur prend en charge toutes les options d'extraction : FIRST, LAST, NEXT, PRIOR, RELATIVE et ABSOLUTE. Vous ne pouvez pas spécifier SCROLL lorsque vous avez également spécifié FAST_FORWARD.  
  
FORWARD_ONLY  
Spécifie que le curseur gère uniquement l'option FETCH NEXT. Le curseur est récupéré dans une seule direction, de la première à la dernière ligne. Lorsque vous spécifiez FORWARD_ONLY sans le mot clé STATIC, KEYSET ou DYNAMIC, le curseur est implémenté sous la forme DYNAMIC. Si vous ne spécifiez ni FORWARD_ONLY ni SCROLL, FORWARD_ONLY est choisi par défaut, sauf si vous spécifiez les mots clés STATIC, KEYSET ou DYNAMIC. Pour les curseurs STATIC, KEYSET et DYNAMIC, SCROLL est la valeur par défaut.  
  
STATIC  
Définit un curseur qui effectue une copie temporaire des données qu'il doit utiliser. Toutes les requêtes au curseur reçoivent une réponse de cette table temporaire dans tempdb. Par conséquent, les modifications apportées aux tables de base après l’ouverture du curseur ne sont pas répercutées dans les données renvoyées par les extractions effectuées sur le curseur. Et ce curseur ne prend pas en charge les modifications.  
  
KEYSET  
Spécifie que l'appartenance au curseur et l'ordre des lignes sont fixés lors de l'ouverture du curseur. Le groupe de clés qui identifie de manière unique les lignes est intégré à la table keyset dans tempdb. Les modifications apportées aux valeurs non-clé dans les tables de base, par le propriétaire du curseur ou validées par d'autres utilisateurs, sont visibles lorsque le propriétaire du curseur déplace le curseur. Les insertions effectuées par d’autres utilisateurs ne sont pas visibles et les insertions ne peuvent pas être effectuées via un curseur côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Si vous supprimez une ligne, une tentative d’extraction de la ligne retourne une valeur -2 pour @@FETCH_STATUS. Les mises à jour de valeurs clés effectuées hors du curseur sont semblables à la suppression de l'ancienne ligne suivie de l'insertion d'une nouvelle. La ligne avec les nouvelles valeurs n’est pas visible et les tentatives d’extraction avec les anciennes valeurs retournent une valeur @@FETCH_STATUS de -2. Les nouvelles valeurs sont visibles si la mise à jour est effectuée via le curseur en spécifiant la clause WHERE CURRENT OF.  
  
DYNAMIC  
Définit un curseur qui reflète toutes les modifications des lignes dans son jeu de résultats lorsque le propriétaire du curseur déplace le curseur. Les valeurs des données, l'ordre et l'appartenance aux lignes peuvent changer à chaque extraction. Les curseurs dynamiques ne prennent pas en charge les options d’extraction ABSOLUTE et RELATIVE.  
  
FAST_FORWARD  
Spécifie un curseur FORWARD_ONLY, READ_ONLY pour lequel les optimisations sont activées. Vous ne pouvez pas spécifier FAST_FORWARD lorsque SCROLL est spécifié.  
  
READ_ONLY  
Empêche les mises à jour par l'intermédiaire de ce curseur. Le curseur ne peut pas être référencé dans une clause WHERE CURRENT OF d’une instruction UPDATE ou DELETE. Cette option remplace la possibilité par défaut de mise à jour d'un curseur.  
  
SCROLL LOCKS  
Spécifie que la réussite des mises à jour ou des suppressions positionnées effectuées via le curseur est garantie. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrouille les lignes lorsqu’elles sont lues dans le curseur pour garantir leur disponibilité lors des modifications ultérieures. Vous ne pouvez pas spécifier SCROLL_LOCKS lorsque FAST_FORWARD est spécifié.  
  
OPTIMISTIC  
Spécifie que les mises à jour ou les suppressions positionnées effectuées avec le curseur échouent si la ligne a été mise à jour depuis qu’elle a été lue dans le curseur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne verrouille pas les lignes lorsqu’elles sont lues dans le curseur. En revanche, il utilise des comparaisons des valeurs de la colonne timestamp, ou une valeur de somme de contrôle si la table n’a pas de colonne timestamp, pour déterminer si la ligne a été modifiée après avoir été lue dans le curseur. Si la ligne a été modifiée, la mise à jour ou la suppression positionnée que vous avez tentée échoue. Vous ne pouvez pas spécifier OPTIMISTIC lorsque FAST_FORWARD est spécifié.  
  
TYPE_WARNING  
Indique qu'un message d'avertissement est envoyé au client lorsque le curseur est converti implicitement du type demandé vers un autre type.  
  
FOR *select_statement*  
Instruction SELECT standard qui définit le jeu de résultats du curseur. Les mots clés FOR BROWSE et INTO ne sont pas autorisés dans l’instruction *select_statement* d’une déclaration de curseur.  
  
Si vous utilisez DISTINCT, UNION, GROUP BY ou HAVING ou si vous incluez une expression d’agrégation dans *select_list*, le curseur créé est de type STATIC.  
  
Si chaque table sous-jacente ne dispose pas d’un index unique et d’un curseur SCROLL ISO ou si un curseur KEYSET [!INCLUDE[tsql](../../includes/tsql-md.md)] est demandé, le curseur est automatiquement de type STATIC.  
  
Si l’instruction *select_statement* contient une clause ORDER BY dans laquelle les colonnes ne sont pas des identificateurs de ligne uniques, un curseur DYNAMIC est converti en curseur KEYSET ou en curseur STATIC si aucun curseur KEYSET ne peut être ouvert. Ceci se produit également pour un curseur défini en utilisant la syntaxe ISO sans le mot clé STATIC.  
  
READ ONLY  
Empêche les mises à jour par l'intermédiaire de ce curseur. Le curseur ne peut pas être référencé dans une clause WHERE CURRENT OF d’une instruction UPDATE ou DELETE. Cette option remplace la possibilité par défaut de mise à jour d'un curseur. Ce mot clé est désormais représenté par les mots READ et ONLY séparés par un espace, au lieu d'un trait de soulignement (READ_ONLY).  
  
`UPDATE [OF column_name[ ,... n ] ]`  
Définit les colonnes qui peuvent être mises à jour par le curseur. Si OF *column_name* [**,**...*n*] est fourni, seules les colonnes indiquées autorisent les modifications. Lorsqu’aucune liste n’est spécifiée, toutes les colonnes peuvent être mises à jour, sauf si le curseur a été défini en tant que curseur READ_ONLY.  
  
## <a name="remarks"></a>Notes  
Dès qu’une variable est déclarée, elle est initialisée avec la valeur NULL. Utilisez l’instruction SET pour assigner une valeur autre que NULL à une variable déclarée. L'instruction SET qui affecte une valeur à la variable renvoie une seule valeur. Lorsque vous initialisez plusieurs variables, utilisez une instruction SET distincte pour chaque variable locale.  
  
Vous ne pouvez utiliser les variables que dans les expressions, pas dans les noms d’objet ou les mots clés. Pour créer des instructions dynamiques [!INCLUDE[tsql](../../includes/tsql-md.md)], utilisez EXECUTE.  
  
Les règles de syntaxe pour SET **@** _cursor_variable_ n’incluent pas les mots clés LOCAL et GLOBAL. Lorsque vous utilisez la syntaxe SET **@** _cursor_variable_ = CURSOR... est utilisée, le curseur créé est de type GLOBAL ou LOCAL, selon la configuration de l’option de base de données default to local cursor.  
  
Les variables de curseurs sont toujours locales, même lorsqu'elles font référence à un curseur global. Dans ce cas, le curseur comporte à la fois une référence de curseur global et de curseur local. Pour plus d'informations, consultez l'exemple C.  
  
Pour plus d’informations, consultez [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md).  
  
Vous pouvez utiliser l’opérateur d’affectation composée partout où vous avez une affectation avec une expression à droite de l’opérateur, y compris des variables, et un SET dans une instruction UPDATE, SELECT et RECEIVE.  
  
N’utilisez pas de variable dans une instruction INSERT pour concaténer les valeurs (c’est-à-dire, pour calculer des valeurs agrégées). Ceci peut engendrer des résultats de requête inattendus. Car toutes les expressions dans la liste SELECT (y compris les affectations) ne sont pas exécutées une seule fois exactement pour chaque ligne de sortie. Pour plus d’informations, consultez [cet article de la Base de connaissances](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/287515).  
  
## <a name="permissions"></a>Autorisations  
Nécessite l'appartenance au rôle public. Tous les utilisateurs peuvent utiliser SET **@** _local_variable_.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-printing-the-value-of-a-variable-initialized-by-using-set"></a>R. Impression de la valeur d'une variable initialisée avec SET  
L’exemple suivant crée la variable `@myvar`, place une valeur de chaîne dans la variable et imprime la valeur de la variable `@myvar`.  
  
```sql  
DECLARE @myvar CHAR(20);  
SET @myvar = 'This is a test';  
SELECT @myvar;  
GO  
```  
  
### <a name="b-using-a-local-variable-assigned-a-value-by-using-set-in-a-select-statement"></a>B. Utilisation d'une variable locale ayant reçu une valeur avec SET dans une instruction SELECT  
L’exemple suivant crée une variable locale nommée `@state` et l’utilise dans une instruction `SELECT` pour trouver le prénom et le nom de tous les employés qui résident dans l’État de l’`Oregon`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @state CHAR(25);  
SET @state = N'Oregon';  
SELECT RTRIM(FirstName) + ' ' + RTRIM(LastName) AS Name, City  
FROM HumanResources.vEmployee  
WHERE StateProvinceName = @state;  
```  
  
### <a name="c-using-a-compound-assignment-for-a-local-variable"></a>C. Utilisation d'une assignation composée pour une variable locale  
Les deux exemples suivants produisent le même résultat. Ils créent une variable locale nommée `@NewBalance`, la multiplie par 10 et affiche la nouvelle valeur de la variable locale dans une instruction `SELECT`. Le deuxième exemple utilise un opérateur d'assignation composée.  
  
```sql  
/* Example one */  
DECLARE  @NewBalance  INT ;  
SET  @NewBalance  =  10;  
SET  @NewBalance  =  @NewBalance  *  10;  
SELECT  @NewBalance;  
  
/* Example Two */  
DECLARE @NewBalance INT = 10;  
SET @NewBalance *= 10;  
SELECT @NewBalance;  
```  
  
### <a name="d-using-set-with-a-global-cursor"></a>D. Utilisation de SET avec un curseur global  
L'exemple suivant crée une variable locale et affecte le nom de curseur global à la variable curseur.  
  
```sql  
DECLARE my_cursor CURSOR GLOBAL   
FOR SELECT * FROM Purchasing.ShipMethod  
DECLARE @my_variable CURSOR ;  
SET @my_variable = my_cursor ;   
--There is a GLOBAL cursor declared(my_cursor) and a LOCAL variable  
--(@my_variable) set to the my_cursor cursor.  
DEALLOCATE my_cursor;   
--There is now only a LOCAL variable reference  
--(@my_variable) to the my_cursor cursor.  
```  
  
### <a name="e-defining-a-cursor-by-using-set"></a>E. Définition d'un curseur à l'aide de SET  
L'exemple suivant utilise l'instruction `SET` pour définir un curseur.  
  
```sql  
DECLARE @CursorVar CURSOR;  
  
SET @CursorVar = CURSOR SCROLL DYNAMIC  
FOR  
SELECT LastName, FirstName  
FROM AdventureWorks2012.HumanResources.vEmployee  
WHERE LastName like 'B%';  
  
OPEN @CursorVar;  
  
FETCH NEXT FROM @CursorVar;  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    FETCH NEXT FROM @CursorVar  
END;  
  
CLOSE @CursorVar;  
DEALLOCATE @CursorVar;  
```  
  
### <a name="f-assigning-a-value-from-a-query"></a>F. Attribution d'une valeur depuis une requête  
L'exemple suivant utilise une requête pour affecter une valeur à une variable.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @rows INT;  
SET @rows = (SELECT COUNT(*) FROM Sales.Customer);  
SELECT @rows;  
```  
  
### <a name="g-assigning-a-value-to-a-user-defined-type-variable-by-modifying-a-property-of-the-type"></a>G. Attribution d'une valeur à une variable de type défini par l'utilisateur en modifiant une propriété du type  
L'exemple suivant définit une valeur pour le type `Point` défini par l'utilisateur en modifiant la valeur de la propriété `X` du type.  
  
```sql  
DECLARE @p Point;  
SET @p.X = @p.X + 1.1;  
SELECT @p;  
GO  
```  
  
### <a name="h-assigning-a-value-to-a-user-defined-type-variable-by-invoking-a-method-of-the-type"></a>H. Attribution d'une valeur à une variable de type défini par l'utilisateur en appelant une méthode du type  
L’exemple suivant définit une valeur pour le type **point** défini par l’utilisateur en appelant la méthode `SetXY` du type.  
  
```sql  
DECLARE @p Point;  
SET @p=point.SetXY(23.5, 23.5);  
```  
  
### <a name="i-creating-a-variable-for-a-clr-type-and-calling-a-mutator-method"></a>I. Création d'une variable pour un type CLR et appel d'une méthode mutateur  
L'exemple suivant crée une variable pour le type `Point`, puis exécute une méthode mutateur dans `Point`.  
  
```sql  
CREATE ASSEMBLY mytest FROM 'c:\test.dll' WITH PERMISSION_SET = SAFE  
CREATE TYPE Point EXTERNAL NAME mytest.Point  
GO  
DECLARE @p Point = CONVERT(Point, '')  
SET @p.SetXY(22, 23);  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="j-printing-the-value-of-a-variable-initialized-by-using-set"></a>J. Impression de la valeur d'une variable initialisée avec SET  
L’exemple suivant crée la variable `@myvar`, place une valeur de chaîne dans la variable et imprime la valeur de la variable `@myvar`.  
  
```sql  
DECLARE @myvar CHAR(20);  
SET @myvar = 'This is a test';  
SELECT TOP 1 @myvar FROM sys.databases;
```  
  
### <a name="k-using-a-local-variable-assigned-a-value-by-using-set-in-a-select-statement"></a>K. Utilisation d'une variable locale ayant reçu une valeur avec SET dans une instruction SELECT  
L’exemple suivant crée une variable locale nommée `@dept` et l’utilise dans une instruction `SELECT` pour rechercher le nom et le prénom de tous les employés qui travaillent dans le service `Marketing`.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @dept CHAR(25);  
SET @dept = N'Marketing';  
SELECT RTRIM(FirstName) + ' ' + RTRIM(LastName) AS Name  
FROM DimEmployee   
WHERE DepartmentName = @dept;  
```  
  
### <a name="l-using-a-compound-assignment-for-a-local-variable"></a>L. Utilisation d'une assignation composée pour une variable locale  
Les deux exemples suivants produisent le même résultat. Ils créent une variable locale nommée `@NewBalance`, la multiplient par `10` et affichent la nouvelle valeur de la variable locale dans une instruction `SELECT`. Le deuxième exemple utilise un opérateur d'assignation composée.  
  
```sql  
/* Example one */  
DECLARE  @NewBalance INT;  
SET  @NewBalance  =  10;  
SET  @NewBalance  =  @NewBalance  *  10;  
SELECT TOP 1 @NewBalance FROM sys.tables;  
  
/* Example Two */  
DECLARE @NewBalance INT = 10;  
SET @NewBalance *= 10;  
SELECT TOP 1 @NewBalance FROM sys.tables;  
```  
  
### <a name="m-assigning-a-value-from-a-query"></a>M. Attribution d'une valeur depuis une requête  
L'exemple suivant utilise une requête pour affecter une valeur à une variable.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @rows INT;  
SET @rows = (SELECT COUNT(*) FROM dbo.DimCustomer);  
SELECT TOP 1 @rows FROM sys.tables;  
```  
  
## <a name="see-also"></a>Voir aussi  
[Opérateurs composés &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
[EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
[SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
[Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

