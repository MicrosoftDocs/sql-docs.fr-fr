---
title: Renommer une procédure stockée | Microsoft Docs
description: Découvrez comment renommer une procédure stockée dans SQL Server 2019 (15.x) à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], renaming
- renaming stored procedures
ms.assetid: 5d2e4c68-7e0b-4405-8919-f5b203e46770
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 433fa034c85418919aa48886c3a7c06757de890f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347671"
---
# <a name="rename-a-stored-procedure"></a>Renommer une procédure stockée

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette rubrique explique comment renommer une procédure stockée dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour renommer une procédure stockée à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Les noms des procédures doivent respecter les conventions concernant les [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
-   Le fait de renommer une procédure stockée conserve la valeur `object_id` et toutes les autorisations spécifiquement assignées à la procédure. Le fait de supprimer et de recréer l’objet crée une nouvelle valeur `object_id` et supprime toutes les autorisations spécifiquement assignées à la procédure.

-   Le fait de renommer une procédure stockée ne modifie pas le nom de l’objet correspondant dans la colonne de définition de la vue de catalogue **sys.sql_modules**. Pour ce faire, vous devez supprimer et recréer la procédure stockée avec son nouveau nom.  

-   La modification du nom ou de la définition d'une procédure peut entraîner l'échec de ses objets dépendants si ceux-ci n'ont pas été mis à jour pour refléter les modifications apportées à la procédure. Pour plus d’informations, consultez [Afficher les dépendances d’une procédure stockée](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 CREATE PROCEDURE  
 Nécessite l’autorisation CREATE PROCEDURE dans la base de données et l’autorisation ALTER sur le schéma dans lequel la procédure est créée, ou nécessite l’appartenance au rôle de base de données fixe **db_ddladmin**.  
  
 ALTER PROCEDURE  
 Requiert l’autorisation ALTER sur la procédure ou l’appartenance au rôle de base de données fixe **db_ddladmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-rename-a-stored-procedure"></a>Pour renommer une procédure stockée  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] et développez-la.  
2.  Développez **Bases de données**, développez la base de données à laquelle appartient la procédure, puis développez **Programmabilité**.  
3.  [Déterminez les dépendances de la procédure stockée](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
4.  Développez **Procédures stockées**, cliquez avec le bouton droit sur la procédure à renommer, puis cliquez sur **Renommer**.  
5.  Modifiez le nom de la procédure.  
6.  Modifiez le nom de la procédure référencé dans tous les objets ou scripts dépendants.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-rename-a-stored-procedure"></a>Pour renommer une procédure stockée  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment renommer une procédure en la supprimant puis en la recréant avec un nouveau nom. Le premier exemple crée la procédure stockée `'HumanResources.uspGetAllEmployeesTest`. Le deuxième exemple renomme la procédure stockée en `HumanResources.uspEveryEmployeeTest`.  
  
```sql  
--Create the stored procedure.  
USE AdventureWorks2012;  
GO  

CREATE PROCEDURE HumanResources.uspGetAllEmployeesTest  
AS  
    SET NOCOUNT ON;  
    SELECT LastName, FirstName, Department  
    FROM HumanResources.vEmployeeDepartmentHistory;  
GO  
  
--Rename the stored procedure.  
EXEC sp_rename 'HumanResources.uspGetAllEmployeesTest', 'uspEveryEmployeeTest'; 
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [Créer une procédure stockée](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [Modifier une procédure stockée](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [Supprimer une procédure stockée](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)   
 [Afficher la définition d'une procédure stockée](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)   
 [Afficher les dépendances d’une procédure stockée](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)  
  
  
