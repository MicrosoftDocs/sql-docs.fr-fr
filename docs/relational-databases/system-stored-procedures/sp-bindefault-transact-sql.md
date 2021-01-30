---
description: sp_bindefault (Transact-SQL)
title: sp_bindefault (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_bindefault
- sp_bindefault_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_bindefault
ms.assetid: 3da70c10-68d0-4c16-94a5-9e84c4a520f6
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4aacb6bff81f204e89e8704406d61ffb69ae84fb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199194"
---
# <a name="sp_bindefault-transact-sql"></a>sp_bindefault (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Lie une valeur par défaut à une colonne ou à un type de données d'alias.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Nous vous recommandons de créer des définitions par défaut à l’aide du mot clé DEFAULT des instructions [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou [Create table](../../t-sql/statements/create-table-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_bindefault [ @defname = ] 'default' ,   
    [ @objname = ] 'object_name'   
    [ , [ @futureonly = ] 'futureonly_flag' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @defname = ] 'default'` Nom de la valeur par défaut créée par CREATe DEFAULT. la *valeur par défaut* est **nvarchar (776)**, sans valeur par défaut.  
  
`[ @objname = ] 'object_name'` Nom de la table et de la colonne ou type de données de l’alias auquel la valeur par défaut doit être liée. *object_name* est de type **nvarchar (776)** sans valeur par défaut. *object_name* ne peut pas être défini avec les types **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML** ou CLR définis par l’utilisateur.  
  
 Si *object_name* est un nom en une partie, il est résolu en tant que type de données d’alias. S'il s'agit d'un nom à deux ou trois composantes, il est d'abord résolu en tant que table et colonne. Si la résolution échoue, il est résolu en tant que type de données d'alias. Par défaut, les colonnes existantes du type de données alias héritent *par défaut*, sauf si une valeur par défaut a été liée directement à la colonne. Une valeur par défaut ne peut pas être liée à une colonne de type **Text**, **ntext**, **image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML**, **timestamp** ou CLR défini par l’utilisateur, une colonne avec la propriété Identity, une colonne calculée ou une colonne qui a déjà une contrainte default.  
  
> [!NOTE]  
>  les *object_name* peuvent contenir des crochets **[]** comme identificateurs délimités. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
`[ @futureonly = ] 'futureonly_flag'` Est utilisé uniquement lors de la liaison d’une valeur par défaut à un type de données alias. *futureonly_flag* est de type **varchar (15)** avec NULL comme valeur par défaut. Lorsque ce paramètre est défini sur **futureonly**, les colonnes existantes de ce type de données ne peuvent pas hériter de la nouvelle valeur par défaut. Il ne s'emploie jamais lors de la liaison d'une valeur par défaut à une colonne. Si *futureonly_flag* a la valeur null, la nouvelle valeur par défaut est liée à toute colonne du type de données de l’alias qui n’a pas de valeur par défaut ou qui utilise la valeur par défaut existante du type de données de l’alias.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Vous pouvez utiliser **sp_bindefault** pour lier une nouvelle valeur par défaut à une colonne, bien que l’utilisation de la contrainte par défaut soit préférable ou à un type de données alias sans dissocier une valeur par défaut existante. L'ancienne valeur par défaut est remplacée par la nouvelle. Vous ne pouvez pas lier une valeur par défaut à une type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à un type CLR défini par l'utilisateur. En cas d'incompatibilité entre la valeur par défaut et la colonne à laquelle vous l'avez liée, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] retourne un message d'erreur quand il essaie d'insérer la valeur par défaut et non au moment de sa liaison.  
  
 Les colonnes existantes du type de données alias héritent de la nouvelle valeur par défaut, sauf si une valeur par défaut est liée directement à ces dernières ou *futureonly_flag* est spécifié comme **futureonly**. Les nouvelles colonnes qui utilisent le type de données d'alias héritent toujours la valeur par défaut.  
  
 Lorsque vous liez une valeur par défaut à une colonne, les informations connexes sont ajoutées à la vue de catalogue **sys. Columns** . Lorsque vous liez une valeur par défaut à un type de données d’alias, les informations connexes sont ajoutées à l’affichage catalogue **sys. types** .  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit être propriétaire de la table ou être membre du rôle serveur fixe **sysadmin** ou des rôles de base de données fixes **db_owner** et **db_ddladmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-binding-a-default-to-a-column"></a>R. Liaison d'une valeur par défaut à une colonne  
 Une valeur par défaut nommée `today` a été définie dans la base de données actuelle par une instruction CREATE DEFAULT, l'exemple suivant lie cette valeur par défaut à la colonne `HireDate` de la table `Employee`. Si aucune valeur n'est fournie pour la colonne `Employee` lors de l'ajout d'une ligne dans la table `HireDate`, la colonne prend la valeur de `today` par défaut.  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'today', 'HumanResources.Employee.HireDate';  
```  
  
### <a name="b-binding-a-default-to-an-alias-data-type"></a>B. Liaison d'une valeur par défaut à un type de données d'alias  
 Une valeur par défaut nommée `def_ssn` et un type de données d'alias nommé `ssn` existent déjà. L'exemple suivant lie la valeur par défaut `def_ssn` à `ssn`. Lors de la création d'une table, toutes les colonnes affectées au type de données d'alias `ssn` héritent la valeur par défaut. Les colonnes existantes de type **SSN** héritent également de la **def_ssn** par défaut, sauf si **futureonly** est spécifié pour *futureonly_flag* valeur, ou à moins que la colonne ait directement une liaison par défaut. Les valeurs par défaut liées aux colonnes ont toujours priorité sur celles liées à des types de données.  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'def_ssn', 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. Utilisation de l’futureonly_flag  
 L'exemple suivant lie la valeur par défaut `def_ssn` à un type de données d'alias `ssn`. Comme **futureonly** est spécifié, aucune colonne existante de type `ssn` n’est affectée.  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'def_ssn', 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. Utilisation d’identificateurs délimités  
 L’exemple suivant montre l’utilisation d’identificateurs délimités, `[t.1]` , dans *object_name*.  
  
```  
USE master;  
GO  
CREATE TABLE [t.1] (c1 int);   
-- Notice the period as part of the table name.  
EXEC sp_bindefault 'default1', '[t.1].c1' ;  
-- The object contains two periods;   
-- the first is part of the table name,   
-- and the second distinguishes the table name from the column name.  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
