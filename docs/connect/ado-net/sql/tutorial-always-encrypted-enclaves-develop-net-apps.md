---
description: 'Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées'
title: 'Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées | Microsoft Docs'
ms.custom: ''
ms.date: 07/09/2020
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 59c377d5055e8eb3858e1005c50d9c4dfb8334ab
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771529"
---
# <a name="tutorial-develop-a-net-application-using-always-encrypted-with-secure-enclaves"></a>Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-xxxx-md.md)]

Ce tutoriel vous apprend à développer une application simple qui émet des requêtes de base de données utilisant une enclave sécurisée côté serveur pour [Always Encrypted avec enclaves sécurisées](../../../relational-databases/security/encryption/always-encrypted-enclaves.md).

> [!NOTE]
> Always Encrypted avec enclaves sécurisés n’est disponible que sur Windows.

## <a name="prerequisites"></a>Prérequis

Ce didacticiel est la continuation du [Didacticiel : Prise en main d’Always Encrypted avec enclaves sécurisées en utilisant SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md). Assurez-vous que vous avez terminé, avant de suivre les étapes ci-dessous.

Par ailleurs, vous devez avoir Visual Studio (version 2019 recommandée), que vous pouvez télécharger depuis [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). Votre environnement de développement d’applications doit utiliser .NET Framework 4.6 ou version ultérieure ou .NET Core 2.1 ou version ultérieure.

## <a name="step-1-set-up-your-visual-studio-project"></a>Étape 1 : Configurez votre projet Visual Studio

Pour utiliser Always Encrypted avec des enclaves sécurisées dans une application .NET Framework, vous devez vous assurer que votre application cible .NET Framework 4.6 ou une version ultérieure. Pour utiliser Always Encrypted avec des enclaves sécurisées dans une application .NET Core, vous devez vous assurer que votre application cible .NET Core 2.1 ou une version ultérieure.

En outre, si vous stockez votre clé principale de colonne dans Azure Key Vault, vous devez également intégrer votre application à [Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider NuGet](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider).

1. Ouvrez Visual Studio.

2. Créez un nouveau projet (.NET Framework / Core) d’application console C\#.

3. Assurez-vous que votre projet cible au moins .NET Framework 4.6 ou .NET Core 2.1. Cliquez avec le bouton de droite sur le projet dans l’Explorateur de solutions, sélectionnez Propriétés et définissez la version cible de .NET Framework.

4. Installez le package NuGet suivant en accédant à **Outils** (menu principal) > **Gestionnaire de Package NuGet** > **Console du gestionnaire de package**. Exécutez le code suivant dans la Console du gestionnaire de Package.

   ```powershell
   Install-Package Microsoft.Data.SqlClient -Version 1.1.0
   ```

5. Si vous utilisez Azure Key Vault pour stocker vos clés principales de colonne, installez les packages NuGet suivants en accédant à **Outils** (menu principal) > **Gestionnaire de package NuGet** > **Console de gestionnaire de package**. Exécutez le code suivant dans la Console du gestionnaire de Package.

   ```powershell
   Install-Package Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider -Version 1.0.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

6. Spécifiez le `Attestation Protocol` et le `Enclave Attestation Url` dans la chaîne de connexion, qui sera utilisée dans votre application pour communiquer avec le SQL Server.

  ```cs
   Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Column Encryption Setting = Enabled
   ```

## <a name="step-2-implement-your-application-logic"></a>Étape 2 : Implémentez votre logique d’application

Votre application se connecte à la base de données **ContosoHR** à partir du [Tutoriel : Prise en main de Always Encrypted avec enclaves sécurisées en utilisant SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) pour exécute une requête qui contient le prédicat `LIKE` sur la colonne **SSN** et une comparaison de plages sur la colonne **Salaire**.

1. Remplacez le contenu du fichier Program.cs (généré par Visual Studio) par le code ci-dessous. Mettez à jour la chaîne de connexion de base de données avec le nom de votre serveur et l’URL d’attestation d’enclave pour votre environnement. Vous pouvez aussi mettre à jour les paramètres d’authentification de base de données.

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

                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [dbo].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%1111";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 900;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();

                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader);
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

- [Utilisation d’Always Encrypted avec le Fournisseur de données Microsoft .NET pour SQL Server](sqlclient-support-always-encrypted.md)
- [Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted](azure-key-vault-example.md)
- [Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted activé avec des Enclaves sécurisées](azure-key-vault-enclave-example.md)
