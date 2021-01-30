---
description: sp_control_plan_guide (Transact-SQL)
title: sp_control_plan_guide (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_control_plan_guide
- sp_control_plan_guide_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_control_plan_guide
ms.assetid: c96d43d5-6507-4d66-b3f5-f44c0617cb5c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0b4f5d1adbdadf9d8291257708a3098fb50ce175
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185780"
---
# <a name="sp_control_plan_guide-transact-sql"></a>sp_control_plan_guide (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime, active ou désactive un repère de plan.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_control_plan_guide [ @operation = ] N'<control_option>'  
  [ , [ @name = ] N'plan_guide_name' ]  
  
<control_option>::=  
{   
    DROP   
  | DROP ALL  
  | DISABLE  
  | DISABLE ALL  
  | ENABLE   
  | ENABLE ALL  
}  
```  
  
## <a name="arguments"></a>Arguments  
 **N'** _plan_guide_name_ **'**  
 Spécifie le repère de plan à supprimer, activer ou désactiver. *plan_guide_name* est résolue dans la base de données actuelle. S’il n’est pas spécifié, *plan_guide_name* la valeur par défaut est null.  
  
 DROP  
 Supprime le repère de plan spécifié par *plan_guide_name*. Après la suppression d'un repère de plan, les exécutions futures d'une requête auparavant filtrée par le repère de plan ne sont plus influencées par ce repère de plan.  
  
 DROP ALL  
 Supprime tous les repères de plan dans la base de données actuelle. **N'**_plan_guide_name_ ne peut pas être spécifié lorsque l’instruction DROP all est spécifiée.  
  
 DISABLE  
 Désactive le repère de plan spécifié par *plan_guide_name*. Après la désactivation d'un repère de plan, les exécutions futures d'une requête auparavant filtrée par le repère de plan ne sont plus influencées par ce repère de plan.  
  
 DISABLE ALL  
 Désactive tous les repères de plan dans la base de données actuelle. **N'**_plan_guide_name_ ne peut pas être spécifié lorsque Disable All est spécifié.  
  
 ENABLE  
 Active le repère de plan spécifié par *plan_guide_name*. Une fois activé, un repère de plan peut être associé à une requête admissible. Par défaut, les repères de plan sont activés au moment de leur création.  
  
 ENABLE ALL  
 Active tous les repères de plan dans la base de données actuelle. **N'**_plan_guide_name_**'** ne peut pas être spécifié lorsque enable All est spécifié.  
  
## <a name="remarks"></a>Notes  
 Si vous tentez de supprimer ou de modifier une fonction, une procédure stockée ou un déclencheur DML référencé par un repère de plan, qu'il soit activé ou désactivé, une erreur se produit.  
  
 Désactiver un repère de plan désactivé, ou activer un repère de plan activé n'a pas d'effet et ne provoque pas d'erreur.  
  
 Les repères de plan ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md). Toutefois, vous pouvez exécuter **sp_control_plan_guide** avec l’option DROP ou DROP all dans n’importe quelle édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter **sp_control_plan_guide** sur un repère de plan de type Object (créé en spécifiant **@type = '** Object **'** ), vous devez disposer de l’autorisation ALTER sur l’objet référencé par le repère de plan. Tous les autres repères de plan nécessitent l'autorisation ALTER DATABASE.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-enabling-disabling-and-dropping-a-plan-guide"></a>R. Suppression, activation ou désactivation d'un repère de plan  
 L'exemple qui suit crée un repère de plan, le désactive, l'active et le supprime.  
  
```  
--Create a procedure on which to define the plan guide.  
IF OBJECT_ID(N'Sales.GetSalesOrderByCountry', N'P') IS NOT NULL  
    DROP PROCEDURE Sales.GetSalesOrderByCountry;  
GO  
CREATE PROCEDURE Sales.GetSalesOrderByCountry   
    (@Country nvarchar(60))  
AS  
BEGIN  
    SELECT *  
    FROM Sales.SalesOrderHeader AS h   
    INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
    INNER JOIN Sales.SalesTerritory AS t ON c.TerritoryID = t.TerritoryID  
    WHERE t.CountryRegionCode = @Country;  
END  
GO  
--Create the plan guide.  
EXEC sp_create_plan_guide N'Guide3',  
    N'SELECT *  
    FROM Sales.SalesOrderHeader AS h   
    INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
    INNER JOIN Sales.SalesTerritory AS t ON c.TerritoryID = t.TerritoryID  
    WHERE t.CountryRegionCode = @Country',  
    N'OBJECT',  
    N'Sales.GetSalesOrderByCountry',  
    NULL,  
    N'OPTION (OPTIMIZE FOR (@Country = N''US''))';  
GO  
--Disable the plan guide.  
EXEC sp_control_plan_guide N'DISABLE', N'Guide3';  
GO  
--Enable the plan guide.  
EXEC sp_control_plan_guide N'ENABLE', N'Guide3';  
GO  
--Drop the plan guide.  
EXEC sp_control_plan_guide N'DROP', N'Guide3';  
```  
  
### <a name="b-disabling-all-plan-guides-in-the-current-database"></a>B. Désactivation de tous les repères de plan dans la base de données actuelle  
 L'exemple qui suit désactive tous les repères de plan dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_control_plan_guide N'DISABLE ALL';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sys.plan_guides &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)   
 [Repères de plan](../../relational-databases/performance/plan-guides.md)  
  
