---
title: Catalogue de bases de données OLAP WideWorldImporters-SQL | Microsoft Docs
description: Comprendre les schémas, les tables et les procédures stockées utilisées pour l’entreposage des données et le traitement analytique dans la base de données WideWorldImportersDW.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 08/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 4481a0c96a8f4a7f2f82e8babc5d8dce5ea97e92
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077810"
---
# <a name="wideworldimportersdw-database-catalog"></a>Catalogue de bases de données WideWorldImportersDW
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
Explications pour les schémas, les tables et les procédures stockées dans la base de données WideWorldImportersDW. 

La base de données WideWorldImportersDW est utilisée pour l’entreposage des données et le traitement analytique. Les données transactionnelles relatives aux ventes et aux achats sont générées dans la base de données WideWorldImporters et chargées dans la base de données WideWorldImportersDW à l’aide d’un **processus ETL quotidien**.

Les données de WideWorldImportersDW reflètent donc les données dans WideWorldImporters, mais les tables sont organisées différemment. Bien que WideWorldImporters ait un schéma normalisé traditionnel, WideWorldImportersDW utilise l’approche de [schéma en étoile](https://wikipedia.org/wiki/Star_schema) pour sa conception de table. Outre les tables de faits et de dimension, la base de données comprend un certain nombre de tables de mise en lots utilisées dans le processus ETL.

## <a name="schemas"></a>Schémas

Les différents types de tables sont organisés en trois schémas.

|schéma|Description|
|-----------------------------|---------------------|
|Dimension|Tables de dimension.|
|Fact| Tables de faits.|  
|Intégration|Tables de mise en lots et autres objets nécessaires à l’ETL.|  

## <a name="tables"></a>Tables

Les tables de dimension et de faits sont répertoriées ci-dessous. Les tables du schéma d’intégration sont uniquement utilisées pour le processus ETL et ne sont pas répertoriées.

### <a name="dimension-tables"></a>Tables de dimension

WideWorldImportersDW possède les tables de dimension suivantes. La description comprend la relation avec les tables sources dans la base de données WideWorldImporters.

|Table de charge de travail|Tables sources|
|-----------------------------|---------------------|
|City|`Application.Cities`, `Application.StateProvinces`, `Application.Countries`.|
|Customer|`Sales.Customers`, `Sales.BuyingGroups`, `Sales.CustomerCategories`.|
|Date|Nouvelle table avec des informations sur les dates, y compris l’année financière (basée sur le 1er novembre de l’année financière).|
|Employé|`Application.People`.|
|StockItem|`Warehouse.StockItems`, `Warehouse.Colors`, `Warehouse.PackageType`.|
|Fournisseur|`Purchasing.Suppliers`, `Purchasing.SupplierCategories`.|
|PaymentMethod|`Application.PaymentMethods`.|
|TransactionType|`Application.TransactionTypes`.|

### <a name="fact-tables"></a>Tables de faits

WideWorldImportersDW contient les tables de faits suivantes. La description comprend la relation avec les tables sources dans la base de données WideWorldImporters, ainsi que les classes de requêtes Analytics/Reporting chaque table de faits est généralement utilisée avec.

|Table de charge de travail|Tables sources|Exemple d’analyse|
|-----------------------------|---------------------|---------------------|
|JSON|`Sales.Orders` et `Sales.OrderLines`|Les vendeurs, le sélecteur/la productivité des packs et le temps de prélèvement des commandes. En outre, les situations de stock faible conduisent à des commandes en différé.|
|Sale|`Sales.Invoices` et `Sales.InvoiceLines`|Les ventes, les dates de livraison, la rentabilité dans le temps, la rentabilité par commercial.|
|Acheter|`Purchasing.PurchaseOrderLines`|Temps de leads comparatifs attendus|
|Transaction|`Sales.CustomerTransactions` et `Purchasing.SupplierTransactions`|Mesure des dates de sortie et des dates de finalisation et des montants.|
|Movement|`Warehouse.StockTransactions`|Mouvements dans le temps.|
|Détention des actions|`Warehouse.StockItemHoldings`|Niveaux et valeurs de stock disponibles.|

## <a name="stored-procedures"></a>Procédures stockées

Les procédures stockées sont principalement utilisées pour le processus ETL et à des fins de configuration.

Toutes les extensions de l’exemple sont encouragées à utiliser le `Reports` schéma pour les rapports Reporting Services et le `PowerBI` schéma pour l’accès à Power bi.

### <a name="application-schema"></a>Schéma d’application

Ces procédures sont utilisées pour configurer l’exemple. Ils sont utilisés pour appliquer des fonctionnalités Enterprise Edition à la version Standard Edition de l’exemple, ajouter Polybase et réamorcer ETL.

|Procédure|Objectif|
|-----------------------------|---------------------|
|Configuration_ApplyPartitionedColumnstoreIndexing|Applique à la fois le partitionnement et les index ColumnStore pour les tables de faits.|
|Configuration_ConfigureForEnterpriseEdition|Applique le partitionnement, l’indexation ColumnStore et la mémoire.|
|Configuration_EnableInMemory|Remplace les tables de mise en lots d’intégration par des tables optimisées en mémoire SCHEMA_ONLY pour améliorer les performances ETL.|
|Configuration_ApplyPolyBase|Configure une source de données externe, un format de fichier et une table.|
|Configuration_PopulateLargeSaleTable|Applique les modifications de l’édition Enterprise, puis remplit une plus grande quantité de données pour l’année civile 2012 en tant qu’historique supplémentaire.|
|Configuration_ReseedETL|Supprime les données existantes et redémarre les graines ETL. Cela permet le remplissage de la base de données OLAP pour qu’elle corresponde aux lignes mises à jour dans la base de données OLTP.|

### <a name="integration-schema"></a>Schéma d’intégration

Les procédures utilisées dans le processus ETL sont classées dans les catégories suivantes :
- Procédures d’assistance pour le package ETL-toutes les procédures obtenir *.
- Procédures utilisées par le package ETL pour la migration des données intermédiaires vers les procédures de migration des tables DW *.
- `PopulateDateDimensionForYear` -Prend un an et s’assure que toutes les dates de cette année sont remplies dans la `Dimension.Date` table.

### <a name="sequences-schema"></a>Schéma de séquences

Procédures pour configurer les séquences dans la base de données.

|Procédure|Objectif|
|-----------------------------|---------------------|
|ReseedAllSequences|Appelle la procédure `ReseedSequenceBeyondTableValue` pour toutes les séquences.|
|ReseedSequenceBeyondTableValue|Utilisé pour repositionner la valeur de séquence suivante au-delà de la valeur dans une table qui utilise la même séquence. (Par exemple `DBCC CHECKIDENT` , pour les colonnes d’identité équivalentes pour les séquences, mais dans les tables potentiellement multiples.)|
