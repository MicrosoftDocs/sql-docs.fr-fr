---
title: Utilisation d’Always Encrypted avec SqlClient
description: Découvrez comment développer des applications avec Microsoft.Data.SqlClient et Always Encrypted pour garantir la sécurité de vos données.
ms.date: 03/03/2021
ms.assetid: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: d5b2e79205e3c82e22f723536e594fffd06ee99f
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464863"
---
# <a name="using-always-encrypted-with-the-microsoft-net-data-provider-for-sql-server"></a>Utilisation d’Always Encrypted avec le Fournisseur de données Microsoft .NET pour SQL Server

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

Cet article fournit des informations sur le développement d’applications .NET à l’aide d’[Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) ou [Always Encrypted avec enclaves sécurisées](../../../relational-databases/security/encryption/always-encrypted-enclaves.md) et du [**Fournisseur de données Microsoft .NET pour SQL Server**](../microsoft-ado-net-sql-server.md).

Always Encrypted permet aux applications clientes de chiffrer des données sensibles et de ne jamais révéler les données ou les clés de chiffrement à SQL Server ou Azure SQL Database. Pour garantir cette sécurité, un pilote activé avec Always Encrypted, tel que le **Fournisseur de données .NET pour SQL Server**, chiffre et déchiffre de manière transparente les données sensibles dans l’application cliente. Le pilote détermine automatiquement les paramètres de requêtes qui correspondent aux colonnes de base de données sensibles (protégées avec Always Encrypted) et chiffre les valeurs de ces paramètres avant de transmettre les données à SQL Server ou Azure SQL Database. De même, il déchiffre de manière transparente les données récupérées dans les colonnes de base de données chiffrées, qui figurent dans les résultats de la requête. Pour plus d’informations, consultez [Développer des applications à l’aide d’Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-client-development.md) et [Développer des applications à l’aide d’Always Encrypted avec enclaves sécurisées](../../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md).

## <a name="prerequisites"></a>Prérequis

- Configurez Always Encrypted dans votre base de données. Ce processus consiste à provisionner des clés Always Encrypted et à configurer le chiffrement pour les colonnes de base de données sélectionnées. Si vous n’avez pas déjà une base de données configurée avec Always Encrypted, suivez les instructions de [Bien démarrer avec Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted).
- Si vous utilisez Always Encrypted avec enclaves sécurisées, consultez [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](../../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md) pour connaître les autres prérequis.
- Vérifiez que la plateforme .NET requise est installée sur votre ordinateur de développement. Avec [Microsoft. Data. SqlClient](../microsoft-ado-net-sql-server.md), la fonctionnalité Always Encrypted est prise en charge pour .NET Framework et .NET Core. Assurez-vous que [.NET Framework 4.6](/dotnet/framework/) ou version ultérieure ou [.NET Core 2.1](/dotnet/core/) ou version ultérieure est configurée comme version cible de la plateforme .NET dans votre environnement de développement. À partir de Microsoft.Data.SqlClient version 2.1.0, la fonctionnalité Always Encrypted est également prise en charge pour [.NET Standard 2.0](/dotnet/standard/net-standard). Pour utiliser Always Encrypted avec enclaves sécurisés, [.NET Standard 2.1](/dotnet/standard/net-standard) est requis. Si vous utilisez Visual Studio, veuillez vous reporter à la [Vue d’ensemble ciblant l’infrastructure](/visualstudio/ide/visual-studio-multi-targeting-overview).

La table suivante récapitule les plateformes .NET requises pour utiliser Always Encrypted avec **Microsoft.Data.SqlClient**.

| Support Always Encrypted | Support Always Encrypted avec enclave sécurisée  | Framework cible | Version Microsoft.Data.SqlClient | Système d’exploitation |
|:--|:--|:--|:--:|:--:|
| Oui | Oui | .NET Framework 4.6+ | 1.1.0+ | Windows |
| Oui | Oui | .NET Core 2.1+ | 2.1.0+<sup>1</sup> | Windows, Linux, macOS |
| Oui | Non | .NET Standard 2.0 | 2.1.0+ | Windows, Linux, macOS |
| Oui | Oui | .NET Standard 2.1+ | 2.1.0+ | Windows, Linux, macOS |

> [!NOTE]
> <sup>1</sup> Avant Microsoft.Data.SqlClient version 2.1.0, Always Encrypted est pris en charge uniquement sur Windows.

## <a name="enabling-always-encrypted-for-application-queries"></a>Activation d’Always Encrypted pour les requêtes d’application

Le moyen le plus simple d’activer le chiffrement des paramètres et le déchiffrement des résultats de requête ciblant des colonnes chiffrées est de définir la valeur du mot clé de la chaîne de connexion `Column Encryption Setting` sur **activé**.

Voici un exemple de chaîne de connexion activant Always Encrypted :

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
SqlConnection connection = new SqlConnection(connectionString);
```

L’exemple d’extrait de code suivant est équivalent et utilise la propriété SqlConnectionStringBuilder.ColumnEncryptionSetting.

```cs
SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
builder.DataSource = "server63";
builder.InitialCatalog = "Clinic";
builder.IntegratedSecurity = true;
builder.ColumnEncryptionSetting = SqlConnectionColumnEncryptionSetting.Enabled;
SqlConnection connection = new SqlConnection(builder.ConnectionString);
connection.Open();
```

Always Encrypted peut également être activé pour les requêtes individuelles. Consultez la section **Contrôle de l’impact d’Always Encrypted sur les performances** ci-dessous.
L’activation d’Always Encrypted ne suffit pas à la réussite du chiffrement ou du déchiffrement. Vous devez également vérifier ce qui suit :

- L’application dispose des autorisations de base de données *VIEW ANY COLUMN MASTER KEY DEFINITION* et *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* qui sont nécessaires pour accéder aux métadonnées des clés Always Encrypted dans la base de données. Pour plus d’informations, consultez la [section Autorisations de bases de données dans Always Encrypted (moteur de base de données)](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).
- L’application peut accéder à la clé principale de colonne qui protège les clés de chiffrement de colonne, qui chiffrent les colonnes de base de données interrogées.

## <a name="enabling-always-encrypted-with-secure-enclaves"></a>Activation d’Always Encrypted avec des enclaves sécurisées

À partir de la version 1.1.0 de Microsoft.Data.SqlClient, le pilote prend en charge [Always Encrypted avec enclaves sécurisées](../../../relational-databases/security/encryption/always-encrypted-enclaves.md).

Pour obtenir des informations d’ordre général, consultez [Développer des applications à l’aide d’Always Encrypted avec enclaves sécurisées](../../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md).

Pour activer les calculs d’enclave pour une connexion de base de données, vous devez définir les mots clés de chaînes de connexion suivants, en plus de l’activation d’Always Encrypted (comme expliqué dans la section précédente) :

- `Attestation Protocol` : spécifie un protocole d’attestation.
  - Si vous utilisez [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH), ce mot clé doit avoir la valeur `HGS`.
  - Si vous utilisez [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] et Microsoft Azure Attestation, ce mot clé doit avoir la valeur `AAS`.

- `Enclave Attestation URL` : spécifie une URL d’attestation (un point de terminaison de service d’attestation). Vous devez obtenir une URL d’attestation pour votre environnement auprès de votre administrateur de services fédérés d’attestation.

  - Si vous utilisez [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH), consultez [Déterminer et partager l’URL d’attestation SGH](../../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Si vous utilisez [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] et Microsoft Azure Attestation, consultez [Déterminer l’URL d’attestation de votre stratégie d’attestation](../../../relational-databases/security/encryption/always-encrypted-enclaves.md#secure-enclave-attestation).

Pour obtenir un tutoriel pas à pas, consultez [Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées](tutorial-always-encrypted-enclaves-develop-net-apps.md).

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Récupération et modification des données dans des colonnes chiffrées

Lorsque vous activez Always Encrypted pour les requêtes d’application, vous pouvez utiliser les API SqlClient standard (consultez [Extraction et modification de données dans ADO.NET](/dotnet/framework/data/adonet/retrieving-and-modifying-data)) ou les API du [**Fournisseur de données Microsoft .NET pour SQL Server**](index.md) qui sont définies dans [Microsoft.Data.SqlClient Namespace](/dotnet/api/microsoft.data.sqlclient), pour récupérer ou modifier les données de colonnes de base de données chiffrées. Si votre application a les autorisations de base de données nécessaires et qu’elle a accès à la clé principale de colonne, le **Fournisseur de données Microsoft .NET pour SQL Server** chiffre tous les paramètres de requête qui ciblent des colonnes chiffrées et déchiffre les données extraites des colonnes chiffrées, en retournant les valeurs de texte en clair des types .NET qui correspondent aux types de données SQL Server définis pour les colonnes du schéma de base de données.
Si Always Encrypted n’est pas activé, les requêtes ayant des paramètres qui ciblent des colonnes chiffrées échouent. Une requête peut toujours récupérer des données à partir de colonnes chiffrées, tant qu’aucun de ses paramètres ne cible des colonnes chiffrées. Toutefois, le **Fournisseur de données Microsoft .NET pour SQL Server** ne tente pas de déchiffrer les valeurs extraites des colonnes chiffrées et l’application reçoit des données chiffrées binaires (sous la forme de tableaux d’octets).

Le tableau ci-dessous récapitule le comportement des requêtes, selon qu’Always Encrypted est activé ou non :

|Caractéristique de la requête | Always Encrypted est activé et l’application peut accéder aux clés et à leurs métadonnées|Always Encrypted est activé et l’application ne peut pas accéder aux clés et à leurs métadonnées | Always Encrypted est désactivé|
|:---|:---|:---|:---|
| Requêtes avec des paramètres ciblant des colonnes chiffrées. | Des valeurs de paramètres sont chiffrées en toute transparence. | Error | Error |
| Requêtes qui récupèrent des données à partir de colonnes chiffrées, sans paramètres ciblant des colonnes chiffrées. | Les résultats de colonnes chiffrées sont déchiffrés de manière transparente. L’application reçoit des valeurs en texte clair des types de données .NET correspondant aux types SQL Server configurés pour les colonnes chiffrées. | Error | Les résultats des colonnes chiffrées ne sont pas déchiffrés. L’application reçoit des valeurs chiffrées sous la forme de tableaux d’octets (byte[]). |

Les exemples suivants illustrent la récupération et la modification de données dans des colonnes chiffrées. Ces exemples impliquent une table cible avec le schéma ci-dessous. Les colonnes `SSN` et `BirthDate` sont chiffrées.

```sql
CREATE TABLE [dbo].[Patients]([PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

### <a name="inserting-data-example"></a>Exemple d’insertion de données

Cet exemple insère une ligne dans la table Patients. Notez les points suivants :

- L’exemple de code ne contient aucun élément propre au chiffrement. Le **Fournisseur de données Microsoft .NET pour SQL Server** détecte et chiffre automatiquement les paramètres `paramSSN` et `paramBirthdate` qui ciblent des colonnes chiffrées. Ce comportement rend le chiffrement transparent pour l’application.
- Les valeurs insérées dans les colonnes de base de données, y compris les colonnes chiffrées, sont passées en tant qu’objets [SqlParameter](/dotnet/api/microsoft.data.sqlclient.sqlparameter) . L’utilisation de **SqlParameter** est facultative pour l’envoi de valeurs aux colonnes non chiffrées (même si elle est vivement recommandée, car elle contribue à empêcher l’injection SQL), mais elle est nécessaire pour les valeurs qui ciblent des colonnes chiffrées. Si les valeurs insérées dans les colonnes `SSN` ou `BirthDate` ont été transmises en tant que littéraux incorporés dans l’instruction de requête, la requête échouera, car le **Fournisseur de données Microsoft .NET pour SQL Server** ne sera pas en mesure de déterminer les valeurs des colonnes chiffrées cibles et ne chiffrera donc pas les valeurs. Par conséquent, le serveur les rejettera en les considérant comme incompatibles avec les colonnes chiffrées.
- Le type de données du paramètre ciblant la colonne `SSN` est défini sur une chaîne ANSI (non Unicode) qui est mappée vers le type de données SQL Server char/varchar. Si le type du paramètre est défini sur une chaîne Unicode (String) mappée à nchar/nvarchar, la requête échoue, car Always Encrypted ne prend pas en charge les conversions de valeurs nchar/nvarchar chiffrées en valeurs char/varchar chiffrées. Pour plus d’informations sur les mappages de type de données, consultez [Mappages de type de données SQL Server](/dotnet/framework/data/adonet/sql-server-data-type-mappings) .
- Le type de données du paramètre inséré dans la colonne `BirthDate` est explicitement défini sur le type de données SQL Server cible à l’aide de la [Propriété SqlParameter.SqlDbType](/dotnet/api/microsoft.data.sqlclient.sqlparameter.sqldbtype), au lieu d’utiliser le mappage implicite des types .NET vers les types de données SQL Server appliqués lors de l’utilisation de la [Propriété SqlParameter.DbType](/dotnet/api/microsoft.data.sqlclient.sqlparameter.dbtype). Par défaut, la [structure DateHeure](/dotnet/api/system.datetime) est mappée vers le type de données SQL Server DateHeure. Étant donné que les données de la colonne `BirthDate` sont de type « date » et qu’Always Encrypted ne prend pas en charge la conversion de valeurs « datetime » chiffrées en valeurs « date » chiffrées, l’utilisation du mappage par défaut entraînerait une erreur.

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";

using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
using (SqlCommand cmd = connection.CreateCommand())
{
    connection.Open();
    cmd.CommandText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

    SqlParameter paramSSN = cmd.CreateParameter();
    paramSSN.ParameterName = @"@SSN";
    paramSSN.DbType = DbType.AnsiStringFixedLength;
    paramSSN.Direction = ParameterDirection.Input;
    paramSSN.Value = "795-73-9838";
    paramSSN.Size = 11;
    cmd.Parameters.Add(paramSSN);

    SqlParameter paramFirstName = cmd.CreateParameter();
    paramFirstName.ParameterName = @"@FirstName";
    paramFirstName.DbType = DbType.String;
    paramFirstName.Direction = ParameterDirection.Input;
    paramFirstName.Value = "Catherine";
    paramFirstName.Size = 50;
    cmd.Parameters.Add(paramFirstName);

    SqlParameter paramLastName = cmd.CreateParameter();
    paramLastName.ParameterName = @"@LastName";
    paramLastName.DbType = DbType.String;
    paramLastName.Direction = ParameterDirection.Input;
    paramLastName.Value = "Abel";
    paramLastName.Size = 50;
    cmd.Parameters.Add(paramLastName);

    SqlParameter paramBirthdate = cmd.CreateParameter();
    paramBirthdate.ParameterName = @"@BirthDate";
    paramBirthdate.SqlDbType = SqlDbType.Date;
    paramBirthdate.Direction = ParameterDirection.Input;
    paramBirthdate.Value = new DateTime(1996, 09, 10);
    cmd.Parameters.Add(paramBirthdate);

    cmd.ExecuteNonQuery();
}
```

### <a name="retrieving-plaintext-data-example"></a>Exemple de récupération de données en texte clair

L’exemple suivant montre le filtrage de données basé sur des valeurs chiffrées, ainsi que la récupération de données en texte clair à partir de colonnes chiffrées.

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
using (SqlCommand cmd = connection.CreateCommand())
{
    connection.Open();
    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN=@SSN";

    SqlParameter paramSSN = cmd.CreateParameter();
    paramSSN.ParameterName = @"@SSN";
    paramSSN.DbType = DbType.AnsiStringFixedLength;
    paramSSN.Direction = ParameterDirection.Input;
    paramSSN.Value = "795-73-9838";
    paramSSN.Size = 11;
    cmd.Parameters.Add(paramSSN);
    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
            {
                Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
            }
        }
    }
}
```

> [!NOTE]
>
> - La valeur utilisée dans la clause WHERE pour filtrer la colonne `SSN` doit être transmise à l’aide de SqlParameter, afin que le **Fournisseur de données .NET Microsoft pour SQL Server** puisse la chiffrer de manière transparente avant de l’envoyer à la base de données.
>
> - Toutes les valeurs imprimées par le programme sont sous la forme de texte en clair, car le **Fournisseur de données .NET Microsoft pour SQL Server** déchiffre de manière transparente les données récupérées à partir des colonnes `SSN` et `BirthDate`.
>
> - Les requêtes peuvent effectuer des comparaisons d’égalité sur des colonnes si elles sont chiffrées à l’aide du chiffrement déterministe. Pour plus d’informations, consultez [Sélection d’un chiffrement déterministe ou aléatoire](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

### <a name="retrieving-encrypted-data-example"></a>Exemple de récupération de données chiffrées

Si Always Encrypted n’est pas activé, une requête peut toujours récupérer des données à partir de colonnes chiffrées, tant qu’aucun de ses paramètres ne ciblent des colonnes chiffrées.

L’exemple suivant montre comment récupérer des données chiffrées binaires à partir de colonnes chiffrées.

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";

using (SqlConnection connection = new SqlConnection(connectionString))
using (SqlCommand cmd = connection.CreateCommand())
{
    connection.Open();
    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName";

    SqlParameter paramLastName = cmd.CreateParameter();
    paramLastName.ParameterName = @"@LastName";
    paramLastName.DbType = DbType.String;
    paramLastName.Direction = ParameterDirection.Input;
    paramLastName.Value = "Abel";
    paramLastName.Size = 50;
    cmd.Parameters.Add(paramLastName);
    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
            {
                Console.WriteLine(@"{0}, {1}, {2}, {3}", BitConverter.ToString((byte[])reader[0]), reader[1], reader[2], BitConverter.ToString((byte[])reader[3]));
            }
        }
    }
}
```

> [!NOTE]
> - Étant donné qu’Always Encrypted n’est pas toujours activé dans la chaîne de connexion, la requête retourne des valeurs `SSN` et `BirthDate`chiffrées sous la forme de tableaux d’octets (le programme convertit les valeurs en chaînes).
>
> - Une requête qui récupère des données à partir de colonnes chiffrées lorsqu’Always Encrypted est désactivé peut avoir des paramètres, tant qu’aucun d’eux ne cible une colonne chiffrée. La requête ci-dessus filtre par nom (LastName), qui n’est pas chiffré dans la base de données. Si la requête filtre par `SSN` ou `BirthDate`, la requête échoue.

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Éviter les problèmes courants lors de l’interrogation de colonnes chiffrées

Cette section décrit des catégories d’erreurs courantes liées à l’interrogation des colonnes chiffrées à partir d’applications .NET et fournit des conseils sur la façon de les éviter.

### <a name="unsupported-data-type-conversion-errors"></a>Erreurs liées à la conversion de types de données non pris en charge

Always Encrypted ne prend en charge que peu de conversions de types de données chiffrées. Consultez [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) pour obtenir la liste détaillée des conversions de types prises en charge. Procédez comme suit pour éviter les erreurs de conversion de type de données :

- Définissez les types de paramètres ciblant les colonnes chiffrées pour que le type de données SQL Server du paramètre soit exactement le même que le type de la colonne cible ou pour que soit prise en charge la conversion du type de données SQL Server du paramètre vers le type cible de la colonne. Vous pouvez appliquer le mappage des types de données .NET vers des types de données SQL Server spécifiques à l’aide de la propriété SqlParameter.SqlDbType.
- Vérifiez que la précision et l’échelle des paramètres ciblant les colonnes des types de données SQL Server decimal et numeric sont les mêmes que celles configurées pour la colonne cible.  
- Vérifiez que la précision des paramètres ciblant des colonnes des types de données SQL Server datetime2, datetimeoffset ou time n’est pas supérieure à celle de la colonne cible dans les requêtes qui modifient les valeurs de la colonne cible.  

### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Erreurs dues au passage de texte en clair au lieu de valeurs chiffrées

Les valeurs qui ciblent une colonne chiffrée doivent être chiffrées dans l’application. Toute tentative d’insertion, de modification ou de filtrage par une valeur de texte en clair dans une colonne chiffrée entraîne une erreur similaire à celle-ci :

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): Operand type clash: varchar is incompatible with varchar(8000) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', column_encryption_key_database_name = 'Clinic') collation_name = 'SQL_Latin1_General_CP1_CI_AS'
```

Pour éviter ces erreurs, procédez comme suit :

- Activez Always Encrypted pour les requêtes d’application ciblant des colonnes chiffrées (pour la chaîne de connexion ou dans l’objet [SqlCommand](/dotnet/api/microsoft.data.sqlclient.sqlcommand) pour une requête spécifique).
- Utilisez SqlParameter pour envoyer des données ciblant des colonnes chiffrées. L’exemple suivant illustre une requête qui filtre incorrectement une colonne chiffrée (SSN) à l’aide d’un littéral (ou d’une constante), au lieu de transférer le littéral à l’intérieur d’un objet SqlParameter.

```cs
using (SqlCommand cmd = connection.CreateCommand())
{
    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN = '795-73-9838'";
    cmd.ExecuteNonQuery();
}
```

## <a name="working-with-column-master-key-stores"></a>Utilisation de magasins de clés principales de colonne

Pour chiffrer une valeur de paramètre ou déchiffrer les données des résultats de requête, le **Fournisseur de données Microsoft .NET pour SQL Server** doit obtenir une clé de chiffrement de colonne qui soit configurée pour la colonne cible. Les clés de chiffrement de colonne sont stockées sous forme chiffrée dans les métadonnées de la base de données. Chaque clé de chiffrement de colonne a une clé principale de colonne correspondante qui a été utilisée pour chiffrer la clé de chiffrement de colonne. Les métadonnées de la base de données ne stockent pas les clés principales de colonne. Elles contiennent uniquement des informations sur un magasin de clés contenant une clé principale de colonne et sur l’emplacement de cette clé dans le magasin.

Pour obtenir une valeur en texte clair d’une clé de chiffrement de colonne, le **Fournisseur de données Microsoft .NET pour SQL Server** obtient d’abord les métadonnées relatives à la clé de chiffrement de colonne et à la clé principale de colonne correspondante. Il utilise ensuite les informations trouvées dans les métadonnées pour contacter le magasin de clés qui contient la clé principale de colonne et pour chiffrer la clé de chiffrement de colonne chiffrée. Le **Fournisseur de données Microsoft .NET pour SQL Server** communique avec un magasin de clés à l’aide d’un fournisseur de magasin de clé principales de colonne, qui est une instance de classe dérivée de la [classe SqlColumnEncryptionKeyStoreProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptionkeystoreprovider).

Le processus pour obtenir une clé de chiffrement de colonne est le suivant :

1. Si Always Encrypted est activé pour une requête, le **Fournisseur de données Microsoft .NET pour SQL Server** appelle en toute transparence **sys.sp_describe_parameter_encryption** pour récupérer les métadonnées de chiffrement pour les paramètres ciblant les colonnes chiffrées, si la requête possède des paramètres. Pour les données chiffrées contenues dans les résultats d’une requête, SQL Server affecte automatiquement des métadonnées de chiffrement. Les informations sur la clé principale de colonne sont les suivantes :
    - Le nom d’un fournisseur de magasins de clés qui encapsule un magasin de clés contenant la clé principale de colonne.
    - Le chemin d’accès à une clé qui spécifie l’emplacement de la clé CMK dans le magasin de clés.

    Les informations sur la clé de chiffrement de colonne sont les suivantes :

    - La valeur chiffrée d’une clé de chiffrement de colonne.
    - Le nom de l’algorithme utilisé pour chiffrer la clé CEK.
2. Le **Fournisseur de données Microsoft .NET pour SQL Server** utilise le nom du fournisseur du magasin de clés principales de colonne pour rechercher l’objet du fournisseur, qui est une instance de classe dérivée de la classe SqlColumnEncryptionKeyStoreProvider, dans une structure de données interne.
3. Pour déchiffrer la clé de chiffrement de colonne, le **Fournisseur de données Microsoft .NET pour SQL Server** appelle la méthode `SqlColumnEncryptionKeyStoreProvider.DecryptColumnEncryptionKey()`, en transférant le chemin d’accès de la clé principale de colonne, la valeur chiffrée de la clé de chiffrement de colonne et le nom de l’algorithme de chiffrement utilisé pour générer la clé de chiffrement de la colonne chiffrée.

### <a name="using-built-in-column-master-key-store-providers"></a>Utilisation des fournisseurs de magasin de clés principales de colonne intégrés

Le **Fournisseur de données Microsoft .NET pour SQL Server** est fourni avec les fournisseurs de magasin de clés principales de colonne intégrés suivants, qui sont pré-inscrits avec des noms de fournisseurs spécifiques (utilisés pour leur recherche). Ces fournisseurs de magasins de clés intégrés ne sont pris en charge que sur Windows.

| Classe | Description | Nom de fournisseur (pour la recherche) | Plateforme |
|:---|:---|:---|:---|
|[Classe SqlColumnEncryptionCertificateStoreProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider) | Fournisseur du magasin de certificats Windows. | MSSQL_CERTIFICATE_STORE | Windows |
|[Classe SqlColumnEncryptionCngProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncngprovider) | Fournisseur de magasin de clés pour l’ [API de chiffrement Microsoft : API de la prochaine génération (CNG)](/windows/win32/seccng/cng-portal). En général, ce type de magasin est un module de sécurité matériel, c’est-à-dire un périphérique physique qui protège et gère les clés numériques et fournit un traitement du chiffrement. | MSSQL_CNG_STORE | Windows |
| [Classe SqlColumnEncryptionCspProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncspprovider) | Fournisseur de magasin de clés pour l’ [API de chiffrement Microsoft (CAPI)](/windows/win32/seccrypto/cryptographic-service-providers). En général, ce type de magasin est un module de sécurité matériel, c’est-à-dire un périphérique physique qui protège et gère les clés numériques et fournit un traitement du chiffrement. | MSSQL_CSP_PROVIDER | Windows |

Vous n’avez pas besoin d’apporter des modifications au code de l’application pour utiliser ces fournisseurs, mais prenez les points suivants en considération :

- Vous (ou votre administrateur de base de données) devez vérifier que le nom du fournisseur (configuré dans les métadonnées de clé principale de colonne) est correct et que le chemin de la clé principale de colonne est valide pour un fournisseur donné. Il est recommandé de configurer les clés à l’aide d’outils tels que SQL Server Management Studio, qui génère automatiquement des noms de fournisseurs et des chemins de clés valides lors de l’émission de l’instruction [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) . Pour plus d’informations, consultez [Configurer Always Encrypted à l’aide de SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md) et [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).
- Vérifiez que votre application peut accéder à la clé dans le magasin de clés. Pour ce processus, vous devrez peut-être accorder à votre application l’accès à la clé et/ou au magasin de clés (selon le magasin de clés) ou effectuer d’autres étapes de configuration spécifiques au magasin de clés. Par exemple, pour accéder à un magasin de clés qui implémente CNG ou CAPI (comme un module de sécurité matériel), vous devrez vérifier qu’une bibliothèque implémentant CNG ou CAPI pour votre magasin est installée sur la machine de votre application. Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne pour Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

### <a name="using-the-azure-key-vault-provider"></a>Utilisation du fournisseur Azure Key Vault

Azure Key Vault est un outil est très pratique qui permet de stocker et de gérer des clés principales de colonne Always Encrypted, en particulier si vos applications sont hébergées dans Azure. Le **fournisseur de données Microsoft .NET pour SQL Server** ne comprend pas de fournisseur de magasin de clés principales de colonne intégré pour Azure Key Vault. Toutefois, celui-ci est disponible sous la forme d’un package NuGet ([Microsoft.Data.SqLClient.AlwaysEncrypted.AzureKeyVaultProvider](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider)) que vous pouvez facilement intégrer à votre application. Pour plus d’informations, consultez [Always Encrypted - Protéger les données sensibles de la base de données SQL à l’aide du chiffrement de base de données et stocker vos clés de chiffrement dans Azure Key Vault](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure).

| Classe | Description | Nom de fournisseur (pour la recherche) | Plateforme |
|:---|:---|:---|:---|
|[Classe SqlColumnEncryptionAzureKeyVaultProvider](/dotnet/api/microsoft.data.sqlclient.alwaysencrypted.azurekeyvaultprovider.sqlcolumnencryptionazurekeyvaultprovider) | Fournisseur pour Azure Key Vault. | AZURE_KEY_VAULT | Windows, Linux, macOS |

#### <a name="net-supportability"></a>Capacité de prise en charge de .NET

| Version | Version Microsoft.Data.SqlClient | Plateformes .NET | 
| ------- | ----------- | ------- |
| 2.0.0 | 1.1.3+ <br/> 2.1.0+ | .NET Framework 4.6.1+, .NET Core 2.1+ <br/>.NET Standard 2.0+ |
| 1.2.0 | 1.0.19269.1+ <br/> 2.1.0+ | .NET Framework 4.6+, .NET Core 2.1+ <br/>.NET Standard 2.0+ |
| 1.1.0 | 1.0.19269.1+ | .NET Framework 4.6+, .NET Core 2.1+ |
| 1.0.0 | 1.0.19269.1+ | .NET Framework 4.6+, .NET Core 2.1+ |
|||

À partir de la **version 2.0.0**, `Microsoft.Data.SqLClient.AlwaysEncrypted.AzureKeyVaultProvider` prend en charge les nouvelles API Azure.Core et Azure.Identity pour effectuer l’authentification auprès d’Azure Key Vault. Une instance de l’implémentation [`TokenCredential`](/dotnet/api/azure.core.tokencredential) peut maintenant être passée aux constructeurs [`SqlColumnEncryptionAzureKeyVaultProvider`](/dotnet/api/microsoft.data.sqlclient.alwaysencrypted.azurekeyvaultprovider.sqlcolumnencryptionazurekeyvaultprovider) pour initialiser l’objet fournisseur Azure Key Vault.

Pour obtenir des exemples illustrant le chiffrement/déchiffrement avec Azure Key Vault, consultez [Utilisation d’Azure Key Vault avec Always Encrypted](azure-key-vault-example.md) et [Utilisation d’Azure Key Vault avec Always Encrypted avec enclaves sécurisées](azure-key-vault-enclave-example.md).

### <a name="implementing-a-custom-column-master-key-store-provider"></a>Implémentation d’un fournisseur de magasin de clés principales de colonne personnalisé

Si vous voulez stocker des clés principales de colonne dans un magasin de clés qui n’est pas pris en charge par un fournisseur existant, vous pouvez implémenter un fournisseur personnalisé en étendant la [classe SqlColumnEncryptionKeyStoreProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptionkeystoreprovider) et en inscrivant le fournisseur à l’aide de la méthode [SqlConnection.RegisterColumnEncryptionKeyStoreProviders](/dotnet/api/microsoft.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders).

```cs
public class MyCustomKeyStoreProvider : SqlColumnEncryptionKeyStoreProvider
{
    public const string ProviderName = "MY_CUSTOM_STORE";

    public override byte[] EncryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] columnEncryptionKey)
    {
        // Logic for encrypting a column encrypted key.
    }
    public override byte[] DecryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] EncryptedColumnEncryptionKey)
    {
        // Logic for decrypting a column encrypted key.
    }
}  
class Program
{
    static void Main(string[] args)
    {
        Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
            new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();
        providers.Add(MyCustomKeyStoreProvider.ProviderName, new MyCustomKeyStoreProvider());
        SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        // ...
    }
}
```

### <a name="using-column-master-key-store-providers-for-programmatic-key-provisioning"></a>Utilisation des fournisseurs de magasin de clés principales de colonne pour la mise en service des clés par programmation

Quand il accède à des colonnes chiffrées, le **Fournisseur de données Microsoft .NET pour SQL Server** localise et appelle de manière transparente le fournisseur de magasin de clés principales de colonne qui convient pour déchiffrer les clés de chiffrement de colonne. En règle générale, un code d’application normal n’appelle pas directement les fournisseurs de magasin de clés principales de colonne. Vous pouvez toutefois instancier et appeler explicitement un fournisseur afin de provisionner et gérer programmatiquement les clés Always Encrypted, dans le but de générer une clé de chiffrement de colonne chiffrée et de déchiffrer une clé de chiffrement de colonne (par exemple, dans le cadre d’une rotation des clés principales de colonne). Pour plus d’informations, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).
L’implémentation de vos propres outils de gestion de clés peut être nécessaire uniquement si vous utilisez un fournisseur de magasin de clés personnalisé. Quand vous utilisez des clés stockées dans des magasins de clés (pour lesquels des fournisseurs intégrés existent) ou dans Azure Key Vault, vous pouvez utiliser les outils existants, tels que SQL Server Management Studio ou PowerShell, pour gérer et mettre en service les clés.
L’exemple ci-dessous illustre la génération d’une clé de chiffrement de colonne et l’utilisation de la [classe SqlColumnEncryptionCertificateStoreProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider) pour chiffrer la clé avec un certificat.

```cs
using System.Security.Cryptography;
static void Main(string[] args)
{
    byte[] EncryptedColumnEncryptionKey = GetEncryptedColumnEncryptonKey();
    Console.WriteLine("0x" + BitConverter.ToString(EncryptedColumnEncryptionKey).Replace("-", ""));
    Console.ReadKey();
}

static byte[]  GetEncryptedColumnEncryptonKey()
{
    int cekLength = 32;
    String certificateStoreLocation = "CurrentUser";
    String certificateThumbprint = "698C7F8E21B2158E9AED4978ADB147CF66574180";
    // Generate the plaintext column encryption key.
    byte[] columnEncryptionKey = new byte[cekLength];
    RNGCryptoServiceProvider rngCsp = new RNGCryptoServiceProvider();
    rngCsp.GetBytes(columnEncryptionKey);

    // Encrypt the column encryption key with a certificate.
    string keyPath = String.Format(@"{0}/My/{1}", certificateStoreLocation, certificateThumbprint);
    SqlColumnEncryptionCertificateStoreProvider provider = new SqlColumnEncryptionCertificateStoreProvider();
    return provider.EncryptColumnEncryptionKey(keyPath, @"RSA_OAEP", columnEncryptionKey);
}
```

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>Contrôle de l’impact d’Always Encrypted sur les performances

Always Encrypted étant une technologie de chiffrement côté client, la surcharge des performances s’observe côté client et non dans la base de données. Outre le coût des opérations de chiffrement et de déchiffrement, les autres sources de surcharge des performances côté client sont les suivantes :

- Allers-retours supplémentaires avec la base de données afin de récupérer des métadonnées pour les paramètres de requête.
- Appels au magasin de clés principales de colonne pour accéder à une clé principale de colonne.

Cette section décrit les optimisations des performances intégrées au **Fournisseur Microsoft .NET pour SQL Server** et comment vous pouvez contrôler l’impact sur les performances des deux facteurs ci-dessus.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Contrôle des allers-retours vers la base de données en vue de la récupération des métadonnées pour les paramètres de requête

Par défaut, si Always Encrypted est activé pour une connexion, le **Fournisseur de données Microsoft .NET pour SQL Server** appelle [sys.sp_describe_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) pour chaque requête paramétrable, en passant l’instruction de requête (sans valeurs de paramètre) à SQL Server. **sys.sp_describe_parameter_encryption** analyse l’instruction de requête pour vérifier si des paramètres doivent être chiffrés, et, si tel est le cas, pour chacun, il renvoie les informations relatives au chiffrement qui autorisent le **Fournisseur de données Microsoft .NET pour SQL Server** à chiffrer des valeurs de paramètres. Ce comportement garantit un haut niveau de transparence à l’application cliente. L’application (et le développeur d’applications) n’ont pas besoin de connaître les requêtes qui accèdent aux colonnes chiffrées, tant que les valeurs ciblant les colonnes chiffrées sont transmises au **Fournisseur de données Microsoft .NET pour SQL Server** dans des objets SqlParameter.

### <a name="query-metadata-caching"></a>Mise en cache des métadonnées de requête

Le **Fournisseur de données Microsoft .NET pour SQL Server** met en cache les résultats de **sys.sp_describe_parameter_encryption** pour chaque instruction de requête. Par conséquent, si la même instruction de requête est exécutée plusieurs fois, le pilote appelle **sys.sp_describe_parameter_encryption** une seule fois. La mise en cache des métadonnées de chiffrement pour les instructions de requête réduit sensiblement le coût en termes de performances de l’extraction des métadonnées à partir de la base de données. La mise en cache est activée par défaut. Vous pouvez désactiver la mise en cache des métadonnées de paramètre en définissant la [propriété SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled) sur false, bien que cela ne soit pas recommandé, sauf dans de très rares cas comme celui décrit ci-dessous :

Supposons une base de données ayant deux schémas différents : `s1` et `s2`. Chaque schéma contient une table portant le même nom : `t`. Les définitions des tables `s1.t` et `s2.t` sont identiques, à l’exception des propriétés relatives au chiffrement : Une colonne nommée`c` dans `s1.t` n’est pas chiffrée, et elle est chiffrée dans `s2.t`. La base de données comporte deux utilisateurs : `u1` et `u2`. Le schéma par défaut des utilisateurs `u1` est `s1`. Le schéma par défaut pour `u2` est `s2`. Une application .NET ouvre deux connexions sur la base de données, en empruntant l’identité de l’utilisateur `u1` sur une connexion et celle de l’utilisateur `u2` sur une autre. L’application envoie une requête avec un paramètre ciblant la colonne `c` sur la connexion pour l’utilisateur `u1` (le schéma de l’utilisateur par défaut est utilisé, dans la mesure où la requête ne spécifie aucun schéma). Ensuite, l’application envoie la même requête sur la connexion pour l’utilisateur `u2`. Si la mise en cache des métadonnées de la requête est activée, après la première requête, le cache sera rempli avec les métadonnées en indiquant que la colonne `c`, ciblée par le paramètre de requête, n’est pas chiffrée. Dans la mesure où la deuxième requête comporte la même instruction de requête, les informations stockées dans le cache seront utilisées. Par conséquent, le pilote envoie la requête sans chiffrer le paramètre (qui est incorrect, car la colonne cible, `s2.t.c`, est chiffrée), entraînant la fuite de la valeur de texte en clair du paramètre vers le serveur. Le serveur détecte cette incompatibilité et force le pilote à actualiser le cache. L’application renvoie alors en toute transparence la requête avec la valeur de paramètre correctement chiffrée. Dans ce cas, la mise en cache doit être désactivée pour empêcher toute fuite de valeurs sensibles vers le serveur.

### <a name="setting-always-encrypted-at-the-query-level"></a>Configuration d’Always Encrypted au niveau de la requête

Pour contrôler l’impact sur les performances de la récupération des métadonnées de chiffrement pour les requêtes paramétrables, vous pouvez activer Always Encrypted pour chaque requête, au lieu de le configurer pour la connexion. De cette façon, **sys.sp_describe_parameter_encryption** est appelé uniquement pour les requêtes dont les paramètres ciblent des colonnes chiffrées. Notez, toutefois, que de cette façon, vous réduisez la transparence du chiffrement. Si vous modifiez les propriétés de chiffrement de vos colonnes de base de données, vous devrez modifier le code de votre application pour l’aligner sur les modifications du schéma.

> [!NOTE]
> La définition de Always Encrypted au niveau de la requête limite le gain de performances de la mise en cache des métadonnées de chiffrement de paramètres.

Pour contrôler le comportement d’Always Encrypted des requêtes, vous devez utiliser ce constructeur de [SqlCommand](/dotnet/api/microsoft.data.sqlclient.sqlcommand) et [SqlCommandColumnEncryptionSetting](/dotnet/api/microsoft.data.sqlclient.sqlcommandcolumnencryptionsetting). Voici quelques conseils utiles :

- Si la plupart des requêtes qu’une application cliente exécute ont accès aux colonnes chiffrées :
  - Définissez le mot clé de la chaîne de connexion **Paramètre de chiffrement de colonne** sur **Activé**.
  - Définissez **SqlCommandColumnEncryptionSetting** sur **Désactivé** pour les requêtes qui n’ont pas accès aux colonnes chiffrées. Ce paramètre désactive à la fois l’appel de **sys.sp_describe_parameter_encryption** et la tentative de déchiffrement des valeurs du jeu de résultats.
  - Définissez **SqlCommandColumnEncryptionSetting** sur **ResultSetOnly** pour les requêtes individuelles qui n’ont aucun paramètre exigeant un chiffrement, mais qui récupèrent des données de colonnes chiffrées. Cela désactive l’appel de **sys.sp_describe_parameter_encryption** et le chiffrement des paramètres. La requête est alors en mesure de déchiffrer les résultats des colonnes de chiffrement.
- Si la plupart des requêtes qu’une application cliente exécute n’ont pas accès aux colonnes chiffrées :
  - Définissez le mot clé de la chaîne de connexion **Paramètre de chiffrement de colonne** sur **Désactivé**.
  - Définissez **SqlCommandColumnEncryptionSetting** sur **Activé** pour les requêtes qui ont des paramètres exigeant un chiffrement. Ce paramètre active à la fois l’appel de **sys.sp_describe_parameter_encryption** et le déchiffrement des résultats de requête récupérés à partir des colonnes chiffrées.
  - Définissez **SqlCommandColumnEncryptionSetting** sur **ResultSetOnly** pour les requêtes qui n’ont aucun paramètre exigeant un chiffrement, mais qui récupèrent des données de colonnes chiffrées. Ce paramètre désactive l’appel de **sys.sp_describe_parameter_encryption** et le chiffrement des paramètres. La requête est alors en mesure de déchiffrer les résultats des colonnes de chiffrement.

Dans l’exemple ci-dessous, Always Encrypted est désactivé pour la connexion de base de données. La requête envoyée par l’application comprend un paramètre qui cible la colonne LastName qui n’est pas chiffrée. La requête récupère les données des colonnes `SSN` et `BirthDate` qui sont toutes deux chiffrées. Dans ce cas, il n’est pas nécessaire d’appeler **sys.sp_describe_parameter_encryption** pour récupérer les métadonnées de chiffrement. Toutefois, le déchiffrement des résultats de requête doit être activé, afin que l’application puisse recevoir des valeurs en texte clair à partir des deux colonnes chiffrées. Le paramètre **SqlCommandColumnEncryptionSetting** défini sur **ResultSetOnly** est utilisé à cette fin.

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
using (SqlConnection connection = new SqlConnection(connectionString))
using (SqlCommand cmd = new SqlCommand(@"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName",
connection, null, SqlCommandColumnEncryptionSetting.ResultSetOnly))
{
    connection.Open();
    SqlParameter paramLastName = cmd.CreateParameter();
    paramLastName.ParameterName = @"@LastName";
    paramLastName.DbType = DbType.String;
    paramLastName.Direction = ParameterDirection.Input;
    paramLastName.Value = "Abel";
    paramLastName.Size = 50;
    cmd.Parameters.Add(paramLastName);
    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
            {
                Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
            }
        }
    }
}
```

### <a name="column-encryption-key-caching"></a>Mise en cache des clés de chiffrement de colonne

Pour réduire le nombre d’appels vers un magasin de clés principales de colonne afin de déchiffrer les clés de chiffrement de colonne, le **Fournisseur de données Microsoft .NET pour SQL Server** met en cache les clés de chiffrement de colonne en texte en clair dans la mémoire. Après avoir reçu la valeur de clé de chiffrement de colonne chiffrée à partir des métadonnées de la base de données, le pilote tente d’abord de trouver la clé de chiffrement de colonne en texte en clair qui correspond à la valeur de clé chiffrée. Le pilote appelle le magasin de clés qui contient la clé principale de colonne uniquement s’il ne peut pas trouver la valeur de la clé de chiffrement de colonne chiffrée dans le cache.

Les entrées du cache sont supprimées après un intervalle de durée de vie configurable pour des raisons de sécurité. La valeur de durée de vie par défaut est de 2 heures. Si vous avez des exigences de sécurité plus strictes concernant la durée pendant laquelle les clés de chiffrement de colonne peuvent être mises en cache en texte clair dans l’application, vous pouvez la modifier à l’aide de la [propriété SqlConnection.ColumnEncryptionKeyCacheTtl](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionkeycachettl).

## <a name="enabling-extra-protection-for-a-compromised-sql-server"></a>Activation d’une protection supplémentaire pour un serveur SQL Server compromis

Par défaut, le **Fournisseur de données Microsoft .NET pour SQL Server** repose sur le système de base de données (SQL Server ou Azure SQL Database) pour fournir des métadonnées indiquant les colonnes de la base de données à chiffrer et de quelle manière. Les métadonnées de chiffrement permettent au **Fournisseur de données Microsoft .NET pour SQL Server** de chiffrer des paramètres de requête et de déchiffrer des résultats de requête sans aucune entrée de l’application, ce qui réduit considérablement le nombre de modifications requises dans l’application. Toutefois, si le processus SQL Server est compromis et qu’un attaquant falsifie les métadonnées envoyées par SQL Server au **Fournisseur de données Microsoft .NET pour SQL Server**, ce dernier peut être en mesure de dérober des informations sensibles. Cette section décrit les API qui fournissent un niveau de protection supplémentaire contre ce type d’attaque, mais au prix d’une réduction de la transparence.

### <a name="forcing-parameter-encryption"></a>Forcer le chiffrement de paramètre

Avant que le **Fournisseur de données Microsoft .NET pour SQL Server** n’envoie une requête paramétrable à SQL Server, celui-ci demande à SQL Server (en appelant [sys.sp_describe_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md)) d’analyser l’instruction de requête et de fournir des informations sur les paramètres de requête à chiffrer. Une instance de SQL Server compromise pourrait induire en erreur le **Fournisseur de données Microsoft .NET pour SQL Server** en envoyant les métadonnées indiquant que le paramètre ne cible pas une colonne chiffrée, en dépit du fait que la colonne est chiffrée dans la base de données. Par conséquent, le **Fournisseur de données Microsoft .NET pour SQL Server** ne chiffrerait pas la valeur du paramètre et l’enverrait en tant que texte en clair à l’instance compromise.

Pour éviter ce type d’attaque, une application peut définir la propriété [SqlParameter.ForceColumnEncryption](/dotnet/api/microsoft.data.sqlclient.sqlparameter.forcecolumnencryption) du paramètre sur true. Avec ce paramètre, le **Fournisseur de données Microsoft .NET pour SQL Server** lève une exception si les métadonnées reçues du serveur indiquent que le paramètre n’a pas besoin d’être chiffré.

Bien que l’utilisation de la **propriété SqlParameter.ForceColumnEncryption** contribue à durcir la sécurité, elle réduit également la transparence du chiffrement sur l’application cliente. Si vous mettez à jour le schéma de base de données pour modifier le jeu de colonnes chiffrées, vous devrez apporter des modifications à l’application.

L’exemple de code suivant illustre l’utilisation de la **propriété SqlParameter.ForceColumnEncryption** afin d’éviter l’envoi de numéros de sécurité sociale sous forme de texte en clair vers la base de données.

```cs
using (SqlCommand cmd = _sqlconn.CreateCommand())
{
    // Use parameterized queries to access Always Encrypted data.

    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = @SSN;";

    SqlParameter paramSSN = cmd.CreateParameter();
    paramSSN.ParameterName = @"@SSN";
    paramSSN.DbType = DbType.AnsiStringFixedLength;
    paramSSN.Direction = ParameterDirection.Input;
    paramSSN.Value = ssn;
    paramSSN.Size = 11;
    paramSSN.ForceColumnEncryption = true;
    cmd.Parameters.Add(paramSSN);

    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        // Do something.
    }
}
```

### <a name="configuring-trusted-column-master-key-paths"></a>Configuration des chemins d’accès des clés principales de colonne approuvés

Les métadonnées de chiffrement renvoyées par SQL Server pour les paramètres de requête ciblant des colonnes chiffrées et pour les résultats récupérés à partir de colonnes de chiffrement, incluent le chemin d’accès des clés principales de colonne qui identifie le magasin de clés et l’emplacement de la clé dans ce dernier. Si l’instance est compromise, celle-ci pourrait envoyer le chemin d’accès des clés dirigeant le **Fournisseur de données Microsoft .NET pour SQL Server** vers l’emplacement contrôlé par un attaquant. Ce processus peut entraîner une fuite des informations d’identification du magasin de clés, quand le magasin de clés exige l’authentification de l’application.

Pour empêcher ces attaques, l’application peut spécifier la liste des chemins d’accès des clés approuvés pour un serveur donné en utilisant la [propriété SqlConnection.ColumnEncryptionTrustedMasterKeyPaths](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths). Si le **Fournisseur de données Microsoft .NET pour SQL Server** reçoit un chemin d’accès des clés ne figurant pas dans la liste des chemins d’accès de clé approuvés, il lève une exception.

Bien que la définition de chemins de clé approuvés améliore la sécurité de votre application, vous devez changer le code ou/et la configuration de l’application chaque fois que vous permutez votre clé principale de colonne (à chaque changement du chemin de la clé principale de colonne).

L’exemple suivant montre comment configurer les chemins d’accès des clés principales de colonne approuvés :

```cs
// Configure trusted key paths to protect against fake key paths sent by a compromised SQL Server instance
// First, create a list of trusted key paths for your server
List<string> trustedKeyPathList = new List<string>();
trustedKeyPathList.Add("CurrentUser/my/425CFBB9DDDD081BB0061534CE6AB06CB5283F5Ea");

// Register the trusted key path list for your server
SqlConnection.ColumnEncryptionTrustedMasterKeyPaths.Add(serverName, trustedKeyPathList);

```

## <a name="copying-encrypted-data-using-sqlbulkcopy"></a>Copie de données chiffrées à l’aide de SqlBulkCopy

Grâce à SqlBulkCopy, les données qui sont déjà chiffrées et stockées dans une table peuvent être copiées vers une autre table, sans que vous ayez à les déchiffrer. Pour ce faire :

- Vérifiez que la configuration du chiffrement de la table cible est identique à celle de la table source. Les deux tables doivent avoir les mêmes colonnes chiffrées, et ces colonnes doivent être chiffrées à l’aide des mêmes types et des mêmes clés de chiffrement. Si les colonnes cibles sont chiffrées différemment de la colonne source correspondante, vous ne pourrez pas déchiffrer les données de la table cible après les avoir copiées. Les données seront endommagées.
- Configurez les deux connexions de base de données, c’est-à-dire celle vers la table source et celle vers la table cible, sans activer Always Encrypted.
- Définissez l’option `AllowEncryptedValueModifications` (consultez [SqlBulkCopyOptions](/dotnet/api/microsoft.data.sqlclient.sqlbulkcopyoptions)).

> [!NOTE]
> Utilisez `AllowEncryptedValueModifications` avec précaution. Ce paramètre expose la base de données à un risque d’endommagement, car le **Fournisseur de données Microsoft .NET pour SQL Server** ne vérifie pas que les données sont chiffrées ni qu’elles sont correctement chiffrées avec le même type de chiffrement, le même algorithme et la même clé que ceux de la colonne cible.

Voici un exemple qui copie les données d’une table à une autre. Les colonnes `SSN` et `BirthDate` sont censées être chiffrées.

```cs
static public void CopyTablesUsingBulk(string sourceTable, string targetTable)
{
    string sourceConnectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
    string targetConnectionString = "Data Source=server64; Initial Catalog=Clinic; Integrated Security=true";
    using (SqlConnection connSource = new SqlConnection(sourceConnectionString))
    {
        connSource.Open();
        using (SqlCommand cmd = new SqlCommand(string.Format("SELECT [PatientID], [SSN], [FirstName], [LastName], [BirthDate] FROM {0}", sourceTable), connSource))
        {
            using (SqlDataReader reader = cmd.ExecuteReader())
            using (SqlBulkCopy copy = new SqlBulkCopy(targetConnectionString, SqlBulkCopyOptions.KeepIdentity | SqlBulkCopyOptions.AllowEncryptedValueModifications))
            {
                copy.EnableStreaming = true;
                copy.DestinationTableName = targetTable;
                copy.WriteToServer(reader);
            }
        }
    }
}
```

## <a name="always-encrypted-api-reference"></a>Référence de l’API Chiffrement intégral

**Espace de noms :** [Microsoft.Data.SqlClient](/dotnet/api/microsoft.data.sqlclient)

**Assembly :** Microsoft.Data.SqlClient.dll

|Nom|Description|
|:---|:---|
|[Classe SqlColumnEncryptionCertificateStoreProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider)|Un fournisseur de magasin de clés pour le magasin de certificats Windows.|
|[Classe SqlColumnEncryptionCngProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncngprovider)|Un fournisseur de magasin de clés pour l’API de chiffrement Microsoft : de la prochaine génération (CNG).|
|[Classe SqlColumnEncryptionCspProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncspprovider)|Un fournisseur de magasin de clés pour les fournisseurs de services de chiffrement (CSP) basés sur la CAPI Microsoft.|
|[classe SqlColumnEncryptionKeyStoreProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptionkeystoreprovider)|Classe de base des fournisseurs de magasins de clés.|
|[Énumération SqlCommandColumnEncryptionSetting](/dotnet/api/microsoft.data.sqlclient.sqlcommandcolumnencryptionsetting)|Paramètres pour contrôler le comportement d’Always Encrypted pour des requêtes individuelles.|
|[Énumération SqlConnectionAttestationProtocol](/dotnet/api/microsoft.data.sqlclient.sqlconnectionattestationprotocol)|Spécifie une valeur pour le protocole d’attestation lors de l’utilisation d’Always Encrypted avec des enclaves sécurisées|
|[Énumération SqlConnectionColumnEncryptionSetting](/dotnet/api/microsoft.data.sqlclient.sqlconnectioncolumnencryptionsetting)|Paramètres pour activer le chiffrement et le déchiffrement d’une connexion de base de données.|
|[Propriété SqlConnectionStringBuilder.ColumnEncryptionSetting](/dotnet/api/microsoft.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting)|Obtient et définit Always Encrypted dans la chaîne de connexion.|
|[propriété SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled)|Active et désactive la mise en cache des métadonnées de requête de chiffrement.|
|[SqlConnection.ColumnEncryptionKeyCacheTtl](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionkeycachettl)|Obtient et définit la durée de vie des entrées du cache de clé de chiffrement de colonne.|
|[SqlConnection.ColumnEncryptionTrustedMasterKeyPaths](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths)|Vous permet de définir une liste de chemins d’accès de clés approuvés pour un serveur de bases de données. Si le pilote reçoit un chemin de clé qui ne figure pas dans la liste pendant le traitement d’une requête d’application, celle-ci échouera. Cette propriété offre une protection supplémentaire contre les attaques au niveau de la sécurité durant lesquelles une machine SQL Server compromise fournit de faux chemins de clés, ce qui peut donner lieu à une fuite des informations d’identification du magasin de clés.|
|[Méthode SqlConnection.RegisterColumnEncryptionKeyStoreProviders](/dotnet/api/microsoft.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders)|Vous permet d’inscrire des fournisseurs de magasins de clés personnalisés. Il s’agit d’un dictionnaire qui mappe des noms de fournisseurs de magasins de clés à des implémentations de fournisseurs de magasins de clés.|
|[Constructeur SqlCommand (String, SqlConnection, SqlTransaction, SqlCommandColumnEncryptionSetting)](/dotnet/api/microsoft.data.sqlclient.sqlcommand.-ctor?view=sqlclient-dotnet-core-1.0&preserve-view=true#Microsoft_Data_SqlClient_SqlCommand__ctor_System_String_Microsoft_Data_SqlClient_SqlConnection_Microsoft_Data_SqlClient_SqlTransaction_Microsoft_Data_SqlClient_SqlCommandColumnEncryptionSetting_)|Permet de contrôler le comportement d’Always Encrypted pour des requêtes individuelles.|
|[SqlParameter.ForceColumnEncryption](/dotnet/api/microsoft.data.sqlclient.sqlparameter.forcecolumnencryption)|Applique le chiffrement d’un paramètre. Si SQL Server informe le pilote que le paramètre ne nécessite pas de chiffrement, la requête utilisant le paramètre échoue. Cette propriété offre une protection supplémentaire contre les attaques au niveau de la sécurité durant lesquelles une machine SQL Server compromise fournit des métadonnées de chiffrement incorrectes au client, ce qui peut entraîner la divulgation de données.|
|[Mot clé](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring) de la chaîne de connexion : `Column Encryption Setting=enabled`|Active ou désactive la fonctionnalité Always Encrypted pour la connexion.|

## <a name="see-also"></a>Voir aussi

- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted avec enclaves sécurisées](../../../relational-databases/security/encryption/always-encrypted-enclaves.md)
- [Didacticiel de SQL Database : Protéger les données à l’aide d’Always Encrypted](/azure/azure-sql/database/always-encrypted-certificate-store-configure)
- [Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Exemple : Azure Key Vault fonctionnant avec Always Encrypted](azure-key-vault-example.md)
- [Exemple : Utilisation d’Azure Key Vault avec Always Encrypted avec enclaves sécurisées](azure-key-vault-enclave-example.md)
