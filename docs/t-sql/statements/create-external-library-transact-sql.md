---
description: CREATE EXTERNAL LIBRARY (Transact-SQL) - SQL Server
title: CREATE EXTERNAL LIBRARY (Transact-SQL) - SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 01/28/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- CREATE EXTERNAL LIBRARY
- CREATE_EXTERNAL_LIBRARY_TSQL
- EXTERNAL LIBRARY
- EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 94de0210201668f5ce7096c93220c7776aa5e9d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345248"
---
# <a name="create-external-library-transact-sql"></a>CREATE EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Charge des fichiers de package R, Python ou Java vers une base de données à partir du chemin de fichier ou du flux d’octets spécifié. Cette instruction sert de mécanisme générique permettant à l’administrateur de base de données de charger des artefacts nécessaires pour tout nouveau runtime de langage externe et toute nouvelle plateforme de système d’exploitation pris en charge par [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. 

> [!NOTE]
> Dans SQL Server 2017, le langage R et la plateforme Windows sont pris en charge. R, Python et les langages externes sur les plateformes Windows et Linux sont pris en charge dans SQL Server 2019 et ultérieur.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
Charge des fichiers de package R, Python ou Java vers une base de données à partir du chemin de fichier ou du flux d’octets spécifié. Cette instruction sert de mécanisme générique permettant à l’administrateur de base de données de charger les artefacts nécessaires. 

> [!NOTE]
> Dans Azure SQL Managed Instance, vous pouvez utiliser **sqlmlutils** pour installer une bibliothèque. Pour plus d’informations, consultez [Installer des packages Python avec sqlmlutils](../../machine-learning/package-management/install-additional-python-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true) et [Installer de nouveaux packages R avec sqlmlutils](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true).
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="syntax-for-sql-server-2019"></a>Syntaxe pour SQL Server 2019

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = <language> )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = { <client_library_specifier> | <library_bits> }  
    [, PLATFORM = <platform> ])  
}  

<client_library_specifier> :: = 
{
    '[file_path\]manifest_file_name'  
} 

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}

<platform> :: = 
{
      WINDOWS
    | LINUX
}

<language> :: = 
{
      'R'
    | 'Python'
    | <external_language>
}

```
::: moniker-end
::: moniker range=">=sql-server-2017 <=sql-server-2017"
## <a name="syntax-for-sql-server-2017"></a>Syntaxe pour SQL Server 2017

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = 'R' )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = { <client_library_specifier> | <library_bits> })  
}  

<client_library_specifier> :: = 
{
    '[file_path\]manifest_file_name'
} 

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}
```
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
## <a name="syntax-for-azure-sql-managed-instance"></a>Syntaxe d’Azure SQL Managed Instance

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = <language> )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = <library_bits>)  
}  

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}

<language> :: = 
{
      'R'
    | 'Python'
}
```
::: moniker-end

### <a name="arguments"></a>Arguments

**library_name**

Les bibliothèques chargées vers l’instance peuvent être publiques ou privées. Si la bibliothèque est créée par un membre de `dbo`, elle est publique et peut être partagée avec tous les utilisateurs. Dans le cas contraire, la bibliothèque est privée pour cet utilisateur uniquement.

Les noms de bibliothèques doivent être uniques dans le contexte d’un utilisateur ou d’un propriétaire donné. Par exemple, deux utilisateurs **RUser1** et **RUser2** peuvent chacun charger la bibliothèque R `ggplot2` individuellement et séparément. Toutefois, si **RUser1** souhaitait charger une version plus récente de `ggplot2`, la deuxième instance devrait être nommée différemment ou remplacer la bibliothèque existante.

Les noms de bibliothèques ne peuvent pas être affectés arbitrairement. Le nom de la bibliothèque doit être identique au nom nécessaire pour charger la bibliothèque dans le script externe.

**owner_name**

Spécifie le nom de l’utilisateur ou du rôle propriétaire de la bibliothèque externe. En l'absence de spécification, la propriété revient à l'utilisateur actuel.

Les bibliothèques appartenant au propriétaire de la base de données sont considérées comme globales pour la base de données et le runtime. En d’autres termes, les propriétaires de bases de données peuvent créer des bibliothèques qui contiennent un ensemble commun de bibliothèques ou de packages partagés par de nombreux utilisateurs. Quand une bibliothèque externe est créée par un utilisateur autre que l’utilisateur `dbo`, la bibliothèque externe est privée pour cet utilisateur uniquement.

Quand l’utilisateur **RUser1** exécute un script externe, la valeur de `libPath` peut contenir plusieurs chemins. Le premier est toujours le chemin de la bibliothèque partagée créée par le propriétaire de la base de données. La deuxième partie de `libPath` spécifie le chemin contenant les packages chargés individuellement par **RUser1**.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
**file_spec**

Spécifie le contenu du package pour une plateforme spécifique. Un seul artefact de fichier par plateforme est pris en charge.

Le fichier peut être spécifié sous la forme d’un chemin local ou d’un chemin réseau.

Lors d’une tentative d’accès au fichier spécifié dans **client_library_specifier>** , SQL Server emprunte l’identité du contexte de sécurité de la connexion Windows active. Si **<client_assembly_specifier>** spécifie un emplacement réseau (chemin UNC), l’emprunt d’identité de la connexion active n’est pas transféré à l’emplacement réseau en raison des restrictions de délégation. Dans ce cas, l’accès a lieu en utilisant le contexte de sécurité du compte de service SQL Server. Pour plus d’informations, consultez [Informations d’identification (moteur de base de données)](../../relational-databases/security/authentication-access/credentials-database-engine.md).

::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Si vous le souhaitez, vous pouvez spécifier une plateforme de système d’exploitation pour le fichier. Un seul artefact de fichier ou contenu est autorisé pour chaque plateforme de système d’exploitation pour un langage ou un runtime spécifique.
::: moniker-end

**library_bits**

Spécifie le contenu du package en tant que littéral hexadécimal, similaire aux assemblys. 

Cette option est utile si vous devez créer une bibliothèque ou modifier une bibliothèque existante (et que vous disposez des autorisations nécessaires), mais que le système de fichiers sur le serveur est restreint et vous ne pouvez pas copier les fichiers de bibliothèque vers un emplacement accessible au serveur.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**PLATFORM**

Spécifie la plateforme pour le contenu de la bibliothèque. La valeur par défaut est la plateforme hôte sur laquelle SQL Server est en cours d’exécution. Par conséquent, l’utilisateur n’a pas à spécifier la valeur. Elle est obligatoire dans le cas où plusieurs plateformes sont prises en charge, ou quand l’utilisateur a besoin de spécifier une autre plateforme.
Dans SQL Server 2019, les plateformes Windows et Linux sont prises en charge.
::: moniker-end

::: moniker range="=sql-server-2017"
**LANGUAGE = 'R'**

Spécifie le langage du package. R est pris en charge dans SQL Server 2017.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
**language**

Spécifie le langage du package. La valeur peut être `R` ou `Python` dans Azure SQL Managed Instance.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**language**

Spécifie le langage du package. La valeur peut être `R`, `Python` ou le nom d’un langage externe (consultez [CREATE EXTERNAL LANGUAGE](create-external-language-transact-sql.md)).
::: moniker-end

## <a name="remarks"></a>Notes

::: moniker range=">=sql-server-2017 <=sql-server-2017"
Pour le langage R, lors de l’utilisation d’un fichier, les packages doivent être préparés sous la forme de fichiers d’archives compressés avec l’extension .ZIP pour Windows. 
Dans SQL Server 2017, seule la plateforme Windows est pris en charge.
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Pour le langage R, lors de l’utilisation d’un fichier, les packages doivent être préparés sous la forme de fichiers d’archive compressés avec l’extension .ZIP.  

Pour le langage Python, le package contenu dans un fichier .whl ou .zip doit être préparé sous la forme d’un fichier d’archive compressé. Si le package est déjà un fichier .zip, il doit être inclus dans un nouveau fichier .zip. Le chargement direct d’un package sous la forme de fichier .whl ou .zip n’est actuellement pas pris en charge.
::: moniker-end

L’instruction `CREATE EXTERNAL LIBRARY` charge les bits de la bibliothèque vers la base de données. La bibliothèque est installée quand un utilisateur exécute un script externe à l’aide de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) et appelle le package ou la bibliothèque.

Les bibliothèques chargées vers l’instance peuvent être publiques ou privées. Si la bibliothèque est créée par un membre de `dbo`, elle est publique et peut être partagée avec tous les utilisateurs. Dans le cas contraire, la bibliothèque est privée pour cet utilisateur uniquement.

Un certain nombre de packages, appelés *packages système*, sont préinstallés dans une instance SQL. Les packages systèmes ne peuvent être ni ajoutés, ni mis à jour, ni supprimés par l’utilisateur.

## <a name="permissions"></a>Autorisations

Nécessite l’autorisation `CREATE EXTERNAL LIBRARY`. Par défaut, les utilisateurs qui possèdent **dbo**, membre du rôle **db_owner**, disposent des autorisations nécessaires pour créer une bibliothèque externe. En ce qui concerne les autres utilisateurs, vous devez leur en donner l’autorisation explicitement à l’aide une instruction [GRANT](./grant-database-permissions-transact-sql.md), en spécifiant le privilège CREATE EXTERNAL LIBRARY.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Dans SQL Server 2019, en plus de l’autorisation « CREATE EXTERNAL LIBRARY », l’utilisateur a besoin d’autorisations de référence sur un langage externe afin de créer des bibliothèques externes pour ce dernier.

```sql
GRANT REFERENCES ON EXTERNAL LANGUAGE::Java to user
GRANT CREATE EXTERNAL LIBRARY to user
```

::: moniker-end

La modification de toute bibliothèque nécessite l’autorisation distincte `ALTER ANY EXTERNAL LIBRARY`.

Pour créer une bibliothèque externe en utilisant un chemin de fichier, l’utilisateur doit avoir établi une connexion Windows authentifiée ou être membre du rôle serveur fixe sysadmin.

## <a name="examples"></a>Exemples

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
### <a name="add-an-external-library-to-a-database"></a>Ajouter une bibliothèque externe à une base de données  

L’exemple suivant ajoute une bibliothèque externe nommée `customPackage` à une base de données.

```sql
CREATE EXTERNAL LIBRARY customPackage
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\customPackage.zip') WITH (LANGUAGE = 'R');
```

Une fois la bibliothèque chargée vers l’instance, un utilisateur exécute la procédure `sp_execute_external_script` pour installer la bibliothèque.

```sql
EXEC sp_execute_external_script 
@language =N'R', 
@script=N'library(customPackage)'
```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Pour le langage Python dans SQL Server 2019, l’exemple fonctionne également en remplaçant `'R'` par `'Python'`.
::: moniker-end

### <a name="installing-packages-with-dependencies"></a>Installation de packages avec des dépendances

Si le package que vous souhaitez installer a des dépendances, vous devez impérativement analyser les dépendances de premier niveau et de deuxième niveau, et vérifier que tous les packages nécessaires sont disponibles _avant_ d’essayer d’installer le package cible.

Par exemple, supposez que vous souhaitez installer un nouveau package, `packageA` :

+ `packageA` a une dépendance envers `packageB`.
+ `packageB` a une dépendance envers `packageC`.

Pour que l’installation de `packageA` réussisse, vous devez créer des bibliothèques pour `packageB` et `packageC` en même temps que vous ajoutez `packageA` à SQL Server. Veillez à vérifier également les versions de package nécessaires.

Dans la pratique, les dépendances de package pour les packages populaires sont généralement beaucoup plus compliquées que cet exemple simple. Par exemple, **ggplot2** peut avoir besoin de plus de 30 packages, qui eux-mêmes peuvent réclamer des packages supplémentaires non disponibles sur le serveur. Tout package manquant ou dont la version est incorrecte peut provoquer l’échec de l’installation.

Sachant qu’il peut être difficile de déterminer toutes les dépendances en examinant simplement le manifeste du package, nous vous recommandons d’utiliser un package comme [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html) pour identifier tous les packages potentiellement nécessaires à l’installation.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"

+ Chargez le package cible et ses dépendances. Tous les fichiers doivent être dans un dossier accessible au serveur.

    ```sql
    CREATE EXTERNAL LIBRARY packageA 
    FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageA.zip') 
    WITH (LANGUAGE = 'R'); 
    GO

    CREATE EXTERNAL LIBRARY packageB FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageB.zip') 
    WITH (LANGUAGE = 'R');
    GO

    CREATE EXTERNAL LIBRARY packageC FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageC.zip') 
    WITH (LANGUAGE = 'R');
    GO
    ```

+ Installez d’abord les packages nécessaires.

    Si un package nécessaire a déjà été chargé vers l’instance, vous n’avez pas besoin de l’ajouter à nouveau. Vérifiez simplement si le package existant est la version correcte. 
    
    Les packages nécessaires `packageC` et `packageB` sont installés dans l’ordre correct, quand `sp_execute_external_script` est exécuté pour la première fois afin d’installer le package `packageA`.

    Toutefois, si un package nécessaire n’est pas disponible, l’installation du package cible `packageA` échoue.

    ```sql
    EXEC sp_execute_external_script 
    @language =N'R', 
    @script=N'
    # load the desired package packageA
    library(packageA)
    '
    ```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Pour le langage Python dans SQL Server 2019, l’exemple fonctionne également en remplaçant `'R'` par `'Python'`.
::: moniker-end

### <a name="create-a-library-from-a-byte-stream"></a>Créer une bibliothèque à partir d’un flux d’octets

Si vous n’êtes pas en mesure d’enregistrer les fichiers du package à un emplacement sur le serveur, vous pouvez transmettre son contenu dans une variable. L’exemple suivant crée une bibliothèque en passant les bits sous forme de littéral hexadécimal.

```SQL
CREATE EXTERNAL LIBRARY customLibrary FROM (CONTENT = 0xABC123...) WITH (LANGUAGE = 'R');
```

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Pour le langage Python dans SQL Server 2019, l’exemple fonctionne également en remplaçant **'R'** par **'Python'** .
::: moniker-end

> [!NOTE]
> Cet exemple de code montre uniquement la syntaxe ; la valeur binaire de `CONTENT =` a été tronquée pour des raisons de clarté et ne crée pas une bibliothèque fonctionnelle. Son véritable contenu serait beaucoup plus long.

### <a name="change-an-existing-package-library"></a>Changer une bibliothèque de package existante

Vous pouvez utiliser l’instruction DDL `ALTER EXTERNAL LIBRARY` pour ajouter du nouveau contenu à la bibliothèque ou modifier le contenu existant de la bibliothèque. La modification d’une bibliothèque existante nécessite l’autorisation `ALTER ANY EXTERNAL LIBRARY`.

Pour plus d’informations, consultez [ALTER EXTERNAL LIBRARY](alter-external-library-transact-sql.md).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
### <a name="add-a-java-jar-file-to-a-database"></a>Ajouter un fichier .jar Java à une base de données  

L’exemple suivant ajoute un fichier jar externe nommé `customJar` à une base de données.

```sql
CREATE EXTERNAL LIBRARY customJar
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\customJar.jar') 
WITH (LANGUAGE = 'Java');
```

Une fois la bibliothèque chargée vers l’instance, un utilisateur exécute la procédure `sp_execute_external_script` pour installer la bibliothèque.

```sql
EXEC sp_execute_external_script
    @language = N'Java'
    , @script = N'customJar.MyCLass.myMethod'
    , @input_data_1 = N'SELECT * FROM dbo.MyTable'
WITH RESULT SETS ((column1 int))
```

### <a name="add-an-external-package-for-both-windows-and-linux"></a>Ajouter un package externe pour Windows et Linux

Vous pouvez spécifier jusqu’à deux `<file_spec>`, un pour Windows et l’autre pour Linux.

```sql
CREATE EXTERNAL LIBRARY lazyeval 
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\packageA.zip', PLATFORM = WINDOWS),
(CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\packageA.tar.gz', PLATFORM = LINUX)
WITH (LANGUAGE = 'R')
```

Quand vous utilisez `sp_execute_external_script` pour installer le package, le contenu de la bibliothèque utilisé est celui correspondant à la plateforme sur laquelle s’exécute l’instance SQL Server.

```sql
EXECUTE sp_execute_external_script 
    @LANGUAGE = N'R',
    @SCRIPT = N'
library(packageA)
```
::: moniker-end

## <a name="see-also"></a>Voir aussi

[ALTER EXTERNAL LIBRARY (Transact-SQL)](alter-external-library-transact-sql.md)  
[DROP EXTERNAL LIBRARY (Transact-SQL)](drop-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)
