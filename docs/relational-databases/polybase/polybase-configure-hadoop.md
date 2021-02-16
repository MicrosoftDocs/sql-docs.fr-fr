---
title: 'Accéder aux données externes : Hadoop - PolyBase'
description: L’article utilise PolyBase sur une instance SQL Server avec Hadoop. PolyBase est adapté aux requêtes ad hoc de tables externes et à l’importation/exportation de données.
ms.date: 12/13/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.custom: seo-dt-2019
ms.openlocfilehash: 1124d05dc57697d9e27757dad2b4fee842a22a5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351800"
---
# <a name="configure-polybase-to-access-external-data-in-hadoop"></a>Configurer PolyBase pour accéder à des données externes dans Hadoop

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

L’article explique comment utiliser PolyBase sur une instance SQL Server pour interroger des données externes dans Hadoop.

## <a name="prerequisites"></a>Prérequis

- Si vous n’avez pas installé PolyBase, consultez [Installation de PolyBase](polybase-installation.md). Cet article décrit les prérequis pour l’installation.

<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

- À compter de SQL Server 2019, vous devez également [activer la fonctionnalité PolyBase](polybase-installation.md#enable).

::: moniker-end

- PolyBase prend en charge deux fournisseurs Hadoop, HDP (Hortonworks Data Platform) et CDH (Cloudera Distributed Hadoop). Hadoop suit le modèle « majeure.mineure.version » pour ses nouvelles versions, et toutes les versions d’une version majeure ou mineure prise en charge sont prises en charge. Les fournisseurs Hadoop suivants sont pris en charge :

  - Hortonworks HDP 1.3, 2.1-2.6, 3.0 sur Linux
  - Hortonworks HDP 1.3, 2.1-2.3 sur Windows Server
  - Cloudera CDH 4.3, 5.1 - 5.5, 5.9 - 5.13 sur Linux

> [!NOTE]
> PolyBase prend en charge les zones de chiffrement Hadoop à partir de SQL Server 2016 SP1 CU7 et SQL Server 2017 CU3. Si vous utilisez des [groupes de scale-out PolyBase](polybase-scale-out-groups.md), tous les nœuds de calcul doivent également se trouver sur une build qui inclut la prise en charge des zones de chiffrement Hadoop.

### <a name="configure-hadoop-connectivity"></a>Configurer la connectivité Hadoop

Configurez d’abord SQL Server PolyBase pour utiliser votre fournisseur Hadoop spécifique.

1. Exécutez [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) avec 'hadoop connectivity' et définissez une valeur appropriée pour votre fournisseur. Pour trouver la valeur pour votre fournisseur, consultez [Configuration de la connectivité PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md). Par défaut, la connectivité Hadoop est définie sur 7.

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Vous devez redémarrer SQL Server avec **services.msc**. Le redémarrage de SQL Server redémarre ces services :  

   - Service de déplacement des données SQL Server PolyBase  
   - Moteur SQL Server PolyBase  
  
   ![arrêter et démarrer les services PolyBase dans services.msc](../../relational-databases/polybase/media/polybase-stop-start.png "arrêter et démarrer les services PolyBase dans services.msc")  
  
## <a name="enable-pushdown-computation"></a><a id="pushdown"></a>Activer le calcul pushdown  

Pour améliorer les performances des requêtes, activez le calcul pushdown sur votre cluster Hadoop :  
  
1. Recherchez le fichier **yarn-site.XML** dans le chemin d’installation de SQL Server. En règle générale, le chemin d’accès est le suivant :  

   ```xml  
   C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf\  
   ```  

1. Sur l’ordinateur Hadoop, recherchez le fichier analogue dans le répertoire de configuration Hadoop. Dans le fichier, recherchez et copiez la valeur de la clé de configuration yarn.application.classpath.  
  
1. Sur l’ordinateur SQL Server, dans le **fichier yarn-site.xml** , recherchez la propriété **yarn.application.classpath**. Collez la valeur de l’ordinateur Hadoop dans l’élément de valeur.  
  
1. Pour toutes les versions 5.X de CDH, vous devez ajouter les paramètres de configuration mapreduce.application.classpath, soit à la fin de votre fichier yarn-site.xml, soit dans le fichier mapred-site.xml. HortonWorks inclut ces configurations dans les configurations yarn.application.classpath. Consultez [Configuration PolyBase](../../relational-databases/polybase/polybase-configuration.md) pour voir des exemples.

## <a name="configure-an-external-table"></a>Configurer une table externe

Pour interroger les données dans votre source de données Hadoop, vous devez définir une table externe à utiliser dans les requêtes Transact-SQL. Les étapes suivantes décrivent comment configurer la table externe.

1. Créez une clé principale pour la base de données, si celle-ci n’en a pas. C’est nécessaire pour chiffrer le secret des informations d’identification.

     ```sql
      CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';  
     ```
    ## <a name="arguments"></a>Arguments
    PASSWORD ='password'

    Mot de passe utilisé pour chiffrer la clé principale dans la base de données. Le mot de passe doit remplir les critères de la stratégie de mot de passe Windows de l’ordinateur qui héberge l’instance SQL Server.
1. Créez des informations d’identification limitées à la base de données pour les clusters Hadoop sécurisés par Kerberos.

   ```sql
   -- IDENTITY: the Kerberos user name.  
   -- SECRET: the Kerberos password  
   CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
   WITH IDENTITY = '<hadoop_user_name>', Secret = '<hadoop_password>';  
   ```

2. Créez une source de données externe avec [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

   ```sql
   -- LOCATION (Required) : Hadoop Name Node IP address and port.  
   -- RESOURCE MANAGER LOCATION (Optional): Hadoop Resource Manager location to enable pushdown computation.  
   -- CREDENTIAL (Optional):  the database scoped credential, created above.  
   CREATE EXTERNAL DATA SOURCE MyHadoopCluster WITH (  
         TYPE = HADOOP,
         LOCATION ='hdfs://10.xxx.xx.xxx:xxxx',
         RESOURCE_MANAGER_LOCATION = '10.xxx.xx.xxx:xxxx',
         CREDENTIAL = HadoopUser1
   );  
   ```

3. Créez un format de fichier externe avec [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE))
   ```

4. Créez une table externe pointant vers les données stockées dans Hadoop avec [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md). Dans cet exemple, les données externes contiennent des données provenant de capteurs sur des voitures.

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
         DATA_SOURCE = MyHadoopCluster,  
         FILE_FORMAT = TextFileFormat  
   );  
   ```

5. Créez des statistiques sur une table externe.

   ```sql
   CREATE STATISTICS StatsForSensors on CarSensor_Data(CustomerKey, Speed)  
   ```

## <a name="polybase-queries"></a>Requêtes PolyBase

PolyBase est approprié pour trois fonctions :  
  
- Requêtes ad hoc sur des tables externes.  
- Importation de données.  
- Exportation de données.  

Les requêtes suivantes fournissent un exemple avec des données fictives provenant de capteurs sur des voitures.

### <a name="ad-hoc-queries"></a>Requêtes ad hoc  

La requête ad hoc suivante fait une jointure entre des données relationnelles et des données Hadoop. Elle sélectionne les clients qui dépassent la vitesse de 35 mph, en faisant une jointure entre les données structurées sur les clients stockées dans SQL Server et les données des capteurs des véhicules stockées dans Hadoop.  

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

La requête suivante exporte des données depuis SQL Server vers Hadoop. Pour cela, vous devez d’abord activer l’exportation PolyBase. Elle crée une table externe pour la destination avant d’y exporter les données.

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

## <a name="view-polybase-objects-in-ssms"></a>Afficher les objets PolyBase dans SSMS  

Dans SSMS, les tables externes sont affichées dans un dossier distinct, **Tables externes**. Les sources de données externes et les formats de fichiers externes figurent dans des sous-dossiers du dossier **Ressources externes**.  
  
![Objets PolyBase dans SSMS](media/polybase-management.png)  

## <a name="next-steps"></a>Étapes suivantes

Explorez d’autres façons d’utiliser et de superviser PolyBase dans les articles suivants :

[Groupes de scale-out PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).  
[Résolution des problèmes de PolyBase](polybase-troubleshooting.md).  
