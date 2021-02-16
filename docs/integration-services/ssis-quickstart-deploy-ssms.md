---
title: Déployer un projet SSIS avec SSMS
description: Déployer un projet SSIS avec SQL Server Management Studio (SSMS)
ms.topic: quickstart
ms.prod: sql
ms.technology: integration-services
f1_keywords:
- sql13.swb.deploymentwizard.deploymentsettings.f1
author: chugugrace
ms.author: chugu
ms.reviewer: ''
ms.custom: ''
ms.date: 05/21/2018
ms.openlocfilehash: 3749daf45eb51cda3e37d2bd3d31105fcb23c1c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354220"
---
# <a name="deploy-an-ssis-project-with-sql-server-management-studio-ssms"></a>Déployer un projet SSIS avec SQL Server Management Studio (SSMS)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Ce guide de démarrage rapide montre comment utiliser SQL Server Management Studio (SSMS) pour se connecter à la base de données du catalogue SSIS, puis exécuter l’Assistant Déploiement d’Integration Services pour déployer un projet SSIS sur le catalogue SSIS. 

SQL Server Management Studio est un environnement intégré pour la gestion des infrastructures SQL, de SQL Server à SQL Database. Pour plus d’informations sur SSMS, consultez [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md).

## <a name="prerequisites"></a>Prérequis

Avant de commencer, vérifiez que vous disposez de la dernière version de SQL Server Management Studio. Pour télécharger SSMS, consultez [Télécharger SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).

La validation décrite dans cet article pour le déploiement sur Azure SQL Database nécessite SQL Server Data Tools (SSDT) version 17.4 ou ultérieure. Pour obtenir la dernière version de SSDT, consultez [Télécharger SSDT (SQL Server Data Tools)](../ssdt/download-sql-server-data-tools-ssdt.md).

Un serveur Azure SQL Database écoute sur le port 1433. Si vous essayez de vous connecter à un serveur Azure SQL Database en étant derrière un pare-feu d’entreprise, ce port doit être ouvert dans le pare-feu d’entreprise pour que vous puissiez vous connecter.

## <a name="supported-platforms"></a>Plateformes prises en charge

Vous pouvez utiliser les informations de ce guide de démarrage rapide pour déployer un projet SSIS sur les plateformes suivantes :

-   SQL Server sur Windows.

-   Azure SQL Database. Pour plus d’informations sur le déploiement et l’exécution de packages dans Azure, consultez [Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Vous ne pouvez pas utiliser les informations de ce guide de démarrage rapide pour déployer un package SSIS sur SQL Server sur Linux. Pour plus d’informations sur l’exécution de packages sur Linux, consultez [Extraire, transformer et charger des données sur Linux avec SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Pour Azure SQL Database, obtenez les informations de connexion.

Pour déployer le projet sur Azure SQL Database, obtenez les informations de connexion dont vous avez besoin pour vous connecter à la base de données du catalogue SSIS (SSISDB). Vous avez besoin des informations de connexion et du nom de serveur complet dans les procédures qui suivent.

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
2. Sélectionnez **Bases de données SQL** dans le menu de gauche, puis sélectionnez la base de données SSISDB dans la page **Bases de données SQL**. 
3. Dans la page **Vue d’ensemble** de votre base de données, notez le nom complet du serveur. Pour voir l’option **Cliquer pour copier**, pointez sur le nom du serveur. 
4. Si vous avez oublié vos informations de connexion au serveur Azure SQL Database, accédez à la page du serveur SQL Database pour voir le nom de l’administrateur du serveur. Vous pouvez réinitialiser le mot de passe si nécessaire.

## <a name="authentication-methods-for-deployment"></a>Méthodes d’authentification pour le déploiement

Si vous déployez sur un serveur SQL Server avec l’Assistant Déploiement, vous devez utiliser l’authentification Windows. Vous ne pouvez pas utiliser l’authentification SQL Server.

Si vous déployez sur un serveur Azure SQL Database, vous devez utiliser l’authentification SQL Server ou l’authentification Azure Active Directory. Vous ne pouvez pas utiliser l’authentification Windows.

## <a name="connect-to-the-ssisdb-database"></a>Se connecter à la base de données SSISDB

Utilisez SQL Server Management Studio pour établir une connexion au catalogue SSIS. 

1. Ouvrez SQL Server Management Studio.

2. Dans la fenêtre **Se connecter au serveur**, entrez les valeurs suivantes :

   | Paramètre       | Valeur suggérée | En savoir plus | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Type de serveur** | Moteur de base de données | Cette valeur est requise. |
   | **Nom du serveur** | Nom complet du serveur | Si vous vous connectez à un serveur Azure SQL Database, le nom est au format suivant : `<server_name>.database.windows.net`. |
   | **Authentification** | l’authentification SQL Server | Avec l’authentification SQL Server, vous pouvez vous connecter à SQL Server ou à Azure SQL Database. Consultez [Méthodes d’authentification dans le déploiement](#authentication-methods-for-deployment) dans cet article. |
   | **Connexion** | Compte d’administrateur de serveur | Il s’agit du compte que vous avez spécifié quand vous avez créé le serveur. |
   | **Mot de passe** | Mot de passe de votre compte d’administrateur de serveur | Il s’agit du mot de passe que vous avez spécifié quand vous avez créé le serveur. |

3. Cliquez sur **Connecter**. La fenêtre Explorateur d’objets s’ouvre dans SSMS. 

4. Dans l’Explorateur d’objets, développez **Catalogues Integration Services**, puis développez **SSISDB** pour afficher les objets de la base de données de catalogues SSIS.

## <a name="start-the-integration-services-deployment-wizard"></a>Démarrer l’Assistant Déploiement d’Integration Services
1. Dans l’Explorateur d’objets, après avoir développé les nœuds **Catalogues Integration Services** et **SSISDB**, développez un dossier.

2.  Sélectionnez le nœud **Projets**.

3.  Cliquez avec le bouton droit sur le nœud **Projets**, puis sélectionnez **Déployer le projet**. L’Assistant Déploiement d’Integration Services s’ouvre. Vous pouvez déployer un projet à partir du catalogue actif ou du système de fichiers.

## <a name="deploy-a-project-with-the-wizard"></a>Déployer un projet avec l’Assistant
1. Dans la page **Introduction** de l’Assistant, examinez l’introduction. Cliquez sur **Suivant** pour ouvrir la page **Sélectionner la source**.

2. Dans la page **Sélectionner la source**, sélectionnez le projet SSIS existant à déployer.
    -   Pour déployer un fichier de déploiement de projet que vous avez créé en générant un projet dans l’environnement de développement, sélectionnez **Fichier de déploiement de projet** et entrez le chemin du fichier .ispac.
    -   Pour déployer un projet déjà déployé dans une base de données du catalogue SSIS, sélectionnez **Catalogue Integration Services** et entrez le nom du serveur et le chemin du projet dans le catalogue.
    Cliquez sur **Suivant** pour afficher la page **Sélectionner la destination** .
  
3.  Dans la page **Sélectionner la destination**, sélectionnez la destination du projet.
    -   Entrez le nom complet du serveur. Si le serveur cible est un serveur Azure SQL Database, le nom est au format suivant : `<server_name>.database.windows.net`.
    -   Fournissez les informations d’authentification et sélectionnez **Se connecter**. Consultez [Méthodes d’authentification dans le déploiement](#authentication-methods-for-deployment) dans cet article.
    -   Sélectionnez ensuite **Parcourir** pour sélectionner le dossier cible dans SSISDB.
    -   Ensuite, sélectionnez **Suivant** pour ouvrir la page **Vérifier**. (Le bouton **Suivant** est activé seulement si vous sélectionnez **Se connecter**.)
  
4.  Dans la page **Vérifier**, vérifiez les paramètres que vous avez sélectionnés.
    -   Vous pouvez modifier vos sélections en cliquant sur **Précédent** ou en cliquant sur l'une des étapes dans le volet gauche.
    -   Cliquez sur **Déployer** pour démarrer le processus de déploiement.

5.  Si vous déployez sur un serveur Azure SQL Database, la page **Valider** s’ouvre et inspecte les packages du projet à la recherche de problèmes connus susceptibles d’impacter leur exécution dans Azure-SSIS Integration Runtime. Pour plus d’informations, consultez [Valider des packages SSIS déployés sur Azure](lift-shift/ssis-azure-validate-packages.md).

6.  Une fois le processus de déploiement effectué, la page **Résultats** s’affiche. Cette page indique la réussite ou l’échec de chaque action.
    -   Si l’action a échoué, cliquez sur **Échec** dans la colonne **Résultat** pour afficher une explication de l’erreur.
    -   Si vous le souhaitez, cliquez sur **Enregistrer le rapport** pour enregistrer les résultats dans un fichier XML.
    -   Cliquez sur **Fermer** pour quitter l’Assistant.

## <a name="next-steps"></a>Étapes suivantes
- Envisagez d’autres méthodes pour déployer un package.
    - [Déployer un package SSIS avec Transact-SQL (SSMS)](./ssis-quickstart-deploy-tsql-ssms.md)
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