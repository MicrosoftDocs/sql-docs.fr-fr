---
title: API de copie en bloc pour l'insertion dans JDBC
description: Le pilote Microsoft JDBC pour SQL Server prend en charge l’utilisation de la copie en bloc pour les opérations d’insertion par lot afin de charger les données plus rapidement dans la base de données.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4a769d73f799b8ca0b4b806a3e656517377e23ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161568"
---
# <a name="using-bulk-copy-api-for-batch-insert-operation"></a>Utilisation de l'API de copie en bloc pour l'opération d'insertion par lot

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Le pilote Microsoft JDBC pour SQL Server version 9.2 et ultérieure prend en charge l’utilisation de l’API de copie en bloc pour les opérations d’insertion par lot. Cette fonctionnalité permet aux utilisateurs d’autoriser le pilote à effectuer une opération de copie en bloc sous-jacente lors de l’exécution des opérations d’insertion par lot. Le pilote vise à améliorer les performances lors de l’insertion des mêmes données que celles que le pilote aurait insérées avec l’opération d’insertion de lot normale. Le pilote analyse la requête SQL de l’utilisateur en utilisant l’API de copie en bloc au lieu de l’opération d’insertion par lot habituelle. Vous trouverez ci-dessous différentes façons d’activer l’API de copie en bloc pour la fonctionnalité d’insertion par lot et la liste de ses limitations. Cette page contient également un petit exemple de code qui illustre une utilisation et l’amélioration des performances associée.

Cette fonctionnalité s’applique uniquement aux API `executeBatch()` & `executeLargeBatch()` de PreparedStatement et de CallableStatement.

## <a name="prerequisites"></a>Prérequis

Prérequis à l’activation de l’API de copie en bloc pour l’insertion par lot.

* La requête doit être une requête d’insertion (la requête peut contenir des commentaires, mais la requête doit commencer par le mot clé INSERT pour que cette fonctionnalité soit prise en compte).

## <a name="enabling-bulk-copy-api-for-batch-insert"></a>Activation de l'API de copie en bloc pour l'insertion de lot

Il existe trois façons d’activer l’API de copie en bloc pour l’insertion de lot.

### <a name="1-enabling-with-connection-property"></a>1. Activation avec la propriété de connexion

L’ajout de **useBulkCopyForBatchInsert=true;** à la chaîne de connexion active cette fonctionnalité.

```java
Connection connection = DriverManager.getConnection("jdbc:sqlserver://<server>:<port>;userName=<user>;password=<password>;database=<database>;useBulkCopyForBatchInsert=true;");
```

### <a name="2-enabling-with-setusebulkcopyforbatchinsert-method-from-sqlserverconnection-object"></a>2. Activation avec la méthode setUseBulkCopyForBatchInsert() à partir de l’objet SQLServerConnection

L’appel de **SQLServerConnection.setUseBulkCopyForBatchInsert(true)** active cette fonctionnalité.

**SQLServerConnection.getUseBulkCopyForBatchInsert()** récupère la valeur actuelle de la propriété de connexion **useBulkCopyForBatchInsert**.

La valeur de **useBulkCopyForBatchInsert** reste constante pour chaque PreparedStatement au moment de son initialisation. Tout appel ultérieur à **SQLServerConnection.setUseBulkCopyForBatchInsert()** n’affectera pas la valeur de la PreparedStatement déjà créée.

### <a name="3-enabling-with-setusebulkcopyforbatchinsert-method-from-sqlserverdatasource-object"></a>3. Activation avec la méthode setUseBulkCopyForBatchInsert() à partir de l’objet SQLServerDataSource

Semblable à la méthode ci-dessus, mais cette fois à l’aide de SQLServerDataSource pour créer un objet SQLServerConnection. Les deux méthodes permettent d’obtenir le même résultat.

## <a name="known-limitations"></a>Limitations connues

Ces limitations s’appliquent actuellement à cette fonctionnalité.

* Les requêtes INSERT qui contiennent des valeurs non paramétrables (par exemple, `INSERT INTO TABLE VALUES (?, 2`) ne sont pas prises en charge. Les caractères génériques (?) sont les seuls paramètres pris en charge pour cette fonction.
* Les requêtes INSERT qui contiennent des expressions INSERT-SELECT (par exemple `INSERT INTO TABLE SELECT * FROM TABLE2`) ne sont pas prises en charge.
* Les requêtes INSERT qui contiennent plusieurs expressions VALUE (par exemple `INSERT INTO TABLE VALUES (1, 2) (3, 4)`) ne sont pas prises en charge.
* Les requêtes INSERT qui sont suivies de la clause OPTION, jointes à plusieurs tables ou suivies d’une autre requête, ne sont pas prises en charge.
* En raison des limitations de l’API de copie en bloc, les types de données `MONEY`, `SMALLMONEY`, `DATE`, `DATETIME`, `DATETIMEOFFSET`, `SMALLDATETIME`, `TIME`, `GEOMETRY` et `GEOGRAPHY` ne sont actuellement pas pris en charge pour cette fonctionnalité.

Si la requête échoue en raison d’erreurs non liées à « SQL Server », le pilote consigne le message d’erreur et bascule sur la logique d’origine pour l’insertion de lot.

## <a name="example"></a>Exemple

Voici un exemple de code qui montre le cas d’usage d’une opération d’insertion par lot d’un millier de lignes pour le scénario normal et pour le scénario de l’API de copie en bloc.

```java
    public static void main(String[] args) throws Exception
    {
        String tableName = "batchTest";
        String tableNameBulkCopyAPI = "batchTestBulk";

        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<database>;user=<user>;password=<password>";

        try (Connection con = DriverManager.getConnection(connectionUrl);
                Statement stmt = con.createStatement();
                PreparedStatement pstmt = con.prepareStatement("insert into " + tableName + " values (?, ?)");) {

            String dropSql = "if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[" + tableName + "]') and OBJECTPROPERTY(id, N'IsUserTable') = 1) DROP TABLE [" + tableName + "]";
            stmt.execute(dropSql);

            String createSql = "create table " + tableName + " (c1 int, c2 varchar(20))";
            stmt.execute(createSql);

            System.out.println("Starting batch operation using regular batch insert operation.");
            long start = System.currentTimeMillis();
            for (int i = 0; i < 1000; i++) {
                pstmt.setInt(1, i);
                pstmt.setString(2, "test" + i);
                pstmt.addBatch();
            }
            pstmt.executeBatch();

            long end = System.currentTimeMillis();

            System.out.println("Finished. Time taken : " + (end - start) + " milliseconds.");
        }

        try (Connection con = DriverManager.getConnection(connectionUrl + ";useBulkCopyForBatchInsert=true");
                Statement stmt = con.createStatement();
                PreparedStatement pstmt = con.prepareStatement("insert into " + tableNameBulkCopyAPI + " values (?, ?)");) {

            String dropSql = "if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[" + tableNameBulkCopyAPI + "]') and OBJECTPROPERTY(id, N'IsUserTable') = 1) DROP TABLE [" + tableNameBulkCopyAPI + "]";
            stmt.execute(dropSql);

            String createSql = "create table " + tableNameBulkCopyAPI + " (c1 int, c2 varchar(20))";
            stmt.execute(createSql);

            System.out.println("Starting batch operation using Bulk Copy API.");
            long start = System.currentTimeMillis();
            for (int i = 0; i < 1000; i++) {
                pstmt.setInt(1, i);
                pstmt.setString(2, "test" + i);
                pstmt.addBatch();
            }
            pstmt.executeBatch();

            long end = System.currentTimeMillis();

            System.out.println("Finished. Time taken : " + (end - start) + " milliseconds.");
        }
    }
```

Résultat :

```bash
Starting batch operation using regular batch insert operation.
Finished. Time taken : 104132 milliseconds.
Starting batch operation using Bulk Copy API.
Finished. Time taken : 1058 milliseconds.
```

## <a name="see-also"></a>Voir aussi

[Amélioration des performances et de la fiabilité avec le pilote JDBC](improving-performance-and-reliability-with-the-jdbc-driver.md)
