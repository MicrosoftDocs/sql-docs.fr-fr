---
description: sp_unbindrule (Transact-SQL)
title: sp_unbindrule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_unbindrule_TSQL
- sp_unbindrule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_unbindrule
ms.assetid: f54ee155-c3c9-4f1a-952e-632a8339f0cc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 42b652e2cc604465ac0db1d2c6f65739916c8684
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209682"
---
# <a name="sp_unbindrule-transact-sql"></a>sp_unbindrule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Dissocie une règle d'une colonne ou d'un type de données alias dans la base de données active.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Nous vous recommandons de créer à la place des définitions default en utilisant le mot clé DEFAULT dans les instructions [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou [Create table](../../t-sql/statements/create-table-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_unbindrule [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @objname = ] 'object_name'` Nom de la table et de la colonne ou du type de données alias à partir duquel la règle est détachée. *object_name* est de type **nvarchar (776)**, sans valeur par défaut. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] essaie d'abord de résoudre en noms de colonnes les identificateurs en deux parties, puis en types de données alias. Lorsque vous dissociez une règle d'un type de données alias, les colonnes de ce type de données ayant la même règle sont également dissociées. Les colonnes de ce type de données auxquelles des règles sont directement liées ne sont pas affectées.  
  
> [!NOTE]  
>  les *object_name* peuvent contenir des crochets **[]** comme des caractères d’identificateur délimités. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
`[ @futureonly = ] 'futureonly_flag'` Est utilisé uniquement lors de la dissociation d’une règle d’un type de données alias. *futureonly_flag* est de type **varchar (15)**, avec NULL comme valeur par défaut. Lorsque *futureonly_flag* est **futureonly**, les colonnes existantes de ce type de données ne perdent pas la règle spécifiée.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Pour afficher le texte d’une règle, exécutez **sp_helptext** en donnant le nom de la règle comme paramètre.  
  
 Lorsqu’une règle est indépendante, les informations sur la liaison sont supprimées de la table **sys. Columns** si la règle a été liée à une colonne, et à partir de la table **sys. types** si la règle a été liée à un type de données d’alias.  
  
 Lorsqu'une règle est dissociée d'un type de données alias, elle l'est aussi des colonnes ayant ce type de données alias. La règle peut également être liée à des colonnes dont les types de données ont été modifiés ultérieurement par la clause ALTER COLUMN d’une instruction ALTER TABLE, vous devez détacher spécifiquement la règle de ces colonnes à l’aide de **sp_unbindrule** et en spécifiant le nom de la colonne.  
  
## <a name="permissions"></a>Autorisations  
 La dissociation d'une règle d'une colonne de table nécessite l'autorisation ALTER sur la table. La dissociation d'une règle d'un type de données alias nécessite l'autorisation CONTROL sur le type ou l'autorisation ALTER sur le schéma auquel le type appartient.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-unbinding-a-rule-from-a-column"></a>R. Dissociation d'une règle d'une colonne  
 L'exemple suivant dissocie la règle de la colonne `startdate` de la table `employees`.  
  
```  
EXEC sp_unbindrule 'employees.startdate';  
```  
  
### <a name="b-unbinding-a-rule-from-an-alias-data-type"></a>B. Dissociation d'une règle d'un type de données alias  
 L'exemple suivant dissocie la règle du type de données alias `ssn`. Il dissocie la règle des colonnes existantes et à venir de ce type.  
  
```  
EXEC sp_unbindrule ssn;  
```  
  
### <a name="c-using-futureonly_flag"></a>C. Utilisation de l'argument futureonly_flag  
 L'exemple suivant dissocie la règle du type de données alias `ssn` sans affecter les colonnes `ssn` existantes.  
  
```  
EXEC sp_unbindrule 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. Utilisation d’identificateurs délimités  
 L’exemple suivant montre l’utilisation d’identificateurs délimités dans le paramètre *object_name* .  
  
```  
CREATE TABLE [t.4] (c1 int); -- Notice the period as part of the table   
-- name.  
GO  
CREATE RULE rule2 AS @value > 100;  
GO  
EXEC sp_bindrule rule2, '[t.4].c1' -- The object contains two   
-- periods; the first is part of the table name and the second   
-- distinguishes the table name from the column name.  
GO  
EXEC sp_unbindrule '[t.4].c1';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [sp_bindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
