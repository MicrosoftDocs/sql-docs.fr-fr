---
description: Spécifier un facteur de remplissage pour un index
title: Spécifier un facteur de remplissage pour un index | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- fill factor [SQL Server]
- page splits [SQL Server]
ms.assetid: 237a577e-b42b-4adb-90cf-aa7fb174f3ab
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: e1e0a2b406d6817c43a5a4a51bea90b556c5cea6
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048772"
---
# <a name="specify-fill-factor-for-an-index"></a>Spécifier un facteur de remplissage pour un index
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Cette rubrique explique ce qu'est le facteur de remplissage et comment spécifier une valeur de facteur de remplissage sur un index dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 L'option de facteur de remplissage permet de paramétrer précisément les performances et le stockage des données d'index. Lorsqu'un index est créé ou régénéré, la valeur du facteur de remplissage détermine le pourcentage d'espace sur chaque page de niveau feuille à remplir de données, réservant ainsi le reste de chaque page comme espace disponible pour une croissance future. Par exemple, une valeur de facteur de remplissage de 80 signifie que 20 pour cent de chaque page de niveau feuille reste vide afin que les index puissent s'étendre à mesure que des données sont ajoutées à la table sous-jacente. L'espace vide est réservé entre les lignes d'index plutôt qu'à la fin de l'index.  
  
 La valeur du facteur de remplissage est un pourcentage de 1 à 100 et la valeur par défaut à l'échelle du serveur est 0, ce qui signifie que les pages de niveau feuille sont entièrement remplies.  
  
> [!NOTE]  
>  Les valeurs du facteur de remplissage 0 et 100 sont identiques à tous les égards.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Considérations relatives aux performances](#Performance)  
  
     [Sécurité](#Security)  
  
-   **Pour spécifier un taux de remplissage dans un index, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="performance-considerations"></a><a name="Performance"></a> Considérations relatives aux performances  
  
#### <a name="page-splits"></a>Fractionnements de pages  
 Une valeur de facteur de remplissage correctement choisie peut réduire les risques de fractionnements de pages en fournissant suffisamment d'espace pour l'extension des index à mesure que des données sont ajoutées à la table sous-jacente. Lorsqu'une nouvelle ligne est ajoutée à une page d'index complète, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] déplace environ la moitié des lignes vers une nouvelle page afin de faire de la place pour la nouvelle ligne. Ce type de réorganisation est désigné par l'expression « fractionnement de pages ». Un fractionnement de pages fait de la place pour de nouveaux enregistrements, mais cette opération peut prendre du temps et est gourmande en ressources. En outre, il peut causer une fragmentation qui accroît les opérations d'E/S. Lorsque des fractionnements de pages se produisent fréquemment, vous pouvez régénérer l'index à l'aide d'une valeur de facteur de remplissage nouvelle ou existante afin de redistribuer les données. Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 Bien qu'une faible valeur de facteur de remplissage, différente de 0, puisse réduire la nécessité de fractionner les pages à mesure que l'index s'étend, ce dernier a besoin de davantage d'espace de stockage et peut diminuer les performances de lecture. Même pour une application orientée vers de nombreuses opérations d'insertion et de mise à jour, le nombre de lectures effectuées dans la base de données est généralement 5 à 10 fois plus élevé que le nombre d'écritures. Par conséquent, la spécification d'un facteur de remplissage différent de la valeur par défaut peut diminuer les performances de lecture dans la base de données selon un coefficient inversement proportionnel au facteur de remplissage défini. Par exemple, un facteur de remplissage de 50 peut diminuer de moitié les performances de lecture dans la base de données. Les performances de lecture diminuent, car l'index contient davantage de pages, ce qui augmente le nombre d'opérations d'E/S disque nécessaires à la récupération des données.  
  
#### <a name="adding-data-to-the-end-of-the-table"></a>Ajout de données à la fin de la table  
 Un facteur de remplissage différent de 0 ou 100 peut être bénéfique pour les performances si les nouvelles données sont réparties de manière égale dans l'ensemble de la table. Toutefois, si toutes les données sont ajoutées à la fin de la table, l'espace vide dans les pages d'index n'est pas rempli. Par exemple, si la colonne clé de l'index est une colonne IDENTITY, la clé des nouvelles lignes augmente sans cesse et les lignes d'index sont ajoutées logiquement à la fin de l'index. Si les lignes existantes sont mises à jour avec des données qui font augmenter la taille des lignes, utilisez un facteur de remplissage inférieur à 100. Les octets supplémentaires de chaque page permettent de réduire les fractionnements de pages provoqués par l'augmentation de la taille des lignes.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite une autorisation ALTER sur la table ou la vue. L’utilisateur doit être membre du rôle serveur fixe **sysadmin** ou des rôles de base de données fixes **db_ddladmin** et **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-specify-a-fill-factor-by-using-table-designer"></a>Pour spécifier un facteur de remplissage à l'aide du Concepteur de tables  
  
1.  Dans l’Explorateur d’objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez spécifier un facteur de remplissage d’index.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez avec le bouton droit sur la table sur laquelle vous souhaitez spécifier un facteur de remplissage d’index et sélectionnez **Conception**.  
  
4.  Dans le menu **Concepteur de tables** , cliquez sur **Index/Clés**.  
  
5.  Sélectionnez l'index avec le facteur de remplissage que vous souhaitez spécifier.  
  
6.  Développez **Spécification du remplissage**, sélectionnez la ligne **Facteur de remplissage** et entrez le facteur de remplissage que vous souhaitez dans la ligne.  
  
7.  Cliquez sur **Fermer**.  
  
8.  Dans le menu **Fichier** , sélectionnez **Enregistrer**_nom_table_.  
  
#### <a name="to-specify-a-fill-factor-in-an-index-by-using-object-explorer"></a>Pour spécifier un facteur de remplissage dans un index à l'aide de l'Explorateur d'objets  
  
1.  Dans l’Explorateur d’objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez spécifier un facteur de remplissage d’index.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table sur laquelle vous souhaitez spécifier un facteur de remplissage d’index.  
  
4.  Cliquez sur le signe plus (+) pour développer le dossier **Index** .  
  
5.  Cliquez avec le bouton droit sur l’index avec le facteur de remplissage que vous souhaitez spécifier et sélectionnez **Propriétés**.  
  
6.  Sous **Sélectionner une page**, sélectionnez **Options**.  
  
7.  Dans la ligne **Facteur de remplissage** , entrez le facteur de remplissage souhaité.  
  
8.  Cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-specify-a-fill-factor-in-an-existing-index"></a>Pour spécifier un taux de remplissage dans un index existant  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple reconstruit un index existant et applique le facteur de remplissage spécifié pendant l'opération de reconstruction.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Rebuilds the IX_Employee_OrganizationLevel_OrganizationNode index   
    -- with a fill factor of 80 on the HumanResources.Employee table.  
  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
    REBUILD WITH (FILLFACTOR = 80);   
    GO  
    ```  
  
#### <a name="another-way-to-specify-a-fill-factor-in-an-index"></a>Autre méthode permettant de spécifier un facteur de remplissage dans un index  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    /* Drops and re-creates the IX_Employee_OrganizationLevel_OrganizationNode index on the HumanResources.Employee table with a fill factor of 80.  
    */  
  
    CREATE INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
       (OrganizationLevel, OrganizationNode)   
    WITH (DROP_EXISTING = ON, FILLFACTOR = 80);   
    GO  
    ```  
  
 Pour plus d’informations, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
  
