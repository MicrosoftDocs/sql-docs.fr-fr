---
description: DISABLE TRIGGER (Transact-SQL)
title: DISABLE TRIGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DISABLE_TSQL
- DISABLE
- DISABLE TRIGGER
- DISABLE_TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DML triggers, disabling
- DDL triggers, disabling
- DISABLE TRIGGER statement
- triggers [SQL Server], disabling
- disabling triggers
ms.assetid: e6529f06-e442-437e-a7bf-41790bc092c5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 01302c2d9acdf1ce89246be18fd54c3cae72ebff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159352"
---
# <a name="disable-trigger-transact-sql"></a>DISABLE TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Désactive un déclencheur.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DISABLE TRIGGER { [ schema_name . ] trigger_name [ ,...n ] | ALL }  
ON { object_name | DATABASE | ALL SERVER } [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *schema_name*  
 Nom du schéma auquel appartient le déclencheur. Vous ne pouvez pas spécifier *schema_name* pour des déclencheurs DDL ou de connexion.  
  
 *trigger_name*  
 Nom du déclencheur à désactiver.  
  
 ALL  
 Indique que tous les déclencheurs définis sur l'étendue de la clause ON sont désactivés.  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée des déclencheurs dans des bases de données qui sont publiées pour la réplication de fusion. Le fait de spécifier ALL dans les bases de données publiées désactive ces déclencheurs, ce qui interrompt la réplication. Vérifiez que la base de données active n'est pas publiée pour la réplication de fusion avant de spécifier ALL.  
  
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
 Les déclencheurs sont activés par défaut lors de leur création. La désactivation d'un déclencheur ne le supprime pas. Le déclencheur existe toujours en tant qu'objet dans la base de données actuelle. Cependant, il ne se déclenche pas lorsque des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sur lesquelles il a été programmé sont exécutées. Il est possible de réactiver des déclencheurs à l’aide de [ENABLE TRIGGER](../../t-sql/statements/enable-trigger-transact-sql.md). Il est également possible d’activer ou de désactiver les déclencheurs DML définis sur des tables au moyen de la commande [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 Le fait de modifier le déclencheur à l’aide de l’instruction **ALTER TRIGGER** active le déclencheur.  
  
## <a name="permissions"></a>Autorisations  
 Pour désactiver un déclencheur DML, un utilisateur doit avoir au minimum l'autorisation ALTER pour la table ou la vue sur laquelle le déclencheur a été créé.  
  
 La désactivation d'un déclencheur DDL avec une étendue de serveur (ON ALL SERVER) ou d'un déclencheur de connexion nécessite l'autorisation CONTROL SERVER sur le serveur. Pour désactiver un déclencheur DDL sur l'étendue d'une base de données (ON DATABASE), un utilisateur doit avoir au minimum l'autorisation ALTER ANY DATABASE DDL TRIGGER pour la base de données active.  
  
## <a name="examples"></a>Exemples  
Les exemples suivants sont décrits dans la base de données AdventureWorks2012.
  
### <a name="a-disabling-a-dml-trigger-on-a-table"></a>R. Désactivation d'un déclencheur DML sur une table  
 L'exemple suivant désactive le déclencheur `uAddress` créé sur la table `Person`.  
  
```sql  
DISABLE TRIGGER Person.uAddress ON Person.Address;  
GO  
```  
  
### <a name="b-disabling-a-ddl-trigger"></a>B. Désactivation d'un déclencheur DDL  
 L'exemple suivant crée un déclencheur DDL `safety` sur l'étendue de la base de données, puis le désactive.  
  
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
```  
  
### <a name="c-disabling-all-triggers-that-were-defined-with-the-same-scope"></a>C. Désactivation de tous les déclencheurs définis sur la même étendue  
 L'exemple suivant montre la désactivation de tous les déclencheurs DDL créés dans l'étendue du serveur.  
  
```sql  
DISABLE Trigger ALL ON ALL SERVER;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)  
  
  
