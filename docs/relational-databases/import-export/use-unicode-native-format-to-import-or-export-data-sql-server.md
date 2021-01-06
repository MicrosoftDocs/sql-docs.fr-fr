---
title: Utiliser le format natif Unicode pour importer ou exporter des données (SQL Server) | Microsoft Docs
description: Utilisez le format natif Unicode pour le transfert en bloc de données entre des instances de SQL Server, ce qui élimine la conversion des types de données vers et à partir du format caractère.
ms.custom: ''
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- Unicode [SQL Server], bulk importing and exporting
- data formats [SQL Server], Unicode native
ms.assetid: a6213308-f3d5-406e-9029-19d8bb3367f3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 977e7c4222a200d697304be0be774214fbc4376a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643458"
---
# <a name="use-unicode-native-format-to-import-or-export-data-sql-server"></a>Utiliser le format natif Unicode pour importer ou exporter des données (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Le format natif Unicode est utile quand vous devez copier des informations d’une installation de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers une autre. L'utilisation du format natif pour les données qui ne sont pas de type caractère (char) permet de gagner du temps, puisque vous supprimez les conversions inutiles des types de données depuis et vers le format caractère. L'utilisation du format caractère Unicode pour toutes les données de type caractère évite la perte des caractères étendus lorsque vous transférez en bloc des données entre des serveurs utilisant des pages de codes différentes. Un fichier de données au format natif Unicode peut être lu par toutes les méthodes d'importation en bloc.  
  
 Le format natif Unicode est recommandé pour le transfert en bloc des données entre plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par le biais d'un fichier de données qui contient des caractères étendus ou des caractères codés sur deux octets (DBCS). Pour les données qui ne sont pas de type caractère, le format natif Unicode utilise des types de données (bases de données) natifs. Pour les données de type caractère, comme [char](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [texte](../../t-sql/data-types/ntext-text-and-image-transact-sql.md), [varchar (max)](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nvarchar (max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)et [ntext](../../t-sql/data-types/ntext-text-and-image-transact-sql.md), le format natif Unicode utilise le format de données de type caractère Unicode.  
  
 Les données [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) qui sont stockées en tant que SQLVARIANT dans un fichier de données au format natif Unicode fonctionnent de la même manière que dans un fichier de données au format natif, sauf que les valeurs [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) et [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) sont converties en [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) et [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), ce qui double l’espace de stockage nécessaire pour les colonnes concernées. Les métadonnées d’origine sont conservées et les valeurs sont reconverties dans leur type de données d’origine ( [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) et [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) ) au moment où elles sont importées en bloc dans une colonne de table.  
 
 |Dans cette rubrique :|
|---|
|[Options de commande pour le format natif Unicode](#command_options)|
|[Exemples de conditions de test](#etc)<br />&emsp;&#9679;&emsp;[Exemple de table](#sample_table)<br />&emsp;&#9679;&emsp;[Exemple de fichier de format non XML](#nonxml_format_file)|
|[Exemples](#examples)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format natif Unicode pour exporter des données](#bcp_widenative_export)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format natif Unicode pour importer des données sans un fichier de format](#bcp_widenative_import)<br />&emsp;&#9679;&emsp;[Utilisation de bcp et du format natif Unicode pour importer des données avec un fichier de format non XML](#bcp_widenative_import_fmt)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et du format natif Unicode sans un fichier de format](#bulk_widenative)<br />&emsp;&#9679;&emsp;[Utilisation de BULK INSERT et du format natif Unicode avec un fichier de format non XML](#bulk_widenative_fmt)<br />&emsp;&#9679;&emsp;[Utilisation d’OPENROWSET et du format natif Unicode avec un fichier de format non XML](#openrowset_widenative_fmt)|
|[Tâches associées](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|
  
## <a name="command-options-for-unicode-native-format"></a>Options de commande pour le format natif Unicode<a name="command_options"></a>  
Vous pouvez importer des données au format natif Unicode dans une table à l’aide de [bcp](../../tools/bcp-utility.md), [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) ou [INSERT... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Si vous utilisez une commande [bcp](../../tools/bcp-utility.md) ou une instruction [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), vous pouvez spécifier le format de données dans l’instruction.  Pour une instruction [INSERT... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md), vous devez spécifier le format de données dans un fichier de format.  
  
Le format natif Unicode est pris en charge par les options de commande suivantes :  
  
|Commande|Option|Description|  
|-------------|------------|-----------------|  
|bcp|**-N**|Amène l’utilitaire **bcp** à utiliser le format natif Unicode, qui utilise des types de données (bases de données) natifs pour toutes les données qui ne sont pas de type caractère, et le format de données de type caractère Unicode pour toutes les données de type caractère (**char**, **nchar**, **varchar**, **nvarchar**, **text** et **ntext**).|  
|BULK INSERT|DATAFILETYPE **='widenative'**|Utilise le format natif Unicode lors de l’importation en bloc des données.|  
|OPENROWSET|N/A|Doit utiliser un fichier de format.|
    
> [!NOTE]
>  Vous pouvez également spécifier le formatage par champ dans un fichier de format. Pour plus d’informations, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  
## <a name="example-test-conditions"></a>Exemples de conditions de test<a name="etc"></a>  
Les exemples de cette rubrique sont fondés sur la table et le fichier de format définis ci-dessous.

### <a name="sample-table"></a>**Exemple de table**<a name="sample_table"></a>
Le script ci-dessous crée une base de données test, une table nommée `myWidenative` et remplit la table avec des valeurs initiales.  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myWidenative ( 
    PersonID smallint NOT NULL,
    FirstName nvarchar(25) NOT NULL,
    LastName nvarchar(30) NOT NULL,
    BirthDate date,
    AnnualSalary money
);

-- Populate table
INSERT TestDatabase.dbo.myWidenative
VALUES 
(1, N'ϴAnthony', N'Grosse', '02-23-1980', 65000.00),
(2, N'❤Alica', N'Fatnowna', '11-14-1963', 45000.00),
(3, N'☎Stella', N'Rossenhain', '03-02-1992', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="sample-non-xml-format-file"></a>**Exemple de fichier de format non XML**<a name="nonxml_format_file"></a>
SQL Server prend en charge deux types de fichier de format : format XML et format non XML.  Le format non XML est le format d’origine pris en charge dans les versions précédentes de SQL Server.  Veuillez consulter [Fichiers de format non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise [l’utilitaire bcp](../../tools/bcp-utility.md) pour générer un fichier de format non xml `myWidenative.fmt`basé sur le schéma de `myWidenative`.  Pour utiliser une commande [bcp](../../tools/bcp-utility.md) pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données.  L’option format nécessite également l’option **-f** .  De plus, pour cet exemple, le qualificateur **c** est utilisé pour spécifier les données de type caractère et **T** est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d’une invite de commandes, entrez les commandes suivantes :

```
bcp TestDatabase.dbo.myWidenative format nul -f D:\BCP\myWidenative.fmt -T -N

REM Review file
Notepad D:\BCP\myWidenative.fmt
```

> [!IMPORTANT]
> Vérifiez que votre fichier de format non XML se termine par un retour charriot\saut de ligne.  Sinon, vous recevez probablement le message d’erreur suivant :
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Exemples<a name="examples"></a>
Les exemples ci-dessous utilisent la base de données et les fichiers de format créés ci-dessus.

### <a name="using-bcp-and-unicode-native-format-to-export-data"></a>**Utilisation de bcp et du format natif Unicode pour exporter des données**<a name="bcp_widenative_export"></a>
Commutateur **-N** et commande **OUT** .  Remarque : Le fichier de données créé dans cet exemple est utilisé dans tous les exemples suivants.  À partir d’une invite de commandes, entrez les commandes suivantes :
```
bcp TestDatabase.dbo.myWidenative OUT D:\BCP\myWidenative.bcp -T -N

REM Review results
NOTEPAD D:\BCP\myWidenative.bcp
```

### <a name="using-bcp-and-unicode-native-format-to-import-data-without-a-format-file"></a>**Utilisation de bcp et du format natif Unicode pour importer des données sans un fichier de format**<a name="bcp_widenative_import"></a>
Commutateur **-N** et commande **IN** .  À partir d’une invite de commandes, entrez les commandes suivantes :
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidenative;"

REM Import data
bcp TestDatabase.dbo.myWidenative IN D:\BCP\myWidenative.bcp -T -N

REM Review results is SSMS
```

### <a name="using-bcp-and-unicode-native-format-to-import-data-with-a-non-xml-format-file"></a>**Utilisation de bcp et du format natif Unicode pour importer des données avec un fichier de format non XML**<a name="bcp_widenative_import_fmt"></a>
Commutateurs **-N** et **-f** switches et **IN** commet.  À partir d’une invite de commandes, entrez les commandes suivantes :
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidenative;"

REM Import data
bcp TestDatabase.dbo.myWidenative IN D:\BCP\myWidenative.bcp -f D:\BCP\myWidenative.fmt -T -N

REM Review results is SSMS
```

### <a name="using-bulk-insert-and-unicode-native-format-without-a-format-file"></a>**Utilisation de BULK INSERT et du format natif Unicode sans un fichier de format**<a name="bulk_widenative"></a>
Argument **DATAFILETYPE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative; -- for testing
BULK INSERT TestDatabase.dbo.myWidenative
    FROM 'D:\BCP\myWidenative.bcp'
    WITH (
        DATAFILETYPE = 'widenative'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="using-bulk-insert-and-unicode-native-format-with-a-non-xml-format-file"></a>**Utilisation de BULK INSERT et du format natif Unicode avec un fichier de format non XML**<a name="bulk_widenative_fmt"></a>
Argument **FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative; -- for testing
BULK INSERT TestDatabase.dbo.myWidenative
   FROM 'D:\BCP\myWidenative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myWidenative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="using-openrowset-and-unicode-native-format-with-a-non-xml-format-file"></a>**Utilisation d’OPENROWSET et du format natif Unicode avec un fichier de format non XML**<a name="openrowset_widenative_fmt"></a>
Argument **FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative;  -- for testing
INSERT INTO TestDatabase.dbo.myWidenative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myWidenative.bcp', 
        FORMATFILE = 'D:\BCP\myWidenative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

## <a name="related-tasks"></a>Tâches associées<a name="RelatedTasks"></a>
Pour utiliser des formats de données pour l'importation ou l'exportation en bloc  
-   [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format natif pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format caractère Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
  
  
