---
title: Always Encrypted avec le fournisseur de données .NET Framework
description: Découvrez comment développer des applications .NET à l’aide de la fonctionnalité Always Encrypted pour SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: security, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 827e509e-3c4f-4820-aa37-cebf0f7bbf80
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 26b598865668ce5fb10e34eb9558fe018a7c60a8
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075891"
---
# <a name="using-always-encrypted-with-the-net-framework-data-provider-for-sql-server"></a>Utilisation d’Always Encrypted avec le fournisseur de données .NET Framework pour SQL Server
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Cet article fournit des informations sur le développement d’applications .NET à l’aide d’[Always Encrypted](always-encrypted-database-engine.md) ou [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md) et du [fournisseur de données .NET Framework pour SQL Server](/dotnet/framework/data/adonet/sql/).

Always Encrypted permet aux applications clientes de chiffrer des données sensibles et de ne jamais révéler les données ou les clés de chiffrement à SQL Server ou Azure SQL Database. À cette fin, un pilote avec Always Encrypted, tel que le fournisseur de données .NET Framework, chiffre et déchiffre de manière transparente les données sensibles dans l’application cliente. Le pilote détermine automatiquement les paramètres de requêtes qui correspondent aux colonnes de base de données sensibles (protégées avec Always Encrypted) et chiffre les valeurs de ces paramètres avant de transmettre les données à SQL Server ou Azure SQL Database. De même, il déchiffre de manière transparente les données récupérées dans les colonnes de base de données chiffrées, qui figurent dans les résultats de la requête. Pour plus d’informations, consultez [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md).

## <a name="prerequisites"></a>Prérequis

- Configurez Always Encrypted dans votre base de données. Pour cela, vous devez mettre en service des clés Always Encrypted et configurer le chiffrement pour les colonnes de base de données sélectionnées. Si vous n’avez pas déjà une base de données configurée avec Always Encrypted, suivez les instructions de [Bien démarrer avec Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted).
- Si vous utilisez Always Encrypted avec enclaves sécurisées, consultez [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md) pour connaître les prérequis supplémentaires.
- Vérifiez que .NET Framework version 4.6.1 ou ultérieure est installé sur votre machine de développement. Pour plus d’informations, consultez [.NET Framework 4.6](/dotnet/framework/). Vous devez également vérifier que .NET Framework version 4.6 ou ultérieure est configuré comme version cible de .NET Framework dans votre environnement de développement. Si vous utilisez Visual Studio, consultez [Guide pratique pour cibler une version de .NET Framework](/visualstudio/ide/how-to-target-a-version-of-the-dotnet-framework). 

> [!NOTE]
> Le niveau de prise en charge d’Always Encrypted varie selon la version du .NET Framework. Pour plus d’informations, consultez la section « Référence de l’API Always Encrypted » ci-dessous.

## <a name="enabling-always-encrypted-for-application-queries"></a>Activation d’Always Encrypted pour les requêtes d’application
Le moyen le plus simple d’activer le chiffrement des paramètres et le déchiffrement des résultats de requête ciblant des colonnes chiffrées, est de définir la valeur du mot clé de chaîne de connexion du paramètre de chiffrement de colonne sur **Activé**.

Voici un exemple de chaîne de connexion activant Always Encrypted :

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
SqlConnection connection = new SqlConnection(connectionString);
```

L’exemple suivant est équivalent et utilise la propriété SqlConnectionStringBuilder.ColumnEncryptionSetting.

```cs
SqlConnectionStringBuilder strbldr = new SqlConnectionStringBuilder();
strbldr.DataSource = "server63";
strbldr.InitialCatalog = "Clinic";
strbldr.IntegratedSecurity = true;
strbldr.ColumnEncryptionSetting = SqlConnectionColumnEncryptionSetting.Enabled;
SqlConnection connection = new SqlConnection(strbldr.ConnectionString);
```

Always Encrypted peut également être activé pour les requêtes individuelles. Consultez la section **Contrôle de l’impact d’Always Encrypted sur les performances** ci-dessous.
L’activation d’Always Encrypted ne suffit pas à la réussite du chiffrement ou du déchiffrement. Vous devez également vérifier ce qui suit :
- L’application dispose des autorisations de base de données *VIEW ANY COLUMN MASTER KEY DEFINITION* et *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* qui sont nécessaires pour accéder aux métadonnées des clés Always Encrypted dans la base de données. Pour plus d’informations, consultez la [section « Autorisations » de la rubrique « Always Encrypted (moteur de base de données) »](./always-encrypted-database-engine.md#database-permissions).
- L’application peut accéder à la clé principale de colonne qui protège les clés de chiffrement de colonne, en chiffrant les colonnes de base de données interrogées.

## <a name="enabling-always-encrypted-with-secure-enclaves"></a>Activation d’Always Encrypted avec les enclaves sécurisées

À partir de .NET Framework version 4.7.2, le pilote prend en charge [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md). 

Pour obtenir des informations générales sur le rôle du pilote client dans les calculs d’enclave et l’attestation d’enclave, consultez [Développer des applications à l’aide d’Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md). 

Pour configurer votre application :

1. Activez Always Encrypted pour les requêtes de votre application, comme expliqué dans la section précédente.
2. Intégrez le package NuGet [Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders) à votre application. NuGet est une bibliothèque de fournisseurs d’enclave qui implémente la logique côté client pour des protocoles d’attestation et pour établir un canal sécurisé avec une enclave sécurisée.  
3. Mettez à jour la configuration de votre application (par exemple, dans web.config ou app.config) de manière à définir le mappage entre un type d’enclave configuré pour votre base de données et un fournisseur d’enclave. 
    1. Si vous utilisez [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH), vous devrez mapper le type d’enclave VBS à la classe Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider du package NuGet.
    2. Si vous utilisez [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] et Microsoft Azure Attestation, vous devrez mapper le type d’enclave SGX à la classe Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider du package NuGet.

    Pour obtenir des instructions détaillées expliquant comment modifier la configuration de votre application, consultez [Tutoriel : Développer des applications à l’aide d’Always Encrypted et d’enclaves sécurisées](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md).

4. Configurez le mot clé `Enclave Attestation URL` de votre chaîne de connexion de base de données sur une URL d’attestation (point de terminaison de service d’attestation). Vous devez obtenir une URL d’attestation pour votre environnement auprès de votre administrateur de service d’attestation.
   1. Si vous utilisez [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH), consultez [Déterminer et partager l’URL d’attestation SGH](always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
   2. Si vous utilisez [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] et Microsoft Azure Attestation, consultez [Déterminer l’URL d’attestation de votre de stratégie d’attestation](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation).

Pour obtenir un tutoriel pas à pas, consultez [Tutoriel : Développer une application .NET Framework avec Always Encrypted avec enclaves sécurisées](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Récupération et modification des données dans des colonnes chiffrées

Lorsque vous activez Always Encrypted pour les requêtes d’application, vous pouvez utiliser les API ADO.NET standard (voir [Extraction et modification de données dans ADO.NET](/dotnet/framework/data/adonet/retrieving-and-modifying-data)) ou les API du [Fournisseur de données .NET Framework pour SQL Server](/dotnet/framework/data/adonet/sql/) qui sont définies dans [System.Data.SqlClient Namespace](/dotnet/api/system.data.sqlclient), pour récupérer ou modifier les données de colonnes de base de données chiffrées. Si votre application dispose des autorisations de base de données nécessaires et qu’elle peut accéder à la clé principale de colonne, le fournisseur de données .NET Framework pour SQL Server chiffre tous les paramètres de requête qui ciblent des colonnes chiffrées et déchiffre les données extraites des colonnes chiffrées en retournant des valeurs de types .NET en texte clair, qui correspondent aux types de données SQL Server définis pour les colonnes du schéma de base de données.
Si Always Encrypted n’est pas activé, les requêtes ayant des paramètres qui ciblent des colonnes chiffrées échouent. Une requête peut toujours récupérer des données à partir de colonnes chiffrées, tant qu’aucun de ses paramètres ne cible des colonnes chiffrées. Toutefois, le fournisseur de données .NET Framework pour SQL Server ne tente pas de déchiffrer les valeurs extraites des colonnes chiffrées et l’application reçoit des données chiffrées binaires (sous la forme de tableaux d’octets).

Le tableau ci-dessous récapitule le comportement des requêtes, selon qu’Always Encrypted est activé ou non :

|Caractéristique de la requête | Always Encrypted est activé et l’application peut accéder aux clés et à leurs métadonnées|Always Encrypted est activé et l’application ne peut pas accéder aux clés et à leurs métadonnées | Always Encrypted est désactivé|
|:---|:---|:---|:---|
| Requêtes avec des paramètres ciblant des colonnes chiffrées. | Des valeurs de paramètres sont chiffrées en toute transparence. | Error | Error|
| Requêtes qui récupèrent des données à partir de colonnes chiffrées, sans paramètres ciblant des colonnes chiffrées.| Les résultats de colonnes chiffrées sont déchiffrés de manière transparente. L’application reçoit des valeurs en texte clair des types de données .NET correspondant aux types SQL Server configurés pour les colonnes chiffrées. | Error | Les résultats des colonnes chiffrées ne sont pas déchiffrés. L’application reçoit des valeurs chiffrées sous la forme de tableaux d’octets (byte[]). 

Les exemples suivants illustrent la récupération et la modification de données dans des colonnes chiffrées. Ces exemples impliquent une table cible avec le schéma ci-dessous. Les colonnes SSN et BirthDate sont chiffrées.


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
- L’exemple de code ne contient aucun élément spécifique au chiffrement. Le fournisseur de données .NET Framework pour SQL Server détecte et chiffre automatiquement les paramètres *paramSSN* et *paramBirthdate* qui ciblent des colonnes chiffrées. Le chiffrement est donc transparent pour l’application. 
- Les valeurs insérées dans les colonnes de base de données, y compris les colonnes chiffrées, sont passées en tant qu’objets [SqlParameter](/dotnet/api/system.data.sqlclient.sqlparameter) . L’utilisation de **SqlParameter** est facultative pour l’envoi de valeurs aux colonnes non chiffrées (même si elle est vivement recommandée, car elle contribue à empêcher l’injection SQL), mais elle est nécessaire pour les valeurs qui ciblent des colonnes chiffrées. Si les valeurs insérées dans les colonnes SSN ou BirthDate ont été passées en tant que littéraux incorporés dans l’instruction de requête, la requête échouera, car le fournisseur de données .NET Framework pour SQL Server ne sera pas en mesure de déterminer les valeurs des colonnes chiffrées cibles et ne chiffrera donc pas les valeurs. Par conséquent, le serveur les rejettera en les considérant comme incompatibles avec les colonnes chiffrées.
- Le type de données du paramètre ciblant la colonne SSN est défini sur une chaîne ANSI (non Unicode) qui est mappée vers le type de données SQL Server char/varchar. Si le type du paramètre est défini sur une chaîne Unicode (String) mappée à nchar/nvarchar, la requête échoue, car Always Encrypted ne prend pas en charge les conversions de valeurs nchar/nvarchar chiffrées en valeurs char/varchar chiffrées. Pour plus d’informations sur les mappages de type de données, consultez [Mappages de type de données SQL Server](/dotnet/framework/data/adonet/sql-server-data-type-mappings) .
- Le type de données du paramètre inséré dans la colonne BirthDate est explicitement défini sur le type de données SQL Server cible à l’aide de la [propriété SqlParameter.SqlDbType](/dotnet/api/system.data.sqlclient.sqlparameter.sqldbtype), au lieu d’utiliser le mappage implicite des types .NET vers les types de données SQL Server appliqués lors de l’utilisation de la [propriété SqlParameter.DbType](/dotnet/api/system.data.sqlclient.sqlparameter.dbtype). Par défaut, la [structure DateTime](/dotnet/api/system.datetime) est mappée vers le type de données SQL Server datetime. Étant donné que les données de la colonne BirthDate sont de type « date » et qu’Always Encrypted ne prend pas en charge la conversion de valeurs « datetime » chiffrées en valeurs « date » chiffrées, l’utilisation du mappage par défaut entraînerait une erreur. 

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
using (SqlConnection connection = new SqlConnection(strbldr.ConnectionString))
{
   using (SqlCommand cmd = connection.CreateCommand())
   {
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
}
```

### <a name="retrieving-plaintext-data-example"></a>Exemple de récupération de données en texte clair

L’exemple suivant montre le filtrage de données basé sur des valeurs chiffrées, ainsi que la récupération de données en texte clair à partir de colonnes chiffrées. Notez les points suivants :
- La valeur utilisée dans la clause WHERE pour filtrer la colonne SSN doit être passée à l’aide de SqlParameter, afin que le fournisseur de données .NET Framework pour SQL Server puisse la chiffrer de manière transparente avant de l’envoyer à la base de données.
- Toutes les valeurs sont imprimées par le programme sous la forme de texte en clair, car le fournisseur de données .NET Framework pour SQL Server déchiffre de manière transparente les données récupérées à partir des colonnes SSN et BirthDate.


> [!NOTE]
> Les requêtes peuvent effectuer des comparaisons d’égalité sur des colonnes si elles sont chiffrées à l’aide du chiffrement déterministe. Pour plus d’informations, consultez [Sélection d’un chiffrement déterministe ou aléatoire](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
    
using (SqlConnection connection = new SqlConnection(strbldr.ConnectionString))
 {
    using (SqlCommand cmd = connection.CreateCommand())
 {

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
```

### <a name="retrieving-encrypted-data-example"></a>Exemple de récupération de données chiffrées

Si Always Encrypted n’est pas activé, une requête peut toujours récupérer des données à partir de colonnes chiffrées, tant qu’aucun de ses paramètres ne ciblent des colonnes chiffrées.

L’exemple suivant montre comment récupérer des données chiffrées binaires à partir de colonnes chiffrées. Notez les points suivants :

- Étant donné qu’Always Encrypted n’est pas toujours activé dans la chaîne de connexion, la requête retourne des valeurs SSN et BirthDate chiffrées sous la forme de tableaux d’octets (le programme convertit les valeurs en chaînes).
- Une requête qui récupère des données à partir de colonnes chiffrées lorsqu’Always Encrypted est désactivé peut avoir des paramètres, tant qu’aucun d’eux ne cible une colonne chiffrée. La requête ci-dessus filtre par nom (LastName), qui n’est pas chiffré dans la base de données. Si la requête filtre par SSN ou BirthDate, la requête échoue.


```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
                
using (SqlConnection connection = new SqlConnection(connectionString))
{
   connection.Open();
   using (SqlCommand cmd = connection.CreateCommand())
   {
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
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Éviter les problèmes courants lors de l’interrogation de colonnes chiffrées

Cette section décrit des catégories d’erreurs courantes liées à l’interrogation des colonnes chiffrées à partir d’applications .NET et fournit des conseils sur la façon de les éviter.

### <a name="unsupported-data-type-conversion-errors"></a>Erreurs liées à la conversion de types de données non pris en charge

Always Encrypted ne prend en charge que peu de conversions de types de données chiffrées. Consultez [Always Encrypted](always-encrypted-database-engine.md) pour obtenir la liste détaillée des conversions de types prises en charge. Procédez comme suit pour éviter les erreurs de conversion de type de données :

- Définissez les types de paramètres ciblant les colonnes chiffrées pour que le type de données SQL Server du paramètre soit exactement le même que le type de la colonne cible, ou pour que soit prise en charge la conversion du type de données SQL Server du paramètre vers le type cible de la colonne. Vous pouvez appliquer le mappage des types de données .NET vers des types de données SQL Server spécifiques à l’aide de la propriété SqlParameter.SqlDbType.
- Vérifiez que la précision et l’échelle des paramètres ciblant les colonnes des types de données SQL Server decimal et numeric sont les mêmes que celles configurées pour la colonne cible.  
- Vérifiez que la précision des paramètres ciblant des colonnes des types de données SQL Server datetime2, datetimeoffset ou time n’est pas supérieure à celle de la colonne cible dans les requêtes qui modifient les valeurs de la colonne cible.  

### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Erreurs dues au passage de texte en clair au lieu de valeurs chiffrées

Les valeurs qui ciblent une colonne chiffrée doivent être chiffrées dans l’application. Toute tentative d’insertion, de modification ou de filtrage par une valeur en texte clair dans une colonne chiffrée entraîne une erreur similaire à celle-ci :


```cs
System.Data.SqlClient.SqlException (0x80131904): Operand type clash: varchar is incompatible with varchar(8000) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', column_encryption_key_database_name = 'Clinic') collation_name = 'SQL_Latin1_General_CP1_CI_AS'
```

Pour éviter ces erreurs, procédez comme suit :
- Activez Always Encrypted pour les requêtes d’application ciblant des colonnes chiffrées (pour la chaîne de connexion ou dans l’objet [SqlCommand](/dotnet/api/system.data.sqlclient.sqlcommand) pour une requête spécifique).
- Utilisez SqlParameter pour envoyer des données ciblant des colonnes chiffrées. L’exemple suivant illustre une requête qui filtre incorrectement une colonne chiffrée (SSN) à l’aide d’un littéral (ou d’une constante), au lieu de passer le littéral à l’intérieur d’un objet SqlParameter. 


```cs
using (SqlCommand cmd = connection.CreateCommand())
{
   cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
cmd.ExecuteNonQuery();
}
```

## <a name="working-with-column-master-key-stores"></a>Utilisation de magasins de clés principales de colonne

Pour chiffrer une valeur de paramètre ou déchiffrer les données des résultats de requête, le fournisseur de données .NET Framework pour SQL Server doit obtenir une clé de chiffrement de colonne qui soit configurée pour la colonne cible. Les clés de chiffrement de colonne sont stockées sous forme chiffrée dans les métadonnées de la base de données. Chaque clé de chiffrement de colonne a une clé principale de colonne correspondante qui a été utilisée pour chiffrer la clé de chiffrement de colonne. Les métadonnées de la base de données ne stockent pas les clés principales de colonne. Elles contiennent uniquement des informations sur un magasin de clés contenant une clé principale de colonne et sur l’emplacement de cette clé dans le magasin.

Pour obtenir la valeur en texte clair d’une clé de chiffrement de colonne, le fournisseur de données .NET Framework pour SQL Server doit d’abord obtenir les métadonnées relatives à la clé de chiffrement de colonne et à la clé principale de colonne correspondante. Il doit ensuite utiliser les informations des métadonnées pour contacter le magasin de clés qui contient la clé principale de colonne et pour déchiffrer la clé de chiffrement de colonne chiffrée. Le fournisseur de données .NET Framework pour SQL Server communique avec un magasin de clés à l’aide d’un fournisseur de magasin de clés principales de colonne, qui est une instance d’une classe dérivée de la [classe SqlColumnEncryptionKeyStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptionkeystoreprovider).


Le processus pour obtenir une clé de chiffrement de colonne est le suivant :

1.  Si Always Encrypted est activé pour une requête, le fournisseur de données .NET Framework pour SQL Server appelle en toute transparence **sys.sp_describe_parameter_encryption** pour récupérer les métadonnées de chiffrement pour les paramètres ciblant les colonnes chiffrées, si la requête possède des paramètres. Pour les données chiffrées contenues dans les résultats d’une requête, SQL Server affecte automatiquement des métadonnées de chiffrement. Les informations sur la clé principale de colonne sont les suivantes :
    - Le nom d’un fournisseur de magasins de clés qui encapsule un magasin de clés contenant la clé principale de colonne. 
    - Le chemin d’accès à une clé qui spécifie l’emplacement de la clé CMK dans le magasin de clés.
    
    Les informations sur la clé de chiffrement de colonne sont les suivantes :

    - La valeur chiffrée d’une clé de chiffrement de colonne.
    - Le nom de l’algorithme utilisé pour chiffrer la clé CEK.
2.  Le fournisseur de données .NET Framework pour SQL Server utilise le nom du magasin de clés principales de colonne (une instance d’une classe dérivée de la classe SqlColumnEncryptionKeyStoreProvider) dans une structure de données interne.
3.  Pour déchiffrer la clé de chiffrement de colonne, le fournisseur de données .NET Framework pour SQL Server appelle la méthode SqlColumnEncryptionKeyStoreProvider.DecryptColumnEncryptionKey, en passant le chemin d’accès de la clé principale de colonne, la valeur chiffrée de la clé de chiffrement de colonne et le nom de l’algorithme de chiffrement utilisé pour générer la clé de chiffrement de la colonne chiffrée.




### <a name="using-built-in-column-master-key-store-providers"></a>Utilisation des fournisseurs de magasin de clés principales de colonne intégrés

Le fournisseur de données .NET Framework pour SQL Server est fourni avec les fournisseurs de magasin de clés principales de colonne intégrés suivants, qui sont pré-inscrits avec des noms de fournisseurs spécifiques (utilisés pour leur recherche).


| Classe | Description | Nom de fournisseur (pour la recherche) |
|:---|:---|:---|
|Classe SqlColumnEncryptionCertificateStoreProvider| Fournisseur du magasin de certificats Windows. | MSSQL_CERTIFICATE_STORE |
|[Classe SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider) <br><br>**Remarque :** Ce fournisseur est disponible dans .NET Framework 4.6.1 et versions ultérieures. |Fournisseur de magasin de clés pour l’ [API de chiffrement Microsoft : API de la prochaine génération (CNG)](/windows/win32/seccng/cng-portal). En général, ce type de magasin est un module de sécurité matériel, c’est-à-dire un périphérique physique qui protège et gère les clés numériques et fournit un traitement du chiffrement.  | MSSQL_CNG_STORE|
| [Classe SqlColumnEncryptionCspProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncspprovider)<br><br>**Remarque :** Ce fournisseur est disponible dans .NET Framework 4.6.1 et versions ultérieures.| Fournisseur de magasin de clés pour l’ [API de chiffrement Microsoft (CAPI)](/previous-versions/visualstudio/aa266944(v=vs.60)). En général, ce type de magasin est un module de sécurité matériel, c’est-à-dire un périphérique physique qui protège et gère les clés numériques et fournit un traitement du chiffrement.| MSSQL_CSP_PROVIDER |
  
Vous n’avez pas besoin d’apporter des modifications au code de l’application pour utiliser ces fournisseurs, toutefois, notez les points suivants :

- Vous (ou votre administrateur de base de données) devez vérifier que le nom du fournisseur (configuré dans les métadonnées de clé principale de colonne) est correct et que le chemin de la clé principale de colonne est valide pour un fournisseur donné. Il est recommandé de configurer les clés à l’aide d’outils tels que SQL Server Management Studio, qui génère automatiquement des noms de fournisseurs et des chemins de clés valides lors de l’émission de l’instruction [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) . Pour plus d’informations, consultez [Configurer Always Encrypted à l’aide de SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md) et [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).
- Vérifiez que votre application peut accéder à la clé dans le magasin de clés. Pour cela, vous devrez peut-être accorder à votre application l’accès à la clé ou au magasin de clés (selon le magasin de clés) ou effectuer d’autres étapes de configuration spécifiques au magasin de clés. Par exemple, pour accéder à un magasin de clés qui implémente CNG ou CAPI (tel qu’un module de sécurité matériel), vous devez vérifier qu’une bibliothèque implémentant CNG ou CAPI pour votre magasin est installée sur l’ordinateur de votre application. Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne pour Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

### <a name="using-azure-key-vault-provider"></a>Utilisation du fournisseur Azure Key Vault

Azure Key Vault est un outil est très pratique qui permet de stocker et de gérer des clés principales de colonne Always Encrypted, en particulier si vos applications sont hébergées dans Azure. Le fournisseur de données .NET Framework pour SQL Server ne comprend pas de fournisseur de magasin de clés principales de colonne intégré pour Azure Key Vault. Toutefois, celui-ci est disponible sous la forme d’un package NuGet que vous pouvez facilement intégrer à votre application. Pour plus d’informations, consultez [Always Encrypted - Protéger les données sensibles de la base de données SQL à l’aide du chiffrement de base de données et stocker vos clés de chiffrement dans Azure Key Vault](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure).

### <a name="implementing-a-custom-column-master-key-store-provider"></a>Implémentation d’un fournisseur de magasin de clés principales de colonne personnalisé

Si vous voulez stocker des clés principales de colonne dans un magasin de clés qui n’est pas pris en charge par un fournisseur existant, vous pouvez implémenter un fournisseur personnalisé en étendant la [classe SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider) et en inscrivant le fournisseur à l’aide de la méthode [SqlConnection.RegisterColumnEncryptionKeyStoreProviders](/dotnet/api/system.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders) .


```cs
public class MyCustomKeyStoreProvider : SqlColumnEncryptionKeyStoreProvider
    {
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
            Dictionary\<string, SqlColumnEncryptionKeyStoreProvider> providers =
               new Dictionary\<string, SqlColumnEncryptionKeyStoreProvider>();
            providers.Add("MY_CUSTOM_STORE", customProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
            providers.Add(SqlColumnEncryptionCertificateStoreProvider.ProviderName, customProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers); 
       // ...
        }

    }
```
 
### <a name="using-column-master-key-store-providers-for-programmatic-key-provisioning"></a>Utilisation des fournisseurs de magasin de clés principales de colonne pour la mise en service des clés par programmation

Quand il accède à des colonnes chiffrées, le fournisseur de données .NET Framework pour SQL Server localise et appelle de manière transparente le fournisseur de magasin de clés principales de colonne qui convient pour déchiffrer les clés de chiffrement de colonne. En règle générale, un code d’application normal n’appelle pas directement les fournisseurs de magasin de clés principales de colonne. Vous pouvez, toutefois, instancier et appeler explicitement un fournisseur afin de mettre en service et de gérer par programmation les clés Always Encrypted, et ce, dans le but de générer une clé de chiffrement de colonne chiffrée et de déchiffrer une clé de chiffrement de colonne (par exemple, dans le cadre d’une permutation de clé principale de colonne). Pour plus d’informations, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).
L’implémentation de vos propres outils de gestion de clés peut être nécessaire uniquement si vous utilisez un fournisseur de magasin de clés personnalisé. Quand vous utilisez des clés stockées dans des magasins de clés (pour lesquels des fournisseurs intégrés existent) ou dans Azure Key Vault, vous pouvez utiliser les outils existants, tels que SQL Server Management Studio ou PowerShell, pour gérer et mettre en service les clés.
L’exemple ci-dessous illustre la génération d’une clé de chiffrement de colonne et l’utilisation de la [classe SqlColumnEncryptionCertificateStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider) pour chiffrer la clé avec un certificat.


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


## <a name="controlling-performance-impact-of-always-encrypted"></a>Contrôle de l’impact d’Always Encrypted sur les performances

Étant donné qu’Always Encrypted est une technologie côté client, la dégradation des performances s’observe côté client, et non dans la base de données. Outre les opérations de chiffrement et de déchiffrement, les autres sources de dégradation des performances côté client sont les suivantes :
- Allers-retours supplémentaires vers la base de données pour récupérer des métadonnées pour les paramètres de requête.
- Appels au magasin de clés principales de colonne pour accéder à une clé principale de colonne.

Cette section décrit les outils intégrés d’optimisation des performances du fournisseur .NET Framework pour SQL Server à l’aide desquels vous pouvez contrôler l’impact sur les performances des deux facteurs ci-dessus.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Contrôle des allers-retours vers la base de données en vue de la récupération des métadonnées pour les paramètres de requête

Par défaut, si Always Encrypted est activé pour une connexion, le fournisseur de données .NET Framework pour SQL Server appelle [sys.sp_describe_parameter_encryption](../../system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) pour chaque requête paramétrable, en passant l’instruction de requête (sans valeurs de paramètre) à SQL Server. **sys.sp_describe_parameter_encryption** analyse l’instruction de requête afin de savoir si des paramètres doivent être chiffrés. Si c’est le cas, pour chaque paramètre à chiffrer, il renvoie des informations relatives au chiffrement qui permettent au fournisseur de données .NET Framework de chiffrer les valeurs de paramètre. Ce comportement garantit un haut niveau de transparence à l’application cliente. L’application (et le développeur d’application) n’ont pas besoin de connaître les requêtes qui accèdent aux colonnes chiffrées, tant que les valeurs ciblant les colonnes chiffrées sont passées au fournisseur de données .NET Framework pour SQL Server dans des objets SqlParameter.


### <a name="query-metadata-caching"></a>Mise en cache des métadonnées de requête

Dans .NET Framework 4.6.2 et versions ultérieures, le fournisseur de données .NET Framework pour SQL Server met en cache les résultats de **sys.sp_describe_parameter_encryption** pour chaque instruction de requête. Par conséquent, si la même instruction de requête est exécutée plusieurs fois, le pilote appelle **sys.sp_describe_parameter_encryption** une seule fois. La mise en cache des métadonnées de chiffrement pour les instructions de requête réduit sensiblement le coût en termes de performances de l’extraction des métadonnées à partir de la base de données. La mise en cache est activée par défaut. Vous pouvez désactiver la mise en cache des métadonnées de paramètre en définissant la [propriété SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled) sur false, bien que cela ne soit pas recommandé, sauf dans de très rares cas comme celui décrit ci-dessous :

Supposons une base de données ayant deux schémas différents : s1 et s2. Chaque schéma contient une table portant le même nom : t. Les définitions des tables s1.t et s2.t sont identiques, à l’exception des propriétés relatives au chiffrement : Une colonne nommée c dans s1.t n’est pas chiffrée, et elle est chiffrée dans s2.t. La base de données comporte deux utilisateurs : u1 et u2. Le schéma par défaut de l’utilisateur u1 est s1. Le schéma par défaut de l’utilisateur u2 est s2. Une application .NET ouvre deux connexions sur la base de données, en empruntant l’identité de l’utilisateur u1 sur une connexion et celle de l’utilisateur u2 sur une autre. L’application envoie une requête avec un paramètre ciblant la colonne c sur la connexion pour l’utilisateur u1 (le schéma de l’utilisateur par défaut est utilisé, dans la mesure où la requête ne spécifie aucun schéma). Ensuite, l’application envoie la même requête sur la connexion pour l’utilisateur u2. Si la mise en cache des métadonnées de requête est activée, après la première requête, le cache sera rempli avec les métadonnées en indiquant que la colonne c (celle ciblée par le paramètre de requête) n’est pas chiffrée. Dans la mesure où la deuxième requête comporte la même instruction de requête, les informations stockées dans le cache seront utilisées. Par conséquent, le pilote envoie la requête sans chiffrer le paramètre (qui est incorrect, car la colonne cible, s2.t.c, est chiffrée), entraînant la fuite de la valeur de texte en clair du paramètre vers le serveur. Le serveur détecte cette incompatibilité et force le pilote à actualiser le cache. L’application renvoie alors en toute transparence la requête avec la valeur de paramètre correctement chiffrée. Dans ce cas, la mise en cache doit être désactivée pour empêcher toute fuite de valeurs sensibles vers le serveur. 




### <a name="setting-always-encrypted-at-the-query-level"></a>Configuration d’Always Encrypted au niveau de la requête

Pour contrôler l’impact sur les performances de la récupération des métadonnées de chiffrement pour les requêtes paramétrables, vous pouvez activer Always Encrypted pour chaque requête, au lieu de le configurer pour la connexion. De cette façon, **sys.sp_describe_parameter_encryption** est appelé uniquement pour les requêtes dont les paramètres ciblent des colonnes chiffrées. Notez toutefois que de cette façon, vous réduisez la transparence du chiffrement. Si vous modifiez les propriétés de chiffrement de vos colonnes de base de données, vous devrez modifier le code de votre application pour l’aligner sur les modifications du schéma.

> [!NOTE]
> La définition d’Always Encrypted au niveau de la requête montre des avantages limités en termes de performances dans .NET 4.6.2 et versions ultérieures, qui implémentent la mise en cache des métadonnées de chiffrement de paramètre.

Pour contrôler le comportement d’Always Encrypted des requêtes, vous devez utiliser ce constructeur de [SqlCommand](/dotnet/api/system.data.sqlclient.sqlcommand) et [SqlCommandColumnEncryptionSetting](/dotnet/api/system.data.sqlclient.sqlcommandcolumnencryptionsetting). Voici quelques conseils utiles :
- Si la plupart des requêtes qu’une application cliente envoie via une connexion de base de données accèdent à des colonnes chiffrées :
    - Définissez le mot clé de la chaîne de connexion **Paramètre de chiffrement de colonne** sur *Activé*.
    - Définissez **SqlCommandColumnEncryptionSetting.Disabled** pour les requêtes qui n’accèdent à aucune colonne chiffrée. Cela désactive à la fois l’appel de sys.sp_describe_parameter_encryption et la tentative de déchiffrement des valeurs du jeu de résultats.
    - Définissez **SqlCommandColumnEncryptionSetting.ResultSet** pour les requêtes qui n’ont aucun paramètre exigeant un chiffrement, mais qui récupèrent des données de colonnes chiffrées. Cela désactive l’appel de sys.sp_describe_parameter_encryption et le chiffrement des paramètres. La requête est alors en mesure de déchiffrer les résultats des colonnes de chiffrement.
- Si la plupart des requêtes qu’une application cliente envoie via une connexion de base de données n’accèdent pas à des colonnes chiffrées :
    - Définissez le mot clé de la chaîne de connexion **Paramètre de chiffrement de colonne** sur **Désactivé**.
    - Définissez **SqlCommandColumnEncryptionSetting.Enabled** pour les requêtes qui ont des paramètres qui doivent être chiffrés. Cela active à la fois l’appel de sys.sp_describe_parameter_encryption et le déchiffrement des résultats de requête récupérés à partir des colonnes chiffrées.
    - Définissez **SqlCommandColumnEncryptionSetting.ResultSet** pour les requêtes qui n’ont aucun paramètre exigeant un chiffrement, mais qui récupèrent des données de colonnes chiffrées. Cela désactive l’appel de sys.sp_describe_parameter_encryption et le chiffrement des paramètres. La requête est alors en mesure de déchiffrer les résultats des colonnes de chiffrement.

Dans l’exemple ci-dessous, Always Encrypted est désactivé pour la connexion de base de données. La requête envoyée par l’application comprend un paramètre qui cible la colonne LastName qui n’est pas chiffrée. La requête récupère les données des colonnes SSN et BirthDate qui sont toutes deux chiffrées. Dans ce cas, il n’est pas nécessaire d’appeler sys.sp_describe_parameter_encryption pour récupérer les métadonnées de chiffrement. Toutefois, le déchiffrement des résultats de requête doit être activé, afin que l’application puisse recevoir des valeurs en texte clair à partir des deux colonnes chiffrées. Le paramètre SqlCommandColumnEncryptionSetting.ResultSet est utilisé à cette fin.



```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
using (SqlConnection connection = new SqlConnection(connectionString))
{
    connection.Open();
    using (SqlCommand cmd = new SqlCommand(@"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName",
connection, null, SqlCommandColumnEncryptionSetting.ResultSetOnly))
    {
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
}
```


### <a name="column-encryption-key-caching"></a>Mise en cache des clés de chiffrement de colonne

Pour réduire le nombre d’appels à un magasin de clés principales de colonne pour déchiffrer les clés de chiffrement de colonne, le fournisseur de données .NET Framework pour SQL Server met en cache les clés de chiffrement de colonne en texte en clair dans la mémoire. Après avoir reçu la valeur de clé de chiffrement de colonne chiffrée à partir des métadonnées de la base de données, le pilote tente d’abord de trouver la clé de chiffrement de colonne en texte en clair qui correspond à la valeur de clé chiffrée. Le pilote appelle le magasin de clés qui contient la clé principale de colonne uniquement s’il ne peut pas trouver la valeur de clé de chiffrement de colonne chiffrée dans le cache.

> [!NOTE]
> Dans .NET Framework 4.6 et 4.6.1, les entrées de clés de chiffrement de colonne ne sont jamais supprimées du cache. Cela signifie que, pour une clé de chiffrement de colonne chiffrée, le pilote ne contacte le magasin de clés qu’une seule fois au cours de la durée de vie de l’application.

Dans .NET Framework 4.6.2 et versions ultérieures, les entrées du cache sont supprimées après un intervalle de durée de vie configurable pour des raisons de sécurité. La valeur de durée de vie par défaut est de 2 heures. Si vous avez des exigences de sécurité plus strictes concernant la durée pendant laquelle les clés de chiffrement de colonne peuvent être mises en cache en texte clair dans l’application, vous pouvez la modifier à l’aide de la propriété [SqlConnection.ColumnEncryptionKeyCacheTtl](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionkeycachettl). 


## <a name="enabling-additional-protection-for-a-compromised-sql-server"></a>Activation de la protection supplémentaire pour un serveur SQL Server compromis

Par défaut, les *fournisseur de données .NET Framework pour SQL Server* repose sur le système de base de données (SQL Server ou Azure SQL Database) pour fournir des métadonnées indiquant les colonnes de la base de données à chiffrer et de quelle manière. Les métadonnées de chiffrement permettent au fournisseur de données .NET Framework pour SQL Server de chiffrer des paramètres de requête et de déchiffrer les résultats de requête sans aucune entrée de l’application, ce qui réduit considérablement le nombre de changements nécessaires dans l’application. Toutefois, si le processus SQL Server est compromis et qu’une personne malveillante falsifie les métadonnées envoyées par SQL Server au fournisseur de données .NET Framework pour SQL Server, cette dernière peut être en mesure de dérober des informations sensibles. Cette section décrit les API qui permettent d’offrir un niveau de protection supplémentaire contre ce type d’attaque au prix d’une réduction de la transparence. 

### <a name="forcing-parameter-encryption"></a>Forcer le chiffrement de paramètre 

Avant que le fournisseur de données .NET Framework pour SQL Server n’envoie une requête paramétrable à SQL Server, celui-ci demande à SQL Server (en appelant [sys.sp_describe_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md)) d’analyser l’instruction de requête et de fournir des informations sur les paramètres de requête à chiffrer. Une instance de SQL Server compromise pourrait induire en erreur le fournisseur de données .NET Framework pour SQL Server en envoyant les métadonnées indiquant que le paramètre ne cible pas une colonne chiffrée, en dépit du fait que la colonne est chiffrée dans la base de données. Par conséquent, le fournisseur de données .NET Framework pour SQL Server ne chiffrerait pas la valeur du paramètre, et l’enverrait en tant que texte en clair à l’instance de SQL Server compromise.

Pour éviter ce type d’attaque, une application peut définir la propriété [SqlParameter.ForceColumnEncryption](/dotnet/api/system.data.sqlclient.sqlparameter.forcecolumnencryption) du paramètre sur true. Ainsi, le fournisseur de données .NET Framework pour SQL Server lève une exception, si les métadonnées reçues du serveur indiquent que le paramètre n’a pas besoin d’être chiffré.

Bien que l’utilisation de la **propriété SqlParameter.ForceColumnEncryption** contribue à durcir la sécurité, elle réduit également la transparence du chiffrement sur l’application cliente. Si vous mettez à jour le schéma de base de données pour modifier le jeu de colonnes chiffrées, vous devrez apporter des modifications à l’application.

L’exemple de code suivant illustre l’utilisation de la propriété **SqlParameter.ForceColumnEncryption** afin d’éviter l’envoi de numéros de sécurité sociale sous forme de texte en clair. 



```cs
SqlCommand cmd = _sqlconn.CreateCommand(); 

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

SqlDataReader reader = cmd.ExecuteReader();
```
 

### <a name="configuring-trusted-column-master-key-paths"></a>Configuration des chemins d’accès des clés principales de colonne approuvés 

Les métadonnées de chiffrement, renvoyées par SQL Server pour les paramètres de requête ciblant des colonnes chiffrées et pour les résultats récupérés à partir de colonnes de chiffrement, incluent le chemin d’accès des clés principales de colonne qui identifie le magasin de clés et l’emplacement de la clé dans ce dernier. Si l’instance SQL Server est compromise, celle-ci pourrait envoyer le chemin d’accès des clés dirigeant le fournisseur de données .NET Framework pour SQL Server vers l’emplacement contrôlé par une personne malveillante. Cela peut entraîner une fuite des informations d’identification du magasin de clés, dans le cas d’un magasin de clés nécessitant l’authentification de l’application. 

Pour empêcher ces attaques, l’application peut spécifier la liste des chemins d’accès des clés approuvés pour un serveur donné en utilisant la propriété [SqlConnection.ColumnEncryptionTrustedMasterKeyPaths](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths). Si le.fournisseur de données .NET Framework pour SQL Server reçoit un chemin d’accès des clés ne figurant pas dans la liste des chemins d’accès de clé approuvés, il lève une exception. 

Bien que la définition de chemins de clé approuvés durcisse la sécurité de votre application, vous devez changer le code ou/et la configuration de l’application chaque fois que vous permutez votre clé principale de colonne (à chaque changement du chemin de la clé principale de colonne). 

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

- Vérifiez que la configuration du chiffrement de la table cible est identique à celle de la table source. Les deux tables doivent avoir les mêmes colonnes chiffrées, et ces colonnes doivent être chiffrées à l’aide des mêmes types et des mêmes clés de chiffrement. Remarque : Si les colonnes cibles sont chiffrées différemment de la colonne source correspondante, vous ne pourrez pas déchiffrer les données de la table cible après les avoir copiées. Les données seront endommagées.
- Configurez les deux connexions de base de données, c’est-à-dire, celle vers la table source et celle vers la table cible, sans activer Always Encrypted. 
- Définissez l’option AllowEncryptedValueModifications (voir [SqlBulkCopyOptions](/dotnet/api/system.data.sqlclient.sqlbulkcopyoptions)). Remarque : Faites attention lorsque vous spécifiez AllowEncryptedValueModifications, car cela peut endommager la base de données. En effet, le fournisseur de données .NET Framework pour SQL Server ne vérifie pas si les données sont chiffrées, ou si elles sont correctement chiffrées à l’aide du même type de chiffrement, du même algorithme et de la même clé que la colonne cible.

L’option AllowEncryptedValueModifications est disponible dans .NET Framework 4.6.1 et versions ultérieures.

Voici un exemple qui copie les données d’une table à une autre. Les colonnes SSN et BirthDate sont censées être chiffrées.
        

```cs
static public void CopyTablesUsingBulk(string sourceTable, string targetTable)
{
   string sourceConnectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
   string targetConnectionString = "Data Source= server64; Initial Catalog=Clinic; Integrated Security=true";
   using (SqlConnection connSource = new SqlConnection(sourceConnectionString))
   {
      connSource.Open();
      using (SqlCommand cmd = new SqlCommand(string.Format("SELECT [PatientID], [SSN], [FirstName], [LastName], [BirthDate] FROM {0}", sourceTable), connSource))
      {
         using (SqlDataReader reader = cmd.ExecuteReader())
         {
            SqlBulkCopy copy = new SqlBulkCopy(targetConnectionString, SqlBulkCopyOptions.KeepIdentity | SqlBulkCopyOptions.AllowEncryptedValueModifications);
            copy.EnableStreaming = true;
            copy.DestinationTableName = targetTable;
            copy.WriteToServer(reader);
         }
      }
}
```


## <a name="always-encrypted-api-reference"></a>Référence de l’API Always Encrypted

**Espace de noms :** [System.Data.SqlClient](/dotnet/api/system.data.sqlclient)

**Assembly :** System.Data (dans System.Data.dll)




|Nom|Description|Introduit avec .NET
|:---|:---|:---
|[Classe SqlColumnEncryptionCertificateStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider)|Fournisseur de magasin de clés pour le magasin de certificats Windows.|  4,6
|[Classe SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider)|Fournisseur de magasin de clés pour l’API de chiffrement Microsoft de nouvelle génération (CNG).|  4.6.1
|[Classe SqlColumnEncryptionCspProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncspprovider)|Fournisseur de magasin de clés pour les fournisseurs de services de chiffrement (CSP) basés sur la CAPI Microsoft.|4.6.1  
|[classe SqlColumnEncryptionKeyStoreProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptionkeystoreprovider)|Classe de base des fournisseurs de magasins de clés.|  4,6
|[Énumération SqlCommandColumnEncryptionSetting](/dotnet/api/system.data.sqlclient.sqlcommandcolumnencryptionsetting)|Paramètres pour activer le chiffrement et le déchiffrement d’une connexion de base de données.|4,6  
|[Énumération SqlConnectionColumnEncryptionSetting](/dotnet/api/system.data.sqlclient.sqlconnectioncolumnencryptionsetting)|Paramètres pour contrôler le comportement d’Always Encrypted pour des requêtes individuelles.|4,6  
| [Propriété SqlConnectionStringBuilder.ColumnEncryptionSetting](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting)|Obtient et définit Always Encrypted dans la chaîne de connexion.|4,6|
| [propriété SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled) | Active et désactive la mise en cache des métadonnées de requête de chiffrement. | 4.6.2
| [SqlConnection.ColumnEncryptionKeyCacheTtl](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptionkeycachettl) | Obtient et définit la durée de vie des entrées du cache de clé de chiffrement de colonne. | 4.6.2
|[SqlConnection.ColumnEncryptionTrustedMasterKeyPaths](/dotnet/api/system.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths)|Vous permet de définir une liste de chemins d’accès de clés approuvés pour un serveur de bases de données. Si le pilote reçoit un chemin de clé qui ne figure pas dans la liste pendant le traitement d’une requête d’application, celle-ci échouera. Cette propriété fournit une protection supplémentaire contre les attaques de sécurité durant lesquelles un ordinateur SQL Server compromis fournit des chemins d’accès de clés factices, qui peuvent entraîner une fuite des informations d’identification du magasin de clés.|  4.6
|[Méthode SqlConnection.RegisterColumnEncryptionKeyStoreProviders](/dotnet/api/system.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders)|Vous permet d’inscrire des fournisseurs de magasins de clés personnalisés. Il s’agit d’un dictionnaire qui mappe des noms de fournisseurs de magasins de clés à des implémentations de fournisseurs de magasins de clés.|  4,6
|[Constructeur SqlCommand (String, SqlConnection, SqlTransaction, SqlCommandColumnEncryptionSetting)](https://msdn.microsoft.com/library/dn956511\(v=vs.110\).aspx)|Permet de contrôler le comportement d’Always Encrypted pour des requêtes individuelles.|  4,6
|[SqlParameter.ForceColumnEncryption](/dotnet/api/system.data.sqlclient.sqlparameter.forcecolumnencryption)|Applique le chiffrement d’un paramètre. Si SQL Server informe le pilote que le paramètre ne nécessite pas de chiffrement, la requête utilisant le paramètre échoue. Cette propriété fournit une protection supplémentaire contre les attaques au niveau de la sécurité qui impliquent un serveur SQL Server compromis fournissant des métadonnées de chiffrement incorrectes au client, ce qui peut entraîner la divulgation de données.|4,6  
|Nouveau mot clé de [chaîne de connexion](/dotnet/api/system.data.sqlclient.sqlconnection.connectionstring) : `Column Encryption Setting=enabled`|Active ou désactive la fonctionnalité Always Encrypted pour la connexion.| 4,6 
  

## <a name="see-also"></a>Voir aussi

- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Blog Chiffrement intégral](/archive/blogs/sqlsecurity/getting-started-with-always-encrypted)
- [Didacticiel de SQL Database : Protéger les données à l’aide d’Always Encrypted](/azure/azure-sql/database/always-encrypted-certificate-store-configure)
