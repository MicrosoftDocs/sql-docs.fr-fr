---
title: Formater des fichiers pour importer et exporter des données
description: Lorsque vous importez en bloc des données dans une table SQL Server ou que vous les exportez en bloc à partir d’une table, un fichier de format peut stocker les informations de format des champs pour un fichier de données relatif à une table.
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], format files
- bulk importing [SQL Server], format files
- format files [SQL Server]
ms.assetid: b7b97d68-4336-4091-aee4-1941fab568e3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 35650bcd31127dcf3eaad46be4c958bc8f94547e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351384"
---
# <a name="format-files-to-import-or-export-data-sql-server"></a>Formater des fichiers pour importer ou exporter des données (SQL Server)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Lorsque vous importez en bloc des données dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou exportez en bloc des données depuis une table, utilisez un *fichier de format* pour stocker toutes les informations de format nécessaires à l'exportation ou l'importation en bloc des données. Cela inclut les informations de format pour chaque champ dans un fichier de données relatif à cette table.

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] prend en charge deux types de fichiers de format : les fichiers de formats XML et de format non-XML. Les fichiers de format XML et non-XML contiennent la description de chacun des champs d'un fichier de données, et les fichiers de format XML contiennent également des descriptions des colonnes de table correspondantes. En règle générale, les fichiers de format XML et non-XML sont interchangeables. Toutefois, nous recommandons d'utiliser la syntaxe XML pour les nouveaux fichiers de format, car elle offre plusieurs avantages par rapport aux fichiers de format non-XML. Pour plus d’informations, consultez [Fichiers de format XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md).

## <a name="benefits-of-format-files"></a><a name="Benefits"></a> Avantages des fichiers de format

- Fournit un système souple d'écriture de fichiers de données, nécessitant peu ou aucune édition pour se conformer aux autres formats de données, ou de lecture de fichiers de données provenant d'autres logiciels.
- Vous permet d'importer les données en bloc sans avoir à ajouter ou supprimer des données inutiles ou à réorganiser les données du fichier de données. Les fichiers de format sont particulièrement utiles lorsqu'il existe des différences entre les champs du fichier de données et les colonnes dans la table.

## <a name="examples-of-format-files"></a><a name="ExamplesOfFFs"></a> Exemples de fichiers de format

Les exemples suivants illustrent la structure d'un fichier de format XML et non XML. Ces fichiers de format correspondent à la table `HumanResources.myTeam` dans l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Cette table contient quatre colonnes : `EmployeeID`, `Name`, `Title`et `ModifiedDate`.

> [!NOTE]
> Pour plus d’informations sur cette table et la manière de la créer, consultez [Exemple de table HumanResources.myTeam &#40;SQL Server&#41;](../../relational-databases/import-export/humanresources-myteam-sample-table-sql-server.md).

### <a name="a-using-a-non-xml-format-file"></a>R. Utilisation d'un fichier de format non-XML

Le fichier de format non XML suivant utilise le format de données natif [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la table `HumanResources.myTeam` . Ce fichier de format a été créé à l'aide de la commande `bcp` suivante.

```cmd
bcp AdventureWorks.HumanResources.myTeam format nul -f myTeam.Fmt -n -T
The contents of this format file are as follows: 9.0
4
1       SQLSMALLINT   0       2       ""   1     EmployeeID               ""  
2       SQLNCHAR      2       100     ""   2     Name                     SQL_Latin1_General_CP1_CI_AS  
3       SQLNCHAR      2       100     ""   3     Title                    SQL_Latin1_General_CP1_CI_AS  
4       SQLNCHAR      2       100     ""   4     Background               SQL_Latin1_General_CP1_CI_AS  
```  

Pour plus d’informations, consultez [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).

### <a name="b-using-an-xml-format-file"></a>B. Utilisation d'un fichier de format XML

Le fichier de format XML suivant utilise le format de données natif [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la table `HumanResources.myTeam` . Ce fichier de format a été créé à l'aide de la commande `bcp` suivante.

```cmd
bcp AdventureWorks.HumanResources.myTeam format nul -f myTeam.Xml -x -n -T
```

Le fichier de format contient :

```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<RECORD>
  <FIELD ID="1" xsi:type="NativePrefix" LENGTH="1"/>
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="EmployeeID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>
  <COLUMN SOURCE="3" NAME="Title" xsi:type="SQLNVARCHAR"/>
  <COLUMN SOURCE="4" NAME="Background" xsi:type="SQLNVARCHAR"/>
</ROW>
</BCPFORMAT>
```

Pour plus d’informations, consultez [Fichiers de format XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md). 

## <a name="when-is-a-format-file-required"></a><a name="WhenFFrequired"></a> Quand faut-il utiliser un format de fichier ?

- Une instruction INSERT ... SELECT * FROM OPENROWSET(BULK...) nécessite toujours un fichier de format.
- Pour **bcp** ou BULK INSERT, dans les cas simples, l’utilisation d’un fichier de format est facultative et rarement nécessaire. Toutefois, pour les importations en bloc complexes, un fichier de format est souvent nécessaire.

Des fichiers de format sont nécessaires dans les cas suivants :

- Le même fichier de données est utilisé comme source pour plusieurs tables possédant des schémas différents.
- Le fichier de données contient un nombre de champs différent du nombre de colonnes dans la table cible. Par exemple :

  - La table cible contient au moins une colonne pour laquelle une valeur par défaut est définie ou une valeur NULL est autorisée.
  - Les utilisateurs n'ont pas les autorisations SELECT/INSERT sur une ou plusieurs colonnes de la table.
  - Un seul fichier de données est utilisé avec au moins deux tables dont les schémas sont différents.

- L'ordre des colonnes est différent dans le fichier des données et la table.
- Les caractères de fin ou les longueurs de préfixes sont différents dans les colonnes du fichier de données.

> [!NOTE]
> S’il n’existe pas de fichier de format et si une commande **bcp** définit un commutateur de format de données ( **-n**, **-c**, **-w** ou **-N**) ou si une opération BULK INSERT définit l’option DATAFILETYPE, le format de données défini est utilisé comme méthode par défaut pour interpréter les champs du fichier de données.

## <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées

- [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)
- [Utiliser un fichier de format pour importer des données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Utiliser un fichier de format pour ignorer un champ de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)
- [Utiliser un fichier de format pour mapper les colonnes d’une table sur les champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>Voir aussi

- [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
- [Fichiers de format XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md)
- [Formats de données pour l’importation ou l’exportation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)
