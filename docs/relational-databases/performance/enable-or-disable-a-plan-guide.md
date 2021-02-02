---
title: Activer ou désactiver un repère de plan | Microsoft Docs
description: Apprenez à désactiver et activer des repères de plan à l’aide de SQL Server Management Studio ou de Transact-SQL. Désactivez ou activez un ou tous les repères de plan dans une base de données.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- plan guides [SQL Server], disabling
- enabling plan guides
- plan guides [SQL Server], enabling
- disabling plan guides
ms.assetid: b00ab550-5308-4cb8-8330-483cd1d25654
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e6fd86e44411fafef9e70e8981d0fa8728af9e76
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048941"
---
# <a name="enable-or-disable-a-plan-guide"></a>Activer ou désactiver un repère de plan
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Vous pouvez désactiver et activer les repères de plan dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Il est possible d'activer ou de désactiver un seul repère de plan ou tous les repères de plan d'une base de données.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour désactiver et activer des repères de plan, à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Si vous tentez de supprimer ou de modifier une fonction, une procédure stockée ou un déclencheur DML référencé par un repère de plan, qu'il soit activé ou désactivé, une erreur se produit. Vérifiez toujours les dépendances avant de supprimer ou de modifier l'un des objets répertoriés ci-dessus.  
  
-   Désactiver un repère de plan désactivé, ou activer un repère de plan activé n'a pas d'effet et ne provoque pas d'erreur.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 La désactivation ou l'activation d'un repère de plan OBJECT nécessite l'autorisation ALTER sur l'objet (par exemple : fonction, procédure stockée) qui est référencé par le repère de plan. Tous les autres repères de plan nécessitent l'autorisation ALTER DATABASE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-disable-or-enable-a-plan-guide"></a>Pour désactiver ou activer un repère de plan  
  
1.  Cliquez sur le signe plus pour développer la base de données dans laquelle vous souhaitez désactiver ou activer un repère de plan, puis cliquez sur le signe plus pour développer le dossier **Programmabilité** .  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Repères de plan** .  
  
3.  Cliquez avec le bouton droit sur le repère de plan que vous souhaitez désactiver ou activer et sélectionnez **Désactiver** ou **Activer**.  
  
4.  Dans la boîte de dialogue **Désactiver le repère de plan** ou **Activer le repère de plan** , vérifiez que l'action choisie a abouti, puis cliquez sur **Fermer**.  

#### <a name="to-disable-or-enable-all-plan-guides-in-a-database"></a>Pour activer ou désactiver tous les repères de plan dans une base de données  
  
1.  Cliquez sur le signe plus pour développer la base de données dans laquelle vous souhaitez désactiver ou activer un repère de plan, puis cliquez sur le signe plus pour développer le dossier **Programmabilité** .  
  
2.  Cliquez avec le bouton droit sur le dossier **Repères de plan** , puis sélectionnez **Tout activer** ou **Tout désactiver**.  
  
3.  Dans la boîte de dialogue **Désactiver tous les repères de plan** ou **Activer tous les repères de plan** , vérifiez que l'action choisie a abouti, puis cliquez sur **Fermer**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-disable-or-enable-a-plan-guide"></a>Pour désactiver ou activer un repère de plan  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
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
    --Disable the plan guide.  
    EXEC sp_control_plan_guide N'DISABLE', N'Guide3';  
    GO  
    --Enable the plan guide.  
    EXEC sp_control_plan_guide N'ENABLE', N'Guide3';  
    GO  
  
    ```  
  
#### <a name="to-disable-or-enable-all-plan-guides-in-a-database"></a>Pour activer ou désactiver tous les repères de plan dans une base de données  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    --Disable all plan guides in the database.  
    EXEC sp_control_plan_guide N'DISABLE ALL';  
    GO  
    --Enable all plan guides in the database.  
    EXEC sp_control_plan_guide N'ENABLE ALL';  
    GO  
  
    ```  
  
 Pour plus d’informations, consultez [sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md).  
  
  
