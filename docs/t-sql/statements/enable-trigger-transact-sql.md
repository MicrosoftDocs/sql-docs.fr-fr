---
description: ENABLE TRIGGER (Transact-SQL)
title: ENABLE TRIGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ENABLE TRIGGER
- ENABLE_TSQL
- ENABLE_TRIGGER_TSQL
- ENABLE
dev_langs:
- TSQL
helpviewer_keywords:
- DDL triggers, enabling
- triggers [SQL Server], enabling
- DML triggers, enabling
- ENABLE TRIGGER statement
ms.assetid: 6e21f0ad-68d0-432f-9c7c-a119dd2d3fc9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3ed2081a912291eed4682ba0e853df66f36e1801
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091927"
---
# <a name="enable-trigger-transact-sql"></a>ENABLE TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Active un déclencheur DML, DDL ou de connexion.  
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ENABLE TRIGGER { [ schema_name . ] trigger_name [ ,...n ] | ALL }  
ON { object_name | DATABASE | ALL SERVER } [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*schema_name*  
Nom du schéma auquel appartient le déclencheur. Vous ne pouvez pas spécifier *schema_name* pour des déclencheurs DDL ou de connexion.  
  
*trigger_name*  
Nom du déclencheur à activer.  
  
ALL  
Indique que tous les déclencheurs définis dans l'étendue de la clause ON sont activés.  
  
*object_name*  
Nom de la table ou de la vue sur laquelle le déclencheur DML *trigger_name* a été créé pour s’exécuter.  
  
DATABASE  
Pour un déclencheur DDL, indique que *trigger_name* a été créé ou modifié pour s’exécuter sur l’étendue de la base de données.  
  
ALL SERVER  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
Pour un déclencheur DDL, indique que *trigger_name* a été créé ou modifié pour s’exécuter sur l’étendue du serveur. ALL SERVER s'applique également aux déclencheurs de connexion.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
## <a name="remarks"></a>Notes  
Un déclencheur n'est pas recréé par son activation. Un déclencheur désactivé continue d'exister en tant qu'objet de la base de données active, mais il ne s'exécute pas. Une fois activé, un déclencheur s'exécute dès lors qu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] dans laquelle il était initialement programmé est exécuté. Les déclencheurs sont désactivés à l’aide de [DISABLE TRIGGER](../../t-sql/statements/disable-trigger-transact-sql.md). Il est également possible d’activer ou de désactiver les déclencheurs DML définis sur des tables au moyen de la commande [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
Pour activer un déclencheur DML, un utilisateur doit disposer au minimum d'une autorisation ALTER sur la table ou sur la vue sur laquelle le déclencheur a été créé.  
  
L'activation d'un déclencheur DDL avec une étendue de serveur (ON ALL SERVER) ou d'un déclencheur de connexion défini nécessite l'autorisation CONTROL SERVER sur le serveur. Pour activer un déclencheur DDL dans l'étendue de la base de données (ON DATABASE), un utilisateur doit au minimum disposer d'une autorisation ALTER ANY DATABASE DDL TRIGGER dans la base de données active.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-enabling-a-dml-trigger-on-a-table"></a>R. Activation d'un déclencheur DML sur une table  
Dans l’exemple suivant, le déclencheur `uAddress`, qui a été créé sur la table `Address` dans la base de données AdventureWorks, est désactivé puis activé.  
  
```sql  
DISABLE TRIGGER Person.uAddress ON Person.Address;  
GO  
ENABLE Trigger Person.uAddress ON Person.Address;  
GO  
```  
  
### <a name="b-enabling-a-ddl-trigger"></a>B. Activation d'un déclencheur DDL  
L’exemple suivant crée un déclencheur DDL `safety` sur l’étendue de la base de données, puis le désactive et l’active.  
  
```sql  
CREATE TRIGGER safety   
ON DATABASE   
FOR DROP_TABLE, ALTER_TABLE   
AS   
   PRINT 'You must disable Trigger "safety" to drop or alter tables!'   
   ROLLBACK;  
GO  
DISABLE TRIGGER safety ON DATABASE;  
GO  
ENABLE TRIGGER safety ON DATABASE;  
GO  
```  
  
### <a name="c-enabling-all-triggers-that-were-defined-with-the-same-scope"></a>C. Activation de tous les déclencheurs définis dans la même étendue  
L'exemple suivant montre l'activation de tous les déclencheurs DDL créés dans l'étendue du serveur.  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
```sql  
ENABLE Trigger ALL ON ALL SERVER;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)  
  
  
