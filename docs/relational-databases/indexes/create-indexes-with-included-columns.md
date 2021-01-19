---
description: Créer des index avec colonnes incluses
title: Créer des index avec colonnes incluses | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- index size [SQL Server]
- index keys [SQL Server]
- index columns [SQL Server]
- size [SQL Server], indexes
- key columns [SQL Server]
- included columns
- nonclustered indexes [SQL Server], included columns
- designing indexes [SQL Server], included columns
- nonkey columns
ms.assetid: d198648d-fea5-416d-9f30-f9d4aebbf4ec
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d972f88744b6c8b108b1f52a54d4bf8507975017
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171151"
---
# <a name="create-indexes-with-included-columns"></a>Créer des index avec colonnes incluses
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cette rubrique explique comment ajouter des colonnes incluses (ou non-clés) pour étendre les fonctionnalités des index non cluster dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'inclusion de colonnes non-clés permet de créer des index non-cluster qui couvrent davantage de requêtes. En effet, les colonnes non-clés présentent les avantages suivants :  
  
-   Elles peuvent contenir des types de données qui ne sont pas autorisés dans les colonnes de clés d'index.  
-   Elles ne sont pas prises en compte par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] lors du calcul du nombre de colonnes clés d'index ou de la taille de la clé d'index.  
  
 Un index contenant des colonnes non-clés peut améliorer considérablement les performances des requêtes lorsque toutes les colonnes de la requête sont incluses dans l'index en tant que colonnes clés ou non-clés. Les gains de performances sont dus au fait que l'optimiseur de requête peut localiser toutes les valeurs des colonnes dans l'index ; l'accès aux données de table et d'index n'a pas lieu, produisant ainsi un nombre moindre d'opérations d'E/S sur le disque.  
  
> [!NOTE]  
> Quand un index contient toutes les colonnes auxquelles une requête fait référence, on dit qu’il *couvre la requête*.  
   
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="design-recommendations"></a><a name="DesignRecs"></a> Recommandations relatives à la conception  
  
-   Refaites la conception des index non-cluster qui ont une clé d’index de grande taille, de sorte que seules les colonnes utilisées pour la recherche soient des colonnes clés. Toutes les autres colonnes qui couvrent la requête doivent être des colonnes non-clés. De cette manière, vous disposez de toutes les colonnes nécessaires pour couvrir la requête, mais la clé d'index elle-même est petite et efficace.  
  
-   Incluez les colonnes non clés dans un index non cluster pour éviter de dépasser les limitations actuelles de taille d’index, établies à 32 colonnes clés au maximum et à une taille de clé d’index maximale de 1 700 octets (16 colonnes clés et 900 octets avant [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]). Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne tient pas compte des colonnes non-clés lors du calcul du nombre de colonnes clés d'index ou de la taille de la clé d'index.  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Les colonnes non-clés peuvent uniquement être définies sur des index non cluster.  
  
-   Tous les types de données, à l'exception de **text**, de **ntext** et de **image** , peuvent être utilisés en tant que colonnes non-clés.  
  
-   Les colonnes calculées déterministes et précises ou imprécises peuvent être des colonnes non-clés. Pour plus d'informations, consultez [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).  
  
-   Les colonnes calculées dérivées des types de données **image**, **ntext** et **text** peuvent être des colonnes non-clés tant que le type de données de la colonne calculée est autorisé en tant que colonne d'index non-clé.  
  
-   Les colonnes non-clés ne peuvent pas être supprimées d’une table, sauf si l’index de cette table est d’abord supprimé.  
  
-   Les colonnes non-clés ne peuvent pas être modifiées, sauf pour effectuer les opérations suivantes :  
  
    -   modifier la possibilité de valeur NULL de la colonne de NOT NULL à NULL ;  
  
    -   augmenter la longueur des colonnes **varchar**, **nvarchar** ou **varbinary** .  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite une autorisation ALTER sur la table ou la vue. L’utilisateur doit être membre du rôle serveur fixe **sysadmin** ou des rôles de base de données fixes **db_ddladmin** et **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-an-index-with-nonkey-columns"></a>Pour créer un index avec des colonnes non-clés  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez créer un index avec des colonnes non-clés.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table sur laquelle vous souhaitez créer un index avec des colonnes non-clés.  
  
4.  Cliquez avec le bouton droit sur le dossier **Index**, pointez sur **Nouvel index**, puis sélectionnez **Index non cluster...** .  
  
5.  Dans la boîte de dialogue **Nouvel index** , sur la page **Général** , entrez le nom du nouvel index dans la zone **Nom de l'index** .  
  
6.  Sous l’onglet **Colonnes clés d’index**, cliquez sur **Ajouter…** .  
  
7.  Dans la boîte de dialogue **Sélectionner des colonnes dans** _nom\_table_, cochez les cases correspondant aux colonnes de table à ajouter à l’index.  
  
8.  Cliquez sur **OK**.  
  
9. Sous l’onglet **Colonnes incluses**, cliquez sur **Ajouter...** .  
  
10. Dans la boîte de dialogue **Sélectionner des colonnes dans** _nom\_table_, cochez les cases correspondant aux colonnes de table à ajouter à l’index en tant que colonnes non-clés.  
  
11. Cliquez sur **OK**.  
  
12. Dans la boîte de dialogue **Nouvel index** , cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-an-index-with-nonkey-columns"></a>Pour créer un index avec des colonnes non-clés  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    -- Creates a nonclustered index on the Person.Address table with four included (nonkey) columns.   
    -- index key column is PostalCode and the nonkey columns are  
    -- AddressLine1, AddressLine2, City, and StateProvinceID.  
    CREATE NONCLUSTERED INDEX IX_Address_PostalCode  
    ON Person.Address (PostalCode)  
    INCLUDE (AddressLine1, AddressLine2, City, StateProvinceID);  
    GO  
    ```  

## <a name="related-content"></a>Contenu connexe  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)    
[Guide de conception d’index SQL Server](../../relational-databases/sql-server-index-design-guide.md)   
