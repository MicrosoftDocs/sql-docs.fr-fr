---
description: CREATE EXTERNAL LANGUAGE (Transact-SQL) - SQL Server
title: CREATE EXTERNAL LANGUAGE (Transact-SQL) - SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 04/03/2020
ms.prod: sql
ms.technology: language-extensions
ms.topic: language-reference
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: a299c47b9d0d2fccde5c44c26d9f78e8bc1cca2c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070666"
---
# <a name="create-external-language-transact-sql"></a>CREATE EXTERNAL LANGUAGE (Transact-SQL)
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Inscrit les extensions de langage externe dans la base de données à partir du flux d’octets ou du chemin de fichier spécifié. Cette instruction sert de mécanisme générique permettant à l’administrateur de base de données d’inscrire de nouvelles extensions de langage externe sur toute plateforme de système d’exploitation prise en charge par SQL Server. Pour plus d’informations, consultez [Extensions de langage](../../language-extensions/language-extensions-overview.md).

> [!NOTE]
> **R** et **Python** étant des noms réservés, aucun langage externe ne peut être créé avec ces derniers. Pour plus d’informations sur l’utilisation de **R** et de **Python**, consultez [SQL Server Machine Learning Services](../../machine-learning/index.yml).

## <a name="syntax"></a>Syntaxe

```syntaxsql
CREATE EXTERNAL LANGUAGE language_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
[ ; ]  

<file_spec> ::=  
{
    ( CONTENT = { <external_lang_specifier> | <content_bits> },
    FILE_NAME = <external_lang_file_name>
    [ , PLATFORM = <platform> ]
    [ , PARAMETERS = <external_lang_parameters> ]
    [ , ENVIRONMENT_VARIABLES = <external_lang_env_variables> ] )
}

<external_lang_specifier> :: =  
{
    '[file_path\]os_file_name'  
}

<content_bits> :: =  
{
    varbinary_literal
    | varbinary_expression
}

<external_lang_file_name> :: =  
'extension_file_name'


<platform> :: =
{
    WINDOWS
  | LINUX
}

<external_lang_parameters> :: =  
'extension_specific_parameters'
```

### <a name="arguments"></a>Arguments

**language_name**

Les langages sont des objets dont l’étendue est limitée à la base de données. Les noms de langage doivent être uniques dans la base de données.

**owner_name**

Spécifie le nom de l’utilisateur ou du rôle propriétaire du langage externe. En l'absence de spécification, la propriété revient à l'utilisateur actuel. Suivant les autorisations, il peut s’avérer nécessaire d’accorder à d’autres utilisateurs une autorisation explicite pour exécuter des scripts à l’aide d’un langage spécifique.

**file_spec**

Spécifie le contenu de l’extension de langage. Une seule spécification de fichier est autorisée pour un langage spécifique par plateforme.

**external_lang_specifier**

Chemin complet du fichier .zip ou tar.gz contenant le code des extensions. Ce contenu peut être le chemin d’un fichier .zip (sur Windows) ou d’un fichier tar.gz (sur Linux).

**content_bits**

Spécifie le contenu du langage en tant que littéral hexadécimal, similaire aux assemblys.

Cette option est utile si vous devez créer un langage ou modifier un langage existant (et que vous disposez des autorisations nécessaires), mais que le système de fichiers sur le serveur est restreint et vous ne pouvez pas copier les fichiers de bibliothèque vers un emplacement accessible au serveur.

**external_lang_file_name**

Nom du fichier d’extension .dll ou .so. Cet argument est nécessaire pour identifier le fichier approprié, au cas où il existerait plusieurs fichiers .dll ou .so dans le fichier .zip ou tar.gz défini par <external_lang_specifier>.

**external_lang_parameters**

Cet argument permet de donner un ensemble de paramètres au runtime de langage externe. Les valeurs de paramètre sont fournies au runtime externe une fois que le processus externe a démarré. Toutefois, les variables d’environnement sont accessibles à l’extension de langage avant le démarrage du processus externe.

**external_lang_env_variables**

Cet argument permet de donner un ensemble de variables d’environnement au runtime de langage externe avant le démarrage du processus externe. Un exemple de variable d’environnement est le répertoire de base du runtime lui-même. Par exemple : JRE_HOME.

**platform**

Ce paramètre est nécessaire pour les scénarios de systèmes d’exploitation hybrides. Dans une architecture hybride, le langage doit être inscrit une fois par plateforme. La plateforme et le nom du langage constituent la clé unique par langage externe. Si aucune plateforme n’est spécifiée, le système d’exploitation actuel est pris en considération.

## <a name="permissions"></a>Autorisations

Nécessite l’autorisation `CREATE EXTERNAL LANGUAGE`. Par défaut, les utilisateurs qui ont **dbo**, membre du rôle **db_owner**, disposent des autorisations nécessaires pour créer un langage externe. En ce qui concerne les autres utilisateurs, vous devez leur en donner l’autorisation explicitement à l’aide d’une instruction [GRANT](./grant-database-permissions-transact-sql.md), en spécifiant le privilège CREATE EXTERNAL LANGUAGE.

La modification d’une bibliothèque nécessite l’autorisation distincte `ALTER ANY EXTERNAL LANGUAGE`.

### <a name="execute-external-script-permission"></a>Autorisation EXECUTE EXTERNAL SCRIPT

Vous pouvez utiliser des autorisations EXECUTE EXTERNAL SCRIPT afin qu’une exécution de script externe puisse être accordée sur des langages spécifiques. C’est différent de l’autorisation de base de données EXECUTE ANY EXTERNAL SCRIPT qui ne permet pas d’accorder d’autorisation d’exécution sur un langage spécifique.

Cela signifie que les utilisateurs non-**dbo** doivent se voir accorder une autorisation pour exécuter un langage spécifique :

```sql
GRANT EXECUTE EXTERNAL SCRIPT ON EXTERNAL LANGUAGE ::language_name 
TO database_principal_name;
```

### <a name="reference-permissions-to-external-libraries"></a>Autorisations de référence sur les bibliothèques externes

À l’image des assemblys, des autorisations de référence sont nécessaires pour les langages externes afin qu’il existe un lien entre les bibliothèques externes et ces derniers. Par exemple, si un langage externe est sur le point d’être supprimé, l’utilisateur doit d’abord s’assurer que toutes les bibliothèques externes faisant référence à ce langage sont supprimées. Vous pouvez considérer le langage externe comme un objet de niveau plus élevé que les bibliothèques externes dans une hiérarchie.

## <a name="examples"></a>Exemples

### <a name="a-create-an-external-language-in-a-database"></a>R. Créer un langage externe dans une base de données  

L’exemple suivant ajoute un langage externe appelé Java à une base de données sur SQL Server sur Windows.

```sql
CREATE EXTERNAL LANGUAGE Java 
FROM (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

### <a name="b-create-an-external-language-for-both-windows-and-linux"></a>B. Créer un langage externe pour Windows et Linux

Vous pouvez spécifier jusqu’à deux `<file_spec>`, un pour Windows et l’autre pour Linux.

```sql
CREATE EXTERNAL LANGUAGE Java
FROM
(CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll', PLATFORM = WINDOWS),
(CONTENT = N'<path-to-tar.gz>', FILE_NAME = 'javaextension.so', PLATFORM = LINUX);
GO
```
### <a name="c-grant-permissions-to-execute-external-script"></a>C. Accorder des autorisations pour exécuter un script externe

L’exemple suivant permet au principal **mylogin** d’exécuter des scripts à l’aide du langage externe **Java**.

```sql
GRANT EXECUTE EXTERNAL SCRIPT ON EXTERNAL LANGUAGE ::Java 
TO mylogin;
```


## <a name="see-also"></a>Voir aussi

[ALTER EXTERNAL LANGUAGE (Transact-SQL)](alter-external-language-transact-sql.md)  
[DROP EXTERNAL LANGUAGE (Transact-SQL)](drop-external-language-transact-sql.md)  
[sys.external_languages](../../relational-databases/system-catalog-views/sys-external-languages-transact-sql.md)  
[sys.external_language_files](../../relational-databases/system-catalog-views/sys-external-language-files-transact-sql.md)