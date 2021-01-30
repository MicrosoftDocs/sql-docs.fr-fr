---
description: sp_describe_cursor_tables (Transact-SQL)
title: sp_describe_cursor_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_cursor_tables_TSQL
- sp_describe_cursor_tables
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_cursor_tables
ms.assetid: 02c0f81a-54ed-4ca4-aa4f-bb7463a9ab9a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b5c2e609eb3a521a991848b3b795b04b2ffbd01f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200795"
---
# <a name="sp_describe_cursor_tables-transact-sql"></a>sp_describe_cursor_tables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Indique les objets ou tables de base référencés par un curseur côté serveur.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_describe_cursor_tables   
     [ @cursor_return = ] output_cursor_variable OUTPUT   
     { [ , [ @cursor_source = ] N'local'  
     , [ @cursor_identity = ] N'local_cursor_name' ]   
   | [ , [ @cursor_source = ] N'global'  
     , [ @cursor_identity = ] N'global_cursor_name' ]   
   | [ , [ @cursor_source = ] N'variable'  
     , [ @cursor_identity = ] N'input_cursor_variable' ]   
     }   
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @cursor_return =] sortie *output_cursor_variable*  
 Nom d'une variable de curseur déclarée devant recevoir la sortie du curseur. *output_cursor_variable* est **Cursor**, sans valeur par défaut, et ne doit pas être associé à des curseurs au moment où sp_describe_cursor_tables est appelé. Le curseur retourné est un curseur en lecture seule, dynamique et permettant les défilements.  
  
 [ @cursor_source =] {N’local' | N’global' | N’variable'}  
 Indique si le curseur qui fait l'objet du rapport est défini en utilisant le nom d'un curseur local, d'un curseur global ou d'une variable de curseur. Le paramètre est de type **nvarchar (30)**.  
  
 [ @cursor_identity =] N'*local_cursor_name*'  
 Nom d’un curseur créé par une instruction DECLARE CURSOR contenant soit le mot clé LOCAL, soit la valeur locale par défaut. *local_cursor_name* est **de type nvarchar (128)**.  
  
 [ @cursor_identity =] N'*global_cursor_name*'  
 Nom d'un curseur créé par une instruction DECLARE CURSOR contenant soit le mot clé GLOBAL, soit celui défini par défaut pour GLOBAL. *global_cursor_name* peut également être le nom d’un curseur de serveur d’API ouvert par une application ODBC qui a ensuite nommé le curseur en appelant SQLSetCursorName. *global_cursor_name* est **de type nvarchar (128)**.  
  
 [ @cursor_identity =] N'*input_cursor_variable*'  
 Nom d'une variable de curseur associée à un curseur ouvert. *input_cursor_variable* est **de type nvarchar (128)**.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="cursors-returned"></a>Curseurs retournés  
 sp_describe_cursor_tables encapsule son rapport sous la forme d’un paramètre de sortie de [!INCLUDE[tsql](../../includes/tsql-md.md)] **curseur** . Cela permet aux lots, procédures stockées et déclencheurs [!INCLUDE[tsql](../../includes/tsql-md.md)] de travailler sur une seule ligne de sortie à la fois. Par ailleurs, la procédure ne peut pas être appelée directement depuis les fonctions d'API. Le paramètre de sortie **Cursor** doit être lié à une variable de programme, mais les API ne prennent pas en charge les paramètres ou les variables de **curseur** de liaison.  
  
 La table suivante montre le format du curseur qui est retourné en utilisant sp_describe_cursor_tables.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|table_owner|**sysname**|ID d'utilisateur du propriétaire de table.|  
|Table_name|**sysname**|Nom de l'objet ou de la table de base. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les curseurs côté serveur retournent toujours l'objet spécifié par l'utilisateur, et non les tables de base.|  
|optimizer_hint|**smallint**|Bitmap composée d'un ou plusieurs des éléments suivants :<br /><br /> 1 = Verrouillage au niveau de la ligne (ROWLOCK)<br /><br /> 4 = Verrouillage au niveau de la page (PAGELOCK)<br /><br /> 8 = Verrou de table (TABLOCK)<br /><br /> 16 = Verrou de table exclusif (TABLOCKX)<br /><br /> 32 = Verrou de mise à jour (UPDLOCK)<br /><br /> 64 = Pas de verrou (NOLOCK)<br /><br /> 128 = Option de première ligne rapide (FASTFIRST)<br /><br /> 4096 = Lecture des sémantiques répétées lorsqu'elles sont utilisées avec DECLARE CURSOR (HOLDLOCK)<br /><br /> Si vous fournissez plusieurs options, le système utilise les plus restrictives. Toutefois, sp_describe_cursor_tables affiche les indicateurs spécifiés dans la requête.|  
|lock_type|**smallint**|Type de verrou de défilement demandé soit explicitement, soit implicitement pour chaque table de base sous-jacente de ce curseur. Il peut s'agir de l'une des valeurs suivantes :<br /><br /> 0 = Aucun(e)<br /><br /> 1 = Partagé<br /><br /> 3 = Mettre à jour|  
|nom_serveur|**sysname, Nullable**|Nom du serveur lié sur lequel réside la table. Prend la valeur NULL quand OPENQUERY ou OPENROWSET sont utilisés.|  
|objectid|**int**|ID d’objet de la table. Prend la valeur 0 quand OPENQUERY ou OPENROWSET sont utilisés.|  
|dbid|**int**|ID de la base de données dans laquelle réside la table. Prend la valeur 0 quand OPENQUERY ou OPENROWSET sont utilisés.|  
|dbname|**sysname**, **Nullable**|Nom de la base de données dans laquelle réside la table. Prend la valeur NULL quand OPENQUERY ou OPENROWSET sont utilisés.|  
  
## <a name="remarks"></a>Notes  
 La procédure sp_describe_cursor_tables décrit les tables de base qui sont référencées par un curseur de serveur. Utilisez sp_describe_cursor_columns pour obtenir la description des attributs du jeu de résultats retourné par le curseur. Utilisez la procédure sp_describe_cursor pour obtenir la description des caractéristiques globales du curseur, par exemple sa capacité à permettre le défilement et les mises à jour. Utilisez la procédure stockée sp_cursor_list pour obtenir un rapport sur les curseurs côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)] qui sont visibles à la connexion.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 Cet exemple ouvre un curseur global et utilise `sp_describe_cursor_tables` pour fournir un rapport sur les tables qui sont référencées par le curseur.  
  
```  
USE AdventureWorks2012;  
GO  
-- Declare and open a global cursor.  
DECLARE abc CURSOR KEYSET FOR  
SELECT LastName  
FROM Person.Person  
WHERE LastName LIKE 'S%';  
  
OPEN abc;  
GO  
-- Declare a cursor variable to hold the cursor output variable  
-- from sp_describe_cursor_tables.  
DECLARE @Report CURSOR;  
  
-- Execute sp_describe_cursor_tables into the cursor variable.  
EXEC master.dbo.sp_describe_cursor_tables  
      @cursor_return = @Report OUTPUT,  
      @cursor_source = N'global', @cursor_identity = N'abc';  
  
-- Fetch all the rows from the sp_describe_cursor_tables output cursor.  
FETCH NEXT from @Report;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
   FETCH NEXT from @Report;  
END  
  
-- Close and deallocate the cursor from sp_describe_cursor_tables.  
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
 [sp_cursor_list &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursor-list-transact-sql.md)   
 [sp_describe_cursor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-cursor-transact-sql.md)   
 [sp_describe_cursor_columns &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-cursor-columns-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
