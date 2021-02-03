---
description: NEXT VALUE FOR (Transact-SQL)
title: NEXT VALUE FOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NEXT_VALUE_TSQL
- NEXT
- NEXT VALUE
- NEXT VALUE FOR
- NEXT_TSQL
- NEXT_VALUE_FOR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- NEXT VALUE FOR function
- sequence number object, NEXT VALUE FOR function
ms.assetid: 92632ed5-9f32-48eb-be28-a5e477ef9076
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 4af385b2d2e8874fe7554fe213c355a97382d520
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189941"
---
# <a name="next-value-for-transact-sql"></a>NEXT VALUE FOR (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Génère un numéro séquentiel de l'objet séquence spécifié.  
  
 Pour une description complète de la création et de l’utilisation de séquences, consultez [Numéros séquentiels](../../relational-databases/sequence-numbers/sequence-numbers.md). Utilisez [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) pour réserver une plage de numéros séquentiels.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
NEXT VALUE FOR [ database_name . ] [ schema_name . ]  sequence_name  
   [ OVER (<over_order_by_clause>) ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *database_name*  
 Nom de la base de données qui contient l'objet séquence.  
  
 *schema_name*  
 Nom du schéma qui contient l'objet séquence.  
  
 *sequence_name*  
 Nom de l'objet séquence qui génère le nombre.  
  
 *over_order_by_clause*  
 Détermine l'ordre dans lequel la valeur de séquence est affectée aux lignes d'une partition. Pour plus d’informations, consultez [Clause OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Types de retour  
 Retourne un nombre à l'aide du type de la séquence.  
  
## <a name="remarks"></a>Remarques  
 La fonction **NEXT VALUE FOR** peut être utilisée dans les procédures stockées et les déclencheurs.  
  
 Lorsque la fonction **NEXT VALUE FOR** est utilisée dans une requête ou une contrainte par défaut, si le même objet séquence est utilisé plusieurs fois, ou si le même objet séquence est utilisé à la fois dans l’instruction qui fournit les valeurs et dans une contrainte par défaut en cours d’exécution, la même valeur est retournée pour toutes les colonnes qui référencent la même séquence dans une ligne dans le jeu de résultats.  
  
 La fonction **NEXT VALUE FOR** est non déterministe et n’est autorisée que dans les contextes où le nombre de valeurs de séquence générées est bien défini. Vous trouverez ci-dessous la définition du nombre de valeurs qui sera utilisé pour chaque objet séquence référencé dans une instruction donnée :  
  
-   **SELECT** - Pour chaque objet séquence référencé, une nouvelle valeur est générée une fois par ligne dans le résultat de l’instruction.  
  
-   **INSERT** ... **VALUES** - Pour chaque objet séquence référencé, une nouvelle valeur est générée une fois par ligne insérée dans l’instruction.  
  
-   **UPDATE** - Pour chaque objet séquence référencé, une nouvelle valeur est générée pour chaque ligne mise à jour par l’instruction.  
  
-   Instructions de procédure (telles que **DECLARE**, **SET**, etc.) - Pour chaque objet séquence référencé, une nouvelle valeur est générée pour chaque instruction.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 La fonction **NEXT VALUE FOR** ne peut pas être utilisée dans les situations suivantes :  
  
-   Lorsqu'une base de données est en mode en lecture seule.  
  
-   Comme argument à une fonction table.  
  
-   Comme argument à une fonction d'agrégation.  
  
-   Dans les sous-requêtes, notamment les expressions de table communes et les tables dérivées.  
  
-   Dans les vues, dans les fonctions définies par l'utilisateur ou dans les colonnes calculées.  
  
-   Dans une instruction qui utilise l’opérateur **DISTINCT**, **UNION**, **UNION ALL**, **EXCEPT** ou **INTERSECT**.  
  
-   Dans une instruction qui utilise la clause **ORDER BY**, sauf si **NEXT VALUE FOR** ... **OVER** (**ORDER BY** ...) est utilisé.  
  
-   Dans les clauses suivantes : **FETCH**, **OVER**, **OUTPUT**, **ON**, **PIVOT**, **UNPIVOT**, **GROUP BY**, **HAVING**, **COMPUTE**, **COMPUTE BY** ou **FOR XML**.  
  
-   Dans les expressions conditionnelles qui utilisent **CASE**, **CHOOSE**, **COALESCE**, **IIF**, **ISNULL** ou **NULLIF**.  
  
-   Dans une clause **VALUES** qui ne fait pas partie d’une instruction **INSERT**.  
  
-   Dans la définition d'une contrainte de validation.  
  
-   Dans la définition d'une règle ou d'un objet par défaut. (Il peut être utilisé dans une contrainte par défaut.)  
  
-   Comme valeur par défaut dans un type de table défini par l'utilisateur.  
  
-   Dans une instruction qui utilise **TOP**, **OFFSET**, ou lorsque l’option **ROWCOUNT** est définie.  
  
-   Dans la clause **WHERE** d’une instruction.  
  
-   Dans une instruction **MERGE**. (Sauf lorsque la fonction **NEXT VALUE FOR** est utilisée dans une contrainte par défaut dans la table cible et que la valeur par défaut est utilisée dans l’instruction **CREATE** de l’instruction **MERGE**.)  
  
## <a name="using-a-sequence-object-in-a-default-constraint"></a>Utilisation d'un objet séquence dans une contrainte par défaut  
 Quand vous utilisez la fonction **NEXT VALUE FOR** dans une contrainte par défaut, les règles suivantes s’appliquent :  
  
-   Un objet séquence unique peut être référencé à partir de contraintes par défaut dans plusieurs tables.  
  
-   La table et l'objet séquence doivent résider dans la même base de données.  
  
-   L'utilisateur qui ajoute la contrainte par défaut doit avoir l'autorisation REFERENCES sur l'objet séquence.  
  
-   Un objet séquence référencé à partir d'une contrainte par défaut ne peut pas être supprimé avant la suppression de la contrainte par défaut.  
  
-   Le même numéro séquentiel est retourné pour toutes les colonnes dans une ligne si plusieurs contraintes par défaut utilisent le même objet séquence, ou si le même objet séquence est utilisé à la fois dans l'instruction qui fournit les valeurs et dans une contrainte par défaut en cours d'exécution.  
  
-   Les références à la fonction **NEXT VALUE FOR** dans une contrainte par défaut ne peuvent pas spécifier la clause **OVER**.  
  
-   Un objet séquence référencé dans une contrainte par défaut peut être modifié.  
  
-   Dans le cas d’une instruction `INSERT ... SELECT` ou `INSERT ... EXEC` où les données insérées proviennent d’une requête qui utilise une clause **ORDER BY**, les valeurs retournées par la fonction **NEXT VALUE FOR** sont générées dans l’ordre spécifié par la clause **ORDER BY**.  
  
## <a name="using-a-sequence-object-with-an-over-order-by-clause"></a>Utilisation d'un objet séquence avec une clause OVER ORDER BY  
 La fonction **NEXT VALUE FOR** prend en charge la génération de valeurs de séquence triées en appliquant la clause **OVER** à l’appel **NEXT VALUE FOR**. En utilisant la clause **OVER**, un utilisateur a la certitude que les valeurs retournées sont générées dans l’ordre de la sous-clause **ORDER BY** de la clause **OVER**. Les règles supplémentaires suivantes s’appliquent lors de l’utilisation de la fonction **NEXT VALUE FOR** avec la clause **OVER** :  
  
-   Les appels multiples à la fonction **NEXT VALUE FOR** pour le même générateur de séquence dans une instruction unique doivent tous utiliser la même définition de clause **OVER**.  
  
-   Les appels multiples à la fonction **NEXT VALUE FOR** qui référence différents générateurs de séquence dans une instruction unique peuvent avoir des définitions de clause **OVER** différentes.  
  
-   Une clause **OVER** appliquée à la fonction **NEXT VALUE FOR** ne prend pas en charge la sous-clause **PARTITION BY**.  
  
-   Si tous les appels à la fonction **NEXT VALUE FOR** dans une instruction **SELECT** spécifient la clause **OVER**, une clause **ORDER BY** peut être utilisée dans l’instruction **SELECT**.  
  
-   La clause **OVER** est autorisée avec la fonction **NEXT VALUE FOR** en cas d’utilisation dans une instruction **SELECT** ou une instruction `INSERT ... SELECT ...`. L’utilisation de la clause **OVER** avec la fonction **NEXT VALUE FOR** n’est pas autorisée dans des instructions **UPDATE** ou **MERGE**.  
  
-   Si un autre processus accède simultanément à l'objet séquence, les nombres retournés peuvent comporter des espaces vides.  
  
## <a name="metadata"></a>Métadonnées  
 Pour plus d’informations sur les séquences, interrogez la vue de catalogue [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md).  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **UPDATE** sur l’objet séquence ou le schéma de la séquence. Pour obtenir un exemple d'octroi d'une autorisation, consultez l'exemple F dans la suite de cette rubrique.  
  
### <a name="ownership-chaining"></a>Chaînage des propriétés  
 Les objets séquences prennent en charge le chaînage des propriétés. Si l'objet séquence a le même propriétaire que la procédure stockée appelante, le déclencheur ou la table (ayant un objet séquence comme contrainte par défaut), aucun contrôle d'autorisation n'est obligatoire sur l'objet séquence. Si l'objet séquence n'est pas détenu par le même utilisateur que la procédure stockée appelante, le déclencheur ou la table, un contrôle d'autorisation est obligatoire sur l'objet séquence.  
  
 Lorsque la fonction **NEXT VALUE FOR** est utilisée comme valeur par défaut dans une table, les utilisateurs ont à la fois besoin de l’autorisation **INSERT** sur la table et de l’autorisation **UPDATE** sur l’objet séquence, pour insérer des données à l’aide de la valeur par défaut.  
  
-   Si la contrainte par défaut a le même propriétaire que l'objet séquence, aucune autorisation n'est obligatoire sur l'objet séquence lorsque la contrainte par défaut est appelée.  
  
-   Si la contrainte par défaut et l'objet séquence ne sont pas détenus par le même utilisateur, des autorisations sont requises sur l'objet séquence même s'il est appelé via la contrainte par défaut.  
  
### <a name="audit"></a>Audit  
 Pour auditer la fonction **NEXT VALUE FOR**, surveillez SCHEMA_OBJECT_ACCESS_GROUP.  
  
## <a name="examples"></a>Exemples  
 Pour obtenir des exemples de création de séquences et d’utilisation de la fonction **NEXT VALUE FOR** pour générer des numéros séquentiels, consultez [Numéros séquentiels](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 Les exemples suivants utilisent une séquence nommée `CountBy1` dans un schéma nommé `Test`. Exécutez l'instruction suivante pour créer la séquence `Test.CountBy1`. Les exemples C et E utilisent la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] ; par conséquent, la séquence `CountBy1` est créée dans cette base de données.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
  
CREATE SCHEMA Test;  
GO  
  
CREATE SEQUENCE Test.CountBy1  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="a-using-a-sequence-in-a-select-statement"></a>R. Utilisation d'une séquence dans une instruction SELECT  
 L'exemple suivant crée une séquence nommée `CountBy1` qui augmente de 1 chaque fois qu'elle est utilisée.  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1 AS FirstUse;  
SELECT NEXT VALUE FOR Test.CountBy1 AS SecondUse;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
FirstUse  
1  
  
SecondUse  
2
```  
  
### <a name="b-setting-a-variable-to-the-next-sequence-value"></a>B. Définition d'une variable sur la valeur de séquence suivante  
 L'exemple suivant illustre trois façons d'affecter à une variable la valeur suivante d'un numéro séquentiel.  
  
```sql  
DECLARE @myvar1 BIGINT = NEXT VALUE FOR Test.CountBy1  
DECLARE @myvar2 BIGINT ;  
DECLARE @myvar3 BIGINT ;  
SET @myvar2 = NEXT VALUE FOR Test.CountBy1 ;  
SELECT @myvar3 = NEXT VALUE FOR Test.CountBy1 ;  
SELECT @myvar1 AS myvar1, @myvar2 AS myvar2, @myvar3 AS myvar3 ;  
GO  
```  
  
### <a name="c-using-a-sequence-with-a-ranking-window-function"></a>C. Utilisation d'une séquence avec une fonction de fenêtre de classement  
  
```sql  
USE AdventureWorks2012 ;  
GO  
  
SELECT NEXT VALUE FOR Test.CountBy1 OVER (ORDER BY LastName) AS ListNumber,  
    FirstName, LastName  
FROM Person.Contact ;  
GO  
```  
  
### <a name="d-using-the-next-value-for-function-in-the-definition-of-a-default-constraint"></a>D. Utilisation de la fonction NEXT VALUE FOR dans la définition d'une contrainte par défaut  
 L’utilisation de la fonction **NEXT VALUE FOR** dans la définition d’une contrainte par défaut est prise en charge. Pour obtenir un exemple d’utilisation de **NEXT VALUE FOR** dans une instruction **CREATE TABLE**, consultez l’exemple C de [Numéros séquentiels](../../relational-databases/sequence-numbers/sequence-numbers.md). L'exemple suivant utilise `ALTER TABLE` pour ajouter une séquence comme valeur par défaut à une table actuelle.  
  
```sql
CREATE TABLE Test.MyTable  
(  
    IDColumn NVARCHAR(25) PRIMARY KEY,  
    name VARCHAR(25) NOT NULL  
) ;  
GO  
  
CREATE SEQUENCE Test.CounterSeq  
    AS INT  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
  
ALTER TABLE Test.MyTable  
    ADD   
        DEFAULT N'AdvWorks_' +   
        CAST(NEXT VALUE FOR Test.CounterSeq AS NVARCHAR(20))   
        FOR IDColumn;  
GO  
  
INSERT Test.MyTable (name)  
VALUES ('Larry') ;  
GO  
  
SELECT * FROM Test.MyTable;  
GO  
```  
  
### <a name="e-using-the-next-value-for-function-in-an-insert-statement"></a>E. Utilisation de la fonction NEXT VALUE FOR dans une instruction INSERT  
 L'exemple suivant crée une table nommée `TestTable`, puis utilise la fonction `NEXT VALUE FOR` pour insérer une ligne.  
  
```sql  
CREATE TABLE Test.TestTable  
     (CounterColumn INT PRIMARY KEY,  
    Name NVARCHAR(25) NOT NULL) ;   
GO  
  
INSERT Test.TestTable (CounterColumn,Name)  
    VALUES (NEXT VALUE FOR Test.CountBy1, 'Syed') ;  
GO  
  
SELECT * FROM Test.TestTable;   
GO  
```  
  
### <a name="e-using-the-next-value-for-function-with-select--into"></a>E. Utilisation de la fonction NEXT VALUE FOR avec SELECT ... INTO  
 L’exemple suivant utilise l’instruction `SELECT ... INTO` pour créer une table nommée `Production.NewLocation` et utilise la fonction `NEXT VALUE FOR` pour numéroter chaque ligne.  
  
```sql  
USE AdventureWorks2012 ;   
GO  
  
SELECT NEXT VALUE FOR Test.CountBy1 AS LocNumber, Name   
    INTO Production.NewLocation  
    FROM Production.Location ;  
GO  
  
SELECT * FROM Production.NewLocation ;  
GO  
```  
  
### <a name="f-granting-permission-to-execute-next-value-for"></a>F. Octroi d'une autorisation pour exécuter NEXT VALUE FOR  
 L’exemple suivant accorde l’autorisation **UPDATE** à un utilisateur nommé `AdventureWorks\Larry` afin de l’autoriser à exécuter `NEXT VALUE FOR` à l’aide de la séquence `Test.CounterSeq`.  
  
```sql  
GRANT UPDATE ON OBJECT::Test.CounterSeq TO [AdventureWorks\Larry] ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)   
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [Numéros de séquence](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
