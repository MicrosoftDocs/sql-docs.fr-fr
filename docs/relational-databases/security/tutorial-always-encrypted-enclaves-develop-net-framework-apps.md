---
description: 'Tutoriel : Développer une application .NET Framework avec Always Encrypted avec enclaves sécurisées'
title: 'Tutoriel : Développer une application .NET Framework avec Always Encrypted avec enclaves sécurisées | Microsoft Docs'
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 092220a2ef2715b8d5cd414ab5a4bc3e3493acb5
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534738"
---
# <a name="tutorial-develop-a-net-framework-application-using-always-encrypted-with-secure-enclaves"></a>Tutoriel : Développer une application .NET Framework avec Always Encrypted avec enclaves sécurisées

[!INCLUDE [sqlserver2019-windows-only-asdb](../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Ce tutoriel vous apprend à développer une application qui émet des requêtes de base de données utilisant une enclave sécurisée côté serveur pour [Always Encrypted avec enclaves sécurisées](encryption/always-encrypted-enclaves.md). 

## <a name="prerequisites"></a>Prérequis
Veillez à suivre l’un des tutoriels ci-dessous avant d’effectuer les étapes de ce tutoriel :

- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

Vous aurez également besoin de Visual Studio (version 2019 recommandée), que vous pouvez télécharger ici : [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). Votre ordinateur de développement d’applications doit exécuter .NET Framework 4.7.2 ou version ultérieure.

## <a name="step-1-set-up-your-visual-studio-project"></a>Étape 1 : Configurez votre projet Visual Studio

Pour utiliser Always Encrypted avec enclaves sécurisées dans une application .NET Framework, vous devez vérifier que votre application repose sur .NET Framework 4.7.2 et qu’elle est intégrée au package NuGet [Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders). En outre, si vous stockez votre clé principale de colonne dans Azure Key Vault, vous devez également intégrer votre application au package NuGet [Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider) version 2.2.0 ou ultérieure. 

1. Ouvrez Visual Studio.

2. Créez un projet (.NET Framework) d’application console C\#.

3. Assurez-vous que votre projet cible au moins .NET Framework 4.7.2. Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, sélectionnez **Propriétés**, puis configurez **Framework cible** sur .NET Framework 4.7.2.

4. Installez le package NuGet suivant en accédant à **Outils** (menu principal) > **Gestionnaire de Package NuGet** > **Console du gestionnaire de package**. Exécutez le code suivant dans la Console du gestionnaire de Package.

   ```powershell
   Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders -IncludePrerelease
   ```

5. Si vous utilisez Azure Key Vault pour stocker vos clés principales de colonne, installez les packages NuGet suivants en accédant à **Outils** (menu principal) > **Gestionnaire de package NuGet** > **Console de gestionnaire de package**. Exécutez le code suivant dans la Console du gestionnaire de Package.

   ```powershell
   Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider -IncludePrerelease -Version 2.2.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

6. Ouvrez le fichier App.config pour votre projet.

7. Recherchez la section `<configuration>`, et ajoutez ou mettez à jour les sections `<configSections>`.

   1. Si la section `<configuration>` ne contient **pas** de section `<configSections>`, ajoutez le contenu suivant juste en dessous de `<configuration>`.

      ```xml
      <configSections>
        <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
      </configSections>
      ```

   2. Si la section `<configuration>` contient déjà la section `<configSections>`, ajoutez la ligne suivante à la section `<configSections>` :

      ```xml
      <section name="SqlColumnEncryptionEnclaveProviders"  type="System.   Data.SqlClient.   SqlColumnEncryptionEnclaveProviderConfigurationSection, System.   Data,  Version=4.0.0.0, Culture=neutral,    PublicKeyToken=b77a5c561934e089" />
      ```

8. Dans la section `<configuration>`, sous `</configSections>`, ajoutez une section qui spécifie le fournisseur d’enclave à utiliser pour attester et interagir avec votre enclave sécurisée côté serveur.

   1. Si vous utilisez [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH) (vous utilisez la base de données du [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md)), ajoutez la section ci-dessous.

      ```xml
      <SqlColumnEncryptionEnclaveProviders>
        <providers>
          <add name="VBS"  type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider,  Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders,    Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"/>
        </providers>
      </SqlColumnEncryptionEnclaveProviders>
      ```

      Voici un exemple complet de fichier app.config pour une application console simple.

      ```xml
      <?xml version="1.0" encoding="utf-8" ?>
      <configuration>
        <configSections>
          <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
        </configSections>
        <SqlColumnEncryptionEnclaveProviders>
          <providers>
            <add name="VBS"  type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider,  Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders,    Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"/>
          </providers>
        </SqlColumnEncryptionEnclaveProviders>
        <startup> 
         <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
        </startup>
      </configuration>
      ```

   1. Si vous utilisez [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et Microsoft Azure Attestation (vous utilisez la base de données du [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)), ajoutez la section ci-dessous.

      ```xml
      <SqlColumnEncryptionEnclaveProviders>
        <providers>
          <add name="SGX" type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.AzureAttestationEnclaveProvider, Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" />
        </providers>
      </SqlColumnEncryptionEnclaveProviders>
      ```

      Voici un exemple complet de fichier app.config pour une application console simple.

      ```xml
      <?xml version="1.0" encoding="utf-8" ?>
      <configuration>
        <configSections>
          <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
        </configSections>
        <SqlColumnEncryptionEnclaveProviders>
          <providers>
            <add name="SGX" type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.AzureAttestationEnclaveProvider, Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" />
          </providers>
        </SqlColumnEncryptionEnclaveProviders>
        <startup> 
         <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
        </startup>
      </configuration>
      ```

## <a name="step-2-implement-your-application-logic"></a>Étape 2 : Implémentez votre logique d’application

Votre application se connecte à la base de données **ContosoHR** à partir du [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md) ou [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) et exécute une requête qui contient le prédicat `LIKE` sur la colonne **SSN** et une comparaison de plages sur la colonne **Salary**.

1. Remplacez le contenu du fichier Program.cs (généré par Visual Studio) par le code ci-dessous. Mettez à jour la chaîne de connexion de base de données avec le nom de votre serveur, les paramètres d’authentification de la base de données et l’URL d’attestation d’enclave de votre environnement.

    ```cs
    using System;
    using System.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {
    
                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                //string connectionString = "Data Source = myserver.database.windows.net; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Enclave Attestation Url = https://myattestationprovider.uks.attest.azure.net/attest/SgxEnclave; User ID=user; Password=password";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [HR].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%9838";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 20000;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();
    
                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader[0] + ", " + reader[1] + ", " + reader[2] + ", " + reader[3]);
                    }   
                    Console.ReadKey();
                }
            }
        }
    }
    ```

2. Générez et exécutez l’application.  

## <a name="see-also"></a>Voir aussi

- [Utilisation d’Always Encrypted avec le fournisseur de données .NET Framework pour SQL Server](encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)
- [Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées](../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
