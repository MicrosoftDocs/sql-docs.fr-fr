---
description: COLUMNS_UPDATED (Transact-SQL)
title: COLUMNS_UPDATED (Transact-SQL)
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COLUMNS_UPDATED_TSQL
- COLUMNS_UPDATED
dev_langs:
- TSQL
helpviewer_keywords:
- COLUMNS_UPDATED function
- testing columns
- column testing [SQL Server]
- updated columns
ms.assetid: 765fde44-1f95-4015-80a4-45388f18a42c
author: cawrites
ms.author: chadam
ms.openlocfilehash: c927fbad08c222aad4e284a66d3b3e61061e82e9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101278"
---
# <a name="columns_updated-transact-sql"></a>COLUMNS_UPDATED (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction retourne un modèle binaire de type **varbinary** qui indique les colonnes insérées ou mises à jour d’une table ou d’une vue. Utilisez `COLUMNS_UPDATED` à n’importe quel endroit du corps d’un déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)] INSERT ou UPDATE pour tester si celui-ci doit exécuter certaines actions.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
COLUMNS_UPDATED ( )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
**varbinary**
  
## <a name="remarks"></a>Notes  
`COLUMNS_UPDATED` effectue un test pour déterminer si des actions UPDATE ou INSERT sont réalisées sur plusieurs colonnes. Pour tester des tentatives UPDATE ou INSERT sur une colonne, utilisez [UPDATE()](../../t-sql/functions/update-trigger-functions-transact-sql.md).
  
`COLUMNS_UPDATED` retourne un ou plusieurs octets classés de gauche à droite. Le bit le plus à droite dans chaque octet est le bit le moins significatif. Le bit le plus à droite de l’octet le plus à gauche représente la première colonne de la table, le suivant à gauche représente la deuxième colonne, etc. `COLUMNS_UPDATED` retourne plusieurs octets si la table sur laquelle le déclencheur est créé contient plus de huit colonnes, l’octet le moins significatif étant celui le plus à gauche. `COLUMNS_UPDATED` retourne TRUE pour toutes les colonnes des actions INSERT, car les valeurs insérées dans ces colonnes sont explicites ou implicites (NULL).
  
Pour tester l’existence de mises à jour ou d’insertions dans des colonnes spécifiques, utilisez dans la syntaxe un opérateur au niveau du bit et un masque de bits d’entier des colonnes testées. Par exemple, la table **t1** contient les colonnes **C1**, **C2**, **C3**, **C4** et **C5**. Pour vérifier que les colonnes **C2**, **C3** et **C4** ont toutes été mises à jour avec succès (en supposant que la table **t1** ait un déclencheur UPDATE), indiquez dans la syntaxe **& 14**. Pour vérifier si seule la colonne **C2** est mise à jour, spécifiez **& 2**. Consultez [Exemple A](#a-using-columns_updated-to-test-the-first-eight-columns-of-a-table) et [Exemple B](#b-using-columns_updated-to-test-more-than-eight-columns) pour obtenir des exemples réels.
  
Utilisez `COLUMNS_UPDATED` n’importe où dans un déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)] INSERT ou UPDATE.
  
La colonne ORDINAL_POSITION de la vue INFORMATION_SCHEMA.COLUMNS n’est pas compatible avec le modèle binaire des colonnes retournées par `COLUMNS_UPDATED`. Pour obtenir un modèle binaire compatible avec `COLUMNS_UPDATED`, référencez la propriété `ColumnID` de la fonction système `COLUMNPROPERTY` quand vous interrogez la vue `INFORMATION_SCHEMA.COLUMNS`, comme illustré dans l’exemple suivant :
  
```sql
SELECT TABLE_NAME, COLUMN_NAME,  
    COLUMNPROPERTY(OBJECT_ID(TABLE_SCHEMA + '.' + TABLE_NAME),  
    COLUMN_NAME, 'ColumnID') AS COLUMN_ID  
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS  
WHERE TABLE_NAME = 'Person';  
```  

Si un déclencheur s’applique à une colonne, `COLUMNS_UPDATED` retourne `true` ou `1`, même si la valeur de colonne reste inchangée. Cela est intentionnel. Le déclencheur doit implémenter une logique métier qui détermine si l’opération d’insertion/de mise à jour/de suppression est autorisée ou non. 
  
## <a name="column-sets"></a>Jeux de colonnes
Quand un jeu de colonnes est défini sur une table, la fonction `COLUMNS_UPDATED` se comporte comme suit :
-   Lors de la mise à jour explicite d’une colonne membre du jeu de colonnes, le bit correspondant à cette colonne est défini sur 1 et le bit du jeu de colonnes est défini sur 1.  
-   Lors de la mise à jour explicite d’un jeu de colonnes, le bit du jeu de colonnes est défini sur 1 et les bits pour toutes les colonnes éparses de cette table sont définis sur 1.  
-   Pour les opérations d'insertion, tous les bits sont définis sur 1.  
  
     Comme des modifications apportées à un jeu de colonnes entraînent la réinitialisation de toutes les colonnes du jeu de colonnes à 1, les colonnes non modifiées d’un jeu de colonnes apparaissent comme étant modifiées. Pour plus d’informations sur les jeux de colonnes, consultez [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-columns_updated-to-test-the-first-eight-columns-of-a-table"></a>R. Utilisation de COLUMNS_UPDATED pour tester les huit premières colonnes d'une table  
Cet exemple crée deux tables, `employeeData` et `auditEmployeeData`. La table `employeeData` contient des informations sensibles relatives aux salaires des employés et les membres du service des ressources humaines peuvent la modifier. Si le numéro de sécurité sociale, le salaire annuel ou le numéro de compte bancaire d’un employé change, un enregistrement d’audit est généré et inséré dans la table d’audit `auditEmployeeData`.
  
Avec la fonction `COLUMNS_UPDATED()`, nous pouvons tester rapidement si des modifications ont été apportées aux colonnes contenant des informations sensibles relatives aux employés. Cette utilisation de `COLUMNS_UPDATED()` fonctionne seulement pour détecter des modifications apportées aux huit premières colonnes de la table.
  
```sql
USE AdventureWorks2012;  
GO  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_NAME = 'employeeData')  
   DROP TABLE employeeData;  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_NAME = 'auditEmployeeData')  
   DROP TABLE auditEmployeeData;  
GO  
CREATE TABLE dbo.employeeData (  
   emp_id INT NOT NULL PRIMARY KEY,  
   emp_bankAccountNumber CHAR (10) NOT NULL,  
   emp_salary INT NOT NULL,  
   emp_SSN CHAR (11) NOT NULL,  
   emp_lname NCHAR (32) NOT NULL,  
   emp_fname NCHAR (32) NOT NULL,  
   emp_manager INT NOT NULL  
   );  
GO  
CREATE TABLE dbo.auditEmployeeData (  
   audit_log_id uniqueidentifier DEFAULT NEWID() PRIMARY KEY,  
   audit_log_type CHAR (3) NOT NULL,  
   audit_emp_id INT NOT NULL,  
   audit_emp_bankAccountNumber CHAR (10) NULL,  
   audit_emp_salary INT NULL,  
   audit_emp_SSN CHAR (11) NULL,  
   audit_user sysname DEFAULT SUSER_SNAME(),  
   audit_changed DATETIME DEFAULT GETDATE()  
   );  
GO  
CREATE TRIGGER dbo.updEmployeeData   
ON dbo.employeeData   
AFTER UPDATE AS  
/* Check whether columns 2, 3 or 4 have been updated. If any or all  
columns 2, 3 or 4 have been changed, create an audit record.
The bitmask is: power(2, (2-1)) + power(2, (3-1)) + power(2, (4-1)) = 14.
This bitmask translates into base_10 as: 2 + 4 + 8 = 14.
To test whether all columns 2, 3, and 4 are updated, use = 14 instead of > 0  
(below). */
  
   IF (COLUMNS_UPDATED() & 14) > 0  
/* Use IF (COLUMNS_UPDATED() & 14) = 14 to see whether all columns 2, 3,   
and 4 are updated. */  
      BEGIN  
-- Audit OLD record.  
      INSERT INTO dbo.auditEmployeeData  
         (audit_log_type,  
         audit_emp_id,  
         audit_emp_bankAccountNumber,  
         audit_emp_salary,  
         audit_emp_SSN)  
         SELECT 'OLD',   
            del.emp_id,  
            del.emp_bankAccountNumber,  
            del.emp_salary,  
            del.emp_SSN  
         FROM deleted del;  
  
-- Audit NEW record.  
      INSERT INTO dbo.auditEmployeeData  
         (audit_log_type,  
         audit_emp_id,  
         audit_emp_bankAccountNumber,  
         audit_emp_salary,  
         audit_emp_SSN)  
         SELECT 'NEW',  
            ins.emp_id,  
            ins.emp_bankAccountNumber,  
            ins.emp_salary,  
            ins.emp_SSN  
         FROM inserted ins;  
   END;  
GO  
  
/* Inserting a new employee does not cause the UPDATE trigger to fire. */  
INSERT INTO employeeData  
   VALUES ( 101, 'USA-987-01', 23000, 'R-M53550M', N'Mendel', N'Roland', 32);  
GO  
  
/* Updating the employee record for employee number 101 to change the   
salary to 51000 causes the UPDATE trigger to fire and an audit trail to   
be produced. */  
  
UPDATE dbo.employeeData  
   SET emp_salary = 51000  
   WHERE emp_id = 101;  
GO  
SELECT * FROM auditEmployeeData;  
GO  
  
/* Updating the employee record for employee number 101 to change both   
the bank account number and social security number (SSN) causes the   
UPDATE trigger to fire and an audit trail to be produced. */  
  
UPDATE dbo.employeeData  
   SET emp_bankAccountNumber = '133146A0', emp_SSN = 'R-M53550M'  
   WHERE emp_id = 101;  
GO  
SELECT * FROM dbo.auditEmployeeData;  
  
GO  
```  
  
### <a name="b-using-columns_updated-to-test-more-than-eight-columns"></a>B. Utilisation de COLUMNS_UPDATED pour tester plus de huit colonnes  
Pour tester l’existence de mises à jour affectant des colonnes autres que les huit premières colonnes d’une table, utilisez la fonction `SUBSTRING` afin de tester le bit adéquat retourné par `COLUMNS_UPDATED`. Cet exemple teste l’existence de mises à jour affectant les colonnes `3`, `5` et `9` de la table `AdventureWorks2012.Person.Person`.
  
```sql
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.uContact2', N'TR') IS NOT NULL  
    DROP TRIGGER Person.uContact2;  
GO  
CREATE TRIGGER Person.uContact2 ON Person.Person  
AFTER UPDATE AS  
    IF ( (SUBSTRING(COLUMNS_UPDATED(), 1, 1) & 20 = 20)   
        AND (SUBSTRING(COLUMNS_UPDATED(), 2, 1) & 1 = 1) )   
    PRINT 'Columns 3, 5 and 9 updated';  
GO  
  
UPDATE Person.Person   
   SET NameStyle = NameStyle,  
      FirstName=FirstName,  
      EmailPromotion=EmailPromotion;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi
[Opérateurs au niveau du bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
[CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)  
[UPDATE&#40;&#41; &#40;Transact-SQL&#41;](../../t-sql/functions/update-trigger-functions-transact-sql.md)
  
  
