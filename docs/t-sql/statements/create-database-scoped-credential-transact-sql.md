---
description: CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)
title: CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE SCOPED CREDENTIAL
- DATABASE_SCOPED_CREDENTIAL_TSQL
- SCOPED_TSQL
- CREATE_DATABASE_SCOPED_CREDENTIAL
- CREATE_DATABASE_SCOPED_CREDENTIAL_TSQL
- SCOPED_CREDENTIAL_TSQL
- SCOPED_CREDENTIAL
helpviewer_keywords:
- DATABASE SCOPED CREDENTIAL statement
- credentials [SQL Server], DATABASE SCOPED CREDENTIAL statement
ms.assetid: fe830577-11ca-44e5-953b-2d589d54d045
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=aps-pdw-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1b9a9c27b80d702706d0b6bf1d7e7b3cb7d1d09b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192730"
---
# <a name="create-database-scoped-credential-transact-sql"></a>CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Crée les informations d’identification d’une base de données. Les informations d’identification d’une base de données ne sont pas mappées à un compte de connexion de serveur ou à un utilisateur de la base de données. Les informations d’identification sont utilisées par la base de données pour accéder à l’emplacement externe chaque fois que la base de données effectue une opération nécessitant un accès.

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
CREATE DATABASE SCOPED CREDENTIAL credential_name
WITH IDENTITY = 'identity_name'
    [ , SECRET = 'secret' ]

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*credential_name* : Spécifie le nom des informations d’identification délimitées à la base de données à créer. *credential_name* ne peut pas commencer par le signe dièse (#). Les informations d'identification système commencent avec ##.

IDENTITY **=’** _identity\_name_ **’**  : Spécifie le nom du compte à utiliser lors d’une connexion en dehors du serveur. Pour importer un fichier à partir du stockage Blob Azure avec une clé partagée, le nom de l’identité doit être `SHARED ACCESS SIGNATURE`. Pour charger des données dans Azure Synapse Analytics, n’importe quelle valeur valide peut être utilisée pour l’identité. Pour plus d’informations sur les signatures d’accès partagé, consultez [Utilisation des signatures d’accès partagé (SAP)](/azure/storage/storage-dotnet-shared-access-signature-part-1). Lorsque vous utilisez Kerberos (Windows Active Directory ou MIT KDC), n’utilisez pas le nom de domaine dans l’argument IDENTITY. Il doit s’agir simplement du nom du compte.

> [!IMPORTANT]
> Les connecteurs ODBC SQL, Oracle, Teradata et MongoDB pour Polybase prennent uniquement en charge l’authentification de base (l’authentification Kerberos n’est pas prise en charge).

> [!NOTE]
> WITH IDENTITY n’est pas requis si le conteneur dans le stockage Blob Azure est activé pour l’accès anonyme. Pour obtenir un exemple d’interrogation du stockage Blob Azure, consultez [Importation dans une table à partir d’un fichier stocké dans le stockage Blob Azure](../functions/openrowset-transact-sql.md#j-importing-into-a-table-from-a-file-stored-on-azure-blob-storage).

SECRET **=’** _secret_ **’**  : Spécifie le secret nécessaire pour l’authentification sortante. `SECRET` est obligatoire pour importer un fichier à partir du stockage Blob Azure. Pour effectuer le chargement du Stockage Blob Azure vers Azure Synapse Analytics ou Parallel Data Warehouse, le secret doit être la clé de stockage Azure.
> [!WARNING]
> La valeur de clé SAP peut commencer par un point d’interrogation (« ? »). Quand vous utilisez la clé SAP, vous devez supprimer le caractère « ? » initial. Sinon, vos efforts risquent d’être vains.

## <a name="remarks"></a>Notes

Les informations d’identification délimitées à la base de données sont un enregistrement qui contient les informations d’authentification exigées pour la connexion à une ressource en dehors de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La plupart des informations d'identification incluent un utilisateur et un mot de passe Windows.

Avant de créer des informations d’identification délimitées à la base de données, la base de données doit avoir une clé principale pour protéger les informations d’identification. Pour plus d’informations, consultez [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md).

Lorsque IDENTITY correspond à un utilisateur Windows, le secret peut être le mot de passe. Le secret est chiffré à l'aide de la clé principale de service. Si la clé principale de service est régénérée, le secret est chiffré de nouveau au moyen de la nouvelle clé principale de service.

Des détails sur les informations d’identification délimitées à la base de données sont consultables dans la vue de catalogue [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md).

Voici certaines applications d’informations d’identification délimitées à la base de données :

- [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] utilise les informations d’identification délimitées à la base de données pour accéder au stockage Blob Azure non public ou à des clusters Hadoop sécurisés par Kerberos avec PolyBase. Pour en savoir plus, consultez [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md).

- [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] utilise les informations d’identification délimitées à la base de données pour accéder au stockage Blob Azure non public avec PolyBase. Pour en savoir plus, consultez [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md).

- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] utilise les informations d’identification délimitées à la base de données pour sa fonctionnalité de requête globale. Il s’agit de la possibilité d’effectuer des requêtes sur plusieurs partitionnements de base de données.

- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] utilise les informations d’identification délimitées à la base de données pour écrire des fichiers d’événements étendus dans le stockage Blob Azure.

- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] utilise les informations d’identification délimitées à la base de données pour les pools élastiques. Pour plus d’informations, consultez la rubrique expliquant comment [maîtriser la croissance spectaculaire des bases de données élastiques](/azure/azure-sql/database/elastic-pool-overview).

- [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) utilisent les informations d’identification délimitées à la base de données pour accéder aux données à partir du stockage Blob Azure. Pour plus d’informations, consultez [Exemples d’accès en bloc à des données dans Stockage Blob Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md). 

## <a name="permissions"></a>Autorisations

Exige l’autorisation **CONTROL** sur la base de données.

## <a name="examples"></a>Exemples

### <a name="a-creating-a-database-scoped-credential-for-your-application"></a>R. Création d’informations d’identification délimitées à la base de données pour votre application

L’exemple suivant crée des informations d’identification délimitées à la base de données nommées `AppCred`. Les informations d’identification délimitées à la base de données contiennent l’utilisateur Windows `Mary5` et un mot de passe.

```sql
-- Create a db master key if one does not already exist, using your own password.
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';

-- Create a database scoped credential.
CREATE DATABASE SCOPED CREDENTIAL AppCred WITH IDENTITY = 'Mary5',
    SECRET = '<EnterStrongPasswordHere>';
```

### <a name="b-creating-a-database-scoped-credential-for-a-shared-access-signature"></a>B. Création d’informations d’identification délimitées à la base de données pour une signature d’accès partagé

L’exemple suivant crée des informations d’identification délimitées à la base de données qui peuvent être utilisées pour créer une [source de données externe](../../t-sql/statements/create-external-data-source-transact-sql.md), qui peut effectuer des opérations en bloc, telles que [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) et [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md). Les signatures d’accès partagé ne peuvent pas être utilisées avec PolyBase dans SQL Server, APS ou Azure Synapse Analytics.

```sql
-- Create a db master key if one does not already exist, using your own password.
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';

-- Create a database scoped credential.
CREATE DATABASE SCOPED CREDENTIAL MyCredentials
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
SECRET = 'QLYMgmSXMklt%2FI1U6DcVrQixnlU5Sgbtk1qDRakUBGs%3D';
```

### <a name="c-creating-a-database-scoped-credential-for-polybase-connectivity-to-azure-data-lake-store"></a>C. Création d’informations d’identification délimitées à la base de données pour la connectivité de PolyBase à Azure Data Lake Store

L’exemple suivant crée des informations d’identification délimitées à la base de données qui peuvent être utilisées pour créer une [source de données externe](../../t-sql/statements/create-external-data-source-transact-sql.md), qui peut être utilisée par PolyBase dans [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].

Azure Data Lake Store utilise une application Azure Active Directory pour l’authentification entre services.
[Créez une application AAD](/azure/data-lake-store/data-lake-store-authenticate-using-active-directory) et documentez vos paramètres client_id, OAuth_2.0_Token_EndPoint et Key avant d’essayer de créer des informations d’identification délimitées à la base de données.

```sql
-- Create a db master key if one does not already exist, using your own password.
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';

-- Create a database scoped credential.
CREATE DATABASE SCOPED CREDENTIAL ADL_User
WITH
    IDENTITY = '<client_id>@\<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;
```

## <a name="more-information"></a>Informations complémentaires

- [Informations d’identification &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [ALTER DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-credential-transact-sql.md)
- [DROP DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-scoped-credential-transact-sql.md)
- [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md)
- [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)
- [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)
