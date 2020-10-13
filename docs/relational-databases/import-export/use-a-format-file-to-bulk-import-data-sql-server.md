---
title: Utiliser un fichier de format pour importer des données en bloc
description: Dans SQL Server, vous pouvez utiliser un fichier de format dans les opérations d’importation en bloc. Un fichier de format met en relation les champs du fichier de données avec les colonnes de la table.
ms.date: 09/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], format files
- format files [SQL Server], importing data using
ms.assetid: 2956df78-833f-45fa-8a10-41d6522562b9
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 1699ac4a2ad49a6a65fafed6a75c71585514de51
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868049"
---
# <a name="use-a-format-file-to-bulk-import-data-sql-server"></a>Utiliser un fichier de format pour importer des données en bloc (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette rubrique illustre l'utilisation d'un fichier de format dans les importations en bloc.  Un fichier de format met en relation les champs du fichier de données avec les colonnes de la table.  Veuillez consulter [Créer un fichier de format (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) pour plus d’informations.
  
## <a name="before-you-begin"></a>Avant de commencer
* Pour qu’un fichier de format fonctionne avec un fichier de données de caractères Unicode, tous les champs d’entrée doivent être des chaînes de texte Unicode (autrement dit, des chaînes Unicode de taille fixe ou terminées par un caractère).
* Pour exporter ou importer en bloc des données [SQLXML](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md) , utilisez l’un des types de données ci-dessous dans votre fichier de format.
  * SQLCHAR ou SQLVARYCHAR (les données sont envoyées dans la page de codes du client ou dans la page de codes impliquée par le classement)
  * SQLNCHAR ou SQLNVARCHAR (les données sont envoyées au format Unicode)
  * SQLBINARY ou SQLVARYBIN (les données sont envoyées sans être converties).
* Azure SQL Database et Azure SQL Data Warehouse prennent uniquement en charge [bcp](../../tools/bcp-utility.md).  Si vous souhaitez en savoir plus, veuillez consulter :
  * [Chargement de données dans Azure SQL Data Warehouse](/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading)
  * [Charger des données à partir de SQL Server dans Azure SQL Data Warehouse (fichiers plats)](/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading)
  * [Migration de vos données](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-develop)

## <a name="example-test-conditions"></a>Exemples de conditions de test
Les fichiers de format pris en exemple dans cette rubrique sont fondés sur la table et le fichier de données définis ci-dessous.

### <a name="sample-table"></a>Exemple de table
Le script ci-dessous crée une base de données de test et une table nommée `myFirstImport`.  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.MyFirstImport (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30),
   BirthDate Date
   );
```

### <a name="sample-data-file"></a>Exemple de fichier de données
À l’aide du Bloc-notes, créez un fichier vide `D:\BCP\myFirstImport.bcp` et insérez les données suivantes :
```
1,Anthony,Grosse,1980-02-23
2,Alica,Fatnowna,1963-11-14
3,Stella,Rosenhain,1992-03-02
```

Vous pouvez aussi exécuter le script PowerShell suivant pour créer et remplir le fichier de données :
```powershell
Clear-Host
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = Join-Path -Path $dir -ChildPath 'MyFirstImport.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '1,Anthony,Grosse,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,1963-11-14';
Add-Content -Path $bcpFile -Value '3,Stella,Rosenhain,1992-03-02';

#Review content
Get-Content -Path $bcpFile;
Notepad.exe $bcpfile;
```

## <a name="creating-the-format-files"></a>Création des fichiers de format
SQL Server prend en charge deux types de fichier de format : format XML et format non XML.  Le format non XML est le format d’origine pris en charge dans les versions précédentes de SQL Server.

### <a name="creating-a-non-xml-format-file"></a>Création d'un fichier de format non XML
Veuillez consulter [Fichiers de format non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise [l’utilitaire bcp](../../tools/bcp-utility.md) pour générer un fichier de format non xml `myFirstImport.fmt`basé sur le schéma de `myFirstImport`.  Pour utiliser une commande bcp pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données.  L’option format nécessite également l’option **-f** .  Pour cet exemple, le qualificateur **c** est utilisé pour spécifier les données de caractère, **t** est utilisé pour spécifier une virgule comme [délimiteur de champ](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md), et **T** est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d'une invite de commandes, entrez la commande suivante :

```cmd
bcp TestDatabase.dbo.myFirstImport format nul -c -f D:\BCP\myFirstImport.fmt -t, -T

REM Review file
Notepad D:\BCP\myFirstImport.fmt
```

Votre fichier de format non XML `D:\BCP\myFirstImport.fmt` doit se présenter comme suit :
```
13.0
4
1       SQLCHAR             0       7       ","      1     PersonID               ""
2       SQLCHAR             0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR             0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR             0       11      "\r\n"   4     BirthDate              ""
```

> [!IMPORTANT]
> Vérifiez que votre fichier de format non XML se termine par un retour charriot\saut de ligne.  Sinon, vous recevez probablement le message d’erreur suivant :
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

### <a name="creating-an-xml-format-file"></a>Création d'un fichier de format XML
Veuillez consulter [Fichiers de format XML (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise l’ [utilitaire bcp](../../tools/bcp-utility.md) pour créer un fichier de format xml `myFirstImport.xml`basé sur le schéma de `myFirstImport`. Pour utiliser une commande bcp pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données.  L’option de format nécessite toujours l’option **-f** . De plus, pour créer un fichier de format XML, vous devez spécifier l’option **-x** .  Pour cet exemple, le qualificateur **c** est utilisé pour spécifier les données de caractère, **t** est utilisé pour spécifier une virgule comme [délimiteur de champ](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md), et **T** est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d'une invite de commandes, entrez la commande suivante :
```cmd
bcp TestDatabase.dbo.myFirstImport format nul -c -x -f D:\BCP\myFirstImport.xml -t, -T

REM Review file
Notepad D:\BCP\myFirstImport.xml
```

Votre fichier de format XML `D:\BCP\myFirstImport.xml` doit se présenter comme suit :
```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="http://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="11"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="4" NAME="BirthDate" xsi:type="SQLDATE"/>
 </ROW>
</BCPFORMAT>
```

## <a name="using-a-format-file-to-bulk-import-data"></a>Importation en bloc des données à l’aide d’un fichier de format
Les exemples ci-dessous utilisent la base de données, le fichier de données et les fichiers de format créés ci-dessus.

### <a name="using-bcp-and-non-xml-format-file"></a>Utilisation de [bcp](../../tools/bcp-utility.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
À partir d'une invite de commandes, entrez la commande suivante :
```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.MyFirstImport;"

REM Import data
bcp TestDatabase.dbo.myFirstImport IN D:\BCP\myFirstImport.bcp -f D:\BCP\myFirstImport.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.MyFirstImport"
```


### <a name="using-bcp-and-xml-format-file"></a>Utilisation de [bcp](../../tools/bcp-utility.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)
À partir d'une invite de commandes, entrez la commande suivante :
```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.MyFirstImport;"

REM Import data
bcp TestDatabase.dbo.myFirstImport IN D:\BCP\myFirstImport.bcp -f D:\BCP\myFirstImport.xml -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.MyFirstImport;"
```


### <a name="using-bulk-insert-and-non-xml-format-file"></a>Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
BULK INSERT dbo.myFirstImport   
   FROM 'D:\BCP\myFirstImport.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myFirstImport.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-bulk-insert-and-xml-format-file"></a>Utilisation de [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)
Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
BULK INSERT dbo.myFirstImport   
   FROM 'D:\BCP\myFirstImport.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myFirstImport.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-openrowsetbulk-and-non-xml-format-file"></a>Utilisation d’[OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et d’un [fichier de format non-XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)    
Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
INSERT INTO dbo.myFirstImport
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myFirstImport.bcp',
        FORMATFILE = 'D:\BCP\myFirstImport.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-openrowsetbulk-and-xml-format-file"></a>[Utilisation d’OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) et d’un [fichier de format XML](../../relational-databases/import-export/xml-format-files-sql-server.md)
Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
INSERT INTO dbo.myFirstImport 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myFirstImport.bcp',
        FORMATFILE = 'D:\BCP\myFirstImport.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```
  
## <a name="more-examples"></a>Plus d'exemples
 [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
 [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
 [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
 [Utiliser un fichier de format pour mapper les colonnes d’une table sur les champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [Fichiers de format XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md)  
  [Fichiers de format pour l'importation ou l'exportation de données (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)
