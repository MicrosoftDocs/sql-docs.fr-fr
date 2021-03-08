---
description: Présentation de la prise en charge de Java EE
title: Présentation de la prise en charge de Java EE | Microsoft Docs
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a9448b80-b7a3-49cf-8bb4-322c73676005
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6d9e6a008c4cf3e3e9030bf25dfe00e2f10852b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837506"
---
# <a name="understanding-java-ee-support"></a>Présentation de la prise en charge de Java EE

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Les sections suivantes expliquent comment le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] assure le support de la plateforme Java, d’Enterprise Edition (Java EE) et des fonctionnalités d’API JDBC 3.0 facultatives. Les exemples de code source fournis dans ce système d'aide constituent une bonne référence pour débuter avec ces fonctionnalités.  
  
En premier lieu, assurez-vous que votre environnement Java (JDK, JRE) inclut le package javax.sql. Il s'agit d'un package requis pour toute application JDBC qui utilise l'API facultative. JDK 1.5 et les versions ultérieures contiennent déjà ce package ; il n’est donc pas nécessaire de l’installer séparément.  
  
## <a name="driver-name"></a>Nom du pilote

Le nom de la classe du pilote est **com.microsoft.sqlserver.jdbc.SQLServerDriver**. Pour les pilotes JDBC Driver 4.1, 4.2 et 6.0, le pilote est contenu dans le fichier **sqljdbc.jar**, **sqljdbc4.jar**, **sqljdbc41.jar** ou **sqljdbc42.jar**.

Pour le pilote JDBC 6.2, le pilote est contenu dans **mssql-jdbc-6.2.2.jre7.jar** ou dans **mssql-jdbc-6.2.2.jre8.jar**.

Pour le pilote JDBC 6.4, le pilote est contenu dans **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** ou **mssql-jdbc-6.4.0.jre9.jar**.

Pour le pilote JDBC 7.0, le pilote est contenu dans **mssql-jdbc-7.0.0.jre8.jar** ou dans **mssql-jdbc-7.0.0.jre10.jar**.

Pour le pilote JDBC 7.2, le pilote est contenu dans **mssql-jdbc-7.2.2.jre8.jar** ou dans **mssql-jdbc-7.2.2.jre11.jar**.

Pour le pilote JDBC 7.4, le pilote est contenu dans **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** ou **mssql-jdbc-7.4.1.jre12.jar**.

Dans la version 8.2, le pilote JDBC est contenu dans **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** ou **mssql-jdbc-8.2.2.jre13.jar**.

Dans la version 8.4, le pilote JDBC est contenu dans **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** ou **mssql-jdbc-8.4.1.jre14.jar**.

Pour JDBC Driver 9.2, le pilote se trouve dans **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** ou **mssql-jdbc-9.2.1.jre15.jar**.

Le nom de la classe est utilisé chaque fois que le pilote est chargé avec la classe DriverManager JDBC et chaque fois que le nom de la classe du pilote est spécifié dans n’importe quelle configuration de pilote. Par exemple, la configuration d'une source de données dans un serveur d'applications Java EE peut imposer d’entrer le nom de la classe du pilote.  
  
## <a name="data-sources"></a>Sources de données

Le pilote JDBC assure la prise en charge des sources de données Java EE / JDBC 3.0. La classe [SQLServerXADataSource](../../connect/jdbc/reference/sqlserverxadatasource-class.md) du pilote JDBC est implémentée par `com.microsoft.sqlserver.jdbc.SQLServerXADataSource`.  
  
### <a name="datasource-names"></a>Noms des sources de données

Vous pouvez établir des connexions de base de données à l'aide de sources de données. Les sources de données disponibles avec le pilote JDBC sont décrites dans le tableau suivant :  
  
|Type DataSource|Nom de la classe et description|  
|---------------|--------------------------|  
|DataSource|`com.microsoft.sqlserver.jdbc.SQLServerDataSource` <br/> <br/> Source de données sans regroupement.|  
|ConnectionPoolDataSource|`com.microsoft.sqlserver.jdbc.SQLServerConnectionPoolDataSource` <br/> <br/> Source de données pour configurer des regroupements de connexions de serveur d'applications JAVA EE. Utilisée en général lorsque l'application s'exécute sur un serveur d'applications JAVA EE.|  
|XADataSource|`com.microsoft.sqlserver.jdbc.SQLServerXADataSource` <br/> <br/> Source de données pour configurer des sources de données JAVA EE XA. Utilisée en général lorsque l'application s'exécute sur un serveur d'applications JAVA EE et un gestionnaire de transactions XA.|  
  
### <a name="data-source-properties"></a>Propriétés de la source de données

Toutes les sources de données prennent en charge la capacité à définir et à obtenir toute propriété associée à l'ensemble des propriétés du pilote sous-jacent.  
  
Exemples :  
  
`setServerName("localhost");`  
`setDatabaseName("AdventureWorks");`  
  
L'exemple suivant montre comment une application se connecte en utilisant une source de données :  

```java
//initialize JNDI ..  
Context ctx = new InitialContext(System.getProperties());
...
DataSource ds = (DataSource) ctx.lookup("MyDataSource");
Connection c = ds.getConnection("user", "pwd");  
```

Pour plus d'informations sur les propriétés de la source de données, consultez [Définition des propriétés de la source de données](../../connect/jdbc/setting-the-data-source-properties.md).  
  
## <a name="see-also"></a>Voir aussi

[Présentation du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
