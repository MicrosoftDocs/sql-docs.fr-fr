---
title: 'Accéder aux données externes : Stockage Blob Azure - PolyBase'
description: L’article utilise PolyBase sur une instance SQL Server avec le stockage Blob Azure. PolyBase est adapté aux requêtes ad hoc de tables externes et à l’importation/exportation de données.
ms.date: 12/02/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.custom: seo-dt-2019, seo-lt-2019
ms.openlocfilehash: 5363cabdce211884ae9e0f998fcde3056700d7a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073643"
---
# <a name="configure-polybase-to-access-external-data-in-azure-blob-storage"></a>Configurer PolyBase pour accéder à des données externes dans Stockage Blob Azure

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

L’article explique comment utiliser PolyBase sur une instance SQL Server pour interroger des données externes dans Stockage Blob Azure.

## <a name="prerequisites"></a>Prérequis

Si vous n’avez pas installé PolyBase, consultez [Installation de PolyBase](polybase-installation.md). Cet article décrit les prérequis pour l’installation.

### <a name="configure-azure-blob-storage-connectivity"></a>Configurer la connectivité avec Stockage Blob Azure

Configurez d’abord SQL Server PolyBase pour l’utilisation de Stockage Blob Azure.

1. Exécutez [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) avec 'hadoop connectivity' défini sur un fournisseur Stockage Blob Azure. Pour trouver la valeur pour les fournisseurs, consultez [Configuration de la connectivité PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md). Par défaut, la connectivité Hadoop est définie sur 7.

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Redémarrez SQL Server avec **services.msc**. Le redémarrage de SQL Server redémarre ces services :  

   - Service de déplacement des données SQL Server PolyBase  
   - Moteur SQL Server PolyBase  
  
   ![arrêter et démarrer les services PolyBase dans services.msc](../../relational-databases/polybase/media/polybase-stop-start.png "arrêter et démarrer les services PolyBase dans services.msc")  
  
## <a name="configure-an-external-table"></a>Configurer une table externe

Pour interroger les données dans votre source de données Hadoop, vous devez définir une table externe à utiliser dans les requêtes Transact-SQL. Les étapes suivantes décrivent comment configurer la table externe.

1. Créez une clé principale sur la base de données. La clé principale est nécessaire pour chiffrer le secret des informations d’identification.

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
   ```

1. Créez des informations d’identification limitées à la base de données pour Stockage Blob Azure.

   ```sql
   -- IDENTITY: any string (this is not used for authentication to Azure storage).  
   -- SECRET: your Azure storage account key.  
   CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
   WITH IDENTITY = 'user', Secret = '<azure_storage_account_key>';
   ```

1. Créez une source de données externe avec [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

   ```sql
   -- LOCATION:  Azure account storage account name and blob container name.  
   -- CREDENTIAL: The database scoped credential created above.  
   CREATE EXTERNAL DATA SOURCE AzureStorage with (  
         TYPE = HADOOP,
         LOCATION ='wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',  
         CREDENTIAL = AzureStorageCredential  
   );  
   ```

1. Créez un format de fichier externe avec [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE))  
   ```

1. Créez une table externe pointant vers les données stockées dans Stockage Azure avec [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md). Dans cet exemple, les données externes contiennent des données provenant de capteurs sur des voitures.

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

La requête ad hoc suivante fait une jointure entre des données relationnelles et des données Hadoop. Elle sélectionne les clients qui dépassent la vitesse de 35 mph. Elle effectue également une jointure entre les données client structurées stockées dans SQL Server et les données des capteurs des véhicules stockées dans Hadoop.  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
OPTION (FORCE EXTERNALPUSHDOWN);   -- or OPTION (DISABLE EXTERNALPUSHDOWN)  
```  

### <a name="importing-data"></a>Importation de données  

La requête suivante importe des données externes dans SQL Server. Cet exemple importe les données pour les conducteurs roulant rapidement dans SQL Server pour effectuer une analyse plus approfondie. Pour améliorer les performances, elle tire parti de la technologie Columnstore.  

```sql
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

### <a name="exporting-data"></a>Exportation de données  

La requête suivante exporte des données depuis SQL Server vers Stockage Blob Azure. Elle commence par activer l’exportation PolyBase. Ensuite, elle crée une table externe pour la destination avant d’y exporter les données.

```sql
-- Enable INSERT into external table  
sp_configure 'allow polybase export', 1;  
reconfigure  
  
-- Create an external table.
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
INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  

Cette méthode d’exportation PolyBase est susceptible de créer plusieurs fichiers.

## <a name="view-polybase-objects-in-ssms"></a>Afficher les objets PolyBase dans SSMS  

Dans SSMS, les tables externes sont affichées dans un dossier distinct, **Tables externes**. Les sources de données externes et les formats de fichiers externes figurent dans des sous-dossiers du dossier **Ressources externes**.  
  
![Objets PolyBase dans SSMS](media/polybase-management.png)  

## <a name="next-steps"></a>Étapes suivantes

Explorez d’autres façons d’utiliser et de superviser PolyBase dans les articles suivants :

[Groupes de scale-out PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).  
[Résolution des problèmes de PolyBase](polybase-troubleshooting.md).  
