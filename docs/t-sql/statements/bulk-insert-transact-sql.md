---
title: BULK INSERT (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour l’instruction BULK INSERT.
ms.date: 02/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BULK_TSQL
- BULK_INSERT
- BULK_INSERT_TSQL
- BULK INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- tables [SQL Server], importing data into
- inserting files
- views [SQL Server], importing data into
- BULK INSERT statement
- views [SQL Server], exporting data from
- importing data, bulk import
- bulk importing [SQL Server], BULK INSERT statement
- file importing [SQL Server]
ms.assetid: be3984e1-5ab3-4226-a539-a9f58e1e01e2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9e0d729d951de184aa16717e784f14ae244639d8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098488"
---
# <a name="bulk-insert-transact-sql"></a>BULK INSERT (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Importe un fichier de données dans une table ou vue de base de données dans un format spécifié par l'utilisateur dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
BULK INSERT
   { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }
      FROM 'data_file'
     [ WITH
    (
   [ [ , ] BATCHSIZE = batch_size ]
   [ [ , ] CHECK_CONSTRAINTS ]
   [ [ , ] CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]
   [ [ , ] DATAFILETYPE =
      { 'char' | 'native'| 'widechar' | 'widenative' } ]
   [ [ , ] DATA_SOURCE = 'data_source_name' ]
   [ [ , ] ERRORFILE = 'file_name' ]
   [ [ , ] ERRORFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] FIRSTROW = first_row ]
   [ [ , ] FIRE_TRIGGERS ]
   [ [ , ] FORMATFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] KEEPIDENTITY ]
   [ [ , ] KEEPNULLS ]
   [ [ , ] KILOBYTES_PER_BATCH = kilobytes_per_batch ]
   [ [ , ] LASTROW = last_row ]
   [ [ , ] MAXERRORS = max_errors ]
   [ [ , ] ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) ]
   [ [ , ] ROWS_PER_BATCH = rows_per_batch ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
   [ [ , ] TABLOCK ]

   -- input file format options
   [ [ , ] FORMAT = 'CSV' ]
   [ [ , ] FIELDQUOTE = 'quote_characters']
   [ [ , ] FORMATFILE = 'format_file_path' ]
   [ [ , ] FIELDTERMINATOR = 'field_terminator' ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
    )]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*database_name* : Nom de la base de données qui contient la table ou la vue spécifiée. S'il n'est pas spécifié, la base de données actuelle est utilisée.

*schema_name* : Nom du schéma de la table ou de la vue. *schema_name* est facultatif si le schéma par défaut de l’utilisateur réalisant l’opération d’importation en bloc est le schéma de la table ou de la vue spécifiée. Si *schema* n’est pas spécifié et que le schéma par défaut de l’utilisateur réalisant l’opération d’importation en bloc diffère de celui de la table ou de la vue spécifiée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne un message d’erreur et l’opération d’importation en bloc est annulée.

*table_name* : Nom de la table ou de la vue dans laquelle les données doivent être importées en bloc. Seules des vues dans lesquelles toutes les colonnes font référence à la même table de base peuvent être utilisées. Pour plus d’informations sur les restrictions relatives au chargement des données dans les vues, consultez [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).

**’** _data_file_ **’**  : Chemin complet du fichier de données contenant les données à importer dans la table ou la vue spécifiée. BULK INSERT peut importer des données à partir d’un disque ou de Stockage Blob Azure (notamment réseau, disquette, disque dur, etc.).

*data_file* doit spécifier un chemin valide à partir du serveur où [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute. Si *data_file* correspond à un fichier distant, spécifiez le nom UNC (Universal Naming Convention). Un nom UNC se présente sous la forme \\\\*nom_système*\\*nom_partage*\\*chemin*\\*nom_fichier*. Par exemple :

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.dat';
```

**S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 et Azure SQL Database.
À partir de [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP1.1, le data_file peut être dans le Stockage Blob Azure. Dans ce cas, vous devez spécifier l’option **data_source_name**. Pour obtenir un exemple, consultez [Importation de données à partir d’un fichier dans Stockage Blob Azure](#f-importing-data-from-a-file-in-azure-blob-storage).

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du stockage Blob Azure.

**'** _data_source_name_ **'** 
**S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 et Azure SQL Database.
Source de données externe nommée pointant vers l’emplacement de Stockage Blob Azure du fichier qui sera importé. La source de données externe doit être créée à l’aide de l’option `TYPE = BLOB_STORAGE` ajoutée dans [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1. Pour plus d’informations, consultez [CRÉER UNE SOURCE DE DONNÉES EXTERNES](../../t-sql/statements/create-external-data-source-transact-sql.md). Pour obtenir un exemple, consultez [Importation de données à partir d’un fichier dans Stockage Blob Azure](#f-importing-data-from-a-file-in-azure-blob-storage).

BATCHSIZE **=** _batch_size_ : Spécifie le nombre de lignes contenues dans un lot. Chaque lot est copié sur le serveur comme une transaction unique. En cas d'échec, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide ou annule la transaction pour chaque lot. Par défaut, toutes les données du fichier spécifié constituent un seul lot. Pour plus d'informations sur les performances, consultez la section « Notes » plus loin dans cette rubrique.

CHECK_CONSTRAINTS : Spécifie que toutes les contraintes sur la table ou la vue cible doivent être vérifiées lors l’opération d’importation en bloc. Sans l’option CHECK_CONSTRAINTS, toute contrainte CHECK et FOREIGN KEY est ignorée. Après l’opération, la contrainte sur la table est marquée comme non approuvée.

> [!NOTE]
> Les contraintes UNIQUE et PRIMARY KEY sont toujours appliquées. Lors de l'importation dans une colonne de type character définie avec une contrainte NOT NULL, BULK INSERT insère une chaîne vide si aucune valeur n'est spécifiée dans le fichier texte.

À un certain moment, il devient nécessaire d'examiner les contraintes définies sur l'ensemble de la table. Si la table n’était pas vide avant l’opération d’importation en bloc, le coût de la revalidation de la contrainte peut dépasser celui de l’application des contraintes CHECK aux données incrémentielles.

Il peut notamment convenir de désactiver les contraintes (comportement par défaut) si les données d'entrée contiennent des lignes qui violent des contraintes. Une fois les contraintes CHECK désactivées, vous pouvez importer les données, puis utiliser les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] pour supprimer les données non valides.

> [!NOTE]
> L'option MAXERRORS ne s'applique pas à la vérification des contraintes.

CODEPAGE **=** { **’** ACP **’** \| **’** OEM **’** \| **’** RAW **’** \| **’** _page_codes_ **’** } Spécifie la page de codes des données dans le fichier de données. CODEPAGE n’est justifié que si les données contiennent des colonnes de type **char**, **varchar** ou **text** dont les valeurs de caractères sont supérieures à **127** ou inférieures à **32**. Pour obtenir un exemple, consultez [Spécification d’une page de codes](#d-specifying-a-code-page).

> [!IMPORTANT]
> CODEPAGE n’est pas une option prise en charge sur Linux pour [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]. Avec [!INCLUDE[ssSQLv15_md](../../includes/sssqlv15-md.md)], seule l’option **« RAW »** est autorisée pour CODEPAGE.

> [!NOTE]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] vous recommande de spécifier un nom de classement pour chaque colonne dans un [fichier de format](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).

|Valeur CODEPAGE|Description|
|--------------------|-----------------|
|ACP|Les colonnes de type de données **char**, **varchar** ou **text** sont converties de la page de codes [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]/[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows (ISO 1252) à la page de codes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|OEM (valeur par défaut)|Les colonnes de type de données **char**, **varchar** ou **text** sont converties de la page de codes du système OEM à la page de codes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|RAW|Aucune conversion d'une page de codes vers une autre ne se produit ; ceci est l'option la plus rapide.|
|*code_page*|Numéro de la page de codes, par exemple 850.<br /><br /> **&#42;&#42; Important &#42;&#42;** Les versions antérieures à la version [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] ne prennent pas en charge la page de codes 65001 (encodage UTF-8).|
| &nbsp; | &nbsp; |

DATAFILETYPE **=** { **’char’** \| **’native’** \| **’widechar’** \| **’widenative’** } Spécifie que BULK INSERT réalise l’opération d’importation en utilisant la valeur définie pour le type de fichier de données.

&nbsp;

|Valeur DATAFILETYPE|Toutes les données représentées dans :|
|------------------------|------------------------------|
|**char** (par défaut)|Format caractères.<br /><br /> Pour plus d’informations, consultez [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).|
|**native**|Types de données (base de données) natif. Créer le fichier de données natif en important en bloc les données à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de l’utilitaire **bcp**.<br /><br /> La valeur native offre de meilleures performances que la valeur char. Il est recommandé d’utiliser le format natif quand vous transférez des données en bloc entre plusieurs instances de SQL Server au moyen d’un fichier de données qui ne contient pas de caractères d’un jeu de caractères étendus/codés sur deux octets (DBCS).<br /><br /> Pour plus d’informations, consultez [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).|
|**widechar**|Caractères Unicode.<br /><br /> Pour plus d’informations, consultez [Utiliser le format caractère Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).|
|**widenative**|Types de données (base de données) natif, à l’exception des colonnes de type **char**, **varchar** et **text** dans lesquelles les données sont stockées au format Unicode. Créez le fichier de données **widenative** en important en bloc les données à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de l’utilitaire **bcp**.<br /><br /> La valeur **widenative** offre de meilleures performances que la valeur **widechar**. Si le fichier de données contient des caractères étendus [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)], sélectionnez **widenative**.<br /><br /> Pour plus d’informations, consultez [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).|
| &nbsp; | &nbsp; |

ERRORFILE **='** _file_name_ **'**  : Spécifie le fichier utilisé pour collecter les lignes comportant des erreurs de mise en forme et impossibles à convertir en un ensemble de lignes OLE DB. Ces lignes sont copiées « en l'état » du fichier de données vers ce fichier d'erreur.

Le fichier d'erreur est créé lors de l'exécution de la commande. Une erreur se produit si le fichier existe déjà. De plus, un fichier de contrôle portant l'extension .ERROR.txt est créé. Il fait référence à chacune des lignes du fichier d'erreur et propose un diagnostic. Dès que les erreurs ont été corrigées, les données peuvent être chargées.
**S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
À partir de [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], `error_file_path` peut se trouver dans Stockage Blob Azure.

'errorfile_data_source_name' **S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Source de données externe nommée pointant vers l’emplacement de Stockage Blob Azure du fichier d’erreur contenant les erreurs détectées lors de l’importation. La source de données externe doit être créée à l’aide de l’option `TYPE = BLOB_STORAGE` ajoutée dans [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1. Pour plus d’informations, consultez [CRÉER UNE SOURCE DE DONNÉES EXTERNES](../../t-sql/statements/create-external-data-source-transact-sql.md).

FIRSTROW **=** _first_row_ : Spécifie le numéro de la première ligne à charger. La valeur par défaut est la première ligne du fichier de données spécifié. FIRSTROW commence à 1.

> [!NOTE]
> L'attribut FIRSTROW n'est pas destiné à ignorer les en-têtes de colonnes. Le fait d'ignorer des en-têtes n'est pas pris en charge par l'instruction BULK INSERT. Si des lignes sont ignorées, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] examine uniquement les marques de fin de champ et ne valide pas les données figurant dans les champs des lignes ignorées.

FIRE_TRIGGERS : Spécifie que tous les déclencheurs d’insertion définis sur la table de destination s’exécutent au cours de l’opération d’importation en bloc. Si des déclencheurs sont définis pour les opérations INSERT réalisées sur la table cible, ils sont activés à la fin de chaque lot.

Si FIRE_TRIGGERS n’est pas spécifié, aucun déclencheur d’insertion ne s’exécute.

FORMATFILE_DATA_SOURCE **=** 'data_source_name' **S’applique à :**  [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 1.1.
Source de données externe nommée pointant vers l’emplacement de Stockage Blob Azure du fichier de format qui définira le schéma des données importées. La source de données externe doit être créée à l’aide de l’option `TYPE = BLOB_STORAGE` ajoutée dans [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1. Pour plus d’informations, consultez [CRÉER UNE SOURCE DE DONNÉES EXTERNES](../../t-sql/statements/create-external-data-source-transact-sql.md).

KEEPIDENTITY : Spécifie que la ou les valeurs d’identité figurant dans le fichier de données importé doivent être utilisées pour la colonne d’identité. Si KEEPIDENTITY n'est pas spécifié, les valeurs d'identité de cette colonne sont vérifiées mais pas importées, et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affecte automatiquement des valeurs uniques en fonction de la valeur initiale et d'un incrément spécifié lors de la création de la table. Si le fichier de données ne contient pas de valeurs pour la colonne d'identité de la table ou de la vue, utilisez un fichier de format pour indiquer que la colonne d'identité ne doit pas être prise en compte lors de l'importation des données ; dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attribue automatiquement des valeurs uniques à la colonne. Pour plus d’informations, consultez [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md).

Pour plus d’informations sur la conservation des valeurs d’identité, consultez [Conserver des valeurs d’identité lors de l’importation de données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md).

KEEPNULLS : Spécifie que lors l’importation en bloc, les colonnes vides doivent conserver la valeur NULL, au lieu que des valeurs par défaut soient insérées dans ces colonnes. Pour plus d’informations, consultez [Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l’importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).

KILOBYTES_PER_BATCH **=** _kilobytes_per_batch_ : Spécifie le nombre approximatif de kilo-octets (Ko) de données par lot sous la forme *kilobytes_per_batch*. Par défaut, KILOBYTES_PER_BATCH est inconnu. Pour plus d'informations sur les performances, consultez la section « Notes » plus loin dans cette rubrique.

LASTROW **=** _last_row_ : Spécifie le numéro de la dernière ligne à charger. La valeur par défaut est 0, c'est-à-dire la dernière ligne du fichier de données spécifié.

MAXERRORS **=** _max_errors_ : Spécifie le nombre maximal d’erreurs de syntaxe autorisées dans les données avant l’annulation de l’importation en bloc. Chaque ligne ne pouvant pas être importée par l'opération d'importation en bloc est ignorée et compte comme une erreur. Si *max_errors* n’est pas spécifié, la valeur par défaut est de 10.

> [!NOTE]
> L’option MAX_ERRORS ne s’applique pas aux vérifications de contraintes ni à la conversion des types de données **money** et **bigint**.

ORDER ( { *column* [ ASC | DESC ] } [ **,** ... *n* ] ) : Spécifie la façon dont les données du fichier de données sont triées. Les performances de l'importation en bloc sont améliorées si les données importées sont triées en fonction de l'index cluster de la table, le cas échéant. Si le fichier de données est trié dans un autre ordre (c'est-à-dire pas dans l'ordre d'une clé d'index cluster) ou s'il n'y a pas d'index cluster dans la table, la clause ORDER est ignorée. Les noms de colonnes fournis doivent être des noms de colonnes valides dans la table de destination. Par défaut, l'opération d'insertion en bloc considère que le fichier de données n'est pas ordonné. Pour une importation en bloc optimisée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide également le fait que les données importées sont triées.

*n* : Espace réservé qui indique que plusieurs colonnes peuvent être spécifiées.

ROWS_PER_BATCH **=** _rows_per_batch_ : Indique le nombre approximatif de lignes de données que compte le fichier de données.

Par défaut, toutes les données du fichier de données sont envoyées au serveur en une seule transaction, et le nombre de lignes du lot est inconnu de l'optimiseur de requête. Si vous spécifiez ROWS_PER_BATCH (valeur > 0), le serveur utilise cette valeur pour optimiser l’importation en bloc. La valeur spécifiée pour ROWS_PER_BATCH devrait correspondre à peu près au nombre réel de lignes. Pour plus d'informations sur les performances, consultez la section « Notes » plus loin dans cette rubrique.

TABLOCK : Spécifie qu’un verrou de niveau table est acquis pour la durée de l’opération d’importation en bloc. Une table peut être chargée simultanément par plusieurs clients à condition qu'elle ne comporte pas d'index et que TABLOCK soit spécifié. Par défaut, le comportement du verrouillage est déterminé par l'option **table lock on bulk load**. En verrouillant la table pendant la durée de l'importation en bloc, vous réduisez la contention de verrouillage et augmentez considérablement, dans certains cas, les performances. Pour plus d'informations sur les performances, consultez la section « Notes » plus loin dans cette rubrique.

For columnstore index. le comportement de verrouillage est différent, car il est divisé en interne en plusieurs ensembles de lignes. Chaque thread charge des données exclusivement dans chaque ensemble de lignes en prenant un verrou X sur l’ensemble de lignes, ce qui autorise le chargement de données en parallèle avec des sessions de chargement de données simultanées. L’utilisation de l’option TABLOCK fait en sorte que le thread prenne un verrou X sur la table (contrairement aux verrous BU pour des ensembles de lignes traditionnels), ce qui empêche les autres threads simultanés de charger des données en même temps.

### <a name="input-file-format-options"></a>Options de format de fichier d’entrée

FORMAT **=** 'CSV' **S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Spécifie un fichier de valeurs séparées par des virgules conforme à la norme [RFC 4180](https://tools.ietf.org/html/rfc4180).

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.csv'
WITH ( FORMAT='CSV');
```

FIELDQUOTE **=** 'field_quote' **S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1.
Spécifie un caractère qui sera utilisé comme caractère de guillemet dans le fichier CSV. Si vous ne spécifiez pas cet argument, le caractère guillemet (") servira de caractère guillemet tel que défini dans la norme [RFC 4180](https://tools.ietf.org/html/rfc4180).

FORMATFILE **=** '_format_file_path_' : Spécifie le chemin complet d’un fichier de format. Un fichier de format décrit le fichier de données qui contient les réponses stockées créées à l’aide de l’utilitaire **bcp** dans la même table ou vue. Le fichier de format doit être utilisé dans les contextes suivants :

- Le fichier de données contient plus ou moins de colonnes que la table ou la vue.
- Les colonnes sont ordonnées différemment.
- Les délimiteurs de colonne sont différents.
- Le format des données présente d'autres changements. Les fichiers de format sont généralement créés au moyen de l’utilitaire **bcp**, puis modifiés, au besoin, à l’aide d’un éditeur de texte. Pour plus d’informations, consultez [Utilitaire bcp](../../tools/bcp-utility.md) et [Créer un fichier de format](../../relational-databases/import-export/create-a-format-file-sql-server.md).

**S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1 et Azure SQL Database.
À partir de [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1, format_file_path peut être dans Stockage Blob Azure.

FIELDTERMINATOR **='** _field_terminator_ **'**  : Spécifie la marque de fin de champ à utiliser pour les fichiers de données de type **char** et **widechar**. La marque de fin de champ par défaut est le caractère de tabulation (\t). Pour plus d’informations, consultez [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

ROWTERMINATOR **='** _row_terminator_ **'**  : Spécifie la marque de fin de ligne à utiliser pour les fichiers de données de type **char** et **widechar**. Par défaut, il s’agit de **\r\n** (caractère de nouvelle ligne). Pour plus d’informations, consultez [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

## <a name="compatibility"></a>Compatibilité

BULK INSERT impose une validation et un contrôle stricts des données lues dans un fichier qui pourraient faire échouer les scripts existants si ces derniers s'exécutent sur des données non valides. Par exemple, BULK INSERT vérifie que :

- Les représentations en mode natif des types de données **float** ou **real** sont valides.
- les données Unicode comportent un nombre d'octets pair.

## <a name="data-types"></a>Types de données

### <a name="string-to-decimal-data-type-conversions"></a>Conversion du type de données String en type Decimal

Les conversions de type de données String (chaîne) en Decimal (décimal) utilisées dans BULK INSERT suivent les mêmes règles que la fonction [!INCLUDE[tsql](../../includes/tsql-md.md)] [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md), qui rejette les chaînes représentant des valeurs numériques qui utilisent la notation scientifique. BULK INSERT traite donc ce genre de chaînes comme des valeurs non valides et signale des erreurs de conversion.

Pour contourner ce problème, utilisez un fichier de format permettant d’importer en bloc des données de type **float** à notation scientifique dans une colonne décimale. Dans le fichier de format, décrivez explicitement la colonne comme étant de type **real** ou **float**. Pour plus d’informations sur ces types de données, consultez [float et real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md).

> [!NOTE]
> Les fichiers de format représentent des données **real** en tant que type de données **SQLFLT4**, et des données **float** en tant que type de données **SQLFLT8**. Pour plus d’informations sur les fichiers de format non-XML, consultez [Spécifier le type de stockage de fichiers à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md).

#### <a name="example-of-importing-a-numeric-value-that-uses-scientific-notation"></a>Exemple d'importation d'une valeur numérique qui utilise la notation scientifique

Cet exemple utilise la table suivante :

```sql
CREATE TABLE t_float(c1 FLOAT, c2 DECIMAL (5,4));
```

 L'utilisateur veut importer des données en bloc dans la table `t_float`. Le fichier de données, C:\t_float-c.dat, contient des données **float** à notation scientifique, par exemple :

```input
8.0000000000000002E-28.0000000000000002E-2
```

Cependant, l'instruction BULK INSERT ne peut pas importer ces données directement dans `t_float`, car sa deuxième colonne, `c2`, utilise le type de données `decimal`. Un fichier de format est donc nécessaire, Il doit mapper les données **float** à notation scientifique au format décimal de la colonne `c2`.

Le fichier de format suivant utilise le type de données `SQLFLT8` pour établir la correspondance entre le deuxième champ de données et la deuxième colonne :

```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<RECORD>
<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t" MAX_LENGTH="30"/>
<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30"/> </RECORD> <ROW>
<COLUMN SOURCE="1" NAME="c1" xsi:type="SQLFLT8"/>
<COLUMN SOURCE="2" NAME="c2" xsi:type="SQLFLT8"/> </ROW> </BCPFORMAT>
```

Si vous voulez opter pour ce fichier de format (avec le nom de fichier `C:\t_floatformat-c-xml.xml`) afin d'importer les données de test dans la table de test, exécutez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :

```sql
BULK INSERT bulktest..t_float
FROM 'C:\t_float-c.dat' WITH (FORMATFILE='C:\t_floatformat-c-xml.xml');
```

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du stockage Blob Azure.

### <a name="data-types-for-bulk-exporting-or-importing-sqlxml-documents"></a>Types de données pour l'importation et l'exportation en bloc de documents SQLXML

Pour exporter ou importer en bloc des données SQLXML, utilisez l'un des types de données ci-dessous dans votre fichier de format :

|Type de données|Résultat|
|---------------|------------|
|SQLCHAR ou SQLVARCHAR|Les données sont envoyées dans la page de codes du client ou dans la page de codes impliquée par le classement. Le résultat est le même que si vous définissiez la propriété DATAFILETYPE **='char'** sans spécifier de fichier de format.|
|SQLNCHAR ou SQLNVARCHAR|Les données sont envoyées au format Unicode. Le résultat est le même que si vous définissiez la propriété DATAFILETYPE **= 'widechar'** sans spécifier de fichier de format.|
|SQLBINARY ou SQLVARBIN|Les données sont envoyées sans être converties.|
| &nbsp; | &nbsp; |

## <a name="general-remarks"></a>Remarques d'ordre général

Pour obtenir une comparaison de l’instruction BULK INSERT, de l’instruction INSERT ... SELECT \* FROM OPENROWSET(BULK...), et de la commande **bcp**, consultez [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).

Pour plus d’informations sur la préparation des données en vue d’une importation en bloc, consultez [Préparer des données en vue d’une exportation ou d’une importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

L'instruction BULK INSERT peut être exécutée dans une transaction définie par l'utilisateur pour importer des données dans une table ou une vue. Pour utiliser des correspondances multiples pour l'importation en bloc de données, une transaction peut éventuellement spécifier la clause BATCHSIZE dans l'instruction BULK INSERT. Si une transaction sur plusieurs lots est restaurée, chaque lot que la transaction a envoyé à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est restaurée.

## <a name="interoperability"></a>Interopérabilité

### <a name="importing-data-from-a-csv-file"></a>Importation des données d'un fichier CSV

À compter de [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1, BULK INSERT prend en charge le format CSV, comme le fait Azure SQL Database.
Avant [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1.1, les fichiers CSV ne sont pas pris en charge par les opérations d’importation en bloc [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Toutefois, dans certains cas, un fichier CSV peut être utilisé comme fichier de données pour une importation en bloc de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur la configuration requise pour importer des données à partir d’un fichier de données CSV, consultez [Préparer des données en vue d’une exportation ou d’une importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

## <a name="logging-behavior"></a>Comportement de journalisation

 Pour plus d’informations sur le moment où les opérations d’insertion de ligne effectuées par l’importation en bloc dans SQL Server sont consignées dans le journal des transactions, consultez [Prérequis pour une journalisation minimale dans l’importation en bloc](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md). La journalisation minimale n’est pas prise en charge dans Azure SQL Database.

## <a name="restrictions"></a><a name="Limitations"></a> Restrictions

Lorsque vous utilisez un fichier de format avec BULK INSERT, vous pouvez spécifier jusqu'à 1 024 champs. Il s'agit du nombre maximal de colonnes autorisé dans une table. Si vous utilisez un fichier de format avec BULK INSERT et un fichier de données qui contient plus de 1024 champs, BULK INSERT génère l’erreur 4822. L’[utilitaire bcp](../../tools/bcp-utility.md) ne présentant pas cette limitation, pour les fichiers de données qui contiennent plus de 1024 champs, utilisez BULK INSERT sans fichier de format ou utilisez la commande **bcp**.

## <a name="performance-considerations"></a>Considérations relatives aux performances

Si le nombre de pages à vider dans un lot unique dépasse un seuil interne, une analyse complète du pool de mémoires tampons peut s'effectuer afin d'identifier les pages à vider lors de la validation du lot. Cette analyse complète peut nuire aux performances de l'importation en bloc. Le dépassement du seuil interne se produit lorsqu'un pool de mémoires tampons de grande taille est associé à un sous-système d'E/S lent. Pour éviter tout dépassement de mémoire tampon sur les ordinateurs de grande capacité, évitez d'utiliser l'indication TABLOCK (qui entraîne la suppression des optimisations en bloc) ou utilisez une taille de lot plus petite (qui permet de conserver les optimisations en bloc).

En raison de la diversité des ordinateurs, nous vous recommandons de tester différentes tailles de lot avec votre charge de données pour trouver la solution la plus adaptée.

Avec Azure SQL Database, vous pouvez augmenter temporairement le niveau des performances de la base de données ou de l’instance avant l’importation si vous importez un grand volume de données.

## <a name="security"></a>Sécurité

### <a name="security-account-delegation-impersonation"></a>Délégation de compte de sécurité (emprunt d'identité)

Si un utilisateur a recours à une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le profil de sécurité du compte du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est alors utilisé. Une connexion via l'authentification SQL Server ne peut pas être authentifiée en dehors du moteur de base de données. Par conséquence, lorsqu'une commande BULK INSERT est initiée par une connexion via l'authentification SQL Server, la connexion aux données s'effectue à l'aide du contexte de sécurité du compte du processus SQL Server (compte utilisé par le service de moteur de base de données SQL Server). Pour pouvoir lire les données sources, vous devez octroyer au compte utilisé par le moteur de base de données SQL Server l'accès aux données sources. Par opposition, si un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'est connecté via l'authentification Windows, il peut lire uniquement les fichiers accessibles par le compte d'utilisateur, indépendamment du profil de sécurité du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Si vous exécutez l’instruction BULK INSERT à l’aide de **sqlcmd** ou **osql** à partir d’un ordinateur, en insérant les données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un deuxième ordinateur et en spécifiant un *data_file* sur un troisième ordinateur sous forme de chemin UNC, vous risquez de recevoir une erreur 4861.

Pour résoudre cette erreur, utilisez l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et spécifiez une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisant le profil de sécurité du compte de processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ou configurez Windows de façon à activer la délégation de compte de sécurité. Pour plus d'informations sur l'activation d'un compte d'utilisateur en vue de son approbation pour la délégation, consultez l'aide de Windows.

Pour plus d’informations sur les considérations relatives à l’utilisation de BULK INSERT, consultez [Importer des données en bloc à l’aide de BULK INSERT ou OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

Lors d’une importation depuis Stockage Blob Azure et si les données ne sont pas publiques (accès anonyme), créez des [DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md) basées sur une clé SAS chiffrée avec une [MASTER KEY](create-master-key-transact-sql.md), puis créez une [source de base de données externe](../../t-sql/statements/create-external-data-source-transact-sql.md) pour l’utiliser dans votre commande BULK INSERT. Pour obtenir un exemple, consultez [Importation de données à partir d’un fichier dans Stockage Blob Azure](#f-importing-data-from-a-file-in-azure-blob-storage).

### <a name="permissions"></a>Autorisations

Requiert les autorisations INSERT et ADMINISTER BULK OPERATIONS. Dans Azure SQL Database, les opérations INSERT et ADMINISTER DATABASE BULK OPERATIONS sont nécessaires. Les autorisations ADMINISTER BULK OPERATIONS ou le rôle bulkadmin ne sont pas pris en charge pour SQL Server sur Linux. Seul `sysadmin` peut effectuer des insertions en bloc pour SQL Server sur Linux. 

De plus, l'autorisation ALTER TABLE est nécessaire si une ou plusieurs des conditions suivantes sont réunies :

- Des contraintes existent et l'option CHECK_CONSTRAINTS n'est pas spécifiée.

   > [!NOTE]
   > La désactivation des contraintes est le comportement par défaut. Pour vérifier les contraintes explicitement, utilisez l'option CHECK_CONSTRAINTS.

- Des déclencheurs existent et l'option FIRE_TRIGGER n'est pas spécifiée.

   > [!NOTE]
   > Par défaut, les déclencheurs ne sont pas activés. Pour activer les déclencheurs explicitement, utilisez l'option FIRE_TRIGGER.

- Vous utilisez l'option KEEPIDENTITY pour importer une valeur d'identité du fichier de données.

## <a name="examples"></a>Exemples

### <a name="a-using-pipes-to-import-data-from-a-file"></a>R. Importation des données d'un fichier à l'aide du caractère (|)

L'exemple suivant importe des informations détaillées de commande dans la table `AdventureWorks2012.Sales.SalesOrderDetail`, à partir du fichier de données spécifié en utilisant le caractère `|` comme marque de fin de champ et `|\n` comme marque de fin de ligne.

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
      (  
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR =' |\n'
      );
```

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du stockage Blob Azure.

### <a name="b-using-the-fire_triggers-argument"></a>B. Utilisation de l'argument FIRE_TRIGGERS

L'exemple suivant spécifie l'argument `FIRE_TRIGGERS`.

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
     (
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR = ':\n'
         , FIRE_TRIGGERS
      );
```

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du stockage Blob Azure.

### <a name="c-using-line-feed-as-a-row-terminator"></a>C. Utilisation du retour à la ligne comme délimiteur de ligne

 L'exemple suivant importe un fichier qui utilise le retour à la ligne comme marque de fin de ligne, par exemple une sortie UNIX :

```sql
DECLARE @bulk_cmd VARCHAR(1000);
SET @bulk_cmd = 'BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
FROM ''<drive>:\<path>\<filename>''
WITH (ROWTERMINATOR = '''+CHAR(10)+''')';
EXEC(@bulk_cmd);
```

> [!NOTE]
> En raison de la manière dont Microsoft Windows traite les fichiers texte **(\n**, est remplacé automatiquement par **\r\n)** .

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du stockage Blob Azure.

### <a name="d-specifying-a-code-page"></a>D. Spécification d’une page de codes

L’exemple suivant montre comment spécifier une page de codes.

```sql
BULK INSERT MyTable
FROM 'D:\data.csv'
WITH
( CODEPAGE = '65001'
   , DATAFILETYPE = 'char'
   , FIELDTERMINATOR = ','
);
```

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du stockage Blob Azure.

### <a name="e-importing-data-from-a-csv-file"></a>E. Importation de données d’un fichier CSV

L’exemple suivant montre comment spécifier un fichier CSV en ignorant l’en-tête (première ligne), à l’aide de `;` comme marque de fin de champ et `0x0a` comme marque de fin de ligne :

```sql
BULK INSERT Sales.Invoices
FROM '\\share\invoices\inv-2016-07-25.csv'
WITH (FORMAT = 'CSV'
      , FIRSTROW=2
      , FIELDQUOTE = '\'
      , FIELDTERMINATOR = ';'
      , ROWTERMINATOR = '0x0a');
```

> [!IMPORTANT]
> Azure SQL Database prend uniquement en charge la lecture à partir du stockage Blob Azure.

### <a name="f-importing-data-from-a-file-in-azure-blob-storage"></a>F. Importation de données à partir d’un fichier dans Stockage Blob Azure

L’exemple suivant montre comment charger des données à partir d’un fichier CSV dans un emplacement de stockage Blob Azure sur lequel vous avez créé une clé SAS. L’emplacement du stockage Blob Azure est configuré comme source de données externe. Ceci nécessite des informations d’identification délimitées à la base de données avec une signature d’accès partagé chiffrée à l’aide d’une clé principale dans la base de données utilisateur.

```sql
--> Optional - a MASTER KEY is not required if a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'YourStrongPassword1';
GO
--> Optional - a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE DATABASE SCOPED CREDENTIAL MyAzureBlobStorageCredential
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************';

 -- NOTE: Make sure that you don't have a leading ? in SAS token, and
 -- that you have at least read permission on the object that should be loaded srt=o&sp=r, and
 -- that expiration period is valid (all dates are in UTC time)

CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
WITH ( TYPE = BLOB_STORAGE,
          LOCATION = 'https://****************.blob.core.windows.net/invoices'
          , CREDENTIAL= MyAzureBlobStorageCredential --> CREDENTIAL is not required if a blob is configured for public (anonymous) access!
);

BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (DATA_SOURCE = 'MyAzureBlobStorage');
```

> [!IMPORTANT]
> Azure SQL prend uniquement en charge la lecture à partir du Stockage Blob Azure.

### <a name="g-importing-data-from-a-file-in-azure-blob-storage-and-specifying-an-error-file"></a>G. Importation de données à partir d’un fichier dans Stockage Blob Azure et spécification d’un fichier d’erreurs

L’exemple suivant montre comment charger des données à partir d’un fichier CSV dans un emplacement Stockage Blob Azure qui a été configuré comme source de données externe et spécifier un fichier d’erreurs. Cela nécessite des informations d’identification délimitées à la base de données avec une signature d’accès partagé. Notez qu’en cas d’exécution sur Azure SQL Database, l’option ERRORFILE doit être accompagnée d’ERRORFILE_DATA_SOURCE. Sinon, l’importation peut échouer avec une erreur d’autorisations. Le fichier spécifié dans ERRORFILE ne doit pas exister dans le conteneur.

```sql
BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (
         DATA_SOURCE = 'MyAzureInvoices'
         , FORMAT = 'CSV'
         , ERRORFILE = 'MyErrorFile'
         , ERRORFILE_DATA_SOURCE = 'MyAzureInvoices');
```

Pour obtenir des exemples `BULK INSERT` complets, illustrant notamment la configuration des informations d’identification et de la source de données externe, consultez [Exemples d’accès en bloc à des données dans Stockage Blob Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

### <a name="additional-examples"></a>Autres exemples

 Les rubriques suivantes fournissent d’autres exemples de la commande `BULK INSERT` :

- [Exemples d’importation et d’exportation en bloc de documents XML &#40;SQL Server&#41;](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [Conserver des valeurs d’identité lors de l’importation de données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)
- [Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l’importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)
- [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)
- [Utiliser un fichier de format pour importer des données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)
- [Utiliser le format natif pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)
- [Utiliser le format caractère Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)
- [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)
- [Utiliser un fichier de format pour ignorer une colonne de table &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Utiliser un fichier de format pour mapper les colonnes d’une table sur les champs d’un fichier de données &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>Voir aussi

- [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)
- [Utilitaire bcp](../../tools/bcp-utility.md)
- [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)
- [Préparer des données en vue d’une exportation ou d’une importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)
- [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)