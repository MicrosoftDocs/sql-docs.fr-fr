---
description: rowversion (Transact-SQL)
title: rowversion (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- timestamp_TSQL
- timestamp
dev_langs:
- TSQL
helpviewer_keywords:
- rowversion data type
- size [SQL Server], rowversion
- row version-stamping [SQL Server]
- rowversion columns
- version-stamping table rows
- unique rowversion numbers
- unique timestamp numbers
- timestamp data type
- timestamp columns
- size [SQL Server], timestamp
ms.assetid: 65c9cf0e-3e8a-45f8-87b3-3460d96afb0b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 47117ba5b13d7b494174576d76cca9a5472f0c9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187829"
---
# <a name="rowversion-transact-sql"></a>rowversion (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Type de données présentant des nombres binaires uniques automatiquement générés à l'intérieur d'une base de données. **rowversion** est généralement utilisé comme mécanisme de marquage de version des lignes de tables. La taille de stockage est égale à 8 octets. Le type de données **rowversion** est seulement un numéro à incrémentation et ne permet pas de conserver une date ni une heure. Pour enregistrer une date ou une heure, utilisez le type de données **datetime2**.
  
## <a name="remarks"></a>Notes  
Chaque base de données dispose d’un compteur qui est incrémenté chaque fois qu’une opération d’insertion ou de mise à jour est effectuée dans une table contenant une colonne **rowversion** dans la base de données. Ce compteur est la base de données rowversion. Il suit une heure relative au sein d'une base de données, et non pas une heure réelle qui peut être associée à une horloge. Une table ne peut comporter qu’une seule colonne **rowversion**. Chaque fois qu’une ligne associée à une colonne **rowversion** est modifiée ou insérée, la valeur incrémentée rowversion de la base de données est insérée dans la colonne **rowversion**. Cette propriété fait de la colonne **rowversion** un candidat peu valable pour les clés, en particulier les clés primaires. En effet, toute mise à jour apportée à la ligne a pour effet de changer la valeur rowversion, et par conséquent de modifier la valeur de la clé. Si la colonne se trouve dans une clé primaire, l'ancienne valeur de clé n'est plus valide et les clés étrangères faisant référence à l'ancienne valeur ne sont plus valides. Si la table est référencée dans un curseur dynamique, toutes les mises à jour modifient la position des lignes dans le curseur. Si la colonne est une clé d'index, toutes les mises à jour apportées à la ligne de données génèrent également des mises à jour de l'index.  La valeur **rowversion** est incrémentée avec toute instruction de mise à jour, même si aucune valeur de ligne n’est modifiée. (Par exemple, si la valeur d’une colonne est égale à 5 et qu’une instruction de mise à jour définit la valeur 5, cette action est considérée comme une mise à jour même si aucune modification n’a été apportée et la valeur **rowversion** est incrémentée.)
  
**timestamp** est le synonyme du type de données **rowversion** et est soumis au comportement des synonymes des types de données. Dans les instructions DDL, utilisez **rowversion** au lieu de **timestamp** autant que possible. Pour plus d’informations, consultez [Synonymes des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-synonyms-transact-sql.md).
  
Le type de données  **timestamp** de [!INCLUDE[tsql](../../includes/tsql-md.md)] est différent du type de données **timestamp** défini dans la norme ISO.
  
> [!NOTE]  
>  La syntaxe **timestamp** est dépréciée. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
Dans une instruction CREATE TABLE ou ALTER TABLE, il n’est pas nécessaire de spécifier un nom de colonne pour le type de données **timestamp**, par exemple :
  
```sql
CREATE TABLE ExampleTable (PriKey int PRIMARY KEY, timestamp);  
```  
  
Si vous ne spécifiez pas de nom de colonne, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] génère le nom de colonne **timestamp** ; toutefois, le synonyme **rowversion** ne suit pas ce comportement. Quand vous utilisez **rowversion**, vous devez spécifier un nom de colonne, par exemple :
  
```sql
CREATE TABLE ExampleTable2 (PriKey int PRIMARY KEY, VerCol rowversion) ;  
```  
  
> [!NOTE]  
>  Des valeurs **rowversion** en double peuvent être générées quand l’instruction SELECT INTO est utilisée et qu’elle contient une colonne **rowversion** dans la liste SELECT. Nous vous déconseillons d’utiliser **rowversion** de cette façon.  
  
Une colonne **rowversion** qui n’accepte pas la valeur Null est sémantiquement équivalente à une colonne **binary(8)**. Une colonne **rowversion** qui accepte la valeur Null est sémantiquement équivalente à une colonne **varbinary(8)**.
  
La colonne **rowversion** d’une ligne permet de déterminer facilement si une instruction de mise à jour a été exécutée sur la ligne depuis la dernière fois qu’elle a été lue. Si une instruction de mise à jour est exécutée sur la ligne, la valeur rowversion est mise à jour. Si aucune instruction de mise à jour n’est exécutée sur la ligne, la valeur rowversion est la même que la dernière fois qu’elle a été lue. Pour retourner la valeur rowversion actuelle pour une base de données, utilisez [@@DBTS](../../t-sql/functions/dbts-transact-sql.md).
  
Vous pouvez ajouter une colonne **rowversion** à une table afin de mieux assurer l’intégrité de la base de données quand plusieurs utilisateurs mettent à jour des lignes en même temps. Vous pouvez souhaiter également savoir combien de lignes et quelles lignes ont été mises à jour sans réinterroger la table.
  
Par exemple, supposons que vous créez une table nommée `MyTest`. Vous insérez des données dans la table en exécutant les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes.
  
```sql
CREATE TABLE MyTest (myKey int PRIMARY KEY  
    ,myValue int, RV rowversion);  
GO   
INSERT INTO MyTest (myKey, myValue) VALUES (1, 0);  
GO   
INSERT INTO MyTest (myKey, myValue) VALUES (2, 0);  
GO  
```  
  
Vous pouvez utiliser ensuite les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] de l'exemple pour implémenter un contrôle d'accès concurrentiel optimiste sur la table `MyTest` pendant la mise à jour. Le script utilise `<myRv>` pour représenter la valeur **rowversion** de la dernière fois que vous avez lu la ligne. Remplacez la valeur par la valeur **rowversion** réelle. Un exemple de valeur **rowversion** réelle est `0x00000000000007D3`.
  
```sql
DECLARE @t TABLE (myKey int);  
UPDATE MyTest  
SET myValue = 2  
    OUTPUT inserted.myKey INTO @t(myKey)   
WHERE myKey = 1   
    AND RV = <myRv>;  
IF (SELECT COUNT(*) FROM @t) = 0  
    BEGIN  
        RAISERROR ('error changing row with myKey = %d'  
            ,16 -- Severity.  
            ,1 -- State   
            ,1) -- myKey that was changed   
    END;  
```  
  


Vous pouvez également mettre les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] de l'exemple dans une transaction. En interrogeant la variable `@t` dans l'étendue de la transaction, vous pouvez récupérer la colonne `myKey` mise à jour de la table sans réinterroger la table `MyTest`.

Voici le même exemple avec la syntaxe **timestamp**. Remplacez `<myTS>` par un **horodatage** réel.

```sql
CREATE TABLE MyTest2 (myKey int PRIMARY KEY  
    ,myValue int, TS timestamp);  
GO   
INSERT INTO MyTest2 (myKey, myValue) VALUES (1, 0);  
GO   
INSERT INTO MyTest2 (myKey, myValue) VALUES (2, 0);  
GO  
DECLARE @t TABLE (myKey int);  
UPDATE MyTest2  
SET myValue = 2  
    OUTPUT inserted.myKey INTO @t(myKey)   
WHERE myKey = 1   
    AND TS = <myTS>;  
IF (SELECT COUNT(*) FROM @t) = 0  
    BEGIN  
        RAISERROR ('error changing row with myKey = %d'  
            ,16 -- Severity.  
            ,1 -- State   
            ,1) -- myKey that was changed   
    END;  
```  
  
## <a name="see-also"></a>Voir aussi
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)  
[INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)  
[MIN_ACTIVE_ROWVERSION &#40;Transact-SQL&#41;](../../t-sql/functions/min-active-rowversion-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)
  
  
