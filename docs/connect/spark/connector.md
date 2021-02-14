---
title: Exemple de connecteur Apache Spark pour SQL Server
description: Découvrez comment utiliser le connecteur Apache Spark pour SQL Server.
ms.custom: ''
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.openlocfilehash: 15a4bdb66ea87f85337540adfec4b5e175b1c55b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058124"
---
# <a name="apache-spark-connector-sql-server--azure-sql"></a>Connecteur Apache Spark : SQL Server et Azure SQL

Le Connecteur Apache Spark pour SQL Server et Azure SQL est un connecteur à hautes performances qui vous permet d’utiliser des données transactionnelles dans l’analytique du Big Data et de conserver les résultats pour des requêtes ad hoc ou des rapports. Le connecteur vous permet d’utiliser n’importe quelle base de données SQL, locale ou dans le cloud, comme source de données d’entrée ou récepteur de données de sortie pour les travaux Spark.

Cette bibliothèque contient le code source du connecteur Apache Spark pour SQL Server et Azure SQL.

[Apache Spark](https://spark.apache.org/) est un moteur d’analytique unifié pour le traitement des données à grande échelle.

Vous pouvez obtenir le connecteur à partir de [Maven](https://search.maven.org/search?q=spark-mssql-connector). Pour importer le connecteur, utilisez la coordonnée `com.microsoft.azure:spark-mssql-connector:<latest version>`. Vous pouvez également créer le connecteur à partir de la source ou télécharger le fichier jar à partir de la section Release dans GitHub. Pour obtenir les informations les plus récentes sur le connecteur, consultez le [Référentiel GitHub du connecteur SQL Spark](https://github.com/microsoft/sql-spark-connector).

## <a name="supported-features"></a>Fonctionnalités prises en charge

* Prise en charge de toutes les liaisons Spark (Scala, Python, R)
* Authentification de base et prise en charge des fichiers keytab Active Directory (AD)
* Prise en charge de l’écriture de `dataframe` réorganisée
* Prise en charge de l’écriture sur une instance SQL Server unique et un pool de données dans les clusters Big Data SQL Server
* Prise en charge fiable des connecteurs pour une instance SQL Server unique

| Composant                            | Versions prises en charge              |
|--------------------------------------|---------------------------------|
| Apache Spark                         | 2.4.5 (Spark 3.0 non pris en charge) |
| Scala                                | 2.11                            |
| Pilote Microsoft JDBC pour SQL Server | 8,2                             |
| Microsoft SQL Server                 | SQL Server 2008 ou version ultérieure        |
| Bases de données SQL Azure                  | Pris en charge                       |

> [!NOTE]
> L’utilisation d’Azure Synapse Analytics n’est pas testée avec ce connecteur. Il pourrait fonctionner, mais il peut y avoir des conséquences inattendues.

### <a name="supported-options"></a>Options prises en charge
Le connecteur Apache Spark pour SQL Server et Azure SQL prend en charge les options définies ici : [SQL DataSource JDBC](https://spark.apache.org/docs/latest/sql-data-sources-jdbc.html)

Par ailleurs, les options suivantes sont prises en charge

| Option | Default | Description |
| --------- | ------------------ | ------------------------------------------ |
| `reliabilityLevel` | `BEST_EFFORT` | `BEST_EFFORT` ou `NO_DUPLICATES`. `NO_DUPLICATES` implémente une insertion fiable dans les scénarios de redémarrage d’exécuteur |
| `dataPoolDataSource` | `none` | `none` implique que la valeur n’est pas définie et que le connecteur doit écrire sur l’instance unique de SQL Server. Définissez cette valeur sur le nom de la source de données pour écrire dans une table de pool de données dans Clusters Big Data|
| `isolationLevel` | `READ_COMMITTED` | Spécifier le niveau d’isolation |
| `tableLock` | `false` | Implémente une insertion avec l’option `TABLOCK` pour améliorer les performances en écriture |
| `schemaCheckEnabled` | `true` | Désactive la trame de données stricte et la vérification du schéma de table sql lorsque la valeur est false |

D’autres [options de copie en bloc](../jdbc/using-bulk-copy-with-the-jdbc-driver.md#sqlserverbulkcopyoptions) peuvent être définies en tant qu’options sur le `dataframe` et seront transmises aux API `bulkcopy` en écriture

## <a name="performance-comparison"></a>Comparaison entre les performances

Le connecteur Apache Spark pour SQL Server et Azure SQL est 15x plus rapide que le connecteur JDBC générique pour l’écriture sur SQL Server. Les caractéristiques de performances varient selon le type, le volume de données et les options utilisées, et peuvent présenter des variations d’exécution. Les résultats de performances suivants sont le temps nécessaire pour remplacer une table SQL avec 143,9 M lignes dans un `dataframe` Spark. Le `dataframe` Spark est construit en lisant la table HDFS `store_sales` générée à l’aide du [Benchmark TPCDS Spark](https://github.com/databricks/spark-sql-perf). Le temps de lecture de `store_sales` sur `dataframe` est exclu. La moyenne des résultats est calculée sur trois exécutions.

| Type de connecteur | Options | Description |  Durée d’écriture |
| --------- | ------------------ | -------------------------------------| ---------- |
| `JDBCConnector` | Par défaut | Connecteur JDBC générique avec les options par défaut |  1385 secondes |
| `sql-spark-connector` | `BEST_EFFORT` | Effort optimal de `sql-spark-connector` avec les options par défaut |580 secondes |
| `sql-spark-connector` | `NO_DUPLICATES ` | `sql-spark-connector` fiable | 709 secondes |
| `sql-spark-connector` | `BEST_EFFORT` + tabLock=true | Effort optimal de `sql-spark-connector` avec le verrouillage de table activé | 72 secondes |
| `sql-spark-connector` | `NO_DUPLICATES ` + tabLock=true| `sql-spark-connector` fiable avec verrouillage de table activé| 198 secondes |

Config

- Configuration Spark : num_executors = 20, executor_memory = '1664 m', executor_cores = 2
- Configuration de génération de données : scale_factor=50, partitioned_tables=true
- Fichier de données `store_sales` avec le nb de lignes 143 997 590

Environnement

- [Cluster Big Data SQL Server](../../big-data-cluster/release-notes-big-data-cluster.md) CU5
- `master` + 6 nœuds
- Chaque nœud serveur Gen 5 avec 512 Go de RAM, NVM 4 To par nœud, carte réseau 10 Go

## <a name="commonly-faced-issues"></a>Problèmes courants

### `java.lang.NoClassDefFoundError: com/microsoft/aad/adal4j/AuthenticationException`

Ce problème provient de l’utilisation d’une version antérieure du pilote MSSQL (qui est maintenant inclus dans ce connecteur) dans votre environnement Hadoop. Si vous utilisez le connecteur Azure SQL précédent et que vous avez installé manuellement des pilotes sur ce cluster pour la compatibilité Azure Active Directory, vous devrez supprimer ces pilotes.

Étapes pour résoudre le problème :

1. Si vous utilisez un environnement Hadoop générique, vérifiez et supprimez le fichier jar mssql : `rm $HADOOP_HOME/share/hadoop/yarn/lib/mssql-jdbc-6.2.1.jre7.jar`. Si vous utilisez Databricks, ajoutez un script global ou un script d’initialisation de cluster pour supprimer les anciennes versions du pilote mssql du dossier `/databricks/jars`, ou ajoutez cette ligne à un script existant : `rm /databricks/jars/*mssql*`
2. Ajoutez les packages `adal4j` et `mssql`. Par exemple, vous pouvez utiliser Maven, mais toute méthode devrait fonctionner. 

   > [!CAUTION]
   > N’installez pas le connecteur SQL Spark de cette manière.

1. Ajoutez la classe du pilote à votre configuration de connexion. Exemple :

   ```csharp
   connectionProperties = {
     `Driver`: `com.microsoft.sqlserver.jdbc.SQLServerDriver`
   }`
   ```

Pour plus d’informations et d’explications, consultez la résolution pour [https://github.com/microsoft/sql-spark-connector/issues/26](https://github.com/microsoft/sql-spark-connector/issues/26#issuecomment-672006339).

## <a name="get-started"></a>Découvrir

Le connecteur Apache Spark pour SQL Server et Azure SQL est basé sur l’API Spark DataSourceV1 et l’API de bloc SQL Server et utilise la même interface que le connecteur JDBC Spark-SQL intégré. Cette intégration vous permet d’intégrer facilement le connecteur et de migrer vos travaux Spark existants en mettant simplement à jour le paramètre de format avec `com.microsoft.sqlserver.jdbc.spark`.

Pour inclure le connecteur dans vos projets, téléchargez ce référentiel et générez le fichier jar à l’aide de SBT.

## <a name="write-to-a-new-sql-table"></a>Écrire dans une nouvelle table SQL

> [!WARNING]
> Le mode `overwrite` supprime d’abord la table si elle existe déjà dans la base de données par défaut. Utilisez cette option avec soin pour éviter les pertes de données inattendues.

Lorsque vous utilisez le mode `overwrite` et que vous n’utilisez pas l’option `truncate`, les index sont perdus lors de la recréation de la table. , une table ColumnStore serait maintenant un segment de mémoire. Si vous souhaitez conserver l’indexation existante, spécifiez également l’option `truncate` avec la valeur true. Par exemple : `.option("truncate","true")`.

```python
server_name = "jdbc:sqlserver://{SERVER_ADDR}"
database_name = "database_name"
url = server_name + ";" + "databaseName=" + database_name + ";"

table_name = "table_name"
username = "username"
password = "password123!#" # Please specify password here

try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("overwrite") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

### <a name="append-to-sql-table"></a>Ajouter à une table SQL

```python
try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("append") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

## <a name="specify-the-isolation-level"></a>Spécifier le niveau d’isolation

Par défaut, ce connecteur utilise le niveau d’isolation `READ_COMMITTED` lors de l’insertion en bloc dans la base de données. Si vous souhaitez remplacer le niveau d’isolation, utilisez l’option `mssqlIsolationLevel` comme indiqué ci-dessous.

```python
    .option("mssqlIsolationLevel", "READ_UNCOMMITTED") \
```

## <a name="read-from-sql-table"></a>Lire à partir d’une table SQL

```python
jdbcDF = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("user", username) \
        .option("password", password).load()
```

## <a name="azure-active-directory-authentication"></a>Authentification Azure Active Directory

### <a name="python-example-with-service-principal"></a>Exemple Python avec un principal de service

```python
context = adal.AuthenticationContext(authority)
token = context.acquire_token_with_client_credentials(resource_app_id_url, service_principal_id, service_principal_secret)
access_token = token["accessToken"]

jdbc_db = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("accessToken", access_token) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

### <a name="python-example-with-active-directory-password"></a>Exemple Python avec mot de passe Active Directory

```python
jdbc_df = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("authentication", "ActiveDirectoryPassword") \
        .option("user", user_name) \
        .option("password", password) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

Une dépendance requise doit être installée pour pouvoir s’authentifier à l’aide d’Active Directory.

Pour **Scala,** l’artefact `_com.microsoft.aad.adal4j_` doit être installé.

Pour **Python,** la bibliothèque `_adal_` doit être installée.  Cela est possible via pip.

Pour obtenir des exemples, consultez les [Exemples de notebooks](https://github.com/microsoft/sql-spark-connector/tree/master/samples).

## <a name="support"></a>Support

Le connecteur Apache Spark pour Azure SQL et SQL Server est un projet open source. Ce connecteur n’est fourni avec aucun support technique de Microsoft. Pour les problèmes liés au connecteur, créez un problème dans le référentiel de ce projet. La communauté du connecteur est active et surveille les envois.

## <a name="next-steps"></a>Étapes suivantes

Visitez le [référentiel GitHub de SQL Spark](https://github.com/microsoft/sql-spark-connector).

Pour plus d’informations sur les niveaux d’isolement, consultez [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).
