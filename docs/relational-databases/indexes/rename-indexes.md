---
description: Renommer des index
title: Renommer des index | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- renaming indexes
- index names [SQL Server]
- indexes [SQL Server], renaming
ms.assetid: d3d612a1-ea1b-4d99-85d2-0a2ad54f4b0e
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6e58f48dcaf91c4bc0621d064c7d5c6f44004e3d
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048846"
---
# <a name="rename-indexes"></a>Renommer des index
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cette rubrique explique comment renommer un index dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Le changement du nom d'un index remplace le nom actuel de l'index par le nouveau nom fourni. Le nom spécifié doit être unique dans la table ou la vue. Par exemple, deux tables peuvent contenir un index nommé **XPK_1**, mais la même table ne peut pas avoir deux index nommés **XPK_1**. Vous ne pouvez pas créer un index dont le nom est le même qu'un index désactivé existant. Le changement du nom d'un index ne provoque pas sa reconstruction.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour renommer un index, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
 Quand vous créez une contrainte PRIMARY KEY ou UNIQUE sur une table, un index du même nom que la contrainte est automatiquement créé pour la table. Comme les noms d'index doivent être uniques dans la table, vous ne pouvez pas créer ou renommer un index pour qu'il porte le même nom qu'une contrainte PRIMARY KEY ou UNIQUE sur la table.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert une autorisation ALTER sur l'index.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-rename-an-index-by-using-the-table-designer"></a>Pour renommer un index à l'aide du Concepteur de tables  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez renommer un index.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez avec le bouton droit sur la table sur laquelle vous souhaitez renommer un index et sélectionnez **Conception**.  
  
4.  Dans le menu **Concepteur de tables** , cliquez sur **Index/Clés**.  
  
5.  Sélectionnez l’index à renommer dans la zone de texte **Clé ou index Primary/Unique sélectionné** .  
  
6.  Dans la grille, cliquez sur **Nom** et tapez un nouveau nom dans la zone de texte.  
  
7.  Cliquez sur **Fermer**.  
  
8.  Dans le menu **Fichier** , cliquez sur **Enregistrer**_nom_table_.  

#### <a name="to-rename-an-index-by-using-object-explorer"></a>Pour renommer un index à l'aide de l'Explorateur d'objets  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez renommer un index.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table sur laquelle vous souhaitez renommer un index.  
  
4.  Cliquez sur le signe plus (+) pour développer le dossier **Index** .  
  
5.  Cliquez avec le bouton droit sur l’index que vous souhaitez renommer et sélectionnez **Renommer**.  
  
6.  Tapez le nouveau nom de l’index et appuyez sur Entrée.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-rename-an-index"></a>Pour renommer un index  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    --Renames the IX_ProductVendor_VendorID index on the Purchasing.ProductVendor table to IX_VendorID.   
  
    EXEC sp_rename N'Purchasing.ProductVendor.IX_ProductVendor_VendorID', N'IX_VendorID', N'INDEX';   
    GO  
    ```  
  
 Pour plus d’informations, consultez [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md).  
  
  
