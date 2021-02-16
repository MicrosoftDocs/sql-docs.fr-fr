---
title: Déployer et exécuter un package SSIS dans Azure | Microsoft Docs
description: Découvrez comment déployer un projet SQL Server Integration Services (SSIS) sur le catalogue SSIS sur Azure SQL Database et exécuter un package.
ms.date: 05/22/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: f603b138773ea76fe73ea3d6929ece535779e23c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351993"
---
# <a name="tutorial-deploy-and-run-a-sql-server-integration-services-ssis-package-in-azure"></a>Tutoriel : Déployer et exécuter un package SQL Server Integration Services (SSIS) sur Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Ce tutoriel vous montre comment déployer un projet SQL Server Integration Services sur le catalogue SSIS dans Azure SQL Database, comment exécuter un package dans le runtime d’intégration Azure-SSIS et comment surveiller le package en cours d’exécution.

## <a name="prerequisites"></a>Prérequis

Avant de commencer, vérifiez que vous avez la version 17.2 ou ultérieure de SQL Server Management Studio. Pour télécharger la dernière version de SSMS, consultez [Télécharger SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

Vérifiez également que vous avez configuré la base de données SSISDB dans Azure et que vous avez provisionné le runtime d’intégration Azure-SSIS. Pour plus d’informations sur le provisionnement de SSIS sur Azure, consultez [Déployer des packages SQL Server Integration Services sur Azure](/azure/data-factory/tutorial-deploy-ssis-packages-azure).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Pour Azure SQL Database, obtenez les informations de connexion.

Pour exécuter le package sur Azure SQL Database, obtenez les informations de connexion dont vous avez besoin pour vous connecter à la base de données du catalogue SSIS (SSISDB). Vous avez besoin des informations de connexion et du nom de serveur complet dans les procédures qui suivent.

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
2. Sélectionnez **Bases de données SQL** dans le menu de gauche, puis sélectionnez la base de données SSISDB dans la page **Bases de données SQL**. 
3. Dans la page **Vue d’ensemble** de votre base de données, notez le nom complet du serveur. Pour voir l’option **Cliquer pour copier**, pointez sur le nom du serveur. 
4. Si vous avez oublié vos informations de connexion au serveur Azure SQL Database, accédez à la page du serveur SQL Database pour voir le nom de l’administrateur du serveur. Vous pouvez réinitialiser le mot de passe si nécessaire.

## <a name="connect-to-the-ssisdb-database"></a>Se connecter à la base de données SSISDB

Utilisez SQL Server Management Studio pour vous connecter au catalogue SSIS sur votre serveur Azure SQL Database. Pour obtenir plus d’informations et des captures d’écran, consultez [Se connecter à la base de données de catalogues SSISDB sur Azure](ssis-azure-connect-to-catalog-database.md).

Voici les deux points les plus importants à retenir. Ces étapes sont décrites dans la procédure suivante.
-   Entrez le nom complet du serveur Azure SQL Database au format **mysqldbserver.database.windows.net**.
-   Sélectionnez `SSISDB` comme base de données pour la connexion.

> [!IMPORTANT]
> Un serveur Azure SQL Database écoute sur le port 1433. Si vous tentez de vous connecter à un serveur Azure SQL Database derrière un pare-feu d’entreprise, ce port doit être ouvert dans le pare-feu d’entreprise pour que vous puissiez vous connecter correctement.

1. Ouvrez SQL Server Management Studio.

2. **Connectez-vous au serveur**. Dans la fenêtre **Se connecter au serveur**, entrez les valeurs suivantes :

   | Paramètre       | Valeur suggérée | Description | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Type de serveur** | Moteur de base de données | Cette valeur est requise. |
   | **Nom du serveur** | Nom complet du serveur | Le nom doit être au format suivant : **mysqldbserver.database.windows.net**. Si vous avez besoin du nom du serveur, consultez [Se connecter à la base de données de catalogues SSISDB sur Azure](ssis-azure-connect-to-catalog-database.md). |
   | **Authentification** | l’authentification SQL Server | Vous ne pouvez pas vous connecter à Azure SQL Database avec l’authentification Windows. |
   | **Connexion** | Compte d’administrateur de serveur | Le compte que vous avez spécifié lorsque vous avez créé le serveur. |
   | **Mot de passe** | Mot de passe de votre compte d’administrateur de serveur | Il s’agit du mot de passe que vous avez spécifié quand vous avez créé le serveur. |

3. **Connectez-vous à la base de données SSISDB**. Sélectionnez **Options** pour développer la boîte de dialogue **Se connecter au serveur**. Dans la boîte de dialogue **Se connecter au serveur** développée, sélectionnez l’onglet **Propriétés de connexions**. Dans le champ **Se connecter à la base de données**, sélectionnez ou entrez `SSISDB`.

4. Sélectionnez **Connecter**. La fenêtre Explorateur d’objets s’ouvre dans SSMS. 

5. Dans l’Explorateur d’objets, développez **Catalogues Integration Services**, puis développez **SSISDB** pour afficher les objets de la base de données de catalogues SSIS.

## <a name="deploy-a-project-with-the-deployment-wizard"></a>Déployer un projet avec l’Assistant Déploiement

Pour en savoir plus sur le déploiement de packages et sur l’Assistant Déploiement, consultez [Déployer des projets et des packages Integration Services (SSIS)](../packages/deploy-integration-services-ssis-projects-and-packages.md) et [Assistant Déploiement d’Integration Services](../packages/deploy-integration-services-ssis-projects-and-packages.md#integration-services-deployment-wizard).

> [!NOTE]
> Le déploiement sur Azure prend uniquement en charge le modèle de déploiement de projet.

### <a name="start-the-integration-services-deployment-wizard"></a>Démarrer l’Assistant Déploiement d’Integration Services
1. Dans l’Explorateur d’objets de SSMS, après avoir développé le nœud **Catalogues Integration Services** et le nœud **SSISDB**, développez un dossier de projet.

2.  Sélectionnez le nœud **Projets**.

3.  Cliquez avec le bouton droit sur le nœud **Projets**, puis sélectionnez **Déployer le projet**. L’Assistant Déploiement d’Integration Services s’ouvre. Vous pouvez déployer un projet à partir d’une base de données de catalogues SSIS ou du système de fichiers.

    ![Déployer un projet à partir de SSMS](media/ssis-azure-deploy-run-monitor-tutorial/ssisdb-deploy-project1.png)

    ![La boîte de dialogue de l’Assistant Déploiement de SSIS s’ouvre](media/ssis-azure-deploy-run-monitor-tutorial/ssisdb-deploy-project2.png)

### <a name="deploy-a-project-with-the-deployment-wizard"></a>Déployer un projet avec l’Assistant Déploiement
1. Dans la page **Introduction** de l’Assistant Déploiement, lisez l’introduction. Sélectionnez **Suivant** pour ouvrir la page **Sélectionner la source**.

2. Dans la page **Sélectionner la source**, sélectionnez le projet SSIS existant à déployer.
    -   Pour déployer un fichier de déploiement de projet que vous avez créé, sélectionnez **Fichier de déploiement de projet** , puis entrez le chemin d’accès du fichier .ispac.
    -   Pour déployer un projet qui réside dans un catalogue SSIS, sélectionnez **Catalogue Integration Services**, puis entrez le nom du serveur et le chemin du projet au sein du catalogue. Seuls les projets qui résident dans SSISDB hébergé par SQL Server peuvent être redéployés au cours de cette étape.
    -   Sélectionnez **Suivant** pour voir la page **Sélectionner la destination**.
  
3.  Dans la page **Sélectionner la destination**, sélectionnez la destination du projet.
    -   Entrez le nom complet du serveur au format `<server_name>.database.windows.net`.
    -   Fournissez les informations d’authentification et sélectionnez **Se connecter**.
    -   Sélectionnez ensuite **Parcourir** pour sélectionner le dossier cible dans SSISDB.
    -   Ensuite, sélectionnez **Suivant** pour ouvrir la page **Vérifier**. (Le bouton **Suivant** est activé seulement si vous sélectionnez **Se connecter**.)
  
4.  Dans la page **Vérifier**, vérifiez les paramètres que vous avez sélectionnés.
    -   Vous pouvez changer vos choix en sélectionnant **Précédent**, ou en sélectionnant l’une des étapes du volet gauche.
    -   Sélectionnez **Déployer** pour démarrer le processus de déploiement.

    > [!NOTE]
    > Si le message d’erreur **Il n’y a aucun Worker Agent actif (Fournisseur de données SqlClient .Net)** s’affiche, vérifiez que le Runtime d’intégration Azure-SSIS est en cours d’exécution. Cette erreur se produit si vous essayez de déployer pendant que le Runtime d’intégration Azure-SSIS est à l’état arrêté.

5.  Une fois le processus de déploiement effectué, la page **Résultats** s’affiche. Cette page indique la réussite ou l’échec de chaque action.
    -   En cas d’échec de l’action, sélectionnez **Échec** dans la colonne **Résultat** pour afficher une explication de l’erreur.
    -   Sélectionnez éventuellement **Enregistrer le rapport** pour enregistrer les résultats dans un fichier XML.
    -   Pour quitter l’Assistant, sélectionnez **Fermer**.

## <a name="deploy-a-project-with-powershell"></a>Déployer un projet avec PowerShell

Pour déployer un projet avec PowerShell sur SSISDB sur Azure SQL Database, adaptez le script suivant à vos besoins. Le script énumère les dossiers enfants sous `$ProjectFilePath` et les projets de chaque dossier enfant, puis crée les mêmes dossiers dans SSISDB et déploie les projets dans ces dossiers.

Ce script nécessite l’installation de SQL Server Data Tools version 17.x ou de SQL Server Management Studio sur l’ordinateur sur lequel vous exécutez le script.

```powershell
# Variables
$ProjectFilePath = "C:\<folder>"
$SSISDBServerEndpoint = "<servername>.database.windows.net"
$SSISDBServerAdminUserName = "<username>"
$SSISDBServerAdminPassword = "<password>"

# Load the IntegrationServices Assembly
[System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.Management.IntegrationServices") | Out-Null;

# Store the IntegrationServices Assembly namespace to avoid typing it every time
$ISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"

Write-Host "Connecting to server ..."

# Create a connection to the server
$sqlConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword + ";Initial Catalog=SSISDB"
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString

# Create the Integration Services object
$integrationServices = New-Object $ISNamespace".IntegrationServices" $sqlConnection

# Get the catalog
$catalog = $integrationServices.Catalogs['SSISDB']

write-host "Enumerating all folders..."

$folders = ls -Path $ProjectFilePath -Directory

if ($folders.Count -gt 0)
{
    foreach ($filefolder in $folders)
    {
        Write-Host "Creating Folder " + $filefolder.Name + " ..."

        # Create a new folder
        $folder = New-Object $ISNamespace".CatalogFolder" ($catalog, $filefolder.Name, "Folder description")
        $folder.Create()

        $projects = ls -Path $filefolder.FullName -File -Filter *.ispac
        if ($projects.Count -gt 0)
        {
            foreach($projectfile in $projects)
            {
                $projectfilename = $projectfile.Name.Replace(".ispac", "")
                Write-Host "Deploying " + $projectfilename + " project ..."

                # Read the project file, and deploy it to the folder
                [byte[]] $projectFileContent = [System.IO.File]::ReadAllBytes($projectfile.FullName)
                $folder.DeployProject($projectfilename, $projectFileContent)
            }
        }
    }
}

Write-Host "All done." 
```

## <a name="run-a-package"></a>Exécuter un package

1. Dans l’Explorateur d’objets de SSMS, sélectionnez le package à exécuter.

2. Cliquez avec le bouton droit et sélectionnez **Exécuter** pour ouvrir la boîte de dialogue **Exécuter le package**.

3.  Dans la boîte de dialogue **Exécuter le package**, configurez l’exécution du package à l’aide des paramètres situés sous les onglets **Paramètres**, **Gestionnaires de connexions** et **Avancé**.

4.  Sélectionnez **OK** pour exécuter le package.

## <a name="monitor-the-running-package-in-ssms"></a>Surveiller l’exécution du package dans SSMS

Pour afficher l’état des opérations Integration Services en cours d’exécution sur le serveur Integration Services, par exemple le déploiement, la validation et l’exécution du package, utilisez la boîte de dialogue **Opérations actives** dans SSMS. Pour ouvrir la boîte de dialogue **Opérations actives**, cliquez avec le bouton droit sur **SSISDB**, puis sélectionnez **Opérations actives**.

Vous pouvez également sélectionner un package dans l’Explorateur d’objets, cliquer avec le bouton droit et sélectionner **Rapports**, **Rapports standard**, puis **Toutes les exécutions**.

Pour plus d’informations sur la surveillance des packages en cours d’exécution dans SSMS, consultez la section [Surveiller les packages en cours d’exécution et autres opérations](../performance/monitor-running-packages-and-other-operations.md).

## <a name="monitor-the-execute-ssis-package-activity"></a>Surveiller l’activité Exécuter le package SSIS

Si vous exécutez un package dans le cadre d’un pipeline Azure Data Factory avec l’activité Exécuter le package SSIS, vous pouvez surveiller les exécutions du pipeline dans l’interface utilisateur de Data Factory. Vous pouvez obtenir l’ID de l’exécution de SSISDB à partir de la sortie de l’exécution de l’activité, puis l’utiliser pour examiner des messages d’erreur et des journaux d’exécution plus complets dans SSMS.

![Obtenir l’ID d’exécution de package dans Data Factory](media/ssis-azure-deploy-run-monitor-tutorial/get-execution-id.png)

## <a name="monitor-the-azure-ssis-integration-runtime"></a>Surveiller le runtime d’intégration Azure SSIS

Pour obtenir des informations d’état sur le runtime d’intégration Azure-SSIS dans lequel les packages sont en cours d’exécution, utilisez les commandes PowerShell suivantes. Pour chacune des commandes, spécifiez les noms de la fabrique de données, du runtime d’intégration Azure-SSIS et du groupe de ressources.

Pour plus d’informations, consultez [Surveiller le runtime d’intégration Azure-SSIS](/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime).

### <a name="get-metadata-about-the-azure-ssis-integration-runtime"></a>Obtenir des métadonnées sur le runtime d’intégration Azure SSIS

```powershell
Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSsisIRName -ResourceGroupName $ResourceGroupName
```

### <a name="get-the-status-of-the-azure-ssis-integration-runtime"></a>Obtenir l’état du runtime d’intégration Azure SSIS

```powershell
Get-AzDataFactoryV2IntegrationRuntime -Status -DataFactoryName $DataFactoryName -Name $AzureSsisIRName -ResourceGroupName $ResourceGroupName
```

## <a name="next-steps"></a>Étapes suivantes
- Apprenez à planifier l’exécution du package. Pour plus d’informations, consultez [Planifier l’exécution d’un package SSIS sur Azure](ssis-azure-schedule-packages.md)
