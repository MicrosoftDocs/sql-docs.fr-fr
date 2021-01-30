---
description: sp_bindrule (Transact-SQL)
title: sp_bindrule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_bindrule_TSQL
- sp_bindrule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_bindrule
ms.assetid: 2606073e-c52f-498d-a923-5026b9d97e67
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 266707eca11f6664d816a958e9a3051d3648339b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197697"
---
# <a name="sp_bindrule-transact-sql"></a>sp_bindrule (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Lie une règle à une colonne ou un type de données d'alias.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez[des contraintes unique et des contraintes de validation à](../../relational-databases/tables/unique-constraints-and-check-constraints.md) la place. Les contraintes CHECK sont créées à l’aide du mot clé CHECK des instructions [Create table](../../t-sql/statements/create-table-transact-sql.md) ou [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_bindrule [ @rulename = ] 'rule' ,   
     [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @rulename = ] 'rule'` Nom d’une règle créée par l’instruction CREATe RULE. *rule* est de type **nvarchar (776)**, sans valeur par défaut.  
  
`[ @objname = ] 'object_name'` Table et colonne, ou type de données d’alias auquel la règle doit être liée. Une règle ne peut pas être liée à un type **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, **xml**, à un type CLR défini par l’utilisateur, ou à une colonne **timestamp**. Enfin, une règle ne peut pas être liée à une colonne calculée.  
  
 *object_name* est de type **nvarchar (776)** sans valeur par défaut. Si *object_name* est un nom en une partie, il est résolu en tant que type de données d’alias. S'il s'agit d'un nom en deux ou trois parties, il est d'abord résolu en tant que table et colonne. Si la résolution échoue, il est résolu en tant que type de données d'alias. Par défaut, les colonnes existantes du type de données alias héritent de la *règle* , sauf si une règle a été liée directement à la colonne.  
  
> [!NOTE]  
>  *object_name* peut contenir les caractères **[** et **]** entre crochets comme des caractères d’identificateur délimités. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
>  Les règles créées sur des expressions utilisant des types de données d'alias peuvent être liées à des colonnes ou des types de données d'alias, mais ne peuvent pas être compilées lorsqu'elles sont référencées. Évitez d'utiliser des règles créées sur des types de données d'alias.  
  
`[ @futureonly = ] 'futureonly_flag'` Est utilisé uniquement lors de la liaison d’une règle à un type de données d’alias. *future_only_flag* est de type **varchar (15)** avec NULL comme valeur par défaut. Lorsque la valeur de ce paramètre est définie sur **futureonly** , les colonnes existantes d’un type de données alias héritent de la nouvelle règle. Si *futureonly_flag* a la valeur null, la nouvelle règle est liée à toutes les colonnes du type de données de l’alias qui n’ont actuellement aucune règle ou qui utilisent la règle existante du type de données de l’alias.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Vous pouvez lier une nouvelle règle à une colonne (même si vous préférez utiliser une contrainte CHECK) ou à un type de données alias avec **sp_bindrule** sans dissocier une règle existante. L'ancienne règle est remplacée par la nouvelle. Si une règle est associée à une colonne à l'aide d'une contrainte CHECK existante, toutes les restrictions sont évaluées. Vous ne pouvez pas lier de règle à un type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La règle s'impose lors d'une tentative d'insertion de données (INSERT) et non au moment de la liaison. Vous pouvez lier une règle de caractères à une colonne de type de données **numérique** , bien qu’une telle opération d’insertion ne soit pas valide.  
  
 Les colonnes existantes du type de données alias héritent de la nouvelle règle, sauf si *futureonly_flag* est spécifié comme **futureonly**. Les nouvelles colonnes définies avec le type de données d'alias héritent toujours de la règle. Si la clause ALTER COLUMN d'une instruction ALTER TABLE change le type de données d'une colonne en un type de données d'alias lié à une règle, la colonne n'hérite pas de cette règle liée au type de données. La règle doit être spécifiquement liée à la colonne à l’aide de **sp_bindrule**.  
  
 Lorsque vous liez une règle à une colonne, les informations connexes sont ajoutées à la table **sys. Columns** . Lorsque vous liez une règle à un type de données d’alias, des informations connexes sont ajoutées à la table **sys. types** .  
  
## <a name="permissions"></a>Autorisations  
 Pour lier une règle à une colonne de table, une autorisation ALTER est nécessaire sur la table. Pour lier une règle à un type de données d'alias, il est nécessaire de disposer d'une autorisation CONTROL sur le type de données d'alias ou d'une autorisation ALTER sur le schéma auquel le type appartient.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-binding-a-rule-to-a-column"></a>R. Liaison d'une règle à une colonne  
 En supposant qu'une règle nommée `today` ait été créée dans la base de données active par l'instruction CREATE RULE, l'exemple suivant lie la règle à la colonne `HireDate` de la table `Employee`. Quand une ligne est ajoutée à la table `Employee`, SQL Server vérifie si les données fournies pour la colonne `HireDate` respectent la règle `today`.  
  
```  
USE master;  
GO  
EXEC sp_bindrule 'today', 'HumanResources.Employee.HireDate';  
```  
  
### <a name="b-binding-a-rule-to-an-alias-data-type"></a>B. Liaison d'une règle à un type de données d'alias  
 En supposant l'existence d'une règle nommée `rule_ssn` et d'un type de données d'alias nommé `ssn`, cet exemple lie `rule_ssn` à `ssn`. Toutes les colonnes créées par une instruction CREATE TABLE avec le type de données `ssn` héritent de la règle `rule_ssn`. Les colonnes de type existantes `ssn` héritent également de la `rule_ssn` règle, sauf si **futureonly** est spécifié pour *futureonly_flag* ou `ssn` a une règle liée directement à celle-ci. Les règles liées à des colonnes ont toujours priorité sur les règles liées à des types de données.  
  
```  
USE master;  
GO  
EXEC sp_bindrule 'rule_ssn', 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. Utilisation de l’futureonly_flag  
 L'exemple suivant lie la règle `rule_ssn` au type de données d'alias `ssn`. Puisque l'option `futureonly` est incluse, aucune colonne existante de type `ssn` n'est affectée.  
  
```  
USE master;  
GO  
EXEC sp_bindrule rule_ssn, 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. Utilisation d’identificateurs délimités  
 L’exemple suivant illustre l’utilisation d’identificateurs délimités dans *object_name* paramètre.  
  
```  
USE master;  
GO  
CREATE TABLE [t.2] (c1 int) ;  
-- Notice the period as part of the table name.  
EXEC sp_bindrule rule1, '[t.2].c1' ;  
-- The object contains two periods;   
-- the first is part of the table name   
-- and the second distinguishes the table name from the column name.  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [sp_unbindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
