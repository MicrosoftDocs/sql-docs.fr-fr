---
description: Créer des index non cluster
title: Créer des index non cluster | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- index creation [SQL Server], nonclustered indexes
- nonclustered indexes [SQL Server], creating
- nonclustered indexes [SQL Server], UNIQUE constraint
- indexes [SQL Server], nonclustered
- nonclustered indexes [SQL Server], PRIMARY KEY constraint
ms.assetid: 9402029a-1227-46c4-93aa-c2122eb1b943
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9200edd16f0a33a52b12493660b18cfe45ae6086
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048947"
---
# <a name="create-nonclustered-indexes"></a>Créer des index non cluster
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Vous pouvez créer des index non-cluster dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Un index non-cluster est une structure d'index séparé des données stockées dans une table qui réorganise une ou plusieurs colonnes sélectionnées. Les index non-cluster peuvent vous aider à trouver plus rapidement les données au lieu de rechercher dans la table sous-jacente. Il est parfois possible de répondre entièrement aux requêtes selon les données dans l'index non-cluster, ou l'index non-cluster peut indiquer au [!INCLUDE[ssDE](../../includes/ssde-md.md)] les lignes dans la table sous-jacente. En général, les index non-cluster sont créés pour améliorer les performances des requêtes fréquemment utilisées qui ne sont pas couvertes par l'index cluster ou pour rechercher des lignes dans une table sans index cluster (ce qui s'appelle un « segment »). Vous pouvez créer plusieurs index non cluster sur une table ou une vue indexée.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="typical-implementations"></a><a name="Implementations"></a> Implémentations types  
 Les index non-cluster sont implémentés comme ceci :  
  
-   **Contraintes UNIQUE**  
  
     Lorsque vous créez une contrainte UNIQUE, un index non-cluster unique est créé pour appliquer par défaut une contrainte UNIQUE. Vous pouvez spécifier un index cluster unique si aucun index cluster n'existe déjà sur la table. Pour plus d’informations, consultez [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
-   **Index indépendant d'une contrainte**  
  
     Par défaut, un index non-cluster est créé si l'option CLUSTERED n'est pas spécifiée. Le nombre maximal d'index non cluster pouvant être créés par table est de 999. Cela inclut tous les index créés par des contraintes PRIMARY KEY ou UNIQUE, mais pas les index XML.  
  
-   **Index non-cluster sur une vue indexée**  
  
     Une fois qu'un index cluster unique a été créé sur une vue, vous pouvez créer des index non-cluster. Pour plus d’informations, consultez [Créer des vues indexées](../../relational-databases/views/create-indexed-views.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite une autorisation ALTER sur la table ou la vue. L’utilisateur doit être membre du rôle serveur fixe **sysadmin** ou des rôles de base de données fixes **db_ddladmin** et **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-nonclustered-index-by-using-the-table-designer"></a>Pour créer un index non-cluster à l'aide du Concepteur de tables  
  
1.  Dans l'Explorateur d'objets, développez la base de données qui contient la table sur laquelle vous souhaitez créer un index non-cluster.  
  
2.  Développez le dossier **Tables** .  
  
3.  Cliquez avec le bouton droit sur la table sur laquelle vous souhaitez créer un index non-cluster, puis sélectionnez **Conception**.  
  
4.  Dans le menu **Concepteur de tables** , cliquez sur **Index/Clés**.  
  
5.  Dans la boîte de dialogue **Index/Clés** , cliquez sur **Ajouter**.  
  
6.  Sélectionnez le nouvel index dans la zone de texte **Clé ou index Primary/Unique sélectionné** .  
  
7.  Dans la grille, sélectionnez **Créer comme Clustered** et choisissez **Non** dans la liste déroulante à droite de la propriété.  
  
8.  Cliquez sur **Fermer**.  
  
9. Dans le menu **Fichier** , cliquez sur **Enregistrer**_nom_table_.  

#### <a name="to-create-a-nonclustered-index-by-using-object-explorer"></a>Pour créer un index non-cluster à l'aide de l'Explorateur d'objets  
  
1.  Dans l'Explorateur d'objets, développez la base de données qui contient la table sur laquelle vous souhaitez créer un index non-cluster.  
  
2.  Développez le dossier **Tables** .  
  
3.  Développez la table sur laquelle vous souhaitez créer un index non-cluster.  
  
4.  Cliquez avec le bouton droit sur le dossier **Index**, pointez sur **Nouvel index**, puis sélectionnez **Index non cluster...** .  
  
5.  Dans la boîte de dialogue **Nouvel index** , sur la page **Général** , entrez le nom du nouvel index dans la zone **Nom de l'index** .  
  
6.  Sous **Colonnes clés d’index**, cliquez sur **Ajouter…** .  
  
7.  Dans la boîte de dialogue **Sélectionnez les colonnes à partir de**_nom_table_ , cochez la ou les cases de la ou des colonnes de table à ajouter à l’index non cluster.  
  
8.  Cliquez sur **OK**.  
  
9. Dans la boîte de dialogue **Nouvel index** , cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-a-nonclustered-index-on-a-table"></a>Pour créer un index non-cluster sur une table  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    -- Find an existing index named IX_ProductVendor_VendorID and delete it if found.   
    IF EXISTS (SELECT name FROM sys.indexes  
                WHERE name = N'IX_ProductVendor_VendorID')   
        DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;   
    GO  
    -- Create a nonclustered index called IX_ProductVendor_VendorID   
    -- on the Purchasing.ProductVendor table using the BusinessEntityID column.   
    CREATE NONCLUSTERED INDEX IX_ProductVendor_VendorID   
        ON Purchasing.ProductVendor (BusinessEntityID);   
    GO  
    ```  
  
## <a name="related-content"></a>Contenu connexe  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
[Guide de conception d’index SQL Server](../../relational-databases/sql-server-index-design-guide.md) 
