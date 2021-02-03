---
description: Supprimer des fonctions définies par l'utilisateur
title: Supprimer des fonctions définies par l’utilisateur | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: db1d668a-23b7-4757-a9c5-1bd848ba7f6d
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9b532481a55f23116d0fd90760d1a940f4cd82f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184296"
---
# <a name="delete-user-defined-functions"></a>Supprimer des fonctions définies par l'utilisateur
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Vous pouvez supprimer des fonctions définies par l’utilisateur dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour supprimer une fonction définie par l'utilisateur, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Vous ne pourrez pas supprimer la fonction si la base de données contient des fonctions ou des vues Transact-SQL qui font référence à cette fonction et qui ont été créées au moyen de SCHEMABINDING. Elle échoue également s'il existe des colonnes calculées, des contraintes CHECK ou des contraintes DEFAULT qui font référence à cette fonction.  
  
-   Vous ne pourrez pas supprimer la fonction si des colonnes calculées qui ont été indexées font référence à cette fonction.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Exige l'autorisation ALTER sur le schéma auquel la fonction appartient, ou l'autorisation CONTROL sur la fonction.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-delete-a-user-defined-function"></a>Pour supprimer une fonction définie par l'utilisateur  
  
1.  Cliquez sur le signe plus (+° en regard de la base de données qui contient la fonction à modifier.  
  
2.  Cliquez sur le signe plus en regard du dossier **Programmabilité** .  
  
3.  Cliquez sur le signe plus en regard du dossier qui contient la fonction à modifier :  
  
    -   Fonction table  
  
    -   Fonction scalaire  
  
    -   Fonction d'agrégation  
  
4.  Cliquez avec le bouton droit sur la fonction à supprimer, puis sélectionnez **Supprimer**.  
  
5.  Dans la boîte de dialogue **Supprimer l'objet** , cliquez sur **OK**.  

    > [!IMPORTANT]  
    >  Cliquez sur **Afficher les dépendances** dans la boîte de dialogue **Supprimer un objet** pour ouvrir la boîte de dialogue _Dépendances de \_nom_**fonction**. Cette opération affiche tous les objets qui dépendent de la fonction et tous les objets dont la fonction dépend.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-delete-a-user-defined-function"></a>Pour supprimer une fonction définie par l'utilisateur  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- creates function called "Sales.ufn_SalesByStore"  
    USE AdventureWorks2012;  
    GO  
    CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)  
    RETURNS TABLE  
    AS  
    RETURN   
    (  
        SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'  
        FROM Production.Product AS P   
        JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID  
        JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID  
        JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID  
        WHERE C.StoreID = @storeid  
        GROUP BY P.ProductID, P.Name  
    );  
    GO  
    ```  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- determines if function exists in database  
    IF OBJECT_ID (N'Sales.fn_SalesByStore', N'IF') IS NOT NULL  
    -- deletes function  
        DROP FUNCTION Sales.fn_SalesByStore;  
    GO  
    ```  
  
 Pour plus d’informations, consultez [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md).  
  
  
