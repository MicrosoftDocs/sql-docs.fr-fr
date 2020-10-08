---
description: Utilisation du type de données SQL_variant
title: Utilisation du type de données sql_variant | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 198c8a21fcea9a1386effe8d30c8d954180d6dc5
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91727482"
---
# <a name="using-sql_variant-data-type"></a>Utilisation du type de données SQL_variant

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

À partir de la version 6.3.0, le pilote JDBC prend en charge le type de données sql_variant. sql_variant est également pris en charge lors de l’utilisation de fonctionnalités telles que les paramètres table et BulkCopy avec certaines limitations mentionnées plus loin dans cette page. Tous les types de données ne peuvent pas être stockés dans le type de données sql_variant. Pour obtenir la liste des types de données pris en charge avec sql_variant, consultez la [Documentation](../../t-sql/data-types/sql-variant-transact-sql.md) de SQL Server.

##  <a name="populating-and-retrieving-a-table"></a>Remplissage et extraction d’une table :
En supposant qu’il existe une table avec une colonne sql_variant comme suit :

```sql
CREATE TABLE sampleTable (col1 sql_variant)  
```

Exemple de script pour insérer des valeurs à l’aide d’une instruction :

```java
try (Statement stmt = connection.createStatement()){
    stmt.execute("insert into sampleTable values (1)");
}
```

Insertion d’une valeur à l’aide d’une instruction préparée :

```java
try (PreparedStatement preparedStatement = con.prepareStatement("insert into sampleTable values (?)")) {
    preparedStatement.setObject(1, 1);  
    preparedStatement.execute();
}
```      

Si le type sous-jacent des données passées est connu, vous pouvez utiliser la méthode setter respective. Par exemple, vous pouvez utiliser `preparedStatement.setInt()` lors de l’insertion d’une valeur entière.

```java
try (PreparedStatement preparedStatement = con.prepareStatement("insert into table values (?)")) {
    preparedStatement.setInt (1, 1);
    preparedStatement.execute();
}
```

Pour la lecture des valeurs à partir de la table, les getters respectifs peuvent être utilisés. Par exemple, les méthodes `getInt()` ou `getString()` peuvent être utilisées si les valeurs provenant du serveur sont connues :    

```java
try (SQLServerResultSet resultSet = (SQLServerResultSet) stmt.executeQuery("select * from sampleTable ")) {
    resultSet.next();          
    resultSet.getInt(1); //or rs.getString(1); or rs.getObject(1);
}
```

## <a name="using-stored-procedures-with-sql_variant"></a>Utiliser des procédures stockées avec sql_variant :   
Disposer d’une procédure stockée telle que :     

```java
String sql = "CREATE PROCEDURE " + inputProc + " @p0 sql_variant OUTPUT AS SELECT TOP 1 @p0=col1 FROM sampleTable ";
``` 
    
Les paramètres de sortie doivent être enregistrés :

```java
try (CallableStatement callableStatement = con.prepareCall(" {call " + inputProc + " (?) }")) {
    callableStatement.registerOutParameter(1, microsoft.sql.Types.SQL_VARIANT);      
    callableStatement.execute();
}
```

## <a name="limitations-of-sql_variant"></a>Limitations de sql_variant :
- Quand vous utilisez TVP pour remplir une table avec une valeur stockée `datetime`/`smalldatetime`/`date` dans un sql_variant, l’appel de `getDateTime()`/`getSmallDateTime()`/`getDate()` sur un ResultSet ne fonctionne pas et lève l’exception suivante :
    
    `Java.lang.String cannot be cast to java.sql.Timestamp`
   
    Solution de contournement : utilisez `getString()` ou `getObject()` à la place. 
    
- L’utilisation de TVP pour remplir une table et l’envoi d’une valeur null dans un sql_variant n’est pas prise en charge et lève une exception :
    
    `Inserting null value with column type sql_variant in TVP is not supported.`

## <a name="see-also"></a>Voir aussi

[Présentation des types de données du pilote JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md)