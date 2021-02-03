---
description: DROP TYPE (Transact-SQL)
title: DROP TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP TYPE
- DROP_TYPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- user-defined types [SQL Server], deleting
- UDTs [SQL Server], deleting
- alias data types [SQL Server], removing
- DROP TYPE statement
ms.assetid: 11bf83f9-0718-4238-a835-83d2eb14ae7b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a22856b7bbc61df5ccb9b47c2538eab4d0a0af85
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237774"
---
# <a name="drop-type-transact-sql"></a>DROP TYPE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Supprime de la base de données active un type de données alias ou un type de données CLR (Common Language Runtime) défini par l'utilisateur.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DROP TYPE [ IF EXISTS ] [ schema_name. ] type_name [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *IF EXISTS*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] à la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Supprime, de manière conditionnelle, le type uniquement s’il existe déjà.  
  
 *schema_name*  
 Nom du schéma auquel appartient le type de données alias ou défini par l'utilisateur  
  
 *TYPE_NAME*  
 Nom du type de données alias ou défini par l'utilisateur à supprimer  
  
## <a name="remarks"></a>Notes  
 L'instruction DROP TYPE ne s'exécutera pas si l'une des informations suivantes est vraie :  
  
-   Des tables de la base de données contiennent des colonnes du type de données alias ou du type défini par l'utilisateur. Il est possible d’obtenir des informations sur les colonnes de type alias ou défini par l’utilisateur en interrogeant la vue de catalogue [sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) ou [sys.column_type_usages](../../relational-databases/system-catalog-views/sys-column-type-usages-transact-sql.md).  
  
-   Il existe des colonnes calculées, des contraintes CHECK, des vues liées au schéma et des fonctions liées au schéma dont les définitions font référence au type alias ou au type défini par l'utilisateur. Il est possible d’obtenir des informations sur ces références en interrogeant la vue de catalogue [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md).  
  
-   Il existe des fonctions, des procédures stockées ou des déclencheurs créés dans la base de données, et ces routines utilisent des variables et des paramètres de type alias ou de type défini par l'utilisateur. Il est possible d’obtenir des informations sur les paramètres de type alias ou défini par l’utilisateur en interrogeant la vue de catalogue [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md) ou [sys.parameter_type_usages](../../relational-databases/system-catalog-views/sys-parameter-type-usages-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite soit une autorisation CONTROL sur *type_name*, soit une autorisation ALTER sur *schema_name*.  
  
## <a name="examples"></a>Exemples  
 Cet exemple suppose qu'un type nommé `ssn` est déjà créé dans la base de données actuelle.  
  
```sql  
DROP TYPE ssn ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
