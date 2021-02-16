---
title: '[] Caractère générique pour rechercher la correspondance de caractères'
description: Utilisez un caractère générique pour rechercher la correspondance d’un ou plusieurs caractères.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Match
- wildcard
- '[ ]'
- '[_]_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- wildcard characters [SQL Server]
- '[ ] (wildcard - character(s) to match)'
ms.assetid: 57817576-0bf1-49ed-b05d-fac27e8fed7a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2c69fbf1900de88da7faf08e477eb4775187b964
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339804"
---
# <a name="--wildcard---characters-to-match-transact-sql"></a>\[ \] (Caractère générique - Caractères à rechercher) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Recherche la correspondance de chaque caractère, dans la plage ou l’ensemble spécifié entre crochets `[ ]`. Ces caractères génériques peuvent être utilisés dans des comparaisons de chaînes qui impliquent des critères spéciaux tels que `LIKE` et `PATINDEX`.  

 
## <a name="examples"></a>Exemples  
### <a name="a-simple-example"></a>A : Exemple simple   
L’exemple suivant retourne les noms qui commencent par la lettre `m`. `[n-z]` spécifie que la deuxième lettre doit être comprise dans la plage allant de `n` à `z`. Le caractère générique de pourcentage `%` autorise tout caractère ou aucun caractère à partir du troisième caractère. Les bases de données `model` et `msdb` répondent à ces critères. La base de données `master` n’y répond pas et est donc exclue du jeu de résultats.
 
```sql
SELECT name FROM sys.databases
WHERE name LIKE 'm[n-z]%';
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
name
-----
model
msdb
```   
 Vous avez peut-être d’autres bases de données installées qui satisfont ces critères.


### <a name="b-more-complex-example"></a>B : Exemple plus complexe   
 L'exemple suivant utilise l'opérateur [] pour rechercher l'ID et le nom de tous les employés de [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] dont le code postal se compose de quatre chiffres.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.BusinessEntityID, p.FirstName, p.LastName, a.PostalCode  
FROM HumanResources.Employee AS e  
INNER JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.BusinessEntityAddress AS ea ON e.BusinessEntityID = ea.BusinessEntityID  
INNER JOIN Person.Address AS a ON a.AddressID = ea.AddressID  
WHERE a.PostalCode LIKE '[0-9][0-9][0-9][0-9]';  
```  
  
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  
  
```  
EmployeeID      FirstName      LastName      PostalCode  
----------      ---------      ---------     ----------  
290             Lynn           Tsoflias      3000  
```  

### <a name="c-using-a-set-that-combines-ranges-and-single-characters"></a>C : Utilisation d’un jeu qui combine des plages et des caractères uniques

Un jeu de caractères génériques peut inclure à la fois des caractères uniques et des plages. L’exemple suivant utilise l’opérateur [] pour rechercher une chaîne qui commence par un chiffre ou une série de caractères spéciaux.

```sql
SELECT [object_id], OBJECT_NAME(object_id) AS [object_name], name, column_id 
FROM sys.columns 
WHERE name LIKE '[0-9!@#$.,;_]%';
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
object_id     object_name                         name  column_id
---------     -----------                         ----  ---------
615673241     vSalesPersonSalesByFiscalYears      2002  5
615673241     vSalesPersonSalesByFiscalYears      2003  6
615673241     vSalesPersonSalesByFiscalYears      2004  7
1591676718    JunkTable                           _xyz  1
```
  
## <a name="see-also"></a>Voir aussi  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
  [% &#40;Caractère générique - Caractères à rechercher &#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)   
 [&#91; ^&#93; &#40;Caractère générique - Caractères à exclure&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)     
 [\_ &#40;Caractère générique - Recherche d’un seul caractère&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)  
    
  
