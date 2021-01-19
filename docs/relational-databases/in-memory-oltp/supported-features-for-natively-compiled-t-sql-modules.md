---
title: Fonctionnalités des modules T-SQL compilés nativement
description: Découvrez la surface d’exposition T-SQL et les fonctionnalités prises en charge dans le corps des modules T-SQL compilés en mode natif, tels que les procédures stockées et les fonctions scalaires définies par l’utilisateur.
ms.custom: seo-dt-2019
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 05515013-28b5-4ccf-9a54-ae861448945b
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 42d638afd9fc98fb68eb024c0a712bea5f278a26
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170221"
---
# <a name="supported-features-for-natively-compiled-t-sql-modules"></a>Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


  Cette rubrique présente la surface d’exposition T-SQL et dresse la liste des fonctionnalités prises en charge dans le corps des modules T-SQL compilés en mode natif, notamment les procédures stockées ([CREATE PROCEDURE Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)), les fonctions scalaires définies par l’utilisateur, les fonctions table incluses inline et les déclencheurs.  

 Pour connaître les fonctionnalités prises en charge autour de la définition des modules natifs, consultez [DDL pris en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-ddl-for-natively-compiled-t-sql-modules.md).  

 Pour plus d’informations sur les constructions qui ne sont pas prises en charge et sur la manière de contourner certaines des fonctionnalités non prises en charge dans les modules compilés en mode natif, consultez [Migration Issues for Natively Compiled Stored Procedures](./a-guide-to-query-processing-for-memory-optimized-tables.md). Pour plus d’informations sur les fonctionnalités non prises en charge, consultez [Les constructions Transact-SQL ne sont pas prises en charge par l’OLTP en mémoire](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md).  

##  <a name="query-surface-area-in-native-modules"></a><a name="qsancsp"></a> Surface d’exposition de requête dans les modules natifs  

Les constructions de requête suivantes sont prises en charge :  

Expression CASE : CASE peut être utilisé dans n'importe quelle instruction ou clause qui autorise une expression valide.
   - **S’applique à :** [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)].  
    Depuis [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)], les instructions CASE sont désormais prises en charge pour les modules T-SQL compilés en mode natif.

Clause SELECT :  

-   Alias de colonnes et de noms (en utilisant la syntaxe AS ou =).  

-   Sous-requêtes scalaires
    - **S’applique à :** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      Depuis [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], les sous-requêtes scalaires sont désormais prises en charge dans les modules compilés en mode natif.

-   TOP*  

-   SELECT DISTINCT  
    - **S’applique à :** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      Depuis [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], l’opérateur DISTINCT est pris en charge dans les modules compilés en mode natif.

        - Les fonctions d’agrégation DISTINCT ne sont pas prises en charge.  

-   UNION et UNION ALL
    - **S’applique à :** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      Depuis [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], les opérateurs UNION et UNION ALL sont désormais pris en charge dans les modules compilés en mode natif.

-   Affectations variables  

Clause FROM :  

-   FROM \<memory optimized table or table variable>  

-   FROM \<natively compiled inline TVF>  

-   LEFT OUTER JOIN, RIGHT OUTER JOIN, CROSS JOIN et INNER JOIN.
    - **S’applique à :** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      Depuis [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], les jointures sont désormais prises en charge dans les modules compilés en mode natif.

-   Sous-requêtes `[AS] table_alias`. Pour plus d’informations, consultez [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md). 
    - **S’applique à :** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      Depuis [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], les sous-requêtes sont désormais prises en charge dans les modules compilés en mode natif.

Clause WHERE :  

-   Prédicat de filtre IS [NOT] NULL  

-   AND, BETWEEN  
-   OR, NOT, IN, EXISTS
    - **S’applique à :** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].
      Depuis [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], les opérateurs OR/NOT/IN/EXISTS sont désormais pris en charge dans les modules compilés en mode natif.


Clause[GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md) :

- Fonctions d’agrégation AVG, COUNT, COUNT_BIG, MIN, MAX et SUM.  

- MIN et MAX ne sont pas pris en charge pour les types nvarchar, char, varchar, varchar, varbinary, et binary.  

Clause[ORDER BY](../../t-sql/queries/select-order-by-clause-transact-sql.md) :


- Il n’existe aucune prise en charge de **DISTINCT** dans la clause **ORDER BY** .


- Est pris en charge avec [GROUP BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-group-by-transact-sql.md) si une expression dans la liste ORDER BY apparaît de façon textuelle dans la liste GROUP BY.
  - Par exemple, GROUP BY a + b ORDER BY a + b est pris en charge, mais GROUP BY a, b ORDER BY a + b n'est pas pris en charge.  


Clause HAVING :

- Est assujetti aux mêmes limitations d’expression que la clause WHERE.


#### <a name="order-by-and-top-are-supported-in-natively-compiled-modules-with-some-restrictions"></a>ORDER BY et TOP sont pris en charge dans les modules compilés en mode natif, avec quelques restrictions.


- Il n'existe aucune prise en charge de **WITH TIES** ou **PERCENT** dans la clause **TOP** .


- Il n’existe aucune prise en charge de **DISTINCT** dans la clause **ORDER BY** .  


- La clause **TOP** associée à **ORDER BY** ne prend pas en charge plus de 8 192 lignes lorsqu'une constante est utilisée dans la clause **TOP** .
  - Cette limite peut être abaissée lorsque la requête contient des jointures ou des fonctions d'agrégation. Par exemple, avec une jointure (entre deux tables), la limite est de 4 096 lignes. Avec deux jointures (entre trois tables), la limite est de 2 730 lignes.  
  - Vous pouvez obtenir plus de 8 192 résultats en stockant le nombre de lignes dans une variable :  

```sql
DECLARE @v INT = 9000;
SELECT TOP (@v) ... FROM ... ORDER BY ...
```

Cependant, l'utilisation d'une constante dans la clause **TOP** donne de meilleurs résultats qu'une variable.  

Ces limitations sur [!INCLUDE[tsql](../../includes/tsql-md.md)] compilé en mode natif ne s’appliquent pas à l’accès [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété sur les tables optimisées en mémoire.  


##  <a name="data-modification"></a><a name="dml"></a> Modification de données  

Les instructions DML suivantes sont prises en charge.  

-   INSERT VALUES (une ligne par instruction) et INSERT... SELECT  

-   UPDATE  

-   Suppression  

-   La clause WHERE est prise en charge avec les instructions UPDATE et DELETE.  

##  <a name="control-of-flow-language"></a><a name="cof"></a> Langage de contrôle de flux  
 Les constructions de langage de contrôle de flux suivantes sont prises en charge.  

-   [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)  

-   [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  

-   [RETURN &#40;Transact-SQL&#41;](../../t-sql/language-elements/return-transact-sql.md)  

-   [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md) peut utiliser tous les [Types de données pris en charge pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md), ainsi que les types de table optimisée en mémoire. Les variables peuvent être déclarées comme NULL ou NOT NULL.  

-   [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  

-   [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)  

    - Pour optimiser les performances, utilisez un seul bloc TRY/CATCH pour un module T-SQL entièrement compilé en mode natif.  

-   [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)  

-   BEGIN ATOMIC (à l’extérieur de la procédure stockée). Pour plus d’informations, consultez [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).  

##  <a name="supported-operators"></a><a name="so"></a> Opérateurs pris en charge  
 Les opérateurs suivants sont pris en charge :  

-   [Opérateurs de comparaison &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md) (par exemple, >, \<, >= et <=)  

-   Opérateurs unaires (+, -).  

-   Opérateurs binaires (*, /, +, -, % (modulo)).  

    - L'opérateur plus (+) est pris en charge pour les nombres et les chaînes.  

-   Opérateurs logiques (AND, OR, NOT).  

-   Opérateurs au niveau du bit ~, &, |, et ^  

-   APPLY (opérateur)
    - **S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].  
      À partir de [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], l’opérateur APPLY est pris en charge dans les modules compilés en mode natif.

##  <a name="built-in-functions-in-natively-compiled-modules"></a><a name="bfncsp"></a> Fonctions intégrées dans les modules compilés en mode natif  
 Les fonctions suivantes sont prises en charge dans les contraintes sur les tables optimisées en mémoire et dans les modules T-SQL compilés en mode natif.  

-   Toutes les [Fonctions mathématiques &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)  

-   Fonctions de date : CURRENT_TIMESTAMP, DATEADD, DATEDIFF, DATEFROMPARTS, DATEPART, DATETIME2FROMPARTS, DATETIMEFROMPARTS, DAY, EOMONTH, GETDATE, GETUTCDATE, MONTH, SMALLDATETIMEFROMPARTS, SYSDATETIME, SYSUTCDATETIME et YEAR.  

-   Fonctions de chaînes : LEN, LTRIM, RTRIM et SUBSTRING.  
    - **S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].  
      À compter de [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], les fonctions intégrées suivantes sont également prises en charge : TRIM, TRANSLATE et CONCAT_WS.  

-   Fonctions d’identité : SCOPE_IDENTITY  

-   Fonctions NULL : ISNULL  

-   Fonctions Uniqueidentifier : NEWID et NEWSEQUENTIALID  

-   Fonctions JSON  
    - **S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].  
      À partir de [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], les fonctions JSON sont prises en charge dans les modules compilés en mode natif.

-   Fonctions d’erreur : ERROR_LINE, ERROR_MESSAGE, ERROR_NUMBER, ERROR_PROCEDURE, ERROR_SEVERITY et ERROR_STATE  

-   Fonctions système : @@rowcount. Les instructions dans les procédures stockées compilées en mode natif mettent à jour @@rowcount et vous pouvez utiliser @@rowcount dans une procédure stockée compilée en mode natif pour déterminer le nombre de lignes affectées par la dernière instruction exécutée dans cette procédure stockée compilée en mode natif. Cependant, @@rowcount est réinitialisé à 0 au début et à la fin de l’exécution d’une procédure stockée compilée en mode natif.  

-   Fonctions de sécurité : IS_MEMBER({'group' | 'role'}), IS_ROLEMEMBER ('role' [, 'database_principal']), IS_SRVROLEMEMBER ('role' [, 'login']), ORIGINAL_LOGIN(), SESSION_USER, CURRENT_USER, SUSER_ID(['login']), SUSER_SID(['login'] [, Param2]), SUSER_SNAME([server_user_sid]), SYSTEM_USER, SUSER_NAME, USER, USER_ID(['user']), USER_NAME([id]), CONTEXT_INFO().

-   Les exécutions de modules natifs peuvent être imbriquées.

##  <a name="auditing"></a><a name="auditing"></a> Audit  
 L'audit au niveau de la procédure est pris en charge dans les procédures stockées compilées en mode natif.  

 Pour plus d'informations sur l'audit, consultez [Créer une spécification de l'audit du serveur et de la base de données](../../relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification.md).  

##  <a name="table-and-query-hints"></a><a name="tqh"></a> Indicateurs de requête et de table  
 Les constructions suivantes sont admises :  

-   Indicateurs INDEX, FORCESCAN et FORCESEEK, dans la syntaxe des indicateurs de table ou dans la [Clause OPTION &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md) de la requête. Pour plus d’informations, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  

-   FORCE ORDER  

-   Indicateur LOOP JOIN  

-   OPTIMIZE FOR  

 Pour plus d’informations, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  

##  <a name="limitations-on-sorting"></a><a name="los"></a> Limitations sur le tri  
 Vous pouvez trier plus de 8000 lignes dans une requête qui utilise [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md) et une [Clause ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md). Toutefois, sans [Clause ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md), [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md) peut trier jusqu’à 8000 lignes (moins s’il existe des jointures).  

 Si votre requête utilise à la fois l’opérateur [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md) et une [Clause ORDER BY&#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md), vous pouvez spécifier jusqu’à 8192 lignes pour l’opérateur TOP. Si vous spécifiez plus de 8 192 lignes, le message d’erreur suivant s’affiche : **Msg 41398, Niveau 16, État 1, Procédure *\<procedureName>* , Ligne *\<lineNumber>* L’opérateur TOP peut retourner au maximum 8 192 lignes ; *\<number>* demandées.**  

 Si vous n'avez pas de clause TOP, triez les lignes avec ORDER BY.  

 Si vous n'utilisez pas de clause ORDER BY, utilisez une valeur entière avec l'opérateur TOP.  

 Exemple avec TOP N = 8192 : Compiles  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    SELECT TOP 8192 ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 Exemple avec TOP N > 8192 : Fails to compile.  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    SELECT TOP 8193 ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 La limitation de 8192 lignes s'applique uniquement à `TOP N` où `N` est une constante, comme dans les exemples précédents.  Si `N` doit être supérieur à 8192, vous pouvez affecter la valeur à une variable et utiliser cette variable avec `TOP`.  

 Exemple utilisant une variable : Compiles  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    DECLARE @v int = 8193   
    SELECT TOP (@v) ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 **Limitations sur les lignes retournées :** il existe deux cas de figure qui peuvent potentiellement réduire le nombre de lignes retournées par l’opérateur TOP :  

-   L'utilisation de JOINs dans la requête.  L'impact de JOINs sur une limitation dépend du plan de requête.  

-   L'utilisation de fonctions d'agrégation ou de références à des fonctions d'agrégation dans la clause ORDER BY.  

 La formule pour calculer la valeur maximale N prise en charge dans le pire des cas dans TOP est : `N = floor ( 65536 / number_of_tables * 8 + total_size+of+aggs )`.  

## <a name="see-also"></a>Voir aussi  
 [Procédures stockées compilées en mode natif](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [Problèmes de migration pour les procédures stockées compilées en mode natif](./a-guide-to-query-processing-for-memory-optimized-tables.md)