---
title: Utilisation du connecteur Apache Spark pour SQL Server et Azure SQL
titleSuffix: SQL Server big data clusters
description: Découvrez comment utiliser le connecteur Apache Spark pour SQL Server et Azure SQL pour lire et écrire dans SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 29a660452841ee0057485c1a44c8ff30abd6f230
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045589"
---
# <a name="use-the-apache-spark-connector-for-sql-server-and-azure-sql"></a>Utiliser le connecteur Apache Spark pour SQL Server et Azure SQL

Le [Connecteur Apache Spark pour SQL Server et Azure SQL](https://github.com/microsoft/sql-spark-connector) est un connecteur à hautes performances qui vous permet d’utiliser des données transactionnelles dans l’analytique du Big Data et de conserver les résultats pour des requêtes ad hoc ou des rapports. Le connecteur vous permet d’utiliser n’importe quelle base de données SQL, locale ou dans le cloud, comme source de données d’entrée ou récepteur de données de sortie pour les travaux Spark. Le connecteur utilise les API d’écriture en bloc SQL Server. Tous les paramètres d’écriture en bloc peuvent être passés en tant que paramètres facultatifs par l’utilisateur et sont passés en l’état par le connecteur à l’API sous-jacente. Pour plus d’informations sur les opérations d’écriture en bloc, consultez [Utilisation de la copie en bloc avec le pilote JDBC]( ../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md#sqlserverbulkcopyoptions).

Le connecteur est inclus par défaut dans les clusters Big Data SQL Server.

Pour en savoir plus sur le connecteur, consultez le [référentiel open source](https://github.com/microsoft/sql-spark-connector). Pour obtenir des exemples, consultez [Exemples](https://github.com/microsoft/sql-spark-connector/tree/master/samples).

## <a name="write-to-a-new-sql-table"></a>Écrire dans une nouvelle table SQL

>[!CAUTION]
> En mode `overwrite`, le connecteur supprime d’abord la table si elle existe déjà dans la base de données par défaut. Utilisez cette option avec soin pour éviter les pertes de données inattendues.
> 
> Lorsque vous utilisez le mode `overwrite` et que vous n’utilisez pas l’option `truncate`, les index sont perdus lors de la recréation de la table. Par exemple, une table ColumnStore devient un tas. Si vous souhaitez conserver l’indexation existante, spécifiez également l’option `truncate` avec la valeur `true`. Par exemple `.option("truncate",true)`

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

## <a name="append-to-sql-table"></a>Ajouter à une table SQL
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

Par défaut, ce connecteur utilise le niveau d’isolation READ_COMMITTED lors de l’insertion en bloc dans la base de données. Si vous souhaitez remplacer cela par un autre niveau d’isolation, utilisez l’option `mssqlIsolationLevel` comme indiqué ci-dessous.
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

## <a name="non-active-directory-mode"></a>Mode non Active Directory

En mode de sécurité non Active Directory, chaque utilisateur dispose d’un nom d’utilisateur et d’un mot de passe qui doivent être fournis en tant que paramètres pendant l’instanciation du connecteur afin d’effectuer des opérations de lecture et/ou d’écriture.

Voici un exemple d’instanciation de connecteur pour le mode non Active Directory. Avant d’exécuter le script, remplacez `?` par la valeur de votre compte.

```python
# Note: '?' is a placeholder for a necessary user-specified value
connector_type = "com.microsoft.sqlserver.jdbc.spark" 

url = "jdbc:sqlserver://master-p-svc;databaseName=?;"
writer = df.write \ 
   .format(connector_type)\ 
   .mode("overwrite") 
   .option("url", url) \ 
   .option("user", ?) \ 
   .option("password",?) 
writer.save() 
```

## <a name="active-directory-mode"></a>Mode Active Directory

En mode de sécurité Active Directory, une fois que l’utilisateur a généré un fichier keytab, il doit fournir `principal` et `keytab` en tant que paramètres lors de l’instanciation du connecteur.

Dans ce mode, le pilote charge le fichier keytab dans les conteneurs d’exécuteur respectifs. Ensuite, les exécuteurs utilisent le nom principal et le keytab pour générer un jeton qui est utilisé afin de créer un connecteur JDBC pour la lecture et l’écriture de données.

Voici un exemple d’instanciation de connecteur pour le mode Active Directory. Avant d’exécuter le script, remplacez `?` par la valeur de votre compte.

```python
# Note: '?' is a placeholder for a necessary user-specified value
connector_type = "com.microsoft.sqlserver.jdbc.spark"

url = "jdbc:sqlserver://master-p-svc;databaseName=?;integratedSecurity=true;authenticationScheme=JavaKerberos;" 
writer = df.write \ 
   .format(connector_type)\ 
   .mode("overwrite") 
   .option("url", url) \ 
   .option("principal", ?) \ 
   .option("keytab", ?)   

writer.save() 
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les clusters Big Data, consultez [Guide pratique pour déployer des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md)

Vous avez des commentaires ou des recommandations concernant les fonctionnalités des clusters Big Data SQL Server ? [Faites-nous part de vos commentaires sur les clusters Big Data SQL Server](https://aka.ms/sql-server-bdc-feedback).
