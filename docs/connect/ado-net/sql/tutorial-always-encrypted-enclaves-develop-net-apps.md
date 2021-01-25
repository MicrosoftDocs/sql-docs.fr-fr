---
description: 'Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées'
title: 'Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées | Microsoft Docs'
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 177737bd2927583bdfda1c9b36904faf4ed6023d
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534698"
---
# <a name="tutorial-develop-a-net-application-using-always-encrypted-with-secure-enclaves"></a>Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-xxxx-md.md)]

Ce tutoriel vous apprend à développer une application qui émet des requêtes de base de données utilisant une enclave sécurisée côté serveur pour [Always Encrypted avec enclaves sécurisées](../../../relational-databases/security/encryption/always-encrypted-enclaves.md).

> [!NOTE]
> Always Encrypted avec enclaves sécurisés n’est disponible que sur Windows.

## <a name="prerequisites"></a>Prérequis

Veillez à suivre l’un des tutoriels ci-dessous avant d’effectuer les étapes de ce tutoriel :

- [Tutoriel : Démarrage avec Always Encrypted avec enclaves sécurisées dans SQL Server](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutoriel : Démarrage avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

Par ailleurs, vous devez avoir Visual Studio (version 2019 recommandée), que vous pouvez télécharger depuis [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). Votre environnement de développement d’applications doit utiliser .NET Framework 4.6 ou version ultérieure ou .NET Core 2.1 ou version ultérieure.

## <a name="step-1-set-up-your-visual-studio-project"></a>Étape 1 : Configurez votre projet Visual Studio

Pour utiliser Always Encrypted avec des enclaves sécurisées dans une application .NET Framework, vous devez vous assurer que votre application cible .NET Framework 4.6 ou une version ultérieure. Pour utiliser Always Encrypted avec des enclaves sécurisées dans une application .NET Core, vous devez vous assurer que votre application cible .NET Core 2.1 ou une version ultérieure.

En outre, si vous stockez votre clé principale de colonne dans Azure Key Vault, vous devez également intégrer votre application à [Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider NuGet](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider).

1. Ouvrez Visual Studio.

2. Créez un nouveau projet (.NET Framework / Core) d’application console C\#.

3. Assurez-vous que votre projet cible au moins .NET Framework 4.6 ou .NET Core 2.1. Cliquez avec le bouton de droite sur le projet dans l’Explorateur de solutions, sélectionnez **Propriétés** et définissez la version cible de .Net Framework.

4. Installez le package NuGet suivant en accédant à **Outils** (menu principal) > **Gestionnaire de Package NuGet** > **Console du gestionnaire de package**. Exécutez le code suivant dans la Console du gestionnaire de Package.

   ```powershell
   Install-Package Microsoft.Data.SqlClient -Version 1.1.0
   ```

5. Si vous utilisez Azure Key Vault pour stocker vos clés principales de colonne, installez les packages NuGet suivants en accédant à **Outils** (menu principal) > **Gestionnaire de package NuGet** > **Console de gestionnaire de package**. Exécutez le code suivant dans la Console du gestionnaire de Package.

   ```powershell
   Install-Package Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider -Version 1.0.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

## <a name="step-2-implement-your-application-logic"></a>Étape 2 : Implémentez votre logique d’application

Votre application se connecte à la base de données **ContosoHR** à partir du [Tutoriel : Démarrage avec Always Encrypted avec enclaves sécurisées en utilisant SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) ou - [Tutoriel : Démarrage avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) pour l’exécution d’une requête qui contient le prédicat `LIKE` sur la colonne **SSN** et une comparaison de plages sur la colonne **Salaire**.

1. Remplacez le contenu du fichier Program.cs (généré par Visual Studio) par le code ci-dessous. 

    ```cs
    using System;
    using Microsoft.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {

                // Connection string for SQL Server
                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                // Connection string for Azure SQL Database
                //string connectionString = "Data Source = myserver.database.windows.net; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = AAS; Enclave Attestation Url = https://myattestationprovider.uks.attest.azure.net/attest/SgxEnclave; User ID=user; Password=password";

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

2. Mise à jour de la chaîne de connexion de base de données.
    1. Définissez le nom du serveur valide et les paramètres d’authentification de votre base de données.
    2. Définissez la valeur du mot clé `Attestation Protocol` sur :
       - `HGS` - si vous utilisez [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH).
       - `AAS` - si vous utilisez [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] et Microsoft Azure attestation.
    3. Définissez `Enclave Attestation URL` sur une URL d’attestation pour votre environnement.

3. Générez et exécutez l’application.

## <a name="see-also"></a>Voir aussi

- [Utilisation d’Always Encrypted avec le Fournisseur de données Microsoft .NET pour SQL Server](sqlclient-support-always-encrypted.md)
- [Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted](azure-key-vault-example.md)
- [Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted activé avec des Enclaves sécurisées](azure-key-vault-enclave-example.md)
