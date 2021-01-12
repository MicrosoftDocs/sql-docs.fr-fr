---
description: FETCH (Transact-SQL)
title: FETCH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FETCH
- FETCH_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FETCH statement
- cursors [SQL Server], fetching
- Transact-SQL cursors, fetching and scrolling
- retrieving rows
- fetching [SQL Server]
- SCROLL option
- row fetching [SQL Server]
ms.assetid: 5d68dac2-f91b-4342-bb4e-209ee132665f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 43ce2fb10cf07ec766d29b4b3e96e63404c0fbb1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100355"
---
# <a name="fetch-transact-sql"></a>FETCH (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Récupère une ligne spécifique d'un curseur côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
FETCH   
          [ [ NEXT | PRIOR | FIRST | LAST   
                    | ABSOLUTE { n | @nvar }   
                    | RELATIVE { n | @nvar }   
               ]   
               FROM   
          ]   
{ { [ GLOBAL ] cursor_name } | @cursor_variable_name }   
[ INTO @variable_name [ ,...n ] ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 NEXT  
 Renvoie la ligne de résultats immédiatement après la ligne courante et incrémente cette dernière sur la ligne renvoyée. Si `FETCH NEXT` est la première extraction effectuée sur un curseur, elle retourne la première ligne du jeu de résultats. `NEXT` est l’option d’extraction du curseur par défaut.  
  
 PRIOR  
 Renvoie la ligne de résultats immédiatement avant la ligne courante, et décrémente cette dernière sur la ligne renvoyée. Si `FETCH PRIOR` est la première extraction effectuée sur un curseur, aucune ligne n’est retournée et le curseur reste placé avant la première ligne.  
  
 FIRST  
 Renvoie la première ligne dans le curseur et la transforme en ligne courante.  
  
 LAST  
 Renvoie la dernière ligne dans le curseur et la transforme en ligne courante.  
  
 ABSOLUTE { *n*| \@*nvar*}  
 Si *n* ou \@*nvar* est un nombre positif, retourne la ligne située à *n* lignes à l’avant du curseur, et fait de la ligne retournée la nouvelle ligne actuelle. Si *n* ou \@*nvar* est un nombre négatif, retourne la ligne située à *n* lignes avant la fin du curseur, et fait de la ligne retournée la nouvelle ligne actuelle. Si *n* ou \@*nvar* est égal à 0, aucune ligne n’est retournée. *n* doit être une constante entière et \@*nvar* doit être de type **smallint**, **tinyint** ou **int**.  
  
 RELATIVE { *n*| \@*nvar*}  
 Si *n* ou \@*nvar* est un nombre positif, retourne la ligne située à *n* lignes au-delà de la ligne actuelle, et fait de la ligne retournée la nouvelle ligne actuelle. Si *n* ou \@*nvar* est un nombre négatif, retourne la ligne située à *n* lignes avant la ligne actuelle, et fait de la ligne retournée la nouvelle ligne actuelle. Si *n* ou \@*nvar* est égal à 0, retourne la ligne actuelle. Si `FETCH RELATIVE` est spécifié avec *n* ou \@*nvar* défini sur un nombre négatif ou égal à 0 lors de la première extraction effectuée sur un curseur, aucune ligne n’est retournée. *n* doit être une constante entière et \@*nvar* doit être de type **smallint**, **tinyint** ou **int**.  
  
 GLOBAL  
 Indique que *cursor_name* fait référence à un curseur global.  
  
 *cursor_name*  
 Nom du curseur ouvert grâce auquel s'effectue l'extraction. Si un curseur global et un curseur local ont tous les deux le même nom *cursor_name*, *cursor_name* fait référence au curseur global si GLOBAL est spécifié, et au curseur local dans le cas contraire.  
  
 \@*cursor_variable_name*  
 Nom d'une variable de curseur qui fait référence au curseur ouvert à partir duquel l'extraction doit être effectuée.  
  
 INTO \@*variable_name*[ ,...*n*]  
 Permet aux données issues des colonnes d'une extraction d'être placées dans des variables locales. Chaque variable de la liste (de gauche à droite) est associée à la colonne correspondante dans le jeu de résultats du curseur. Le type de données de chaque variable doit correspondre ou être une conversion implicite du type de données de la colonne du jeu de résultats correspondante. Le nombre de variables doit correspondre au nombre de colonnes dans la liste de sélection du curseur.  
  
## <a name="remarks"></a>Remarques  
 Si l’option `SCROLL` n’est pas spécifiée dans une instruction `DECLARE CURSOR` de style ISO, `NEXT` est la seule option `FETCH` prise en charge. Si `SCROLL` est spécifié dans un `DECLARE CURSOR` de style ISO, toutes les options `FETCH` sont prises en charge.  
  
 Lorsque les extensions de curseur [!INCLUDE[tsql](../../includes/tsql-md.md)] DECLARE sont utilisées, les règles suivantes sont respectées :  
  
-   Si `FORWARD_ONLY` ou `FAST_FORWARD` est spécifié, `NEXT` est la seule option `FETCH` prise en charge.  
  
-   Si `DYNAMIC`, `FORWARD_ONLY` ou `FAST_FORWARD` ne sont pas spécifiés, et qu’une des options `KEYSET`, `STATIC` ou `SCROLL` sont spécifiées, toutes les options `FETCH` sont prises en charge.  
  
-   Les curseurs `DYNAMIC SCROLL` prennent en charge toutes les options `FETCH`, sauf `ABSOLUTE`.  
  
 La fonction `@@FETCH_STATUS` signale l’état de la dernière instruction `FETCH`. Les mêmes informations sont enregistrées dans la colonne fetch_status du curseur renvoyé par sp_describe_cursor. Ces informations d’état doivent être utilisées pour déterminer la validité des données retournées par une instruction `FETCH` avant de tenter une opération sur ces données. Pour plus d’informations, consultez [@@FETCH_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/fetch-status-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, tout utilisateur valide a l’autorisation d’utiliser l’instruction `FETCH`.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-fetch-in-a-simple-cursor"></a>R. Utilisation de FETCH dans un curseur simple  
 L'exemple suivant déclare un curseur simple pour les lignes de la table `Person.Person` dont le nom commence par `B`, et il utilise `FETCH NEXT` pour parcourir les lignes. L'instruction `FETCH` renvoie la valeur de la colonne spécifiée dans `DECLARE CURSOR` comme un jeu de résultats sur une seule ligne.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE contact_cursor CURSOR FOR  
SELECT LastName FROM Person.Person  
WHERE LastName LIKE 'B%'  
ORDER BY LastName;  
  
OPEN contact_cursor;  
  
-- Perform the first fetch.  
FETCH NEXT FROM contact_cursor;  
  
-- Check @@FETCH_STATUS to see if there are any more rows to fetch.  
WHILE @@FETCH_STATUS = 0  
BEGIN  
   -- This is executed as long as the previous fetch succeeds.  
   FETCH NEXT FROM contact_cursor;  
END  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
### <a name="b-using-fetch-to-store-values-in-variables"></a>B. Utilisation de FETCH pour stocker des valeurs dans des variables  
 L'exemple suivant ressemble au précédent, excepté que le résultat des instructions `FETCH` est stocké dans des variables locales au lieu d'être directement renvoyé au client. L'instruction `PRINT` combine les variables dans une chaîne unique et les renvoie au client.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Declare the variables to store the values returned by FETCH.  
DECLARE @LastName VARCHAR(50), @FirstName VARCHAR(50);  
  
DECLARE contact_cursor CURSOR FOR  
SELECT LastName, FirstName FROM Person.Person  
WHERE LastName LIKE 'B%'  
ORDER BY LastName, FirstName;  
  
OPEN contact_cursor;  
  
-- Perform the first fetch and store the values in variables.  
-- Note: The variables are in the same order as the columns  
-- in the SELECT statement.   
  
FETCH NEXT FROM contact_cursor  
INTO @LastName, @FirstName;  
  
-- Check @@FETCH_STATUS to see if there are any more rows to fetch.  
WHILE @@FETCH_STATUS = 0  
BEGIN  
  
   -- Concatenate and display the current values in the variables.  
   PRINT 'Contact Name: ' + @FirstName + ' ' +  @LastName  
  
   -- This is executed as long as the previous fetch succeeds.  
   FETCH NEXT FROM contact_cursor  
   INTO @LastName, @FirstName;  
END  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
### <a name="c-declaring-a-scroll-cursor-and-using-the-other-fetch-options"></a>C. Déclaration d'un curseur SCROLL et utilisation des autres options FETCH  
 L'exemple suivant crée un curseur `SCROLL` pour autoriser des capacités de défilement complètes à travers les options `LAST`, `PRIOR`, `RELATIVE` et `ABSOLUTE`.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Execute the SELECT statement alone to show the   
-- full result set that is used by the cursor.  
SELECT LastName, FirstName FROM Person.Person  
ORDER BY LastName, FirstName;  
  
-- Declare the cursor.  
DECLARE contact_cursor SCROLL CURSOR FOR  
SELECT LastName, FirstName FROM Person.Person  
ORDER BY LastName, FirstName;  
  
OPEN contact_cursor;  
  
-- Fetch the last row in the cursor.  
FETCH LAST FROM contact_cursor;  
  
-- Fetch the row immediately prior to the current row in the cursor.  
FETCH PRIOR FROM contact_cursor;  
  
-- Fetch the second row in the cursor.  
FETCH ABSOLUTE 2 FROM contact_cursor;  
  
-- Fetch the row that is three rows after the current row.  
FETCH RELATIVE 3 FROM contact_cursor;  
  
-- Fetch the row that is two rows prior to the current row.  
FETCH RELATIVE -2 FROM contact_cursor;  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [OPEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/open-transact-sql.md)  
  
  
