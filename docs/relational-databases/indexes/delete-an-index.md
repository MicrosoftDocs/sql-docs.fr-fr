---
description: Supprimer un index
title: Supprimer un index | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- removing indexes
- deleting indexes
- dropping indexes
- indexes [SQL Server], dropping
- index deletions [SQL Server]
ms.assetid: fd38a0ed-26c4-4c76-9ef7-e0a16147329d
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: 5275fe35478ddfcccb7cd7f70da0d2d8c8998c7f
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048863"
---
# <a name="delete-an-index"></a>Supprimer un index
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Cette rubrique explique comment supprimer un index dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour supprimer un index, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
 Les index résultant d'une contrainte PRIMARY KEY ou UNIQUE ne peuvent pas être supprimés au moyen de cette méthode. Dans ce cas, c'est la contrainte qui doit être supprimée. Pour supprimer la contrainte et l'index correspondant, utilisez [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) avec la clause DROP CONSTRAINT dans [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour plus d’informations, consultez [Delete Primary Keys](../../relational-databases/tables/delete-primary-keys.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite une autorisation ALTER sur la table ou la vue. L’autorisation est accordée par défaut au rôle serveur fixe **sysadmin** et aux rôles de base de données fixes **db_ddladmin** et **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-delete-an-index-by-using-object-explorer"></a>Pour supprimer un index à l'aide de l'Explorateur d'objets  
  
1.  Dans l'Explorateur d'objets, développez la base de données qui contient la table sur laquelle vous souhaitez supprimer un index.  
  
2.  Développez le dossier **Tables** .  
  
3.  Développez la table contenant l'index à supprimer.  
  
4.  Développez le dossier **Index** .  
  
5.  Cliquez avec le bouton droit sur l’index à supprimer, puis sélectionnez **Supprimer**.  
  
6.  Dans la boîte de dialogue **Supprimer un objet** , vérifiez que l'index correct figure dans la grille **Objet à supprimer** , puis cliquez sur **OK**.  
  
#### <a name="to-delete-an-index-using-table-designer"></a>Pour supprimer un index à l'aide du Concepteur de tables  
  
1.  Dans l'Explorateur d'objets, développez la base de données qui contient la table sur laquelle vous souhaitez supprimer un index.  
  
2.  Développez le dossier **Tables** .  
  
3.  Cliquez avec le bouton droit sur la table contenant l'index que vous souhaitez supprimer et cliquez sur Conception.  
  
4.  Dans le menu **Concepteur de tables** , cliquez sur **Index/Clés**.  
  
5.  Dans la boîte de dialogue **Index/Clés** , sélectionnez l’index que vous souhaitez supprimer.  
  
6.  Cliquez sur **Supprimer**.  
  
7.  Cliquez sur **Fermer**.  
  
8.  Dans le menu **Fichier** , sélectionnez **Enregistrer**_nom_table_.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-delete-an-index"></a>Pour supprimer un index  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- delete the IX_ProductVendor_BusinessEntityID index  
    -- from the Purchasing.ProductVendor table  
    DROP INDEX IX_ProductVendor_BusinessEntityID   
        ON Purchasing.ProductVendor;  
    GO  
    ```  
  
 Pour plus d’informations, consultez [DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md).  
  
  
