---
description: Déployer un projet SSIS à partir de SSMS avec Transact-SQL
title: Déployer un projet SSIS avec Transact-SQL (SSMS) | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 609090873281197875eba8cfeb54292df9f79ba9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354229"
---
# <a name="deploy-an-ssis-project-from-ssms-with-transact-sql"></a>Déployer un projet SSIS à partir de SSMS avec Transact-SQL

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Ce guide de démarrage rapide montre comment utiliser SQL Server Management Studio (SSMS) pour se connecter à la base de données du catalogue SSIS, puis utiliser des instructions Transact-SQL pour déployer un projet SSIS dans le catalogue SSIS. 

SQL Server Management Studio est un environnement intégré pour la gestion des infrastructures SQL, de SQL Server à SQL Database. Pour plus d’informations sur SSMS, consultez [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md).

## <a name="prerequisites"></a>Prérequis

Avant de commencer, vérifiez que vous disposez de la dernière version de SQL Server Management Studio. Pour télécharger SSMS, consultez [Télécharger SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).

## <a name="supported-platforms"></a>Plateformes prises en charge

Vous pouvez utiliser les informations de ce guide de démarrage rapide pour déployer un projet SSIS sur les plateformes suivantes :

-   SQL Server sur Windows.

Vous ne pouvez pas utiliser les informations de ce guide de démarrage rapide pour déployer un package SSIS sur Azure SQL Database. La procédure stockée `catalog.deploy_project` attend le chemin du fichier `.ispac` dans le système de fichiers local. Pour plus d’informations sur le déploiement et l’exécution de packages dans Azure, consultez [Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Vous ne pouvez pas utiliser les informations de ce guide de démarrage rapide pour déployer un package SSIS sur SQL Server sur Linux. Pour plus d’informations sur l’exécution de packages sur Linux, consultez [Extraire, transformer et charger des données sur Linux avec SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="supported-authentication-method"></a>Méthode d’authentification prise en charge

Consultez [Méthodes d’authentification pour le déploiement](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment).

## <a name="connect-to-the-ssis-catalog-database"></a>Se connecter à la base de données du catalogue SSIS

Utilisez SQL Server Management Studio pour établir une connexion au catalogue SSIS. 

1. Ouvrez SQL Server Management Studio.

2. Dans la fenêtre **Se connecter au serveur**, entrez les valeurs suivantes :

   | Paramètre       | Valeur suggérée | En savoir plus | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Type de serveur** | Moteur de base de données | Cette valeur est requise. |
   | **Nom du serveur** | Nom complet du serveur |  |
   | **Authentification** | l’authentification SQL Server | |
   | **Connexion** | Compte d’administrateur de serveur | Il s’agit du compte que vous avez spécifié quand vous avez créé le serveur. |
   | **Mot de passe** | Mot de passe de votre compte d’administrateur de serveur | Il s’agit du mot de passe que vous avez spécifié quand vous avez créé le serveur. |

3. Cliquez sur **Connecter**. La fenêtre Explorateur d’objets s’ouvre dans SSMS. 

4. Dans l’Explorateur d’objets, développez **Catalogues Integration Services**, puis développez **SSISDB** pour afficher les objets de la base de données de catalogues SSIS.


## <a name="run-the-t-sql-code"></a>Exécuter le code T-SQL
Exécutez le code Transact-SQL suivant pour déployer un projet SSIS.

1.  Dans SSMS, ouvrez une nouvelle fenêtre de requête et collez le code suivant.

2.  Mettez à jour les valeurs de paramètres dans la procédure stockée `catalog.deploy_project` pour votre système.

3.  Vérifiez que **SSISDB** est la base de données active.

4.  Exécutez le script.

5. Dans l’Explorateur d’objets, actualisez le contenu de **SSISDB** si nécessaire et vérifiez la présence du projet que vous avez déployé.

```sql
DECLARE @ProjectBinary AS varbinary(max)
DECLARE @operation_id AS bigint
SET @ProjectBinary =
    (SELECT * FROM OPENROWSET(BULK '<project_file_path>.ispac', SINGLE_BLOB) AS BinaryData)

EXEC catalog.deploy_project @folder_name = '<target_folder>',
    @project_name = '<project_name',
    @Project_Stream = @ProjectBinary,
    @operation_id = @operation_id out
```

## <a name="next-steps"></a>Étapes suivantes
- Envisagez d’autres méthodes pour déployer un package.
    - [Déployer un package SSIS avec SSMS](./ssis-quickstart-deploy-ssms.md)
    - [Déployer un package SSIS avec Transact-SQL (VS Code)](ssis-quickstart-deploy-tsql-vscode.md)
    - [Déployer un package SSIS à partir de l’invite de commandes](./ssis-quickstart-deploy-cmdline.md)
    - [Déployer un package SSIS avec PowerShell](ssis-quickstart-deploy-powershell.md)
    - [Déployer un package SSIS avec C#](./ssis-quickstart-deploy-dotnet.md) 
- Exécutez un package déployé. Pour exécuter un package, vous pouvez choisir parmi plusieurs outils et langages. Pour plus d’informations, consultez les articles suivants :
    - [Exécuter un package SSIS avec SSMS](./ssis-quickstart-run-ssms.md)
    - [Exécuter un package SSIS avec Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Exécuter un package SSIS avec Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Exécuter un package SSIS à partir de l’invite de commandes](./ssis-quickstart-run-cmdline.md)
    - [Exécuter un package SSIS avec PowerShell](ssis-quickstart-run-powershell.md)
    - [Exécuter un package SSIS avec C#](./ssis-quickstart-run-dotnet.md)