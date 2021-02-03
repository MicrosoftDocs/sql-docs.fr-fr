---
title: COPY INTO (Transact-SQL)
titleSuffix: (Azure Synapse Analytics) - SQL Server
description: Utilisez l’instruction COPY dans Azure Synapse Analytics pour le chargement à partir de comptes de stockage externes.
ms.date: 09/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COPY_TSQL
- COPY INTO
- COPY
- LOAD
dev_langs:
- TSQL
author: kevinvngo
ms.author: kevin
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: fb52e9e869c0210e8e320c11dfec21aa8f7d585e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076807"
---
# <a name="copy-transact-sql"></a>COPY (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Cet article explique comment utiliser l’instruction COPY dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] pour le chargement à partir de comptes de stockage externes. L’instruction COPY offre une souplesse maximale pour l’ingestion de données à débit élevé dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]. Utilisez COPY pour les fonctionnalités suivantes :

- Utiliser le chargement aux utilisateurs avec privilèges plus restreints, sans avoir besoin d’autorisations CONTROL strictes sur l’entrepôt de données
- Exécuter uniquement une instruction T-SQL sans avoir à créer d’objets de base de données supplémentaires
- Analyser et charger correctement les fichiers CSV où les **séparateurs** (chaîne, champ, ligne) **sont** **placés dans une séquence d’échappement dans des colonnes délimitées par des chaînes**
- Spécifier un meilleur modèle d’autorisation sans exposer les clés de compte de stockage à l’aide de signatures d’accès partagé (SAS)
- Utiliser un autre compte de stockage pour l’emplacement ERRORFILE (REJECTED_ROW_LOCATION)
- Personnaliser les valeurs par défaut pour chaque colonne cible et spécifier les champs de données sources à charger dans des colonnes cibles spécifiques
- Spécifier une marque de fin de ligne personnalisée pour les fichiers CSV
- Tirer parti des formats de date SQL Server pour les fichiers CSV
- Spécifier des caractères génériques et plusieurs fichiers dans le chemin de l’emplacement du stockage

Pour obtenir des exemples complets et des Démarrages rapides à l’aide de l’instruction COPY, consultez la documentation suivante :

- [Démarrage rapide : Charger en masse des données à l’aide de l’instruction COPY](/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql)
- [Démarrage rapide : Exemples utilisant l’instruction COPY et les méthodes d’authentification prises en charge](/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql-examples)
- [Démarrage rapide : Création de l’instruction COPY à l’aide de l’interface utilisateur de Synapse Studio enrichie](/azure/synapse-analytics/quickstart-load-studio-sql-pool)

## <a name="syntax"></a>Syntaxe  

```syntaxsql
COPY INTO [schema.]table_name
[(Column_list)] 
FROM '<external_location>' [,...n]
WITH  
 ( 
 [FILE_TYPE = {'CSV' | 'PARQUET' | 'ORC'} ]
 [,FILE_FORMAT = EXTERNAL FILE FORMAT OBJECT ]  
 [,CREDENTIAL = (AZURE CREDENTIAL) ]
 [,ERRORFILE = '[http(s)://storageaccount/container]/errorfile_directory[/]]' 
 [,ERRORFILE_CREDENTIAL = (AZURE CREDENTIAL) ]
 [,MAXERRORS = max_errors ] 
 [,COMPRESSION = { 'Gzip' | 'DefaultCodec'| 'Snappy'}] 
 [,FIELDQUOTE = 'string_delimiter'] 
 [,FIELDTERMINATOR =  'field_terminator']  
 [,ROWTERMINATOR = 'row_terminator']
 [,FIRSTROW = first_row]
 [,DATEFORMAT = 'date_format'] 
 [,ENCODING = {'UTF8'|'UTF16'}] 
 [,IDENTITY_INSERT = {'ON' | 'OFF'}]
)
```

## <a name="arguments"></a>Arguments  

*schema_name*  
Est facultatif si le schéma par défaut de l’utilisateur réalisant l’opération est le schéma de la table spécifiée. Si *schema* n’est pas spécifié et que le schéma par défaut de l’utilisateur réalisant l’opération COPY diffère de celui de la table, cette opération est annulée, et un message d’erreur est retourné.  

*table_name*  
Correspond au nom de la table dans laquelle copier les données. La table cible peut être une table temporaire ou permanente et elle doit déjà exister dans la base de données. 

*(column_list)*  
Est une liste facultative d’une ou de plusieurs colonnes utilisées pour mapper les champs de données sources aux colonnes de la table cible en vue du chargement des données. *column_list* doit être placé entre parenthèses et délimité par des virgules. La liste des colonnes est au format suivant :

[(Column_name [Default_value] [Field_number] [,...n])]

- *Column_name* : nom de la colonne dans la table cible.
- *Default_value* : valeur par défaut qui remplace toute valeur NULL dans le fichier d’entrée. Elle s’applique à tous les formats de fichier. L’instruction COPY tente de charger une valeur NULL à partir du fichier d’entrée quand une colonne de la liste de colonnes est omise ou qu’un champ est vide dans le fichier d’entrée.
- *Field_number* : numéro du champ du fichier d’entrée à mapper au nom de la colonne cible.
- L’indexation des champs commence au numéro 1.

Quand aucune liste de colonnes n’est spécifiée, COPY mappe les colonnes en suivant l’ordre de la source et de la cible : le champ d’entrée 1 est mappé à la colonne cible 1, le champ 2 à la colonne 2, et ainsi de suite.

*Emplacement(s) externe(s)*</br>
Désigne l’emplacement où sont placés les fichiers de données. Actuellement, Azure Data Lake Storage (ADLS) Gen2 et Stockage Blob Azure sont pris en charge :

- *Emplacement externe* pour le Stockage Blob : https://<account>.blob.core.windows.net/<container>/<path>
- *Emplacement externe* pour ADLS Gen2 : https://<account>. dfs.core.windows.net/<container>/<path>

> [!NOTE]  
> Le point de terminaison .blob est également disponible pour la ADLS Gen2 et produit actuellement les meilleures performances. Utilisez le point de terminaison .blob quand .dfs n’est pas nécessaire pour votre méthode d’authentification.

- *Account* : nom du compte de stockage

- *Container* : nom du conteneur d’objets blob

- *Path* : chemin du fichier ou dossier contenant les données. L’emplacement part du conteneur. Si un dossier est spécifié, l’opération COPY récupère tous les fichiers du dossier et de tous ses sous-dossiers. COPY ignore les dossiers masqués et ne retourne pas les fichiers dont le nom débute par un trait de soulignement (_) ou un point (.), sauf si ces caractères sont explicitement spécifiés dans le chemin. Ce comportement est le même quand vous spécifiez un chemin comportant un caractère générique.

Les caractères génériques sont autorisés dans les chemins dans les cas suivants

- La correspondance des noms de chemin contenant des caractères génériques respecte la casse
- Un caractère générique peut être placé dans une séquence d’échappement par l’usage de la barre oblique inverse (\\)
- Le développement des caractères génériques est effectué de manière récursive. Par exemple, tous les fichiers CSV sous Customer1 (y compris les sous-répertoires de Customer1 seront chargés dans l’exemple suivant : « Account/Container/Customer1/*.csv »

> [!NOTE]  
> Pour des performances optimales, ne spécifiez pas de caractères génériques qui seraient développés dans plus de fichiers. Dans la mesure du possible, listez plusieurs emplacements de fichiers au lieu de spécifier des caractères génériques.

Les emplacements de fichiers multiples peuvent uniquement être spécifiés à partir du même compte de stockage et du même conteneur en utilisant une liste d’emplacements séparés par des virgules, comme ceci :

- 'https://<account>.blob.core.windows.net/<container>/<path>', 'https://<account>.blob.core.windows.net/<container>/<path>', etc.

*FILE_TYPE = { ‘CSV’ | ‘PARQUET’ | ‘ORC’ }*</br>
*FILE_TYPE* spécifie le format des données externes.

- CSV : spécifie un fichier de valeurs séparées par des virgules conformément à la norme [RFC 4180](https://tools.ietf.org/html/rfc4180).
- PARQUET : spécifie un format Parquet.
- ORC : Spécifie un format ORC.

>[!NOTE]  
>Le type de fichier « Texte délimité » dans Polybase est remplacé par le format de fichier « CSV » dans lequel le délimiteur par défaut (la virgule) peut être configuré à l’aide du paramètre FIELDTERMINATOR. 

*FILE_FORMAT = external_file_format_name*</br>
*FILE_FORMAT* s’applique aux fichiers Parquet et ORC uniquement ; il spécifie le nom de l’objet de format de fichier externe qui stocke le type de fichier et la méthode de compression des données externes. Pour créer un format de fichier externe, utilisez [CREATE EXTERNAL FILE FORMAT](create-external-file-format-transact-sql.md).

*CREDENTIAL (IDENTITY = ‘’, SECRET = ‘’)*</br>
*CREDENTIAL* spécifie le mécanisme d’authentification pour accéder au compte de stockage externe. Voici les méthodes d’authentification :

|                          |                CSV                |                      Parquet                       |                        ORC                         |
| :----------------------: | :-------------------------------: | :------------------------------------------------: | :------------------------------------------------: |
|  **Stockage Blob Azure**  | SAS/MSI/SERVICE PRINCIPAL/KEY/AAD |                      SAS/KEY                       |                      SAS/KEY                       |
| **Azure Data Lake Gen2** | SAS/MSI/SERVICE PRINCIPAL/KEY/AAD | SAS (blob<sup>1</sup>)/MSI (dfs<sup>2</sup>)/SERVICE PRINCIPAL/KEY/AAD | SAS (blob<sup>1</sup>)/MSI (dfs<sup>2</sup>)/SERVICE PRINCIPAL/KEY/AAD |

1 : Le point de terminaison .blob ( **.blob**.core.windows.net) dans le chemin de votre emplacement externe est nécessaire pour cette méthode d’authentification.

2 : Le point de terminaison .dfs ( **.dfs**.core.windows.net) dans le chemin de votre emplacement externe est nécessaire pour cette méthode d’authentification.


> [!NOTE]  
>
> - Avec l’authentification à l’aide d’AAD ou auprès d’un compte de stockage public, vous n’êtes pas tenu de spécifier CREDENTIAL. 
> - Si votre compte de stockage est associé à un réseau virtuel, vous devez vous authentifier à l’aide de MSI (Managed Identity).

- Authentification avec la signature d’accès partagé (SAS)
  
  - *IDENTITY : constante avec une valeur « Shared Access Signature »*
  - *SECRET : La* [*signature d’accès partagé*](/azure/storage/common/storage-sas-overview) *fournit un accès délégué aux ressources de votre compte de stockage.*
  -  Autorisations minimales requises : READ et LIST
  
- Authentification avec des [*principaux de service*](/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store#create-a-credential)

  - *IDENTITY : <ClientID>@<OAuth_2.0_Token_EndPoint>*
  - *SECRET : clé du principal de service de l’application AAD*
  -  Rôles RBAC minimum requis : Contributeur aux données Blob du stockage, Propriétaire des données Blob du stockage ou Lecteur des données Blob du stockage

- Authentification avec une clé de compte de stockage
  
  - *IDENTITY : constante avec une valeur « Storage Account Key »*
  - *SECRET : clé du compte de stockage*
  
- Authentification avec une [identité managée](/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase#authenticate-using-managed-identities-to-load-optional) (points de terminaison du service VNet)
  
  - *IDENTITY : constante avec une valeur « Managed Identity »*
  - Rôles RBAC minimum requis : Contributeur aux données Blob du stockage ou Propriétaire des données Blob du stockage pour le serveur SQL Database inscrit auprès d’AAD
  
- Authentification avec un utilisateur AAD
  
  - *CREDENTIAL n’est pas requis*
  - Rôles RBAC minimum requis : Contributeur aux données Blob du stockage ou Propriétaire des données Blob du stockage pour l’utilisateur AAD

*ERRORFILE = emplacement du répertoire*</br>
*ERRORFILE* s’applique uniquement au format CSV. Spécifie le répertoire dans l’instruction COPY dans lequel les lignes rejetées et le fichier d’erreur correspondant doivent être écrits. Il est possible de spécifier le chemin complet du compte de stockage ou le chemin relatif du conteneur. Si le chemin spécifié n’existe pas, un chemin est créé pour vous. Un répertoire enfant est créé sous le nom « _rejectedrows ». Le caractère «_   » garantit que le répertoire est placé dans une séquence d’échappement pour le traitement d’autres données, sauf s’il est explicitement nommé dans le paramètre d’emplacement. 

Dans ce répertoire figure un dossier qui est créé d’après l’heure de soumission du chargement au format AnnéeMoisJour - HeureMinuteSeconde (par exemple, 20180330-173205). Dans ce dossier, deux types de fichiers sont écrits : le fichier de la raison (erreur) et le fichier des données (ligne). Chaque fichier est prérempli avec un ID de requête (queryID), un ID de distribution (distributionID) et un GUID de fichier. Comme les données et la raison se trouvent dans des fichiers distincts, les fichiers correspondants ont un préfixe analogue.

Si ERRORFILE a le chemin complet du compte de stockage défini, ERRORFILE_CREDENTIAL sera utilisé pour les connexions à ce stockage. Sinon, la valeur spécifiée pour CREDENTIAL sera utilisée.

*ERRORFILE_CREDENTIAL = (IDENTITY= ‘’, SECRET = ‘’)*</br>
*ERRORFILE_CREDENTIAL* s’applique uniquement aux fichiers CSV. Les sources de données et les méthodes d’authentification suivantes sont prises en charge :

- Stockage Blob Azure - SAS/SERVICE PRINCIPAL/KEY/AAD
- Azure Data Lake Gen2 - SAS/MSI/SERVICE PRINCIPAL/KEY/AAD
  
- Authentification avec la signature d’accès partagé (SAS)
  - *IDENTITY : constante avec une valeur « Shared Access Signature »*
  - *SECRET : La* [*signature d’accès partagé*](/azure/storage/common/storage-sas-overview) *fournit un accès délégué aux ressources de votre compte de stockage.*
  - Autorisations minimales requises : READ, LIST, WRITE, CREATE, DELETE
  
- Authentification avec des [*principaux de service*](/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store#create-a-credential)
  - *IDENTITY : <ClientID>@<OAuth_2.0_Token_EndPoint>*
  - *SECRET : clé du principal de service de l’application AAD*
  - Rôles RBAC minimum requis : Contributeur aux données Blob du stockage ou Propriétaire des données Blob du stockage
  
> [!NOTE]  
> Utilisez le point de terminaison de jeton OAuth 2.0 **V1**

- Authentification avec une clé de compte de stockage
  - *IDENTITY : constante avec une valeur « Storage Account Key »*
  - *SECRET : clé du compte de stockage*
  
- Authentification avec une [identité managée](/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase#authenticate-using-managed-identities-to-load-optional) (points de terminaison du service VNet)
  - *IDENTITY : constante avec une valeur « Managed Identity »*
  - Rôles RBAC minimum requis : Contributeur aux données Blob du stockage ou Propriétaire des données Blob du stockage pour le serveur SQL Database inscrit auprès d’AAD
  
- Authentification avec un utilisateur AAD
  - *CREDENTIAL n’est pas requis*
  - Rôles RBAC minimum requis : Contributeur aux données Blob du stockage ou Propriétaire des données Blob du stockage pour l’utilisateur AAD

> [!NOTE]  
> Si vous utilisez le même compte de stockage pour ERRORFILE et que vous spécifiez pour ERRORFILE un chemin relatif à la racine du conteneur, vous n’avez pas besoin de spécifier ERROR_CREDENTIAL.

*MAXERRORS = max_errors*</br>
*MAXERRORS* spécifie le nombre maximal de lignes rejetées autorisées dans la charge avant l’annulation de l’opération COPY. Chaque ligne qui ne peut pas être importée par l’opération COPY est ignorée et est comptabilisée comme une erreur. Si max_errors n’est pas spécifié, la valeur par défaut est 0.

*COMPRESSION = { 'DefaultCodec '\| ’Snappy’ \| ‘GZIP’ \| ‘NONE’}*</br>
*COMPRESSION* est facultatif et spécifie la méthode de compression appliquée aux données externes.

- CSV prend en charge GZIP
- Parquet prend en charge GZIP et Snappy
- ORC prend en charge DefaultCodec et Snappy
- Zlib est la compression par défaut utilisée pour ORC

La commande COPY détecte automatiquement le type de compression en fonction de l’extension de fichier quand ce paramètre n’est pas spécifié :

- .gz  - **GZIP**
- .snappy – **Snappy**
- .deflate - **DefaultCodec**  (Parquet et ORC uniquement)

 *FIELDQUOTE = 'field_quote'*</br>
*FIELDQUOTE* s’applique au format CSV et spécifie un caractère unique qui sera utilisé comme guillemet (délimiteur de chaîne) dans le fichier CSV. Si vous ne spécifiez pas cet argument, le caractère guillemet (") servira de guillemet conformément à l’utilisation définie dans la norme RFC 4180. Les caractères ASCII étendus et à plusieurs octets ne sont pas pris en charge avec UTF-8 pour FIELDQUOTE.

> [!NOTE]  
> Les caractères FIELDQUOTE sont placés dans une séquence d’échappement dans les colonnes de type chaîne qui contiennent un double FIELDQUOTE (délimiteur). 

*FIELDTERMINATOR = 'field_terminator’*</br>
*FIELDTERMINATOR* s’applique uniquement au format CSV. Spécifie la marque de fin de champ à utiliser dans le fichier CSV. La marque de fin de champ peut être spécifiée en notation hexadécimale. Plusieurs caractères peuvent être utilisés comme marque de fin de champ. La virgule (,) est la marque de fin de champ par défaut. Les caractères ASCII étendus et à plusieurs octets et ne sont pas pris en charge avec UTF-8 pour FIELDTERMINATOR.

ROW TERMINATOR = 'row_terminator'</br>
*ROW TERMINATOR* s’applique uniquement au format CSV. Spécifie la marque de fin de ligne à utiliser dans le fichier CSV. La marque de fin de ligne peut être spécifiée en notation hexadécimale. Plusieurs caractères peuvent être utilisés comme marque de fin de ligne. La combinaison de caractères « \r\n » est la marque de fin de ligne par défaut. 

La commande COPY ajoute le caractère \r quand vous spécifiez le caractère \n (nouvelle ligne), ce qui donne \r\n. Pour spécifier le caractère \n uniquement, utilisez sa notation hexadécimale (0x0A). Si vous spécifiez des marques de fin de ligne à plusieurs caractères au format hexadécimal, n’ajoutez pas « 0x » entre chaque caractère.

Les caractères ASCII étendus et à plusieurs octets ne sont pas pris en charge avec UTF-8 pour ROW TERMINATOR.

*FIRSTROW  = First_row_int*</br>
*FIRSTROW* s’applique au format CSV et spécifie le numéro de ligne qui est lu en premier dans tous les fichiers par la commande COPY. Les valeurs commencent à 1, la valeur par défaut. Si la valeur est définie sur 2, la première ligne de chaque fichier (ligne d’en-tête) est ignorée lorsque les données sont chargées. Les lignes sont ignorées en présence de marques de fin de ligne.

*DATEFORMAT = { ‘mdy’ \| ‘dmy’ \| ‘ymd’ \| ‘ydm’ \| ‘myd’ \| ‘dym’ }*</br>
DATEFORMAT s’applique uniquement au format CSV et spécifie le format de date pour le mappage des dates avec les formats de date SQL Server. Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure Transact-SQL, consultez [Types de données et fonctions de date et d’heure (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md). Dans la commande COPY, DATEFORMAT a une précédence supérieure au [DATEFORMAT configuré au niveau de la session](set-dateformat-transact-sql.md).

*ENCODING = ‘UTF8’ | ‘UTF16’*</br>
*ENCODING* s’applique uniquement au format CSV. La valeur par défaut est UTF8. Spécifie la norme d’encodage des données dans les fichiers chargés par la commande COPY. 

*IDENTITY_INSERT = ‘ON’ | ‘OFF’*</br>
IDENTITY_INSERT spécifie si la ou les valeurs d’identité figurant dans le fichier de données importé doivent être utilisées pour la colonne d’identité. Si IDENTITY_INSERT est défini sur OFF (valeur par défaut), les valeurs d’identité de cette colonne sont vérifiées, mais pas importées. Azure Synapse Analytics affectera automatiquement des valeurs uniques en fonction des valeurs de départ et d’incrément spécifiées au moment de la création de la table. Notez le comportement suivant avec la commande COPY :

- Quand IDENTITY_INSERT est défini sur OFF et que la table possède une colonne d’identité
  - Une liste de colonnes doit être spécifiée, mais elle ne doit pas mapper un champ d’entrée à la colonne d’identité.
- Quand IDENTITY_INSERT est défini sur ON et que la table possède une colonne d’identité
  - Si une liste de colonnes est passée, elle doit mapper un champ d’entrée à la colonne d’identité.
- La valeur par défaut n’est pas prise en charge pour la colonne d’identité (IDENTITY) dans la liste des colonnes.
- IDENTITY_INSERT ne peut être défini que pour une table à la fois.

### <a name="permissions"></a>Autorisations  

L’utilisateur qui exécute la commande COPY doit avoir les autorisations suivantes : 

- [ADMINISTER DATABASE BULK OPERATIONS](grant-database-permissions-transact-sql.md#remarks)
- [INSERT ](grant-database-permissions-transact-sql.md#remarks)

Requiert les autorisations INSERT et ADMINISTER BULK OPERATIONS. Dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], les autorisations INSERT et ADMINISTER DATABASE BULK OPERATIONS sont nécessaires.

## <a name="examples"></a>Exemples  

### <a name="a-load-from-a-public-storage-account"></a>R. Charger à partir d’un compte de stockage public

L’exemple suivant est la forme la plus simple de la commande COPY, qui charge les données à partir d’un compte de stockage public. Ici, les valeurs par défaut de l’instruction COPY correspondent au format du fichier CSV « lineitem ».

```sql
COPY INTO dbo.[lineitem] FROM 'https://unsecureaccount.blob.core.windows.net/customerdatasets/folder1/lineitem.csv'
```

Ces valeurs par défaut sont les suivantes :

- DATEFORMAT = Session DATEFORMAT 

- MAXERRORS = 0

- COMPRESSION = désactivée par défaut

- FIELDQUOTE = “” 

- FIELDTERMINATOR = “,” 

- ROWTERMINATOR = '\n'

> [!IMPORTANT]
> COPY traite '\n' comme '\r\n' en interne. Pour plus d’informations, consultez la section ROWTERMINATOR.

- FIRSTROW = 1

- ENCODING = 'UTF8'

- FILE_TYPE = 'CSV'

- IDENTITY_INSERT = 'OFF'

### <a name="b-load-authenticating-via-share-access-signature-sas"></a>B. Charger l’authentification avec la signature d’accès partagé (SAS)

L’exemple suivant charge des fichiers qui utilisent le saut de ligne comme marque de fin de ligne, par exemple une sortie UNIX. Il utilise également une clé SAS pour l’authentification auprès du Stockage Blob Azure.

```sql
COPY INTO test_1
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>'),
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='?sv=2018-03-28&ss=bfqt&srt=sco&sp=rl&st=2016-10-17T20%3A14%3A55Z&se=2021-10-18T20%3A19%3A00Z&sig=IEoOdmeYnE9%2FKiJDSHFSYsz4AkNa%2F%2BTx61FuQ%2FfKHefqoBE%3D'),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=';',
    ROWTERMINATOR='0X0A',
    ENCODING = 'UTF8',
    DATEFORMAT = 'ymd',
    MAXERRORS = 10,
    ERRORFILE = '/errorsfolder',--path starting from the storage container
    IDENTITY_INSERT = 'ON'
)
```

### <a name="c-load-with-a-column-list-with-default-values-authenticating-via-storage-account-key"></a>C. Charger avec une liste de colonnes contenant des valeurs par défaut pour l’authentification avec une clé de compte de stockage

Cet exemple charge des fichiers en spécifiant une liste de colonnes avec des valeurs par défaut. 

```sql
--Note when specifying the column list, input field numbers start from 1
COPY INTO test_1 (Col_one default 'myStringDefault' 1, Col_two default 1 3)
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='<Your_Account_Key>'),
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='x6RWv4It5F2msnjelv3H4DA80n0PQW0daPdw43jM0nyetx4c6CpDkdj3986DX5AHFMIf/YN4y6kkCnU8lb+Wx0Pj+6MDw=='),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=',',
    ROWTERMINATOR='0x0A',
    ENCODING = 'UTF8',
    FIRSTROW = 2
)
```

### <a name="d-load-parquet-or-orc-using-existing-file-format-object"></a>D. Charger Parquet ou ORC à l’aide d’un objet de format de fichier existant

 Cet exemple utilise un caractère générique pour charger tous les fichiers Parquet dans un dossier. 

```sql
COPY INTO test_parquet
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/*.parquet'
WITH (
    FILE_FORMAT = myFileFormat,
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>')
)
```

### <a name="e-load-specifying-wild-cards-and-multiple-files"></a>E. Charger en spécifiant des caractères génériques et plusieurs fichiers

```sql
COPY INTO t1
FROM 
'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt', 
    'https://myaccount.blob.core.windows.net/myblobcontainer/folder1'
WITH ( 
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= '<client_id>@<OAuth_2.0_Token_EndPoint>',SECRET='<key>'),
    FIELDTERMINATOR = '|'
)
```

### <a name="f-load-using-msi-credentials"></a>F. Charger en utilisant des informations d’identification MSI

```sql
COPY INTO dbo.myCOPYDemoTable
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL = (IDENTITY = 'Managed Identity'),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=','
)
```

## <a name="faq"></a>Questions fréquentes (FAQ)

### <a name="what-is-the-performance-of-the-copy-command-compared-to-polybase"></a>Quelles sont les performances de la commande COPY par rapport à Polybase ?
La commande COPY offre de meilleures performances en fonction de votre charge de travail. Pour obtenir de meilleures performances de chargement, envisagez de fractionner votre entrée en plusieurs fichiers lors du chargement du CSV. Ces instructions s’appliquent également aux fichiers compressés avec gzip.

### <a name="what-is-the-file-splitting-guidance-for-the-copy-command-loading-csv-files"></a>Quelles sont les recommandations de fractionnement de fichiers pour la commande COPY lors du chargement de fichiers CSV ?
Le tableau ci-dessous indique les nombres de fichiers conseillés. Une fois le nombre de fichiers recommandé atteint, vous obtenez de meilleures performances, plus les fichiers sont volumineux. Pour une expérience de fractionnement de fichiers simple, reportez-vous à la [documentation](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/how-to-maximize-copy-load-throughput-with-file-splits/ba-p/1314474) suivante. 

| **DWU** | **Nombre de fichiers** |
| :-----: | :--------: |
|   100   |     60     |
|   200   |     60     |
|   300   |     60     |
|   400   |     60     |
|   500   |     60     |
|  1 000  |    120     |
|  1 500  |    180     |
|  2 000  |    240     |
|  2 500  |    300     |
|  3 000  |    360     |
|  5 000  |    600     |
|  6 000 / 750  |    720     |
|  7 500  |    900     |
| 10 000  |    1200    |
| 15,000  |    1800    |
| 30,000  |    3600    |


### <a name="what-is-the-file-splitting-guidance-for-the-copy-command-loading-parquet-or-orc-files"></a>Quelles sont les recommandations de fractionnement de fichiers pour la commande COPY lors du chargement de fichiers Parquet ou ORC ?
Il n’est pas nécessaire de fractionner les fichiers Parquet et ORC car la commande COPY le fait automatiquement. Les fichiers Parquet et ORC du compte de stockage Azure doivent avoir une taille de 256 Mo ou plus pour obtenir des performances optimales. 

### <a name="are-there-any-limitations-on-the-number-or-size-of-files"></a>Existe-t-il des restrictions quant au nombre ou à la taille des fichiers ?
Il n’existe aucune limite quant au nombre ou à la taille des fichiers ; toutefois, pour des performances optimales, nous recommandons des fichiers d’au moins 4 Mo.

### <a name="are-there-any-known-issues-with-the-copy-statement"></a>Existe-t-il des problèmes connus avec l’instruction COPY ?
Si vous avez un espace de travail Synapse qui a été créé avant le 07/12/2020, vous pouvez rencontrer un message d’erreur similaire lors de l’authentification avec une identité managée :

*com.microsoft.sqlserver.jdbc.SQLServerException : Managed Service Identity n’a pas été activée sur ce serveur. Activez Managed Service Identity, puis réessayez.*

Effectuez les étapes suivantes pour contourner ce problème en réinscrivant l’identité managée de l’espace de travail :

1. Accédez à votre espace de travail Synapse sur le portail Azure
2. Accédez au panneau Identités managées 
3. Si l’option « Autoriser les pipelines » est déjà activée, vous devez désactiver ce paramètre et enregistrer
4. Cochez l’option « Autoriser les pipelines » et enregistrez


## <a name="see-also"></a>Voir aussi  

 [Vue d’ensemble du chargement avec [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/design-elt-data-loading)
