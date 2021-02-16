---
title: Scénarios de requêtes PolyBase | Microsoft Docs
description: Consultez des exemples de requêtes utilisant la fonctionnalité PolyBase de SQL Server, notamment SELECT, la jointure externe à des tables locales avec JOIN, l’importation/exportation de données et de nouveaux affichages catalogue.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
keywords:
- PolyBase
helpviewer_keywords:
- PolyBase, import and export
- Hadoop, import with PolyBase
- Hadoop, export with PolyBase
- Azure blob storage, import with PolyBase
- Azure blob storage, export with PolyBase
ms.assetid: 2c5aa2bd-af7d-4f57-9a28-9673c2a4c07e
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: cdcd5d06395d8216fdf3e9c61b8434ab226c9837
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351833"
---
# <a name="polybase-query-scenarios"></a>Scénarios de requêtes PolyBase

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Cet article donne des exemples de requêtes qui utilisent la fonctionnalité [PolyBase](../../relational-databases/polybase/polybase-guide.md) de SQL Server ( à partir de la version 2016). Avant d’utiliser ces exemples, vous devez d’abord installer et configurer PolyBase. Pour plus d’informations, consultez [Vue d’ensemble de PolyBase](polybase-guide.md).
  
Exécutez des instructions Transact-SQL sur les tables externes ou utilisez des outils d’aide à la décision pour interroger des tables externes.
  
## <a name="select-from-external-table"></a>Requête SELECT sur une table externe  

Requête simple qui retourne des données à partir d’une table externe définie.  

```sql  
SELECT TOP 10 * FROM [dbo].[SensorData];   
```

Requête simple qui inclut un prédicat.

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65;
```

## <a name="join-external-tables-with-local-tables"></a>Jointure de tables externes à des tables locales avec JOIN

```sql
SELECT InsuranceCustomers.FirstName,   
   InsuranceCustomers.LastName,   
   SensorData.Speed  
FROM InsuranceCustomers INNER JOIN SensorData    
ON InsuranceCustomers.CustomerKey = SensorData.CustomerKey   
WHERE SensorData.Speed > 65   
ORDER BY SensorData.Speed DESC  
  
```  

## <a name="import-data"></a>Importer des données

Importez des données de Hadoop ou d’Azure Storage dans SQL Server à des fins de stockage permanent. Utilisez SELECT INTO pour importer des données référencées par une table externe, en vue de leur stockage permanent dans SQL Server. Créez une table relationnelle à la volée, puis créez un index columnstore en haut de la table.

```sql
-- PolyBase scenario - import external data into SQL Server
-- Import data for fast drivers into SQL Server to do more in-depth analysis
-- Leverage columnstore technology
  
SELECT DISTINCT   
        Insured_Customers.FirstName, Insured_Customers.LastName,   
        Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
        SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome  
  
CREATE CLUSTERED COLUMNSTORE INDEX CCI_FastCustomers ON Fast_Customers;  
```

## <a name="export-data"></a>Exporter des données

Exportez des données de SQL Server vers Hadoop ou le Stockage Azure. 

Tout d’abord, activez la fonctionnalité d’exportation en définissant la valeur `sp_configure` de l’option « autoriser l’exportation polybase » sur 1. Créez ensuite une table externe pointant vers le répertoire de destination. L’instruction CREATE EXTERNAL TABLE crée le répertoire de destination, s’il n’existe pas encore. Utilisez ensuite l’instruction INSERT INTO pour exporter les données d’une table SQL Server locale dans la source de données externe. 

Les résultats de l’instruction SELECT sont exportés dans un fichier au format et à l’emplacement spécifiés. Les fichiers externes sont nommés *QueryID_date_time_ID.format*, où *ID* est un identificateur incrémentiel et *format* est le format des données exportées. Exemple de nom de fichier : QID776_20160130_182739_0.orc.

> [!NOTE]
> En cas d’exportation de données vers Hadoop ou le Stockage Blob Azure via PolyBase, seules les données sont exportées, et non les noms de colonnes (métadonnées), comme le définit la commande CREATE EXTERNAL TABLE.

```sql  
-- PolyBase scenario  - export data from SQL Server to Hadoop
-- Create an external table
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
        [FirstName] char(25) NOT NULL,   
        [LastName] char(25) NOT NULL,   
        [YearlyIncome] float NULL,   
        [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
        LOCATION='/old_data/2009/customerdata',  
        DATA_SOURCE = HadoopHDP2,  
        FILE_FORMAT = TextFileFormat,  
        REJECT_TYPE = VALUE,  
        REJECT_VALUE = 0  
);  
  
-- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
INSERT INTO dbo.FastCustomers2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```

## <a name="new-catalog-views"></a>Nouveaux affichages catalogue

Les nouveaux affichages catalogue suivants montrent des ressources externes.

```sql
SELECT * FROM sys.external_data_sources;   
SELECT * FROM sys.external_file_formats;  
SELECT * FROM sys.external_tables;  
```

 Déterminer si une table est une table externe à l’aide de `is_external`  

```sql  
SELECT name, type, is_external FROM sys.tables WHERE name='myTableName'   
```  

## <a name="next-steps"></a>Étapes suivantes  

Pour en savoir plus sur la résolution des problèmes, consultez [Résolution des problèmes de Polybase](../../relational-databases/polybase/polybase-troubleshooting.md).
