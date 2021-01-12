---
description: DELETE (Transact-SQL)
title: DELETE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DELETE
- DELETE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row removal [SQL Server]
- DELETE statement [SQL Server], about DELETE statement
- views [SQL Server], deleting rows
- removing rows
- tables [SQL Server], deleting rows
- DELETE statement [SQL Server]
- deleting rows
- row removal [SQL Server], DELETE statement
- deleting data
ms.assetid: ed6b2105-0f35-408f-ba51-e36ade7ad5b2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b034dd3203324154d1a2742615df0f916916bfdc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100835"
---
# <a name="delete-transact-sql"></a>DELETE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Supprime une ou plusieurs lignes d’une table ou d’une vue dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ WITH <common_table_expression> [ ,...n ] ]  
DELETE   
    [ TOP ( expression ) [ PERCENT ] ]   
    [ FROM ]   
    { { table_alias  
      | <object>   
      | rowset_function_limited   
      [ WITH ( table_hint_limited [ ...n ] ) ] }   
      | @table_variable  
    }  
    [ <OUTPUT Clause> ]  
    [ FROM table_source [ ,...n ] ]   
    [ WHERE { <search_condition>   
            | { [ CURRENT OF   
                   { { [ GLOBAL ] cursor_name }   
                       | cursor_variable_name   
                   }   
                ]  
              }  
            }   
    ]   
    [ OPTION ( <Query Hint> [ ,...n ] ) ]   
[; ]  
  
<object> ::=  
{   
    [ server_name.database_name.schema_name.   
      | database_name. [ schema_name ] .   
      | schema_name.  
    ]  
    table_or_view_name   
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics

[ WITH <common_table_expression> [ ,...n ] ] 
DELETE [database_name . [ schema ] . | schema. ] table_name  
FROM [database_name . [ schema ] . | schema. ] table_name 
JOIN {<join_table_source>}[ ,...n ]  
ON <join_condition>
[ WHERE <search_condition> ]   
[ OPTION ( <query_options> [ ,...n ]  ) ]  
[; ]  

<join_table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [ <tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
}  
```

```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
DELETE 
    [ FROM [database_name . [ schema ] . | schema. ] table_name ]   
    [ WHERE <search_condition> ]   
    [ OPTION ( <query_options> [ ,...n ]  ) ]  
[; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 WITH \<common_table_expression>  
 Spécifie l'ensemble de résultats nommé temporaire, également appelé « expression de table commune », défini dans l'étendue de l'instruction DELETE. Le jeu de résultats est dérivé d'une instruction SELECT.  
  
 Les expressions de table communes peuvent également s'utiliser avec les instructions SELECT, INSERT, UPDATE et CREATE VIEW. Pour plus d’informations, consultez [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 TOP **(** _expression_ **)** [ PERCENT ]  
 Spécifie le nombre ou le pourcentage de lignes aléatoires qui seront supprimées. L'argument *expression* peut être un nombre ou un pourcentage de lignes. Les lignes référencées dans l'expression TOP utilisée dans les instructions INSERT, UPDATE ou DELETE ne sont pas triées dans un ordre précis. Pour plus d’informations, consultez [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
 FROM  
 Mot clé facultatif qui peut être inséré entre le mot clé DELETE et le *table_or_view_name* cible *rowset_function_limited*.  
  
 *table_alias*  
 Alias spécifié dans la clause FROM *table_source* représentant la table ou la vue de laquelle les lignes doivent être supprimées.  
  
 *server_name*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Nom du serveur (utilisant un nom de serveur lié ou la fonction [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) comme nom de serveur) sur lequel se trouve la table ou la vue. Si *server_name* est spécifié, *database_name* et *schema_name* sont obligatoires.  
  
 *database_name*  
 Nom de la base de données.  
  
 *schema_name*  
 Nom du schéma auquel appartient la vue ou la table.  
  
 *table_or_view_name*  
 Nom de la table ou de la vue à partir desquelles les lignes doivent être supprimées.  
  
 Une variable de table, dans son étendue, est également utilisable comme source de la table dans une instruction DELETE.  
  
 La vue référencée par *table_or_view_name* doit pouvoir être mise à jour et faire référence à une seule table de base dans la clause FROM de la définition de la vue. Pour plus d’informations sur les vues pouvant être mises à jour, consultez [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 *rowset_function_limited*  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Fonction [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) ou [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md), selon les capacités du fournisseur.  
  
 WITH **(** \<table_hint_limited> [... *n*] **)**  
 Spécifie un ou plusieurs indicateurs de table autorisés pour une table cible. Le mot clé WITH et les parenthèses sont obligatoires. NOLOCK et READUNCOMMITTED ne sont pas autorisés. Pour plus d’informations sur les indicateurs de table, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 \<OUTPUT_Clause>  
 Retourne des lignes supprimées ou des expressions basées sur ces lignes au cours de l'exécution de l'opération DELETE. Aucune instruction DML ciblant des vues ou des tables distantes ne prend en charge la clause OUTPUT. Pour plus d’informations sur les arguments et le comportement de cette clause, consultez [Clause OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
 FROM *table_source*  
 Spécifie une clause FROM supplémentaire. Cette extension [!INCLUDE[tsql](../../includes/tsql-md.md)] de l’instruction DELETE permet de spécifier des données de \<table_source> et de supprimer les lignes correspondantes de la table référencée dans la première clause FROM.  
  
 Cette extension, qui spécifie une jointure, peut être utilisée dans la clause WHERE à la place d'une sous-requête pour identifier les lignes à supprimer.  
  
 Pour plus d’informations, consultez [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
 WHERE  
 Spécifie les conditions limitant le nombre de lignes à supprimer. Si une clause WHERE n'est pas spécifiée, DELETE supprime toutes les lignes de la table.  
  
 Il existe deux types d'opérations de suppression, en fonction des conditions définies dans la clause WHERE :  
  
-   Une suppression par recherche spécifie une condition de recherche permettant de désigner les lignes à supprimer. Par exemple, WHERE *column_name* = *value*.  
  
-   Une suppression positionnée utilise la clause CURRENT OF pour spécifier un curseur. La suppression a lieu à la position actuelle du curseur. Cette opération peut être plus précise qu’une instruction DELETE élaborée qui utilise une clause WHERE *search_condition* pour spécifier les lignes à supprimer. Une instruction DELETE par recherche supprime plusieurs lignes si la condition de recherche n'identifie pas de façon univoque une ligne unique.  
  
\<search_condition>  
 Spécifie les conditions de limitation applicables aux lignes à supprimer. Le nombre de prédicats inclus dans une condition de recherche est illimité. Pour plus d’informations, consultez [Condition de recherche &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
 CURRENT OF  
 Spécifie que l'instruction DELETE s'effectue à l'emplacement actuel du curseur spécifié.  
  
 GLOBAL  
 Indique que *cursor_name* fait référence à un curseur global.  
  
 *cursor_name*  
 Nom du curseur ouvert à partir duquel a lieu l'extraction. Si un curseur global et un curseur local portent tous les deux le nom *cursor_name*, cet argument fait référence au curseur global si GLOBAL est spécifié, et au curseur local dans tous les autres cas. Le curseur doit pouvoir gérer les mises à jour.  
  
 *cursor_variable_name*  
 Nom d'une variable curseur. La variable de curseur doit référencer un curseur qui autorise les mises à jour.  
  
 OPTION **(** \<query_hint> [ **,** ... *n*] **)**  
 Mots clés spécifiant les indicateurs d’optimiseur utilisés pour personnaliser le traitement de l’instruction par le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Pour plus d’informations, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="best-practices"></a>Bonnes pratiques  
 Pour supprimer toutes les lignes dans une table, utilisez TRUNCATE TABLE. TRUNCATE TABLE est plus rapide et utilise moins de ressources du système et du journal des transactions que l'instruction DELETE. TRUNCATE TABLE présente des restrictions, par exemple, la table ne peut pas participer à la réplication. Pour plus d’informations, consultez [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md).  
  
 Utilisez la fonction @@ROWCOUNT pour retourner le nombre de lignes supprimées à l’application cliente. Pour plus d’informations, consultez [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md).  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Vous pouvez implémenter la gestion des erreurs pour l’instruction DELETE en spécifiant cette dernière dans une construction TRY...CATCH.  
  
 L'instruction DELETE peut échouer si elle viole un déclencheur ou si elle essaie de supprimer une ligne référencée par des données dans une autre table avec une contrainte FOREIGN KEY. Si l'instruction DELETE supprime plusieurs lignes et qu'une de ces lignes viole un déclencheur ou une contrainte, une erreur est retournée et aucune ligne n'est supprimée.  
  
 Quand une instruction DELETE rencontre une erreur arithmétique (erreur de dépassement, de division par zéro ou de domaine) qui se produit lors de l’évaluation de l’expression, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] gère ces erreurs comme si SET ARITHABORT avait la valeur ON. Le reste du lot est annulé et un message d'erreur est retourné.  
  
## <a name="interoperability"></a>Interopérabilité  
 DELETE peut s'utiliser dans le corps d'une fonction définie par l'utilisateur si l'objet modifié est une variable de table.  
  
 Lorsque vous supprimez une ligne qui contient une colonne FILESTREAM, vous supprimez également ses fichiers de système de fichiers sous-jacents. Les fichiers sous-jacents sont supprimés par le garbage collector FILESTREAM. Pour plus d’informations, consultez [Accéder aux données FILESTREAM avec Transact-SQL](../../relational-databases/blob/access-filestream-data-with-transact-sql.md).  
  
 Il n'est pas possible de spécifier la clause FROM dans une instruction DELETE qui référence directement ou indirectement une vue sur laquelle est défini le déclencheur INSTEAD OF. Pour plus d’informations sur les déclencheurs INSTEAD OF, consultez [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Lorsque TOP est utilisé avec DELETE, les lignes référencées ne sont pas réorganisées dans un ordre particulier et la clause ORDER BY ne peut pas être spécifiée directement dans cette instruction. Si vous devez utiliser une clause TOP pour supprimer des lignes dans un ordre chronologique significatif, vous devez associer à cette clause TOP une clause ORDER BY dans une instruction de sous-sélection. Consultez la section Exemples plus loin dans cette rubrique.  
  
 TOP ne peut pas être utilisé dans une instruction DELETE avec des vues partitionnées.  
  
## <a name="locking-behavior"></a>Comportement de verrouillage  
 Par défaut, une instruction DELETE acquiert toujours un verrou intentionnel exclusif (IX) sur l’objet table qu’elle modifie et maintient ce verrou jusqu’à la fin de la transaction. Un verrou intentionnel exclusif (IX) empêche toute autre transaction de modifier les données ; les opérations de lecture ne peuvent avoir lieu qu’avec l’indicateur NOLOCK ou le niveau d’isolation « lecture non validée ». Vous pouvez spécifier des indicateurs de table pour remplacer ce comportement par défaut pour la durée de l'instruction DELETE en spécifiant une autre méthode de verrouillage ; toutefois, nous vous recommandons de ne recourir aux indicateurs qu'en dernier ressort et seulement si vous êtes un développeur ou un administrateur de base de données expérimenté. Pour plus d’informations, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Lors de la suppression de lignes d’un segment de mémoire, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut utiliser le verrouillage de ligne ou de page pour l’opération. Par conséquent, les pages rendues vides par l'opération de suppression demeurent allouées au segment de mémoire. Lorsque des pages vides ne sont pas désallouées, l'espace associé ne peut pas être réutilisé par d'autres objets dans la base de données.  
  
 Pour supprimer des lignes d'un segment de mémoire et désallouer des pages, utilisez l'une des méthodes suivantes.  
  
-   Spécifiez l'indicateur TABLOCK dans l'instruction DELETE. Grâce à l'indicateur TABLOCK, l'opération de suppression utilise un verrou exclusif sur la table au lieu d'un verrou de ligne ou de page. Les pages sont ainsi désallouées. Pour plus d’informations sur l’indicateur TABLOCK, consultez [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
-   Utilisez `TRUNCATE TABLE` si toutes les lignes doivent être supprimées de la table.  
  
-   Créez un index cluster sur le segment de mémoire avant de supprimer les lignes. Vous pouvez supprimer l'index cluster après avoir supprimé les lignes. Cette méthode prend plus de temps que les méthodes précédentes et utilise davantage de ressources temporaires.  
  
> [!NOTE]  
>  Les pages vides peuvent être supprimées d’un segment de mémoire à tout moment à l’aide de l’instruction `ALTER TABLE <table_name> REBUILD`.  
  
## <a name="logging-behavior"></a>Comportement de journalisation  
L'instruction DELETE est toujours entièrement journalisée.  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Les autorisations `DELETE` sont obligatoires sur la table cible. Des autorisations `SELECT` sont également requises si l’instruction comporte une clause WHERE.  
  
 Les autorisations DELETE sont attribuées par défaut aux membres du rôle serveur fixe `sysadmin`, aux membres des rôles de base de données fixes `db_owner` et `db_datawriter` ainsi qu’au propriétaire de la table. Les membres des rôles `sysadmin`, `db_owner` et `db_securityadmin` ainsi que le propriétaire de la table peuvent transférer des autorisations à d’autres utilisateurs.  
  
## <a name="examples"></a>Exemples  
  
|Category|Éléments syntaxiques proposés|  
|--------------|------------------------------|  
|[Syntaxe de base](#BasicSyntax)|Suppression|  
|[Limitation des lignes supprimées](#LimitRows)|WHERE • FROM • curseur •|  
|[Suppression de lignes dans une table distante](#RemoteTables)|Serveur lié • fonction d'ensemble de lignes OPENQUERY • fonction d'ensemble de lignes OPENDATASOURCE|  
|[Capture des résultats de l’instruction DELETE](#CaptureResults)|Clause OUTPUT|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Syntaxe de base  
 Les exemples fournis dans cette section présentent les fonctionnalités de base de l'instruction DELETE en utilisant la syntaxe minimale requise.  
  
#### <a name="a-using-delete-with-no-where-clause"></a>R. Utilisation de DELETE sans clause WHERE  
 L'exemple suivant supprime toutes les lignes de la table `SalesPersonQuotaHistory` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] , car aucune clause WHERE ne limite le nombre de lignes supprimées.  
  
```sql
DELETE FROM Sales.SalesPersonQuotaHistory;  
GO  
```  
  
###  <a name="limiting-the-rows-deleted"></a><a name="LimitRows"></a> Limitation des lignes supprimées  
 Les exemples de cette section montrent comment limiter le nombre de lignes qui seront supprimées.  
  
#### <a name="b-using-the-where-clause-to-delete-a-set-of-rows"></a>B. Utilisation de la clause WHERE pour supprimer un jeu de lignes  
 L’exemple suivant supprime toutes les lignes de la table `ProductCostHistory` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] dans lesquelles la valeur de la colonne `StandardCost` est supérieure à `1000.00`.  
  
```sql
DELETE FROM Production.ProductCostHistory  
WHERE StandardCost > 1000.00;  
GO  
```  
  
 L'exemple suivant illustre une clause WHERE plus complexe. La clause WHERE définit deux conditions qui doivent être rencontrées pour déterminer les lignes à supprimer. La valeur dans la colonne `StandardCost` doit être comprise entre `12.00` et `14.00` , tandis que la valeur dans la colonne `SellEndDate` doit être Null. L’exemple imprime également la valeur de la fonction **\@\@ROWCOUNT** afin de retourner le nombre de lignes supprimées.  
  
```sql
DELETE Production.ProductCostHistory  
WHERE StandardCost BETWEEN 12.00 AND 14.00  
      AND EndDate IS NULL;  
PRINT 'Number of rows deleted is ' + CAST(@@ROWCOUNT as char(3));  
```  
  
#### <a name="c-using-a-cursor-to-determine-the-row-to-delete"></a>C. Utilisation d'un curseur pour déterminer la ligne à supprimer  
 L’exemple suivant supprime une seule ligne de la table `EmployeePayHistory` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] en utilisant un curseur nommé `complex_cursor`. La suppression est appliquée uniquement à la ligne actuellement extraite à partir du curseur.  
  
```sql
DECLARE complex_cursor CURSOR FOR  
    SELECT a.BusinessEntityID  
    FROM HumanResources.EmployeePayHistory AS a  
    WHERE RateChangeDate <>   
         (SELECT MAX(RateChangeDate)  
          FROM HumanResources.EmployeePayHistory AS b  
          WHERE a.BusinessEntityID = b.BusinessEntityID) ;  
OPEN complex_cursor;  
FETCH FROM complex_cursor;  
DELETE FROM HumanResources.EmployeePayHistory  
WHERE CURRENT OF complex_cursor;  
CLOSE complex_cursor;  
DEALLOCATE complex_cursor;  
GO  
```  
  
#### <a name="d-using-joins-and-subqueries-to-data-in-one-table-to-delete-rows-in-another-table"></a>D. Utilisation de jointures et de sous-requêtes sur les données d'une table pour supprimer des lignes dans une autre table  
 Les exemples suivants illustrent deux façons de supprimer des lignes dans une table selon les données figurant dans une autre table. Dans les deux exemples, des lignes de la table `SalesPersonQuotaHistory` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] sont supprimées en fonction des ventes de l’année stockées dans la table `SalesPerson`. La première instruction `DELETE` montre une solution de sous-requête compatible ISO tandis que la seconde instruction `DELETE` montre l’extension [!INCLUDE[tsql](../../includes/tsql-md.md)] FROM pour joindre les deux tables.  
  
```sql
-- SQL-2003 Standard subquery  
  
DELETE FROM Sales.SalesPersonQuotaHistory   
WHERE BusinessEntityID IN   
    (SELECT BusinessEntityID   
     FROM Sales.SalesPerson   
     WHERE SalesYTD > 2500000.00);  
GO  
```  
  
```sql
-- Transact-SQL extension  
  
DELETE FROM Sales.SalesPersonQuotaHistory   
FROM Sales.SalesPersonQuotaHistory AS spqh  
INNER JOIN Sales.SalesPerson AS sp  
ON spqh.BusinessEntityID = sp.BusinessEntityID  
WHERE sp.SalesYTD > 2500000.00;  
GO  
```  
  
```sql
-- No need to mention target table more than once.  
  
DELETE spqh  
  FROM  
        Sales.SalesPersonQuotaHistory AS spqh  
    INNER JOIN Sales.SalesPerson AS sp  
        ON spqh.BusinessEntityID = sp.BusinessEntityID  
  WHERE  sp.SalesYTD > 2500000.00;  
```  
  
#### <a name="e-using-top-to-limit-the-number-of-rows-deleted"></a>E. Utilisation de TOP pour limiter le nombre de lignes supprimées  
 Quand une clause TOP (*n*) est utilisée avec DELETE, l’opération de suppression est effectuée sur une sélection aléatoire de *n* lignes. L’exemple suivant supprime de manière aléatoire `20` lignes de la table `PurchaseOrderDetail` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] dont la date d’échéance est antérieure au 1er juillet 2006.  
  
```sql
DELETE TOP (20)   
FROM Purchasing.PurchaseOrderDetail  
WHERE DueDate < '20020701';  
GO  
```  
  
 Si vous devez utiliser une clause TOP pour supprimer des lignes dans un ordre chronologique significatif, vous devez associer à cette clause TOP une clause ORDER BY dans une instruction de sous-sélection. La requête suivante supprime les 10 lignes de la table `PurchaseOrderDetail` dont la date d'expiration est la plus proche. Pour garantir que seules 10 lignes sont supprimées, la colonne spécifiée dans l'instruction de sous-sélection (`PurchaseOrderID`) constitue la clé primaire de la table. L'utilisation d'une colonne non-clé dans l'instruction de sous-sélection peut entraîner la suppression de plus de 10 lignes si la colonne spécifiée contient des valeurs dupliquées.  
  
```sql
DELETE FROM Purchasing.PurchaseOrderDetail  
WHERE PurchaseOrderDetailID IN  
   (SELECT TOP 10 PurchaseOrderDetailID   
    FROM Purchasing.PurchaseOrderDetail   
    ORDER BY DueDate ASC);  
GO  
```  
  
###  <a name="deleting-rows-from-a-remote-table"></a><a name="RemoteTables"></a> Suppression de lignes dans une table distante  
 Les exemples présentés dans cette section montrent comment supprimer des lignes dans une table distante en utilisant un [serveur lié](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) ou une [fonction d’ensemble de lignes](../functions/opendatasource-transact-sql.md) pour référencer la table distante. Une table distante existe sur un serveur différent ou une instance différente de SQL Server.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
#### <a name="f-deleting-data-from-a-remote-table-by-using-a-linked-server"></a>F. Suppression de données dans une table distante en utilisant un serveur lié  
 L'exemple ci-dessous supprime des lignes dans une table distante. L’exemple commence par créer un lien vers la source de données distante en utilisant [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). Le nom du serveur lié, `MyLinkServer`, est ensuite spécifié comme partie du nom d’objet en quatre parties qui se présente sous la forme *serveur.catalogue.schéma.objet*.  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' or 'server_name\instance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  
```  
  
```sql
-- Specify the remote data source using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
DELETE MyLinkServer.AdventureWorks2012.HumanResources.Department 
WHERE DepartmentID > 16;  
GO  
```  
  
#### <a name="g-deleting-data-from-a-remote-table-by-using-the-openquery-function"></a>G. Suppression de données dans une table distante en utilisant la fonction OPENQUERY  
 L’exemple suivant supprime des lignes d’une table distante en spécifiant la fonction d’ensemble de lignes [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md). Le nom de serveur lié créé dans l'exemple précédent est utilisé dans cet exemple.  
  
```sql
DELETE OPENQUERY (MyLinkServer, 'SELECT Name, GroupName 
FROM AdventureWorks2012.HumanResources.Department  
WHERE DepartmentID = 18');  
GO  
```  
  
#### <a name="h-deleting-data-from-a-remote-table-by-using-the-opendatasource-function"></a>H. Suppression de données dans une table distante en utilisant la fonction OPENDATASOURCE  
 L’exemple suivant supprime des lignes d’une table distante en spécifiant la fonction d’ensemble de lignes [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md). Spécifiez un nom de serveur valide pour la source de données en utilisant le format *server_name* ou *server_name\instance_name*.  
  
```sql
DELETE FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source= <server_name>; Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department   
WHERE DepartmentID = 17;
```  
  
###  <a name="capturing-the-results-of-the-delete-statement"></a><a name="CaptureResults"></a> Capture des résultats de l’instruction DELETE  
  
#### <a name="i-using-delete-with-the-output-clause"></a>I. Utilisation de DELETE avec la clause OUTPUT  
 L’exemple suivant montre comment enregistrer les résultats d’une instruction `DELETE` dans une variable de table dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
DELETE Sales.ShoppingCartItem  
OUTPUT DELETED.*   
WHERE ShoppingCartID = 20621;  
  
--Verify the rows in the table matching the WHERE clause have been deleted.  
SELECT COUNT(*) AS [Rows in Table] 
FROM Sales.ShoppingCartItem 
WHERE ShoppingCartID = 20621;  
GO  
```  
  
#### <a name="j-using-output-with-from_table_name-in-a-delete-statement"></a>J. Utilisation de la clause OUTPUT avec <from_table_name> dans une instruction DELETE  
 L’exemple suivant supprime des lignes de la table `ProductProductPhoto` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] en fonction de critères de recherche définis dans la clause `FROM` de l’instruction `DELETE`. La clause `OUTPUT` retourne les colonnes `DELETED.ProductID`, `DELETED.ProductPhotoID`de la table en cours de suppression et les colonnes de la table `Product` . Cette méthode s'utilise dans la clause `FROM` pour spécifier les lignes à supprimer.  
  
```sql
DECLARE @MyTableVar table (  
    ProductID int NOT NULL,   
    ProductName nvarchar(50)NOT NULL,  
    ProductModelID int NOT NULL,   
    PhotoID int NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
    WHERE p.ProductModelID BETWEEN 120 and 130;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, ProductModelID, PhotoID   
FROM @MyTableVar  
ORDER BY ProductModelID;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="k-delete-all-rows-from-a-table"></a>K. Supprimer toutes les lignes d’une table  
 L'exemple suivant supprime toutes les lignes de la table `Table1`car aucune clause WHERE ne limite le nombre de lignes supprimées.  
  
```sql
DELETE FROM Table1;  
```  
  
### <a name="l-delete-a-set-of-rows-from-a-table"></a>L. Supprimer (DELETE) un ensemble de lignes d’une table

 L’exemple suivant supprime toutes les lignes de la table `Table1` dont la valeur est supérieure à 1000.00 dans la colonne `StandardCost`.  
  
```sql
DELETE FROM Table1  
WHERE StandardCost > 1000.00;  
```  
  
### <a name="m-using-label-with-a-delete-statement"></a>M. Utilisation de LABEL avec une instruction DELETE

 L’exemple suivant utilise une étiquette avec l’instruction DELETE.  
  
```sql
DELETE FROM Table1  
OPTION ( LABEL = N'label1' );  
  
```  
  
### <a name="n-using-a-label-and-a-query-hint-with-the-delete-statement"></a>N. Utilisation d’une étiquette et d’un indicateur de requête avec l’instruction DELETE

 Cette requête présente la syntaxe de base pour utiliser un indicateur de jointure de requête avec l’instruction DELETE. Pour plus d’informations sur les indicateurs de jointure et la clause OPTION, voir [Clause OPTION (Transact-SQL)](../queries/option-clause-transact-sql.md).
  
```sql
-- Uses AdventureWorks  
  
DELETE FROM dbo.FactInternetSales  
WHERE ProductKey IN (   
    SELECT T1.ProductKey FROM dbo.DimProduct T1   
    JOIN dbo.DimProductSubcategory T2  
    ON T1.ProductSubcategoryKey = T2.ProductSubcategoryKey  
    WHERE T2.EnglishProductSubcategoryName = 'Road Bikes' )  
OPTION ( LABEL = N'CustomJoin', HASH JOIN ) ;  
```  

### <a name="o-delete-using-a-where-clause"></a>O. Supprimer avec une clause WHERE

Cette requête montre comment supprimer avec une clause WHERE et non avec une clause FROM.

```sql
DELETE tableA WHERE EXISTS (
SELECT TOP 1 1 FROM tableB tb WHERE tb.col1 = tableA.col1
)
```

### <a name="p-delete-based-on-the-result-of-joining-with-another-table"></a>P. Supprimer en fonction du résultat de la jointure avec une autre table

Cet exemple montre comment supprimer d’une table en fonction du résultat de la jointure avec une autre table.

```sql
CREATE TABLE dbo.Table1   
    (ColA int NOT NULL, ColB decimal(10,3) NOT NULL);  
GO  

CREATE TABLE dbo.Table2   
    (ColA int PRIMARY KEY NOT NULL, ColB decimal(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0), (1, 20.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  

DELETE dbo.Table2   
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA)
    WHERE dboTable2.ColA = 1;  
```

## <a name="see-also"></a>Voir aussi

 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md)   
 [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md)  
  
