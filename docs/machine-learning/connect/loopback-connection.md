---
title: Connexion de bouclage SQL dans Python et R
description: Découvrez comment utiliser une connexion de bouclage pour vous reconnecter à SQL Server sur ODBC afin de lire ou d’écrire des données à partir d’un script Python ou R exécuté à partir de sp_execute_external_script.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/20/2020
ms.topic: how-to
author: Aniruddh25
ms.author: anmunde
ms.reviewer: dphansen
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 49d62ab967f972d777a02659b5a87b9e11740604
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094566"
---
# <a name="loopback-connection-to-sql-server-from-a-python-or-r-script"></a>Connexion de bouclage à SQL Server à partir d’un script Python ou R
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Découvrez comment utiliser une connexion de bouclage avec [Machine Learning Services](../sql-server-machine-learning-services.md) pour vous reconnecter à SQL Server sur [ODBC](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md) afin de lire ou d’écrire des données à partir d’un script Python ou R exécuté à partir de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Vous pouvez l’utiliser lorsque vous n’êtes pas en mesure d’utiliser les arguments **InputDataSet** et **OutputDataSet** de `sp_execute_external_script`.

## <a name="connection-string"></a>Chaîne de connexion

Pour établir une connexion de bouclage, vous devez utiliser une chaîne de connexion correcte. Les arguments obligatoires courants sont le nom du [pilote ODBC](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md), l’adresse du serveur et le nom de la base de données.

### <a name="connection-string-on-windows"></a>Chaîne de connexion sur Windows

Pour l’authentification sur SQL Server sur Windows, le script Python ou R peut utiliser l’attribut de chaîne de connexion **Trusted_Connection** pour s’authentifier en tant qu’utilisateur identique à celui qui a exécuté le sp_execute_external_script.

Voici un exemple de chaîne de connexion de bouclage sur Windows :

``` 
"Driver=SQL Server;Server=.;Database=nameOfDatabase;Trusted_Connection=Yes;"
```

### <a name="connection-string-on-linux"></a>Chaîne de connexion sur Linux

Pour l’authentification sur SQL Server sur Linux, le script Python ou R doit utiliser les attributs **ClientCertificate** et **Clientkey** du pilote ODBC pour s’authentifier en tant qu’utilisateur identique à celui ayant exécuté `sp_execute_external_script`. Cela nécessite l’utilisation de la [dernière version du pilote ODBC](../../connect/odbc/download-odbc-driver-for-sql-server.md), la version 17.4.1.1.

Voici un exemple de chaîne de connexion de bouclage sur Linux :

```
"Driver=ODBC Driver 17 for SQL Server;Server=fe80::8012:3df5:0:5db1%eth0;Database=nameOfDatabase;ClientCertificate=file:/var/opt/mssql-extensibility/data/baeaac72-60b3-4fae-acfd-c50eff5d34a2/sqlsatellitecert.pem;ClientKey=file:/var/opt/mssql-extensibility/data/baeaac72-60b3-4fae-acfd-c50eff5d34a2/sqlsatellitekey.pem;TrustServerCertificate=Yes;Trusted_Connection=no;Encrypt=Yes"
```

L’adresse du serveur, l’emplacement du fichier de certificat client et l’emplacement du fichier de clé client sont uniques à chaque `sp_execute_external_script` et peuvent être obtenus à l’aide de l’API **rx_get_sql_loopback_connection_string ()** pour Python ou **rxGetSqlLoopbackConnectionString ()** pour R.

Pour plus d’informations sur les attributs de chaîne de connexion, consultez [Attributs et mots clés de chaîne de connexion et DSN](../../connect/odbc/dsn-connection-string-attribute.md#new-connection-string-keywords-and-connection-attributes) pour Microsoft ODBC Driver for SQL Server.

## <a name="generate-connection-string-with-revoscalepy-for-python"></a>Générer une chaîne de connexion avec revoscalepy pour Python

Vous pouvez utiliser l’API **rx_get_sql_loopback_connection_string()** dans [revoscalepy](../python/ref-py-revoscalepy.md) pour générer une chaîne de connexion correcte pour une connexion de bouclage dans un script Python.

Les arguments suivants sont acceptés :

| Argument | Description |
|-|-|
| name_of_database | Nom de la base de données vers laquelle la connexion doit être établie |
| odbc_driver | Nom du pilote ODBC |

### <a name="examples"></a>Exemples

Exemple pour SQL Server sur Windows :

```sql
EXECUTE sp_execute_external_script
@language = N'Python',
@script = N'
from revoscalepy import rx_get_sql_loopback_connection_string, RxSqlServerData, rx_data_step
loopback_connection_string = rx_get_sql_loopback_connection_string(odbc_driver="SQL Server", name_of_database="DBName")
print("Connection String:{0}".format(loopback_connection_string))
data_set = RxSqlServerData(sql_query = "select col1, col2 from tableName",
                           connection_string = loopback_connection_string)
OutputDataSet = rx_data_step(data_set)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

Exemple pour SQL Server sur Linux :

```sql
EXECUTE sp_execute_external_script
@language = N'Python',
@script = N'
from revoscalepy import rx_get_sql_loopback_connection_string, RxSqlServerData, rx_data_step
loopback_connection_string = rx_get_sql_loopback_connection_string(odbc_driver="ODBC Driver 17 for SQL Server",
                                                                   name_of_database="DBName")
print("Loopback Connection String:{0}".format(loopback_connection_string))
data_set = RxSqlServerData(sql_query = "select col1, col2 from tableName",
                           connection_string = loopback_connection_string)
OutputDataSet = rx_data_step(data_set)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

## <a name="generate-connection-string-with-revoscaler-for-r"></a>Générer une chaîne de connexion avec revoscalepy pour R

Vous pouvez utiliser l’API **rxGetSqlLoopbackConnectionString()** dans [RevoScaleR](../r/ref-r-revoscaler.md) pour générer une chaîne de connexion correcte pour une connexion de bouclage dans un script R.

Les arguments suivants sont acceptés :

| Argument | Description |
|-|-|
| nameOfDatabase | Nom de la base de données vers laquelle la connexion doit être établie |
| odbcDriver | Nom du pilote ODBC |

### <a name="examples"></a>Exemples

Exemple pour SQL Server sur Windows :

```sql
EXECUTE sp_execute_external_script
@language = N'R',
@script = N'
    loopbackConnectionString <- rxGetSqlLoopbackConnectionString(nameOfDatabase="DBName", odbcDriver ="SQL Server")
    print(paste("Connection String:", loopbackConnectionString))
    dataSet <- RxSqlServerData(sqlQuery = "select col1, col2 from tableName",
                               connectionString = loopbackConnectionString)
    OutputDataSet <- rxDataStep(dataSet)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

Exemple pour SQL Server sur Linux :

```sql
EXECUTE sp_execute_external_script
@language = N'R',
@script = N'
    loopbackConnectionString <-  rxGetSqlLoopbackConnectionString(nameOfDatabase="DBName", 
                                                                  odbcDriver ="ODBC Driver 17 for SQL Server")
    print(paste("Connection String:", loopbackConnectionString))
    dataSet <- RxSqlServerData(sqlQuery = "select col1, col2 from tableName", 
                               connectionString = loopbackConnectionString)
    OutputDataSet <- rxDataStep(dataSet)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

## <a name="next-steps"></a>Étapes suivantes

+ [Pilote Microsoft ODBC pour SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md)
+ [revoscalepy](../python/ref-py-revoscalepy.md)
+ [RevoScaleR](../r/ref-r-revoscaler.md)
