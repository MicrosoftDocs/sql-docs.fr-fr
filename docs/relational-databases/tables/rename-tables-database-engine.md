---
description: Renommer des tables (moteur de base de données)
title: Renommer des tables (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 02/23/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- table renaming [SQL Server]
- table names [SQL Server]
- tables [SQL Server], Visual Database Tools
- renaming tables
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 87a05942a1061db1f074266d0b5df3b1797f5e73
ms.sourcegitcommit: a5398f107599102af7c8cda815d8e5e9a367ce7e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "92006128"
---
# <a name="rename-tables-database-engine"></a>Renommer des tables (moteur de base de données)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Renommez une table dans SQL Server ou Azure SQL Database.

Pour renommer une table dans Azure Synapse Analytics ou Parallel Data Warehouse, utilisez l’instruction T-SQL [RENAME OBJECT](../../t-sql/statements/rename-transact-sql.md). 
  
> [!CAUTION]  
>  Ne renommez une table qu'après mûre réflexion. En effet, s'il existe des requêtes, des vues, des fonctions définies par l'utilisateur, des procédures stockées ou des programmes qui font référence à cette table, le changement de nom rend tous ces objets non valides.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour renommer une table à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
 Le fait de renommer une table ne renomme pas automatiquement les références à cette table. Vous devez modifier manuellement tout objet qui référence la table renommée. Par exemple, si vous renommez une table et si cette table est référencée dans un déclencheur, vous devez modifier le déclencheur pour refléter le nouveau nom de table. Utilisez [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) pour obtenir la liste des dépendances de la table avant de la renommer.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert une autorisation ALTER sur la table.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-rename-a-table"></a>Pour renommer une table  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la table que vous souhaitez renommer et choisissez **Conception** dans le menu contextuel.  
  
2.  Dans le menu **Affichage** , choisissez **Propriétés** .  
  
3.  Dans le champ de la valeur **Nom** de la fenêtre **Propriétés** , tapez un nouveau nom pour la table.  
  
4.  Pour annuler cette action, appuyez sur la touche Échap avant de quitter ce champ.  
  
5.  Dans le menu **Fichier** , choisissez **Enregistrer** _nom de la table_ .  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-rename-a-table"></a>Pour renommer une table  
  
1.  Dans l' **Explorateur d'objets** , connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête** .  
  
3.  L'exemple suivant renomme la table `SalesTerritory` en `SalesTerr` dans le schéma `Sales` . Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter** .  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    EXEC sp_rename 'Sales.SalesTerritory', 'SalesTerr';  
    ```  
  
 Pour obtenir des exemples supplémentaires, consultez [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md).  
  
  
