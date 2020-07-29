---
title: Interroger des données externes dans Oracle
titleSuffix: SQL Server big data clusters
description: Ce tutoriel montre comment interroger les données Oracle à partir d’un cluster Big Data SQL Server 2019. Vous créez une table externe sur les données dans Oracle, puis exécutez une requête.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
ms.date: 08/21/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7695cf6d88fd05fdbffba28663c910889797ace9
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85772845"
---
# <a name="tutorial-query-oracle-from-a-sql-server-big-data-cluster"></a>Tutoriel : Interroger Oracle à partir d’un cluster Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Ce tutoriel montre comment interroger les données Oracle à partir d’un cluster Big Data SQL Server 2019. Pour exécuter ce tutoriel, vous devez avoir accès à un serveur Oracle. Si vous n’y avez pas accès, ce tutoriel peut vous donner une idée du fonctionnement de la virtualisation de données pour les sources de données externes dans un cluster Big Data SQL Server.

Dans ce tutoriel, vous allez apprendre à :

> [!div class="checklist"]
> * Créer une table externe pour les données d’une base de données Oracle externe.
> * Associer ces données à des données à valeur élevée dans l’instance maître.

> [!TIP]
> Si vous préférez, vous pouvez télécharger et exécuter un script pour les commandes de ce tutoriel. Pour obtenir des instructions, consultez les [exemples de virtualisation de données](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-virtualization) sur GitHub.

## <a name="prerequisites"></a><a id="prereqs"></a> Conditions préalables

- [Outils Big Data](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Extension SQL Server 2019**
- [Charger des exemples de données dans votre cluster Big Data](tutorial-load-sample-data.md)

## <a name="create-an-oracle-table"></a>Créer une table Oracle

Les étapes suivantes permettent de créer un exemple de table nommée `INVENTORY` dans Oracle.

1. Connectez-vous à une instance Oracle et à une base de données que vous souhaitez utiliser pour ce tutoriel.

1. Exécutez l’instruction suivante pour créer la table `INVENTORY` :

   ```sql
    CREATE TABLE "INVENTORY"
    (
        "INV_DATE" NUMBER(10,0) NOT NULL,
        "INV_ITEM" NUMBER(10,0) NOT NULL,
        "INV_WAREHOUSE" NUMBER(10,0) NOT NULL,
        "INV_QUANTITY_ON_HAND" NUMBER(10,0)
    );

    CREATE INDEX INV_ITEM ON HR.INVENTORY(INV_ITEM);
    ```

1. Importez le contenu du fichier **inventory.csv** dans cette table. Ce fichier a été créé par les exemples de scripts de création de la section [Conditions préalables](#prereqs).

## <a name="create-an-external-data-source"></a>Créer une source de données externe

La première étape consiste à créer une source de données externe qui peut accéder à votre serveur Oracle.

1. Dans Azure Data Studio, connectez-vous à l’instance maître SQL Server de votre cluster Big Data. Pour plus d’informations, consultez [Se connecter à l’instance maître SQL Server](connect-to-big-data-cluster.md#master).

1. Double-cliquez sur la connexion dans la fenêtre **Serveurs** pour afficher le tableau de bord de serveur de l’instance maître SQL Server. Sélectionnez **Nouvelle requête**.

   ![Requête d’instance maître SQL Server](./media/tutorial-query-oracle/sql-server-master-instance-query.png)

1. Exécutez la commande Transact-SQL suivante pour remplacer le contexte par celui de la base de données **Sales** dans l’instance maître.

   ```sql
   USE Sales
   GO
   ```

1. Créez des informations d’identification étendues à la base de données pour vous connecter au serveur Oracle. Fournissez les informations d’identification appropriées à votre serveur Oracle dans l’instruction suivante.

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL [OracleCredential]
   WITH IDENTITY = '<oracle_user,nvarchar(100),SYSTEM>', SECRET = '<oracle_user_password,nvarchar(100),manager>';
   ```

1. Créez une source de données externe qui pointe vers le serveur Oracle.

   ```sql
   CREATE EXTERNAL DATA SOURCE [OracleSalesSrvr]
   WITH (LOCATION = 'oracle://<oracle_server,nvarchar(100)>',CREDENTIAL = [OracleCredential]);
   ```

## <a name="create-an-external-table"></a>Créer une table externe

Créez ensuite une table externe nommée **inventory_ora** sur la table `INVENTORY` sur le serveur Oracle.

```sql
CREATE EXTERNAL TABLE [inventory_ora]
    ([inv_date] DECIMAL(10,0) NOT NULL, [inv_item] DECIMAL(10,0) NOT NULL,
    [inv_warehouse] DECIMAL(10,0) NOT NULL, [inv_quantity_on_hand] DECIMAL(10,0))
WITH (DATA_SOURCE=[OracleSalesSrvr],
        LOCATION='<oracle_service_name,nvarchar(30),xe>.<oracle_schema,nvarchar(128),HR>.<oracle_table,nvarchar(128),INVENTORY>');
```

> [!NOTE]
> Les noms de table et de colonne utilisent l’identificateur entre guillemets SQL ANSI lors de l’interrogation d’Oracle. Par conséquent, les noms respectent la casse. Il est important de spécifier le nom dans la définition de table externe qui correspond à la casse exacte des noms de table et de colonne dans les métadonnées Oracle.

## <a name="query-the-data"></a>Interroger les données

Exécutez la requête suivante pour associer les données de la table externe `iventory_ora` aux tables de la base de données `Sales` locale.

```sql
SELECT TOP(100) w.w_warehouse_name, i.inv_item, SUM(i.inv_quantity_on_hand) as total_quantity
  FROM [inventory_ora] as i
  JOIN item as it
    ON it.i_item_sk = i.inv_item
  JOIN warehouse as w
    ON w.w_warehouse_sk = i.inv_warehouse
 WHERE it.i_category = 'Books' and i.inv_item BETWEEN 1 and 18000 --> get items within specific range
 GROUP BY w.w_warehouse_name, i.inv_item;
```

## <a name="clean-up"></a>Nettoyer

Utilisez la commande suivante pour supprimer les objets de base de données créés dans ce tutoriel.

```sql
DROP EXTERNAL TABLE [inventory_ora];
DROP EXTERNAL DATA SOURCE [OracleSalesSrvr] ;
DROP DATABASE SCOPED CREDENTIAL [OracleCredential];
```

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment ingérer des données dans le pool de données :
> [!div class="nextstepaction"]
> [Charger des données dans le pool de données](tutorial-data-pool-ingest-sql.md)
