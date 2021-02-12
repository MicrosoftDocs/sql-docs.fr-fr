---
title: création et exécution d’un test unitaire SQL Server
description: Découvrez comment créer un test unitaire SQL Server. Suivez les étapes de la configuration d’un test qui détecte une erreur dans une procédure stockée.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 992c1d8e-3729-438b-9ef4-cd103e28f145
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6b0eaf98f4d10c08548e898c261a489ad71f0ff4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016729"
---
# <a name="walkthrough-creating-and-running-a-sql-server-unit-test"></a>Procédure pas à pas : création et exécution d’un test unitaire SQL Server

Dans cette procédure pas à pas, vous créez un test unitaire SQL Server qui vérifie le comportement de plusieurs procédures stockées. Vous créez des tests unitaires SQL Server pour identifier les erreurs de code qui peuvent provoquer un comportement d'application incorrect. Vous pouvez exécuter des tests unitaires SQL Server et des tests d'application dans le cadre d'une suite automatisée de tests.  
  
Au cours de cette procédure pas à pas, vous effectuez les tâches suivantes :  
  
-   [Créer un script qui contient un schéma de base de données](#CreateScript)  
  
-   [Créer un projet de base de données et importer ce schéma](#CreateProjectAndImport)  
  
-   [Déployer le projet de base de données dans un environnement de développement isolé](#DeployDBProj)  
  
-   [Créer des tests unitaires SQL Server](#CreateDBUnitTests)  
  
-   [Définir la logique de test](#DefineTestLogic)  
  
-   [Exécuter des tests unitaires SQL Server](#RunTests)  
  
-   [Ajouter un test unitaire négatif](#NegativeTest)  
  
Lorsqu'un des tests unitaires détecte une erreur dans une procédure stockée, vous corrigez cette erreur et réexécutez le test.  
  
## <a name="prerequisites"></a>Prérequis  
Pour effectuer cette procédure pas à pas, vous devez être en mesure de vous connecter à un serveur de base de données (ou à une base de données LocalDB) sur lequel vous êtes autorisé à créer et déployer une base de données. Pour plus d'informations, consultez [Autorisations requises pour les fonctionnalités de base de données de Visual Studio](/previous-versions/visualstudio/visual-studio-2010/aa833413(v=vs.100)).  
  
## <a name="create-a-script-that-contains-a-database-schema"></a><a name="CreateScript"></a>Créer un script qui contient un schéma de base de données  
  
#### <a name="to-create-a-script-from-which-you-can-import-a-schema"></a>Pour créer un script à partir duquel vous pouvez importer un schéma  
  
1.  Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Fichier**.  
  
    La boîte de dialogue **Nouveau fichier** s'affiche.  
  
2.  Dans la liste **Catégories** , cliquez sur **Général** si cette option n'est pas déjà mise en surbrillance.  
  
3.  Dans la liste **Modèles** , cliquez sur **Fichier SQL**, puis cliquez sur **Ouvrir**.  
  
    L’éditeur Transact\-SQL s’ouvre.  
  
4.  Copiez le code Transact\-SQL suivant et collez-le dans l’éditeur Transact\-SQL.  
  
    ```  
    PRINT N'Creating Sales...';  
    GO  
    CREATE SCHEMA [Sales]  
        AUTHORIZATION [dbo];  
    GO  
    PRINT N'Creating Sales.Customer...';  
    GO  
    CREATE TABLE [Sales].[Customer] (  
        [CustomerID]   INT           IDENTITY (1, 1) NOT NULL,  
        [CustomerName] NVARCHAR (40) NOT NULL,  
        [YTDOrders]    INT           NOT NULL,  
        [YTDSales]     INT           NOT NULL  
    );  
    GO  
    PRINT N'Creating Sales.Orders...';  
    GO  
    CREATE TABLE [Sales].[Orders] (  
        [CustomerID] INT      NOT NULL,  
        [OrderID]    INT      IDENTITY (1, 1) NOT NULL,  
        [OrderDate]  DATETIME NOT NULL,  
        [FilledDate] DATETIME NULL,  
        [Status]     CHAR (1) NOT NULL,  
        [Amount]     INT      NOT NULL  
    );  
    GO  
    PRINT N'Creating Sales.Def_Customer_YTDOrders...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [Def_Customer_YTDOrders] DEFAULT 0 FOR [YTDOrders];  
    GO  
    PRINT N'Creating Sales.Def_Customer_YTDSales...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [Def_Customer_YTDSales] DEFAULT 0 FOR [YTDSales];  
    GO  
    PRINT N'Creating Sales.Def_Orders_OrderDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [Def_Orders_OrderDate] DEFAULT GetDate() FOR [OrderDate];  
    GO  
    PRINT N'Creating Sales.Def_Orders_Status...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [Def_Orders_Status] DEFAULT 'O' FOR [Status];  
    GO  
    PRINT N'Creating Sales.PK_Customer_CustID...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [PK_Customer_CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) WITH (ALLOW_PAGE_LOCKS = ON, ALLOW_ROW_LOCKS = ON, PAD_INDEX = OFF, IGNORE_DUP_KEY = OFF, STATISTICS_NORECOMPUTE = OFF);  
    GO  
    PRINT N'Creating Sales.PK_Orders_OrderID...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [PK_Orders_OrderID] PRIMARY KEY CLUSTERED ([OrderID] ASC) WITH (ALLOW_PAGE_LOCKS = ON, ALLOW_ROW_LOCKS = ON, PAD_INDEX = OFF, IGNORE_DUP_KEY = OFF, STATISTICS_NORECOMPUTE = OFF);  
    GO  
    PRINT N'Creating Sales.FK_Orders_Customer_CustID...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [FK_Orders_Customer_CustID] FOREIGN KEY ([CustomerID]) REFERENCES [Sales].[Customer] ([CustomerID]) ON DELETE NO ACTION ON UPDATE NO ACTION;  
    GO  
    PRINT N'Creating Sales.CK_Orders_FilledDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [CK_Orders_FilledDate] CHECK ((FilledDate >= OrderDate) AND (FilledDate < '01/01/2030'));  
    GO  
    PRINT N'Creating Sales.CK_Orders_OrderDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [CK_Orders_OrderDate] CHECK ((OrderDate > '01/01/2005') and (OrderDate < '01/01/2030'));  
    GO  
    PRINT N'Creating Sales.uspCancelOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspCancelOrder]  
    @OrderID INT  
    AS  
    BEGIN  
    DECLARE @Delta INT, @CustomerID INT  
    BEGIN TRANSACTION  
        SELECT @Delta = [Amount], @CustomerID = [CustomerID]  
         FROM [Sales].[Orders] WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Orders]  
       SET [Status] = 'X'  
    WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Customer]  
       SET  
       YTDOrders = YTDOrders - @Delta  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    END  
    GO  
    PRINT N'Creating Sales.uspFillOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspFillOrder]  
    @OrderID INT, @FilledDate DATETIME  
    AS  
    BEGIN  
    DECLARE @Delta INT, @CustomerID INT  
    BEGIN TRANSACTION  
        SELECT @Delta = [Amount], @CustomerID = [CustomerID]  
         FROM [Sales].[Orders] WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Orders]  
       SET [Status] = 'F',  
           [FilledDate] = @FilledDate  
    WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales - @Delta  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    END  
    GO  
    PRINT N'Creating Sales.uspNewCustomer...';  
    GO  
    CREATE PROCEDURE [Sales].[uspNewCustomer]  
    @CustomerName NVARCHAR (40)  
    AS  
    BEGIN  
    INSERT INTO [Sales].[Customer] (CustomerName) VALUES (@CustomerName);  
    RETURN SCOPE_IDENTITY()  
    END  
    GO  
    PRINT N'Creating Sales.uspPlaceNewOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspPlaceNewOrder]  
    @CustomerID INT, @Amount INT, @OrderDate DATETIME, @Status CHAR (1)='O'  
    AS  
    BEGIN  
    DECLARE @RC INT  
    BEGIN TRANSACTION  
    INSERT INTO [Sales].[Orders] (CustomerID, OrderDate, FilledDate, Status, Amount)   
         VALUES (@CustomerID, @OrderDate, NULL, @Status, @Amount)  
    SELECT @RC = SCOPE_IDENTITY();  
    UPDATE [Sales].[Customer]  
       SET  
       YTDOrders = YTDOrders + @Amount  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    RETURN @RC  
    END  
    GO  
    CREATE PROCEDURE [Sales].[uspShowOrderDetails]  
    @CustomerID INT=0  
    AS  
    BEGIN  
    SELECT [C].[CustomerName], CONVERT(date, [O].[OrderDate]), CONVERT(date, [O].[FilledDate]), [O].[Status], [O].[Amount]  
      FROM [Sales].[Customer] AS C  
      INNER JOIN [Sales].[Orders] AS O  
         ON [O].[CustomerID] = [C].[CustomerID]  
      WHERE [C].[CustomerID] = @CustomerID  
    END  
    GO  
    ```  
  
5.  Enregistrez le fichier . Notez l'emplacement, car vous devez utiliser ce script dans la procédure suivante.  
  
6.  Dans le menu **Fichier** , cliquez sur **Fermer la solution**.  
  
    Ensuite, vous créez un projet de base de données et importez le schéma du script créé.  
  
## <a name="create-a-database-project-and-import-a-schema"></a><a name="CreateProjectAndImport"></a>Créer un projet de base de données et importer un schéma  
  
#### <a name="to-create-a-database-project"></a>Pour créer un projet de base de données  
  
1.  Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.  
  
    La boîte de dialogue **Nouveau projet** apparaît.  
  
2.  Sous **Modèles installés**, sélectionnez le nœud **SQL Server**, puis sélectionnez **Projet de base de données SQL Server**.  
  
3.  Dans la zone **Nom**, tapez **SimpleUnitTestDB**.  
  
4.  Activez la case à cocher **Créer le répertoire pour la solution** si ce n'est pas déjà fait.  
  
5.  Désactivez la case à cocher **Ajouter au contrôle de code source** si ce n'est pas déjà fait, puis cliquez sur **OK**.  
  
    Le projet de base de données est créé et s'affiche dans l' **Explorateur de solutions**. Ensuite, vous importez le schéma de la base de données d'un script.  
  
#### <a name="to-import-a-database-schema-from-a-script"></a>Pour importer un schéma de la base de données d'un script  
  
1.  Dans le menu **Projet**, cliquez sur **Importer**, puis sur **Script (\*.sql)** .  
  
2.  Cliquez sur **Suivant** après avoir lu la page d'accueil.  
  
3.  Cliquez sur **Parcourir** et accédez au répertoire dans lequel vous avez enregistré le fichier .sql.  
  
4.  Double-cliquez sur le fichier .sql, puis cliquez sur **Terminer**.  
  
    Le script est importé, et les objets définis dans ce script sont ajoutés à votre projet de base de données.  
  
5.  Lisez le résumé, puis cliquez sur **Terminer** pour terminer l'opération.  
  
    > [!NOTE]  
    > La procédure Sales.uspFillOrder contient une erreur de codage intentionnelle que vous allez découvrir et corriger ultérieurement dans cette procédure.  
  
#### <a name="to-examine-the-resulting-project"></a>Pour examiner le projet résultant  
  
1.  Dans l' **Explorateur de solutions**, examinez les fichiers de script importés dans le projet.  
  
2.  Dans l'**Explorateur d'objets SQL Server**, examinez la base de données dans le nœud Projets.  
  
## <a name="deploying-to-localdb"></a><a name="DeployDBProj"></a>Déploiement dans LocalDB  
Par défaut, lorsque vous appuyez sur F5, vous déployez (ou publiez) la base de données dans une base de données LocalDB. Modifiez l'emplacement de la base de données en accédant à l'onglet Débogage de la page de propriétés du projet et en modifiant la chaîne de connexion.  
  
## <a name="create-sql-server-unit-tests"></a><a name="CreateDBUnitTests"></a>Créer des tests unitaires SQL Server  
  
#### <a name="to-create-a-sql-server-unit-test-for-the-stored-procedures"></a>Pour créer un test unitaire SQL Server pour les procédures stockées  
  
1.  Dans l'**Explorateur d'objets SQL Server** , développez le nœud de projets de **SimpleUnitTestDB** et développez le nœud **Programmabilité**, puis le nœud **Procédures stockées**.  
  
2.  Cliquez avec le bouton droit sur les procédures stockées, puis cliquez sur **Créer des tests unitaires** pour afficher la boîte de dialogue **Créer des tests unitaires**.  
  
3.  Cochez les cases des cinq procédures stockées : **Sales.uspCancelOrder**, **Sales.uspFillOrder**, **Sales.uspNewCustomer**, **Sales.uspPlaceNewOrder** et **Sales.uspShowOrderDetails**.  
  
4.  Dans la liste déroulante **Projet**, sélectionnez **Créer un nouveau projet de test Visual C#** .  
  
5.  Acceptez les noms par défaut de nom de projet et nom de classe, puis cliquez sur **OK**.  
  
6.  Dans la boîte de dialogue de configuration de test, sous **Exécuter les tests unitaires en utilisant la connexion de données suivante**, spécifiez une connexion à la base de données que vous avez déployée précédemment dans cette procédure pas à pas. Par exemple, si vous avez utilisé l'emplacement de déploiement par défaut, (LocalDB), cliquez sur **Nouvelle connexion** et spécifiez **(LocalDB)\Projects**. Ensuite, sélectionnez le nom la base de données. Ensuite, cliquez sur OK pour fermer la boîte de dialogue **Propriétés de connexion** .  
  
    > [!NOTE]  
    > Si vous devez tester des vues ou des procédures stockées qui ont des autorisations limitées, vous indiquez généralement la connexion lors de cette étape. Vous spécifiez ensuite la connexion secondaire, avec des autorisations plus générales, pour valider le test. Si vous disposez d'une connexion secondaire, vous devez ajouter l'utilisateur au projet de base de données et créer une connexion pour l'utilisateur dans le script de prédéploiement.  
  
7.  Dans la section **Déploiement** de la boîte de dialogue de configuration de test, activez la case à cocher **Déployer automatiquement le projet de base de données avant l'exécution des tests unitaires** .  
  
8.  Dans **Projet de base de données**, cliquez sur **SimpleUnitTestDB.sqlproj**.  
  
9. Dans **Configuration de déploiement**, cliquez sur **Débogage**.  
  
    Vous pouvez également générer des données de test dans le cadre de vos tests unitaires SQL Server. Pour cette procédure pas à pas, vous allez ignorer cette étape, car les tests créent leurs propres données.  
  
10. Cliquez sur **OK**.  
  
    Le projet de test est créé et le Concepteur de test unitaire SQL Server apparaît. Ensuite, vous allez mettre à jour la logique de test dans le script Transact\-SQL des tests unitaires.  
  
## <a name="define-test-logic"></a><a name="DefineTestLogic"></a>Définir la logique de test  
Cette base de données très simple possède deux tables, Customer et Order. Vous mettez à jour la base de données à l'aide de procédures stockées suivantes :  
  
-   uspNewCustomer - Cette procédure stockée ajoute un enregistrement à la table Customer, qui définit les colonnes YTDOrders et YTDSales du client à zéro.  
  
-   uspPlaceNewOrder - Cette procédure stockée ajoute un enregistrement dans la table Orders pour le client spécifié et met à jour la valeur YTDOrders dans l'enregistrement correspondant de la table Customer.  
  
-   uspFillOrder - Cette procédure stockée met à jour un enregistrement dans la table Orders en remplaçant l'état « O » par « F » et incrémente la quantité YTDSales dans l'enregistrement correspondant de la table Customer.  
  
-   uspCancelOrder - Cette procédure stockée met à jour un enregistrement dans la table Orders en remplaçant l'état « O » par « X » et décrémente la quantité YTDOrders dans l'enregistrement correspondant de la table Customer.  
  
-   uspShowOrderDetails - Cette procédure stockée joint la table Orders à la table Customer et indique les enregistrements pour un client spécifique.  
  
> [!NOTE]  
> Cet exemple montre comment créer un test unitaire SQL Server simple. Dans une base de données réelle, additionnez le montant total de toutes les commandes avec un état « O » ou « F » pour un client particulier. Cette procédure pas à pas n'utilise pas la gestion des erreurs. Par exemple, elle ne vous empêche pas d'appeler uspFillOrder pour une commande qui a déjà été passée.  
  
Les tests supposent que la base de données démarre dans un état propre. Vous allez créer des tests qui vérifient les conditions suivantes :  
  
-   uspNewCustomer - Vérifiez que la table Customer contient une ligne après avoir exécuté la procédure stockée.  
  
-   uspPlaceNewOrder - Pour le client ayant le CustomerID 1, passez une commande d'un montant de $100. Vérifiez que le montant de YTDOrders pour ce client est 100 et que la quantité de YTDSales est zéro.  
  
-   uspFillOrder - Pour le client ayant le CustomerID 1, passez une commande d'un montant de $50. Passez cette commande. Vérifiez que les montants de YTDOrders et YTDSales sont 50.  
  
-   uspShowOrderDetails - Pour le client ayant le CustomerID 1, passez des commandes $100, $50 et $5. Vérifiez que uspShowOrderDetails retourne le nombre approprié de colonnes et que l'ensemble de résultats correspond à la somme de contrôle attendue.  
  
> [!NOTE]  
> Pour un ensemble complet de tests unitaires SQL Server, vous vérifiez généralement que les autres colonnes sont définies correctement. Afin de conserver cette procédure pas à pas dans une taille gérable, elle n'explique pas comment vérifier le comportement de uspCancelOrder.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspnewcustomer"></a>Pour écrire le test unitaire SQL Server pour uspNewCustomer  
  
1.  Dans la barre de navigation du Concepteur de test unitaire SQL Server, cliquez sur **Sales_uspNewCustomerTest** et vérifiez que **Test** est mis en surbrillance dans la liste adjacente.  
  
    Après avoir effectué l'étape précédente, créez le script de test pour l'action de test dans le test unitaire.  
  
2.  Mettez à jour les instructions Transact \-SQL dans l'éditeur Transact\-SQL de façon à ce qu'elles correspondent aux instructions suivantes :  
  
    ```  
    -- ssNoVersion unit test for Sales.uspNewCustomer  
    DECLARE @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @CustomerName = 'Fictitious Customer';  
  
    EXECUTE @RC = [Sales].[uspNewCustomer] @CustomerName;  
  
    SELECT * FROM [Sales].[Customer];  
    ```  
  
3.  Dans le volet **Conditions de test**, cliquez sur une condition de test peu probante, puis cliquez sur l'icône **Supprimer la condition de test** (X rouge).  
  
4.  Dans le volet **Conditions de test**, cliquez sur **Nombre de lignes** dans la liste, puis cliquez sur l'icône **Ajouter une condition de test** (+ vert).  
  
5.  Ouvrez la fenêtre **Propriétés** (sélectionnez la condition de test, puis appuyez sur F4), et définissez la propriété **Nombre de lignes** sur la valeur 1.  
  
6.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
    Ensuite, définissez la logique de test unitaire pour uspPlaceNewOrder.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspplaceneworder"></a>Pour écrire le test unitaire SQL Server pour uspPlaceNewOrder  
  
1.  Dans la barre de navigation du Concepteur de test unitaire SQL Server, cliquez sur **Sales_uspPlaceNewOrderTest** et vérifiez que **Test** est mis en surbrillance dans la liste adjacente.  
  
    Après avoir effectué cette étape, créez le script de test pour l'action de test dans le test unitaire.  
  
2.  Mettez à jour les instructions Transact \-SQL dans l'éditeur Transact\-SQL de façon à ce qu'elles correspondent aux instructions suivantes :  
  
    ```  
    -- ssNoVersion unit test for Sales.uspPlaceNewOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- place an order for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    -- verify that the YTDOrders value is correct.  
    SELECT @RC = [YTDOrders] FROM [Sales].[Customer] WHERE [CustomerID] = @CustomerID  
  
    SELECT @RC AS RC  
    ```  
  
3.  Dans le volet **Conditions de test** , cliquez sur une condition de test peu probante, puis cliquez sur **Supprimer la condition de test**.  
  
4.  Dans le volet **Conditions de test** , cliquez sur **Valeur scalaire** dans la liste, puis cliquez sur **Ajouter une condition de test**.  
  
5.  Dans la fenêtre **Propriétés** , affectez à la propriété **Valeur attendue** la valeur 100.  
  
6.  Dans la barre de navigation du Concepteur de test unitaire SQL Server, cliquez sur **Sales_uspPlaceNewOrderTest** et vérifiez que **prétest** est mis en surbrillance dans la liste adjacente.  
  
    Après avoir effectué cette étape, spécifiez les instructions qui placent les données dans l'état requis pour exécuter votre test. Pour cet exemple, vous devez créer l'enregistrement de client avant de pouvoir passer une commande.  
  
7.  Cliquez sur **Cliquez ici pour créer** pour créer un script de prétest.  
  
8.  Mettez à jour les instructions Transact \-SQL dans l'éditeur Transact\-SQL de façon à ce qu'elles correspondent aux instructions suivantes :  
  
    ```  
    /*  
    Add Transact-SQL statements here that you want to run before  
    the test script is run.  
    */  
    -- Add a customer for this test with the name 'Fictitious Customer'  
    DECLARE @NewCustomerID AS INT, @CustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
       @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
    ```  
  
9. Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
    Ensuite, créez le test unitaire pour uspFillOrder.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspfillorder"></a>Pour écrire le test unitaire SQL Server pour uspFillOrder  
  
1.  Dans la barre de navigation du Concepteur de test unitaire SQL Server, cliquez sur **Sales_uspFillOrderTest** et vérifiez que **Test** est mis en surbrillance dans la liste adjacente.  
  
    Après avoir effectué cette étape, créez le script de test pour l'action de test dans le test unitaire.  
  
2.  Mettez à jour les instructions Transact \-SQL dans l'éditeur Transact\-SQL de façon à ce qu'elles correspondent aux instructions suivantes :  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
    -- Get the most recently added order.  
    SELECT @OrderID = MAX([OrderID]) FROM [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspFillOrder] @OrderID, @FilledDate;  
  
    -- verify that the YTDOrders value is correct.  
    SELECT @RC = [YTDSales] FROM [Sales].[Customer] WHERE [CustomerID] = @CustomerID  
  
    SELECT @RC AS RC;  
    ```  
  
3.  Dans le volet **Conditions de test** , cliquez sur une condition de test peu probante, puis cliquez sur **Supprimer la condition de test**.  
  
4.  Dans le volet **Conditions de test** , cliquez sur **Valeur scalaire** dans la liste, puis cliquez sur **Ajouter une condition de test**.  
  
5.  Dans la fenêtre **Propriétés** , affectez à la propriété **Valeur attendue** la valeur 100.  
  
6.  Dans la barre de navigation du Concepteur de test unitaire SQL Server, cliquez sur **Sales_uspFillOrderTest** et vérifiez que **Prétest** est mis en surbrillance dans la liste adjacente. Après avoir effectué cette étape, spécifiez les instructions qui placent les données dans l'état requis pour exécuter votre test. Pour cet exemple, vous devez créer l'enregistrement de client avant de pouvoir passer une commande.  
  
7.  Cliquez sur **Cliquez ici pour créer** pour créer un script de prétest.  
  
8.  Mettez à jour les instructions Transact \-SQL dans l'éditeur Transact\-SQL de façon à ce qu'elles correspondent aux instructions suivantes :  
  
    ```  
    /*  
    Add Transact-SQL statements here that you want to run before  
    the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place an order for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
    ```  
  
9. Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspshoworderdetails"></a>Pour écrire le test unitaire SQL Server pour uspShowOrderDetails  
  
1.  Dans la barre de navigation du Concepteur de test unitaire SQL Server, cliquez sur **Sales_uspShowOrderDetailsTest** et vérifiez que **Test** est mis en surbrillance dans la liste adjacente.  
  
    Après avoir effectué cette étape, créez le script de test pour l'action de test dans le test unitaire.  
  
2.  Mettez à jour les instructions Transact \-SQL dans l'éditeur Transact\-SQL de façon à ce qu'elles correspondent aux instructions suivantes :  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspShowOrderDetails] @CustomerID;  
  
    SELECT @RC AS RC;  
    ```  
  
3.  Dans le volet **Conditions de test** , cliquez sur une condition de test peu probante, puis cliquez sur **Supprimer la condition de test**.  
  
4.  Dans le volet **Conditions de test** , cliquez sur **Schéma attendu** dans la liste, puis cliquez sur **Ajouter une condition de test**.  
  
5.  Dans la fenêtre **Propriétés**, dans la propriété **Configuration**, cliquez sur le bouton Parcourir («  **…**  »).  
  
6.  Dans la boîte de dialogue **Configuration de expectedSchemaCondition1** , spécifiez une connexion à la base de données. Par exemple, si vous avez utilisé l'emplacement de déploiement par défaut, (LocalDB), cliquez sur **Nouvelle connexion** et spécifiez **(LocalDB)\Projects**. Ensuite, sélectionnez le nom la base de données.  
  
7.  Cliquez sur **Récupérer**. (Si nécessaire, cliquez sur **Récupérer** jusqu'à ce que les données s'affichent.)  
  
    Le corps Transact\-SQL du test unitaire est exécuté et le schéma résultant s'affiche dans la boîte de dialogue. Étant donné que le code de prétest n'a pas été exécuté, aucune donnée n'est retournée. Comme vous ne vérifiez que le schéma et non les données, cela convient.  
  
8.  Cliquez sur **OK**.  
  
    Le schéma attendu est enregistré avec la condition de test.  
  
9. Dans la barre de navigation du Concepteur de test unitaire SQL Server, cliquez sur **Sales_uspShowOrderDetailsTest** et vérifiez que **Prétest** est mis en surbrillance dans la liste adjacente. Après avoir effectué cette étape, spécifiez les instructions qui placent les données dans l'état requis pour exécuter votre test. Pour cet exemple, vous devez créer l'enregistrement de client avant de pouvoir passer une commande.  
  
10. Cliquez sur **Cliquez ici pour créer** pour créer un script de prétest.  
  
11. Mettez à jour les instructions Transact \-SQL dans l'éditeur Transact\-SQL de façon à ce qu'elles correspondent aux instructions suivantes :  
  
    ```  
    /*  
    Add Transact-SQL statements here to run before the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'FictitiousCustomer'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place 3 orders for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 100, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 50, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 5, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
    ```  
  
12. Dans la barre de navigation du Concepteur de test unitaire SQL Server, cliquez sur **Sales_uspShowOrderDetailsTest**, puis cliquez sur **Test** dans la liste adjacente.  
  
    Vous devez effectuer cette opération, car vous souhaitez appliquer l'état da condition de somme de contrôle au test, et non au prétest.  
  
13. Dans le volet **Conditions de test** , cliquez sur **Checksum de données** dans la liste, puis cliquez sur **Ajouter une condition de test**.  
  
14. Dans la fenêtre **Propriétés**, dans la propriété **Configuration**, cliquez sur le bouton Parcourir («  **…**  »).  
  
15. Dans la boîte de dialogue **Configuration de checksumCondition1** , spécifiez une connexion à la base de données.  
  
16. Remplacez le code Transact\-SQL de la boîte de dialogue (sous le bouton **Modifier la connexion**) par le code suivant :  
  
    ```  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place 3 orders for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 100, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 50, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 5, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @FilledDate AS DATETIME;  
    DECLARE @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspShowOrderDetails] @CustomerID;  
  
    SELECT @RC AS RC;  
    ```  
  
    Ce code associe le code Transact\-SQL du prétest au code Transact\-SQL du test proprement dit. Vous avez besoin des deux afin de retourner le même résultat que celui qui est retourné par le test lorsque vous l'exécutez.  
  
17. Cliquez sur **Récupérer**. (Si nécessaire, cliquez sur **Récupérer** jusqu'à ce que les données s'affichent.)  
  
    Le code Transact\-SQL que vous avez spécifié est exécuté, et une somme de contrôle est calculée pour les données retournées.  
  
18. Cliquez sur **OK**.  
  
    La somme de contrôle calculée est enregistrée avec la condition de test. La somme de contrôle attendue apparaît dans la colonne Valeur de la condition de test Checksum de données.  
  
19. Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
    À ce stade, vous êtes prêt à exécuter vos tests.  
  
## <a name="run-sql-server-unit-tests"></a><a name="RunTests"></a>Exécuter des tests unitaires SQL Server  
  
#### <a name="to-run-the-sql-server-unit-tests"></a>Pour exécuter des tests unitaires SQL Server  
  
1.  Dans le menu **Test**, pointez sur **Fenêtres**, puis cliquez sur **Affichage des tests** dans Visual Studio 2010 ou sur **Explorateur de test**  dans Visual Studio 2012.  
  
2.  Dans la fenêtre **Affichage des tests** (Visual Studio 2010), cliquez sur **Actualiser** dans la barre d'outils pour mettre à jour la liste de tests. Pour afficher la liste de tests dans l'**Explorateur de tests** (Visual Studio 2012), générez la solution.  
  
    La fenêtre **Affichage des tests** ou **Explorateur de tests** répertorie les tests créés précédemment dans cette procédure pas à pas auxquels vous avez ajouté des instructions Transact\-SQL et des conditions de test. Le test nommé TestMethod1 est vide et n'est pas utilisé dans cette procédure pas à pas.  
  
3.  Cliquez avec le bouton droit sur **Sales_uspNewCustomerTest**, puis cliquez sur **Exécuter la sélection**.  
  
    Visual Studio utilise le contexte privilégié que vous avez spécifié pour se connecter à la base de données et appliquer le plan de génération de données. Visual Studio bascule ensuite vers le contexte d'exécution avant d'exécuter le script Transact\-SQL dans le test. Enfin, Visual Studio évalue les résultats du script Transact\-SQL par rapport à ceux que vous avez spécifiés dans la condition de test, et un résultat de réussite ou d'échec s'affiche dans la fenêtre **Résultats des tests**.  
  
4.  Affichez le résultat des tests dans la fenêtre **Résultats des tests** .  
  
    Le test réussit, ce qui signifie que l'instruction **SELECT** retourne une ligne lorsqu'il est exécuté.  
  
5.  Répétez l'étape 3 pour les tests Sales_uspPlaceNewOrderTest, Sales_uspFillOrderTest de Sales_uspShowOrderDetailsTest. Les résultats doivent être les suivants :  
  
    |Test|Résultat attendu|  
    |--------|-------------------|  
    |Sales_uspPlaceNewOrderTest|Réussite|  
    |Sales_uspShowOrderDetailsTest|Réussite|  
    |Sales_uspFillOrderTest|Échec avec l’erreur suivante : « Échec de la condition ScalarValueCondition (scalarValueCondition2) : jeu de résultats 1 ligne 1 colonne 1 : les valeurs ne correspondent pas, réelles '-100' attendues '100'. » Cette erreur se produit, car la définition de la procédure stockée contient une erreur mineure.|  
  
    Ensuite, vous allez corriger l'erreur et réexécuter le test.  
  
#### <a name="to-correct-the-error-in-salesuspfillorder"></a>Pour résoudre l'erreur dans Sales.uspFillOrder  
  
1.  Dans le nœud Projets de l'**Explorateur d'objets SQL Server** pour votre base de données, double-cliquez sur la procédure stockée **uspFillOrder** pour ouvrir sa définition dans l'éditeur Transact\-SQL.  
  
2.  Dans la définition, recherchez l'instruction Transact\-SQL suivante :  
  
    ```  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales - @Delta  
        WHERE [CustomerID] = @CustomerID  
    ```  
  
3.  Modifiez la clause SET de l'instruction de façon à ce qu'elle corresponde à l'instruction suivante :  
  
    ```  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales + @Delta  
        WHERE [CustomerID] = @CustomerID  
    ```  
  
4.  Dans le menu **Fichier** , cliquez sur **Enregistrer uspFillOrder.sql**.  
  
5.  Dans **Affichage des tests**, cliquez avec le bouton droit sur **Sales_uspFillOrderTest**, puis cliquez sur **Exécuter la sélection**.  
  
    Le test réussit.  
  
## <a name="add-a-negative-unit-test"></a><a name="NegativeTest"></a>Ajouter un test unitaire négatif  
Créez un test négatif pour vérifier qu'un test échoue comme prévu. Par exemple, si vous essayez d'annuler une commande qui a déjà été passée, ce test doit échouer. Dans cette partie de la procédure pas à pas, vous créez un test unitaire négatif pour la procédure stockée Sales.uspCancelOrder.  
  
Pour créer et vérifier un test négatif, vous devez effectuer les tâches suivantes :  
  
-   Mettre à jour la procédure stockée pour tester les conditions d'échec  
  
-   Définir un nouveau test unitaire  
  
-   Modifier le code du test unitaire pour indiquer qu'il doit échouer  
  
-   Exécuter le test unitaire  
  
#### <a name="to-update-the-stored-procedure"></a>Pour mettre à jour la procédure stockée  
  
1.  Dans le nœud Projets de l'**Explorateur d'objets SQL Server** pour la base de données SimpleUnitTestDB, développez le nœud Programmabilité, développez le nœud Procédures stockées, puis double-cliquez sur uspCancelOrder.  
  
2.  Dans l'éditeur\-SQL, mettez à jour la définition de la procédure de façon à ce qu'elle corresponde au code suivant :  
  
    ```  
    CREATE PROCEDURE [Sales].[uspCancelOrder]  
    @OrderID INT  
    AS  
    BEGIN  
        DECLARE @Delta INT, @CustomerID INT, @PriorStatus CHAR(1)  
        BEGIN TRANSACTION  
            BEGIN TRY  
                IF (NOT EXISTS(SELECT [CustomerID] from [Sales].[Orders] WHERE [OrderID] = @OrderID))  
                BEGIN  
                    -- Specify WITH LOG option so that the error is  
                    -- written to the application log.  
                    RAISERROR( 'That order does not exist.', -- Message text  
                               16, -- severity  
                                1 -- state  
                            ) WITH LOG;  
                END  
  
                SELECT @Delta = [Amount], @CustomerID = [CustomerID], @PriorStatus = [Status]  
                 FROM [Sales].[Orders] WHERE [OrderID] = @OrderID  
  
                IF @PriorStatus <> 'O'   
                BEGIN  
                    -- Specify WITH LOG option so that the error is  
                    -- written to the application log.  
                    RAISERROR ( 'You can only cancel open orders.', -- Message text  
                                16, -- Severity  
                                1 -- State  
                                ) WITH LOG;  
                END  
                ELSE  
                BEGIN  
                    -- If we make it to here, then we can cancel the order. Update the status to 'X' first...  
                    UPDATE [Sales].[Orders]  
                       SET [Status] = 'X'  
                    WHERE [OrderID] = @OrderID  
                    -- and then remove the amount from the YTDOrders for the customer  
                    UPDATE [Sales].[Customer]  
                           SET  
                               YTDOrders = YTDOrders - @Delta  
                    WHERE [CustomerID] = @CustomerID  
                    COMMIT TRANSACTION  
                    RETURN 1; -- indicate success  
                END  
            END TRY  
            BEGIN CATCH  
                DECLARE @ErrorMessage NVARCHAR(4000);  
                DECLARE @ErrorSeverity INT;  
                DECLARE @ErrorState INT;  
  
                SELECT @ErrorMessage = ERROR_MESSAGE(),  
                       @ErrorSeverity = ERROR_SEVERITY(),  
                       @ErrorState = ERROR_STATE();  
  
                ROLLBACK TRANSACTION  
                -- Use RAISERROR inside the CATCH block to return  
                -- error information about the original error that  
                -- caused execution to jump to the CATCH block.  
                RAISERROR (@ErrorMessage, -- Mesasge text  
                           @ErrorSeverity, -- Severity  
                           @ErrorState -- State  
                          );  
                RETURN 0; -- indicate failure  
            END CATCH;  
    END  
    ```  
  
3.  Dans le menu **Fichier** , cliquez sur **Enregistrer uspCancelOrder.sql**.  
  
4.  Appuyez sur F5 pour déployer **SimpleUnitTestDB**.  
  
    Vous déployez les mises à jour de la procédure stockée uspCancelOrder. Vous n'avez modifié aucun un autre objet, par conséquent seule cette procédure stockée est mise à jour.  
  
    Ensuite, vous définissez le test unitaire associé pour cette procédure.  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspcancelorder"></a>Pour écrire le test unitaire SQL Server pour uspCancelOrder  
  
1.  Dans la barre de navigation du Concepteur de test unitaire SQL Server, cliquez sur **Sales_uspCancelOrderTest** et vérifiez que **Test** est mis en surbrillance dans la liste adjacente.  
  
    Après avoir effectué cette étape, créez le script de test pour l'action de test dans le test unitaire.  
  
2.  Mettez à jour les instructions Transact \-SQL dans l'éditeur Transact\-SQL de façon à ce qu'elles correspondent aux instructions suivantes :  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
    -- Get the most recently added order.  
    SELECT @OrderID = MAX([OrderID]) FROM [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
  
    -- try to cancel an order for that customer that has already been filled  
    EXECUTE @RC = [Sales].[uspCancelOrder] @OrderID;  
  
    SELECT @RC AS RC;  
    ```  
  
3.  Dans le volet **Conditions de test** , cliquez sur une condition de test peu probante, puis cliquez sur l'icône **Supprimer la condition de test** .  
  
4.  Dans le volet **Conditions de test** , cliquez sur **Valeur scalaire** dans la liste, puis cliquez sur l'icône **Ajouter une condition de test** .  
  
5.  Dans la fenêtre **Propriétés** , affectez la valeur 0 à la propriété **Valeur attendue** .  
  
6.  Dans la barre de navigation du Concepteur de test unitaire SQL Server, cliquez sur **Sales_uspCancelOrderTest** et vérifiez que **Prétest** est mis en surbrillance dans la liste adjacente. Après avoir effectué cette étape, spécifiez les instructions qui placent les données dans l'état requis pour exécuter votre test. Pour cet exemple, vous devez créer l'enregistrement de client avant de pouvoir passer une commande.  
  
7.  Cliquez sur **Cliquez ici pour créer** pour créer un script de prétest.  
  
8.  Mettez à jour les instructions Transact \-SQL dans l'éditeur Transact\-SQL de façon à ce qu'elles correspondent aux instructions suivantes :  
  
    ```  
    /*  
    Add Transact-SQL statements here to run before the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @FilledDate AS DATETIME, @Status AS CHAR (1), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
       @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
       @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place an order for that customer  
    EXECUTE @OrderID = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    -- fill the order for that customer  
    EXECUTE @RC = [Sales].[uspFillOrder] @OrderID, @FilledDate;  
  
    COMMIT TRANSACTION  
    ```  
  
9. Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
    À ce stade, vous êtes prêt à exécuter vos tests.  
  
#### <a name="to-run-the-sql-server-unit-tests"></a>Pour exécuter des tests unitaires SQL Server  
  
1.  Dans **Affichage des tests**, cliquez avec le bouton droit sur **Sales_uspCancelOrderTest**, puis cliquez sur **Exécuter la sélection**.  
  
2.  Affichez le résultat des tests dans la fenêtre **Résultats des tests** .  
  
    Le test échoue et l'erreur suivante s'affiche :  
  
    **La méthode de test TestProject1.SqlServerUnitTests1.Sales_uspCancelOrderTest a levé une exception : System.Data.SqlClient.SqlException : Vous pouvez uniquement annuler les commandes ouvertes.**  
  
    Ensuite, vous modifiez le code pour indiquer que l'exception est attendue.  
  
#### <a name="to-modify-the-code-for-the-unit-test"></a>Pour modifier le code du test unitaire  
  
1.  Dans l'**Explorateur de solutions**, développez **TestProject1**, cliquez avec le bouton droit sur **SqlServerUnitTests1.cs**, puis cliquez sur **Afficher le code**.  
  
2.  Dans l'éditeur de code, accédez à la méthode Sales_uspCancelOrderTest. Modifiez les attributs de la méthode de façon à ce qu'elle corresponde au code suivant :  
  
    ```  
    [TestMethod(), ExpectedSqlException(Severity=16, MatchFirstError=false, State=1)]  
    public void Sales_uspCancelOrderTest()  
    ```  
  
    Spécifiez que vous attendez une exception spécifique. Vous pouvez éventuellement spécifier un numéro d'erreur spécifique. Si vous n'ajoutez pas cet attribut, le test unitaire échoue et un message s'affiche dans la fenêtre Résultats de tests  
  
    > [!IMPORTANT]  
    > Actuellement, Visual Studio 2012 ne prend pas en charge l'attribut ExpectedSqlException. Pour plus d'informations sur la façon de contourner cela, consultez [Impossible d'exécuter le test unitaire de base de données « Échec attendu »](https://social.msdn.microsoft.com/Forums/en-US/e74e06ad-e3c9-4cb0-97ad-a6f235a52345/unable-to-run-quotexpected-failurequot-database-unit-test).  
  
3.  Dans le menu Fichier, cliquez sur Enregistrer SqlServerUnitTests1.cs.  
  
    Ensuite, réexécutez le test unitaire pour vérifier qu'il échoue comme prévu.  
  
#### <a name="to-re-run-the-sql-server-unit-tests"></a>Pour réexécuter des tests unitaires SQL Server  
  
1.  Dans **Affichage des tests**, cliquez avec le bouton droit sur **Sales_uspCancelOrderTest**, puis cliquez sur **Exécuter la sélection**.  
  
2.  Affichez le résultat des tests dans la fenêtre **Résultats des tests** .  
  
    Le test réussit, ce qui signifie que la procédure a échoué comme prévu.  
  
## <a name="next-steps"></a>Étapes suivantes  
Dans un projet standard, vous définissez des tests unitaires supplémentaires pour vérifier que tous les objets de base de données critiques fonctionnent correctement. Lorsque l'ensemble des tests est terminé, vous devez vérifier ces tests dans le contrôle de version pour les partager avec l'équipe.  
  
Après avoir généré une ligne de base, créez et modifiez les objets de base de données, puis créez des tests associés pour vérifier si une modification va interrompre le comportement attendu.  
  
## <a name="see-also"></a>Voir aussi  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Vérifier le code de la base de données à l’aide de tests unitaires SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
[Procédure : créer un test unitaire SQL Server vide](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)  
[Procédure : configurer l’exécution de test unitaire SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md)  
