---
description: sp_describe_undeclared_parameters (Transact-SQL)
title: sp_describe_undeclared_parameters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_undeclared_parameters
- sp_describe_undeclared_parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_undeclared_parameters
ms.assetid: 6f016da6-dfee-4228-8b0d-7cd8e7d5a354
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 46dd9d66c73337920300162f331ee7a59b1e559f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200764"
---
# <a name="sp_describe_undeclared_parameters-transact-sql"></a>sp_describe_undeclared_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)] 

  Retourne un jeu de résultats qui contient des métadonnées sur les paramètres non déclarés dans un [!INCLUDE[tsql](../../includes/tsql-md.md)] lot. Considère chaque paramètre utilisé dans le lot **\@ tsql** , mais non déclaré dans les **\@ paramètres**. Le jeu de résultats retourné contient une ligne pour chaque paramètre de ce genre, avec les informations de type déduites pour ce paramètre. La procédure retourne un jeu de résultats vide si le lot d’entrée **\@ tsql** n’a pas de paramètres, à l’exception de ceux déclarés dans **\@ params**.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql
  
sp_describe_undeclared_parameters   
    [ @tsql = ] 'Transact-SQL_batch'   
    [ , [ @params = ] N'parameters' data type ] [, ...n]  
```  

> [!Note] 
> Pour utiliser cette procédure stockée dans Azure Synapse Analytics dans un pool SQL dédié, définissez le niveau de compatibilité de la base de données à 20 ou plus.   Pour vous désabonner, définissez le niveau de compatibilité de la base de données sur 10.

## <a name="arguments"></a>Arguments  
`[ \@tsql = ] 'Transact-SQL\_batch'` Une ou plusieurs [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions. *Transact-SQL_batch* peut être de type **nvarchar (**_n_**)** ou **nvarchar (max)**.  
  
`[ \@params = ] N'parameters'`\@params fournit une chaîne de déclaration pour les paramètres du [!INCLUDE[tsql](../../includes/tsql-md.md)] lot, de la même façon que sp_executesql fonctionne. Les *paramètres* peuvent être de type **nvarchar (**_n_**)** ou **nvarchar (max)**.  
  
 Est une chaîne qui contient les définitions de tous les paramètres qui ont été incorporés dans *Transact-SQL_batch*. Cette chaîne doit être une constante Unicode ou une variable Unicode. Chaque définition de paramètre se compose d'un nom de paramètre et d'un type de données. n correspond à un espace réservé pour d'autres définitions de paramètres. Si l’instruction ou le lot Transact-SQL dans l’instruction ne contient pas de paramètres, \@ params n’est pas obligatoire. La valeur par défaut de ce paramètre est NULL.  
  
 Datatype  
 Type de données du paramètre.  
  
## <a name="return-code-values"></a>Codet de retour  
 **sp_describe_undeclared_parameters** retourne toujours le statut de retour de zéro en cas de réussite. Si la procédure génère une erreur et que la procédure est appelée comme un RPC, l’état de retour est rempli par le type d’erreur, comme décrit dans la colonne error_type de sys.dm_exec_describe_first_result_set. Si la procédure est appelée depuis [!INCLUDE[tsql](../../includes/tsql-md.md)], la valeur de retour est toujours égale à zéro, même en cas d'erreur.  
  
## <a name="result-sets"></a>Jeux de résultats  
 **sp_describe_undeclared_parameters** retourne le jeu de résultats suivant.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**parameter_ordinal**|**int NOT NULL**|Contient la position ordinale du paramètre dans le jeu de résultats. La position du premier paramètre sera spécifiée comme 1.|  
|**name**|**sysname non NULL**|Contient le nom du paramètre.|  
|**suggested_system_type_id**|**int NOT NULL**|Contient le **system_type_id** du type de données du paramètre tel que spécifié dans sys. types.<br /><br /> Pour les types CLR, même si la colonne **system_type_name** retourne la valeur null, cette colonne retourne la valeur 240.|  
|**suggested_system_type_name**|**nvarchar (256) NULL**|Contient le nom du type de données. Inclut des arguments (tels que la longueur, la précision, l'échelle) spécifiés pour le type de données du paramètre. Si le type de données est un type d'alias défini par l'utilisateur, le type de système sous-jacent est spécifié ici. S'il s'agit d'un type de données CLR défini par l'utilisateur, NULL est retourné dans cette colonne. Si le type du paramètre ne peut pas être déduit, NULL est retourné.|  
|**suggested_max_length**|**smallint n’est pas NULL**|Consultez sys. Columns. pour **max_length** Description de la colonne.|  
|**suggested_precision**|**tinyint non NULL**|Consultez sys. Columns. pour la description de la colonne de précision.|  
|**suggested_scale**|**tinyint non NULL**|Consultez sys. Columns. pour la description de la colonne d'échelle.|  
|**suggested_user_type_id**|**int NULL**|Pour les types d'alias et CLR, contient l'information user_type_id du type de données de la colonne comme spécifié dans sys.types. Sinon, la valeur est NULL.|  
|**suggested_user_type_database**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom de la base de données dans laquelle le type est défini. Sinon, la valeur est NULL.|  
|**suggested_user_type_schema**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom du schéma dans lequel le type est défini. Sinon, la valeur est NULL.|  
|**suggested_user_type_name**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom du type. Sinon, la valeur est NULL.|  
|**suggested_assembly_qualified_type_name**|**nvarchar (4000) NULL**|Pour les types CLR, retourne le nom de l'assembly et de la classe qui définit le type. Sinon, la valeur est NULL.|  
|**suggested_xml_collection_id**|**int NULL**|Contient la xml_collection_id du type de données du paramètre tel que spécifié dans sys. Columns. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**suggested_xml_collection_database**|**sysname NULL**|Contient la base de données dans laquelle la collection de schémas XML associée à ce type est définie. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**suggested_xml_collection_schema**|**sysname NULL**|Contient le schéma dans lequel la collection de schémas XML associée à ce type est définie. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**suggested_xml_collection_name**|**sysname NULL**|Contient le nom de la collection de schémas XML associé à ce type. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**suggested_is_xml_document**|**bit NOT NULL**|Retourne 1 si le type qui est retourné est XML et que ce type est garanti être un document XML. Dans le cas contraire, retourne la valeur 0.|  
|**suggested_is_case_sensitive**|**bit NOT NULL**|Retourne 1 si la colonne est d'un type chaîne sensible à la casse et 0 si ce n'est pas le cas.|  
|**suggested_is_fixed_length_clr_type**|**bit NOT NULL**|Retourne 1 si la colonne est d'un type CLR de longueur fixe et 0 si ce n'est pas le cas.|  
|**suggested_is_input**|**bit NOT NULL**|Retourne 1 si le paramètre est utilisé n'importe où autre que le côté gauche d'une attribution. Dans le cas contraire, retourne la valeur 0.|  
|**suggested_is_output**|**bit NOT NULL**|Retourne 1 si le paramètre est utilisé du côté gauche d'une attribution ou est passé à un paramètre de sortie d'une procédure stockée. Dans le cas contraire, retourne la valeur 0.|  
|**formal_parameter_name**|**sysname NULL**|Si le paramètre est un argument d'une procédure stockée ou une fonction définie par l'utilisateur, retourne le nom du paramètre formel correspondant. Dans le cas contraire, la valeur NULL est retournée.|  
|**suggested_tds_type_id**|**int NOT NULL**|À usage interne uniquement.|  
|**suggested_tds_length**|**int NOT NULL**|À usage interne uniquement.|  
  
## <a name="remarks"></a>Notes  
 **sp_describe_undeclared_parameters** retourne toujours le statut de retour égal à zéro.  
  
 Le cas d'utilisation le plus courant est celui d'une application qui reçoit une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] pouvant contenir des paramètres et devant les traiter d'une certaine façon. Il peut s'agir, par exemple, d'une interface utilisateur (telle qu'ODBCTest ou RowsetViewer) où l'utilisateur fournit une requête avec une syntaxe de paramètre ODBC. L'application doit découvrir dynamiquement le nombre de paramètres et inviter l'utilisateur à fournir chacun d'eux.  
  
 Autre exemple : en l'absence d'entrée de la part de l'utilisateur, une application doit faire une boucle sur les paramètres et obtenir les données pour ces derniers depuis un autre emplacement (tel qu'une table). Dans ce cas, l'application ne doit pas passer l'ensemble des informations de paramètre à la fois. À la place, l'application peut obtenir toutes les informations de paramètres du fournisseur et obtenir les données proprement dites de la table. Le code qui utilise **sp_describe_undeclared_parameters** est plus générique et est moins susceptible d’être modifié si la structure de données change ultérieurement.  
  
 **sp_describe_undeclared_parameters** retourne une erreur dans l’un des cas suivants.  
  
-   Si l’entrée \@ tsql n’est pas un [!INCLUDE[tsql](../../includes/tsql-md.md)] lot valide. La validité est déterminée en analysant et en analysant le [!INCLUDE[tsql](../../includes/tsql-md.md)] lot. Les erreurs provoquées par le lot pendant l’optimisation de la requête ou lors de l’exécution ne sont pas prises en compte pour déterminer si le [!INCLUDE[tsql](../../includes/tsql-md.md)] lot est valide.  
  
-   Si \@ params n’a pas la valeur null et contient une chaîne qui n’est pas une chaîne de déclaration valide syntaxiquement pour les paramètres, ou si elle contient une chaîne qui déclare un paramètre plus d’une fois.  
  
-   Si le lot d’entrée [!INCLUDE[tsql](../../includes/tsql-md.md)] déclare une variable locale du même nom qu’un paramètre déclaré dans \@ params.  
  
- Si l’instruction fait référence à des tables temporaires.

- La requête inclut la création d'une table permanente qui est alors interrogée.
  
 Si \@ tsql n’a pas de paramètres, à l’exception de ceux déclarés dans \@ params, la procédure retourne un jeu de résultats vide.  
  
## <a name="parameter-selection-algorithm"></a>Algorithme de sélection du paramètre  
 Dans le cas d'une requête avec des paramètres non déclarés, la déduction de type de données pour les paramètres non déclarés s'effectue en trois étapes.  
  
 **Étape 1**  
  
 La première étape dans la déduction du type de données pour une requête avec des paramètres non déclarés consiste à rechercher les types de données de toutes les sous-expressions dont les types de données ne dépendent pas des paramètres non déclarés. Le type peut être déterminé pour les expressions suivantes :  
  
-   Colonnes, constantes, variables et paramètres déclarés.  
  
-   Résultats d'un appel à une fonction définie par l'utilisateur (UDF).  
  
-   Expression dont les types de données ne dépendent pas des paramètres non déclarés pour toutes les entrées.  
  
 Prenons l’exemple de la requête `SELECT dbo.tbl(@p1) + c1 FROM t1 WHERE c2 = @p2 + 2`. Les expressions dbo. tbl ( \@ P1) + C1 et C2 ont des types de données, et l’expression \@ P1 et \@ P2 + 2 ne le font pas.  
  
 Après cette étape, si une expression (autre qu'un appel à une fonction UDF) compte deux arguments sans types de données, la déduction du type se solde par une erreur. Les exemples suivants entraînent tous des erreurs :  
  
```sql
SELECT * FROM t1 WHERE @p1 = @p2  
SELECT * FROM t1 WHERE c1 = @p1 + @p2  
SELECT * FROM t1 WHERE @p1 = SUBSTRING(@p2, 2, 3)  
```  
  
 L'exemple suivant ne génère pas d'erreur :  
  
```sql
SELECT * FROM t1 WHERE @p1 = dbo.tbl(c1, @p2, @p3)  
```
  
 **Étape 2**  
  
 Pour un paramètre non déclaré donné \@ , l’algorithme de déduction du type recherche l’expression E (p) la plus profonde \@ qui contient \@ p et est l’un des éléments suivants :  
  
-   Argument d'une comparaison ou d'opérateur d'assignation.  
  
-   Argument d'une fonction définie par l'utilisateur (notamment une fonction UDF table), d'une procédure ou d'une méthode.  
  
-   Argument d’une clause **values** d’une instruction **Insert** .  
  
-   Argument d’un **Cast** ou **Convert**.  
  
 L’algorithme de déduction du type recherche un type de données cible TT ( \@ p) pour E ( \@ p). Les types de données cibles des exemples précédents sont les suivants :  
  
-   Type de données de l'autre côté de la comparaison ou de l'attribution.  
  
-   Type de données déclaré du paramètre auquel cet argument est passé.  
  
-   Type de données de la colonne dans laquelle cette valeur est insérée.  
  
-   Type de données vers lequel l'instruction effectue la conversion.  
  
 Prenons l’exemple de la requête `SELECT * FROM t WHERE @p1 = dbo.tbl(@p2 + c1)`. Puis E ( \@ P1) = \@ P1, e ( \@ P2) = \@ P2 + C1, TT ( \@ P1) est le type de données de retour déclaré de dbo. tbl, et TT ( \@ P2) est le type de données de paramètre déclaré pour dbo. tbl.  
  
 Si \@ p n’est pas contenu dans une expression indiquée au début de l’étape 2, l’algorithme de déduction du type détermine que E ( \@ p) est la plus grande expression scalaire qui contient \@ p, et l’algorithme de déduction du type ne calcule pas le type de données cible TT ( \@ p) pour E ( \@ p). Par exemple, si la requête est SELECT `@p + 2` , alors E ( \@ p) = \@ p + 2, et il n’y a pas de TT ( \@ p).  
  
 **Étape 3**  
  
 Maintenant que E ( \@ p) et TT ( \@ p) sont identifiés, l’algorithme de déduction du type déduit un type de données pour \@ p de l’une des deux manières suivantes :  
  
-   Déduction simple  
  
     Si E ( \@ p) = \@ p et TT ( \@ p) existe, par exemple, si \@ p est directement un argument pour l’une des expressions listées au début de l’étape 2, l’algorithme de déduction du type déduit le type de données de \@ p comme TT ( \@ p). Par exemple :  
  
    ```sql
    SELECT * FROM t WHERE c1 = @p1 AND @p2 = dbo.tbl(@p3)  
    ```  
  
     Le type de données pour \@ P1, \@ P2 et \@ P3 sera le type de données C1, le type de données de retour de dbo. tbl et le type de données de paramètre pour dbo. tbl respectivement.  
  
     Dans un cas particulier, si \@ p est un argument d’un \<, > \<=, or > opérateur, =, les règles de déduction simples ne s’appliquent pas. L'algorithme de la déduction du type utilisera les règles de déduction générales expliquées dans la section suivante. Par exemple, si c1 est une colonne de type de données char(30), considérez les deux requêtes suivantes :  
  
    ```sql
    SELECT * FROM t WHERE c1 = @p  
    SELECT * FROM t WHERE c1 > @p  
    ```  
  
     Dans le premier cas, l’algorithme de déduction du type déduit **char (30)** comme type de données pour \@ p selon les règles précédemment dans cette rubrique. Dans le deuxième cas, l’algorithme de déduction du type déduit **varchar (8000)** en fonction des règles de déduction générales dans la section suivante.  
  
-   Déduction générale  
  
     Si la déduction simple ne s'applique pas, les types de données suivants sont considérés pour les paramètres non déclarés :  
  
    -   Types de données Integer (**bit**, **tinyint**, **smallint**, **int**, **bigint**)  
  
    -   Types de données Money (**smallmoney**, **Money**)  
  
    -   Types de données à virgule flottante (**float**, **Real**)  
  
    -   **Numeric (38, 19)** -les autres types de données numériques ou décimaux ne sont pas pris en compte.  
  
    -   **varchar (8000)**, **varchar (max)**, **nvarchar (4000)** et **nvarchar (max)** -les autres types de données de chaîne (tels que **Text**, **char (8000)**, **nvarchar (30)**, etc.) ne sont pas pris en compte.  
  
    -   **varbinary (8000)** et **varbinary (max)** : les autres types de données binaires ne sont pas pris en compte (comme **image**, **Binary (8000)**, **varbinary (30)**, etc.).  
  
    -   **Date**, **Time (7)**, **smalldatetime**, **DateTime**, **datetime2 (7)**, **DateTimeOffset (7)** -d’autres types de date et d’heure, tels que **Time (4)**, ne sont pas pris en compte.  
  
    -   **sql_variant**  
  
    -   **xml**  
  
    -   Types CLR définis par le système (**hierarchyid**, **Geometry**, **Geography**)  
  
    -   types CLR définis par l'utilisateur  
  
### <a name="selection-criteria"></a>Critères de sélection  
 Parmi les types de données candidats, tout type de données qui invaliderait la requête est rejeté. Parmi les types de données candidats restants, l'algorithme de déduction du type en sélectionne un d'après les règles suivantes.  
  
1.  Le type de données qui produit le plus petit nombre de conversions implicites dans E ( \@ p) est sélectionné. Si un type de données particulier produit un type de données pour E ( \@ p) différent de TT ( \@ p), l’algorithme de déduction du type considère qu’il s’agit d’une conversion implicite supplémentaire du type de données e ( \@ p) vers TT ( \@ p).  
  
     Par exemple :  
  
    ```sql
    SELECT * FROM t WHERE Col_Int = Col_Int + @p  
    ```  
  
     Dans ce cas, E ( \@ p) est Col_Int + \@ p et TT ( \@ p) est de **type int**. **int** est choisi pour \@ p, car il ne produit pas de conversions implicites. Tout autre choix de type de données produit au moins une conversion implicite.  
  
2.  Si plusieurs types de données sont liés pour le plus petit nombre de conversions, le type de données dont la priorité est supérieure est utilisé. Par exemple  
  
    ```sql
    SELECT * FROM t WHERE Col_Int = Col_smallint + @p  
    ```  
  
     Dans ce cas, **int** et **smallint** produisent une conversion. Chaque autre type de données entraîne plusieurs conversions. Comme **int** est prioritaire sur **smallint**, **int** est utilisé pour \@ p. Pour plus d’informations sur la priorité des types de données, consultez [priorité des types de données &#40;&#41;Transact-SQL ](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
     Cette règle s'applique uniquement s'il existe une conversion implicite entre chaque type de données lié d'après la règle 1 et le type de données présentant la priorité la plus élevée. S'il n'existe aucune conversion implicite, la déduction du type de données se solde par une erreur. Par exemple, dans la requête `SELECT @p FROM t` , la déduction du type de données échoue, car tout type de données pour \@ p serait tout aussi bien correct. Par exemple, il n’existe pas de conversion implicite de **int** en **XML**.  
  
3.  Si deux types de données similaires se lient à la règle 1, par exemple **varchar (8000)** et **varchar (max)**, le plus petit type de données (**varchar (8000)**) est choisi. Le même principe s’applique aux types de données **nvarchar** et **varbinary** .  
  
4.  Pour les besoins de la règle 1, l'algorithme de la déduction du type préfère certaines conversions plutôt que d'autres. Conversions classées de la meilleure à la pire :  

    1.  Conversion entre un même type de données de base de longueur différente.  
  
    2.  Conversion entre une version de longueur fixe et une version à longueur variable des mêmes types de données (par exemple, **char** en **varchar**).  
  
    3.  Conversion entre **null** et **int**.  
  
    4.  Toute autre conversion.  
  
 Par exemple, pour la requête `SELECT * FROM t WHERE [Col_varchar(30)] > @p` , **varchar (8000)** est choisi, car la conversion (a) est la meilleure. Pour la requête `SELECT * FROM t WHERE [Col_char(30)] > @p` , **varchar (8000)** est toujours choisi, car il entraîne une conversion de type (b) et un autre choix (tel que **varchar (4000)**) entraîne une conversion de type (d).  
  
 En guise d’exemple final, pour une requête donnée `SELECT NULL + @p` , **int** est choisi pour \@ p, car il entraîne une conversion de type (c).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation d’exécuter l' \@ argument TSQL.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations telles que le type de données attendu pour les paramètres non déclarés `@id` et `@name`.  
  
```sql
sp_describe_undeclared_parameters @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes  
WHERE object_id = @id OR name = @name'  
  
```  
  
 Lorsque le paramètre `@id` est fourni comme référence `@params`, le paramètre `@id` est omis du jeu de résultats et seul le paramètre `@name` est décrit.  
  
```sql
sp_describe_undeclared_parameters @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes  
WHERE object_id = @id OR NAME = @name',  
@params = N'@id int'  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)
