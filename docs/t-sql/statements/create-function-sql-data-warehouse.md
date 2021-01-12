---
description: CREATE FUNCTION (Azure Synapse Analytics)
title: CREATE FUNCTION (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 09/17/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 8cad1b2c-5ea0-4001-9060-2f6832ccd057
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 7a7c60b45397f0a06cb7543dad9d15fff107a15f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095762"
---
# <a name="create-function-azure-synapse-analytics"></a>CREATE FUNCTION (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Crée une fonction définie par l'utilisateur dans [!INCLUDE[ssSDW](../../includes/ssazuresynapse_md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Une fonction définie par l’utilisateur est une routine [!INCLUDE[tsql](../../includes/tsql-md.md)] qui accepte des paramètres, exécute une action, par exemple un calcul complexe, et retourne le résultat de cette action sous forme de valeur. Dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], la valeur de retour doit être une valeur scalaire (unique). Dans [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], CREATE FUNCTION peut retourner une table suivant la syntaxe des fonctions table inline (préversion) ou bien une valeur unique suivant la syntaxe des fonctions scalaires. Utilisez cette instruction pour créer une routine réutilisable, exploitable :  
  
-   dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] telles que SELECT ;  
  
-   dans des applications appelant la fonction ;  
  
-   dans la définition d'une autre fonction définie par l'utilisateur ;  
  
-   pour définir une contrainte CHECK sur une colonne ;  
  
-   pour remplacer une procédure stockée.  
  
-   Utiliser une fonction inline comme prédicat de filtre pour une stratégie de sécurité  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Transact-SQL Scalar Function Syntax  (in Azure Synapse Analytics and Parallel Data Warehouse)
CREATE FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    BEGIN   
        function_body   
        RETURN scalar_expression  
    END  
[ ; ]  
  
<function_option>::=   
{  
    [ SCHEMABINDING ]  
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]  
}  
```

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax (Preview in Azure Synapse Analytics only)
CREATE FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] parameter_data_type
    [ = default ] }
    [ ,...n ]
  ]
)
RETURNS TABLE
    [ WITH SCHEMABINDING ]
    [ AS ]
    RETURN [ ( ] select_stmt [ ) ]
[ ; ]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Arguments  
 *schema_name*  
 Nom du schéma auquel appartient la fonction définie par l'utilisateur.  
  
 *function_name*  
 Nom de la fonction définie par l'utilisateur. Les noms de fonctions doivent respecter les règles applicables aux identificateurs et doivent être uniques dans la base de données et pour son schéma.  
  
> [!NOTE]  
>  Les parenthèses sont requises après le nom de fonction même si aucun paramètre n'est spécifié.  
  
 @*parameter_name*  
 Paramètre dans la fonction définie par l'utilisateur. Un ou plusieurs paramètres peuvent être déclarés.  
  
 Une fonction peut comprendre au maximum 2 100 paramètres. La valeur de chaque paramètre déclaré doit être fournie par l'utilisateur lors de l'exécution de la fonction, sauf si vous définissez une valeur par défaut pour le paramètre.  
  
 Spécifiez un nom de paramètre en plaçant le signe @ comme premier caractère. Le nom de paramètre doit suivre les règles applicables aux identificateurs. Un paramètre étant local à une fonction, vous pouvez utiliser le même nom dans d'autres fonctions. Les paramètres ne peuvent que prendre la place de constantes ; ils ne peuvent pas être utilisés à la place de noms de tables, de colonnes ou d'autres objets de base de données.  
  
> [!NOTE]  
>  ANSI_WARNINGS n'est pas honoré lorsque vous transmettez des paramètres dans une procédure stockée, dans une fonction définie par l'utilisateur ou lorsque vous déclarez et définissez des variables dans une instruction par lot. Par exemple, si une variable est définie comme **char(3)** , puis réglée sur une valeur supérieure à trois caractères, les données sont tronquées à la taille définie et l’instruction INSERT ou UPDATE réussit.  
  
 *parameter_data_type*  
 Type de données du paramètre. Pour les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)], tous les types de données scalaires pris en charge dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] sont autorisés. Le type de données timestamp (rowversion) n’est pas un type pris en charge.  
  
 [ =*default* ]  
 Valeur par défaut pour le paramètre. Si une valeur *default* est définie, la fonction peut être exécutée sans spécifier de valeur pour ce paramètre.  
  
 Lorsque l'un des paramètres de la fonction possède une valeur par défaut, le mot clé DEFAULT doit être spécifié lors de l'appel de la fonction afin de récupérer la valeur par défaut. Ce comportement est différent de l'utilisation de paramètres avec des valeurs par défaut dans des procédures stockées pour lesquelles l'omission du paramètre implique également la prise en compte de la valeur par défaut.  
  
 *return_data_type*  
 Valeur de retour d'une fonction scalaire définie par l'utilisateur. Pour les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)], tous les types de données scalaires pris en charge dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] sont autorisés. Le type de données timestamp (rowversion) n’est pas un type pris en charge. Les types non scalaires cursor et table ne sont pas autorisés.  
  
 *function_body*  
 Série d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  function_body ne peut pas contenir d’instruction SELECT et ne peut pas référencer des données de la base de données.  function_body ne peut pas référencer des tables ou des vues. Le corps de la fonction peut appeler d’autres fonctions déterministes, mais ne peut pas appeler de fonctions non déterministes. 
  
 Dans les fonctions scalaires, *function_body* est une série d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui, ensemble, prennent une valeur scalaire.  
  
 *scalar_expression*  
 Indique la valeur scalaire retournée par la fonction scalaire.  

 *select_stmt* **S’APPLIQUE À** : Azure Synapse Analytics  
 Représente l’instruction SELECT unique qui définit la valeur de retour d’une fonction table inline (préversion).

 TABLE **S’APPLIQUE À** : Azure Synapse Analytics  
 Indique que la valeur de retour de la fonction table est une table. Seules des constantes et des @*local_variables* peuvent être passés aux fonctions table.

 Dans les fonctions table inline (préversion), la valeur de retour TABLE est définie par une instruction SELECT unique. Aucune variable retournée n'est associée à une fonction en ligne.
  
 **\<function_option>::=** 
  
 Spécifie que la fonction aura une ou plusieurs des options ci-dessous.  
  
 SCHEMABINDING  
 Indique que la fonction est liée aux objets de base de données auxquels elle fait référence. Si SCHEMABINDING est précisé, les objets de base ne peuvent pas être modifiés d'une manière susceptible d'affecter la définition de la fonction. Cette dernière doit d'ailleurs être modifiée ou supprimée au préalable pour supprimer les dépendances par rapport à l'objet qui doit être modifié.  
  
 La liaison de la fonction aux objets auxquels elle fait référence est supprimée uniquement lorsqu'une des actions suivantes se produit :  
  
-   La fonction est supprimée.  
  
-   La fonction est modifiée, avec l'instruction ALTER, sans spécification de l'option SCHEMABINDING.  
  
 Une fonction peut être liée au schéma uniquement si les conditions suivantes sont vérifiées :  
  
-   Toute fonction définie par l’utilisateur référencée par la fonction est également liée au schéma.  
  
-   Les fonctions et autres fonctions définies par l’utilisateur référencées par la fonction sont référencées à l’aide d’un nom en une ou deux parties.  
  
-   Seules les fonctions intégrées et les autres fonctions définies par l’utilisateur dans la même base de données peuvent être référencées dans le corps des fonctions définies par l’utilisateur.  
  
-   L'utilisateur qui exécute l'instruction CREATE FUNCTION dispose de l'autorisation REFERENCES pour les objets de base de données auxquels la fonction fait référence.  
  
 Pour supprimer SCHEMABINDING, utilisez ALTER  
  
 RETURNS NULL ON NULL INPUT | **CALLED ON NULL INPUT**  
 Spécifie l’attribut **OnNULLCall** d’une fonction scalaire. S'il n'est pas spécifié, l'argument CALLED ON NULL INPUT est implicite par défaut. Cela signifie que le corps de la fonction est exécuté même si la valeur NULL est transmise comme argument.  
  
## <a name="best-practices"></a>Meilleures pratiques  
 Si une fonction définie par l'utilisateur n'est pas créée avec la clause SCHEMABINDING, les modifications apportées aux objets sous-jacents peuvent affecter la définition de la fonction et produire des résultats inattendus en cas d'appel. Nous vous recommandons d'implémenter l'une des méthodes suivantes pour vous assurer que la fonction ne devient pas obsolète en raison des modifications apportées à ses objets sous-jacents :  
  
-   Spécifiez la clause WITH SCHEMABINDING lors de la création de la fonction. Vous vous assurez ainsi que les objets référencés dans la définition de la fonction ne peuvent pas être modifiés sauf si la fonction est également modifiée.  
  
## <a name="interoperability"></a>Interopérabilité  
 Les instructions suivantes sont valides dans une fonction scalaire :  
  
-   Instructions d'affectation  
  
-   Instructions de contrôle de flux à l'exception des instructions TRY...CATCH  
  
-   Instructions DECLARE définissant des variables de données locales.  

Dans une fonction table inline (préversion), une seule instruction SELECT est autorisée.
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Les fonctions définies par l'utilisateur ne permettent pas d'exécuter des actions qui modifient l'état des bases de données.  
  
 Les fonctions définies par l'utilisateur peuvent être imbriquées ; en d'autres termes, une fonction définie par l'utilisateur peut en appeler une autre. Le niveau d'imbrication est incrémenté lorsque la fonction appelée commence à s'exécuter, et décrémenté lorsque l'exécution est terminée. Les fonctions définies par l'utilisateur peuvent être imbriquées jusqu'à 32 niveaux. Le dépassement des niveaux d'imbrication maximum autorisés, provoque l'échec de la totalité de la chaîne de fonctions appelantes.   
  
## <a name="metadata"></a>Métadonnées  
 Cette section répertorie les vues de catalogue système que vous pouvez utiliser pour retourner des métadonnées sur les fonctions définies par l’utilisateur.  
  
 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) : Affiche la définition des fonctions définies par l’utilisateur [!INCLUDE[tsql](../../includes/tsql-md.md)]. Par exemple :  
  
```sql  
SELECT definition, type   
FROM sys.sql_modules AS m  
JOIN sys.objects AS o   
    ON m.object_id = o.object_id   
    AND type = ('FN');  
GO  
  
```  
  
 [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md) : Affiche des informations sur les paramètres définis dans les fonctions définies par l'utilisateur.  
  
 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) : Affiche les objets sous-jacents référencés par une fonction.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CREATE FUNCTION dans la base de données et l'autorisation ALTER sur le schéma dans lequel la fonction est en cours de création.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-a-scalar-valued-user-defined-function-to-change-a-data-type"></a>R. Utilisation d’une fonction scalaire définie par l’utilisateur pour changer un type de données  
 Cette fonction simple prend un type de données **int** comme entrée et retourne un type de données **decimal(10,2)** comme sortie.  
  
```sql  
CREATE FUNCTION dbo.ConvertInput (@MyValueIn int)  
RETURNS decimal(10,2)  
AS  
BEGIN  
    DECLARE @MyValueOut int;  
    SET @MyValueOut= CAST( @MyValueIn AS decimal(10,2));  
    RETURN(@MyValueOut);  
END;  
GO  
  
SELECT dbo.ConvertInput(15) AS 'ConvertedValue';  
```  

## <a name="examples-sssdwfull"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]  

### <a name="a-creating-an-inline-table-valued-function-preview"></a>R. Création d'une fonction table inline (préversion)
 L’exemple suivant crée une fonction table inline pour retourner des informations clés sur les modules, en filtrant selon le paramètre `objectType` . Il comprend une valeur par défaut permettant de retourner tous les modules lorsque la fonction est appelée avec le paramètre default. Cet exemple utilise certains des affichages catalogue système mentionnés dans [Métadonnées](#metadata).

```sql
CREATE FUNCTION dbo.ModulesByType(@objectType CHAR(2) = '%%')
RETURNS TABLE
AS
RETURN
(
    SELECT 
        sm.object_id AS 'Object Id',
        o.create_date AS 'Date Created',
        OBJECT_NAME(sm.object_id) AS 'Name',
        o.type AS 'Type',
        o.type_desc AS 'Type Description', 
        sm.definition AS 'Module Description'
    FROM sys.sql_modules AS sm  
    JOIN sys.objects AS o ON sm.object_id = o.object_id
    WHERE o.type like '%' + @objectType + '%'
);
GO
```
La fonction peut alors être appelée pour retourner tous les objets de vue (**V**) avec :
```sql
select * from dbo.ModulesByType('V');
```

### <a name="b-combining-results-of-an-inline-table-valued-function-preview"></a>B. Combinaison des résultats d'une fonction table inline (préversion)
 Cet exemple simple utilise la fonction table inline qui a été créée pour montrer comment ses résultats peuvent être combinés avec d’autres tables à l’aide de CROSS APPLY. Ici sont sélectionnées toutes les colonnes de sys.objects et des résultats de `ModulesByType` pour toutes les lignes correspondant à la colonne *type*. Pour plus d’informations sur APPLY, consultez [clause FROM plus JOIN, APPLY, PIVOT](../../t-sql/queries/from-transact-sql.md).

```sql
SELECT * 
FROM sys.objects o
CROSS APPLY dbo.ModulesByType(o.type);
GO
```
  
## <a name="see-also"></a>Voir aussi  
 [ALTER FUNCTION (SQL Server PDW)](/previous-versions/sql/)   
 [DROP FUNCTION (SQL Server PDW)](/previous-versions/sql/)  
  
  

