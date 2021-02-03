---
description: CREATE DEFAULT (Transact-SQL)
title: CREATE DEFAULT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_DEFAULT_TSQL
- CREATE DEFAULT
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], default
- default objects
- CREATE DEFAULT statement
- objects [SQL Server], creating
- DEFAULT definition
ms.assetid: 08475db4-7d90-486a-814c-01a99d783d41
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c1ebdb3e7282fba1f22eedcda9278e44a22de424
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186901"
---
# <a name="create-default-transact-sql"></a>CREATE DEFAULT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Crée un objet appelé default (valeur par défaut). Lorsqu'elle est liée à une colonne ou à un type de données alias, une valeur par défaut spécifie une valeur à insérer dans la colonne à laquelle est lié l'objet (ou dans toutes les colonnes, dans le cas d'un type de données alias) lorsqu'aucune valeur n'est explicitement fournie lors d'une opération d'insertion.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt des définitions par défaut créées à l'aide du mot clé DEFAULT ou de ALTER TABLE ou CREATE TABLE.  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
CREATE DEFAULT [ schema_name . ] default_name   
AS constant_expression [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*schema_name*  
 Nom du schéma auquel appartient la valeur par défaut.  
  
*default_name*  
 Nom de la valeur par défaut. Le nom des valeurs par défaut doit respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md). Vous n'êtes pas obligé de spécifier le nom du propriétaire par défaut.  
  
*constant_expression*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) qui ne contient que des valeurs constantes (ne peut pas inclure les noms de colonnes ou d’autres objets de base de données). Vous pouvez utiliser toute constante, fonction intégrée ou expression mathématique, à l'exception de celles contenant des types d'alias. Les fonctions définies par l’utilisateur ne peuvent pas être utilisées. Placez les constantes de caractère et de date entre guillemets simples (**'**) ; les constantes monétaires, entières et à virgule flottante ne nécessitent pas de guillemets. Les données binaires doivent être précédées de 0x et les données monétaires du signe dollar ($). La valeur par défaut doit être compatible avec le type de données de la colonne.  
  
## <a name="remarks"></a>Notes  
 Vous ne pouvez créer un nom de valeur par défaut que dans la base de données active. Au sein d'une base de données, les noms de valeurs par défaut doivent être uniques pour chaque schéma. Quand vous créez une valeur par défaut, utilisez **sp_bindefault** pour la lier à une colonne ou à un type de données alias.  
  
 En cas d'incompatibilité entre la valeur par défaut et la colonne à laquelle elle est liée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un message d'erreur lors d'une tentative d'insertion de la valeur par défaut. Par exemple, vous ne pouvez pas utiliser N/A comme valeur par défaut pour une colonne de type **numérique**.  
  
 Si la valeur par défaut est trop longue pour la colonne à laquelle elle est liée, la valeur est tronquée.  
  
 L'instruction CREATE DEFAULT ne peut pas s'utiliser conjointement avec d'autres instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dans un même traitement d'instructions.  
  
 Une valeur par défaut doit être supprimée avant d’en créer une nouvelle avec le même nom. Et la valeur par défaut doit être déliée en exécutant **sp_unbindefault** avant sa suppression.  
  
 Si une colonne possède à la fois une valeur par défaut et une règle qui lui est associée, la valeur par défaut ne peut pas enfreindre la règle. Une valeur par défaut qui est en conflit avec une règle n'est jamais insérée et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un message d'erreur à chaque tentative d'insertion de la valeur par défaut.  
  
 Lorsqu'elle est liée à une colonne, une valeur par défaut est insérée lorsque :  
  
-   une valeur n'est pas entrée explicitement ;  
  
-   soit les mots clés DEFAULT VALUES ou DEFAULT sont utilisés avec INSERT pour insérer des valeurs par défaut.  
  
 Si vous précisez NOT NULL lors de la création d'une colonne et ne créez aucune valeur par défaut pour cette colonne, le système génère un message d'erreur lorsque l'utilisateur ne parvient pas à effectuer une entrée dans cette colonne. Le tableau suivant illustre les relations entre l'existence d'une valeur par défaut et la définition d'une colonne à NULL ou NOT NULL (valeurs NULL autorisées ou non). Les entrées du tableau indiquent les résultats.  
  
|Définition de la colonne|Pas d'entrée, pas de valeur par défaut|Pas d'entrée, valeur par défaut|Entrée NULL, pas de valeur par défaut|Entrée NULL, valeur par défaut|  
|-----------------------|--------------------------|-----------------------|----------------------------|-------------------------|  
|**NULL**|NULL|default|NULL|NULL|  
|**NOT NULL**|Erreur|default|error|error|  
  
 Pour renommer une valeur par défaut, utilisez **sp_rename**. Pour obtenir un rapport sur une valeur par défaut, utilisez **sp_help**.  
  
## <a name="permissions"></a>Autorisations  
 Pour utiliser CREATE DEFAULT, un utilisateur doit posséder, au minimum, une autorisation CREATE DEFAULT sur la base de données active et une autorisation ALTER sur le schéma dans lequel la valeur par défaut est créée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-simple-character-default"></a>R. Création d'une valeur par défaut simple de type caractère  
 L'exemple suivant crée une valeur par défaut de type caractère appelée `unknown`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DEFAULT phonedflt AS 'unknown';  
```  
  
### <a name="b-binding-a-default"></a>B. Liaison d'une valeur par défaut  
 L'exemple suivant lie la valeur par défaut créée dans l'exemple A. Celle-ci ne prend effet que si aucune entrée n'est spécifiée pour la colonne `Phone` de la table `Contact`. 
 
 > [!Note] 
 >  Cela ne revient pas du tout au même d'omettre une entrée ou de spécifier explicitement une valeur NULL dans une instruction INSERT.  
  
 Puisqu'il n'existe pas de valeur par défaut nommée `phonedflt`, l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante échoue. Cet exemple n'est donné qu'à titre indicatif.  
  
```sql  
USE AdventureWorks2012;  
GO  
sp_bindefault 'phonedflt', 'Person.PersonPhone.PhoneNumber';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [sp_bindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)  
  
  
