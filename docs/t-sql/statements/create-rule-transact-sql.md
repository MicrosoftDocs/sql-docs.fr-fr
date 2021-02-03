---
description: CREATE RULE (Transact-SQL)
title: CREATE RULE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RULE_TSQL
- CREATE RULE
- CREATE_RULE_TSQL
- RULE
dev_langs:
- TSQL
helpviewer_keywords:
- list rules [SQL Server]
- unbinding rules
- pattern rules [SQL Server]
- range rules [SQL Server]
- alias data types [SQL Server], rules
- CREATE RULE statement
- reports [SQL Server], rules
- status information [SQL Server], rules
- rules [SQL Server], precedence
- binding rules [SQL Server]
- rules [SQL Server], creating
ms.assetid: b016a289-3a74-46b1-befc-a13183be51e4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a1751cef0c499a3fff4da9bf86d08e74f35a7eb7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192686"
---
# <a name="create-rule-transact-sql"></a>CREATE RULE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée un objet appelé règle. Lorsqu'elle est liée à une colonne ou à un type de données alias, une règle spécifie les valeurs acceptables qui peuvent être insérées dans cette colonne.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Nous vous recommandons d'utiliser à la place les contraintes de vérification. Celles-ci sont créées à l'aide du mot clé CHECK de l'instruction CREATE TABLE ou ALTER TABLE. Pour plus d’informations, consultez [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
 Une colonne ou un type de données alias ne peut avoir qu'une seule règle liée. Cependant, une colonne peut être liée à une règle et à une ou plusieurs contraintes CHECK. Dans ce cas, toutes les restrictions sont évaluées.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE RULE [ schema_name . ] rule_name   
AS condition_expression  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *schema_name*  
 Nom du schéma auquel appartient la règle.  
  
 *rule_name*  
 Nom de la nouvelle règle. Le nom des règles doit respecter les conventions se rapportant aux [identificateurs](../../relational-databases/databases/database-identifiers.md). Vous n'êtes pas tenu de spécifier le nom du propriétaire de la règle.  
  
 *condition_expression*  
 Condition(s) définissant la règle. Une règle peut être toute expression valide d'une clause WHERE et inclure des éléments tels que des opérateurs arithmétiques, des opérateurs relationnels et des prédicats (par exemple, IN, LIKE, BETWEEN). Elle ne peut pas faire référence à des colonnes ou à d'autres objets de base de données. Vous pouvez y inclure des fonctions intégrées qui ne font pas référence à des objets de base de données. Les fonctions définies par l'utilisateur ne peuvent pas être utilisées.  
  
 *condition_expression* inclut une variable. Le signe arobase ( **@** ) précède chaque variable locale. L'expression fait référence à la valeur entrée à l'aide des instructions UPDATE ou INSERT. Vous pouvez utiliser n’importe quel nom ou symbole pour représenter la valeur lors de la création de la règle, mais le premier caractère doit être le signe arobase ( **@** ).  
  
> [!NOTE]  
>  Évitez de créer des règles d'après des expressions de type alias. Bien que ce cas de figure soit prévu, les expressions ne parviennent pas à se compiler si elles sont référencées après que la liaison des règles aux colonnes ou au type de données alias est créée.  
  
## <a name="remarks"></a>Notes  
 L'instruction CREATE RULE ne peut pas s'utiliser conjointement avec d'autres instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dans un même traitement. Les règles ne s'appliquent pas aux données qui existent déjà dans la base de données au moment de leur création et elles ne peuvent pas être liées aux types de données système.  
  
 Une règle ne peut être créée que dans la base de données actuelle. Après la création de la règle, exécutez **sp_bindrule** pour la lier à une colonne ou à un type de données alias. Une règle doit être compatible avec le type de données de la colonne. Par exemple, « \@value LIKE A% » ne peut pas servir de règle pour une colonne numérique. Une règle ne peut pas être liée à un type **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, **xml**, à un type CLR défini par l’utilisateur, ou à une colonne **timestamp**. Enfin, une règle ne peut pas être liée à une colonne calculée.  
  
 Délimitez les constantes de type caractère et date par des apostrophes droites (') et faites précéder les constantes binaires de 0x. Si la règle n’est pas compatible avec la colonne à laquelle elle est liée, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] retourne un message d’erreur lors de l’insertion d’une valeur, mais pas au moment de la liaison de la règle.  
  
 Une règle liée à un type de données alias n'est activée que lorsque vous essayez d'insérer une valeur ou de mettre à jour une colonne de type alias tirée d'une base de données. Les règles ne testant pas les variables, n'affectez pas de valeur à une variable de type alias qui serait rejetée par une règle liée à une colonne du même type de données.  
  
 Pour obtenir un rapport sur une règle, utilisez **sp_help**. Pour afficher le texte d’une règle, exécutez **sp_helptext** en donnant le nom de la règle comme paramètre. Pour renommer une règle, utilisez **sp_rename**.  
  
 Avant de créer une règle portant le même nom, vous devez supprimer l’ancienne (à l’aide de l’instruction DROP RULE) et supprimer sa liaison avant de la supprimer (à l’aide de **sp_unbindrule**). Utilisez **sp_unbindrul** pour supprimer la liaison d’une règle à une colonne.  
  
 Vous pouvez lier une nouvelle règle à une colonne ou à un type de données sans supprimer la liaison précédente ; dans ce cas, la nouvelle règle supplante l'ancienne. Les règles liées à des colonnes ont toujours priorité sur les règles liées à des types de données alias. La liaison d'une règle à une colonne remplace la règle déjà liée à un type de données alias de cette colonne. En revanche, la liaison d'une règle à un type de données ne remplace pas une règle liée à une colonne de ce type de données alias. Le tableau ci-dessous montre l'ordre de priorité en vigueur lors de la liaison de règles à des colonnes ou à des types de données alias pour lesquelles il existe déjà des règles.  
  
|Nouvelle règle liée à|ancienne règle liée à<br /><br /> un type de données alias|ancienne règle liée à<br /><br /> Colonne|  
|-----------------------|-------------------------------------------|----------------------------------|  
|Type de données d'alias|Ancienne règle remplacée|Aucun changement|  
|Colonne|Ancienne règle remplacée|Ancienne règle remplacée|  
  
 Si une valeur par défaut et une règle sont toutes deux liées à une colonne, la valeur par défaut doit être cohérente avec le domaine défini par la règle. Une valeur par défaut qui est en conflit avec une règle n'est jamais insérée. Le moteur de base de données SQL Server génère un message d'erreur à chaque tentative d'insertion d'une telle valeur.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter CREATE RULE, un utilisateur doit au moins disposer de l'autorisation CREATE RULE sur la base de données actuelle et de l'autorisation ALTER sur le schéma dans lequel la règle est créée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-rule-with-a-range"></a>R. Création d'une règle avec une plage de valeurs  
 L'exemple suivant crée une règle qui limite la plage des entiers insérés dans la ou les colonnes auxquelles la règle est liée.  
  
```sql  
CREATE RULE range_rule  
AS   
@range>= $1000 AND @range <$20000;  
```  
  
### <a name="b-creating-a-rule-with-a-list"></a>B. Création d'une règle avec une liste  
 L'exemple suivant crée une règle qui limite aux seules valeurs répertoriées dans la règle, les valeurs réelles entrées dans la ou les colonnes auxquelles la règle est liée.  
  
```sql  
CREATE RULE list_rule  
AS   
@list IN ('1389', '0736', '0877');  
```  
  
### <a name="c-creating-a-rule-with-a-pattern"></a>C. Création d'une règle avec un modèle  
 L'exemple suivant crée une règle qui respecte le modèle suivant : n'importe quelle suite de deux caractères consécutifs suivis d'un tiret (`-`), d'un nombre quelconque de caractères (ou aucun) et se terminant par un entier compris entre `0` et `9`.  
  
```sql  
CREATE RULE pattern_rule   
AS  
@value LIKE '__-%[0-9]'  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [sp_bindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  
