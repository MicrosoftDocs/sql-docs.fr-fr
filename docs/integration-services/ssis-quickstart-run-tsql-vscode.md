---
description: Exécuter un package SSIS à partir de Visual Studio Code avec Transact-SQL
title: Exécuter un package SSIS avec Transact-SQL (VS Code) | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c911778dbf9321a09fb901806e50f9fae501507a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336307"
---
# <a name="run-an-ssis-package-from-visual-studio-code-with-transact-sql"></a>Exécuter un package SSIS à partir de Visual Studio Code avec Transact-SQL

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Ce guide de démarrage rapide montre comment utiliser Visual Studio Code pour se connecter à la base de données du catalogue SSIS, puis utiliser des instructions Transact-SQL pour exécuter un package SSIS stocké dans le catalogue SSIS.

Visual Studio Code est un éditeur de code pour Windows, macOS et Linux qui prend en charge les extensions, notamment l’extension `mssql` pour la connexion à Microsoft SQL Server, à Azure SQL Database et à Azure Synapse Analytics. Pour plus d’informations sur VS Code, consultez [Visual Studio Code](https://code.visualstudio.com/).

## <a name="prerequisites"></a>Prérequis

Avant de commencer, vérifiez que vous avez installé la dernière version de Visual Studio Code et chargé l’extension `mssql`. Pour télécharger ces outils, consultez les pages suivantes :
-   [Télécharger Visual Studio Code](https://code.visualstudio.com/Download)
-   [Extension mssql](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)

## <a name="supported-platforms"></a>Plateformes prises en charge

Vous pouvez utiliser les informations de ce guide de démarrage rapide pour exécuter un package SSIS sur les plateformes suivantes :

-   SQL Server sur Windows.

-   Azure SQL Database. Pour plus d’informations sur le déploiement et l’exécution de packages dans Azure, consultez [Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Vous ne pouvez pas utiliser les informations de ce guide de démarrage rapide pour exécuter un package SSIS sur Linux. Pour plus d’informations sur l’exécution de packages sur Linux, consultez [Extraire, transformer et charger des données sur Linux avec SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="set-language-mode-to-sql-in-vs-code"></a>Définir SQL comme mode de langage dans VS Code

Pour activer les commandes `mssql` et T-SQL IntelliSense, définissez **SQL** comme mode de langage dans Visual Studio Code.

1. Ouvrez Visual Studio Code, puis ouvrez une nouvelle fenêtre. 

2. Cliquez sur **Texte brut** dans le coin inférieur droit de la barre d’état.

3. Dans le menu déroulant **Sélectionner le mode de langage** qui s’affiche, sélectionnez ou entrez **SQL**, puis appuyez sur **Entrée** pour définir SQL comme mode de langage. 

## <a name="for-azure-sql-database-get-the-connection-info"></a>Pour Azure SQL Database, obtenez les informations de connexion.

Pour exécuter le package sur Azure SQL Database, obtenez les informations de connexion dont vous avez besoin pour vous connecter à la base de données du catalogue SSIS (SSISDB). Vous avez besoin des informations de connexion et du nom de serveur complet dans les procédures qui suivent.

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
2. Sélectionnez **Bases de données SQL** dans le menu de gauche, puis sélectionnez la base de données SSISDB dans la page **Bases de données SQL**. 
3. Dans la page **Vue d’ensemble** de votre base de données, notez le nom complet du serveur. Pour voir l’option **Cliquer pour copier**, pointez sur le nom du serveur. 
4. Si vous avez oublié vos informations de connexion au serveur Azure SQL Database, accédez à la page du serveur SQL Database pour voir le nom de l’administrateur du serveur. Vous pouvez réinitialiser le mot de passe si nécessaire.

## <a name="connect-to-the-ssis-catalog-database"></a>Se connecter à la base de données du catalogue SSIS

Utilisez Visual Studio Code pour établir une connexion au catalogue SSIS.

> [!IMPORTANT]
> Avant de poursuivre, assurez-vous que votre serveur et votre base de données sont prêts, et que vous disposez de vos informations de connexion. Si vous modifiez le focus à partir de Visual Studio Code après avoir commencé à entrer les informations de profil de connexion, vous devez redémarrer la création du profil de connexion.

1. Dans VS Code, appuyez sur **CTRL+MAJ+P** (ou **F1**) pour ouvrir la Palette de commandes.

2. Tapez **sqlcon**, puis appuyez sur **ENTRÉE**.

3. Appuyez sur **ENTRÉE** pour sélectionner **Créer un profil de connexion**. Cette étape crée un profil de connexion pour votre instance de SQL Server.

4. Suivez les invites pour spécifier les propriétés de connexion de ce nouveau profil de connexion. Après avoir spécifié chaque valeur, appuyez sur **ENTRÉE** pour continuer. 

   | Paramètre       | Valeur suggérée | En savoir plus |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nom du serveur** | Nom complet du serveur | Si vous vous connectez à un serveur Azure SQL Database, le nom est au format suivant : `<server_name>.database.windows.net`. |
   | **Nom de la base de données** | **SSISDB** | Nom de la base de données à laquelle se connecter. |
   | **Authentification** | Connexion SQL | Avec l’authentification SQL Server, vous pouvez vous connecter à SQL Server ou à Azure SQL Database. Si vous vous connectez à un serveur Azure SQL Database, vous ne pouvez pas utiliser l’authentification Windows. |
   | **Nom d'utilisateur** | Compte d’administrateur de serveur | Il s’agit du compte que vous avez spécifié quand vous avez créé le serveur. |
   | **Mot de passe (connexion SQL)** | Mot de passe de votre compte d’administrateur de serveur | Il s’agit du mot de passe que vous avez spécifié quand vous avez créé le serveur. |
   | **Enregistrer le mot de passe ?** | Oui ou Non | Si vous ne souhaitez pas entrer le mot de passe à chaque fois, sélectionnez Oui. |
   | **Entrez un nom pour ce profil** | Nom d’un profil, tel que **mySSISServer** | Un nom de profil enregistré permet d’accélérer votre connexion lors des connexions suivantes. | 

5. Appuyez sur la touche **ÉCHAP** pour fermer le message d’information qui vous informe que le profil est créé et connecté.

6. Vérifiez votre connexion dans la barre d’état.

## <a name="run-the-t-sql-code"></a>Exécuter le code T-SQL
Exécutez le code Transact-SQL suivant pour exécuter un package SSIS.

1. Dans la fenêtre **Éditeur**, entrez la requête suivante dans la fenêtre de requête vide. (Ce code est le code généré par l’option **Script** dans la boîte de dialogue **Exécuter le package** dans SSMS.)

2. Mettez à jour les valeurs de paramètres dans la procédure stockée `catalog.create_execution` pour votre système.

3. Appuyez sur **Ctrl+Maj+E** pour exécuter le code et le package.

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx',
    @execution_id=@execution_id OUTPUT,
    @folder_name=N'Deployed Projects',
      @project_name=N'Integration Services Project1',
    @use32bitruntime=False,
      @reference_id=Null
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,
    @object_type=50,
      @parameter_name=N'LOGGING_LEVEL',
      @parameter_value=@var0
EXEC [SSISDB].[catalog].[start_execution] @execution_id
GO
```

## <a name="next-steps"></a>Étapes suivantes
- Envisagez d’autres méthodes pour exécuter un package.
    - [Exécuter un package SSIS avec SSMS](./ssis-quickstart-run-ssms.md)
    - [Exécuter un package SSIS avec Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Exécuter un package SSIS à partir de l’invite de commandes](./ssis-quickstart-run-cmdline.md)
    - [Exécuter un package SSIS avec PowerShell](ssis-quickstart-run-powershell.md)
    - [Exécuter un package SSIS avec C#](./ssis-quickstart-run-dotnet.md) 
