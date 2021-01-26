---
description: Créer des index filtrés
title: Créer des index filtrés | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- filtered indexes [SQL Server], about filtered indexes
- designing indexes [SQL Server], filtered
- filtered indexes [SQL Server]
- nonclustered indexes [SQL Server], filtered
- indexes [SQL Server], filtered
ms.assetid: 25e1fcc5-45d7-4c53-8c79-5493dfaa1c74
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 546ec1476ecae0da0fd119c7b45d06c2cc4406f6
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764903"
---
# <a name="create-filtered-indexes"></a>Créer des index filtrés
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cette rubrique explique comment créer un index filtré dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Un index filtré est un index non cluster optimisé convenant tout particulièrement aux requêtes qui effectuent des sélections dans un sous-ensemble de données bien défini. Il utilise un prédicat de filtre pour indexer une partie des lignes de la table. Un index filtré bien conçu peut améliorer les performances des requêtes et réduire les coûts de maintenance et de stockage des index par rapport aux index de table entière.  
  
 Les index filtrés peuvent présenter les avantages suivants par rapport aux index de table entière :  
  
-   **Meilleures performances des requêtes et qualité de plan améliorée**  
  
     Un index filtré bien conçu améliore les performances des requêtes et la qualité du plan d'exécution car il est plus petit qu'un index non cluster de table entière et contient des statistiques filtrées. Les statistiques filtrées sont plus précises que les statistiques de table entière car elles couvrent uniquement les lignes de l'index filtré.  
  
-   **Coûts réduits de maintenance des index**  
  
     La maintenance d'un index intervient uniquement lorsque les instructions de langage de manipulation de données (DML) affectent les données de l'index. Un index filtré réduit les coûts de maintenance des index par rapport à un index non cluster de table entière car il est plus petit et sa maintenance n'a lieu que lorsque les données de l'index sont modifiées. Il est possible d'avoir un grand nombre d'index filtrés, notamment s'ils contiennent des données qui sont rarement modifiées. De la même façon, si un index filtré contient uniquement les données fréquemment modifiées, la taille réduite de l'index limite le coût de la mise à jour des statistiques.  
  
-   **Coûts réduits de stockage des index**  
  
     La création d'un index filtré peut réduire le stockage sur disque des index non cluster lorsqu'un index de table entière n'est pas nécessaire. Vous pouvez remplacer un index non cluster de table entière par plusieurs index filtrés sans augmenter considérablement le stockage nécessaire.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Remarques sur la conception](#Design)  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour créer un index filtré, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="design-considerations"></a><a name="Design"></a> Remarques sur la conception  
  
-   Lorsqu'une colonne contient seulement un petit nombre de valeurs pertinentes pour les requêtes, vous pouvez créer un index filtré sur ce sous-ensemble de valeurs. Ainsi, lorsque les valeurs d'une colonne sont principalement NULL et que la requête effectue uniquement des sélections dans les valeurs non NULL, vous pouvez créer un index filtré pour les lignes de données non NULL. L'index ainsi créé sera plus petit et coûtera moins cher en maintenance qu'un index non cluster de table entière défini sur les mêmes colonnes clés.  
  
-   Lorsqu'une table contient des lignes des données hétérogènes, vous pouvez créer un index filtré pour une ou plusieurs catégories de données. Ceci peut améliorer les performances des requêtes sur ces lignes de données en limitant la portée d'une requête à une région spécifique de la table. En outre, l'index ainsi créé sera plus petit et coûtera moins cher en maintenance qu'un index non cluster de table entière.  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Vous ne pouvez pas créer un index filtré sur une vue. Toutefois, l'optimiseur de requête peut tirer parti d'un index filtré défini sur une table référencée dans une vue. L'optimiseur de requête prend en considération un index filtré pour une requête qui effectue des sélections dans une vue si les résultats de la requête sont corrects.

-   Vous ne pouvez pas créer un index filtré sur une table quand la colonne à laquelle l’expression de filtre accède est d’un type de données CLR.
  
-   Les index filtrés présentent les avantages suivants par rapport aux vues indexées :  
  
    -   Coûts réduits de maintenance des index. Par exemple, le processeur de requêtes utilise moins de ressources processeur pour mettre à jour un index filtré qu'une vue indexée.  
  
    -   Qualité de plan améliorée. Par exemple, lors de la compilation de la requête, l'optimiseur de requête envisage beaucoup plus souvent d'utiliser un index filtré que la vue indexée équivalente.  
  
    -   Reconstructions d'index en ligne. Vous pouvez reconstruire des index filtrés alors qu'ils sont disponibles pour les requêtes. Les reconstructions d'index en ligne ne sont pas prises en charge pour les vues indexées. Pour plus d’informations, consultez l’option REBUILD pour [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
    -   Index non uniques. Les index filtrés peuvent être non uniques, alors que les vues indexées doivent être uniques.  
  
-   Les index filtrés sont définis sur une seule table et ne prennent en charge que les [opérateurs de comparaison](../../t-sql/language-elements/comparison-operators-transact-sql.md) simples. Si vous avez besoin d'une expression de filtre qui référence plusieurs tables ou présente une logique complexe, vous devez créer une vue. Les index filtrés ne prennent pas en charge les opérateurs `LIKE`. 
  
-   Il n'est pas nécessaire qu'une colonne de l'expression d'index filtré soit une colonne clé ou incluse dans la définition de l'index filtré si l'expression d'index filtré est équivalente au prédicat de requête et si la requête ne retourne pas la colonne dans l'expression d'index filtré avec les résultats de la requête.  
  
-   Une colonne de l'expression d'index filtré doit être une colonne clé ou incluse dans la définition de l'index filtré si le prédicat de la requête utilise cette colonne dans une comparaison qui n'est pas équivalente à l'expression d'index filtré.  
  
-   Une colonne de l'expression d'index filtré doit être une colonne clé ou incluse dans la définition de l'index filtré si la colonne se trouve dans le jeu de résultats de la requête.  
  
-   Il n'est pas nécessaire que la clé de l'index cluster de la table soit une colonne clé ou incluse dans la définition de l'index filtré. La clé de l'index cluster est automatiquement incluse dans tous les index non cluster, y compris les index filtrés.  
  
-   Si l'opérateur de comparaison spécifié dans l'expression d'index filtré de l'index filtré provoque une conversion de données implicite ou explicite, une erreur se produit si cette conversion se produit du côté gauche d'un opérateur de comparaison. Une solution consiste à écrire l'expression d'index filtré avec l'opérateur de conversion de données (CAST ou CONVERT) à droite de l'opérateur de comparaison.  

- Passez en revue les options SET nécessaires pour la création d’index filtré dans la syntaxe de [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite une autorisation ALTER sur la table ou la vue. L’utilisateur doit être membre du rôle serveur fixe **sysadmin** ou des rôles de base de données fixes **db_ddladmin** et **db_owner** . Pour modifier l'expression d'index filtré, utilisez CREATE INDEX WITH DROP_EXISTING.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-filtered-index"></a>Pour créer un index filtré  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez créer un index filtré.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table sur laquelle vous souhaitez créer un index filtré.  
  
4.  Cliquez avec le bouton droit sur le dossier **Index**, pointez sur **Nouvel index**, puis sélectionnez **Index non cluster...** .  
  
5.  Dans la boîte de dialogue **Nouvel index** , sur la page **Général** , entrez le nom du nouvel index dans la zone **Nom de l'index** .  
  
6.  Sous **Colonnes clés d’index**, cliquez sur **Ajouter…** .  
  
7.  Dans la boîte de dialogue **Sélectionner des colonnes dans** _nom\_table_, cochez les cases correspondant aux colonnes de table à ajouter à l’index unique.  
  
8.  Cliquez sur **OK**.  
  
9. Dans la page **Filtre**, sous **Expression de filtre**, entrez l’expression SQL que vous utiliserez pour créer l’index filtré.  
  
10. Cliquez sur **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-a-filtered-index"></a>Pour créer un index filtré  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Looks for an existing filtered index named "FIBillOfMaterialsWithEndDate"  
    -- and deletes it from the table Production.BillOfMaterials if found.   
    IF EXISTS (SELECT name FROM sys.indexes  
        WHERE name = N'FIBillOfMaterialsWithEndDate'  
        AND object_id = OBJECT_ID (N'Production.BillOfMaterials'))  
    DROP INDEX FIBillOfMaterialsWithEndDate  
        ON Production.BillOfMaterials  
    GO  
    -- Creates a filtered index "FIBillOfMaterialsWithEndDate"  
    -- on the table Production.BillOfMaterials   
    -- using the columms ComponentID and StartDate.  
  
    CREATE NONCLUSTERED INDEX FIBillOfMaterialsWithEndDate  
        ON Production.BillOfMaterials (ComponentID, StartDate)  
        WHERE EndDate IS NOT NULL ;  
    GO  
    ```  
  
     L'index filtré ci-dessus est valide pour la requête suivante. Vous pouvez afficher le plan d'exécution de la requête pour déterminer si l'optimiseur de requête a utilisé l'index filtré.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT ProductAssemblyID, ComponentID, StartDate   
    FROM Production.BillOfMaterials  
    WHERE EndDate IS NOT NULL   
        AND ComponentID = 5   
        AND StartDate > '01/01/2008' ;  
    GO  
    ```  
  
#### <a name="to-ensure-that-a-filtered-index-is-used-in-a-sql-query"></a>Pour vous assurer qu'un index filtré est utilisé dans une requête SQL  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT ComponentID, StartDate FROM Production.BillOfMaterials  
        WITH ( INDEX ( FIBillOfMaterialsWithEndDate ) )   
    WHERE EndDate IN ('20000825', '20000908', '20000918');   
    GO  
    ```  
  
 Pour plus d’informations, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
  
