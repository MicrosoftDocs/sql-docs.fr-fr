---
title: Se connecter au catalogue SSIS (SSISDB) dans Azure | Microsoft Docs
description: Découvrez les informations de connexion nécessaires pour se connecter au catalogue SSIS (SSISDB) hébergé sur un serveur Azure SQL Database.
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: e5ce8d83ba29843cc14c6798eb4f550eab62e557
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352014"
---
# <a name="connect-to-the-ssis-catalog-ssisdb-in-azure"></a>Se connecter au catalogue SSIS (SSISDB) dans Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Découvrez les informations de connexion nécessaires pour se connecter au catalogue SSIS (SSISDB) hébergé sur un serveur Azure SQL Database. Vous avez besoin des éléments suivants pour vous connecter :
- nom complet du serveur
- nom de la base de données
- informations de connexion 

> [!IMPORTANT]
> À l’heure actuelle, il n’est pas possible de créer la base de données du catalogue SSISDB dans Azure SQL Database indépendamment de la création d’Azure-SSIS Integration Runtime dans Azure Data Factory. Le runtime d’intégration Azure-SSIS est l’environnement d’exécution qui exécute les packages SSIS sur Azure. Pour une procédure pas à pas du processus, consultez [Déployer et exécuter un package SSIS dans Azure](/azure/data-factory/tutorial-create-azure-ssis-runtime-portal). 

## <a name="prerequisites"></a>Prérequis
Avant de commencer, veillez à disposer de la version 17.2 ou ultérieure de SQL Server Management Studio (SSMS). Si la base de données du catalogue SSISDB est hébergée sur SQL Managed Instance, vérifiez que vous avez la version 17.6 ou ultérieure de SSMS. Pour télécharger la dernière version de SSMS, consultez [Télécharger SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="get-the-connection-info-from-the-azure-portal"></a>Obtenir les informations de connexion à partir du portail Azure
1. Connectez-vous au [portail Azure](https://portal.azure.com/).
2. Dans le portail Azure, sélectionnez **Bases de données SQL** dans le menu de gauche, puis sélectionnez la base de données `SSISDB` dans la page **Bases de données SQL**. 
3. Dans la page **Vue d’ensemble** de la base de données `SSISDB`, vérifiez le nom complet du serveur comme indiqué dans l’image suivante. Pointez sur le nom du serveur pour afficher l’option **Cliquer pour copier**.

    ![Informations de connexion au serveur](media/ssis-azure-connect-to-catalog-database/server-name.png) 

4. Si vous avez oublié les informations de connexion du serveur SQL Database, accédez à la page du serveur SQL Database. Vous pourrez y trouver le nom de l’administrateur du serveur et, si nécessaire, réinitialiser le mot de passe.

## <a name="connect-with-ssms"></a>Se connecter à SSMS
1. Ouvrez SQL Server Management Studio.

2. **Connectez-vous au serveur**. Dans la fenêtre **Se connecter au serveur**, entrez les valeurs suivantes :

   | Paramètre       | Valeur suggérée | Description | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Type de serveur** | Moteur de base de données | Cette valeur est requise. |
   | **Nom du serveur** | Nom complet du serveur | Le nom doit être au format suivant : **mysqldbserver.database.windows.net**. |
   | **Authentification** | l’authentification SQL Server | |
   | **Connexion** | Compte d’administrateur de serveur | Il s’agit du compte que vous avez spécifié lorsque vous avez créé le serveur. |
   | **Mot de passe** | Mot de passe de votre compte d’administrateur de serveur | Il s’agit du mot de passe que vous avez spécifié lorsque vous avez créé le serveur. |

    ![Se connecter au serveur avec SSMS](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-1.png)

3. **Connectez-vous à la base de données SSISDB**. Sélectionnez **Options** pour développer la boîte de dialogue **Se connecter au serveur**. Dans la boîte de dialogue **Se connecter au serveur** développée, sélectionnez l’onglet **Propriétés de connexions**. Dans le champ **Se connecter à la base de données**, sélectionnez ou entrez `SSISDB`.

    > [!IMPORTANT]
    > Si vous ne sélectionnez pas `SSISDB` quand vous vous connectez, le catalogue SSIS risque de ne pas s’afficher dans l’Explorateur d’objets.

    ![Sélectionner la base de données SSISDB pour la connexion](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-2.png)

4. Sélectionnez **Connecter**.

5. Dans l’Explorateur d’objets, développez **Catalogues Integration Services**, puis développez **SSISDB** pour afficher les objets de la base de données de catalogues SSIS.

    ![Rechercher la base de données SSISDB dans l’Explorateur d’objets dans SSMS](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-3.png)

## <a name="next-steps"></a>Étapes suivantes
- Déployez un package. Pour plus d’informations, consultez [Déployer un projet SSIS avec SQL Server Management Studio (SSMS)](../ssis-quickstart-deploy-ssms.md).
- Exécutez un package. Pour plus d’informations, consultez [Exécuter un package SSIS avec SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md).
- Planifiez un package. Pour plus d’informations, consultez [Planifier des packages SSIS dans Azure](ssis-azure-schedule-packages.md).