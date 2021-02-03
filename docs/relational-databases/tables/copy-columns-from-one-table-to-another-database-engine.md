---
description: Copier des colonnes d'une table vers une autre (Moteur de base de données)
title: Copier des colonnes d’une table vers une autre (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 09/01/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- copying columns
- columns [SQL Server], copying
ms.assetid: 5f5e70dc-69f9-44b8-bc48-b5d51ac20d77
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 46341304bf8f05e53a4143e8c5a151d2f9023155
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181496"
---
# <a name="copy-columns-from-one-table-to-another-database-engine"></a>Copier des colonnes d'une table vers une autre (Moteur de base de données)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Cette rubrique explique comment copier des colonnes d'une table vers une autre, en copiant uniquement la définition de la colonne ou la définition et les données dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour copier des colonnes à l’aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
 Lorsque vous copiez une colonne contenant un type de données alias d'une base de données dans l'autre, le type de données alias risque de ne pas être disponible dans la base de données cible. En pareil cas, la colonne se voit assigner le type de données de base le plus proche disponible dans cette base de données.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert une autorisation ALTER sur la table.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-copy-column-definitions-from-one-table-to-another"></a>Pour copier des définitions de colonne d'une table vers une autre  
  
1.  Ouvrez la table contenant les colonnes à copier et celle dans laquelle vous souhaitez les copier en cliquant avec le bouton droit sur les tables, puis cliquez sur **Conception**.  
  
2.  Cliquez sur l'onglet de la table contenant les colonnes que vous souhaitez copier et sélectionnez ces colonnes.  
  
3.  Dans le menu **Edition** , cliquez sur **Copier**.  
  
4.  Cliquez sur l'onglet de la table dans laquelle vous souhaitez copier les colonnes.  
  
5.  Sélectionnez la colonne qui doit suivre les colonnes insérées, puis cliquez sur **Coller** dans le menu **Edition**.  

#### <a name="to-copy-data-from-one-table-to-another"></a>Pour copier des données d'une table vers une autre  
  
1.  Conformez-vous aux instructions relatives à la copie des définitions de colonne ci-dessus.  
  
    > [!NOTE]  
    >  Avant de commencer à copier des données d'une table vers une autre, assurez-vous que les types de données contenus dans les colonnes de destination sont compatibles avec ceux des données des colonnes sources.  
  
2.  Ouvrez une nouvelle fenêtre de l’éditeur de requête. 

3.  Cliquez avec le bouton droit sur l’éditeur de requête, puis cliquez sur **Concevoir une requête dans l’éditeur**. 

4.  Dans la boîte de dialogue **Ajouter une table** , sélectionnez la table source et de destination, cliquez sur **Ajouter**, puis fermez la boîte de dialogue **Ajouter une table** . 

5.  Cliquez avez le bouton droit dans une zone ouverte de l’éditeur de requête, pointez sur **Modifier le type**, puis cliquez sur **Insérer les résultats**.  

6.  Dans la boîte de dialogue **Choisir la table cible pour la requête Insérer les résultats** , sélectionnez la table de destination. 

7.  Dans la partie supérieure du concepteur de requêtes, cliquez sur la colonne source de la table source.

8. Le concepteur de requêtes a créé une requête INSERT. Cliquez sur OK pour placer la requête dans la fenêtre d’origine de l’éditeur de requête.  

9.  Exécutez la requête pour insérer les données de la table source dans la table de destination.

  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-copy-column-definitions-from-one-table-to-another"></a>Pour copier des définitions de colonne d'une table vers une autre  
  
1.  Vous ne pouvez pas copier des colonnes individuelles d'une table dans une autre table existante à l'aide d'instructions Transact-SQL. Toutefois, vous pouvez créer une table dans le groupe de fichiers par défaut et y insère les lignes résultant de la requête à l'aide de SELECT INTO. Pour plus d’informations, consultez [Clause INTO &#40;Transact-SQL&#41;](../../t-sql/queries/select-into-clause-transact-sql.md).  
  
#### <a name="to-copy-data-from-one-table-to-another"></a>Pour copier des données d'une table vers une autre  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE TABLE dbo.EmployeeSales  
    ( BusinessEntityID   varchar(11) NOT NULL,  
      SalesYTD money NOT NULL  
    );  
    GO  
    INSERT INTO dbo.EmployeeSales  
        SELECT BusinessEntityID, SalesYTD   
        FROM Sales.SalesPerson;  
    GO  
    ```  
  
  
