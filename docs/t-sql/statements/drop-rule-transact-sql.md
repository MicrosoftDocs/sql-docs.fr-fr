---
description: DROP RULE (Transact-SQL)
title: DROP RULE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_RULE_TSQL
- DROP RULE
dev_langs:
- TSQL
helpviewer_keywords:
- rules [SQL Server], removing
- deleting roles
- DROP RULE statement
- removing roles
- dropping roles
ms.assetid: 8370b730-7fd5-43fe-a7f6-8300b3caa16d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 75296a91d239b90d758f7be539861f26cd0c95ab
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237000"
---
# <a name="drop-rule-transact-sql"></a>DROP RULE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime une ou plusieurs règles définies par l'utilisateur de la base de données active.  
  
> [!IMPORTANT]
>  La fonction DROP RULE sera supprimée dans la prochaine version de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez de l’utiliser dans tout nouveau travail de développement et prévoyez la modification des applications qui s’en servent actuellement. À la place, utilisez des contraintes CHECK que vous pouvez créer à l’aide du mot clé CHECK de [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) ou [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md). Pour plus d’informations, consultez [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DROP RULE [ IF EXISTS ] { [ schema_name . ] rule_name } [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *IF EXISTS*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] à la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Supprime, de manière conditionnelle, la règle uniquement si elle existe déjà.  
  
 *schema_name*  
 Nom du schéma auquel appartient la règle.  
  
 *rule*  
 Règle à supprimer. Le nom des règles doit respecter les conventions se rapportant aux [identificateurs](../../relational-databases/databases/database-identifiers.md). Vous n'êtes pas obligé de spécifier le nom du schéma de la règle.  
  
## <a name="remarks"></a>Notes  
 Pour supprimer une règle, vous devez tout d'abord la dissocier si celle-ci est liée à une colonne ou à un type de données d'alias. Pour dissocier la règle, utilisez **sp_unbindrule**. Si la règle est liée lorsque vous tentez de la supprimer, un message d'erreur s'affiche et l'instruction DROP RULE est annulée.  
  
 Après la suppression d'une règle, les nouvelles données sont entrées sans les contraintes de la règle dans les colonnes gouvernées au préalable par celle-ci. Les données existantes ne sont pas affectées.  
  
 L'instruction DROP RULE ne s'applique pas aux contraintes CHECK. Pour plus d’informations sur la suppression des contraintes CHECK, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter DROP RULE, un utilisateur doit, au minimum, posséder l'autorisation ALTER sur le schéma auquel la règle appartient.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant dissocie et supprime la règle `VendorID_rule`. 
  
```sql  
sp_unbindrule 'Production.ProductVendor.VendorID'  
DROP RULE VendorID_rule  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [sp_bindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_unbindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md)