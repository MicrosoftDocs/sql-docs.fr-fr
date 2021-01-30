---
description: sp_indexoption (Transact-SQL)
title: sp_indexoption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_indexoption
- sp_indexoption_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_indexoption
ms.assetid: 75f836be-d322-4a53-a45d-25bee6b42a52
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d8f7011bc4ce2120164683009676953ec44d58c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205525"
---
# <a name="sp_indexoption-transact-sql"></a>sp_indexoption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit les valeurs d'option de verrouillage des index cluster et non cluster ou des tables dépourvues d'index cluster définis par l'utilisateur.  
  
 Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] choisit automatiquement le niveau de verrouillage, à savoir table, ligne ou page. Vous n'avez pas besoin de définir ces options manuellement. **sp_indexoption** est fourni aux utilisateurs expérimentés qui savent avec certitude qu’un type particulier de verrou est toujours approprié.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)] Au lieu de cela, utilisez [ALTER INDEX &#40;&#41;Transact-SQL ](../../t-sql/statements/alter-index-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_indexoption [ @IndexNamePattern = ] 'table_or_index_name'   
    , [ @OptionName = ] 'option_name'   
    , [ @OptionValue = ] 'value'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @IndexNamePattern = ] 'table_or_index_name'` Nom qualifié ou non qualifié d’une table ou d’un index défini par l’utilisateur. *table_or_index_name* est de type **nvarchar (1035)**, sans valeur par défaut. Les guillemets ne sont nécessaires que si l'on spécifie un nom qualifié de table ou d'index. Si un nom de table complet (incluant un nom de base de données) est fourni, le nom de base de données doit être celui de la base de données en cours. Si un nom de table est spécifié sans index, la valeur d'option spécifiée est définie pour tous les index de cette table et, si aucun index cluster n'existe, pour la table elle-même.  
  
`[ @OptionName = ] 'option_name'` Nom d’option d’index. *option_name* est de type **varchar (35)**, sans valeur par défaut. *option_name* peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**AllowRowLocks**|Si la valeur est TRUE, les verrous de ligne sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de ligne sont utilisés. Si la valeur est FALSE, les verrous de ligne ne sont pas utilisés. La valeur par défaut est TRUE.|  
|**AllowPageLocks**|Si la valeur est TRUE, les verrous de page sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de page sont utilisés. Si la valeur est FALSE, les verrous de page ne sont pas utilisés. La valeur par défaut est TRUE.|  
|**DisAllowRowLocks**|Si la valeur est TRUE, les verrous de ligne ne sont pas utilisés. Si la valeur est FALSE, les verrous de ligne sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de ligne sont utilisés.|  
|**DisAllowPageLocks**|Si la valeur est TRUE, les verrous de page ne sont pas utilisés. Si la valeur est FALSE, les verrous de page sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de page sont utilisés.|  
  
`[ @OptionValue = ] 'value'` Spécifie si le paramètre *option_name* est activé (true, on, Yes ou 1) ou désactivé (false, OFF, no ou 0). la *valeur* est de type **varchar (12)**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou supérieur à 0 (échec)  
  
## <a name="remarks"></a>Notes  
 Les index XML ne sont pas pris en charge. Si un index XML est spécifié ou qu'un nom de table est spécifié sans nom d'index et que la table contient un index XML, l'instruction échoue. Pour définir ces options, utilisez [ALTER index](../../t-sql/statements/alter-index-transact-sql.md) à la place.  
  
 Pour afficher les propriétés de verrouillage de page et de ligne actuelles, utilisez [INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md) ou l’affichage catalogue [sys. Indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) .  
  
-   Les verrous de ligne, de page et de table sont autorisés lors de l’accès à l’index lorsque **AllowRowLocks** = true ou **DisallowRowLocks** = false, et **AllowPageLocks** = true ou **DisallowPageLocks** = false. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] choisit le verrou approprié et peut promouvoir un verrou de ligne ou de page en verrou de table.  
  
 Seul un verrou au niveau de la table est autorisé lors de l’accès à l’index lorsque **AllowRowLocks** = false ou **DisallowRowLocks** = true et **AllowPageLocks** = false ou **DisallowPageLocks** = true.  
  
 Si un nom de table est spécifié sans index, les paramétrages sont appliqués à tous les index de cette table. Lorsque la table sous-jacente ne possède aucun index cluster (en d'autres termes, il s'agit d'un segment de mémoire), les paramétrages sont appliqués comme suit :  
  
-   Quand **AllowRowLocks** ou **DisallowRowLocks** a la valeur true ou false, le paramètre est appliqué au segment de mémoire et aux index non cluster associés.  
  
-   Lorsque l’option **AllowPageLocks** a la valeur true ou que **DisallowPageLocks** a la valeur false, le paramètre est appliqué au segment de mémoire et aux index non cluster associés.  
  
-   Lorsque l’option **AllowPageLocks** a la valeur false ou que **DisallowPageLocks** a la valeur true, le paramètre est entièrement appliqué aux index non-cluster. En d'autres termes, tous les verrous de page sont interdits sur les index non-cluster. Sur le segment de mémoire, seuls les verrous partagés (S), de mise à jour (U) et exclusifs (X) de la page sont interdits. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut toujours acquérir un verrou de page intentionnel (IS, IU ou IX) à des fins internes.  
  
## <a name="permissions"></a>Autorisations  
 Requiert une autorisation ALTER sur la table.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-setting-an-option-on-a-specific-index"></a>R. Définition d'une option sur un index spécifique  
 L’exemple suivant interdit les verrous de page sur l' `IX_Customer_TerritoryID` index sur la `Customer` table.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_indexoption N'Sales.Customer.IX_Customer_TerritoryID',  
    N'disallowpagelocks', TRUE;  
```  
  
### <a name="b-setting-an-option-on-all-indexes-on-a-table"></a>B. Définition d'une option sur tous les index d'une table  
 L'exemple suivant interdit les verrous de ligne sur tous les index associés à la table `Product`. L'interrogation de l'affichage catalogue `sys.indexes` avant et après l'exécution de la procédure `sp_indexoption` permet d'afficher les résultats de l'instruction.  
  
```sql  
USE AdventureWorks2012;  
GO  
--Display the current row and page lock options for all indexes on the table.  
SELECT name, type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE object_id = OBJECT_ID(N'Production.Product');  
GO  
-- Set the disallowrowlocks option on the Product table.   
EXEC sp_indexoption N'Production.Product',  
    N'disallowrowlocks', TRUE;  
GO  
--Verify the row and page lock options for all indexes on the table.  
SELECT name, type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE object_id = OBJECT_ID(N'Production.Product');  
GO  
```  
  
### <a name="c-setting-an-option-on-a-table-with-no-clustered-index"></a>C. Définition d'une option sur une table dépourvue d'index cluster  
 L'exemple suivant interdit les verrous de page sur une table dépourvue d'index cluster (un segment de mémoire). L' `sys.indexes` affichage catalogue est interrogé avant et après l’exécution de la `sp_indexoption` procédure pour afficher les résultats de l’instruction.  
  
```sql  
USE AdventureWorks2012;  
GO  
--Display the current row and page lock options of the table.   
SELECT OBJECT_NAME (object_id) AS [Table], type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE OBJECT_NAME (object_id) = N'DatabaseLog';  
GO  
-- Set the disallowpagelocks option on the table.   
EXEC sp_indexoption DatabaseLog,  
    N'disallowpagelocks', TRUE;  
GO  
--Verify the row and page lock settings of the table.  
SELECT OBJECT_NAME (object_id) AS [Table], allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE OBJECT_NAME (object_id) = N'DatabaseLog';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  
