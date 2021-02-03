---
description: IDENT_CURRENT (Transact-SQL)
title: IDENT_CURRENT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IDENT_CURRENT
- IDENT_CURRENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- last identity value generated for table
- identity values [SQL Server], last generated
- identity columns, current value
- IDENT_CURRENT function
ms.assetid: 21517ced-39f5-4cd8-8d9c-0a0b8aff554a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 608f7c4745d2d5590fc4d2956f7fca580c9b4111
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184009"
---
# <a name="ident_current-transact-sql"></a>IDENT_CURRENT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne la dernière valeur d'identité générée pour une table ou une vue spécifiée. La dernière valeur d'identité générée peut être pour toute session et toute étendue.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
IDENT_CURRENT( 'table_or_view' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*table_or_view*  
Nom de la table ou de la vue dont la valeur d’identité est retournée. *table_or_view* est de type **varchar**, sans valeur par défaut.  
  
## <a name="return-types"></a>Types de retour  
**numeric**([@@MAXPRECISION](../../t-sql/functions/max-precision-transact-sql.md),0))  
  
## <a name="exceptions"></a>Exceptions  
Retourne la valeur NULL en cas d'erreur ou si un appelant n'est pas autorisé à afficher l'objet.  
  
Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un utilisateur peut voir uniquement les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d'un droit d'accès. Cela signifie que les fonctions intégrées générant des métadonnées, telles que IDENT_CURRENT, peuvent retourner la valeur NULL si l'utilisateur ne dispose d'aucune autorisation sur l'objet. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Remarques  
IDENT_CURRENT est similaire aux fonctions d’identité SCOPE_IDENTITY et @@IDENTITY de [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Ces trois fonctions renvoient les dernières valeurs d'identité générées. Toutefois, l’étendue et la session dans lesquelles le paramètre *last* est défini diffèrent pour chacune de ces fonctions :  

-   IDENT_CURRENT renvoie la dernière valeur d'identité générée pour une table spécifique dans n'importe quelles session et étendue.  
-   @@IDENTITY renvoie la dernière valeur d’identité générée pour n’importe quelle table dans la session en cours, sur l’ensemble des étendues.  
-   SCOPE_IDENTITY renvoie la dernière valeur d'identité générée pour n'importe quelle table dans la session et l'étendue en cours.  
  
Lorsque la valeur IDENT_CURRENT est NULL (si la table n'a jamais contenu de ligne ou a été tronquée), la fonction IDENT_CURRENT retourne la valeur de départ.  
  
Les instructions et les transactions en échec peuvent modifier l'identité actuelle d'une table et créer des trous dans les valeurs des colonnes d'identité. La valeur d'identité n'est jamais annulée, même si la transaction qui a essayé d'insérer la valeur dans la table n'est pas validée. Par exemple, si une instruction INSERT échoue à cause d'une violation d'identité IGNORE_DUP_KEY, la valeur d'identité actuelle de la table augmente quand même d'une unité.  

Quand vous utilisez IDENT_CURRENT sur une vue qui contient des jointures, la valeur NULL est retournée. Ceci est valable indépendamment du fait qu’une seule ou plusieurs tables jointes aient une colonne d’identité. 
  
> [!IMPORTANT]
> Soyez prudent quand vous utilisez IDENT_CURRENT pour prévoir la valeur d’identité générée suivante. La valeur effectivement générée peut être différente de la somme des valeurs IDENT_CURRENT et IDENT_INCR si d'autres sessions effectuent des insertions.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-last-identity-value-generated-for-a-specified-table"></a>R. Renvoi de la dernière valeur d'identité générée pour une table spécifiée  
 L'exemple suivant renvoie la dernière valeur d'identité générée pour la table `Person.Address` de la base de données `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT IDENT_CURRENT ('Person.Address') AS Current_Identity;  
GO  
```  
  
### <a name="b-comparing-identity-values-returned-by-ident_current-identity-and-scope_identity"></a>B. Comparaison des valeurs d’identité renvoyées par IDENT_CURRENT, @@IDENTITY et SCOPE_IDENTITY  
 L'exemple suivant affiche les valeurs d'identité différentes renvoyées par `IDENT_CURRENT`, `@@IDENTITY` et `SCOPE_IDENTITY`.  
  
```sql 
USE AdventureWorks2012;  
GO  
IF OBJECT_ID(N't6', N'U') IS NOT NULL   
    DROP TABLE t6;  
GO  
IF OBJECT_ID(N't7', N'U') IS NOT NULL   
    DROP TABLE t7;  
GO  
CREATE TABLE t6(id INT IDENTITY);  
CREATE TABLE t7(id INT IDENTITY(100,1));  
GO  
CREATE TRIGGER t6ins ON t6 FOR INSERT   
AS  
BEGIN  
   INSERT t7 DEFAULT VALUES  
END;  
GO  
--End of trigger definition  
  
SELECT id FROM t6;  
--IDs empty.  
  
SELECT id FROM t7;  
--ID is empty.  
  
--Do the following in Session 1  
INSERT t6 DEFAULT VALUES;  
SELECT @@IDENTITY;  
/*Returns the value 100. This was inserted by the trigger.*/  
  
SELECT SCOPE_IDENTITY();  
/* Returns the value 1. This was inserted by the   
INSERT statement two statements before this query.*/  
  
SELECT IDENT_CURRENT('t7');  
/* Returns value inserted into t7, that is in the trigger.*/  
  
SELECT IDENT_CURRENT('t6');  
/* Returns value inserted into t6. This was the INSERT statement four statements before this query.*/  
  
-- Do the following in Session 2.  
SELECT @@IDENTITY;  
/* Returns NULL because there has been no INSERT action   
up to this point in this session.*/  
  
SELECT SCOPE_IDENTITY();  
/* Returns NULL because there has been no INSERT action   
up to this point in this scope in this session.*/  
  
SELECT IDENT_CURRENT('t7');  
/* Returns the last value inserted into t7.*/  
```  
  
## <a name="see-also"></a>Voir aussi  
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)   
 [SCOPE_IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/scope-identity-transact-sql.md)   
 [IDENT_INCR &#40;Transact-SQL&#41;](../../t-sql/functions/ident-incr-transact-sql.md)   
 [IDENT_SEED &#40;Transact-SQL&#41;](../../t-sql/functions/ident-seed-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
