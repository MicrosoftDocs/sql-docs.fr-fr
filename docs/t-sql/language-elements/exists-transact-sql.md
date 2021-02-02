---
description: EXISTS (Transact-SQL)
title: EXISTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EXISTS_TSQL
- EXISTS
dev_langs:
- TSQL
helpviewer_keywords:
- existence testing [SQL Server]
- testing existence
- EXISTS keyword
- subqueries [SQL Server], EXISTS keyword
- queries [SQL Server], comparing
- comparing queries
- NOT EXISTS keyword
- row existence testing [SQL Server]
ms.assetid: b6510a65-ac38-4296-a3d5-640db0c27631
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1fd4ba349ee46946e2dc8f707fb076c1b085e0c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99116340"
---
# <a name="exists-transact-sql"></a>EXISTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Spécifie une sous-requête pour déterminer l'existence ou non de lignes.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
EXISTS ( subquery )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *subquery*  
 Instruction SELECT restreinte. Le mot clé INTO n'est pas autorisé. Pour plus d’informations, consultez les informations relatives aux sous-requêtes dans [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
## <a name="result-types"></a>Types des résultats  
 **Booléen**  
  
## <a name="result-values"></a>Valeurs de résultat  
 Retourne TRUE si une sous-requête contient des lignes.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-null-in-a-subquery-to-still-return-a-result-set"></a>R. Utilisation de NULL dans une sous-requête pour retourner un jeu de résultats  
 Cet exemple retourne un jeu de résultats avec la valeur `NULL` spécifiée dans la sous-requête mais continue à donner TRUE du fait de la clause `EXISTS`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DepartmentID, Name   
FROM HumanResources.Department   
WHERE EXISTS (SELECT NULL)  
ORDER BY Name ASC ;  
```  
  
### <a name="b-comparing-queries-by-using-exists-and-in"></a>B. Comparaison de requêtes à l'aide de EXISTS et de IN  
 Cet exemple compare deux requêtes sémantiquement équivalentes. La première requête utilise `EXISTS` et la seconde `IN`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT a.FirstName, a.LastName  
FROM Person.Person AS a  
WHERE EXISTS  
(SELECT *   
    FROM HumanResources.Employee AS b  
    WHERE a.BusinessEntityID = b.BusinessEntityID  
    AND a.LastName = 'Johnson') ;  
GO  
```  
  
 La requête suivante utilise `IN`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT a.FirstName, a.LastName  
FROM Person.Person AS a  
WHERE a.LastName IN  
(SELECT a.LastName  
    FROM HumanResources.Employee AS b  
    WHERE a.BusinessEntityID = b.BusinessEntityID  
    AND a.LastName = 'Johnson') ;  
GO  
```  
  
 Voici le jeu de résultats pour ces deux requêtes.  
  
 ```
FirstName                                          LastName
-------------------------------------------------- ----------
Barry                                              Johnson
David                                              Johnson
Willis                                             Johnson
  
(3 row(s) affected)
 ```  
  
### <a name="c-comparing-queries-by-using-exists-and--any"></a>C. Comparaison de requêtes à l'aide de EXISTS et de = ANY  
 L'exemple suivant présente deux requêtes permettant de retrouver le nom commun à plusieurs revendeurs. La première requête utilise `EXISTS` et la seconde `=``ANY`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT s.Name  
FROM Sales.Store AS s   
WHERE EXISTS  
(SELECT *  
    FROM Purchasing.Vendor AS v  
    WHERE s.Name = v.Name) ;  
GO  
```  
  
 La requête suivante utilise `= ANY`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT s.Name  
FROM Sales.Store AS s   
WHERE s.Name = ANY  
(SELECT v.Name  
    FROM Purchasing.Vendor AS v ) ;  
GO  
```  
  
### <a name="d-comparing-queries-by-using-exists-and-in"></a>D. Comparaison de requêtes à l'aide de EXISTS et de IN  
 L'exemple suivant illustre des requêtes chargées de retrouver les employés travaillant dans les différentes divisions d'une entreprise et dont le nom commence par `P`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p   
JOIN HumanResources.Employee AS e  
   ON e.BusinessEntityID = p.BusinessEntityID   
WHERE EXISTS  
(SELECT *  
    FROM HumanResources.Department AS d  
    JOIN HumanResources.EmployeeDepartmentHistory AS edh  
       ON d.DepartmentID = edh.DepartmentID  
    WHERE e.BusinessEntityID = edh.BusinessEntityID  
    AND d.Name LIKE 'P%') ;  
GO  
```  
  
 La requête suivante utilise `IN`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p JOIN HumanResources.Employee AS e  
   ON e.BusinessEntityID = p.BusinessEntityID   
JOIN HumanResources.EmployeeDepartmentHistory AS edh  
   ON e.BusinessEntityID = edh.BusinessEntityID   
WHERE edh.DepartmentID IN  
(SELECT DepartmentID  
   FROM HumanResources.Department  
   WHERE Name LIKE 'P%') ;  
GO  
```  
  
### <a name="e-using-not-exists"></a>E. Utilisation de NOT EXISTS  
 NOT EXISTS fonctionne à l'inverse de EXISTS. La clause WHERE figurant dans NOT EXISTS est satisfaite si aucune ligne n'est retournée par la sous-requête. L'exemple suivant recense les employés qui ne travaillent pas dans les différentes divisions d'une entreprise et dont le nom commence par `P`.  
  
```sql  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p   
JOIN HumanResources.Employee AS e  
   ON e.BusinessEntityID = p.BusinessEntityID   
WHERE NOT EXISTS  
(SELECT *  
   FROM HumanResources.Department AS d  
   JOIN HumanResources.EmployeeDepartmentHistory AS edh  
      ON d.DepartmentID = edh.DepartmentID  
   WHERE e.BusinessEntityID = edh.BusinessEntityID  
   AND d.Name LIKE 'P%')  
ORDER BY LastName, FirstName  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
FirstName                      LastName                       Title
------------------------------ ------------------------------ ------------
Syed                           Abbas                          Pacific Sales Manager
Hazem                          Abolrous                       Quality Assurance Manager
Humberto                       Acevedo                        Application Specialist
Pilar                          Ackerman                       Shipping & Receiving Superviso
François                       Ajenstat                       Database Administrator
Amy                            Alberts                        European Sales Manager
Sean                           Alexander                      Quality Assurance Technician
Pamela                         Ansman-Wolfe                   Sales Representative
Zainal                         Arifin                         Document Control Manager
David                          Barber                         Assistant to CFO
Paula                          Barreto de Mattos              Human Resources Manager
Shai                           Bassli                         Facilities Manager
Wanida                         Benshoof                       Marketing Assistant
Karen                          Berg                           Application Specialist
Karen                          Berge                          Document Control Assistant
Andreas                        Berglund                       Quality Assurance Technician
Matthias                       Berndt                         Shipping & Receiving Clerk
Jo                             Berry                          Janitor
Jimmy                          Bischoff                       Stocker
Michael                        Blythe                         Sales Representative
David                          Bradley                        Marketing Manager
Kevin                          Brown                          Marketing Assistant
David                          Campbell                       Sales Representative
Jason                          Carlson                        Information Services Manager
Fernando                       Caro                           Sales Representative
Sean                           Chai                           Document Control Assistant
Sootha                         Charncherngkha                 Quality Assurance Technician
Hao                            Chen                           HR Administrative Assistant
Kevin                          Chrisulis                      Network Administrator
Pat                            Coleman                        Janitor
Stephanie                      Conroy                         Network Manager
Debra                          Core                           Application Specialist
Ovidiu                         Crãcium                        Sr. Tool Designer
Grant                          Culbertson                     HR Administrative Assistant
Mary                           Dempsey                        Marketing Assistant
Thierry                        D'Hers                         Tool Designer
Terri                          Duffy                          VP Engineering
Susan                          Eaton                          Stocker
Terry                          Eminhizer                      Marketing Specialist
Gail                           Erickson                       Design Engineer
Janice                         Galvin                         Tool Designer
Mary                           Gibson                         Marketing Specialist
Jossef                         Goldberg                       Design Engineer
Sariya                         Harnpadoungsataya              Marketing Specialist
Mark                           Harrington                     Quality Assurance Technician
Magnus                         Hedlund                        Facilities Assistant
Shu                            Ito                            Sales Representative
Stephen                        Jiang                          North American Sales Manager
Willis                         Johnson                        Recruiter
Brannon                        Jones                          Finance Manager
Tengiz                         Kharatishvili                  Control Specialist
Christian                      Kleinerman                     Maintenance Supervisor
Vamsi                          Kuppa                          Shipping & Receiving Clerk
David                          Liu                            Accounts Manager
Vidur                          Luthra                         Recruiter
Stuart                         Macrae                         Janitor
Diane                          Margheim                       Research & Development Enginee
Mindy                          Martin                         Benefits Specialist
Gigi                           Matthew                        Research & Development Enginee
Tete                           Mensa-Annan                    Sales Representative
Ramesh                         Meyyappan                      Application Specialist
Dylan                          Miller                         Research & Development Manager
Linda                          Mitchell                       Sales Representative
Barbara                        Moreland                       Accountant
Laura                          Norman                         Chief Financial Officer
Chris                          Norred                         Control Specialist
Jae                            Pak                            Sales Representative
Wanda                          Parks                          Janitor
Deborah                        Poe                            Accounts Receivable Specialist
Kim                            Ralls                          Stocker
Tsvi                           Reiter                         Sales Representative
Sharon                         Salavaria                      Design Engineer
Ken                            Sanchez                        Chief Executive Officer
José                           Saraiva                        Sales Representative
Mike                           Seamans                        Accountant
Ashvini                        Sharma                         Network Administrator
Janet                          Sheperdigian                   Accounts Payable Specialist
Candy                          Spoon                          Accounts Receivable Specialist
Michael                        Sullivan                       Sr. Design Engineer
Dragan                         Tomic                          Accounts Payable Specialist
Lynn                           Tsoflias                       Sales Representative
Rachel                         Valdez                         Sales Representative
Garrett                        Vargar                         Sales Representative
Ranjit                         Varkey Chudukatil              Sales Representative
Bryan                          Walton                         Accounts Receivable Specialist
Jian Shuo                      Wang                           Engineering Manager
Brian                          Welcker                        VP Sales
Jill                           Williams                       Marketing Specialist
Dan                            Wilson                         Database Administrator
John                           Wood                           Marketing Specialist
Peng                           Wu                             Quality Assurance Supervisor
  
(91 row(s) affected)
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-using-exists"></a>F. Utilisation de EXISTS  
 L’exemple suivant détermine si les lignes de la table `ProspectiveBuyer` peuvent être des correspondances des lignes de la table `DimCustomer`. La requête retourne des lignes uniquement quad les deux valeurs `LastName` et `BirthDate` correspondent dans les deux tables.  
  
```sql
-- Uses AdventureWorks  
  
SELECT a.LastName, a.BirthDate  
FROM DimCustomer AS a  
WHERE EXISTS  
(SELECT *   
    FROM dbo.ProspectiveBuyer AS b  
    WHERE (a.LastName = b.LastName) AND (a.BirthDate = b.BirthDate)) ;  
```  
  
### <a name="g-using-not-exists"></a>G. Utilisation de NOT EXISTS  
 NOT EXISTS fonctionne à l’opposé de EXISTS. La clause WHERE figurant dans NOT EXISTS est satisfaite si aucune ligne n'est retournée par la sous-requête. L’exemple suivant recherche les lignes de la table `DimCustomer` dont les valeurs `LastName` et `BirthDate` ne correspondent à aucune entrée dans la table `ProspectiveBuyers`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT a.LastName, a.BirthDate  
FROM DimCustomer AS a  
WHERE NOT EXISTS  
(SELECT *   
    FROM dbo.ProspectiveBuyer AS b  
    WHERE (a.LastName = b.LastName) AND (a.BirthDate = b.BirthDate)) ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  


