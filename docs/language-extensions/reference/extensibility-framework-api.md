---
title: API de framework d’extensibilité
titleSuffix: SQL Server Language Extensions
description: Vous pouvez utiliser le framework d’extensibilité pour écrire des extensions de langage de programmation pour SQL Server. L’API de framework d’extensibilité pour Microsoft SQL Server est une API qui peut être utilisée par une extension de langage pour interagir avec des données et les échanger avec SQL Server.
author: dphansen
ms.author: davidph
ms.date: 10/09/2020
ms.topic: reference
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 21099123d8065fad63e1526a3377f1904aed87b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016523"
---
# <a name="extensibility-framework-api-for-sql-server"></a>API de framework d’extensibilité pour SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Vous pouvez utiliser le framework d’extensibilité pour écrire des extensions de langage de programmation pour SQL Server. L’API de framework d’extensibilité pour Microsoft SQL Server est une API qui peut être utilisée par une extension de langage pour interagir avec des données et les échanger avec SQL Server.

En tant qu’auteur d’extension de langage, vous pouvez utiliser cette référence avec les extensions de langage open source afin de comprendre comment utiliser l’API pour écrire vos propres extensions de langage. Vous trouverez le code source de l’extension de langage sur [aka.ms/mssql-lang-extensions](https://aka.ms/mssql-lang-extensions).

La syntaxe et les informations sur les arguments de toutes les fonctions API sont décrites ci-dessous.

## <a name="return-value"></a>Valeur retournée

Toutes les fonctions retournent un paramètre *SQLRETURN*. Si la valeur est autre que *SQL_SUCCESS*, ceci est traité comme une erreur et l’exécution du script s’arrête.

## <a name="standard-output"></a>Sortie standard

Toute sortie de l’extension vers la sortie standard ou les flux d’erreur est tracée dans le fichier journal de la session, puis finalement retracée vers SQL Server (semblable à tout ce qui est affiché sous l’onglet Messages SSMS).


## <a name="init"></a>Init

Cette fonction est appelée une seule fois et est utilisée pour initialiser le runtime en vue de l’exécution. 

### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN Init(
    SQLCHAR *ExtensionParams,
    SQLULEN ExtensionParamsLength,
    SQLCHAR *ExtensionPath,
    SQLULEN ExtensionPathLength,
    SQLCHAR *PublicLibraryPath,
    SQLULEN PublicLibraryPathLength,
    SQLCHAR *PrivateLibraryPath,
    SQLULEN PrivateLibraryPathLength
);
```

### <a name="arguments"></a>Arguments

*ExtensionParams*  
\[Entrée\] Chaîne se terminant par un caractère Null qui contient la valeur `PARAMETERS` fournie durant [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) ou [ALTER EXTERNAL LANGUAGE](../../t-sql/statements/alter-external-language-transact-sql.md).

*ExtensionParamsLength*  
\[Entrée\] Longueur en octets de *ExtensionParams* (caractère Null de fin non compris).

*ExtensionPath*  
\[Entrée\] Chaîne UTF-8 terminée par un caractère Null, contenant le chemin absolu du répertoire d’installation de l’extension.

*ExtensionPathLength*  
\[Entrée\] Longueur en octets de *ExtensionPath* (caractère Null de fin non compris).

*PublicLibraryPath*  
\[Entrée\] Chaîne UTF-8 terminée par un caractère Null, contenant le chemin absolu du répertoire des bibliothèques externes publiques pour ce langage externe.

*PublicLibraryPathLength*  
\[Entrée\] Longueur en octets de *PublicLibraryPath* (caractère Null de fin non compris).

*PrivateLibraryPath*  
\[Entrée\] Chaîne UTF-8 terminée par un caractère Null, contenant le chemin absolu du répertoire des bibliothèques externes privées pour cet utilisateur et ce langage externe.

*PrivateLibraryPathLength*  
\[Entrée\] Longueur en octets de *PrivateLibraryPath* (caractère Null de fin non compris).

## <a name="initsession"></a>InitSession

Cette fonction est appelée une fois par session et initialise des paramètres propres à la session.

### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN InitSession(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLCHAR*        Script,
    SQLULEN         ScriptLength,
    SQLUSMALLINT    InputSchemaColumnsNumber,
    SQLUSMALLINT    ParametersNumber
    SQLCHAR*        InputDataName,
    SQLUSMALLINT    InputDataNameLength,
    SQLCHAR*        OutputDataName,
    SQLUSMALLINT    OutputDataNameLength
);
```

### <a name="arguments"></a>Arguments

*SessionId*  
\[Entrée\] GUID identifiant de façon unique cette session de script.

*TaskId*  
\[Entrée\] Entier identifiant de façon unique ce processus d’exécution.

Quand `@parallel = 1` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), cette valeur est comprise entre 0 et le degré de parallélisme de la requête.

*Script*  
\[Entrée\] Chaîne UTF-8 terminée par un caractère Null contenant le `@script` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*ScriptLength*  
\[Entrée\] Longueur en octets de *ScriptScript* (caractère Null de fin non compris).

*InputSchemaColumnsNumber*  
\[Entrée\] Nombre de colonnes dans le jeu de résultats de `@input_data_1` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*ParametersNumber*  
\[Entrée\] Nombre de paramètres d’entrée de `@params` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*InputDataName*  
\[Entrée\] Chaîne UTF-8 terminée par un caractère Null contenant le `@input_data_1_name` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*InputDataNameLength*  
\[Entrée\] Longueur en octets de *InputDataName* (caractère Null de fin non compris).

*OutputDataName*  
\[Entrée\] Chaîne UTF-8 terminée par un caractère Null contenant le `@output_data_1_name` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*OutputDataNameLength*  
\[Entrée\] Longueur en octets de *OutputDataName* (caractère Null de fin non compris).

## <a name="initcolumn"></a>InitColumn

Initialiser les informations d’une colonne donnée pour une session particulière.

Cette fonction est appelée pour chaque colonne du jeu de résultats de `@input_data_1` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

La structure de colonne de ce jeu de résultats sera appelée *schéma d’entrée*.

### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN InitColumn(
    SQLGUID       SessionId,
    SQLUSMALLINT  TaskId,
    SQLUSMALLINT  ColumnNumber,
    SQLCHAR*      ColumnName,
    SQLSMALLINT   ColumnNameLength,
    SQLSMALLINT   DataType,
    SQLULEN       ColumnSize,
    SQLSMALLINT   DecimalDigits,
    SQLSMALLINT   Nullable,
    SQLSMALLINT   PartitionByNumber,
    SQLSMALLINT   OrderByNumber
);
```

### <a name="arguments"></a>Arguments

*SessionId*  
\[Entrée\] GUID identifiant de façon unique cette session de script.

*TaskId*  
\[Entrée\] Entier identifiant de façon unique ce processus d’exécution.

Quand `@parallel = 1` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), cette valeur est comprise entre 0 et le degré de parallélisme de la requête.

*ColumnNumber*  
\[Entrée\] Entier identifiant l’index de cette colonne dans le schéma d’entrée. Les colonnes sont numérotées de façon séquentielle dans l’ordre croissant à partir de 0.

*ColumnName*  
\[Entrée\] Chaîne UTF-8 terminée par un caractère Null contenant le nom de la colonne.

*ColumnNameLength*  
\[Entrée\] Longueur en octets de *ColumnName* (caractère Null de fin non compris).

*DataType*  
\[Entrée\] Type C ODBC identifiant le type de données de cette colonne.

*ColumnSize*  
\[Entrée\] Taille maximale, en octets, des données sous-jacentes de cette colonne.

Pour les types de données SQL_C_CHAR, SQL_C_WCHAR et SQL_C_BINARY, les valeurs supérieures à 8000 indiquent que cette colonne représente des objets LOB dont la taille est inférieure à 2 Go.

*DecimalDigits*  
\[Entrée\] Chiffres décimaux des données sous-jacentes de cette colonne, comme défini par [Chiffres décimaux](../../odbc/reference/appendixes/decimal-digits.md).

*Nullable*  
\[Entrée\] Valeur qui indique si cette colonne peut contenir des valeurs Null. Valeurs possibles :

- SQL_NO_NULLS : la colonne ne peut pas contenir de valeurs Null.
- SQL_NULLABLE : la colonne peut contenir des valeurs Null.

*PartitionByNumber*  
\[Entrée\] Valeur qui indique l’index de cette colonne dans la séquence `@input_data_1_partition_by_columns` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Les colonnes sont numérotées de façon séquentielle dans l’ordre croissant à partir de 0. Si cette colonne n’est pas incluse dans la séquence, la valeur est -1.

*OrderByNumber*  
\[Entrée\] Valeur qui indique l’index de cette colonne dans la séquence `@input_data_1_order_by_columns` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Les colonnes sont numérotées de façon séquentielle dans l’ordre croissant à partir de 0. Si cette colonne n’est pas incluse dans la séquence, la valeur est -1.

## <a name="initparam"></a>InitParam

Initialiser les informations relatives à un paramètre d’entrée donné pour une session particulière.

Cette fonction est appelée pour chaque paramètre de `@params` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN InitParam(
    SQLGUID      SessionId,
    SQLUSMALLINT TaskId,
    SQLUSMALLINT ParamNumber,
    SQLCHAR*     ParamName,
    SQLSMALLINT  ParamNameLength,
    SQLSMALLINT  DataType,
    SQLULEN      ParamSize,
    SQLSMALLINT  DecimalDigits,
    SQLPOINTER   ParamValue,
    SQLINTEGER   StrLen_or_Ind,
    SQLSMALLINT  InputOutputType
);
```

### <a name="arguments"></a>Arguments

*SessionId*  
\[Entrée\] GUID identifiant de façon unique cette session de script.

*TaskId*  
\[Entrée\] Entier identifiant de façon unique ce processus d’exécution.

Quand `@parallel = 1` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), cette valeur est comprise entre 0 et le degré de parallélisme de la requête.

*ParamNumber*  
\[Entrée\] Entier identifiant l’index de ce paramètre. Les paramètres sont numérotés de façon séquentielle dans l’ordre croissant à partir de 0.

*ParamName*  
\[Entrée\] Chaîne UTF-8 terminée par un caractère Null contenant le nom du paramètre.

*ParamNameLength*  
\[Entrée\] Longueur en octets de *ParamName* (caractère Null de fin non compris).

*DataType*  
\[Entrée\] Type C ODBC identifiant le type de données de ce paramètre.

*ParamSize*  
\[Entrée\] Taille maximale, en octets, des données sous-jacentes de ce paramètre.

Pour les types de données SQL_C_CHAR, SQL_C_WCHAR et SQL_C_BINARY, les valeurs supérieures à 8000 indiquent que ce paramètre représente des objets LOB dont la taille est inférieure à 2 Go.

*DecimalDigits*  
\[Entrée\] Chiffres décimaux des données sous-jacentes de ce paramètre, comme défini par [Chiffres décimaux](../../odbc/reference/appendixes/decimal-digits.md).

*ParamValue*  
\[Entrée\] Pointeur vers une mémoire tampon contenant la valeur du paramètre.

*StrLen_or_Ind*  
\[Entrée\] Valeur entière indiquant la longueur en octets de *ParamValue*, ou SQL_Null_DATA pour indiquer que les données sont Null.

StrLen_or_Ind\[col\] peut être ignoré si une colonne n’accepte pas la valeur Null et ne représente pas l’un des types de données suivants : SQL_C_CHAR, SQL_C_WCHAR et SQL_C_BINARY, SQL_C_NUMERIC ou SQL_C_TYPE_TIMESTAMP. Sinon, il pointe vers un tableau valide avec \[RowsNumber\] éléments, où chaque élément contient sa longueur ou des données d’indicateur Null.

*InputOutputType*  
\[Entrée\] Type du paramètre. L’argument *InputOutputType* est l’une des valeurs suivantes :

- SQL_PARAM_INPUT
- SQL_PARAM_INPUT_OUTPUT

## <a name="execute"></a>Execute

Exécuter le `@script` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Cette fonction peut être appelée plusieurs fois, une fois pour chaque flux de blocs et pour chaque partition du `@input_data_1_partition_by_columns` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).


### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN Execute(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN         RowsNumber,
    SQLPOINTER*     Data,
    SQLINTEGER**    StrLen_or_Ind,
    SQLUSMALLINT*   OutputSchemaColumnsNumber
);
```

### <a name="arguments"></a>Arguments

*SessionId*  
\[Entrée\] GUID identifiant de façon unique cette session de script.

*TaskId*  
\[Entrée\] Entier identifiant de façon unique ce processus d’exécution.

Quand `@parallel = 1` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), cette valeur est comprise entre 0 et le degré de parallélisme de la requête.

*RowsNumber*  
\[Entrée\] Nombre de lignes dans *Data*.

*Données*  
\[Entrée\] Tableau à deux dimensions qui contient le jeu de résultats de `@input_data_1` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Le nombre total de colonnes est *InputSchemaColumnsNumber* qui a été reçu dans l’appel [InitSession](#initsession). Chaque colonne contient *RowsNumber* éléments qui doivent être interprétés en fonction du type de colonne de [InitColumn](#initcolumn).

Les éléments indiqués comme étant Null dans *StrLen_or_Ind* ne sont pas forcément valides et doivent être ignorés.

*StrLen_or_Ind*  
\[Entrée\] Tableau à deux dimensions qui contient l’indicateur de longueur/Null pour chaque valeur dans *Data*. Valeurs possibles de chaque cellule :

- n, où n > 0. Indication de la longueur des données en octets
- SQL_NULL_DATA, indiquant une valeur Null.

Le nombre total de colonnes est *InputSchemaColumnsNumber* qui a été reçu dans l’appel [InitSession](#initsession). Chaque colonne contient *RowsNumber* éléments qui doivent être interprétés en fonction du type de colonne de [InitColumn](#initcolumn).

StrLen_or_Ind\[col\] peut être ignoré si une colonne n’accepte pas la valeur Null et ne représente pas l’un des types de données suivants : SQL_C_CHAR, SQL_C_WCHAR et SQL_C_BINARY, SQL_C_NUMERIC ou SQL_C_TYPE_TIMESTAMP. Sinon, il pointe vers un tableau valide avec *RowsNumber* éléments, où chaque élément contient sa longueur ou des données d’indicateur Null.

*OutputSchemaColumnsNumber*  
\[Sortie\] Pointeur vers une mémoire tampon dans laquelle retourner le nombre de colonnes dans le jeu de résultats attendu du `@script` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

## <a name="getresultcolumn"></a>GetResultColumn

Récupérer les informations relatives à une colonne de sortie donnée pour une session particulière.

Cette fonction est appelée pour chaque colonne du jeu de résultats de `@script` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
La structure de colonne de ce jeu de résultats sera appelée « schéma de sortie ».

### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN GetResultColumn(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLUSMALLINT    ColumnNumber,
    SQLSMALLINT*    DataType,
    SQLINTEGER*     ColumnSize,
    SQLSMALLINT*    DecimalDigits,
    SQLSMALLINT*    Nullable
);
```

### <a name="arguments"></a>Arguments

*SessionId*  
\[Entrée\] GUID identifiant de façon unique cette session de script.

*TaskId*  
\[Entrée\] Entier identifiant de façon unique ce processus d’exécution.

Quand `@parallel = 1` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), cette valeur est comprise entre 0 et le degré de parallélisme de la requête.

*ColumnNumber*  
\[Entrée\] Entier identifiant l’index de cette colonne dans le schéma de sortie. Les colonnes sont numérotées de façon séquentielle dans l’ordre croissant à partir de 0.

*DataType*  
\[Sortie\] Pointeur vers la mémoire tampon qui contient le type C ODBC identifiant le type de données de cette colonne.

*ColumnSize*  
\[Sortie\] Pointeur vers une mémoire tampon qui contient la taille maximale, en octets, des données sous-jacentes de cette colonne.

*DecimalDigits*  
\[Sortie\] Pointeur vers une mémoire tampon qui contient les chiffres décimaux des données sous-jacentes de cette colonne, comme défini par [Chiffres décimaux](../../odbc/reference/appendixes/decimal-digits.md). Si le nombre de chiffres décimaux ne peut pas être déterminé ou s’il n’est pas applicable, la valeur est ignorée.

*Nullable*  
\[Sortie\] Pointeur vers une mémoire tampon qui contient une valeur indiquant si cette colonne peut contenir des valeurs Null. Valeurs possibles :

- SQL_NO_NULLS : la colonne ne peut pas contenir de valeurs Null.
- SQL_NULLABLE : la colonne peut contenir des valeurs Null.

Si d’autres valeurs sont passées, l’exécution s’arrête.

## <a name="getresults"></a>GetResults

Récupérer le jeu de résultats à partir de l’exécution du `@script` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Cette fonction peut être appelée plusieurs fois, une fois pour chaque flux de blocs et pour chaque partition du `@input_data_1_partition_by_columns` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).


### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN GetResults(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN*        RowsNumber,
    SQLPOINTER**    Data,
    SQLINTEGER***   StrLen_or_Ind
);
```

### <a name="arguments"></a>Arguments

*SessionId*  
\[Entrée\] GUID identifiant de façon unique cette session de script.

*TaskId*  
\[Entrée\] Entier identifiant de façon unique ce processus d’exécution.

Quand `@parallel = 1` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), cette valeur est comprise entre 0 et le degré de parallélisme de la requête.

*RowsNumber*  
\[Sortie\] Pointeur vers une mémoire tampon qui contient le nombre de lignes dans *Data*.

*Données*  
\[Sortie\] Pointeur vers un tableau à deux dimensions alloué par l’extension qui contient le jeu de résultats de `@script` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Le nombre total de colonnes doit être  *OutputSchemaColumnsNumber* qui a été récupéré dans l’appel [Execute](#execute). Chaque colonne doit contenir *RowsNumber* éléments qui doivent être interprétés en fonction du type de colonne de [GetResultColumn](#getresultcolumn).

*StrLen_or_Ind*  
\[Sortie\] Pointeur vers un tableau à deux dimensions alloué par l’extension qui contient l’indicateur de longueur/Null pour chaque valeur dans *Data*. Valeurs possibles de chaque cellule :

- n, où n > 0. Indication de la longueur des données en octets
- SQL_NULL_DATA, indiquant une valeur Null.

Le nombre total de colonnes doit être  *OutputSchemaColumnsNumber* qui a été reçu dans l’appel [Execute](#execute). Chaque colonne contient *RowsNumber* éléments qui doivent être interprétés en fonction du type de colonne de [GetResultColumn](#getresultcolumn).

StrLen_or_Ind\[col\] est ignoré si une colonne n’accepte pas la valeur Null et ne représente pas l’un des types de données suivants : SQL_C_CHAR, SQL_C_WCHAR et SQL_C_BINARY [ajouter des dates]. Sinon, il pointe vers un tableau valide avec *RowsNumber* éléments, où chaque élément contient sa longueur ou des données d’indicateur Null.

## <a name="getoutputparam"></a>GetOutputParam

Récupérer les informations relatives à un paramètre de sortie donné pour une session particulière.

Cette fonction est appelée pour chaque paramètre de `@params` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) marqué avec OUTPUT.

### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN GetOutputParam(
    SQLGUID        SessionId,
    SQLUSMALLINT   ParamNumber,
    SQLPOINTER*    ParamValue,
    SQLINTEGER*    StrLen_or_Ind
);
```

### <a name="arguments"></a>Arguments

*SessionId*  
\[Entrée\] GUID identifiant de façon unique cette session de script.

*ParamValue*  
\[Sortie\] Pointeur vers une mémoire tampon contenant la valeur du paramètre.

*StrLen_or_Ind* \[sortie\] Pointeur vers une mémoire tampon qui contient une valeur entière indiquant la longueur en octets de *ParamValue*, ou SQL_NULL_DATA pour indiquer que les données sont Null.

## <a name="getinterfaceversion"></a>GetInterfaceVersion

Récupérer la version de l’interface.
Cette fonction retourne un entier représentant la version de l’interface de l’extension. Valeurs prises en charge :
1. La version 1 est la version d’API initiale. Pris en charge dans SQL Server 2019 RTM.
1. La version 2 a ajouté la prise en charge des API InstallExternalLibrary et UninstallExternalLibrary, et est prise en charge à partir de SQL Server 2019 CU3.                            

### <a name="syntax"></a>Syntaxe

```cpp
SQLUSMALLINT GetInterfaceVersion();
```

## <a name="cleanupsession"></a>CleanupSession

Nettoyer les informations par session.

### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN CleanupSession(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId
);
```

### <a name="arguments"></a>Arguments

*SessionId*  
\[Entrée\] GUID identifiant de façon unique cette session de script.

*TaskId*  
\[Entrée\] Entier identifiant de façon unique ce processus d’exécution.

Quand `@parallel = 1` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), cette valeur est comprise entre 0 et le degré de parallélisme de la requête.

## <a name="cleanup"></a>Nettoyage

Nettoyer les informations globales et partagées (par exemple JVM).

### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN Cleanup();
```

## <a name="gettelemetryresults"></a>GetTelemetryResults

Récupère les données de télémétrie (paires clé-valeur) à partir de l’extension. La fonction est facultative et ne nécessite pas d’implémentation. La télémétrie est exposée par la vue de gestion dynamique (DMV) `dm_db_external_script_execution_stats`.

Il y a un compteur nommé script_executions, qui est envoyé par le framework. L’extension ne doit pas utiliser ce nom.

Chaque entrée de télémétrie est une paire clé-valeur. Les clés sont des chaînes, les valeurs sont des compteurs entiers 64 bits. Ainsi, la sortie comprend deux tableaux logiques : les noms et les compteurs correspondants. Chaque tableau est une sortie.

La longueur de chaque tableau est *RowsNumber*, qui est une sortie. La première sortie logique contient des pointeurs vers des chaînes. Ainsi, elle est représentée par deux tableaux : *CounterNames* (les données de chaîne réelles) et *CounterNamesLength* (la longueur de chaque chaîne). La deuxième sortie logique est stockée dans le pointeur *CounterValues*.

### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN GetTelemetryResults(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId,
    SQLUINTEGER    *RowsNumber,
    SQLCHAR        ***CounterNames,
    SQLINTEGER      **CounterNamesLength,
    SQLBIGINT       **CounterValues
);
```

### <a name="arguments"></a>Arguments

*SessionId*  
\[Entrée\] GUID identifiant de façon unique cette session de script.

*TaskId*  
\[Entrée\] Entier identifiant de façon unique ce processus d’exécution.

Quand `@parallel = 1` dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), cette valeur est comprise entre 0 et le degré de parallélisme de la requête.

*RowsNumber*  
\[Sortie\] Nombre de paires clé-valeur.

*CounterNames*  
\[Sortie\] Données de chaîne contenant les clés.

*CounterNamesLength*  
\[Sortie\] Longueur de chaque chaîne de clé.

*CounterValues*  
\[Sortie\] Données de type Entier de 64 bits contenant les valeurs.

## <a name="installexternallibrary"></a>InstallExternalLibrary

Installe une bibliothèque. La fonction est facultative et ne nécessite pas d’implémentation. L’implémentation par défaut consiste à copier le contenu de la bibliothèque (voir [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md)) dans un fichier à l’emplacement approprié. Le nom de fichier est le nom de la bibliothèque.

### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN InstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryFile,
    SQLINTEGER LibraryFileLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>Arguments

*SetupSessionId*  
\[Entrée\] GUID identifiant de façon unique cette session de script.

*LibraryName*  
\[Entrée\] Nom de la bibliothèque.

*LibraryNameLength*  
\[Entrée\] Longueur du nom de la bibliothèque.

*LibraryFile*  
\[Entrée\] Chemin (sous forme de chaîne) du fichier de bibliothèque contenant le contenu binaire spécifié par [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

*LibraryFileLength*  
\[Entrée\] Longueur de la chaîne LibraryFile.

*LibraryInstallDirectory :*  
\[Entrez\] Répertoire racine où installer la bibliothèque.

*LibraryInstallDirectoryLength*  
\[Entrée\] Longueur de la chaîne LibraryInstallDirectory.

*LibraryError*  
\[Sortie\] Paramètre de sortie facultatif. En cas d’erreur pendant l’installation de la bibliothèque, LibraryError pointe vers une chaîne décrivant l’erreur.

*LibraryErrorLength*  
\[Sortie\] Longueur de la chaîne LibraryError.

## <a name="uninstallexternallibrary"></a>UninstallExternalLibrary

Désinstalle une bibliothèque. La fonction est facultative et ne nécessite pas d’implémentation. L’implémentation par défaut consiste à annuler le travail effectué par l’implémentation par défaut d’InstallExternalLibrary. L’implémentation par défaut supprime le contenu du fichier *LibraryName* sous *LibraryInstallDirectory*.

### <a name="syntax"></a>Syntaxe

```cpp
SQLRETURN UninstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>Arguments

*SetupSessionId*  
\[Entrée\] GUID identifiant de façon unique cette session de script.

*LibraryName*  
\[Entrée\] Nom de la bibliothèque.

*LibraryNameLength*  
\[Entrée\] Longueur du nom de la bibliothèque.

*LibraryFile*  
\[Entrée\] Chemin (sous forme de chaîne) du fichier de bibliothèque contenant le contenu binaire spécifié par CREATE EXTERNAL LIBRARY.

*LibraryFileLength*  
\[Entrée\] Longueur de la chaîne LibraryFile.

*LibraryInstallDirectory*  
\[Entrez\] Répertoire racine où installer la bibliothèque.

*LibraryInstallDirectoryLength*  
\[Entrée\] Longueur de la chaîne LibraryInstallDirectory.

*LibraryError*  
\[Sortie\] Chaîne d’erreur de la bibliothèque.

*LibraryErrorLength*  
\[Sortie\] Longueur de la chaîne LibraryError.

## <a name="next-steps"></a>Étapes suivantes

- [Kit SDK d’extensibilité Microsoft pour Java pour SQL Server](../how-to/extensibility-sdk-java-sql-server.md)
- [Runtime personnalisé Python](../../machine-learning/install/custom-runtime-python.md)
- [Runtime personnalisé R](../../machine-learning/install/custom-runtime-r.md)