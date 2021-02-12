---
description: Sélectionner les lignes à migrer à l’aide d’une fonction de filtre (Stretch Database)
title: Sélectionner les lignes à migrer avec une fonction de filtre
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, predicates
- predicates for Stretch Database
- Stretch Database, inline table-valued functions
- inline table-valued functions for Stretch Database
ms.assetid: 090890ee-7620-4a08-8e15-d2fbc71dd12f
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 72162b4ed6339bcf7939e1903886170e3df36e63
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030894"
---
# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Sélectionner les lignes à migrer à l’aide d’une fonction de filtre (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Si vous stockez d’anciennes données dans une table séparée, vous pouvez configurer Stretch Database pour migrer la table entière. Par ailleurs, si votre table contient des données actuelles et anciennes, vous pouvez spécifier un prédicat de filtre pour sélectionner les lignes à migrer. Le prédicat de filtre est une fonction table incluse. Cet article explique comment écrire une fonction table incluse pour sélectionner des lignes à migrer.  
  
> [!IMPORTANT]
> Si vous fournissez une fonction de filtre qui fonctionne mal, la migration des données fonctionne mal également. Stretch Database applique la fonction de filtre à la table à l’aide de l’opérateur CROSS APPLY.  
  
 Si vous ne spécifiez pas de fonction de filtre, la table entière est migrée.  
  
 Quand vous exécutez l’Assistant Activer la base de données pour Stretch, vous pouvez migrer une table entière spécifier une fonction de filtre simple dans l’Assistant. Si vous souhaitez utiliser un autre type de fonction de filtre pour sélectionner les lignes à migrer, effectuez l’une des opérations suivantes.  
  
-   Quittez l’Assistant et exécutez l’instruction ALTER TABLE pour activer Stretch pour la table et pour spécifier une fonction de filtre.  
  
-   Exécutez l’instruction ALTER TABLE pour spécifier une fonction de filtre après avoir quitté l’Assistant.  
  
 La syntaxe ALTER TABLE pour ajouter une fonction est décrite plus loin dans cet article.  
  
## <a name="basic-requirements-for-the-filter-function"></a>Conditions de base pour la fonction de filtre  
 La fonction table incluse requise pour un prédicat de filtre Stretch Database ressemble à l’exemple suivant.  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE <predicate>  
```  
  
 Les paramètres de la fonction doivent être des identificateurs pour les colonnes de la table.  
  
 Une liaison de schéma est obligatoire pour empêcher la suppression ou la modification de colonnes utilisées par la fonction de filtre.  
  
### <a name="return-value"></a>Valeur retournée  
 Si la fonction retourne un résultat non vide, la ligne peut être migrée. Autrement, c’est-à-dire si la fonction ne renvoie aucun résultat, la ligne ne peut pas être migrée.  
  
### <a name="conditions"></a>Conditions  
 Le &lt;*prédicat*&gt; consiste en une condition, ou en plusieurs conditions jointes avec l’opérateur logique AND.  
  
```  
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]  
```  
  
 Chaque condition peut à son tour se composer d’une condition primitive ou de plusieurs conditions primitives jointes avec l’opérateur logique OR.  
  
```  
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]  
```  
  
### <a name="primitive-conditions"></a>Conditions primitives  
 Une condition primitive peut effectuer l’une des comparaisons suivantes.  
  
```  
<primitive_condition> ::=   
{  
<function_parameter> <comparison_operator> constant  
| <function_parameter> { IS NULL | IS NOT NULL }  
| <function_parameter> IN ( constant [ ,...n ] )  
}  
  
```  
  
-   Comparer un paramètre de fonction à une expression constante. Par exemple : `@column1 < 1000`.  
  
     Voici un exemple qui vérifie si la valeur d’une colonne *date* est &lt; 1/1/2016.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
    GO  
  
    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
-   Appliquez l’opérateur IS NULL ou IS NOT NULL à un paramètre de fonction.  
  
-   Utilisez l’opérateur IN pour comparer un paramètre de fonction à une liste de valeurs constantes.  
  
     Voici un exemple qui vérifie si la valeur d’une colonne *shipment_status*  est `IN (N'Completed', N'Returned', N'Cancelled')`.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
### <a name="comparison-operators"></a>Opérateurs de comparaison  
 Les opérateurs de comparaison suivants sont pris en charge.  
  
 `<, <=, >, >=, =, <>, !=, !<, !>`  
  
```  
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }  
```  
  
### <a name="constant-expressions"></a>Expressions de constantes  
 Les constantes utilisées dans une fonction de filtre peuvent être toute expression déterministe évaluable lors de la définition de la fonction. Les expressions constantes peuvent contenir les éléments suivants.  
  
-   littéraux. Par exemple : `N'abc', 123`.  
  
-   Expressions algébriques. Par exemple : `123 + 456`.  
  
-   Fonctions déterministes Par exemple : `SQRT(900)`.  
  
-   Conversions déterministes utilisant les fonctions CAST ou CONVERT. Par exemple : `CONVERT(datetime, '1/1/2016', 101)`.  
  
### <a name="other-expressions"></a>Autres expressions  
 Si, après remplacement des opérateurs BETWEEN et NOT BETWEEN par les expressions AND et OR équivalentes, la fonction obtenue est conforme aux règles décrites ici, vous pouvez utiliser les opérateurs BETWEEN et NOT BETWEEN.  
  
 Vous ne pouvez pas utiliser des sous-requêtes ou des fonctions non déterministes telles que RAND() ou GETDATE().  
  
## <a name="add-a-filter-function-to-a-table"></a>Ajouter une fonction de filtre à une table  
 Pour ajouter une fonction de filtre à une table, exécutez l’instruction **ALTER TABLE** et spécifiez une fonction table incluse existante comme valeur du paramètre **FILTER_PREDICATE** . Exemple :  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),  
    MIGRATION_STATE = <desired_migration_state>  
) )  
  
```  
  
 La liaison de la fonction à la table en tant que prédicat a les effets suivants.  
  
-   Lors de la migration de données suivante, seules les lignes pour lesquelles la fonction retourne une valeur non vide sont migrées.  
  
-   Les colonnes utilisées par la fonction sont liés au schéma. Vous ne pouvez pas modifier ces colonnes tant qu’une table utilise la fonction en tant que prédicat de filtre.  
  
 Vous ne pouvez pas supprimer la fonction table incluse tant qu’une table l’utilise en tant que prédicat de filtre. 

> [!TIP]
> Pour améliorer les performances de la fonction de filtre, créez un index sur les colonnes utilisées par la fonction.

 ### <a name="passing-column-names-to-the-filter-function"></a>Transmission de noms de colonnes à la fonction de filtre
 
 Quand vous affectez une fonction de filtre à une table, spécifiez les noms de colonnes transmis à la fonction de filtre avec un nom en une seule partie. Si vous spécifiez un nom en trois parties quand vous transmettez les noms de colonnes, les requêtes ultérieures sur la table Stretch échouent.

Par exemple, si vous spécifiez un nom de colonne en trois parties comme indiqué dans l’exemple suivant, l’instruction s’exécute avec succès, mais les requêtes ultérieures sur la table échouent.

```sql
ALTER TABLE SensorTelemetry 
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Au lieu de cela, spécifiez la fonction de filtre avec un nom de colonne en une partie, comme indiqué dans l’exemple suivant.

```sql
ALTER TABLE SensorTelemetry 
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```
  
## <a name="add-a-filter-function-after-running-the-wizard"></a><a name="addafterwiz"></a>Ajouter une fonction de filtre après avoir exécuté l’Assistant  
  
Si vous souhaitez utiliser une fonction que vous ne pouvez pas créer dans l’ **Assistant Activer la base de données pour Stretch** , vous pouvez exécuter l’instruction **ALTER TABLE** pour spécifier une fonction après avoir quitté l’Assistant. Toutefois, avant de pouvoir appliquer une fonction, vous devez arrêter la migration des données en cours et restaurer les données migrées. (Pour plus d’informations sur la raison pour laquelle ceci est nécessaire, consultez [Remplacer une fonction de filtre existante](#replacePredicate).)
  
1. Inversez le sens de la migration et restaurez les données déjà migrées. Vous ne pouvez pas annuler cette opération après son démarrage. Des frais sur Azure vous sont également facturés pour les transferts de données sortants. Pour plus d’informations, consultez [Tarification Azure](https://azure.microsoft.com/pricing/details/data-transfers/).  
  
    ```sql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  
  
2. Attendez que la migration se termine. Vous pouvez vérifier l’état dans **Surveillance de Stretch Database** dans SQL Server Management Studio, ou vous pouvez interroger l’affichage **sys.dm_db_rda_migration_status** . Pour plus d’informations, consultez [Résoudre les problèmes liés à la migration des données](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) ou [sys.dm_db_rda_migration_status](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md).  
  
3. Créez la fonction de filtre que vous souhaitez appliquer à la table.  
  
4. Ajoutez la fonction à la table et redémarrez la migration des données vers Azure.  
  
    ```sql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
            );   
    ```  
  
## <a name="filter-rows-by-date"></a>Filtrer des lignes par date  
 L’exemple suivant migre les lignes dans lesquelles la colonne **date** contient une valeur antérieure au 1er janvier 2016.  
  
```sql  
-- Filter by date  
--  
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)  
GO  
  
```  
  
## <a name="filter-rows-by-the-value-in-a-status-column"></a>Filtrer des lignes par la valeur figurant dans la colonne d’état  
 L’exemple suivant migre les lignes donc la colonne d’ **état** contient les valeurs spécifiées.  
  
```sql  
-- Filter by status column  
--  
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')  
GO  
  
```  
  
## <a name="filter-rows-by-using-a-sliding-window"></a>Filtrer des lignes à l’aide d’une fenêtre glissante  
 Pour filtrer des lignes à l’aide d’une fenêtre glissante, n’oubliez pas les conditions suivantes applicables à la fonction de filtre.  
  
-   La fonction doit être déterministe. Par conséquent, vous ne pouvez pas créer une fonction qui recalcule automatiquement la fenêtre glissante au fil du temps.  
  
-   La fonction utilise la liaison de schéma. Ainsi, vous ne pouvez pas mettre à jour quotidiennement la fonction « sur place » en appelant l’instruction **ALTER FUNCTION** pour déplacer la fenêtre glissante.  
  
 Commencez avec une fonction de filtre comme dans l’exemple suivant, qui migre les lignes dans lesquelles la colonne **systemEndTime** contient une valeur antérieure au 1er janvier 2016.  
  
```sql  
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)   
RETURNS TABLE   
WITH SCHEMABINDING    
AS    
RETURN SELECT 1 AS is_eligible   
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;  
  
```  
  
 Appliquez la fonction de filtre à la table.  
  
```sql  
ALTER TABLE <table name>   
SET (   
        REMOTE_DATA_ARCHIVE = ON   
                (   
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101(SysEndTime)  
                                , MIGRATION_STATE = OUTBOUND   
                )  
        )   
;  
  
```  
  
 Lorsque vous souhaitez mettre à jour la fenêtre glissante, procédez comme suit.  
  
1.  Créez une fonction spécifiant la nouvelle fenêtre glissante. L’exemple suivant sélectionne les dates antérieures au 2 janvier 2016, au lieu du 1er janvier 2016.  
  
2.  Remplacez la fonction de filtre précédente par la nouvelle, en appelant l’instruction **ALTER TABLE**, comme illustré dans l’exemple suivant.  
  
3.  Vous pouvez également supprimer la fonction de filtre précédente que vous n’utilisez plus, en appelant l’instruction **DROP FUNCTION**. (Cette étape n’apparaît pas dans l’exemple).  
  
```sql  
BEGIN TRAN  
GO  
        /*(1) Create new predicate function definition */  
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)  
        RETURNS TABLE  
        WITH SCHEMABINDING   
        AS   
        RETURN SELECT 1 AS is_eligible  
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)  
        GO  
  
        /*(2) Set the new function as the filter predicate */  
        ALTER TABLE <table name>  
        SET   
        (  
               REMOTE_DATA_ARCHIVE = ON  
               (  
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),  
                       MIGRATION_STATE = OUTBOUND  
               )  
        )   
COMMIT ;  
  
```  
  
## <a name="more-examples-of-valid-filter-functions"></a>Autres exemples de fonctions de filtre valides  
  
-   L’exemple suivant combine deux conditions primitives à l’aide de l’opérateur logique AND.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),  
        MIGRATION_STATE = OUTBOUND  
    ) )  
  
    ```  
  
-   L’exemple suivant utilise plusieurs conditions et une conversion déterministe avec CONVERT.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101) AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL) AND @column3 IN (N'Completed', N'Returned', N'Cancelled')  
    GO  
  
    ```  
  
-   L’exemple suivant utilise des opérateurs et fonctions mathématiques.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < SQRT(400) + 10  
    GO  
  
    ```  
  
-   L’exemple suivant utilise les opérateurs BETWEEN et NOT BETWEEN. Cette utilisation est valide car, après remplacement des opérateurs BETWEEN et NOT BETWEEN par les expressions AND et OR équivalentes, la fonction obtenue est conforme aux règles décrites ici.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 BETWEEN 0 AND 100  
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)  
    GO  
  
    ```  
  
     La fonction qui précède est équivalente à la fonction suivante une fois que vous avez remplacé les opérateurs BETWEEN et NOT BETWEEN par les expressions AND et OR équivalentes.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 >= 0 AND @column1 <= 100 AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)  
    GO  
  
    ```  
  
## <a name="examples-of-filter-functions-that-arent-valid"></a>Exemples de fonctions de filtre non valides  
  
-   La fonction suivante n’est pas valide, car elle contient une conversion non déterministe.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < CONVERT(datetime, '1/1/2016')  
    GO  
  
    ```  
  
-   La fonction suivante n’est pas valide, car elle contient un appel de fonction non déterministe.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 < DATEADD(day, -60, GETDATE())  
    GO  
  
    ```  
  
-   La fonction suivante n’est pas valide, car elle contient une sous-requête.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example7(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct')  
    GO  
  
    ```  
  
-   Les fonctions suivantes ne sont pas valides, car des expressions utilisant des opérateurs algébriques ou des fonctions intégrées doivent prendre la valeur d’une constante lors de la définition de la fonction. Vous ne pouvez pas inclure des références de colonne dans les expressions algébriques ou les appels de fonction.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example8(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE @column1 % 2 =  0  
    GO  
  
    CREATE FUNCTION dbo.fn_example9(@column1 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE SQRT(@column1) = 30  
    GO  
  
    ```  
  
-   La fonction suivante n’est pas valide car, après remplacement de l’opérateur BETWEEN par l’expression AND équivalente, la fonction enfreint les règles décrites ici.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING  
    AS  
    RETURN  SELECT 1 AS is_eligible  
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000  
    GO  
  
    ```  
  
     La fonction précédente équivaut à la fonction suivante après remplacement de l’opérateur BETWEEN par l’expression AND équivalente. Cette fonction n’est pas valide, car des conditions primitives peuvent uniquement utiliser l’opérateur logique OR.  
  
    ```sql  
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)  
    RETURNS TABLE  
    WITH SCHEMABINDING   
    AS   
    RETURN  SELECT 1 AS is_eligible  
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000  
    GO  
  
    ```  
  
## <a name="how-stretch-database-applies-the-filter-function"></a>Comment Stretch Database applique la fonction de filtre  
 Stretch Database applique la fonction de filtre à la table, et détermine les lignes pouvant être migrées à l’aide de l’opérateur CROSS APPLY. Exemple :  
  
```sql  
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)  
```  
  
 Si la fonction retourne un résultat non vide pour la ligne, celle-ci peut être migrée.  
  
## <a name="replace-an-existing-filter-function"></a><a name="replacePredicate"></a>Remplacer une fonction de filtre existante  
 Vous pouvez remplacer une fonction de filtre spécifiée précédemment en réexécutant l’instruction **ALTER TABLE** et en spécifiant une nouvelle valeur pour le paramètre **FILTER_PREDICATE** . Exemple :  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),  
    MIGRATION_STATE = <desired_migration_state>  
    ) ) 
  
```  
  
 La nouvelle fonction table incluse requiert le respect des conditions suivantes.  
  
-   La nouvelle fonction doit être moins restrictive que la précédente.  
  
-   Tous les opérateurs qui existaient dans l’ancienne fonction doivent exister dans la nouvelle.  
  
-   La nouvelle fonction ne peut pas contenir d’opérateurs absents de l’ancienne fonction.  
  
-   L’ordre des arguments d’opérateur ne peut être modifié.  
  
-   Seules des valeurs constantes faisant partie d’une comparaison `<, <=, >, >=`  peuvent être modifiées d’une manière rendant la fonction moins restrictive.  
  
### <a name="example-of-a-valid-replacement"></a>Exemple de remplacement valide  
 Supposons que la fonction suivante est la fonction de filtre actuelle.  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate_old(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
GO  
  
```  
  
 La fonction suivante est un remplacement valide, car la nouvelle constante de date (qui spécifie une date limite postérieure) rend la fonction moins restrictive.  
  
```sql  
CREATE FUNCTION dbo.fn_stretchpredicate_new(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)  
            AND (@column2 < -50 OR @column2 > 50)  
GO  
  
```  
  
### <a name="examples-of-replacements-that-arent-valid"></a>Exemples de remplacements non valides  
 La fonction suivante n’est pas un remplacement valide, car la nouvelle constante de date (qui spécifie une date limite antérieure) ne rend pas la fonction moins restrictive.  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_1(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
GO  
  
```  
  
 La fonction suivante n’est pas un remplacement valide, car l’un des opérateurs de comparaison a été supprimé.  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_2(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -50)  
GO  
  
```  
  
 La fonction suivante n’est pas un remplacement valide, car une nouvelle condition a été ajoutée avec l’opérateur logique AND.  
  
```sql  
CREATE FUNCTION dbo.fn_notvalidreplacement_3(@column1 datetime, @column2 int)  
RETURNS TABLE  
WITH SCHEMABINDING   
AS   
RETURN  SELECT 1 AS is_eligible  
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)  
            AND (@column2 < -100 OR @column2 > 100)  
            AND (@column2 <> 0)  
GO  
  
```  
  
## <a name="remove-a-filter-function-from-a-table"></a>Supprimer une fonction de filtre d’une table  
 Pour migrer la table entière plutôt que des lignes sélectionnées, supprimez la fonction existante en affectant la valeur null à **FILTER_PREDICATE**  . Exemple :  
  
```sql  
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (  
    FILTER_PREDICATE = NULL,  
    MIGRATION_STATE = <desired_migration_state>  
) )  
  
```  
  
 Une fois la fonction de filtre supprimée, toutes les lignes de la table peuvent être migrées. Vous ne pouvez donc plus spécifier de fonction de filtre pour la même table par la suite, sauf si vous récupérez préalablement toutes les données distantes de la table à partir d’Azure. Cette restriction vise à éviter les situations où les lignes qui ne peuvent pas être migrées quand vous fournissez une nouvelle la fonction de filtre ont déjà été migrées vers Azure.  
  
## <a name="check-the-filter-function-applied-to-a-table"></a>Vérifier la fonction de filtre appliquée à une table  
 Pour vérifier la fonction de filtre appliquée à une table, ouvrez l’affichage catalogue **sys.remote_data_archive_tables** et vérifiez la valeur de la colonne **filter_predicate** . Si la valeur est null, la table entière peut être archivée. Pour plus d’informations, consultez [sys.remote_data_archive_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md).  
  
## <a name="security-notes-for-filter-functions"></a>Notes de sécurité pour les fonctions de filtre  
Un compte compromis avec des privilèges db_owner peut effectuer les opérations suivantes.  
  
-   Créer et appliquer une fonction table qui consomme de grandes quantités de ressources serveur ou attend pendant une période prolongée, ce qui entraîne un déni de service.  
  
-   Créer et appliquer une fonction table qui permet de déduire le contenu d’une table pour laquelle l’accès en lecture a été explicitement refusé à l’utilisateur.  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
  
