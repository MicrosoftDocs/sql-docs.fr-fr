---
title: Importer des données d’Excel vers SQL | Microsoft Docs
description: Cet article décrit les méthodes permettant d’importer des données à partir d’Excel vers SQL Server ou Azure SQL Database. Certaines ne nécessitent qu’une seule étape, tandis que d’autres ont besoin d’un fichier texte intermédiaire.
ms.custom: sqlfreshmay19
ms.date: 09/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 84fce410f5ee03301b5ad8e5fe0aa61698b52e0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351338"
---
# <a name="import-data-from-excel-to-sql-server-or-azure-sql-database"></a>Importer des données d’Excel vers SQL Server ou Azure SQL Database

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Il existe plusieurs moyens d’importer des données provenant de fichiers Excel vers SQL Server ou Azure SQL Database. Certaines méthodes permettent d’importer directement des données à partir de fichiers Excel, en une seule étape ; d’autres impliquent d’exporter les données Excel au format texte (fichier CSV) pour pouvoir les importer. Cet article récapitule les méthodes fréquemment utilisées et comporte des liens vers des informations plus détaillées.

## <a name="list-of-methods"></a>Liste des méthodes

Vous pouvez utiliser les outils suivants pour importer des données à partir d’Excel :

| Exporter au format texte en premier (SQL Server et SQL Database) | Directement à partir d’Excel (SQL Server local uniquement) |
| :------------------------------------------------- |:------------------------------------------------- |
| [Assistant Importation de fichier plat](#import-wiz)             |[Assistant Importation et Exportation SQL Server](#wiz)        |
| [BULK INSERT](#bulk-insert), instruction              |[SQL Server Integration Services (SSIS)](#ssis)    |
| [BCP](#bcp)                                        |[OPENROWSET](#openrowset), fonction <br>            |
| [Assistant Copie (Azure Data Factory)](#adf-wiz)       |                                                   |
| [Azure Data Factory](#adf).                         |                                                   |
| &nbsp; | &nbsp; |

Si vous voulez importer plusieurs feuilles de calcul d’un classeur Excel, vous devez généralement exécuter l’un de ces outils une fois pour chaque feuille.

La description complète des outils et services complexes, par exemple SSIS ou Azure Data Factory, n’entre pas dans le cadre de cette liste. Pour plus d’informations sur la solution qui vous intéresse, suivez les liens fournis.

> [!IMPORTANT]
> Pour obtenir des informations détaillées sur la connexion à des fichiers Excel, et sur les limitations et les problèmes connus liés au chargement de données depuis ou vers des fichiers Excel, consultez [Charger des données depuis ou vers Excel avec SQL Server Integration Services (SSIS)](../../integration-services/load-data-to-from-excel-with-ssis.md).

Si vous n’avez pas installé SQL Server, ou que vous avez installé SQL Server mais pas SQL Server Management Studio, consultez [Télécharger SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="sql-server-import-and-export-wizard"></a><a name="wiz"></a> Assistant Importation et Exportation SQL Server

Importez des données directement de fichiers Excel en effectuant les étapes des pages de l’Assistant Importation et Exportation SQL Server. Vous pouvez éventuellement enregistrer les paramètres sous forme de package SQL Server Integration Services (SSIS) pour pouvoir le personnaliser et le réutiliser plus tard.

1. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à une instance du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)].

2. Développez **Bases de données**.
3. Cliquez avec le bouton droit sur le nom d’une base de données.
4. Pointez sur **Tâches**.
5. Cliquez sur l’une des options suivantes :

  - **Importer des données**
  - **Exporter les données**

    ![Démarrer l’Assistant SSMS](../../integration-services/import-export-data/media/start-wizard-ssms.jpg)

![Établir une connexion à une source de données Excel](media/excel-connection.png)

Vous trouverez un exemple d’utilisation de l’Assistant pour importer d’Excel vers SQL Server sur la page [Bien démarrer avec cet exemple simple de l’Assistant Importation et exportation](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md).

Pour en savoir plus sur les autres méthodes de lancer l’Assistant Importation et exportation, consultez [Démarrer l’Assistant Importation et exportation de SQL Server](../../integration-services/import-export-data/start-the-sql-server-import-and-export-wizard.md).

## <a name="sql-server-integration-services-ssis"></a><a name="ssis"></a> SQL Server Integration Services (SSIS)

Si vous connaissez SSIS et que vous ne souhaitez pas exécuter l’Assistant Importation et Exportation SQL Server, créez un package SSIS qui utilise la source Excel et la destination SQL Server dans le flux de données.

Pour plus d’informations sur ces composants SSIS, consultez les rubriques suivantes :

- [Source Excel](../../integration-services/data-flow/excel-source.md)
- [Destination SQL Server](../../integration-services/data-flow/sql-server-destination.md)

Pour apprendre à créer des packages SSIS, consultez le didacticiel [Guide pratique pour créer un Package ETL](../../integration-services/ssis-how-to-create-an-etl-package.md).

![Composants du flux de données](media/excel-to-sql-data-flow.png)

## <a name="openrowset-and-linked-servers"></a><a name="openrowset"></a> OPENROWSET et serveurs liés

> [!IMPORTANT]
> Dans Azure SQL Database, vous ne pouvez pas importer directement à partir d’Excel. Vous devez d’abord exporter les données vers un fichier texte (CSV). Pour obtenir des exemples, consultez [Exemple](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

> [!NOTE]
> Le fournisseur ACE (anciennement fournisseur Jet) qui se connecte à des sources de données Excel est destiné à une utilisation interactive côté client. Si vous utilisez le fournisseur ACE sur SQL Server, en particulier dans des processus automatisés ou qui s’exécutent en parallèle, vous constaterez peut-être des résultats inattendus.

### <a name="distributed-queries"></a>Requêtes distribuées

Importez des données directement dans SQL Server à partir de fichiers Excel à l’aide de la fonction Transact-SQL `OPENROWSET` ou `OPENDATASOURCE`. Cette utilisation est appelée *requête distribuée*.

> [!IMPORTANT]
> Dans Azure SQL Database, vous ne pouvez pas importer directement à partir d’Excel. Vous devez d’abord exporter les données vers un fichier texte (CSV). Pour obtenir des exemples, consultez [Exemple](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

Pour pouvoir exécuter une requête distribuée, vous devez activer l’option de configuration du serveur `ad hoc distributed queries`, comme l’indique l’exemple suivant. Pour plus d’informations, consultez la page [Option de configuration du serveur : requêtes distribuées ad hoc](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md).

```sql
sp_configure 'show advanced options', 1;
RECONFIGURE;
GO
sp_configure 'Ad Hoc Distributed Queries', 1;
RECONFIGURE;
GO
```

L’exemple de code suivant utilise `OPENROWSET` pour importer les données de la feuille de calcul Excel `Sheet1` dans une nouvelle table de base de données.

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_dq
FROM OPENROWSET('Microsoft.ACE.OLEDB.12.0',
    'Excel 12.0; Database=C:\Temp\Data.xlsx', [Sheet1$]);
GO
```

Voici le même exemple avec `OPENDATASOURCE`.

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_dq
FROM OPENDATASOURCE('Microsoft.ACE.OLEDB.12.0',
    'Data Source=C:\Temp\Data.xlsx;Extended Properties=Excel 12.0')...[Sheet1$];
GO
```

Pour *ajouter* les données importées à une table *existante* au lieu d’en créer une nouvelle, utilisez la syntaxe `INSERT INTO ... SELECT ... FROM ...` à la place de la syntaxe `SELECT ... INTO ... FROM ...` utilisée dans les exemples précédents.

Pour interroger les données Excel sans les importer, utilisez simplement la syntaxe `SELECT ... FROM ...` standard.

Pour plus d’informations sur les requêtes distribuées, consultez les rubriques suivantes :

- [Requêtes distribuées](/previous-versions/sql/sql-server-2008-r2/ms188721(v=sql.105)) (Les requêtes distribuées sont toujours prises en charge dans SQL Server 2016, mais la documentation relative à cette fonctionnalité n’a pas été mise à jour.)
- [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)
- [OPENDATASOURCE](../../t-sql/functions/openquery-transact-sql.md)

### <a name="linked-servers"></a>Serveurs liés

Vous pouvez également configurer une connexion permanente de SQL Server au fichier Excel sous forme de *serveur lié*. L’exemple suivant importe les données de la feuille de calcul `Data` sur le serveur lié Excel `EXCELLINK` dans une nouvelle table de base de données SQL Server nommée `Data_ls`.

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_ls FROM EXCELLINK...[Data$];
GO
```

Vous pouvez créer un serveur lié à partir de SQL Server Management Studio, ou en exécutant la procédure stockée de système `sp_addlinkedserver`, comme l’illustre l’exemple suivant.

```sql
DECLARE @RC int

DECLARE @server     nvarchar(128)
DECLARE @srvproduct nvarchar(128)
DECLARE @provider   nvarchar(128)
DECLARE @datasrc    nvarchar(4000)
DECLARE @location   nvarchar(4000)
DECLARE @provstr    nvarchar(4000)
DECLARE @catalog    nvarchar(128)

-- Set parameter values
SET @server =     'EXCELLINK'
SET @srvproduct = 'Excel'
SET @provider =   'Microsoft.ACE.OLEDB.12.0'
SET @datasrc =    'C:\Temp\Data.xlsx'
SET @provstr =    'Excel 12.0'

EXEC @RC = [master].[dbo].[sp_addlinkedserver] @server, @srvproduct, @provider,
@datasrc, @location, @provstr, @catalog
```

Pour plus d’informations sur les serveurs liés, consultez les rubriques suivantes :

- [Créer des serveurs liés](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md)
- [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md)

Pour plus d’exemples et d’informations sur les serveurs liés et les requêtes distribuées, consultez les rubriques suivantes :

- [Guide pratique pour utiliser Excel avec des serveurs liés et des requêtes distribuées SQL Server](https://support.microsoft.com/help/306397/how-to-use-excel-with-sql-server-linked-servers-and-distributed-queries)
- [Comment importer des données d'Excel vers SQL Server](https://support.microsoft.com/help/321686/how-to-import-data-from-excel-to-sql-server)

## <a name="prerequisite---save-excel-data-as-text"></a><a name="prereq"></a> Prérequis - Enregistrer les données Excel sous forme de texte

Pour utiliser les autres méthodes décrites sur cette page (l’instruction BULK INSERT, l’outil BCP ou Azure Data Factory), vous devez d’abord exporter vos données Excel dans un fichier texte.

Dans Excel, sélectionnez **Fichier | Enregistrer sous**, puis **Texte (délimité par des tabulations) (\*.txt)** ou **CSV (séparé par des virgules) (\*.csv)** comme type de fichier de destination.

Si vous voulez exporter plusieurs feuilles de calcul du classeur, sélectionnez chaque feuille et répétez cette procédure. La commande **Enregistrer en tant que** exporte uniquement la feuille active.

> [!TIP]
> Pour obtenir de meilleurs résultats avec les outils d’importation de données, enregistrez les feuilles qui contiennent uniquement les en-têtes de colonnes et les lignes de données. Si les données enregistrées contiennent des titres des pages, des lignes vides, des notes et ainsi de suite, il se peut que vous constatiez des résultats inattendus par la suite, lorsque vous importerez des données.

## <a name="the-import-flat-file-wizard"></a><a name="import-wiz"></a> Assistant Importation d’un fichier plat

Importez des données enregistrées en tant que fichiers texte en parcourant les pages de l’Assistant Importation de fichier plat.

Comme nous l’avons expliqué dans la section [Prérequis](#prereq), il est nécessaire d’exporter les données Excel sous forme de texte pour pouvoir les importer avec l’Assistant Importation de fichier plat.

Pour plus d’informations sur l’Assistant Importation de fichier plat, voir [Assistant Importation de fichier plat vers SQL](import-flat-file-wizard.md).

## <a name="bulk-insert-command"></a><a name="bulk-insert"></a> Commande BULK INSERT

`BULK INSERT` est une commande Transact-SQL exécutable à partir de SQL Server Management Studio. L’exemple suivant charge les données du fichier délimité par des virgules `Data.csv` dans une table de base de données existante.

Comme décrit précédemment dans la section [Prérequis](#prereq), vous devez exporter vos données Excel sous forme de texte pour pouvoir utiliser BULK INSERT pour les importer. BULK INSERT ne peut pas lire les fichiers Excel directement. À l’aide de la commande BULK INSERT, vous pouvez importer un fichier CSV stocké localement ou dans le stockage Blob Azure.

```sql
USE ImportFromExcel;
GO
BULK INSERT Data_bi FROM 'C:\Temp\data.csv'
   WITH (
      FIELDTERMINATOR = ',',
      ROWTERMINATOR = '\n'
);
GO
```

Pour obtenir plus d’informations et des exemples pour SQL Server et SQL Database, consultez les rubriques suivantes :

- [Importer des données en bloc à l’aide de BULK INSERT ou OPENROWSET(BULK...)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)
- [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md)

## <a name="bcp-tool"></a><a name="bcp"></a> Outil BCP

BCP est un programme que vous exécutez à partir de l’invite de commandes. L’exemple suivant charge les données du fichier délimité par des virgules `Data.csv` dans la table de base de données existante `Data_bcp`.

Comme décrit précédemment dans la section [Prérequis](#prereq), vous devez exporter vos données Excel sous forme de texte pour pouvoir utiliser BCP pour les importer. BCP ne peut pas lire les fichiers Excel directement. Utilisez pour importer dans SQL Server ou SQL Database à partir d’un fichier texte (CSV) enregistré dans le stockage local.

> [!IMPORTANT]
> Pour un fichier texte (CSV) stocké dans le stockage Blob Azure, utilisez BULK INSERT ou OPENROWSET. Pour obtenir un exemple, consultez [Exemple](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

```console
bcp.exe ImportFromExcel..Data_bcp in "C:\Temp\data.csv" -T -c -t ,
```

Pour plus d’informations sur BCP, consultez les rubriques suivantes :

- [Importer et exporter des données en bloc à l’aide de l’utilitaire bcp](../../relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md)
- [Utilitaire bcp](../../tools/bcp-utility.md)
- [Préparer des données en vue d’une exportation ou d’une importation en bloc](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)

## <a name="copy-wizard-azure-data-factory"></a><a name="adf-wiz"></a> Assistant Copie (Azure Data Factory)

Importez des données enregistrées en tant que fichiers texte en parcourant les pages de l’Assistant Copie d’Azure Data Factory.

Comme décrit précédemment dans la section [Prérequis](#prereq), vous devez exporter vos données Excel sous forme de texte pour pouvoir utiliser Azure Data Factory pour les importer. Data Factory ne peut pas lire les fichiers Excel directement.

Pour plus d’informations sur l’Assistant Copie, consultez les rubriques suivantes :

- [Assistant Data Factory Copy](/azure/data-factory/data-factory-azure-copy-wizard)
- [Tutoriel : Créer un pipeline avec activité de copie à l’aide de l’Assistant Copie de Data Factory](/azure/data-factory/data-factory-copy-data-wizard-tutorial)

## <a name="azure-data-factory"></a><a name="adf"></a> Azure Data Factory

Si vous connaissez Azure Data Factory et que vous ne voulez pas exécuter l’Assistant Copie, créez un pipeline avec une activité de copie qui permet d’effectuer une copie à partir du fichier texte dans SQL Server ou Azure SQL Database.

Comme décrit précédemment dans la section [Prérequis](#prereq), vous devez exporter vos données Excel sous forme de texte pour pouvoir utiliser Azure Data Factory pour les importer. Data Factory ne peut pas lire les fichiers Excel directement.

Pour plus d’informations sur l’utilisation de ces sources et récepteurs Data Factory, consultez les rubriques suivantes :

- [Système de fichiers](/azure/data-factory/data-factory-onprem-file-system-connector)
- [SQL Server](/azure/data-factory/data-factory-sqlserver-connector)
- [Azure SQL Database](/azure/data-factory/data-factory-azure-sql-connector)

Pour apprendre à copier des données avec Azure Data Factory, consultez les rubriques suivantes :

- [Déplacer des données à l’aide de l’activité de copie](/azure/data-factory/data-factory-data-movement-activities)
- [Tutoriel : Créer un pipeline avec activité de copie à l’aide du Portail Azure](/azure/data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database)

## <a name="common-errors"></a>Erreurs courantes

### <a name="microsoftaceoledb120-has-not-been-registered"></a>« Microsoft.ACE.OLEDB.12.0 » n’a pas été inscrit

Cette erreur se produit car le fournisseur OLE DB n’est pas installé. Installez-le à partir de [Microsoft Access Database Engine 2010 Redistributable](https://www.microsoft.com/download/details.aspx?id=13255). Veillez à installer la version 64 bits si Windows et SQL Server sont tous deux 64 bits.

L’erreur complète est la suivante :

```
Msg 7403, Level 16, State 1, Line 3
The OLE DB provider "Microsoft.ACE.OLEDB.12.0" has not been registered.
```

### <a name="cannot-create-an-instance-of-ole-db-provider-microsoftaceoledb120-for-linked-server-null"></a>Impossible de créer une instance du fournisseur OLE DB « Microsoft.ACE.OLEDB.12.0 » du serveur lié « (null) »

Cela indique que Microsoft OLEDB n’a pas été configuré correctement. Exécutez le code Transact-SQL suivant pour résoudre ce problème :

```sql
EXEC sp_MSset_oledb_prop N'Microsoft.ACE.OLEDB.12.0', N'AllowInProcess', 1
EXEC sp_MSset_oledb_prop N'Microsoft.ACE.OLEDB.12.0', N'DynamicParameters', 1
```

L’erreur complète est la suivante :

```
Msg 7302, Level 16, State 1, Line 3
Cannot create an instance of OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)".
```

### <a name="the-32-bit-ole-db-provider-microsoftaceoledb120-cannot-be-loaded-in-process-on-a-64-bit-sql-server"></a>Impossible de charger le fournisseur OLE DB 32 bits « Microsoft.ACE.OLEDB.12.0 » in-process sur un serveur SQL Server 64 bits

Cela se produit quand une version 32 bits du fournisseur OLE DB est installée avec un serveur SQL Server 64 bits. Pour résoudre ce problème, désinstallez la version 32 bits et installez la version 64 bits du fournisseur OLE DB à la place.

L’erreur complète est la suivante :

```
Msg 7438, Level 16, State 1, Line 3
The 32-bit OLE DB provider "Microsoft.ACE.OLEDB.12.0" cannot be loaded in-process on a 64-bit SQL Server.
```

### <a name="the-ole-db-provider-microsoftaceoledb120-for-linked-server-null-reported-an-error-the-provider-did-not-give-any-information-about-the-error"></a>Le fournisseur OLE DB « Microsoft.ACE.OLEDB.12.0 » du serveur lié « (null) » a signalé une erreur. Le fournisseur n’a donné aucune information quant à cette erreur

### <a name="cannot-initialize-the-data-source-object-of-ole-db-provider-microsoftaceoledb120-for-linked-server-null"></a>Impossible d’initialiser l’objet de la source de données du fournisseur OLE DB « Microsoft.ACE.OLEDB.12.0 » du serveur lié « (null) »

Ces deux erreurs indiquent généralement un problème d’autorisations entre le processus SQL Server et le fichier. Vérifiez que le compte qui exécute le service SQL Server dispose de droits d’accès complet au fichier. Nous vous déconseillons d’essayer d’importer des fichiers à partir du bureau.

Les erreurs complètes sont :

```
Msg 7399, Level 16, State 1, Line 3
The OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)" reported an error. The provider did not give any information about the error.
```

```
Msg 7303, Level 16, State 1, Line 3
Cannot initialize the data source object of OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)".
```

## <a name="see-also"></a> Voir aussi

[Importer des données à partir d’Excel ou exporter des données vers Excel avec SQL Server Integration Services (SSIS)](../../integration-services/load-data-to-from-excel-with-ssis.md)