---
description: sp_describe_first_result_set (Transact-SQL)
title: sp_describe_first_result_set (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_first_result_set
- sp_describe_first_result_set_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_first_result_set
ms.assetid: f2355a75-3a8e-43e6-96ad-4f41038f6d22
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8e9a77fb68a5642b3ba5fac7aac9ae0d9c31c899
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200807"
---
# <a name="sp_describe_first_result_set-transact-sql"></a>sp_describe_first_result_set (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne les métadonnées pour le premier jeu de résultats possible du [!INCLUDE[tsql](../../includes/tsql-md.md)] lot. Retourne un jeu de résultats vide si le lot ne retourne pas de résultats. Génère une erreur si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne peut pas déterminer les métadonnées de la première requête qui sera exécutée en effectuant une analyse statique. La vue de gestion dynamique [sys.dm_exec_describe_first_result_set &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md) retourne les mêmes informations.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_describe_first_result_set [ @tsql = ] N'Transact-SQL_batch'   
    [ , [ @params = ] N'parameters' ]   
    [ , [ @browse_information_mode = ] <tinyint> ] ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ \@tsql = ] 'Transact-SQL_batch'` Une ou plusieurs [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions. *Transact-SQL_batch* peut être de type **nvarchar (**_n_*_)_* ou **nvarchar (max)**.  
  
`[ \@params = ] N'parameters'`\@params fournit une chaîne de déclaration pour les paramètres du [!INCLUDE[tsql](../../includes/tsql-md.md)] lot, qui est similaire à sp_executesql. Les paramètres peuvent être de type **nvarchar (n)** ou **nvarchar (max)**.  
  
 Est une chaîne qui contient les définitions de tous les paramètres qui ont été incorporés dans le [!INCLUDE[tsql](../../includes/tsql-md.md)] *_batch*. Cette chaîne doit être une constante Unicode ou une variable Unicode. Chaque définition de paramètre se compose d'un nom de paramètre et d'un type de données. *n* est un espace réservé qui indique des définitions de paramètres supplémentaires. Chaque paramètre spécifié dans l’instruction doit être défini dans \@ params. Si l' [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction ou le lot dans l’instruction ne contient pas de paramètres, \@ params n’est pas obligatoire. La valeur par défaut de ce paramètre est NULL.  
  
`[ \@browse_information_mode = ] tinyint` Spécifie si des colonnes clés supplémentaires et des informations de table source sont retournées. Si la valeur 1 est définie, chaque requête est analysée comme si elle incluait une option FOR BROWSE sur la requête. Des colonnes clés supplémentaires et les informations de table source sont retournées.  
  
-   Si la valeur est 0, aucune information n'est retournée.  
  
-   Si la valeur 1 est définie, chaque requête est analysée comme si elle incluait une option FOR BROWSE sur la requête. Retourne les noms de tables de base comme informations de colonne source.  
  
-   Si la valeur 2 est définie, chaque requête est analysée comme si elle était utilisée lors de la préparation ou de l'exécution d'un curseur. Retourne les noms de vues comme informations de colonne source.  
  
## <a name="return-code-values"></a>Codet de retour  
 **sp_describe_first_result_set** retourne toujours l’état zéro en cas de réussite. Si la procédure génère une erreur et que la procédure est appelée en tant que RPC, l’état de retour est rempli par le type d’erreur décrit dans la colonne error_type de sys.dm_exec_describe_first_result_set. Si la procédure est appelée de [!INCLUDE[tsql](../../includes/tsql-md.md)], la valeur de retour est toujours de zéro, même en présence d'une erreur.  
  
## <a name="result-sets"></a>Jeux de résultats  
 Ces métadonnées communes sont retournées en tant que jeu de résultats avec une ligne pour chaque colonne dans les métadonnées de résultats. Chaque ligne décrit le type et la possibilité de valeur NULL de la colonne dans le format décrit dans la section suivante. S'il n'existe pas de première instruction pour chaque chemin d'accès de contrôle, un jeu de résultats avec des lignes nulles est retourné.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**is_hidden**|**bit NOT NULL**|Indique que la colonne est une colonne supplémentaire ajoutée à titre d'informations de navigation et qu'elle ne s'affiche pas réellement dans le jeu de résultats.|  
|**column_ordinal**|**int NOT NULL**|Contient la position ordinale de la colonne dans le jeu de résultats. La position de la première colonne sera spécifiée comme 1.|  
|**name**|**sysname NULL**|Contient le nom de la colonne si un nom peut être déterminé. Sinon, il contiendra NULL.|  
|**is_nullable**|**bit NOT NULL**|Contient la valeur 1 si la colonne autorise des valeurs NULL, 0 si la colonne n'autorise pas de valeurs NULL, et 1 s'il n'est pas possible de déterminer si la colonne autorise des valeurs NULL.|  
|**system_type_id**|**int NOT NULL**|Contient le system_type_id du type de données de la colonne tel que spécifié dans sys. types. Pour les types CLR, bien que la colonne system_type_name retourne NULL, cette colonne retournera la valeur 240.|  
|**system_type_name**|**nvarchar (256) NULL**|Contient le nom et les arguments (tels que la longueur, la précision, l'échelle) spécifiés pour le type de données de la colonne. Si le type de données est un type d'alias défini par l'utilisateur, le type de système sous-jacent est spécifié ici. S'il s'agit d'un type clr défini par l'utilisateur, NULL est retourné dans cette colonne.|  
|**max_length**|**smallint n’est pas NULL**|Longueur maximale (en octets) de la colonne.<br /><br /> -1 = le type de données de la colonne est **varchar (max)**, **nvarchar (max)**, **varbinary (max)** ou **XML**.<br /><br /> Pour les colonnes de **texte** , la valeur **max_length** sera 16 ou la valeur définie par **sp_tableoption’texte dans la ligne'**.|  
|**precision**|**tinyint non NULL**|Précision de la colonne si elle est numérique. Dans le cas contraire, retourne la valeur 0.|  
|**scale**|**tinyint non NULL**|Échelle de la colonne si elle est numérique. Dans le cas contraire, retourne la valeur 0.|  
|**collation_name**|**sysname NULL**|Nom du classement de la colonne si elle est basée sur les caractères. Dans le cas contraire, la valeur NULL est retournée.|  
|**user_type_id**|**int NULL**|Pour les types d'alias et CLR, contient l'information user_type_id du type de données de la colonne comme spécifié dans sys.types. Sinon, la valeur est NULL.|  
|**user_type_database**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom de la base de données dans laquelle le type est défini. Sinon, la valeur est NULL.|  
|**user_type_schema**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom du schéma dans lequel le type est défini. Sinon, la valeur est NULL.|  
|**user_type_name**|**sysname NULL**|Pour les types d'alias et CLR, contient le nom du type. Sinon, la valeur est NULL.|  
|**assembly_qualified_type_name**|**nvarchar(4000)**|Pour les types CLR, retourne le nom de l'assembly et de la classe définissant le type. Sinon, la valeur est NULL.|  
|**xml_collection_id**|**int NULL**|Contient l'information xml_collection_id du type de données de la colonne comme spécifié dans sys.columns. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_database**|**sysname NULL**|Contient la base de données dans laquelle la collection de schémas XML associée à ce type est définie. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_schema**|**sysname NULL**|Contient le schéma dans lequel la collection de schémas XML associée à ce type est définie. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_name**|**sysname NULL**|Contient le nom de la collection de schémas XML associé à ce type. Cette colonne retournera NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**is_xml_document**|**bit NOT NULL**|Retourne 1 si le type de données retourné est XML et que ce type est garanti être un document XML complet (nœud racine compris), par opposition à un fragment XML. Dans le cas contraire, retourne la valeur 0.|  
|**is_case_sensitive**|**bit NOT NULL**|Retourne 1 si la colonne est un type chaîne sensible à la casse et 0 si ce n'est pas le cas.|  
|**is_fixed_length_clr_type**|**bit NOT NULL**|Retourne 1 si la colonne est un type CLR de longueur fixe et 0 si ce n'est pas le cas.|  
|**source_server**|**sysname**|Nom du serveur d'origine retourné par la colonne dans ce résultat (s'il provient d'un serveur distant). Le nom est donné tel qu’il apparaît dans sys. Servers. Retourne NULL si la colonne provient du serveur local, ou s'il est impossible de déterminer la provenance du serveur. Fourni uniquement si les informations de navigation sont demandées.|  
|**source_database**|**sysname**|Nom de la base de données d'origine retourné par la colonne dans ce résultat. Retourne NULL si la base de données ne peut pas être déterminée. Fourni uniquement si les informations de navigation sont demandées.|  
|**source_schema**|**sysname**|Nom du schéma d'origine retourné par la colonne dans ce résultat. Retourne NULL si le schéma ne peut pas être déterminé. Fourni uniquement si les informations de navigation sont demandées.|  
|**source_table**|**sysname**|Nom de la table d'origine retourné par la colonne dans ce résultat. Retourne NULL si la table ne peut pas être déterminée. Fourni uniquement si les informations de navigation sont demandées.|  
|**source_column**|**sysname**|Nom de la colonne d'origine retourné par la colonne de résultat. Retourne NULL si la colonne ne peut pas être déterminée. Fourni uniquement si les informations de navigation sont demandées.|  
|**is_identity_column**|**bit NULL**|Retourne 1 si la colonne est une colonne d'identité et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne est une colonne d'identité.|  
|**is_part_of_unique_key**|**bit NULL**|Retourne 1 si la colonne fait partie d'un index unique (notamment la contrainte unique et primaire) et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne fait partie d'un index unique. Fourni uniquement si les informations de navigation sont demandées.|  
|**is_updateable**|**bit NULL**|Retourne 1 si la colonne peut être mise à jour et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne peut être mise à jour.|  
|**is_computed_column**|**bit NULL**|Retourne 1 si la colonne est une colonne calculée et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne est une colonne calculée.|  
|**is_sparse_column_set**|**bit NULL**|Retourne 1 si la colonne est une colonne éparse et 0 dans le cas contraire. Retourne NULL s’il est impossible de déterminer que la colonne fait partie d’un jeu de colonnes éparses.|  
|**ordinal_in_order_by_list**|**smallint NULL**|Position de cette colonne dans la liste ORDER BY. Retourne NULL si la colonne n’apparaît pas dans la liste ORDER BY ou si la liste ORDER BY ne peut pas être déterminée de manière unique.|  
|**order_by_list_length**|**smallint NULL**|Longueur de la liste ORDER BY. Retourne NULL s'il n'existe aucune liste ORDER BY ou si la liste ORDER BY ne peut pas être déterminée de manière unique. Notez que cette valeur sera la même pour toutes les lignes retournées par **sp_describe_first_result_set.**|  
|**order_by_is_descending**|**smallint NULL**|Si la valeur ordinal_in_order_by_list n'est pas NULL, la colonne **order_by_is_descending** indique la direction de la clause ORDER BY pour cette colonne. Sinon, elle indique NULL.|  
|**tds_type_id**|**int NOT NULL**|À usage interne uniquement.|  
|**tds_length**|**int NOT NULL**|À usage interne uniquement.|  
|**tds_collation_id**|**int NULL**|À usage interne uniquement.|  
|**tds_collation_sort_id**|**tinyint NULL**|À usage interne uniquement.|  
  
## <a name="remarks"></a>Notes  
 **sp_describe_first_result_set** garantit que si la procédure retourne les premières métadonnées du jeu de résultats pour (un hypothétique) lot a et si ce lot (a) est exécuté par la suite, le traitement (1) génère une erreur de temps d’optimisation. (2) déclenche une erreur d’exécution, (3) ne retourne aucun jeu de résultats, ou (4) retourne un premier jeu de résultats avec les mêmes métadonnées que celles décrites par **sp_describe_first_result_set**.  
  
 Le nom, la possibilité de valeur NULL et le type de données peuvent différer. Si **sp_describe_first_result_set** retourne un jeu de résultats vide, la garantie est que l’exécution du lot ne retourne pas de jeu de résultats.  
  
 Cette garantie suppose qu’il n’y a aucune modification de schéma pertinente sur le serveur. Les modifications de schéma pertinentes sur le serveur n’incluent pas la création de tables temporaires ou de variables de table dans le lot A entre le moment où **sp_describe_first_result_set** est appelée et l’heure à laquelle le jeu de résultats est retourné pendant l’exécution, y compris les modifications de schéma effectuées par le lot B.  
  
 **sp_describe_first_result_set** retourne une erreur dans l’un des cas suivants.  
  
-   Si l’entrée \@ tsql n’est pas un [!INCLUDE[tsql](../../includes/tsql-md.md)] lot valide. La validité est déterminée en analysant et en analysant le [!INCLUDE[tsql](../../includes/tsql-md.md)] lot. Les erreurs provoquées par le lot pendant l’optimisation de la requête ou lors de l’exécution ne sont pas prises en compte pour déterminer si le [!INCLUDE[tsql](../../includes/tsql-md.md)] lot est valide.  
  
-   Si \@ params n’a pas la valeur null et contient une chaîne qui n’est pas une chaîne de déclaration valide syntaxiquement pour les paramètres, ou si elle contient une chaîne qui déclare un paramètre plus d’une fois.  
  
-   Si le lot d’entrée [!INCLUDE[tsql](../../includes/tsql-md.md)] déclare une variable locale du même nom qu’un paramètre déclaré dans \@ params.  
  
-   Si l'instruction utilise une table temporaire.  
  
-   La requête inclut la création d'une table permanente qui est alors interrogée.  
  
 Si tous les autres contrôles réussissent, tous les chemins d'accès de flux de contrôle possibles à l'intérieur du lot d'entrée sont pris en compte. Cela prend en compte toutes les instructions de contrôle de workflow (GOTO, IF/ELSE, WHILe et [!INCLUDE[tsql](../../includes/tsql-md.md)] try/catch), ainsi que toutes les procédures, les [!INCLUDE[tsql](../../includes/tsql-md.md)] lots dynamiques ou les déclencheurs appelés à partir du lot d’entrée par une instruction EXEC, une instruction DDL qui provoque le déclenchement des déclencheurs DDL, ou une instruction DML qui entraîne le déclenchement des déclencheurs sur une table cible ou sur une table modifiée en raison d’une action en cascade sur une contrainte de clé étrangère. Dans le cas de nombreux chemins d'accès de contrôle possibles, un algorithme s'arrête à un point donné.  
  
 Pour chaque chemin d’accès de workflow de contrôle, la première instruction (le cas échéant) qui retourne un jeu de résultats est déterminée par **sp_describe_first_result_set**.  
  
 Lorsque plusieurs premières instructions possibles sont trouvées dans un lot, leurs résultats peuvent différer selon le nombre de colonnes, le nom de colonne, la possibilité de valeur NULL et le type de données. La gestion de ces différences fait l'objet d'une description plus détaillée dans cette rubrique :  
  
-   Si le nombre de colonnes diffère, une erreur est générée et aucun résultat n'est retourné.  
  
-   Si le nom de colonne diffère, le nom de colonne retourné est défini sur NULL.  
  
-   Si la possibilité de valeur NULL diffère, la possibilité de valeur NULL retournée autorisera des valeurs NULL.  
  
-   Si le type de données diffère, une erreur est générée et aucun résultat n'est retourné à l'exception des cas suivants :  
  
    -   **varchar (a)** en **varchar (a')** où un' >.  
  
    -   **varchar (a)** en **varchar (max)**  
  
    -   **nvarchar (a)** en **nvarchar (a')** où un' >.  
  
    -   **nvarchar (a)** en **nvarchar (max)**  
  
    -   **varbinary (a)** en **varbinary (a')** où un' >.  
  
    -   **varbinary (a)** en **varbinary (max)**  
  
 **sp_describe_first_result_set** ne prend pas en charge la récursivité indirecte.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation d’exécuter l' \@ argument TSQL.  
  
## <a name="examples"></a>Exemples  
  
### <a name="typical-examples"></a>Exemples types  
  
#### <a name="a-simple-example"></a>R. Exemple simple  
 L'exemple suivant décrit le jeu de résultats retourné par une requête unique.  
  
```  
sp_describe_first_result_set @tsql = N'SELECT object_id, name, type_desc FROM sys.indexes'  
```  
  
 L'exemple suivant affiche le jeu de résultats retourné par une requête unique qui contient un paramètre.  
  
```  
sp_describe_first_result_set @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes   
WHERE object_id = @id1'  
, @params = N'@id1 int'  
```  
  
#### <a name="b-browse-mode-examples"></a>B. Exemples de modes de navigation  
 Les trois exemples suivants illustrent la principale différence entre les différents modes de navigation. Seules les colonnes appropriées ont été incluses dans les résultats de la requête.  
  
 L'exemple qui utilise 0 indique qu'aucune information est retournée.  
  
```sql  
CREATE TABLE dbo.t (a int PRIMARY KEY, b1 int);  
GO  
CREATE VIEW dbo.v AS SELECT b1 AS b2 FROM dbo.t;  
GO  
EXEC sp_describe_first_result_set N'SELECT b2 AS b3 FROM dbo.v', null, 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|is_hidden|column_ordinal|name|source_schema|source_table|source_column|is_part_of_unique_key|  
|----------------|---------------------|----------|--------------------|-------------------|--------------------|-------------------------------|  
|0|1|b3|NULL|NULL|NULL|NULL|  
  
 L'exemple qui utilise 1 indique qu'il retourne les informations comme s'il incluait une option FOR BROWSE sur la requête.  
  
```sql  
EXEC sp_describe_first_result_set N'SELECT b2 AS b3 FROM v', null, 1  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|is_hidden|column_ordinal|name|source_schema|source_table|source_column|is_part_of_unique_key|  
|----------------|---------------------|----------|--------------------|-------------------|--------------------|-------------------------------|  
|0|1|b3|dbo|t|B1|0|  
|1|2|a|dbo|t|a|1|  
  
 L'exemple qui utilise 2 indique une analyse comme si vous prépariez un curseur.  
  
```sql  
EXEC sp_describe_first_result_set N'SELECT b2 AS b3 FROM v', null, 2  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|is_hidden|column_ordinal|name|source_schema|source_table|source_column|is_part_of_unique_key|  
|----------------|---------------------|----------|--------------------|-------------------|--------------------|-------------------------------|  
|0|1|B3|dbo|v|B2|0|  
|1|2|ROWSTAT|NULL|NULL|NULL|0|  
  
### <a name="examples-of-problems"></a>Exemples de problèmes  
 Les exemples suivants utilisent deux tables pour tous les exemples. Exécutez les instructions suivantes pour créer les tables d'exemples.  
  
```  
CREATE TABLE dbo.t1 (a int NULL, b varchar(10) NULL, c nvarchar(10) NULL);  
CREATE TABLE dbo.t2 (a smallint NOT NULL, d varchar(20) NOT NULL, e int NOT NULL);  
```  
  
#### <a name="error-because-the-number-of-columns-differ"></a>Erreur car le nombre de colonnes diffère  
 Le nombre de colonnes dans les premiers jeux de résultats possibles diffère dans cet exemple.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT a FROM t1;  
ELSE  
    SELECT a, b FROM t1;  
SELECT * FROM t; -- Ignored, not a possible first result set.'  
  
```  
  
#### <a name="error-because-the-data-types-differ"></a>Erreur car les types de données diffèrent  
 Les types de colonnes diffèrent dans les premiers jeux de résultats possibles.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT a FROM t1;  
ELSE  
    SELECT a FROM t2;  
```  
  
 Résultat : erreur, types incompatibles (**int** et **smallint**).  
  
#### <a name="column-name-cannot-be-determined"></a>Le nom de colonne ne peut pas être déterminé  
 Les colonnes dans les premiers jeux de résultats possibles diffèrent de par la longueur pour le même type de longueur variable, la possibilité de valeur NULL et les noms de colonne :  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT b FROM t1;  
ELSE  
    SELECT d FROM t2; '  
```  
  
 Résultat : \<Unknown Column Name> **varchar (20) null**  
  
#### <a name="column-name-forced-to-be-identical-through-aliasing"></a>Nom de colonne forcé à être identique par crénelage  
 Identique à l'exemple précédent, mais les colonnes ont le même nom via le crénelage de colonne.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT b FROM t1;  
ELSE  
    SELECT d AS b FROM t2;'  
```  
  
 Résultat : b **varchar (20) null**  
  
#### <a name="error-because-column-types-cannot-be-matched"></a>Erreur car les types de colonne ne peuvent pas être mis en correspondance  
 Les types de colonnes diffèrent dans les premiers jeux de résultats possibles.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT b FROM t1;  
ELSE  
    SELECT c FROM t1;'  
```  
  
 Résultat : erreur, types incompatibles (**varchar (10)** et. **nvarchar (10)**).  
  
#### <a name="result-set-can-return-an-error"></a>Le jeu de résultats peut retourner une erreur  
 Le premier jeu de résultats est soit une erreur, soit un jeu de résultats.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    RAISERROR(''Some Error'', 16, 1);  
  
ELSE  
    SELECT a FROM t1;  
SELECT e FROM t2; -- Ignored, not a possible first result set.;'  
```  
  
 Résultat : **intNULL**  
  
#### <a name="some-code-paths-return-no-results"></a>Certains chemins de code ne retournent pas de résultats  
 Le premier jeu de résultats est soit Null, soit un jeu de résultats.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    RETURN;  
SELECT a FROM t1;'  
```  
  
 Résultat : **intNULL**  
  
#### <a name="result-from-dynamic-sql"></a>Résultat du SQL dynamique  
 Le premier jeu de résultats est SQL dynamique qui est détectable car il s'agit d'une chaîne littérale.  
  
```  
sp_describe_first_result_set @tsql =   
N'EXEC(N''SELECT a FROM t1'');'  
```  
  
 Résultat : **int null**  
  
#### <a name="result-failure-from-dynamic-sql"></a>Échec du résultat du SQL dynamique  
 Le premier jeu de résultats est indéfini en raison du SQL dynamique.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
DECLARE @SQL NVARCHAR(max);  
SET @SQL = N''SELECT a FROM t1 WHERE 1 = 1 '';  
IF(1=1)  
    SET @SQL += N'' AND e > 10 '';  
EXEC(@SQL); '  
```  
  
 Résultat : Erreur. Le résultat n'est pas détectable en raison du SQL dynamique.  
  
#### <a name="result-set-specified-by-user"></a>Jeu de résultats spécifié par l'utilisateur  
 Le premier jeu de résultats est spécifié manuellement par l'utilisateur.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
DECLARE @SQL NVARCHAR(max);  
SET @SQL = N''SELECT a FROM t1 WHERE 1 = 1 '';  
IF(1=1)  
    SET @SQL += N'' AND e > 10 '';  
EXEC(@SQL)  
    WITH RESULT SETS(  
        (Column1 BIGINT NOT NULL)  
    ); '  
```  
  
 Résultat : Colonne1 **bigint non null**  
  
#### <a name="error-caused-by-a-ambiguous-result-set"></a>Erreur provoquée par un jeu de résultats ambigu  
 Cet exemple suppose qu’un autre utilisateur nommé Utilisateur1 a une table nommée T1 dans le schéma par défaut S1 avec des colonnes (un **int not null**).  
  
```  
sp_describe_first_result_set @tsql =   
N'  
    IF(@p > 0)  
    EXECUTE AS USER = ''user1'';  
    SELECT * FROM t1;'  
, @params = N'@p int'  
```  
  
 Résultat : Erreur. T1 peut être dbo. T1 ou S1. T1, chacun avec un nombre de colonnes différent.  
  
#### <a name="result-even-with-ambiguous-result-set"></a>Résultat même avec un jeu de résultats ambigu  
 Utilisez les mêmes hypothèses que l'exemple précédent.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
    IF(@p > 0)  
    EXECUTE AS USER = ''user1'';  
    SELECT a FROM t1;'  
```  
  
 Résultat : **int null** , car dbo. T1. a et S1. T1. a ont le type **int** et une possibilité de valeur null différente.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_describe_undeclared_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)  
 
