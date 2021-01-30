---
description: sp_describe_cursor_columns (Transact-SQL)
title: sp_describe_cursor_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_cursor_columns
- sp_describe_cursor_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_cursor_columns
ms.assetid: 6eaa54af-7ba4-4fce-bf6c-6ac67cc1ac94
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 11c2a7610b13e806c7c40d0fd2ff5a9e0363fffc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158434"
---
# <a name="sp_describe_cursor_columns-transact-sql"></a>sp_describe_cursor_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit un rapport des attributs des colonnes contenues dans le jeu de résultats d'un curseur côté serveur.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_describe_cursor_columns   
   [ @cursor_return = ] output_cursor_variable OUTPUT   
    { [ , [ @cursor_source = ] N'local' ,   
          [ @cursor_identity = ] N'local_cursor_name' ]   
    | [ , [ @cursor_source = ] N'global' ,   
          [ @cursor_identity = ] N'global_cursor_name' ]   
    | [ , [ @cursor_source = ] N'variable' ,   
          [ @cursor_identity = ] N'input_cursor_variable' ]   
   }  
```  
  
## <a name="arguments"></a>Arguments  
 [ @cursor_return =] sortie *output_cursor_variable*  
 Nom d'une variable de curseur déclarée devant recevoir la sortie du curseur. *output_cursor_variable* est **Cursor**, sans valeur par défaut, et ne doit pas être associé à des curseurs au moment où sp_describe_cursor_columns est appelé. Le curseur retourné est un curseur en lecture seule, dynamique et permettant les défilements.  
  
 [ @cursor_source =] {N’local' | N’global' | N’variable'}  
 Indique si le curseur qui fait l'objet du rapport est défini en utilisant le nom d'un curseur local, d'un curseur global ou d'une variable de curseur. Le paramètre est de type **nvarchar (30)**.  
  
 [ @cursor_identity =] N'*local_cursor_name*'  
 Nom d'un curseur créé par une instruction DECLARE CURSOR contenant soit le mot clé LOCAL, soit celui défini par défaut pour LOCAL. *local_cursor_name* est **de type nvarchar (128)**.  
  
 [ @cursor_identity =] N'*global_cursor_name*'  
 Nom d’un curseur créé par une instruction DECLARE CURSOR qui possède soit le mot clé GLOBAL, soit celui défini par défaut sur GLOBAL. *global_cursor_name* est **de type nvarchar (128)**.  
  
 *global_cursor_name* peut également être le nom d’un curseur côté serveur d’API ouvert par une application ODBC, puis nommé en appelant SQLSetCursorName.  
  
 [ @cursor_identity =] N'*input_cursor_variable*'  
 Nom d'une variable de curseur associée à un curseur ouvert. *input_cursor_variable* est **de type nvarchar (128)**.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="cursors-returned"></a>Curseurs retournés  
 sp_describe_cursor_columns encapsule son rapport sous la forme d’un paramètre de sortie de [!INCLUDE[tsql](../../includes/tsql-md.md)] **curseur** . Cela permet aux lots, procédures stockées et déclencheurs [!INCLUDE[tsql](../../includes/tsql-md.md)] de travailler sur une seule ligne de sortie à la fois. Cela signifie également que la procédure ne peut pas être appelée directement à partir des fonctions API de base de données. Le paramètre de sortie **Cursor** doit être lié à une variable de programme, mais les API de base de données ne prennent pas en charge la liaison de paramètres ou de variables de **curseur** .  
  
 Vous trouverez dans le tableau suivant une présentation du format de curseur renvoyé par sp_describe_cursor_columns.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|column_name|**sysname** (Nullable)|Nom attribué à la colonne du jeu de résultats. La colonne est NULL si elle a été spécifiée sans être accompagnée de la clause AS.|  
|ordinal_position|**int**|Position relative de la colonne par rapport à la colonne la plus à gauche du jeu de résultats. La première colonne est à la position 0.|  
|column_characteristics_flags|**int**|Masque de bits indiquant les informations stockées dans DBCOLUMNFLAGS dans OLE DB. Il peut s'agir d'un des éléments suivants ou d'une combinaison de ceux-ci :<br /><br /> 1 = Signet<br /><br /> 2 = Longueur fixe<br /><br /> 4 = Pouvant être Null<br /><br /> 8 = Contrôle de version de ligne<br /><br /> 16 = Colonne pouvant être mise à jour (définie dans le cas de colonnes projetées d'un curseur n'ayant pas de clause FOR UPDATE. Si une colonne de ce type existe, il ne peut y en avoir qu'une par curseur).<br /><br /> Lorsque les valeurs binaires sont combinées, les caractéristiques des valeurs binaires combinées s'appliquent. Par exemple, si la valeur binaire est égale à 6, la colonne est de longueur fixe (2) et accepte les valeurs NULL (4).|  
|column_size|**int**|Taille maximale possible d'une valeur de cette colonne.|  
|data_type_sql|**smallint**|Numéro indiquant le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la colonne.|  
|column_precision|**tinyint**|Précision maximale de la colonne, en fonction de la valeur *bPrecision* dans OLE DB.|  
|column_scale|**tinyint**|Nombre de chiffres à droite de la virgule décimale pour les types de données **numériques** ou **décimaux** , en fonction de la valeur *bScale* dans OLE DB.|  
|order_position|**int**|Position de la colonne dans la clé d'ordre relative à la colonne la plus à gauche si la colonne prend part à la définition de l'ordre du jeu de résultats.|  
|order_direction|**varchar (1)**(Nullable)|A = La colonne fait partie de la clé d'ordre et l'ordre est croissant.<br /><br /> D = La colonne fait partie de la clé d'ordre et l'ordre est décroissant.<br /><br /> NULL = La colonne ne fait pas partie de la clé d'ordre.|  
|hidden_column|**smallint**|0 = Cette colonne apparaît dans la liste de sélection.<br /><br /> 1 = Réservé pour un usage ultérieur.|  
|columnid|**int**|Identification de la colonne de base. Si la colonne de l'ensemble de résultats a été créée à partir d'une expression, columnid est égal à -1.|  
|objectid|**int**|ID d'objet de l'objet ou de la table de base qui fournit la colonne. Si la colonne de l'ensemble de résultats a été créée à partir d'une expression, objectid est égal à -1.|  
|dbid|**int**|ID de la base de données contenant la table de base qui fournit la colonne. Si la colonne de l'ensemble de résultats a été créée à partir d'une expression, dbid est égal à -1.|  
|dbname|**sysname**<br /><br /> (accepte les valeurs NULL)|Nom de la base de données contenant la table de base qui fournit la colonne. Si la colonne de l'ensemble de résultats a été créée à partir d'une expression, dbname a la valeur NULL.|  
  
## <a name="remarks"></a>Notes  
 La procédure sp_describe_cursor_columns décrit les attributs des colonnes de l'ensemble de résultats d'un curseur de serveur, tels que le nom et le type de données de chaque curseur. Utilisez sp_describe_cursor pour obtenir une description des attributs globaux du curseur de serveur. Utilisez sp_describe_cursor_tables pour générer un rapport sur les tables de base référencées par le curseur. Pour obtenir un rapport sur les curseurs côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)] visibles pour la connexion, utilisez sp_cursor_list.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant ouvre un curseur global et utilise `sp_describe_cursor_columns` pour fournir un rapport sur colonnes utilisées dans le curseur.  
  
```  
USE AdventureWorks2012;  
GO  
-- Declare and open a global cursor.  
DECLARE abc CURSOR KEYSET FOR  
SELECT LastName  
FROM Person.Person;  
GO  
OPEN abc;  
  
-- Declare a cursor variable to hold the cursor output variable  
-- from sp_describe_cursor_columns.  
DECLARE @Report CURSOR;  
  
-- Execute sp_describe_cursor_columns into the cursor variable.  
EXEC master.dbo.sp_describe_cursor_columns  
    @cursor_return = @Report OUTPUT  
    ,@cursor_source = N'global'   
    ,@cursor_identity = N'abc';  
  
-- Fetch all the rows from the sp_describe_cursor_columns output cursor.  
FETCH NEXT from @Report;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
   FETCH NEXT from @Report;  
END  
  
-- Close and deallocate the cursor from sp_describe_cursor_columns.  
CLOSE @Report;  
DEALLOCATE @Report;  
GO  
-- Close and deallocate the original cursor.  
CLOSE abc;  
DEALLOCATE abc;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Curseurs](../../relational-databases/cursors.md)   
 [CURSOR_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-status-transact-sql.md)   
 [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [sp_describe_cursor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-cursor-transact-sql.md)   
 [sp_cursor_list &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursor-list-transact-sql.md)   
 [sp_describe_cursor_tables &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-cursor-tables-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
