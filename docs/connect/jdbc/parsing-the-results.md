---
description: Analyse des résultats
title: Analyse des résultats | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: rene-ye
ms.author: v-reye
manager: kenvh
ms.openlocfilehash: 7b56223594d1126c373b4fd8b24dabc1e81dab67
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837078"
---
# <a name="parsing-the-results"></a>Analyse des résultats

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Cet article explique comment SQL Server s’attend à ce que les utilisateurs traitent entièrement les résultats renvoyés par une requête.

## <a name="update-counts-and-result-sets"></a>Compteurs de mises à jour et de jeux de résultats

Cette section présente les deux résultats les plus courants renvoyés par SQL Server : Les compteurs de mises à jour et de jeux de résultats. En général, toute requête exécutée par un utilisateur entraîne le retour d’un de ces résultats. Les utilisateurs sont censés gérer les deux lors du traitement des résultats.

Le code suivant est un exemple de la façon dont un utilisateur peut itérer à travers tous les résultats du serveur :
```java
try (Connection connection = DriverManager.getConnection(URL); Statement statement = connection.createStatement()) {
    boolean resultsAvailable = statement.execute(USER_SQL);
    int updateCount = -2;
    while (true) {
        updateCount = statement.getUpdateCount();
        if (!resultsAvailable && updateCount == -1)
            break;
        if (resultsAvailable) {
            // handle ResultSet
        } else {
            // handle Update Count
        }
        resultsAvailable = statement.getMoreResults();
    }
}
```

## <a name="exceptions"></a>Exceptions
Lorsque vous exécutez une instruction qui génère une erreur ou un message d’information, SQL Server répond différemment selon qu’il peut ou non générer un plan d’exécution. Le message d’erreur peut être levé immédiatement après l’exécution de l’instruction, ou il peut nécessiter un jeu de résultats distinct. Dans ce dernier cas, les applications doivent analyser le jeu de résultats pour récupérer l’exception.

Lorsque SQL Server ne parvient pas à générer un plan d’exécution, l’exception est levée immédiatement.

```java
String SQL = "SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    statement.execute(SQL);
} catch (SQLException e) {
    e.printStackTrace();
}
```

Lorsque SQL Server renvoie un message d’erreur dans un jeu de résultats, le jeu de résultats doit être traité pour récupérer l’exception.

```java
String SQL = "SELECT 1/0;";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    if (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            // Exception is thrown on next().
            while (rs.next()) {}
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

Si l’exécution de l’instruction génère plusieurs jeux de résultats, chaque jeu de résultats doit être traité jusqu’à ce que celui avec l’exception soit atteint.

```java
String SQL = "SELECT 1; SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    // Does not throw an exception on execute().
    boolean hasResult = statement.execute(SQL);
    while (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            while (rs.next()) {
                System.out.println(rs.getString(1));
            }
        }
        // Moves the next result set that generates the exception.
        hasResult = statement.getMoreResults();
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

Dans le cas de `String SQL = "SELECT * FROM nonexistentTable; SELECT 1;";`, une exception est levée immédiatement sur `execute()`, et `SELECT 1` n’est pas exécuté du tout.

Si l’erreur de SQL Server a une gravité de `0` à `9`, elle est considérée comme un message d’information et renvoyée comme `SQLWarning`.

```java
String SQL = "RAISERROR ('WarningLevel5', 5, 2);";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    SQLWarning warning = statement.getWarnings();
    System.out.println(warning);
}
```

## <a name="see-also"></a>Voir aussi

[Présentation du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)
