---
description: Créer des index uniques
title: Créer des vues uniques | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- unique indexes
- designing indexes [SQL Server], unique
- clustered indexes, unique
- indexes [SQL Server], unique
- nonclustered indexes [SQL Server], unique
- unique indexes, design guidelines
ms.assetid: 56b5982e-cb94-46c0-8fbb-772fc275354a
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e2c41e97ad36387fe1f3644e5039eb5cd2b6a61d
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048921"
---
# <a name="create-unique-indexes"></a>Créer des index uniques
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cette rubrique explique comment créer un index unique sur une table dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Un index unique garantit que la clé d'index ne contient aucune valeur dupliquée et que, par conséquent, chaque ligne de la table est unique d'une certaine manière. Il n'existe pas de différence notable entre la création d'une contrainte UNIQUE et la création d'un index unique indépendant de toute contrainte. La validation des données se produit d'une manière similaire et l'optimiseur de requête ne fait aucune distinction entre un index unique créé à partir d'une contrainte et un index unique créé manuellement. Toutefois, la création d'une contrainte UNIQUE sur la colonne permet de clarifier l'objectif de l'index. Pour plus d'informations sur les contraintes UNIQUE, consultez [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
 Lorsque vous créez un index unique, une option vous permet d'ignorer les clés en double. Si vous affectez à cette option la valeur **Oui** et que vous tentez de créer une clé dupliquée en ajoutant des données concernant plusieurs lignes (avec l’instruction INSERT), la ligne qui contient le doublon n’est pas ajoutée. Si elle a la valeur **Non**, l'intégralité de l'opération INSERT échoue et toutes les données sont restaurées.  
  
> [!NOTE]  
>  Il n'est pas possible de créer un index unique sur une seule colonne si cette colonne contient des valeurs null dans plusieurs lignes. De même, il n'est pas possible de créer un index unique sur plusieurs colonnes si la combinaison des colonnes contient des valeurs null dans plusieurs lignes. En effet, ces valeurs sont traitées comme des doublons par l'indexation.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Avantages d'un index unique](#Benefits)  
  
     [Implémentations types](#Implementations)  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour créer un index unique sur une table, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="benefits-of-a-unique-index"></a><a name="Benefits"></a> Avantages d'un index unique  
  
-   Les index uniques multicolonnes garantissent que chaque combinaison de valeurs dans la clé d'index est unique. Par exemple, si un index unique est créé sur une combinaison des colonnes **LastName**, **FirstName** et **MiddleName** , deux lignes de la table ne peuvent pas posséder la même combinaison de valeurs pour ces colonnes.  
  
-   À condition que les données contenues dans chaque colonne soient uniques, vous pouvez créer à la fois un index cluster unique et plusieurs index non cluster uniques sur la même table.  
  
-   Les index uniques garantissent l'intégrité des données des colonnes définies.  
  
-   Les index uniques fournissent des informations supplémentaires utiles à l'optimiseur de requête qui peut produire des plans d'exécution plus efficaces.  
  
###  <a name="typical-implementations"></a><a name="Implementations"></a> Implémentations types  
 Les index uniques sont implémentés à l'aide des méthodes suivantes :  
  
-   **Contrainte PRIMARY KEY ou UNIQUE**  
  
     La création d'une contrainte PRIMARY KEY entraîne la création automatique d'un index cluster unique sur la ou les colonnes pour autant qu'il n'existe pas encore d'index cluster dans la table ou qu'un index non-cluster unique n'ait pas été défini explicitement. La colonne clé primaire ne peut pas contenir de valeurs NULL.  
  
     Lorsque vous créez une contrainte UNIQUE, un index non-cluster unique est créé pour appliquer par défaut une contrainte UNIQUE. Vous pouvez spécifier un index cluster unique si aucun index cluster n'existe déjà sur la table.  
  
     Pour plus d'informations, consultez [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md) et [Primary and Foreign Key Constraints](../../relational-databases/tables/primary-and-foreign-key-constraints.md).  
  
-   **Index indépendant d'une contrainte**  
  
     Vous pouvez définir plusieurs index non-cluster uniques dans une table.  
  
     Pour plus d’informations, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
-   **Vue indexée**  
  
     Pour créer une vue indexée, vous devez définir un index cluster unique sur une ou plusieurs colonnes de vue. La vue est exécutée et le jeu de résultats est stocké au niveau feuille de l'index de la même manière que les données de table sont stockées dans un index cluster. Pour plus d’informations, consultez [Créer des vues indexées](../../relational-databases/views/create-indexed-views.md).  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Un index unique, une contrainte UNIQUE ou une contrainte PRIMARY KEY ne peuvent pas être créés si les données comportent des valeurs de clé dupliquées.  
  
-   Un index non cluster unique peut contenir des colonnes non-clés incluses. Pour plus d’informations, consultez [Créer des index avec colonnes incluses](../../relational-databases/indexes/create-indexes-with-included-columns.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite une autorisation ALTER sur la table ou la vue. L’utilisateur doit être membre du rôle serveur fixe **sysadmin** ou des rôles de base de données fixes **db_ddladmin** et **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-unique-index-by-using-the-table-designer"></a>Pour créer un index unique à l'aide du Concepteur de tables  
  
1.  Dans l'Explorateur d'objets, développez la base de données qui contient la table sur laquelle vous souhaitez créer un index unique.  
  
2.  Développez le dossier **Tables** .  
  
3.  Cliquez avec le bouton droit sur la table sur laquelle vous souhaitez créer un index unique, puis sélectionnez **Conception**.  
  
4.  Dans le menu **Concepteur de tables** , sélectionnez **Index/Clés**.  
  
5.  Dans la boîte de dialogue **Index/Clés** , cliquez sur **Ajouter**.  
  
6.  Sélectionnez le nouvel index dans la zone de texte **Clé ou index Primary/Unique sélectionné** .  
  
7.  Dans la grille principale, sous **(Général)** , sélectionnez **Type** puis choisissez **Index** dans la liste.  
  
8.  Sélectionnez **Colonnes**, puis cliquez sur les points de suspension **(…)** .  
  
9. Dans la boîte de dialogue **Colonnes d'index** , sous **Nom de la colonne**, sélectionnez les colonnes que vous souhaitez indexer. Vous pouvez sélectionner jusqu'à 16 colonnes. Si vous souhaitez que les performances soient optimales, évitez d'en sélectionner plus de deux par index. Pour chaque colonne sélectionnée, vous pouvez indiquer si l'index organise ses valeurs en ordre croissant ou décroissant.  
  
10. Lorsque toutes les colonnes sont sélectionnées pour l'index, cliquez sur **OK**.  
  
11. Dans la grille, sous **(Général)** , sélectionnez **Est unique** , puis choisissez **Oui** dans la liste.  
  
12. Facultatif : Dans la grille principale, sous **Concepteur de tables**, sélectionnez **Ignorer les clés dupliquées** , puis choisissez **Oui** dans la liste. Effectuez cette opération si vous souhaitez ignorer les tentatives d'ajout de données qui créeraient une clé dupliquée dans l'index unique.  
  
13. Cliquez sur **Fermer**.  
  
14. Dans le menu **Fichier**, cliquez sur **Enregistrer**_nom\_table_.  
  
#### <a name="create-a-unique-index-by-using-object-explorer"></a>Créer un index unique à l'aide de l'Explorateur d'objets  
  
1.  Dans l'Explorateur d'objets, développez la base de données qui contient la table sur laquelle vous souhaitez créer un index unique.  
  
2.  Développez le dossier **Tables** .  
  
3.  Développez la table sur laquelle vous souhaitez créer un index unique.  
  
4.  Cliquez avec le bouton droit sur le dossier **Index**, pointez sur **Nouvel index**, puis sélectionnez **Index non cluster...** .  
  
5.  Dans la boîte de dialogue **Nouvel index** , sur la page **Général** , entrez le nom du nouvel index dans la zone **Nom de l'index** .  
  
6.  Activez la case à cocher **Unique** .  
  
7.  Sous **Colonnes clés d’index**, cliquez sur **Ajouter…** .  
  
8.  Dans la boîte de dialogue **Sélectionner des colonnes dans** _nom\_table_, cochez les cases correspondant aux colonnes de table à ajouter à l’index unique.  
  
9. Cliquez sur **OK**.  
  
10. Dans la boîte de dialogue **Nouvel index** , cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-a-unique-index-on-a-table"></a>Pour créer un index unique sur une table  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Find an existing index named AK_UnitMeasure_Name and delete it if found  
    IF EXISTS (SELECT name from sys.indexes  
               WHERE name = N'AK_UnitMeasure_Name')   
       DROP INDEX AK_UnitMeasure_Name ON Production.UnitMeasure;   
    GO  
    -- Create a unique index called AK_UnitMeasure_Name  
    -- on the Production.UnitMeasure table using the Name column.  
    CREATE UNIQUE INDEX AK_UnitMeasure_Name   
       ON Production.UnitMeasure (Name);   
    GO  
    ```  
  
 Pour plus d’informations, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
  
