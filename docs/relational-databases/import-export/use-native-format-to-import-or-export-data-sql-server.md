---
title: Utiliser le format natif pour importer et exporter des données
description: Dans le cadre de l’importation ou de l’exportation de données dans SQL Server, le format natif conserve les types de données natifs d’une base de données pour le transfert de données haut débit entre tables SQL Server.
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- native data format [SQL Server]
- data formats [SQL Server], native
ms.assetid: eb279b2f-0f1f-428f-9b8f-2a7fc495b79f
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 54374ae5337abcd49cc2d2eefedc25d36ceee697
ms.sourcegitcommit: 039fb38c583019b3fd06894160568387a19ba04e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87442776"
---
# <a name="use-native-format-to-import-or-export-data-sql-server"></a>Utiliser le format natif pour importer ou exporter des données (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Il est recommandé d'utiliser le format natif lorsque vous transférez des données en bloc entre plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen d'un fichier de données qui ne contient pas de caractères appartenant à un jeu de caractères étendus/codés sur deux octets (DBCS).  

> [!NOTE]
>  Pour transférer des données en bloc entre plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moyen d'un fichier de données qui contient des caractères appartenant à un jeu de caractères étendus/codés sur deux octets, utilisez de préférence le format natif Unicode. Pour plus d’informations, consultez [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).

Le format natif préserve les types de données native d'une base de données. Il est destiné aux transferts de données à haute vitesse entre des tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous utilisez un fichier de format, les tables source et cible ne doivent pas nécessairement être identiques. Le transfert de données se déroule en deux étapes :  
  
1.  exportation en bloc des données d'une table source vers un fichier de données ;  
  
2.  importation en bloc des données d'un fichier de données vers la table cible.  

L'utilisation d'un format natif entre tables identiques permet de gagner du temps et de l'espace puisque les conversions superflues des types de données à partir de et vers le format caractère sont éliminées. Pour atteindre la vitesse de transfert optimale, quelques contrôles sont effectués au niveau du formatage des données. Pour empêcher que des problèmes ne surviennent au niveau des données chargées, tenez compte des restrictions suivantes.  


## <a name="restrictions"></a>Restrictions
Pour importer des données au format natif, veillez à ce que les conditions suivantes soient réunies :  
  
-   Le fichier de données doit être au format natif.  
  
-   Soit la table cible doit être compatible avec le fichier de données (qui doit présenter le nombre voulu de colonnes, avec les mêmes types de données, longueur de colonne, état NULL, etc.), soit vous devez utiliser un fichier de format pour mapper chacun des champs sur sa colonne correspondante.  
  
    > [!NOTE]
    >  Si vous importez des données d'un fichier qui ne concorde pas avec la table cible, l'importation peut réussir mais les valeurs de données insérées dans la table cible peuvent être incorrectes. En effet, les données du fichier sont interprétées en se basant sur le format de la table cible. Par conséquent, tout défaut de correspondance entraîne l'insertion de valeurs incorrectes. Cependant, en aucun cas une telle non-correspondance peut-elle provoquer des incohérences logiques ou physiques dans la base de données.  
  
     Pour plus d’informations sur l’utilisation de fichiers de format, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 Une importation réussie n'endommage pas la table cible.  
  
## <a name="how-bcp-handles-data-in-native-format"></a>Traitement des données au format natif par l’utilitaire bcp
 Cette section comporte des remarques spécifiques liées à la manière dont l'utilitaire **bcp** exporte et importe les données au format natif.  
  
-   Données non-caractère  
  
     [L’utilitaire bcp](../../tools/bcp-utility.md) emploie le format de données binaires interne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour écrire des données qui ne sont pas de type caractère d’une table vers un fichier de données.  
  
-   Données[char](../../t-sql/data-types/char-and-varchar-transact-sql.md) ou [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md)  
  
     Au début de chaque champ [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) ou [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) , [bcp](../../tools/bcp-utility.md) ajoute la longueur du préfixe.  
  
    > [!IMPORTANT]
    >  En mode natif, [l’utilitaire bcp](../../tools/bcp-utility.md) convertit par défaut les caractères de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en caractères OEM avant de les copier dans un fichier de données. [L’utilitaire bcp](../../tools/bcp-utility.md) convertit les caractères d’un fichier de données en caractères ANSI avant de les importer en bloc dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Au cours de ces conversions, il est possible que des données caractères étendus soient perdues. Dans le cas de caractères étendus, vous devez soit utiliser le format natif Unicode, soit spécifier une page de codes.
  
-   Données[sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)  
  
     Si des données [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) sont stockées en tant que SQLVARIANT dans un fichier de données au format natif, elles conservent l’ensemble de leurs caractéristiques. Les métadonnées qui enregistrent le type de données de chaque valeur de données sont stockées en même temps que celle-ci. Les métadonnées sont employées pour recréer la valeur de données avec le même type de données dans une colonne [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) de destination.  
  
     Si le type de données de la colonne de destination n’est pas [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md), chaque valeur de données est convertie dans le type de données de la colonne de destination, suivant les règles normales de la conversion implicite de données. Si une erreur survient pendant la conversion des données, le traitement actif est annulé. Toute valeur [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) ou [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) transférée entre des colonnes [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) peut faire l’objet de problèmes de conversion des pages de codes.  
  
     Pour plus d’informations sur la conversion de données, consultez [Conversion de types de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md).  
  
## <a name="command-options-for-native-format"></a>Options de commande pour le format natif
Vous pouvez importer des données au format natif dans une table à l’aide de l’utilitaire [bcp](../../tools/bcp-utility.md) ou des instructions [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) ou [INSERT... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Si vous utilisez une commande [bcp](../../tools/bcp-utility.md) ou une instruction [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), vous pouvez spécifier le format de données dans l’instruction.  Pour une instruction [INSERT... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md), vous devez spécifier le format de données dans un fichier de format.  

Le format natif est pris en charge par les options de commande suivantes :  

|Commande|Option|Description|  
|-------------|------------|-----------------|  
|bcp|**-n**|Force l’utilitaire bcp à utiliser les types de données natifs des données.*|  
|BULK INSERT|DATAFILETYPE **='native'**|Utilise les types de données native ou widenative des données. Notez que DATAFILETYPE n'est pas nécessaire si un fichier de format spécifie les types de données.|  
|OPENROWSET|N/A|Doit utiliser un fichier de format.|

  
 \*Pour charger des données au format natif ( **-n**) dans un format compatible avec des versions antérieures de clients [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilisez le commutateur **-V** . Pour plus d’informations, consultez [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
  
> [!NOTE]
>  Vous pouvez également spécifier le formatage par champ dans un fichier de format. Pour plus d’informations, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  

## <a name="example-test-conditions"></a>Exemples de conditions de test
Les exemples de cette rubrique sont fondés sur la table et le fichier de format définis ci-dessous.

### <a name="sample-table"></a>Exemple de table
Le script ci-dessous crée une base de données test, une table nommée `myNative` et remplit la table avec des valeurs initiales.  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myNative ( 
   PersonID smallint NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date,
   AnnualSalary money
   );

-- Populate table
INSERT TestDatabase.dbo.myNative
VALUES 
(1, 'Anthony', 'Grosse', '1980-02-23', 65000.00),
(2, 'Alica', 'Fatnowna', '1963-11-14', 45000.00),
(3, 'Stella', 'Rossenhain', '1992-03-02', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="sample-non-xml-format-file"></a>Exemple de fichier de format non XML
SQL Server prend en charge deux types de fichier de format : format XML et format non XML.  Le format non XML est le format d’origine pris en charge dans les versions précédentes de SQL Server.  Veuillez consulter [Fichiers de format non XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) pour obtenir des informations détaillées.  La commande suivante utilise [l’utilitaire bcp](../../tools/bcp-utility.md) pour générer un fichier de format non xml `myNative.fmt`basé sur le schéma de `myNative`.  Pour utiliser une commande [bcp](../../tools/bcp-utility.md) pour créer un fichier de format, spécifiez l’argument **format** et utilisez **nul** à la place d’un chemin de fichier de données.  L’option format nécessite également l’option **-f** .  De plus, pour cet exemple, le qualificateur **c** est utilisé pour spécifier les données de type caractère et **T** est utilisé pour spécifier une connexion approuvée à l’aide de la sécurité intégrée.  À partir d’une invite de commandes, entrez les commandes suivantes :

```cmd
bcp TestDatabase.dbo.myNative format nul -f D:\BCP\myNative.fmt -T 

REM Review file
Notepad D:\BCP\myNative.fmt
```

> [!IMPORTANT]
> Vérifiez que votre fichier de format non XML se termine par un retour charriot\saut de ligne.  Sinon, vous recevez probablement le message d’erreur suivant :
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Exemples
Les exemples ci-dessous utilisent la base de données et les fichiers de format créés ci-dessus.

### <a name="using-bcp-and-native-format-to-export-data"></a>Exportation des données à l’aide de l’utilitaire bcp et du format natif
Commutateur **-n** et commande **OUT** .  Remarque : Le fichier de données créé dans cet exemple est utilisé dans tous les exemples suivants.  À partir d’une invite de commandes, entrez les commandes suivantes :

```cmd
bcp TestDatabase.dbo.myNative OUT D:\BCP\myNative.bcp -T -n

REM Review results
NOTEPAD D:\BCP\myNative.bcp
```

### <a name="using-bcp-and-native-format-to-import-data-without-a-format-file"></a>Importation des données sans fichier de format à l’aide de l’utilitaire bcp et du format natif
Commutateur **-n** et commande **IN** .  À partir d’une invite de commandes, entrez les commandes suivantes :

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -T -n

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bcp-and-native-format-to-import-data-with-a-non-xml-format-file"></a>Importation des données sans fichier de format non XML à l’aide de l’utilitaire bcp et du format natif
Commutateurs **-n** et **-f** switches et **IN** commet.  À partir d’une invite de commandes, entrez les commandes suivantes :

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -f D:\BCP\myNative.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bulk-insert-and-native-format-without-a-format-file"></a>Utilisation de BULK INSERT et du format natif sans fichier de format
Argument**DATAFILETYPE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
    FROM 'D:\BCP\myNative.bcp'
    WITH (
        DATAFILETYPE = 'native'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-bulk-insert-and-native-format-with-a-non-xml-format-file"></a>Utilisation de BULK INSERT et du format natif avec un fichier de format non XML
Argument**FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
   FROM 'D:\BCP\myNative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-openrowset-and-native-format-with-a-non-xml-format-file"></a>Utilisation d’OPENROWSET et du format natif avec un fichier de format non XML
Argument**FORMATFILE** .  Exécutez l’instruction Transact-SQL suivante dans Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) :

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative;  -- for testing
INSERT INTO TestDatabase.dbo.myNative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNative.bcp', 
        FORMATFILE = 'D:\BCP\myNative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```
  
## <a name="related-tasks"></a>Tâches associées
Pour utiliser des formats de données pour l'importation ou l'exportation en bloc 
  
-   [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format caractère Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)   
 [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
  
