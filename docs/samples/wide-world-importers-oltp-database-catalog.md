---
title: Catalogue de bases de données OLTP WideWorldImporters-SQL | Microsoft Docs
description: Comprendre les schémas, les tables, les procédures stockées et les considérations de conception pour le catalogue de bases de données exemple WideWorldImporters.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 991f001ea8fd43298cefe1f9ba542472de6935ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354096"
---
# <a name="wideworldimporters-database-catalog"></a>Catalogue de bases de données WideWorldImporters
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
La base de données WideWorldImporters contient toutes les informations sur les transactions et les données quotidiennes relatives aux ventes et aux achats, ainsi que les données de capteur pour les véhicules et les chambres froides.

## <a name="schemas"></a>Schémas

WideWorldImporters utilise des schémas à des fins différentes, telles que le stockage de données, la définition de la façon dont les utilisateurs peuvent accéder aux données et la fourniture d’objets pour le développement et l’intégration de l’entrepôt de données.

### <a name="data-schemas"></a>Schémas de données

Ces schémas contiennent les données. Un certain nombre de tables sont nécessaires à tous les autres schémas et sont situés dans le schéma d’application.

|schéma|Description|
|-----------------------------|---------------------|
|Application|Utilisateurs, contacts et paramètres au niveau de l’application. Contient également des tables de référence avec des données utilisées par plusieurs schémas|
|Achat|Achat d’articles sur les achats et informations sur les fournisseurs.|  
|Ventes|Stockez les ventes d’articles pour les clients de détail et les détails sur les clients et les vendeurs. |  
|Warehouse|Stock et transactions d’inventaire.|  

### <a name="secure-access-schemas"></a>Schémas d’accès sécurisé

Ces schémas sont utilisés pour les applications externes qui ne sont pas autorisées à accéder directement aux tables de données. Elles contiennent des vues et des procédures stockées utilisées par des applications externes.

|schéma|Description|
|-----------------------------|---------------------|
|Site web|Tout accès à la base de données à partir du site Web de l’entreprise s’effectue par le biais de ce schéma.|
|Rapports|Tout accès à la base de données à partir de Reporting Services rapports s’effectue par le biais de ce schéma.|
|PowerBI|Tout accès à la base de données à partir des tableaux de bord Power BI via la passerelle d’entreprise se fait par le biais de ce schéma.|

Notez que les schémas rapports et PowerBI ne sont pas utilisés dans la version initiale de l’exemple de base de données. Toutefois, tous les exemples Reporting Services et Power BI basés sur cette base de données sont encouragés à utiliser ces schémas.

### <a name="development-schemas"></a>Schémas de développement

Schémas à usage spécial

|schéma|Description|
|-----------------------------|---------------------|
|Intégration|Objets et procédures requis pour l’intégration de l’entrepôt de données (par exemple, la migration des données vers la base de données WideWorldImportersDW).|
|Séquences|Contient les séquences utilisées par toutes les tables de l’application.|

## <a name="tables"></a>Tables

Toutes les tables de la base de données se trouvent dans les schémas de données.

### <a name="application-schema"></a>Schéma d’application

Détails des paramètres et des personnes (utilisateurs et contacts), ainsi que des tables de référence communes (communes à plusieurs autres schémas).

|Table de charge de travail|Description|
|-----------------------------|---------------------|
|SystemParameters|Contient des paramètres configurables à l’ensemble du système.|
|Personnes|Contient des noms d’utilisateur, des informations de contact, pour tous ceux qui utilisent l’application, et pour les personnes que les importateurs grand public traitent au sein des organisations clientes. Cela comprend le personnel, les clients, les fournisseurs et tout autre contact. Pour les personnes qui ont reçu l’autorisation d’utiliser le système ou le site Web, les informations incluent les informations de connexion.|
|Villes|Il existe de nombreuses adresses stockées dans le système, pour les personnes, les adresses de livraison de l’organisation cliente, les adresses des fournisseurs, etc. Chaque fois qu’une adresse est stockée, il y a une référence à une ville dans ce tableau. Il y a également un emplacement spatial pour chaque ville.|
|StateProvinces|Les villes font partie des États ou des provinces. Ce tableau contient les détails de ceux-ci, y compris les données spatiales qui décrivent les limites de chaque État ou province.|
|Pays|Les États ou provinces font partie des pays. Ce tableau contient les détails de ceux-ci, y compris les données spatiales qui décrivent les limites de chaque pays.|
|DeliveryMethods|Choix pour la diffusion d’articles boursiers (par exemple, camion/Van, poste, collecte, Courier, etc.)|
|PaymentMethods|Choix pour effectuer des paiements (par exemple, espèces, chèque, TEF, etc.)|
|TransactionTypes|Types de transactions de clients, fournisseurs ou actions (par exemple, facture, note de crédit, etc.)|

### <a name="purchasing-schema"></a>Schéma d’achat

Détails des fournisseurs et des achats d’articles en stock.

|Table de charge de travail|Description|
|-----------------------------|---------------------|
|Fournisseurs|Table entité principale pour les fournisseurs (organisations)|
|SupplierCategories|Catégories pour les fournisseurs (par exemple, Novelties, jouets, vêtements, emballage, etc.)|
|SupplierTransactions|Toutes les transactions financières liées aux fournisseurs (factures, paiements)|
|PurchaseOrders|Détails des bons de commande du fournisseur|
|PurchaseOrderLines|Lignes de détails à partir des bons de commande fournisseur|

 
### <a name="sales-schema"></a>Schéma Sales

Détails des clients, des commerciaux et des ventes d’articles en stock.

|Table de charge de travail|Description|
|-----------------------------|---------------------|
|Clients|Tables d’entité principales pour les clients (organisations ou personnes)|
|CustomerCategories|Catégories pour les clients (IE, magasins, supermarchés, etc.)|
|BuyingGroups|Les organisations clientes peuvent faire partie des groupes qui exercent une plus grande puissance d’achat|
|CustomerTransactions|Toutes les transactions financières liées aux clients (factures, paiements)|
|SpecialDeals|Tarification spéciale. Cela peut inclure des prix fixes, une remise en dollars ou un pourcentage de remise.|
|Orders (Commandes)|Détails des commandes client|
|OrderLines|Lignes de détails des commandes client|
|Factures|Détails des factures client|
|InvoiceLines|Lignes de détails des factures client|

### <a name="warehouse-schema"></a>Schéma de l’entrepôt

Détails des actions, de leurs exploitations et de leurs transactions.

|Table de charge de travail|Description|
|-----------------------------|---------------------|
|StockItems|Table entité principale pour les articles boursiers|
|StockItemHoldings|Colonnes non temporelles pour les éléments boursiers. Il s’agit de colonnes fréquemment mises à jour.|
|StockGroups|Groupes permettant de classer les éléments boursiers (par exemple, Novelties, jouets, Novelties comestible, etc.)|
|StockItemStockGroups|Les articles boursiers dans lesquels les groupes boursiers (plusieurs à plusieurs)|
|Couleurs|Les éléments boursiers peuvent éventuellement comporter des couleurs|
|PackageTypes|Les articles boursiers peuvent être empaquetés (par exemple, boîte, carton, palette, kg, etc.).|
|StockItemTransactions|Transactions couvrant tous les mouvements de tous les articles boursiers (réception, vente, réécriture)|
|VehicleTemperatures|Températures enregistrées régulièrement des refroidisseurs de véhicules|
|ColdRoomTemperatures|Températures enregistrées régulièrement des refroidisseurs de la chambre froide|


## <a name="design-considerations"></a>Remarques relatives à la conception

La conception de base de données est subjective et il n’existe pas de méthode correcte ou incorrecte pour concevoir une base de données. Les schémas et les tables de cette base de données montrent des idées de conception de votre propre base de données.

### <a name="schema-design"></a>Conception de schéma

WideWorldImporters utilise un petit nombre de schémas afin qu’il soit facile de comprendre le système de base de données et d’illustrer les principes de base de données.  

Dans la mesure du possible, la base de données regroupe les tables qui sont fréquemment interrogées ensemble dans le même schéma pour réduire la complexité de la jointure.

Le schéma de base de données a été généré par du code en fonction d’une série de tables de métadonnées dans une autre base de données WWI_Preparation. Cela donne à WideWorldImporters un très haut niveau de cohérence de la conception, de cohérence des noms et d’exhaustivité. Pour plus d’informations sur la façon dont le schéma a été généré, consultez le code source : [larges-World-Importers/WWI-Database-scripts](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

### <a name="table-design"></a>Conception de tables

- Toutes les tables ont des clés primaires à colonne unique pour la simplicité des jointures.
- Tous les schémas, les tables, les colonnes, les index et les contraintes de validation possèdent une propriété étendue description qui peut être utilisée pour identifier le rôle de l’objet ou de la colonne. Les tables optimisées en mémoire sont une exception à cela puisqu’elles ne prennent pas actuellement en charge les propriétés étendues.
- Toutes les clés étrangères sont indexées automatiquement, sauf s’il existe un autre index non-cluster qui a le même composant de gauche.
- La numérotation automatique dans les tables est basée sur les séquences. Ces séquences sont plus faciles à utiliser entre les serveurs liés et les environnements similaires que les colonnes d’identité. Les tables optimisées en mémoire utilisent des colonnes d’identité, car elles ne prennent pas en charge dans SQL Server 2016.
- Une seule séquence (TransactionID) est utilisée pour les tables suivantes : CustomerTransactions, SupplierTransactions et StockItemTransactions. Cela montre comment un ensemble de tables peut avoir une seule séquence.
- Certaines colonnes ont des valeurs par défaut appropriées.

### <a name="security-schemas"></a>Schémas de sécurité

Pour la sécurité, WideWorldImporters ne permet pas aux applications externes d’accéder directement aux schémas de données. Pour isoler l’accès, WideWorldImporters utilise des schémas d’accès de sécurité qui ne contiennent pas de données, mais qui contiennent des vues et des procédures stockées. Les applications externes utilisent les schémas de sécurité pour récupérer les données qu’elles sont autorisées à afficher.  De cette façon, les utilisateurs peuvent uniquement exécuter les vues et les procédures stockées dans les schémas d’accès sécurisé

Par exemple, cet exemple comprend Power BI tableaux de bord. Une application externe accède à ces tableaux de bord Power BI à partir de la passerelle Power BIen tant qu’utilisateur disposant de l’autorisation de lecture seule sur le schéma PowerBI.  Pour l’autorisation de lecture seule, l’utilisateur a uniquement besoin de l’autorisation SELECT et EXECUTe sur le schéma PowerBI. Un administrateur de base de données sur WWI affecte ces autorisations en fonction des besoins.

## <a name="stored-procedures"></a>Procédures stockées

Les procédures stockées sont organisées dans des schémas. La plupart des schémas sont utilisés à des fins de configuration ou d’exemple.

Le `Website` schéma contient les procédures stockées qui peuvent être utilisées par un serveur Web frontal.

Les `Reports` `PowerBI` schémas et sont destinés à des fins de Reporting Services et de PowerBI. Toutes les extensions de l’exemple sont encouragées à utiliser ces schémas à des fins de création de rapports.

### <a name="website-schema"></a>Schéma de site Web

Il s’agit des procédures utilisées par une application cliente, par exemple un serveur Web frontal.

|Procédure|Objectif|
|-----------------------------|---------------------|
|ActivateWebsiteLogon|Permet à une personne (de `Application.People` ) d’accéder au site Web.|
|ChangePassword|Modifie le mot de passe d’un utilisateur (pour les utilisateurs qui n’utilisent pas de mécanismes d’authentification externe).|
|InsertCustomerOrders|Permet d’insérer une ou plusieurs commandes client (y compris les lignes de commande).|
|InvoiceCustomerOrders|Prend une liste de commandes à facturer et traite les factures.|
|RecordColdRoomTemperatures|Prend une liste de données de capteur, en tant que paramètre table (TVP), et applique les données à la `Warehouse.ColdRoomTemperatures` table temporelle.|
|RecordVehicleTemperature|Prend un tableau JSON et l’utilise pour mettre à jour `Warehouse.VehicleTemperatures` .|
|SearchForCustomers|Recherche des clients en fonction de leur nom ou d’une partie de leur nom (nom de la société ou nom de la personne).|
|SearchForPeople|Recherche des personnes par nom ou partie du nom.|
|SearchForStockItems|Recherche des articles boursiers par nom ou par partie du nom ou des commentaires marketing.|
|SearchForStockItemsByTags|Recherche les éléments boursiers par balises.|
|SearchForSuppliers|Recherche des fournisseurs en fonction de leur nom ou d’une partie de leur nom (nom de la société ou nom de la personne).|

### <a name="integration-schema"></a>Schéma d’intégration

Les procédures stockées dans ce schéma sont utilisées par le processus ETL. Ils obtiennent les données nécessaires à partir de différentes tables pour la plage de temps requise par le [package ETL](wide-world-importers-perform-etl.md).

### <a name="dataloadsimulation-schema"></a>Schéma DataLoadSimulation

Simule une charge de travail qui insère des ventes et des achats. La procédure stockée principale est `PopulateDataToCurrentDate` , qui est utilisée pour insérer des exemples de données jusqu’à la date actuelle.

|Procédure|Objectif|
|-----------------------------|---------------------|
|Configuration_ApplyDataLoadSimulationProcedures|Recrée les procédures nécessaires à la simulation du chargement des données. Cela est nécessaire pour ramener les données à la date actuelle.|
|Configuration_RemoveDataLoadSimulationProcedures|Cela supprime les procédures une fois la simulation de données terminée.|
|DeactivateTemporalTablesBeforeDataLoad|Supprime la nature temporelle de toutes les tables temporelles et, le cas échéant, applique un déclencheur afin que les modifications puissent être apportées comme si elles étaient appliquées à une date antérieure à celle autorisée par les tables sys-temporal.|
|PopulateDataToCurrentDate|Utilisé pour ramener les données jusqu’à la date actuelle. Doit être exécuté avant toute autre option de configuration après la restauration de la base de données à partir d’une sauvegarde initiale.|
|ReactivateTemporalTablesAfterDataLoad|Rétablit les tables temporelles, notamment la vérification de la cohérence des données. (Supprime les déclencheurs associés).|


### <a name="application-schema"></a>Schéma d’application

Ces procédures sont utilisées pour configurer l’exemple. Ils sont utilisés pour appliquer des fonctionnalités Enterprise Edition à la version Standard Edition de l’exemple, ainsi que pour ajouter l’audit et l’indexation de texte intégral.

|Procédure|Objectif|
|-----------------------------|---------------------|
|AddRoleMemberIfNonexistant|Ajoute un membre à un rôle si le membre n’est pas déjà dans le rôle|
|Configuration_ApplyAuditing|Ajoute l’audit. L’audit du serveur est appliqué pour les bases de données Standard Edition ; un audit de base de données supplémentaire est ajouté pour l’édition entreprise.|
|Configuration_ApplyColumnstoreIndexing|Applique l’indexation ColumnStore à `Sales.OrderLines` et `Sales.InvoiceLines` et réindexe de manière appropriée.|
|Configuration_ApplyFullTextIndexing|Applique les index de texte intégral à `Application.People` , `Sales.Customers` , `Purchasing.Suppliers` et `Warehouse.StockItems` . Remplace `Website.SearchForPeople` , `Website.SearchForSuppliers` , `Website.SearchForCustomers` , `Website.SearchForStockItems` , `Website.SearchForStockItemsByTags` par des procédures de remplacement qui utilisent l’indexation de texte intégral.|
|Configuration_ApplyPartitioning|Applique le partitionnement de table à `Sales.CustomerTransactions` et `Purchasing.SupplierTransactions` , puis réorganise les index en fonction.|
|Configuration_ApplyRowLevelSecurity|Applique la sécurité au niveau des lignes pour filtrer les clients par rôle lié à un secteur de vente.|
|Configuration_ConfigureForEnterpriseEdition|Applique l’indexation ColumnStore, le texte intégral, la mémoire, Polybase et le partitionnement.|
|Configuration_EnableInMemory|Ajoute un groupe de fichiers mémoire optimisé (si vous ne travaillez pas dans Azure), remplace `Warehouse.ColdRoomTemperatures` , `Warehouse.VehicleTemperatures` avec les équivalents en mémoire et migre les données, recrée `Website.OrderIDList` les `Website.OrderList` types de tables,,, `Website.OrderLineList` `Website.SensorDataList` avec des équivalents mémoire optimisés, supprime et recrée les procédures `Website.InvoiceCustomerOrders` , `Website.InsertCustomerOrders` et `Website.RecordColdRoomTemperatures` qui utilise ces types de tables.|
|Configuration_RemoveAuditing|Supprime la configuration d’audit.|
|Configuration_RemoveRowLevelSecurity|Supprime la configuration de la sécurité au niveau des lignes (nécessaire pour les modifications apportées aux tables associées).|
|CreateRoleIfNonExistant|Crée un rôle de base de données s’il n’existe pas déjà.|


### <a name="sequences-schema"></a>Schéma de séquences

Procédures pour configurer les séquences dans la base de données.

|Procédure|Objectif|
|-----------------------------|---------------------|
|ReseedAllSequences|Appelle la procédure ReseedSequenceBeyondTableValue pour toutes les séquences.|
|ReseedSequenceBeyondTableValue|Utilisé pour repositionner la valeur de séquence suivante au-delà de la valeur dans une table qui utilise la même séquence. (Comme un DBCC CHECKIDENT pour les colonnes d’identité équivalentes pour les séquences, mais dans les tables potentiellement multiples).|
