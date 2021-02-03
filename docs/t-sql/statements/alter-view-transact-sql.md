---
description: ALTER VIEW (Transact-SQL)
title: ALTER VIEW (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/25/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_VIEW_TSQL
- ALTER VIEW
dev_langs:
- TSQL
helpviewer_keywords:
- indexed views [SQL Server], modifying
- views [SQL Server], modifying
- modifying views
- ALTER VIEW statement
ms.assetid: 03eba220-13e2-49e3-bd9d-ea9df84dc28c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8e01212fb92fc5b589ace24258551b48ae1dbfdb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199477"
---
# <a name="alter-view-transact-sql"></a>ALTER VIEW (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifie une vue précédemment créée. Inclut une vue indexée. ALTER VIEW n'affecte pas les procédures stockées ou déclencheurs dépendants et ne modifie pas les autorisations.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER VIEW [ schema_name . ] view_name [ ( column [ ,...n ] ) ]   
[ WITH <view_attribute> [ ,...n ] ]   
AS select_statement   
[ WITH CHECK OPTION ] [ ; ]  
  
<view_attribute> ::=   
{   
    [ ENCRYPTION ]  
    [ SCHEMABINDING ]  
    [ VIEW_METADATA ]       
}   
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER VIEW [ schema_name . ] view_name [  ( column_name [ ,...n ] ) ]   
AS <select_statement>   
[;]  
``` 
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *schema_name*  
 Nom du schéma auquel appartient la vue.  
  
 *view_name*  
 Vue à modifier.  
  
 *column*  
 Nom(s) d'une ou de plusieurs colonnes séparées par des virgules, devant faire partie de la vue donnée.  
  
> [!IMPORTANT]  
>  Les autorisations d'une colonne sont maintenues uniquement si le nom de la colonne reste le même avant et après l'exécution de l'instruction ALTER VIEW.  
  
> [!NOTE]  
>  Dans les colonnes de la vue, les autorisations pour un nom de colonne s'appliquent d'un bout à l'autre d'une instruction CREATE VIEW ou ALTER VIEW, quelle que soit la source des données sous-jacentes. Par exemple, si des autorisations sont accordées sur la colonne **SalesOrderID** dans une instruction CREATE VIEW, une instruction ALTER VIEW peut renommer la colonne **SalesOrderID**, en **OrderRef** par exemple, et continuer de disposer des autorisations associées à la vue en utilisant **SalesOrderID**.  
  
 ENCRYPTION  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Chiffre les entrées dans des [sys.syscomments](../../relational-databases/system-compatibility-views/sys-syscomments-transact-sql.md) contenant le texte de l’instruction ALTER VIEW. L'utilisation de WITH ENCRYPTION évite la publication de la vue dans le cadre de la réplication SQL Server.  
  
 SCHEMABINDING  
 Lie la vue au schéma des tables sous-jacentes ou des autres tables. Lorsque SCHEMABINDING est spécifié, les tables de base ne peuvent pas être modifiées d'une manière susceptible d'affecter la définition de la vue. La définition de la vue proprement dite doit d'abord être modifiée ou supprimée pour éliminer les dépendances de la table à modifier. Quand vous utilisez l’argument SCHEMABINDING, _select\_statement_ doit comprendre les noms en deux parties (_schema_ **.** _object_) des tables, des vues ou des fonctions définies par l’utilisateur référencées. Tous ces objets référencés doivent se trouver dans la même base de données.  
  
 Les vues ou tables impliquées dans une vue créée avec la clause SCHEMABINDING ne peuvent pas être supprimées, sauf si cette vue perd, à la suite de sa suppression ou de sa modification, la liaison au schéma. Dans le cas contraire, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur. En outre, les instructions ALTER TABLE portant sur des tables impliquées dans des vues liées au schéma échouent si elles affectent la définition des vues.  
  
 VIEW_METADATA  
 Indique que l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne aux interfaces de programmation d'applications (API) DB-Library, ODBC et OLE DB les informations de métadonnées sur la vue, plutôt que sur la ou les tables de base, lorsque des métadonnées en mode lecture sont sollicitées pour une requête qui fait référence à la vue. Les métadonnées en mode de navigation sont des métadonnées supplémentaires que l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] retourne aux API DB-Library, ODBC et OLE DB côté client. Ces métadonnées permettent aux API clientes d'implémenter des curseurs clients pouvant être mis à jour. Les métadonnées en mode Parcourir comprennent des informations sur la table de base à laquelle appartiennent les colonnes du jeu de résultats.  
  
 Dans le cas d'une vue créée par le biais de VIEW_METADATA, les métadonnées en mode Parcourir retournent le nom de la vue, et non celui de la table de base, lors de la description des colonnes de la vue comprise dans le jeu de résultats.  
  
 Quand une vue est créée à l’aide de WITH VIEW_METADATA, toutes ses colonnes, à l’exception de la colonne **timestamp**, peuvent être mises à jour si la vue comporte les déclencheurs INSERT ou UPDATE INSTEAD OF. Pour plus d’informations, consultez la section Notes dans [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 AS  
 Actions que la vue doit entreprendre.  
  
 *select_statement*  
 Instruction SELECT qui définit la vue.  
  
 WITH CHECK OPTION  
 Oblige toutes les instructions de modification de données exécutées sur la vue à respecter les critères définis dans *select_statement*.  
  
## <a name="remarks"></a>Notes  
 Pour plus d’informations sur ALTER VIEW, consultez la section Notes dans [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
> [!NOTE]  
>  Si la définition précédente de la vue à été créée à l'aide des options WITH ENCRYPTION ou CHECK OPTION, ces options doivent figurer dans l'instruction ALTER VIEW pour être activées.  
  
 Si la vue actuellement utilisée est modifiée en utilisant ALTER VIEW, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] pose un verrou de schéma exclusif sur la vue. Lorsque le verrou est attribué (et qu'il n'y a aucun utilisateur actif de la vue), le [!INCLUDE[ssDE](../../includes/ssde-md.md)] supprime toutes les copies de la vue du cache de procédure. Les plans existants qui font référence à la vue restent dans le cache, mais ils sont recompilés lorsqu'ils sont invoqués.  
  
 L"instruction ALTER VIEW peut être appliquée à des vues indexées, mais elle supprime de manière inconditionnelle tous les index de la vue.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter l'instruction ALTER VIEW, il est nécessaire de disposer au minimum de l'autorisation ALTER sur OBJECT.  
  
## <a name="examples"></a>Exemples  
 Cet exemple crée une vue qui contient tous les employés et leurs dates d'embauche, appelée `EmployeeHireDate`. Des autorisations sont accordées à la vue, mais les conditions ayant changé, il est nécessaire de sélectionner des employés dont la date d'embauche tombe avant une certaine date. Ensuite, l'instruction `ALTER VIEW` est utilisée pour remplacer la vue.  
  
```sql 
USE AdventureWorks2012 ;  
GO  
CREATE VIEW HumanResources.EmployeeHireDate  
AS  
SELECT p.FirstName, p.LastName, e.HireDate  
FROM HumanResources.Employee AS e JOIN Person.Person AS  p  
ON e.BusinessEntityID = p.BusinessEntityID ;  
GO  
```  
  
 La vue doit être modifiée pour inclure uniquement les employés qui avaient été embauchés avant l'année `2002`. Si ALTER VIEW n'est pas utilisée mais que la vue est supprimée et recréée, l'instruction GRANT précédemment utilisée et toutes les autres instructions liées aux autorisations appartenant à cette vue doivent être entrées à nouveau.  
  
```sql  
ALTER VIEW HumanResources.EmployeeHireDate  
AS  
SELECT p.FirstName, p.LastName, e.HireDate  
FROM HumanResources.Employee AS e JOIN Person.Person AS p  
ON e.BusinessEntityID = p.BusinessEntityID  
WHERE HireDate < CONVERT(DATETIME,'20020101',101) ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md)   
 [Créer une procédure stockée](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
