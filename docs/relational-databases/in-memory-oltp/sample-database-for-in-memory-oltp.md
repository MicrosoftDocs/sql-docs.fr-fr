---
title: Exemple de base de données pour OLTP en mémoire | Microsoft Docs
description: Découvrez la fonctionnalité OLTP en mémoire et ses avantages en matière de performances. Cet exemple montre des tables à mémoire optimisée et des procédures stockées compilées en mode natif.
ms.custom: ''
ms.date: 12/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: df347f9b-b950-4e3a-85f4-b9f21735eae3
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0d28550dba1a4703bab01324910436b7256597bb
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643841"
---
# <a name="sample-database-for-in-memory-oltp"></a>Exemple de base de données pour OLTP en mémoire
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
    
## <a name="overview"></a>Vue d’ensemble  
 Cet exemple présente la fonctionnalité OLTP en mémoire. Il présente les tables à mémoire optimisée et les procédures stockées compilées nativement. De plus, il permet d’illustrer les avantages relatifs aux performances d’OLTP en mémoire.  
  
> [!NOTE]  
>  Pour afficher cette rubrique pour [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], consultez [Extensions à AdventureWorks pour présenter l’OLTP en mémoire](./overview-and-usage-scenarios.md#in-memory-oltp-overview).  
  
 L’exemple permet de migrer cinq tables de la base de données AdventureWorks vers des tables à mémoire optimisée. De plus, il comporte une charge de travail de démonstration pour le traitement des commandes client. Utilisez cette charge de travail de démonstration pour voir le gain de performances obtenu en utilisant l’OLTP en mémoire sur votre serveur.  
  
 Dans la description de l’exemple, nous abordons les compromis effectués durant la migration des tables vers l’OLTP en mémoire afin de prendre en compte les fonctionnalités qui ne sont pas (encore) prises en charge pour les tables à mémoire optimisée.  
  
 La documentation de l'exemple est structurée comme suit :  
  
-   [Configuration requise](#Prerequisites) pour installer l'exemple et exécuter la charge de travail de démonstration  
  
-   Instructions pour [Installation de l’exemple In-Memory OLTP basé sur AdventureWorks](#InstallingtheIn-MemoryOLTPsamplebasedonAdventureWorks)  
  
-   [Description des exemples de tables et de procédures](#Descriptionofthesampletablesandprocedures) : inclut les descriptions des tables et des procédures ajoutées à AdventureWorks par l’exemple d’OLTP en mémoire ainsi que les considérations relatives à la migration de certaines tables AdventureWorks d’origine vers des tables à mémoire optimisée  
  
-   Instructions pour effectuer des [mesures du niveau de performance à l’aide de la charge de travail de démonstration](#PerformanceMeasurementsusingtheDemoWorkload) : inclut des instructions pour installer et exécuter ostress (un outil de pilotage de la charge de travail) ainsi que pour exécuter la charge de travail de démonstration elle-même  
  
-   [Utilisation de la mémoire et de l’espace disque dans l’exemple](#MemoryandDiskSpaceUtilizationintheSample)  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]  
  
-   Pour tester les performances, un serveur avec des caractéristiques semblables dans votre environnement de production. Pour cet exemple en particulier, vous devez disposer d’au moins 16 Go de mémoire pour SQL Server. Pour obtenir des conseils généraux sur le matériel pour l’OLTP en mémoire, consultez le billet de blog suivant : [Considérations matérielles pour l’OLTP en mémoire dans SQL Server 2014](blog-hardware-in-memory-oltp.md)

##  <a name="installing-the-in-memory-oltp-sample-based-on-adventureworks"></a><a name="InstallingtheIn-MemoryOLTPsamplebasedonAdventureWorks"></a> Installation de l’exemple In-Memory OLTP basé sur AdventureWorks  
 Procédez comme suit pour installer l'exemple :  
  
1.  Téléchargez AdventureWorks2016CTP3.bak et SQLServer2016CTP3Samples.zip à partir de [https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks) et enregistrez ces fichiers dans un dossier local, par exemple « c:\temp ».  
  
2.  Restaurez la sauvegarde de la base de données à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)] ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]:  
  
    1.  Identifiez le dossier cible et le nom du fichier de données, par exemple :  
  
         « h:\DATA\AdventureWorks2016CTP3_Data.mdf »  
  
    2.  Identifiez le dossier cible et le nom du fichier journal, par exemple :  
  
         « i:\DATA\AdventureWorks2016CTP3_log.ldf »  
  
        1.  Le fichier journal doit être placé sur un lecteur différent du fichier de données, idéalement un lecteur à faible latence tel qu'un stockage sur disque SSD ou PCIe, pour des performances maximales.  
  
     Exemple de script T-SQL :  
  
    ```sql
    RESTORE DATABASE [AdventureWorks2016CTP3]   
      FROM DISK = N'C:\temp\AdventureWorks2016CTP3.bak'   
        WITH FILE = 1,    
      MOVE N'AdventureWorks2016_Data' TO N'h:\DATA\AdventureWorks2016CTP3_Data.mdf',    
      MOVE N'AdventureWorks2016_Log' TO N'i:\DATA\AdventureWorks2016CTP3_log.ldf',  
      MOVE N'AdventureWorks2016CTP3_mod' TO N'h:\data\AdventureWorks2016CTP3_mod'  
     GO  
    ```  
  
3.  Pour afficher les exemples de scripts et la charge de travail, décompressez le fichier SQLServer2016CTP3Samples.zip dans un dossier local. Consultez le fichier In-Memory OLTP\readme.txt pour obtenir des instructions sur l’exécution de la charge de travail.  
  
##  <a name="description-of-the-sample-tables-and-procedures"></a><a name="Descriptionofthesampletablesandprocedures"></a> Description des exemples de tables et de procédures  
 L'exemple crée de nouvelles tables pour les produits et les commandes, selon les tables existantes dans AdventureWorks. Le schéma des nouvelles tables est similaire à celui des tables existantes, avec quelques différences, comme expliqué ci-dessous.  
  
 Les nouvelles tables à mémoire optimisée ont le suffixe « _inmem ». L’exemple inclut également les tables correspondantes avec le suffixe « _ondisk » : ces tables peuvent être utilisées pour effectuer une comparaison un-à-un entre les performances des tables à mémoire optimisée et des tables sur disque de votre système.  
  
 Les tables à mémoire optimisée utilisées dans la charge de travail pour la comparaison du niveau de performance sont entièrement durables et entièrement journalisées. Elles ne sacrifient pas la durabilité ou la fiabilité pour atteindre les gains de performance.  
  
 La charge de travail cible pour cet exemple est le traitement des commandes, comprenant également des informations sur les produits et les remises. À cet effet, il utilise les tables SalesOrderHeader, SalesOrderDetail, Product, SpecialOffer, et SpecialOfferProduct.  
  
 Deux nouvelles procédures stockées, Sales.usp_InsertSalesOrder_inmem et Sales.usp_UpdateSalesOrderShipInfo_inmem, sont utilisées pour insérer les commandes et pour mettre à jour les informations d'expédition d'une commande client spécifique.  
  
 Le nouveau schéma « Demo » contient les tables d'assistance et les procédures stockées pour effectuer une charge de travail de démonstration.  
  
 Concrètement, l’exemple d’OLTP en mémoire ajoute les objets suivants dans AdventureWorks :  
  
### <a name="tables-added-by-the-sample"></a>Tables ajoutées par l'exemple  
  
#### <a name="the-new-tables"></a>Nouvelles tables  
 Sales.SalesOrderHeader_inmem  
  
-   Informations sur les en-têtes des commandes. Chaque commande possède une ligne dans cette table.  
  
 Sales.SalesOrderDetail_inmem  
  
-   Détails des commandes. À chaque article d'une commande correspond une ligne dans cette table.  
  
 Sales.SpecialOffer_inmem  
  
-   Informations sur les offres spéciales, y compris le pourcentage de remise associé à chaque offre spéciale.  
  
 Sales.SpecialOfferProduct_inmem  
  
-   Table de référence qui relie les offres spéciales et les produits. Chaque offre spéciale peut contenir zéro ou plusieurs produits, et chaque produit peut être associé à zéro ou plusieurs offres spéciales.  
  
 Production.Product_inmem  
  
-   Informations sur les produits, notamment leur prix catalogue.  
  
 Demo.DemoSalesOrderDetailSeed  
  
-   Utilisé dans la charge de travail de démonstration pour construire des exemples de commandes.  
  
 Variations sur disque des tables :  
  
-   Sales.SalesOrderHeader_ondisk  
  
-   Sales.SalesOrderDetail_ondisk  
  
-   Sales.SpecialOffer_ondisk  
  
-   Sales.SpecialOfferProduct_ondisk  
  
-   Production.Product_ondisk  
  
#### <a name="differences-between-original-disk-based-and-the-and-new-memory-optimized-tables"></a>Différences entre les tables sur disque d'origine et les nouvelles tables optimisées en mémoire  
 Pour la plupart, les nouvelles tables de cet exemple utilisent les mêmes colonnes et les mêmes types de données que les tables d'origine. Toutefois, il existe quelques différences. Nous les avons répertoriées ci-dessous, avec le raisonnement sous-jacent au changement.  
  
 Sales.SalesOrderHeader_inmem  
  
-   Les *contraintes par défaut* sont prises en charge pour les tables optimisées en mémoire, et la plupart des contraintes par défaut ont été migrées en l’état. Toutefois, la table d'origine Sales.SalesOrderHeader contient plusieurs contraintes par défaut qui récupèrent la date actuelle, pour les colonnes OrderDate et ModifiedDate. Dans une charge de travail de traitement des commandes à haut débit avec de nombreuses concurrences, toute ressource globale peut devenir un point de contention. L’heure système est l’une de ces ressources globales, et nous avons observé qu’elle peut devenir un goulot d’étranglement quand une charge de travail d’OLTP en mémoire qui insère des commandes client est exécutée, en particulier si l’heure système doit être extraite pour plusieurs colonnes dans l’en-tête de la commande, ainsi que pour ses détails. Le problème est résolu dans cet exemple en récupérant l'heure système une seule fois pour chaque commande client insérée, puis en utilisant cette valeur pour les colonnes datetime dans SalesOrderHeader_inmem et SalesOrderDetail_inmem, dans la procédure stockée Sales.usp_InsertSalesOrder_inmem.  
  
-   *UDT (types de données définis par l’utilisateur) alias* : la table d’origine utilise deux UDT alias, dbo.OrderNumber et dbo.AccountNumber, pour les colonnes PurchaseOrderNumber et AccountNumber, respectivement. [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] ne prend pas en charge le type défini par l’utilisateur alias pour les tables optimisées en mémoire, par conséquent les nouvelles tables utilisent les types de données système nvarchar(25) et nvarchar(15), respectivement.  
  
-   *Colonnes autorisant des valeurs NULL dans l’index* : dans la table d’origine, la colonne SalesPersonID autorise les valeurs NULL, tandis que dans les nouvelles tables, la colonne n’accepte pas les valeurs NULL et a une contrainte par défaut avec la valeur (-1). Cela est dû au fait que les index des tables à mémoire optimisée ne peuvent pas avoir de colonnes Nullable dans la clé d’index ; -1 est une valeur de substitution de la valeur NULL dans ce cas.  
  
-   *Colonnes calculées[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] : les colonnes calculées SalesOrderNumber et TotalDue sont omises, car* ne prend pas en charge les colonnes calculées dans les tables optimisées en mémoire. La nouvelle vue Sales.vSalesOrderHeader_extended_inmem reflète les colonnes SalesOrderNumber et TotalDue. Par conséquent, vous pouvez utiliser cette vue si ces colonnes sont nécessaires.  

    - **S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.  
À partir de [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1, les colonnes calculées sont prises en charge dans les tables optimisées en mémoire et les index.

  
-   Les *contraintes de clé étrangère* sont prises en charge pour les tables optimisées en mémoire dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], mais uniquement si les tables référencées sont également optimisées en mémoire. Les clés étrangères qui référencent des tables également migrées vers des tables optimisées en mémoire sont conservées dans les tables migrées, tandis que les autres clés étrangères sont omises.  En outre, SalesOrderHeader_inmem est une table très sollicitée dans l'exemple de charge de travail, et les contraintes de clé étrangère nécessitent un traitement supplémentaire pour toutes les opérations DML, avec des recherches dans les autres tables référencées dans ces contraintes. Par conséquent, on formule l’hypothèse que l’application garantit l’intégrité référentielle pour la table Sales.SalesOrderHeader_inmem, et celle-ci n’est pas validée quand des lignes sont insérées.  
  
-   *Rowguid* : la colonne ROWGUID est omise. Alors que uniqueidentifier est pris en charge pour les tables optimisées en mémoire, l'option ROWGUIDCOL n'est pas prise en charge dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]. Les colonnes de ce type sont généralement utilisées pour la réplication de fusion ou pour des tables qui possèdent des colonnes FILESTREAM. Cet exemple ne comporte aucun de ces éléments.  
  
 Sales.SalesOrderDetail  
  
-   *Contraintes par défaut* : similairement à SalesOrderHeader, la contrainte par défaut qui exige la date/l’heure système n’est pas migrée. En revanche, la procédure stockée d’insertion des commandes prend soin d’insérer la date et l’heure système actuelles à la première insertion.  
  
-   *Colonnes calculées* : la colonne calculée LineTotal n’a pas été migrée, car les colonnes calculées ne sont pas prises en charge par les tables à mémoire optimisée dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]. Pour accéder à cette colonne, utilisez la vue Sales.vSalesOrderDetail_extended_inmem.  
  
-   *Rowguid* : la colonne ROWGUID est omise. Pour plus d'informations consultez la description de la table SalesOrderHeader.  
  
 Production.Product  
  
-   *Types définis par l’utilisateur (UDT) alias* : la table d’origine utilise le type de données défini par l’utilisateur dbo.Flag, qui est équivalent au bit de type de données système. La table migrée utilise le type de données bit à la place.  
  
-   *Rowguid* : la colonne ROWGUID est omise. Pour plus d'informations consultez la description de la table SalesOrderHeader.  
  
 Sales.SpecialOffer  
  
-   *Rowguid* : la colonne ROWGUID est omise. Pour plus d'informations consultez la description de la table SalesOrderHeader.  
  
 Sales.SpecialOfferProduct  
  
-   *Rowguid* : la colonne ROWGUID est omise. Pour plus d'informations consultez la description de la table SalesOrderHeader.  
  
#### <a name="considerations-for-indexes-on-memory-optimized-tables"></a>Observations sur les index des tables optimisées en mémoire  
 L'index de base des tables optimisées en mémoire est l'index non cluster, qui prend en charge les recherches de point (recherche d'index dans le prédicat d'égalité), les analyses de plage (recherche d'index dans l'attribut d'inégalité), les analyses d'index complet, et les analyses triées. En outre, les index non cluster prennent en charge la recherche dans les colonnes de début de la clé d'index. En fait, les index non cluster optimisés en mémoire autorisent toutes les opérations prises en charge par les index non cluster sur disque, à la seule exception des analyses ascendantes. Par conséquent, l'utilisation des index non cluster est un choix sûr pour les index.  
  
 Les index HASH peuvent être utilisés pour optimiser davantage la charge de travail. Ils sont particulièrement optimisés pour les recherches de point et les insertions de ligne. Toutefois, il faut considérer qu'ils ne prennent pas en charge les analyses de plage, les analyses triées, ou la recherche sur les colonnes clés d'index. Par conséquent, leur utilisation est plus délicate. En outre, il est nécessaire de spécifier le bucket_count lors de la création. Celui-ci doit généralement correspondre à une valeur comprise entre le nombre de valeurs de clé d'index et son double, mais il peut généralement être surestimé.  
  
Pour plus d’informations, consultez les [instructions d’index](/sql/relational-databases/indexes/guidelines-for-online-index-operations) et les recommandations sur le [choix du bucket_count approprié](https://sqlserver-help.com/tag/bucket_count/) dans la documentation en ligne.  

La documentation en ligne fournit des informations supplémentaires sur les sujets suivants :
- [Recommandations relatives à l’index](/sql/relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables) <!-- On MSDN-TechNet was version sql.120 (2014), library/dn133166 -->

 Les index des tables migrées ont été paramétrés pour la charge de travail de traitement des commandes de démonstration. La charge de travail repose sur les insertions et les recherches de point dans les tables Sales.SalesOrderHeader_inmem et Sales.SalesOrderDetail_inmem, elle porte également sur les recherches de point sur les colonnes clés primaires dans les tables Production.Product_inmem et Sales.SpecialOffer_inmem.  
  
 Sales.SalesOrderHeader_inmem a trois index, qui sont tous des index HASH pour des raisons de performances, et aucune recherche triée ou de plage n'est nécessaire pour la charge de travail.  
  
-   Index HASH sur (SalesOrderID) : le bucket_count est dimensionné à 10 millions (arrondi à 16 millions), car le nombre estimé de commandes est 10 millions.  
  
-   Index HASH sur (SalesPersonID) : le bucket_count est 1 million. Le jeu de données fourni ne comporte pas beaucoup de commerciaux. Toutefois, ce grand bucket_count permet une augmentation future. De plus, vous n’êtes pas affecté par une dégradation du niveau de performance pour les recherches de points si le bucket_count est surdimensionné.  
  
-   Index HASH sur (CustomerID) : le bucket_count est 1 million. Le jeu de données fourni ne contient pas beaucoup de clients, mais il permet une croissance future.  
  
 Sales.SalesOrderDetail_inmem a trois index, qui sont tous des index HASH pour des raisons de performances, et aucune recherche triée ou de plage n'est nécessaire pour la charge de travail.  
  
-   Index HASH sur (SalesOrderID, SalesOrderDetailID) : il s'agit de l'index de clé primaire, et bien que les recherches sur (SalesOrderID, SalesOrderDetailID) soient rares, l'utilisation d'un index de hachage pour la clé accélère les insertions de lignes. Le bucket_count est dimensionné à 50 millions (arrondi à 67 millions) : le nombre attendu de commandes client est de 10 millions, ce qui correspond à une moyenne de cinq articles par commande  
  
-   Index HASH sur (SalesOrderID) : les recherches par commande sont fréquentes : vous souhaitez rechercher tous les articles correspondant à une commande unique.  Le Index bucket_count est dimensionné à 10 millions (arrondi à 16 millions), car le nombre estimé de commandes est 10 millions.  
  
-   Index HASH sur (ProductID) : le bucket_count est 1 million. Le jeu de données fourni ne contient pas beaucoup de produits, mais il permet une croissance future.  
  
 Production.Product_inmem a trois index.  
  
-   Index HASH sur (ProductID) : les recherches sur ProductID sont critiques pour la charge de travail de démonstration, c'est pourquoi il s'agit d'un index de hachage.  
  
-   Index non cluster sur (Name) : il permet les analyses triées par noms de produit.  
  
-   Index non cluster sur (ProductNumber) : il permet les analyses triées par numéros de produit.  
  
 Sales.SpecialOffer_inmem a un index HASH sur (SpecialOfferID) : les recherches de point pour des offres spéciales sont critiques pour la charge de travail de démonstration. Le bucket_count est dimensionné à 1 million pour permettre la croissance future.  
  
 Sales.SpecialOfferProduct_inmem n’est pas référencé dans la charge de travail de démonstration, et il n’est donc pas nécessaire d’utiliser les index de hachage de cette table pour optimiser la charge de travail ; les index sur (SpecialOfferID, ProductID) et (ProductID) sont NONCLUSTERED.  
  
 Notez que ci-dessus, certains bucket_counts sont surdimensionnés, mais pas les bucket_counts des index sur SalesOrderHeader_inmem et SalesOrderDetail_inmem qui sont dimensionnés uniquement à 10 millions de commandes. Cela a pour but de permettre l'installation de l'exemple sur des systèmes avec une faible disponibilité de mémoire ; cependant dans ces cas, la charge de travail de démonstration échoue pour conditions de mémoire insuffisante. Si vous voulez dimensionner au-delà de 10 millions de commandes, augmentez le nombre de compartiments en conséquence.  
  
#### <a name="considerations-for-memory-utilization"></a>Observations sur l'utilisation de la mémoire  
 L'utilisation de la mémoire dans la base de données d'exemple, avant et après l'exécution de la charge de travail de démonstration, est décrite dans la section [Utilisation de la mémoire pour les tables optimisées en mémoire](#Memoryutilizationforthememory-optimizedtables).  
  
### <a name="stored-procedures-added-by-the-sample"></a>Procédures stockées ajoutées par l'exemple  
 Les deux procédures stockées clés d'insertion des commandes et de mise à jour des informations d'expédition sont les suivantes :  
  
-   Sales.usp_InsertSalesOrder_inmem  
  
    -   Insère une nouvelle commande dans la base de données et génère le SalesOrderID pour cette commande. Comme paramètres d'entrée, elle récupère les détails de l'en-tête de la commande client, ainsi que les articles de la commande.  
  
    -   Paramètre de sortie :  
  
        -   @SalesOrderID int : SalesOrderID de la commande qui vient d’être insérée  
  
    -   Paramètres d'entrée (obligatoires) :  
  
        -   @DueDate datetime2  
  
        -   @CustomerID int  
  
        -   @BillToAddressID [int]  
  
        -   @ShipToAddressID [int]  
  
        -   @ShipMethodID [int]  
  
        -   @SalesOrderDetails Sales.SalesOrderDetailType_inmem : paramètre table (TVP) qui contient les articles de la commande  
  
    -   Paramètres d'entrée (facultatifs) :  
  
        -   @Status [tinyint]  
  
        -   @OnlineOrderFlag [bit]  
  
        -   @PurchaseOrderNumber [nvarchar](25\)  
  
        -   @AccountNumber [nvarchar](15\)  
  
        -   @SalesPersonID [int]  
  
        -   @TerritoryID [int]  
  
        -   @CreditCardID [int]  
  
        -   @CreditCardApprovalCode [varchar](15\)  
  
        -   @CurrencyRateID [int]  
  
        -   @Comment nvarchar(128)  
  
-   Sales.usp_UpdateSalesOrderShipInfo_inmem  
  
    -   Met à jour les informations d'expédition d'une commande client spécifique. Met également à jour les informations d'expédition de tous les articles de la commande.  
  
    -   Il s'agit d'une procédure wrapper pour les procédures stockées compilées en mode natif Sales.usp_UpdateSalesOrderShipInfo_native avec la logique de nouvelle tentative permettant de traiter les conflits potentiels (inattendus) avec des transactions simultanées qui mettent à jour la même commande. Pour plus d'informations sur la logique de nouvelle tentative, consultez la rubrique de la documentation en ligne [ici](./transactions-with-memory-optimized-tables.md).  
  
-   Sales.usp_UpdateSalesOrderShipInfo_native  
  
    -   Il s'agit de la procédure stockée compilée en mode natif qui traite effectivement la mise à jour des informations d'expédition. Elle doit être appelée à partir de la procédure stockée wrapper Sales.usp_UpdateSalesOrderShipInfo_inmem. Si le client peut traiter les échecs et implémente la logique de nouvelle tentative, vous pouvez appeler cette procédure directement, au lieu d'utiliser la procédure stockée wrapper.  
  
 La procédure stockée suivante est utilisée pour la charge de travail de démonstration.  
  
-   Demo.usp_DemoReset  
  
    -   Réinitialise la démonstration en vidant et en réamorçant les tables SalesOrderHeader et SalesOrderDetail.  
  
 Les procédures stockées suivantes sont utilisées pour insérer et supprimer des tables optimisées en mémoire tout en garantissant l'intégrité du domaine et l'intégrité référentielle.  
  
-   Production.usp_InsertProduct_inmem  
  
-   Production.usp_DeleteProduct_inmem  
  
-   Sales.usp_InsertSpecialOffer_inmem  
  
-   Sales.usp_DeleteSpecialOffer_inmem  
  
-   Sales.usp_InsertSpecialOfferProduct_inmem  
  
 Enfin, la procédure stockée suivante est utilisée pour vérifier l'intégrité du domaine et l'intégrité référentielle.  
  
1.  dbo.usp_ValidateIntegrity  
  
    -   Paramètre facultatif : @object_id - ID de l’objet dont l’intégrité doit être validée  
  
    -   Cette procédure s’appuie sur les tables dbo.DomainIntegrity, dbo.ReferentialIntegrity et dbo.UniqueIntegrity pour les règles d’intégrité qui doivent être vérifiées. L’exemple remplit ces tables en fonction des contraintes de validation, de clé étrangère et uniques qui existent pour les tables d’origine dans la base de données AdventureWorks.  
  
    -   Elle repose sur les procédures d'assistance dbo.usp_GenerateCKCheck, dbo.usp_GenerateFKCheck, et dbo.GenerateUQCheck pour générer l'instruction T-SQL nécessaire pour effectuer les vérifications d'intégrité.  
  
##  <a name="performance-measurements-using-the-demo-workload"></a><a name="PerformanceMeasurementsusingtheDemoWorkload"></a> Mesures de performance à l'aide de la charge de travail de démonstration  
 Ostress est un outil en ligne de commande qui a été développé par l’équipe de support technique de Microsoft CSS SQL Server. Cet outil peut être utilisé pour exécuter des requêtes ou des procédures stockées distantes en parallèle. Vous pouvez configurer le nombre de threads pour exécuter une instruction T-SQL donnée en parallèle et spécifier combien de fois l'instruction doit être exécutée sur ce thread. Ostress assemblera les threads et exécutera l'instruction sur tous les threads en parallèle. Lorsque l'exécution est terminée pour tous les threads, Ostress indique le temps qu'il a fallu pour terminer l'exécution sur tous les threads.  
  
### <a name="installing-ostress"></a>Installation d'Ostress  
 Ostress est installé avec les utilitaires RML (Report Markup Language). Il n’existe aucune installation autonome pour ostress.  
  
 Étapes d'installation :  
  
1.  Téléchargez et exécutez le package d’installation x64 pour les utilitaires RML à partir de la page suivante : [Télécharger RML pour SQL Server](https://www.microsoft.com/download/details.aspx?id=4511)

2.  Si une boîte de dialogue indique que certains fichiers sont en cours d’utilisation, cliquez sur « Continuer »  
  
### <a name="running-ostress"></a>Exécution d'Ostress  
 Ostress s'exécute à partir de l'invite de ligne de commande. Il est plus pratique d'exécuter l'outil à partir de l'« invite de commandes RML », qui est installé avec les Utilitaires RML.  
  
 Pour ouvrir l'invite de commandes RML exécutez l'instruction suivante :  
  
 Dans Windows Server 2012 R2 et dans Windows 8 et 8.1, ouvrez le menu de démarrage en cliquant sur la touche Windows, et tapez « rml ». Cliquez sur l’« invite de commandes RML », qui apparaît dans la liste de résultats de la recherche.  
  
 Vérifiez que l'invite de commandes se trouve dans le dossier d'installation des utilitaires RML.  
  
 Les options de ligne de commande d'Ostress s'affichent en exécutant simplement ostress.exe, sans besoin d'aucune option de ligne de commande. Les options principales à prendre en compte pour exécuter Ostress avec cet exemple sont les suivantes :  
  
-   -S Nom de l’instance de Microsoft SQL Server à laquelle se connecter  
  
-   -E Utiliser l’authentification Windows pour la connexion (valeur par défaut) ; si vous utilisez l’authentification SQL Server, utilisez les options -U et -P pour spécifier le nom d’utilisateur et le mot de passe, respectivement  
  
-   -d Nom de la base de données, pour cet exemple AdventureWorks2014  
  
-   -Q Instruction T-SQL à exécuter  
  
-   -n Nombre de connexions qui traitent chaque fichier d'entrée/requête  
  
-   -r Nombre d'itérations pour chaque connexion qui exécute chaque fichier d'entrée/requête  
  
### <a name="demo-workload"></a>Charge de travail de démonstration  
 La procédure stockée principale utilisée dans la charge de travail de démonstration est Sales.usp_InsertSalesOrder_inmem/ondisk. Le script ci-dessous construit un paramètre table (TVP) avec des exemples de données, puis appelle la procédure pour insérer une commande client avec cinq articles.  
  
 L'outil Ostress permet d'exécuter des appels de procédure stockée en parallèle, pour simuler des clients insérant des commandes simultanément.  
  
 Réinitialisez la démonstration après chaque exécution contrainte exécutant Demo.usp_DemoReset. Cette procédure supprime les lignes des tables optimisées en mémoire, tronque les tables sur disque, et exécute un point de contrôle de base de données.  
  
 Le script suivant est exécuté simultanément pour simuler une charge de travail de traitement des commandes :  
  
```sql
DECLARE   
      @i int = 0,   
      @od Sales.SalesOrderDetailType_inmem,   
      @SalesOrderID int,   
      @DueDate datetime2 = sysdatetime(),   
      @CustomerID int = rand() * 8000,   
      @BillToAddressID int = rand() * 10000,   
      @ShipToAddressID int = rand() * 10000,   
      @ShipMethodID int = (rand() * 5) + 1;   
  
INSERT INTO @od   
SELECT OrderQty, ProductID, SpecialOfferID   
FROM Demo.DemoSalesOrderDetailSeed   
WHERE OrderID= cast((rand()*106) + 1 as int);   
  
WHILE (@i < 20)   
BEGIN;   
      EXEC Sales.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od;   
      SET @i += 1   
END
```  
  
 Avec ce script, chaque exemple de commande construite est inséré 20 fois, via 20 procédures stockées exécutées dans une boucle WHILE. La boucle est utilisée pour tenir compte du fait que la base de données est utilisée pour construire l'exemple de commande. Dans des environnements de production standard, l'application de niveau intermédiaire construira la commande à insérer.  
  
 Le script ci-dessus insère des commandes dans les tables optimisées en mémoire. Le script pour insérer des commandes dans les tables sur disque est dérivé en remplaçant les deux occurrences de « _inmem » par « _ondisk ».  
  
 Nous utiliserons l'outil Ostress pour exécuter des scripts utilisant plusieurs connexions simultanées. Nous utiliserons le paramètre « -n » pour contrôler le nombre de connexions, et le paramètre « r » pour contrôler le nombre de fois où le script est exécuté sur chaque connexion.  
  
#### <a name="running-the-workload"></a>Exécution de la charge de travail  
 Pour tester à l'échelle, nous insérons 10 millions de commandes, à l'aide de 100 connexions. Ce test peut être exécuté aisément sur un serveur de taille moyenne (par exemple, 8 noyaux physiques, et 16 noyaux logiques), et un stockage SSD de base pour le journal. Si le test ne fonctionne pas correctement sur votre matériel, consultez la section [Dépannage des tests lents](#Troubleshootingslow-runningtests). Si vous voulez réduire le niveau d’extraction pour le test, réduisez le nombre de connexions en modifiant le paramètre « -n ». Par exemple, pour réduire le nombre de connexions à 40, remplacez le paramètre « -n100 » par « -n40 ».  
  
 Comme mesure de performances pour la charge de travail, nous utilisons le temps écoulé tel qu'indiqué par ostress.exe après avoir exécuté la charge de travail.  
  
 Les instructions et les mesures ci-dessous utilisent une charge de travail qui insère 10 millions de commandes. Pour obtenir des instructions sur l’exécution d’une charge de travail réduite en vue d’insérer 1 million de commandes, consultez les instructions figurant dans le fichier « In-Memory OLTP\readme.txt » qui fait partie de l’archive SQLServer2016CTP3Samples.zip.  
  
##### <a name="memory-optimized-tables"></a>Tables optimisées en mémoire  
 Nous allons commencer par exécuter la charge de travail sur les tables optimisées en mémoire. La commande suivante ouvre 100 threads, chacun exécuté pour 5 000 itérations.  Chaque itération insère 20 commandes dans des transactions séparées. Il y a 20 insertions par itération pour compenser le fait que la base de données est utilisée pour générer les données à insérer. Cela donne un total de 20 \* 5 000 \* 100 = 10 000 000 commandes insérées.  
  
 Ouvrez l'invite de commandes RML et exécutez la commande suivante :  
  
 Cliquez sur le bouton correspondant pour copier la commande, et collez-la dans l'invite de commandes des utilitaires RML.  
  
```console
ostress.exe -n100 -r5000 -S. -E -dAdventureWorks2016CTP3 -q -Q"DECLARE @i int = 0, @od Sales.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand() * 10000, @ShipMethodID int = (rand() * 5) + 1; INSERT INTO @od SELECT OrderQty, ProductID, SpecialOfferID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*106) + 1 as int); while (@i < 20) begin; EXEC Sales.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od; set @i += 1 end"  
```  
  
 Sur un serveur de test avec un nombre total de 8 noyaux physiques (16 logiques), ceci a nécessité 2 minutes et 5 secondes. Sur un second serveur de test avec 24 noyaux physiques (48 logiques), ceci a nécessité 1 minute et 0 secondes.  
  
 Observez l'utilisation de l'UC pendant que la charge de travail est exécutée, par exemple via le Gestionnaire des tâches. Vous constaterez que l'utilisation de l'UC est proche de 100 %. Dans le cas contraire, vous avez un goulot d'étranglement d'E/S du journal. Consultez [Dépannage des tests lents](#Troubleshootingslow-runningtests).  
  
##### <a name="disk-based-tables"></a>Tables sur disque  
 La commande suivante exécute la charge de travail sur les tables sur disque. Cette charge de travail peut prendre un certain temps pour s’exécuter, principalement en raison d’une contention de verrous dans le système. Les tables optimisées en mémoire n'ont pas de verrous et ne sont pas concernées par ce problème.  
  
 Ouvrez l'invite de commandes RML et exécutez la commande suivante :  
  
 Cliquez sur le bouton correspondant pour copier la commande, et collez-la dans l'invite de commandes des utilitaires RML.  
  
```console
ostress.exe -n100 -r5000 -S. -E -dAdventureWorks2016CTP3 -q -Q"DECLARE @i int = 0, @od Sales.SalesOrderDetailType_ondisk, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand() * 10000, @ShipMethodID int = (rand() * 5) + 1; INSERT INTO @od SELECT OrderQty, ProductID, SpecialOfferID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*106) + 1 as int); while (@i < 20) begin; EXEC Sales.usp_InsertSalesOrder_ondisk @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od; set @i += 1 end"  
```  
  
 Sur un serveur de test avec un nombre total de 8 noyaux physiques (16 logiques), ceci a nécessité 41 minutes et 25 secondes. Sur un second serveur de test avec 24 noyaux physiques (48 logiques), ceci a nécessité 52 minutes et 16 secondes.  
  
 La raison principale de la différence de performances entre les tables optimisées en mémoire et les tables sur disque pendant ce test, est que quand vous utilisez des tables sur disque, SQL Server n’utilise pas entièrement l’UC. Cela est dû à une contention de verrou : les transactions simultanées tentent d'écrire dans la même page de données ; les verrous sont utilisés pour garantir qu'une seule transaction à la fois écrit sur une page. Le moteur d’OLTP en mémoire n’a pas de verrous, et les lignes de données ne sont pas organisées en pages. Ainsi, les transactions simultanées ne bloquent pas les insertions réciproques, ce qui permet à SQL Server d’utiliser pleinement le processeur.  
  
 Observez l'utilisation de l'UC pendant que la charge de travail est exécutée, par exemple via le Gestionnaire des tâches. Vous verrez qu'avec les tables sur disque, l'utilisation de l'UC est loin d'être de 100 %. Dans une configuration de test avec 16 processeurs logiques, l'utilisation serait d'environ 24 %.  
  
 Éventuellement, vous pouvez afficher le nombre d’attentes de verrous par seconde à l’aide de l’analyseur de performances, avec le compteur de performance « \SQL Server:Latches\Latch Waits/sec ».  
  
#### <a name="resetting-the-demo"></a>Réinitialisation de la démonstration  
 Pour réinitialiser la démonstration, ouvrez l'invite de commandes RML et exécutez la commande suivante :  
  
```console
ostress.exe -S. -E -dAdventureWorks2016CTP3 -Q"EXEC Demo.usp_DemoReset"  
```  
  
 Selon le matériel, l’exécution peut prendre quelques minutes.  
  
 Nous vous recommandons de réinitialiser après chaque exécution de démonstration. Étant donné que cette charge de travail est réservée à l'insertion, chaque exécution consommera plus de mémoire, c'est pourquoi une réinitialisation est requise afin d'éviter des conditions de mémoire insuffisante. La quantité de mémoire consommée après une exécution est décrite dans la section [Utilisation de la mémoire après avoir exécuté la charge de travail](#Memoryutilizationafterrunningtheworkload).  
  
###  <a name="troubleshooting-slow-running-tests"></a><a name="Troubleshootingslow-runningtests"></a> Dépannage des tests lents  
 Les résultats des tests varient généralement selon le matériel, mais aussi selon le niveau de concurrence utilisé dans l'exécution du test. Voici quelques pistes à explorer, si les résultats ne sont pas tels que prévu :  
  
-   Nombre de transactions simultanées : lors de l’exécution de la charge de travail sur un seul thread, le gain de performances avec l’OLTP en mémoire sera probablement inférieur à 2X. La contention de verrou n'est un problème que si le niveau de concurrence est élevé.  
  
-   Nombre faible de cœurs disponibles pour SQL Server  : cela signifie qu’il y aura un niveau d’accès concurrentiel faible dans le système, car il ne peut pas y avoir plus de transactions simultanées en cours d’exécution que de cœurs disponibles pour SQL.  
  
    -   Symptôme : si l'utilisation de l'UC est élevée lors de l'exécution de la charge de travail sur les tables sur disque, cela signifie qu'il n'y a pas beaucoup de contentions, et donc qu'il n'y a pas de concurrence.  
  
-   Vitesse du lecteur de journal : si le lecteur de journal n’arrive pas à suivre le débit des transactions dans le système, la charge de travail devient un goulot d’étranglement dans les E/S du journal. Bien que la journalisation soit plus efficace avec l’OLTP en mémoire, si le journal des E/S est congestionné, le gain de performance potentiel est limité.  
  
    -   Symptôme : si l'utilisation de l'UC n'est pas proche de 100 % ou varie beaucoup pendant l'exécution de la charge de travail sur les tables optimisées en mémoire, il est possible qu'il existe un goulot d'étranglement du journal des E/S. Cela peut être vérifié en ouvrant le moniteur de ressource et en examinant la longueur de la file d'attente du lecteur de journalisation.  
  
##  <a name="memory-and-disk-space-utilization-in-the-sample"></a><a name="MemoryandDiskSpaceUtilizationintheSample"></a> Utilisation de la mémoire et de l’espace disque dans l’exemple  
 Vous trouverez ci-dessous la description de ce à quoi vous devez vous attendre en termes d'utilisation de la mémoire et de l'espace disque pour l'exemple de base de données. Nous présentons également les résultats obtenus pour un serveur de test avec 16 noyaux logiques.  
  
###  <a name="memory-utilization-for-the-memory-optimized-tables"></a><a name="Memoryutilizationforthememory-optimizedtables"></a> Utilisation de la mémoire pour les tables optimisées en mémoire  
  
#### <a name="overall-utilization-of-the-database"></a>Utilisation générale de la base de données  
 La requête suivante peut être utilisée pour obtenir l’utilisation totale de mémoire pour l’OLTP en mémoire dans le système.  
  
```sql
SELECT type  
   , name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
 Instantané de la base de données juste après sa création :  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Default|94|  
|MEMORYCLERK_XTP|DB_ID_5|877|  
|MEMORYCLERK_XTP|Default|0|  
|MEMORYCLERK_XTP|Default|0|  
  
 Les régisseurs de mémoire par défaut contiennent les structures de mémoire à l'échelle du système et sont relativement petits. Le régisseur de mémoire de la base de données utilisateur, dans le cas présent la base de données ayant l’ID 5, a une taille d’environ 900 Mo.  
  
#### <a name="memory-utilization-per-table"></a>Utilisation de la mémoire par table  
 La requête suivante peut être utilisée pour explorer l'utilisation de la mémoire des différentes tables et de leurs index :  
  
```sql
SELECT object_name(t.object_id) AS [Table Name]  
     , memory_allocated_for_table_kb  
 , memory_allocated_for_indexes_kb  
FROM sys.dm_db_xtp_table_memory_stats dms JOIN sys.tables t   
ON dms.object_id=t.object_id  
WHERE t.type='U'  
```  
  
 Le tableau suivant affiche les résultats de cette requête pour une nouvelle installation de l’exemple :  
  
| Nom de la table | memory_allocated_for_table_kb | memory_allocated_for_indexes_kb |
| ---------- | ----------------------------- | ------------------------------- |
|SpecialOfferProduct_inmem|64|3840|  
|DemoSalesOrderHeaderSeed|1984|5504|  
|SalesOrderDetail_inmem|15316|663552|  
|DemoSalesOrderDetailSeed|64|10432|  
|SpecialOffer_inmem|3|8 192|  
|SalesOrderHeader_inmem|7168|147456|  
|Product_inmem|124|12352|  

 Comme vous pouvez le constater, les tables sont assez petites : SalesOrderHeader_inmem a une taille d’environ 7 Mo et SalesOrderDetail_inmem a une taille d’environ 15 Mo.  
  
 Ce qui est frappant ici est la taille de la mémoire allouée aux index, par rapport à la taille des données de table. Cela est dû au fait que les index de hachage de l'exemple sont prédimensionnés pour contenir plus de données. Notez que les index de hachage ont une taille fixe, par conséquent, leur taille n'augmente pas selon la taille des données de la table.  
  
####  <a name="memory-utilization-after-running-the-workload"></a><a name="Memoryutilizationafterrunningtheworkload"></a> Utilisation de la mémoire après avoir exécuté la charge de travail  
 Après l'insertion de 10 millions de commandes, l'utilisation globale de la mémoire devrait s'apparenter à ce qui suit :  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Default|146|  
|MEMORYCLERK_XTP|DB_ID_5|7374|  
|MEMORYCLERK_XTP|Default|0|  
|MEMORYCLERK_XTP|Default|0|  

 Comme vous pouvez le voir, SQL Server utilise un peu moins de 8 Go pour les tables à mémoire optimisée et les index dans l’exemple de base de données.  
  
 Voici l'utilisation de la mémoire détaillée par table après l'exécution d'un exemple :  
  
```sql
SELECT object_name(t.object_id) AS [Table Name]  
     , memory_allocated_for_table_kb  
 , memory_allocated_for_indexes_kb  
FROM sys.dm_db_xtp_table_memory_stats dms JOIN sys.tables t   
ON dms.object_id=t.object_id  
WHERE t.type='U'  
```  
  
| Nom de la table | memory_allocated_for_table_kb | memory_allocated_for_indexes_kb |
| ---------- | ----------------------------- | ------------------------------- |
|SalesOrderDetail_inmem|5113761|663552|  
|DemoSalesOrderDetailSeed|64|10368|  
|SpecialOffer_inmem|2|8 192|  
|SalesOrderHeader_inmem|1575679|147456|  
|Product_inmem|111|12032|  
|SpecialOfferProduct_inmem|64|3712|  
|DemoSalesOrderHeaderSeed|1984|5504|  

 Nous pouvons voir un total d’environ 6,5 Go de données. Notez que la taille des index sur la table SalesOrderHeader_inmem et SalesOrderDetail_inmem est la même que la taille des index avant d'insérer les commandes. La taille de l'index n'a pas changé car les deux tables utilisent des index de hachage, qui sont statiques.  
  
#### <a name="after-demo-reset"></a>Après la réinitialisation de la démonstration  
 La procédure stockée Demo.usp_DemoReset peut être utilisée pour réinitialiser la démonstration. Elle supprime les données dans les tables SalesOrderHeader_inmem et SalesOrderDetail_inmem, puis réinsère les données à partir des tables d'origine SalesOrderHeader et SalesOrderDetail.  
  
 Cependant, même si les lignes des tables ont été supprimées, cela ne signifie pas pour autant que la mémoire est immédiatement récupérée. SQL Server récupère la mémoire des lignes supprimées dans les tables optimisées en mémoire en arrière-plan, si nécessaire. Vous verrez qu'immédiatement après la réinitialisation de la démonstration, sans charge de travail transactionnelle sur le système, la mémoire des lignes supprimées n'est pas encore récupérée :  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Default|2261|  
|MEMORYCLERK_XTP|DB_ID_5|7396|  
|MEMORYCLERK_XTP|Default|0|  
|MEMORYCLERK_XTP|Default|0|  

 C'est le comportement attendu : la mémoire est récupérée lorsque la charge de travail transactionnelle s'exécute.  
  
 Si vous démarrez une deuxième exécution de la charge de travail de démonstration, vous verrez que l'utilisation de la mémoire diminue au début, au fur et à mesure que les lignes précédemment supprimées sont nettoyées. À un certain moment, la taille de la mémoire augmentera de nouveau, jusqu'à ce que la charge de travail soit terminée. Une fois que les 10 millions de lignes ont été insérées après la réinitialisation de la démonstration, l'utilisation de la mémoire sera très similaire à l'utilisation après la première exécution. Par exemple :  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Default|1863|  
|MEMORYCLERK_XTP|DB_ID_5|7390|  
|MEMORYCLERK_XTP|Default|0|  
|MEMORYCLERK_XTP|Default|0|  

### <a name="disk-utilization-for-memory-optimized-tables"></a>Utilisation du disque pour les tables optimisées en mémoire  
 La taille globale sur disque des fichiers de point de contrôle d'une base de données à un moment donné peut être récupérée à l'aide de la requête :  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
  
```  
  
#### <a name="initial-state"></a>InitialState  
 Quand les exemples de groupe de fichiers et de tables à mémoire optimisée sont initialement créés, un certain nombre de fichiers de point de contrôle sont créés au préalable et le système commence à les remplir. Le nombre de fichiers de point de contrôle créés au préalable dépend du nombre de processeurs logiques dans le système. Étant donné que cet exemple a une taille très petite au début, les fichiers créés au préalable seront vides après la création initiale.  
  
 Le code suivant montre la taille initiale sur disque de l’exemple sur une machine dotée de 16 processeurs logiques :  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| Taille sur disque en Mo |
| ------------------ |
|2312|  

 Comme vous pouvez le voir, il existe une grande différence entre la taille sur disque des fichiers de point de contrôle, qui est de 2,3 Go, et la taille réelle des données, qui est plus proche de 30 Mo.  
  
 Pour analyser de plus près la raison de l'utilisation de l'espace disque, vous pouvez utiliser la requête suivante, La taille du disque retournée par cette requête est approximative pour les fichiers ayant l'état 5 (REQUIRED FOR BACKUP/HA), 6 (IN TRANSITION TO TOMBSTONE) ou 7 (TOMBSTONE).  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
 Pour l'état initial de l'exemple, le résultat sera similaire à ce qui suit pour un serveur avec 16 processeurs logiques :  
  
| state_desc | file_type_desc | count | on-disk size MB |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2 048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|1|128|  
|UNDER CONSTRUCTION|DELTA|1|8|  

 Comme vous pouvez le voir, la majeure partie de l'espace est utilisé par les fichiers de données et delta précréés. SQL Server créé au préalable une paire de fichiers (données, delta) par processeur logique. De plus, les fichiers de données sont prédimensionnés à 128 Mo, et les fichiers delta à 8 Mo, afin d’optimiser l’insertion des données dans ces fichiers.  
  
 Les données réelles dans les tables optimisées en mémoire se trouvent dans un seul fichier de données.  
  
#### <a name="after-running-the-workload"></a>Après l'exécution de la charge de travail  
 Après une seule exécution de test qui insère 10 millions de commandes, la taille totale sur disque ressemble à ce qui suit (pour un serveur de test avec 16 noyaux) :  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| Taille sur disque en Mo |
| ------------------ |
|8828|
  
 La taille sur disque est proche de 9 Go, ce qui est proche de la taille en mémoire des données.  
  
 Analysons plus en détail les tailles des fichiers de point de contrôle entre les différents états :  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | on-disk size MB |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2 048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|1|128|  
|UNDER CONSTRUCTION|DELTA|1|8|  

 Nous avons toujours 16 paires de fichiers précréés, prêtes au fur et à mesure que les points de contrôle se ferment.  
  
 Il y a une paire en cours de création, utilisée tant que le point de contrôle actif n'est pas fermé. Avec les fichiers de point de contrôle actifs, cela donne environ 6,5 Go d’utilisation du disque pour 6,5 Go de données en mémoire. N'oubliez pas que les index ne sont pas conservés sur le disque, donc, dans ce cas, la taille globale sur le disque est plus petite que la taille de la mémoire.  
  
#### <a name="after-demo-reset"></a>Après la réinitialisation de la démonstration  
 Après la réinitialisation de la démonstration, l'espace disque n'est pas libéré immédiatement s'il n'y a pas de charge de travail transactionnelle sur le système, et s'il n'y a pas de points de contrôle de base de données. Pour que les fichiers de point de contrôle passent par les différentes étapes et soient inévitablement supprimés, plusieurs points de contrôle et événements de troncation du journal doivent se produire, pour initialiser la fusion des fichiers de point de contrôle, ainsi que pour initialiser le garbage collection. Cela se produit automatiquement si vous avez une charge de travail transactionnelle dans le système (et si vous effectuez des sauvegardes de journaux régulières, dans le cas où vous utilisez le mode de restauration complète), mais pas lorsque le système est inactif, comme dans un scénario de démonstration.  
  
 Dans l'exemple, après la réinitialisation de la démonstration, vous obtiendrez un résultat similaire au suivant.  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| Taille sur disque en Mo |
| ------------------ |
|11839|
  
 À presque 12 Go, cela dépasse nettement les 9 Go que nous avions avant la réinitialisation de la démonstration. Cela est dû au fait que certaines fusions de fichiers de point de contrôle ont commencé, tandis que certaines cibles de fusion n'ont pas encore été installées, et que certains fichiers sources de fusion n'ont pas encore été nettoyés, comme vous pouvez le voir à partir des éléments suivants :  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | on-disk size MB |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2 048|  
|PRECREATED|DELTA|16|128|  
|ACTIVE|DATA|38|5152|  
|ACTIVE|DELTA|38|1331|  
|MERGE TARGET|DATA|7|896|  
|MERGE TARGET|DELTA|7|56|  
|MERGED SOURCE|DATA|13|1772|  
|MERGED SOURCE|DELTA|13|455|  

 Les cibles de fusion sont installées et la source fusionnée est nettoyée au fur et à mesure que l'activité transactionnelle s'exécute dans le système.  
  
 Après une deuxième exécution de la charge de travail de démonstration, et l'insertion de 10 millions de commandes client après la réinitialisation de la démonstration, vous constaterez que les fichiers construits lors de la première exécution de la charge de travail ont été nettoyés. Si vous exécutez la requête ci-dessus plusieurs fois pendant que la charge de travail s'exécute, vous verrez les fichiers de point de contrôle passer à travers les différentes étapes.  
  
 Après la deuxième exécution de la charge de travail et l'insertion de 10 millions de commandes, vous verrez que l'utilisation du disque est très similaire, mais pas nécessairement identique, à celle constatée après la première exécution, car le système est dynamique par nature. Par exemple :  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | on-disk size MB |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2 048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|2|268|  
|UNDER CONSTRUCTION|DELTA|2|16|  
|ACTIVE|DATA|41|5608|  
|ACTIVE|DELTA|41|328|  

 Dans ce cas, il existe deux paires de fichiers de point de contrôle avec l’état « under construction », signifiant que plusieurs paires de fichiers ont été déplacées vers l’état « under construction », probablement en raison du haut niveau de concurrence dans la charge de travail. Plusieurs threads simultanés ont nécessité une nouvelle paire de fichiers en même temps ; par conséquent, une paire est passée de l’état « precreated » à l’état « under construction ».  
  
## <a name="see-also"></a>Voir aussi

[OLTP en mémoire &#40;Optimisation en mémoire&#41;](in-memory-oltp-in-memory-optimization.md)