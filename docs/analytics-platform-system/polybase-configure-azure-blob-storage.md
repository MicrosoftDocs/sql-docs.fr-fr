---
title: Utiliser Polybase pour accéder à des données externes dans le stockage d’objets BLOB Azure
description: Explique comment utiliser Polybase sur un Data Warehouse parallèle (APS) pour interroger des données externes dans le stockage d’objets BLOB Azure.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 6de393578f149f0d7acd82d6867ae545784d7006
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049116"
---
# <a name="configure-polybase-to-access-external-data-in-azure-blob-storage"></a>Configurer PolyBase pour accéder à des données externes dans Stockage Blob Azure

L’article explique comment utiliser PolyBase sur une instance SQL Server pour interroger des données externes dans Stockage Blob Azure.

> [!NOTE]
> Actuellement, les APS prennent uniquement en charge le stockage d’objets BLOB Azure à usage général v1 localement redondant (LRS).

## <a name="prerequisites"></a>Prérequis

 - Stockage d’objets BLOB Azure dans votre abonnement.
 - Conteneur créé dans le stockage d’objets BLOB Azure.

### <a name="configure-azure-blob-storage-connectivity"></a>Configurer la connectivité du stockage d’objets BLOB Azure

Tout d’abord, configurez les points d’accès pour utiliser le stockage d’objets BLOB Azure.

1. Exécutez [sp_configure](../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) avec 'hadoop connectivity' défini sur un fournisseur Stockage Blob Azure. Pour trouver la valeur pour les fournisseurs, consultez [Configuration de la connectivité PolyBase](../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure Blob Storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Redémarrez la région APS à l’aide de la page État du service sur le Configuration Manager de l' [Appliance](launch-the-configuration-manager.md).
  
## <a name="configure-an-external-table"></a>Configurer une table externe

Pour interroger les données dans votre stockage d’objets BLOB Azure, vous devez définir une table externe à utiliser dans les requêtes Transact-SQL. Les étapes suivantes décrivent comment configurer la table externe.

1. Créez une clé principale sur la base de données. Il est nécessaire de chiffrer le secret des informations d’identification.

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
   ```

1. Créez des informations d’identification délimitées à la base de données pour le stockage BLOB Azure.

   ```sql
   -- IDENTITY: any string (this is not used for authentication to Azure storage).  
   -- SECRET: your Azure storage account key.  
   CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
   WITH IDENTITY = 'user', Secret = '<azure_storage_account_key>';
   ```

1. Créez une source de données externe avec [CREATE EXTERNAL DATA SOURCE](../t-sql/statements/create-external-data-source-transact-sql.md).

   ```sql
   -- LOCATION:  Azure account storage account name and blob container name.  
   -- CREDENTIAL: The database scoped credential created above.  
   CREATE EXTERNAL DATA SOURCE AzureStorage with (  
         TYPE = HADOOP,
         LOCATION ='wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',  
         CREDENTIAL = AzureStorageCredential  
   );  
   ```

1. Créez un format de fichier externe avec [CREATE EXTERNAL FILE FORMAT](../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Azure Blob Storage (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   -- In this example, the files are pipe (|) delimited
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE)  
   ```

1. Créez une table externe pointant vers les données stockées dans Stockage Azure avec [CREATE EXTERNAL TABLE](../t-sql/statements/create-external-table-transact-sql.md). Dans cet exemple, les données externes contiennent des données provenant de capteurs sur des voitures.

   ```sql
   -- LOCATION: path to file or directory that contains the data (relative to HDFS root).  
   CREATE EXTERNAL TABLE [dbo].[CarSensor_Data] (  
         [SensorKey] int NOT NULL,
         [CustomerKey] int NOT NULL,
         [GeographyKey] int NULL,
         [Speed] float NOT NULL,
         [YearMeasured] int NOT NULL  
   )  
   WITH (LOCATION='/Demo/',
         DATA_SOURCE = AzureStorage,  
         FILE_FORMAT = TextFileFormat  
   );  
   ```

1. Créez des statistiques sur une table externe.

   ```sql
   CREATE STATISTICS StatsForSensors on CarSensor_Data(CustomerKey, Speed)  
   ```

## <a name="polybase-queries"></a>Requêtes PolyBase

PolyBase est approprié pour trois fonctions :  
  
- Requêtes ad hoc sur des tables externes.  
- Importation de données.  
- Exportation de données.  

Les requêtes suivantes fournissent un exemple avec des données fictives provenant de capteurs sur des voitures.

### <a name="ad-hoc-queries"></a>requêtes ad hoc ;  

La requête ad hoc suivante joint les données relationnelles au stockage d’objets BLOB Azure. Il sélectionne les clients qui ont une vitesse supérieure à 35 km, en joignant les données clientes structurées stockées dans SQL Server avec les données de capteur de voiture stockées dans le stockage d’objets BLOB Azure.  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
```  

### <a name="importing-data"></a>Importation de données  

La requête suivante importe des données externes dans APS. Cet exemple importe des données pour des pilotes rapides dans des points d’accès afin d’effectuer une analyse plus approfondie. Pour améliorer les performances, il tire parti de la technologie ColumnStore dans APS.  

```sql
CREATE TABLE Fast_Customers
WITH
(CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH (CustomerKey))
AS
SELECT DISTINCT
      Insured_Customers.CustomerKey, Insured_Customers.FirstName, Insured_Customers.LastName,   
      Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
from Insured_Customers INNER JOIN   
(  
      SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
```  

### <a name="exporting-data"></a>Exportation de données  

La requête suivante exporte des données à partir de points d’accès vers le stockage d’objets BLOB Azure. Il peut être utilisé pour archiver des données relationnelles dans le stockage d’objets BLOB Azure tout en étant en mesure de les interroger.

```sql
-- Export data: Move old data to Azure Blob Storage while keeping it query-able via an external table.  
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] 
WITH (  
      LOCATION='/archive/customer/2009',  
      DATA_SOURCE = AzureStorage,  
      FILE_FORMAT = TextFileFormat
)  
AS
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  

## <a name="view-polybase-objects-in-ssdt"></a>Afficher les objets Polybase dans SSDT  

Dans SQL Server Data Tools, les tables externes sont affichées dans un dossier distinct **tables externes**. Les sources de données externes et les formats de fichiers externes figurent dans des sous-dossiers du dossier **Ressources externes**.  
  
![Objets Polybase dans SSDT](media/polybase/external-tables-datasource.png)  

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur PolyBase, consultez [Qu’est-ce que PolyBase ?](../relational-databases/polybase/polybase-guide.md). 

