---
title: Créer une procédure stockée | Microsoft Docs
description: Découvrez comment créer une procédure stockée Transact-SQL à l’aide de SQL Server Management Studio et à l’aide de l’instruction Transact-SQL CREATE PROCEDURE.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: quickstart
helpviewer_keywords:
- new stored procedures
- stored procedures [SQL Server], creating
- creating stored procedures
ms.assetid: 76e8a6ba-1381-4620-b356-4311e1331ca7
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c1ab9e2681f229fba5294f061d8d83e98aabac64
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475320"
---
# <a name="create-a-stored-procedure"></a>Créer une procédure stockée
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Cette rubrique explique comment créer une procédure stockée [!INCLUDE[tsql](../../includes/tsql-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE PROCEDURE.  
  
-   **Avant de commencer :**  [Autorisations](#Permissions)  
  
-   **Pour créer une procédure avec :**  [SQL Server Management Studio](#SSMSProcedure), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'autorisation CREATE PROCEDURE dans la base de données et l'autorisation ALTER sur le schéma dans lequel la procédure est créée.  
  
##  <a name="how-to-create-a-stored-procedure"></a><a name="Procedures"></a> Comment créer une procédure stockée  
 Vous pouvez utiliser l'un des éléments suivants :  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour créer une procédure dans l'Explorateur d'objets**  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et développez-la.  
  
2.  Développez **Bases de données**, la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , puis **Programmabilité**.  
  
3.  Cliquez avec le bouton droit sur **Procédures stockées**, puis cliquez sur **Nouvelle procédure stockée**.  
  
4.  Dans le menu **Requête** , cliquez sur **Spécifier les valeurs des paramètres du modèle**.  
  
5.  Dans la boîte de dialogue **Spécifier les valeurs des paramètres du modèle** , entrez les valeurs suivantes pour les paramètres affichés.  
  
    |Paramètre|Valeur|  
    |---------------|-----------|  
    |Auteur|*Votre nom*|  
    |Date de création|*Date du jour*|  
    |Description|Retourne des données sur les employés.|  
    |Procedure_name|HumanResources.uspGetEmployeesTest|  
    |@Param1|@LastName|  
    |@Datatype_For_Param1|**nvarchar**(50)|  
    |Default_Value_For_Param1|NULL|  
    |@Param2|@FirstName|  
    |@Datatype_For_Param2|**nvarchar**(50)|  
    |Default_Value_For_Param2|NULL|  
  
6.  Cliquez sur **OK**.  
  
7.  Dans l' **Éditeur de requête**, remplacez l'instruction SELECT par l'instruction suivante :  
  
    ```sql  
    SELECT FirstName, LastName, Department  
    FROM HumanResources.vEmployeeDepartmentHistory  
    WHERE FirstName = @FirstName AND LastName = @LastName  
        AND EndDate IS NULL;  
    ```  
  
8.  Pour tester la syntaxe, dans le menu **Requête** , cliquez sur **Analyser**. Si un message d'erreur est retourné, comparez les instructions avec les informations ci-dessus et apportez les corrections nécessaires.  
  
9. Pour créer la procédure, dans le menu **Requête** , cliquez sur **Exécuter**. La procédure est créée en tant qu'objet dans la base de données.  
  
10. Pour afficher la procédure répertoriée dans l’Explorateur d’objets, cliquez avec le bouton droit sur **Procédures stockées** et sélectionnez **Actualiser**.  
  
11. Pour exécuter la procédure, dans l’Explorateur d’objets, cliquez avec le bouton droit sur le nom de la procédure stockée **HumanResources.uspGetEmployeesTest** et sélectionnez **Exécuter la procédure stockée**.  
  
12. Dans la fenêtre **Exécuter la procédure** , entrez Margheim comme valeur pour le paramètre @LastName et entrez la valeur Diane comme valeur pour le paramètre @FirstName.  
  
> [!WARNING]  
>  Validez toutes les entrées utilisateur. Ne concaténez pas les entrées utilisateur avant de les avoir validées. N'exécutez jamais une commande élaborée à partir d'une entrée utilisateur non validée.  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour créer une procédure dans l'Éditeur de requête**  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans le menu **Fichier** , cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple crée la même procédure stockée que ci-dessus à l'aide d'un nom de procédure différent.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE PROCEDURE HumanResources.uspGetEmployeesTest2   
        @LastName nvarchar(50),   
        @FirstName nvarchar(50)   
    AS   
  
        SET NOCOUNT ON;  
        SELECT FirstName, LastName, Department  
        FROM HumanResources.vEmployeeDepartmentHistory  
        WHERE FirstName = @FirstName AND LastName = @LastName  
        AND EndDate IS NULL;  
    GO  
  
    ```  
  
4.  Pour exécuter la procédure, copiez et collez l'exemple suivant dans une nouvelle fenêtre de requête, puis cliquez sur **Exécuter**. Notez que différentes méthodes de spécification des valeurs de paramètre sont affichées.  
  
    ```  
    EXECUTE HumanResources.uspGetEmployeesTest2 N'Ackerman', N'Pilar';  
    -- Or  
    EXEC HumanResources.uspGetEmployeesTest2 @LastName = N'Ackerman', @FirstName = N'Pilar';  
    GO  
    -- Or  
    EXECUTE HumanResources.uspGetEmployeesTest2 @FirstName = N'Pilar', @LastName = N'Ackerman';  
    GO  
  
    ```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
  
  
