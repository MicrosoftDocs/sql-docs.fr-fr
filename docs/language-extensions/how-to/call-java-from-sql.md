---
title: Appeler le runtime Java
titleSuffix: SQL Server Language Extensions
description: Découvrez comment appeler des classes Java depuis des procédures stockées SQL Server en utilisant l’extension de langage SQL Server.
author: dphansen
ms.author: davidph
ms.date: 06/25/2020
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 263979480f9cb4d4326560f0bb8bae5157cf777f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072709"
---
# <a name="how-to-call-the-java-runtime-in-sql-server-language-extensions"></a>Guide pratique pour appeler le runtime Java dans les extensions de langage SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Les [extensions de langage SQL Server](../language-extensions-overview.md) utilisent la procédure stockée système [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) en tant qu’interface pour appeler le runtime Java. 

Cet article décrit de manière pratique les détails relatifs à l’implémentation des classes et méthodes Java qui s’exécutent sur SQL Server.

## <a name="where-to-place-java-classes"></a>Où placer les classes Java

Il existe deux méthodes pour appeler des classes Java dans SQL Server :

1. Placez les fichiers **.class** ou **.jar** dans votre [classpath Java](#classpath). 

2. Chargez les classes compilées dans un fichier  **.jar** et les autres dépendances dans la base de données à l’aide de l’instruction DDL relative à la [bibliothèque externe](#external-library). 

> [!NOTE]
> En règle générale, utilisez de préférence des fichiers  **.jar** et non des fichiers **.class** individuels. Il s’agit d’une pratique courante en Java, qui rend l’expérience globale plus facile. Consultez également [Guide pratique pour créer un fichier jar à partir de fichiers class](create-a-java-jar-file-from-class-files.md).

<a name="classpath"></a>

## <a name="use-classpath"></a>Utiliser Classpath

### <a name="basic-principles"></a>Principes de base

Voici quelques principes de base liés à l’exécution de Java sur SQL Server.

* Les classes Java personnalisées compilées doivent exister dans les fichiers **.class** ou **.jar** de votre classpath Java. Le [paramètre CLASSPATH](#set-classpath) fournit le chemin des fichiers Java compilés. 

* La méthode Java que vous appelez doit être indiquée dans le paramètre **script** de la procédure stockée.

* Si la classe appartient à un package, le **packageName** doit être fourni.

* **params** permet de passer des paramètres à une classe Java. L’appel d’une méthode qui nécessite des arguments n’est pas pris en charge. Les paramètres sont donc le seul moyen de passer des valeurs d’argument à votre méthode. 

> [!NOTE]
> Cette remarque rappelle les opérations prises en charge et non prises en charge spécifiques à Java dans SQL Server 2019 version Release Candidate 1.
> * Dans la procédure stockée, les paramètres d’entrée sont pris en charge. Les paramètres de sortie ne le sont pas.

### <a name="call-java-class"></a>Appeler la classe Java

La procédure stockée système [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) est l’interface utilisée pour appeler le runtime Java. L’exemple suivant montre une procédure stockée système `sp_execute_external_script` qui utilise l’extension Java, et des paramètres qui permettent de spécifier le chemin, le script et votre code personnalisé.

> [!NOTE]
> Notez que vous n’avez pas besoin de définir la méthode à appeler. Par défaut, une méthode appelée **execute** est appelée. Cela signifie que vous devez suivre le [kit SDK d’extensibilité pour Java dans SQL Server](extensibility-sdk-java-sql-server.md), et implémenter une méthode execute dans votre classe Java.

```sql
DECLARE @param1 int
SET @param1 = 3

EXEC sp_execute_external_script
  @language = N'Java'
, @script = N'<packageName>.<ClassName>'
, @input_data_1 = N'<Input Query>'
, @param1 = @param1
```

<a name="set-classpath"></a>

### <a name="set-classpath"></a>Définir CLASSPATH

Une fois que vous avez compilé votre classe ou vos classes Java, et créé un fichier jar dans votre classpath Java, vous avez deux options pour fournir le classpath à l’extension Java de SQL Server :

1. Utiliser des bibliothèques externes

    L’option la plus simple consiste à permettre à SQL Server de trouver automatiquement vos classes, en créant des bibliothèques externes et en les faisant pointer vers un fichier jar. [Utiliser des bibliothèques externes pour Java](#external-library)

2. Inscrire une variable d’environnement système

    Vous pouvez créer une variable d’environnement système, et fournir les chemins du fichier jar qui contient les classes. Créez une variable d’environnement système appelée **CLASSPATH**.

<a name="external-library"></a>

## <a name="use-external-library"></a>Utiliser une bibliothèque externe

Dans SQL Server 2019 version Release Candidate 1, vous pouvez utiliser des bibliothèques externes pour le langage Java sur Windows et Linux. Vous pouvez compiler vos classes dans un fichier .jar, et charger le fichier .jar ainsi que d’autres dépendances dans la base de données à l’aide de l’instruction DDL [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

Exemple de chargement d’un fichier .jar avec une bibliothèque externe :

```sql 
CREATE EXTERNAL LIBRARY myJar
FROM (CONTENT = '<local path to .jar file>') 
WITH (LANGUAGE = 'Java'); 
GO
```

En créant une bibliothèque externe, SQL Server a automatiquement accès aux classes Java, et vous n’avez pas besoin de définir d’autorisations spéciales pour le classpath.

Exemple d’appel d’une méthode dans une classe à partir d’un package chargé en tant que bibliothèque externe :

```sql
EXEC sp_execute_external_script
  @language = N'Java'
, @script = N'MyPackage.MyCLass'
, @input_data_1 = N'SELECT * FROM MYTABLE'
with result sets ((column1 int))
```

Pour plus d’informations, consultez [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

## <a name="loopback-connection-to-sql-server"></a>Connexion de bouclage à SQL Server

Utilisez une connexion de bouclage pour vous reconnecter à SQL Server sur JDBC afin de lire ou d’écrire des données à partir de code Java exécuté dans `sp_execute_external_script`. Vous pouvez l’utiliser lorsque vous n’êtes pas en mesure d’utiliser les arguments **InputDataSet** et **OutputDataSet** de `sp_execute_external_script`.
Pour établir une connexion de bouclage sur Windows, utilisez l’exemple suivant :

```
jdbc:sqlserver://localhost:1433;databaseName=Adventureworks;integratedSecurity=true;
``` 

Pour établir une connexion de bouclage sur Linux, il est nécessaire de définir trois propriétés de connexion dans le certificat suivant pour le pilote JDBC :

[Client-Certificate-Authenication](https://github.com/microsoft/mssql-jdbc/wiki/Client-Certificate-Authentication-for-Loopback-Scenarios)


## <a name="next-steps"></a>Étapes suivantes

+ [Tutoriel : Rechercher une chaîne à l’aide d’expressions régulières en Java](../tutorials/search-for-string-using-regular-expressions-in-java.md)