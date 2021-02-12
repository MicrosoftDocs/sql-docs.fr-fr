---
title: 'Tutoriel : Recherche de chaînes d’expressions régulières dans Java'
description: Ce tutoriel vous montre comment utiliser les extensions de langage SQL Server et comment exécuter du code Java qui recherche une chaîne avec des expressions régulières (regex).
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 6502be49b2076e8f6d89bdd88695f24b65dc79ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075180"
---
# <a name="tutorial-search-for-a-string-using-regular-expressions-regex-in-java"></a>Tutoriel : Rechercher une chaîne à l’aide d’expressions régulières (regex) dans Java
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Ce tutoriel vous montre comment utiliser les [extensions de langage SQL Server](../language-extensions-overview.md) pour créer une classe Java qui reçoit deux colonnes (ID et text) de SQL Server et une expression régulière (regex) comme paramètre d’entrée. La classe retourne deux colonnes à SQL Server (ID et text).

Pour un texte donné dans la colonne de texte envoyée à la classe Java, le code vérifie si l’expression régulière donnée est satisfaite, puis retourne ce texte avec l’ID d’origine.

Cet exemple de code utilise une expression régulière qui vérifie si un texte contient le mot « Java » ou « java ».

## <a name="prerequisites"></a>Prérequis

+ Instance de moteur de base de données SQL Server 2019 avec framework d’extensibilité et extension de programmation Java [sur Windows](../install/windows-java.md) ou [sur Linux](../../linux/sql-server-linux-setup-language-extensions-java.md). Pour plus d’informations, consultez [Extension de langage dans SQL Server 2019](../language-extensions-overview.md). Pour plus d’informations sur les exigences en matière de codage, consultez [Comment appeler Java dans SQL Server](../how-to/call-java-from-sql.md).

+ SQL Server Management Studio ou Azure Data Studio pour exécuter le code T-SQL.

+ Java SE Development Kit (JDK) 8 ou JRE 8 sur Windows ou Linux.

+ Fichier **mssql-java-lang-extension.jar** du [kit SDK d’extensibilité Microsoft pour Java pour Microsoft SQL Server](../how-to/extensibility-sdk-java-sql-server.md).

Pour ce tutoriel, la compilation à partir de la ligne de commande avec **javac** suffit.

## <a name="create-sample-data"></a>Créer des exemples de données

Commencez par créer une base de données et remplissez une table **testdata** avec les colonnes **ID** et **text**.

```sql
CREATE DATABASE javatest
GO

USE javatest
GO

CREATE TABLE testdata (
    id int NOT NULL,
    "text" nvarchar(100) NOT NULL
)
GO

-- Insert data into test table
INSERT INTO testdata(id, "text") VALUES (1, 'This sentence contains java')
INSERT INTO testdata(id, "text") VALUES (2, 'This sentence does not')
INSERT INTO testdata(id, "text") VALUES (3, 'I love Java!')
GO
```

## <a name="create-the-main-class"></a>Créer la classe principale

Dans cette étape, créez un fichier de classe appelé **RegexSample.java** et copiez le code Java suivant dans ce fichier.

Cette classe principale importe le kit SDK, ce qui signifie que le fichier jar téléchargé à l’étape 1 doit être détectable à partir de cette classe.

```java
package pkg;

import com.microsoft.sqlserver.javalangextension.PrimitiveDataset;
import com.microsoft.sqlserver.javalangextension.AbstractSqlServerExtensionExecutor;
import java.util.LinkedHashMap;
import java.util.LinkedList;
import java.util.ListIterator;
import java.util.regex.*;

public class RegexSample extends AbstractSqlServerExtensionExecutor {
    private Pattern expr;

    public RegexSample() {
        // Setup the expected extension version, and class to use for input and output dataset
        executorExtensionVersion = SQLSERVER_JAVA_LANG_EXTENSION_V1;
        executorInputDatasetClassName = PrimitiveDataset.class.getName();
        executorOutputDatasetClassName = PrimitiveDataset.class.getName();
    }
    
    public PrimitiveDataset execute(PrimitiveDataset input, LinkedHashMap<String, Object> params) {
        // Validate the input parameters and input column schema
        validateInput(input, params);

        int[] inIds = input.getIntColumn(0);
        String[] inValues = input.getStringColumn(1);
        int rowCount = inValues.length;

        String regexExpr = (String)params.get("regexExpr");
        expr = Pattern.compile(regexExpr);

        System.out.println("regex expression: " + regexExpr);

        // Lists to store the output data
        LinkedList<Integer> outIds = new LinkedList<Integer>();
        LinkedList<String> outValues = new LinkedList<String>();

        // Evaluate each row
        for(int i = 0; i < rowCount; i++) {
            if (check(inValues[i])) {
                outIds.add(inIds[i]);
                outValues.add(inValues[i]);
            }
        }

        int outputRowCount = outValues.size();

        int[] idOutputCol = new int[outputRowCount];
        String[] valueOutputCol = new String[outputRowCount];

        // Convert the list of output columns to arrays
        outValues.toArray(valueOutputCol);

        ListIterator<Integer> it = outIds.listIterator(0);
        int rowId = 0;

        System.out.println("Output data:");
        while (it.hasNext()) {
            idOutputCol[rowId] = it.next().intValue();

            System.out.println("ID: " + idOutputCol[rowId] + " Value: " + valueOutputCol[rowId]);
            rowId++;
        }

        // Construct the output dataset
        PrimitiveDataset output = new PrimitiveDataset();

        output.addColumnMetadata(0, "ID", java.sql.Types.INTEGER, 0, 0);
        output.addColumnMetadata(1, "Text", java.sql.Types.NVARCHAR, 0, 0);

        output.addIntColumn(0, idOutputCol, null);
        output.addStringColumn(1, valueOutputCol);

        return output;
    }

    private void validateInput(PrimitiveDataset input, LinkedHashMap<String, Object> params) {
        // Check for the regex expression input parameter
        if (params.get("regexExpr") == null) {
            throw new IllegalArgumentException("Input parameter 'regexExpr' is not found");
        }

        // The expected input schema should be at least 2 columns, (INTEGER, STRING)
        if (input.getColumnCount() < 2) {
            throw new IllegalArgumentException("Unexpected input schema, schema should be an (INTEGER, NVARCHAR or VARCHAR)");
        }

        // Check that the input column types are expected
        if (input.getColumnType(0) != java.sql.Types.INTEGER &&
                (input.getColumnType(1) != java.sql.Types.VARCHAR && input.getColumnType(1) == java.sql.Types.NVARCHAR )) {
            throw new IllegalArgumentException("Unexpected input schema, schema should be an (INTEGER, NVARCHAR or VARCHAR)");
        }
    }

    private boolean check(String text) {
        Matcher m = expr.matcher(text);

        return m.find();
    }
}
```

## <a name="compile-and-create-a-jar-file"></a>Compiler et créer un fichier .jar

Empaquetez vos classes et dépendances dans des fichiers `.jar`. La plupart des IDE Java (par exemple, Eclipse ou IntelliJ) prennent en charge la génération de fichiers `.jar` quand vous générez ou compilez le projet. Donnez au fichier `.jar` le nom **regex.jar**.

Si vous n’utilisez pas un IDE Java, vous pouvez créer manuellement un fichier `.jar`. Pour plus d’informations, consultez le [guide pratique pour créer un fichier jar Java à partir de fichiers de classe](../how-to/create-a-java-jar-file-from-class-files.md).

> [!NOTE]
> Ce tutoriel utilise des packages. La ligne `package pkg;` en haut de la classe garantit que le code compilé est enregistré dans un sous-dossier appelé **pkg**. Si vous utilisez un IDE, le code compilé est automatiquement enregistré dans ce dossier. Si vous utilisez **javac** pour compiler manuellement les classes, vous devez placer le code compilé dans le dossier **pkg**.

## <a name="create-external-language"></a>Créer un langage externe

Vous devez créer un langage externe dans la base de données. Le langage externe est un objet délimité à une base de données, ce qui signifie qu’un langage externe comme Java doit être créé pour chaque base de données où vous souhaitez l’utiliser.

### <a name="create-external-language-on-windows"></a>Créer un langage externe sur Windows

Si vous utilisez Windows, suivez les étapes ci-dessous pour créer un langage externe pour Java.

1. Créez un fichier .zip contenant l’extension.

    Dans le cadre de l’installation de SQL Server sur Windows, le fichier **.zip** de l’extension Java est enregistré à cet emplacement : `[SQL Server install path]\MSSQL\Binn\java-lang-extension.zip`. Ce fichier .zip contient le fichier **javaextension.dll**.

2. Créez un langage externe pour Java à partir du fichier .zip :

    ```sql
    CREATE EXTERNAL LANGUAGE Java
    FROM
    (CONTENT = N'[SQL Server install path]\MSSQL\Binn\java-lang-extension.zip', FILE_NAME = 'javaextension.dll',
    ENVIRONMENT_VARIABLES = N'{"JRE_HOME":"<path to JRE>"}' );
    GO
    ```

### <a name="create-external-language-on-linux"></a>Créer un langage externe sur Linux

Dans le cadre de l’installation, le fichier **.tar.gz** de l’extension est enregistré dans le chemin suivant : `/opt/mssql-extensibility/lib/java-lang-extension.tar.gz`.

Pour créer un langage externe pour Java, exécutez l’instruction T-SQL suivante sur Linux :

```sql
CREATE EXTERNAL LANGUAGE Java
FROM (CONTENT = N'/opt/mssql-extensibility/lib/java-lang-extension.tar.gz', file_name = 'javaextension.so',
ENVIRONMENT_VARIABLES = N'{"JRE_HOME":"<path to JRE>"}' );
GO
```

### <a name="permissions-to-execute-external-language"></a>Autorisations pour exécuter le langage externe

Pour exécuter du code Java, un utilisateur doit avoir l’autorisation d’exécuter un script externe sur ce langage spécifique.

Pour plus d’informations, consultez [CRÉER UN LANGAGE EXTERNE](../../t-sql/statements/create-external-language-transact-sql.md).

## <a name="create-external-libraries"></a>Créer des bibliothèques externes

Utilisez [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) pour créer une bibliothèque externe pour vos fichiers `.jar`. SQL Server ayant accès aux fichiers `.jar`, vous n’avez pas besoin de définir d’autorisations spéciales pour **classpath**.

Dans cet exemple, vous allez créer deux bibliothèques externes : une pour le kit SDK et une pour le code Java RegEx.

1. Le fichier jar du kit SDK (**mssql-java-lang-extension.jar**) est installé avec SQL Server 2019 sur Windows et Linux.

    + Chemin d’installation par défaut sur Windows : **[répertoire de base de l’installation de l’instance]\MSSQL\Binn\mssql-java-lang-extension.jar**

    + Chemin d’installation par défaut sur Linux : **/opt/mssql/lib/mssql-java-lang-extension.jar**

    Le code est open source et se trouve dans le [dépôt GitHub des extensions de langage SQL Server](https://github.com/microsoft/sql-server-language-extensions). Pour plus d’informations, consultez [SDK d’extensibilité Microsoft pour Java pour Microsoft SQL Server](../how-to/extensibility-sdk-java-sql-server.md).

2. Créez une bibliothèque externe pour le kit SDK.

    ```sql
    CREATE EXTERNAL LIBRARY sdk
    FROM (CONTENT = '<OS specific path from above>/mssql-java-lang-extension.jar')
    WITH (LANGUAGE = 'Java');
    GO
    ```

3. Créez une bibliothèque externe pour le code RegEx.

    ```sql
    CREATE EXTERNAL LIBRARY regex
    FROM (CONTENT = '<path>/regex.jar')
    WITH (LANGUAGE = 'Java');
    GO
    ```

## <a name="set-permissions"></a>Définir des autorisations

> [!NOTE]
> Ignorez cette étape si vous avez opté pour des bibliothèques externes à l’étape précédente. La méthode recommandée consiste à créer une bibliothèque externe à partir de votre fichier `.jar`.

Si vous ne souhaitez pas utiliser de bibliothèques externes, vous devez définir les autorisations nécessaires. L’exécution du script ne réussit que si les identités du processus ont accès à votre code. Pour plus d’informations sur la définition d’autorisations, consultez le [guide d’installation](../install/windows-java.md).

### <a name="on-linux"></a>Sur Linux

Accordez des autorisations de lecture et d’exécution sur classpath à l’utilisateur **mssql_satellite**.

### <a name="on-windows"></a>Sur Windows

Accordez des autorisations de lecture et d’exécution à **SQLRUserGroup** et au SID **Tous les packages d’application** sur le dossier contenant votre code Java compilé.

L’arborescence entière doit avoir les autorisations, du parent racine au dernier sous-dossier.

1. Cliquez avec le bouton droit sur le dossier (par exemple, `C:\myJavaCode`) et choisissez **Propriétés** > **Sécurité**.
2. Cliquez sur **Modifier**.
3. Cliquez sur **Add**.
4. Dans **Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes** :
   1. Cliquez sur **Types d’objets** et veillez à sélectionner *Principes de sécurité intégrés* et *Groupes*.
   2. Cliquez sur **Emplacements** pour sélectionner le nom de l’ordinateur local en haut de la liste.
5. Entrez **SQLRUserGroup**, vérifiez le nom, puis cliquez sur OK pour ajouter le groupe.
6. Entrez **TOUS LES PACKAGES D’APPLICATION**, vérifiez le nom, puis cliquez sur OK pour ajouter. 
    Si le nom n’est pas résolu, revisitez l’étape Emplacements. Le SID est local sur votre ordinateur.

Vérifiez que les deux identités de sécurité ont des autorisations **Lire et exécuter** sur le dossier et le sous-dossier **pkg**.

<a name="call-method"></a>

## <a name="call-the-java-class"></a>Appeler la classe Java

Créez une procédure stockée qui appelle `sp_execute_external_script` pour appeler le code Java à partir de SQL Server. Dans le paramètre **script**, définissez le `package.class` à appeler. Dans le code ci-dessous, la classe appartient à un package appelé **pkg** et à un fichier de classe appelé **RegexSample.java**.

> [!NOTE]
> Le code ne définit pas la méthode à appeler. Par défaut, la méthode **execute** est appelée. Cela signifie que vous devez suivre l’interface du kit SDK et implémenter une méthode execute dans votre classe Java pour pouvoir appeler la classe à partir de SQL Server.

La procédure stockée prend une requête d’entrée (jeu de données d’entrée) et une expression régulière, puis retourne les lignes qui satisfont l’expression régulière donnée. Elle utilise une expression régulière `[Jj]ava` qui vérifie si un texte contient le mot **Java** ou **java**.

```sql
CREATE OR ALTER PROCEDURE [dbo].[java_regex] @expr nvarchar(200), @query nvarchar(400)
AS
BEGIN
--Call the Java program by giving the package.className in @script
--The method invoked in the Java code is always the "execute" method
EXEC sp_execute_external_script
  @language = N'Java'
, @script = N'pkg.RegexSample'
, @input_data_1 = @query
, @params = N'@regexExpr nvarchar(200)'
, @regexExpr = @expr
with result sets ((ID int, text nvarchar(100)));
END
GO

--Now execute the above stored procedure and provide the regular expression and an input query
EXECUTE [dbo].[java_regex] N'[Jj]ava', N'SELECT id, text FROM testdata'
GO
```

### <a name="results"></a>Résultats

Après l’exécution de l’appel, vous devez obtenir un jeu de résultats avec deux lignes.

![Résultats de l’exemple Java](../media/java/java-sample-results.png "Exemples de résultats")

### <a name="if-you-get-an-error"></a>Si vous obtenez une erreur

+ Quand vous compilez vos classes, le sous-dossier **pkg** doit contenir le code compilé des trois classes.

+ Si vous n’utilisez pas de bibliothèques externes, examinez les autorisations sur *chaque* dossier, de la **racine** au sous-dossier **pkg**, pour vérifier que les identités de sécurité qui exécutent le processus externe sont autorisées à lire et à exécuter votre code.

## <a name="next-steps"></a>Étapes suivantes

+ [Guide pratique pour appeler Java dans SQL Server](../how-to/call-java-from-sql.md)
+ [Extensions Java dans SQL Server](../language-extensions-overview.md)
+ [Types de données Java et SQL Server](../how-to/java-to-sql-data-types.md)