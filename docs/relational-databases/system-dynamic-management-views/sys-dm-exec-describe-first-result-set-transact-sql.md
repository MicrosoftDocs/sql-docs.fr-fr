---
title: sys.dm_exec_describe_first_result_set (Transact-SQL)
description: sys.dm_exec_describe_first_result_set (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_describe_first_result_set
- sys.dm_exec_describe_first_result_set_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_describe_first_result_set catalog view
ms.assetid: 6ea88346-0bdb-4f0e-9f1f-4d85e3487d23
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: ''
ms.date: 06/10/2016
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 00bb311a4b1faad0e85a7f3b450db2f1057dcb4e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094099"
---
# <a name="sysdm_exec_describe_first_result_set-transact-sql"></a>sys.dm_exec_describe_first_result_set (Transact-SQL)

[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction de gestion dynamique accepte une [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction en tant que paramètre et décrit les métadonnées du premier jeu de résultats pour l’instruction.  
  
 **sys.dm_exec_describe_first_result_set** a la même définition de jeu de résultats que [sys.dm_exec_describe_first_result_set_for_object &#40;transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md) et est semblable à SP_DESCRIBE_FIRST_RESULT_SET &#40;[Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_exec_describe_first_result_set(@tsql, @params, @include_browse_information)  
```  
  
## <a name="arguments"></a>Arguments  
 *\@TSQL*  
 Une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] *Transact-SQL_batch* peut être de type **nvarchar (**_n_*_)_* ou **nvarchar (max)**.  
  
 *\@params*  
 \@params fournit une chaîne de déclaration pour les paramètres du [!INCLUDE[tsql](../../includes/tsql-md.md)] lot, similaire à sp_executesql. Les paramètres peuvent être de type **nvarchar (n)** ou **nvarchar (max)**.  
  
 Est une chaîne qui contient les définitions de tous les paramètres qui ont été incorporés dans le [!INCLUDE[tsql](../../includes/tsql-md.md)] *_batch*. Cette chaîne doit être une constante Unicode ou une variable Unicode. Chaque définition de paramètre se compose d'un nom de paramètre et d'un type de données. *n* est un espace réservé qui indique des définitions de paramètres supplémentaires. Chaque paramètre spécifié dans stmt doit être défini dans \@ params. Si l' [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction ou le lot dans l’instruction ne contient pas de paramètres, \@ params n’est pas obligatoire. La valeur par défaut de ce paramètre est NULL.  
  
 *\@include_browse_information*  
 Lorsque la valeur 1 est définie, chaque requête est analysée comme si elle comportait une option FOR BROWSE sur la requête. Des colonnes clés supplémentaires et les informations de table source sont retournées.  
  
## <a name="table-returned"></a>Table retournée  
 Ces métadonnées communes sont retournées en tant que jeu de résultats. Une ligne pour chaque colonne dans les métadonnées de résultats décrit le type et la possibilité de valeur NULL de la colonne dans le format indiqué dans le tableau suivant. S'il n'existe pas de première instruction pour chaque chemin d'accès de contrôle, un jeu de résultats avec des lignes nulles est retourné.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**is_hidden**|**bit**|Spécifie que la colonne est une colonne supplémentaire ajoutée pour la navigation et à des fins d'informations qui ne s'affiche pas réellement dans le jeu de résultats.|  
|**column_ordinal**|**int**|Contient la position ordinale de la colonne dans le jeu de résultats. La position de la première colonne sera spécifiée comme 1.|  
|**name**|**sysname**|Contient le nom de la colonne si un nom peut être déterminé. Sinon, la valeur est NULL.|  
|**is_nullable**|**bit**|Contient les valeurs suivantes :<br /><br /> Valeur 1 si la colonne autorise des valeurs NULL.<br /><br /> Valeur 0 si la colonne n'autorise pas de valeurs NULL.<br /><br /> Valeur 1 s'il est impossible de déterminer que la colonne autorise des valeurs NULL.|  
|**system_type_id**|**int**|Contient le system_type_id du type de données de la colonne tel que spécifié dans sys. types. Pour les types CLR, bien que la colonne system_type_name retourne NULL, cette colonne retournera la valeur 240.|  
|**system_type_name**|**nvarchar (256)**|Contient le nom et les arguments (tels que la longueur, la précision, l'échelle) spécifiés pour le type de données de la colonne.<br /><br /> Si le type de données est un type d'alias défini par l'utilisateur, le type de système sous-jacent est spécifié ici.<br /><br /> Si le type de données est un type clr défini par l'utilisateur, NULL est retourné dans cette colonne.|  
|**max_length**|**smallint**|Longueur maximale (en octets) de la colonne.<br /><br /> -1 = le type de données de la colonne est **varchar (max)**, **nvarchar (max)**, **varbinary (max)** ou **XML**.<br /><br /> Pour les colonnes de **texte** , la valeur **max_length** sera 16 ou la valeur définie par **sp_tableoption’texte dans la ligne'**.|  
|**precision**|**tinyint**|Précision de la colonne si elle est numérique. Dans le cas contraire, retourne la valeur 0.|  
|**scale**|**tinyint**|Échelle de la colonne si elle est numérique. Dans le cas contraire, retourne la valeur 0.|  
|**collation_name**|**sysname**|Nom du classement de la colonne si elle est basée sur les caractères. Dans le cas contraire, la valeur NULL est retournée.|  
|**user_type_id**|**int**|Pour les types d'alias et CLR, contient l'information user_type_id du type de données de la colonne comme spécifié dans sys.types. Sinon, la valeur est NULL.|  
|**user_type_database**|**sysname**|Pour les types d'alias et CLR, contient le nom de la base de données dans laquelle le type est défini. Sinon, la valeur est NULL.|  
|**user_type_schema**|**sysname**|Pour les types d'alias et CLR, contient le nom du schéma dans lequel le type est défini. Sinon, la valeur est NULL.|  
|**user_type_name**|**sysname**|Pour les types d'alias et CLR, contient le nom du type. Sinon, la valeur est NULL.|  
|**assembly_qualified_type_name**|**nvarchar(4000)**|Pour les types CLR, retourne le nom de l'assembly et de la classe définissant le type. Sinon, la valeur est NULL.|  
|**xml_collection_id**|**int**|Contient l'information xml_collection_id du type de données de la colonne comme spécifié dans sys.columns. Cette colonne retourne NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_database**|**sysname**|Contient la base de données dans laquelle la collection de schémas XML associée à ce type est définie. Cette colonne retourne NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_schema**|**sysname**|Contient le schéma dans lequel la collection de schémas XML associée à ce type est définie. Cette colonne retourne NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**xml_collection_name**|**sysname**|Contient le nom de la collection de schémas XML associé à ce type. Cette colonne retourne NULL si le type retourné n'est pas associé à une collection de schémas XML.|  
|**is_xml_document**|**bit**|Retourne 1 si le type de données retourné est XML et que ce type est garanti être un document XML complet (nœud racine compris), par opposition à un fragment XML. Dans le cas contraire, retourne la valeur 0.|  
|**is_case_sensitive**|**bit**|Retourne 1 si la colonne est d'un type chaîne sensible à la casse. Retourne 0 dans le cas contraire.|  
|**is_fixed_length_clr_type**|**bit**|Retourne 1 si la colonne est d'un type CLR de longueur fixe. Retourne 0 dans le cas contraire.|  
|**source_server**|**sysname**|Nom du serveur d'origine (s'il provient d'un serveur distant). Le nom est donné tel qu’il apparaît dans sys. Servers. Retourne NULL si la colonne provient du serveur local, ou s'il est impossible de déterminer la provenance du serveur. Fourni uniquement si les informations de navigation sont demandées.|  
|**source_database**|**sysname**|Nom de la base de données d'origine retourné par la colonne dans ce résultat. Retourne NULL si la base de données ne peut pas être déterminée. Fourni uniquement si les informations de navigation sont demandées.|  
|**source_schema**|**sysname**|Nom du schéma d'origine retourné par la colonne dans ce résultat. Retourne NULL si le schéma ne peut pas être déterminé. Fourni uniquement si les informations de navigation sont demandées.|  
|**source_table**|**sysname**|Nom de la table d'origine retourné par la colonne dans ce résultat. Retourne NULL si la table ne peut pas être déterminée. Fourni uniquement si les informations de navigation sont demandées.|  
|**source_column**|**sysname**|Nom de la colonne d'origine retourné par la colonne de résultat. Retourne NULL si la colonne ne peut pas être déterminée. Fourni uniquement si les informations de navigation sont demandées.|  
|**is_identity_column**|**bit**|Retourne 1 si la colonne est une colonne d'identité et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne est une colonne d'identité.|  
|**is_part_of_unique_key**|**bit**|Retourne 1 si la colonne fait partie d'un index unique (notamment les contraintes unique et primaire) et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne fait partie d'un index unique. Fourni uniquement si les informations de navigation sont demandées.|  
|**is_updateable**|**bit**|Retourne 1 si la colonne peut être mise à jour et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne peut être mise à jour.|  
|**is_computed_column**|**bit**|Retourne 1 si la colonne est une colonne calculée et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne est une colonne calculée.|  
|**is_sparse_column_set**|**bit**|Retourne 1 si la colonne est une colonne éparse et 0 dans le cas contraire. Retourne NULL s'il est impossible de déterminer que la colonne fait partie d'un jeu de colonnes éparses.|  
|**ordinal_in_order_by_list**|**smallint**|Position de cette colonne dans la liste ORDER BY. Retourne NULL si la colonne ne s'affiche pas dans la liste ORDER BY ou si la liste ORDER BY ne peut pas être déterminée de manière unique.|  
|**order_by_list_length**|**smallint**|Longueur de la liste ORDER BY. Retourne NULL s'il n'existe aucune liste ORDER BY ou si la liste ORDER BY ne peut pas être déterminée de manière unique. Notez que cette valeur sera la même pour toutes les lignes retournées par sp_describe_first_result_set.|  
|**order_by_is_descending**|**smallint NULL**|Si la valeur ordinal_in_order_by_list n'est pas NULL, la colonne **order_by_is_descending** indique la direction de la clause ORDER BY pour cette colonne. Sinon, elle indique NULL.|  
|**error_number**|**int**|Contient le numéro d'erreur retourné par la fonction. Si aucune erreur ne s'est produite, la colonne contiendra NULL.|  
|**error_severity**|**int**|Contient la gravité retournée par la fonction. Si aucune erreur ne s'est produite, la colonne contiendra NULL.|  
|**error_state**|**int**|Contient le message d'état. retourné par la fonction. Si aucune erreur ne s'est produite, la colonne contiendra NULL.|  
|**error_message**|**nvarchar (4096)**|Contient le message retourné par la fonction. Si aucune erreur ne s'est produite, la colonne contiendra NULL.|  
|**error_type**|**int**|Contient un entier qui représente l'erreur retournée. Mappé à error_type_desc. Consultez la liste sous les notes.|  
|**error_type_desc**|**nvarchar(60)**|Contient une chaîne majuscule courte qui représente l'erreur retournée. Mappé à error_type. Consultez la liste sous les notes.|  
  
## <a name="remarks"></a>Notes  
 Cette fonction utilise le même algorithme que **sp_describe_first_result_set**. Pour plus d’informations, consultez [sp_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  
 Le tableau suivant répertorie les types d'erreur et leur description.  
  
|error_type|error_type|Description|  
|-----------------|-----------------|-----------------|  
|1|MISC|Toutes les erreurs qui ne font pas l'objet d'une description.|  
|2|SYNTAXE|Une erreur de syntaxe s'est produite dans le lot.|  
|3|CONFLICTING_RESULTS|Le résultat n'a pas pu être déterminé en raison d'un conflit entre deux premières instructions possibles.|  
|4|DYNAMIC_SQL|Le résultat n'a pas pu être déterminé en raison du SQL dynamique qui pourrait éventuellement retourner le premier résultat.|  
|5|CLR_PROCEDURE|Le résultat n'a pas pu être déterminé parce qu'une procédure stockée clr pourrait éventuellement retourner le premier résultat.|  
|6|CLR_TRIGGER|Le résultat n'a pas pu être déterminé parce qu'un déclencheur CLR pourrait éventuellement retourner le premier résultat.|  
|7|EXTENDED_PROCEDURE|Le résultat n'a pas pu être déterminé parce qu'une procédure stockée étendue pourrait éventuellement retourner le premier résultat.|  
|8|UNDECLARED_PARAMETER|Le résultat n’a pas pu être déterminé, car le type de données d’une ou de plusieurs colonnes du jeu de résultats dépend potentiellement d’un paramètre non déclaré.|  
|9|RECURSION|Le résultat n'a pas pu être déterminé car le lot contient une instruction récursive.|  
|10|TEMPORARY_TABLE|Le résultat n'a pas pu être déterminé car le lot contient une table temporaire qui n'est pas prise en charge par **sp_describe_first_result_set**.|  
|11|UNSUPPORTED_STATEMENT|Le résultat n'a pas pu être déterminé car le lot contient une instruction qui n'est pas prise en charge par **sp_describe_first_result_set** (par exemple, FETCH, REVERT etc.).|  
|12|OBJECT_TYPE_NOT_SUPPORTED|Le \@ object_id passé à la fonction n’est pas pris en charge (c’est-à-dire, pas une procédure stockée)|  
|13|OBJECT_DOES_NOT_EXIST|Le \@ object_id passé à la fonction est introuvable dans le catalogue système.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation d’exécuter l' \@ argument TSQL.  
  
## <a name="examples"></a>Exemples  
 Des exemples supplémentaires de la rubrique [sp_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) peuvent être adaptés pour utiliser **sys.dm_exec_describe_first_result_set**.  
  
### <a name="a-returning-information-about-a-single-transact-sql-statement"></a>R. Retour d'informations sur une instruction Transact-SQL unique  
 Le code suivant retourne des informations sur les résultats d'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_exec_describe_first_result_set  
(N'SELECT object_id, name, type_desc FROM sys.indexes', null, 0) ;  
```  
  
### <a name="b-returning-information-about-a-procedure"></a>B. Retour d'informations sur une procédure  
 L’exemple suivant crée une procédure stockée nommée pr_TestProc qui retourne deux jeux de résultats. Puis l'exemple montre que **sys.dm_exec_describe_first_result_set** retourne des informations sur le premier jeu de résultats dans la procédure.  
  
```  
USE AdventureWorks2012;  
GO  
  
CREATE PROC Production.TestProc  
AS  
SELECT Name, ProductID, Color FROM Production.Product ;  
SELECT Name, SafetyStockLevel, SellStartDate FROM Production.Product ;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set  
('Production.TestProc', NULL, 0) ;  
```  
  
### <a name="c-returning-metadata-from-a-batch-that-contains-multiple-statements"></a>C. Retour de métadonnées à partir d'un lot qui contient plusieurs instructions  
 L'exemple suivant évalue un lot qui contient deux instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Le jeu de résultats décrit le premier jeu de résultats retourné.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set(  
N'SELECT CustomerID, TerritoryID, AccountNumber FROM Sales.Customer WHERE CustomerID = @CustomerID;  
SELECT * FROM Sales.SalesOrderHeader;',  
N'@CustomerID int', 0) AS a;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sp_describe_undeclared_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)  
  
  
