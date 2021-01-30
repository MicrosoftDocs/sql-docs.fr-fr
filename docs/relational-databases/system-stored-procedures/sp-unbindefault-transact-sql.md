---
description: sp_unbindefault (Transact-SQL)
title: sp_unbindefault (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_unbindefault_TSQL
- sp_unbindefault
dev_langs:
- TSQL
helpviewer_keywords:
- sp_unbindefault
ms.assetid: c96a6c5e-f3ca-4c1e-b64b-0d8ef6986af8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ecc8a6c7a012a36da4deeacf8b4e66e6096c8227
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202349"
---
# <a name="sp_unbindefault-transact-sql"></a>sp_unbindefault (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime la liaison d'une valeur par défaut (ou supprime cette valeur) avec une colonne ou un type de données alias dans la base de données actuelle.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Nous vous recommandons de créer à la place des définitions default en utilisant le mot clé DEFAULT dans les instructions [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou [Create table](../../t-sql/statements/create-table-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_unbindefault [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @objname = ] 'object_name'` Nom de la table et de la colonne ou du type de données alias à partir duquel la valeur par défaut doit être détachée. *object_name* est de type **nvarchar (776)**, sans valeur par défaut. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] essaie d'abord de résoudre en noms de colonnes les identificateurs en deux parties, puis en types de données alias.  
  
 Lorsque vous supprimez la liaison d'une valeur par défaut avec un type de données alias, toutes les colonnes de ce type de données ayant la même valeur par défaut sont également dissociées. Les colonnes de ce type de données auxquelles des valeurs par défaut sont directement liées ne sont pas affectées.  
  
> [!NOTE]  
>  les *object_name* peuvent contenir des crochets **[]** comme des caractères d’identificateur délimités. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
`[ @futureonly = ] 'futureonly_flag'` Est utilisé uniquement pour dissocier une valeur par défaut d’un type de données alias. *futureonly_flag* est de type **varchar (15)**, avec NULL comme valeur par défaut. Lorsque *futureonly_flag* est **futureonly**, les colonnes existantes du type de données ne perdent pas la valeur par défaut spécifiée.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Pour afficher le texte d’une valeur par défaut, exécutez **sp_helptext** avec le nom de la valeur par défaut en tant que paramètre.  
  
## <a name="permissions"></a>Autorisations  
 Pour dissocier une valeur par défaut d'une colonne de table, une autorisation ALTER sur cette table est requise. Pour supprimer la liaison d'une valeur par défaut avec un type de données, il faut disposer d'une autorisation CONTROL sur ce type ou d'une autorisation ALTER sur le schéma auquel appartient ce type.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-unbinding-a-default-from-a-column"></a>R. Suppression de la liaison d'une valeur par défaut avec une colonne  
 Cet exemple supprime la liaison de la valeur par défaut avec la colonne `hiredate` de la table `employees`.  
  
```  
EXEC sp_unbindefault 'employees.hiredate';  
```  
  
### <a name="b-unbinding-a-default-from-an-alias-data-type"></a>B. Suppression de la liaison d'une valeur par défaut avec un type de données alias  
 Cet exemple supprime la liaison de la valeur par défaut avec le type de données alias `ssn`. Il supprime la liaison des colonnes existantes et futures de ce type.  
  
```  
EXEC sp_unbindefault 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. Utilisation de l’futureonly_flag  
 Cet exemple supprime la liaison des utilisations futures du type de données alias `ssn` sans affecter les colonnes existantes de type `ssn`.  
  
```  
EXEC sp_unbindefault 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. Utilisation d’identificateurs délimités  
 L’exemple suivant montre l’utilisation d’identificateurs délimités dans *object_name* paramètre.  
  
```  
CREATE TABLE [t.3] (c1 int); -- Notice the period as part of the table   
-- name.  
CREATE DEFAULT default2 AS 0;  
GO  
EXEC sp_bindefault 'default2', '[t.3].c1' ;  
-- The object contains two periods;  
-- the first is part of the table name and the second   
-- distinguishes the table name from the column name.  
EXEC sp_unbindefault '[t.3].c1';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [sp_bindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
