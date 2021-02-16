---
description: Déployer des projets et des packages Integration Services (SSIS)
title: Déployer des projets et des packages SSIS (Integration Services) | Microsoft Docs
ms.custom: ''
ms.date: 09/26/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: vanto
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.bids.converttolegacydeployment.f1
- sql13.ssis.deploymentwizard.f1
- sql13.ssis.ssms.isenvprop.permissions.f1
- sql13.ssis.ssms.isenvprop.general.f1
- sql13.ssis.ssms.iscreateenv.f1
- sql13.ssis.ssms.isenvprop.variables.f1
- sql13.ssis.migrationwizard.f1
ms.assetid: bea8ce8d-cf63-4257-840a-fc9adceade8c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5ee14ee789174ad12db2cea297202fc63ee68d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346952"
---
# <a name="deploy-integration-services-ssis-projects-and-packages"></a>Déployer des projets et des packages Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prend en charge deux modèles de déploiement : le modèle de déploiement de projet et le modèle de déploiement de package hérité. Le modèle de déploiement de projet vous permet de déployer vos projets sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
Pour plus d’informations sur le modèle de déploiement de package hérité, consultez [Déploiement de packages hérités &#40;SSIS&#41;](../../integration-services/packages/legacy-package-deployment-ssis.md).  
  
> [!NOTE]  
>  Le modèle de déploiement du projet a été présenté pour la première fois dans [!INCLUDE[ssISversion11](../../includes/ssisversion11-md.md)]. Avec ce modèle de déploiement, vous ne pouviez pas déployer un ou plusieurs packages sans déployer le projet dans son ensemble. La fonctionnalité de déploiement incrémentiel de packages présentée pour la première fois dans [!INCLUDE[ssISversion13](../../includes/ssisversion13-md.md)] vous permet de déployer un ou plusieurs packages sans déployer la totalité du projet.

> [!NOTE]
> Cet article décrit comment déployer des packages SSIS de manière générale et comment les déployer au niveau local. Vous pouvez également déployer des packages SSIS sur les plateformes suivantes :
> - **Cloud Microsoft Azure**. Pour plus d’informations, consultez [Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).
> - **Linux**. Pour plus d’informations, consultez [Extraire, transformer et charger des données sur Linux avec SSIS](../../linux/sql-server-linux-migrate-ssis.md).

## <a name="compare-project-deployment-model-and-legacy-package-deployment-model"></a>Comparer le modèle de déploiement de projet et le modèle de déploiement de package hérité

 Le type de modèle de déploiement que vous choisissez pour un projet détermine les options de développement et d'administration qui sont disponibles pour ce projet. Le tableau suivant présente les différences et les ressemblances entre l'utilisation du modèle de déploiement de projet et l'utilisation du modèle de déploiement de package.  
  
|En cas d'utilisation du modèle de déploiement de projet|En cas d’utilisation du modèle de déploiement de package hérité|  
|---------------------------------------------|----------------------------------------------------|  
|Un projet est l'unité de déploiement.|Un package est l'unité de déploiement.|  
|Des paramètres sont utilisés pour affecter des valeurs aux propriétés du package.|Des configurations sont utilisées pour affecter des valeurs aux propriétés du package.|  
|Un projet, contenant des packages et des paramètres, est généré dans un fichier de déploiement de projet (extension .ispac).|Les packages (extension .dtsx) et les configurations (extension .dtsConfig) sont enregistrés individuellement dans le système de fichiers.|  
|Un projet, contenant des packages et des paramètres, est déployé dans le catalogue SSISDB sur une instance de SQL Server.|Les packages et les configurations sont copiés dans le système de fichiers sur un autre ordinateur. Les packages peuvent également être enregistrés dans la base de données MSDB sur une instance de SQL Server.|  
|L'intégration du CLR est requise sur le moteur de base de données.|L'intégration du CLR n'est pas requise sur le moteur de base de données.|  
|Les valeurs des paramètres spécifiques à l'environnement sont stockées dans des variables d'environnement.|Les valeurs de la configuration spécifique à l'environnement sont stockées dans des fichiers de configuration.|  
|Les projets et les packages contenus dans le catalogue peuvent être validés sur le serveur avant l'exécution. Vous pouvez effectuer la validation à l'aide de SQL Server Management Studio, de procédures stockées ou de code managé.|Les packages sont validés juste avant l'exécution. Vous pouvez également valider un package avec dtExec ou du code managé.|  
|Les packages sont exécutés en démarrant une exécution sur le moteur de base de données. Un identificateur de projet, des valeurs de paramètre explicites (facultatif) et des références environnementales (facultatif) sont affectés à une exécution avant son démarrage.<br /><br /> Vous pouvez également exécuter des packages à l'aide de **dtExec**.|Les packages sont exécutés à l'aide des utilitaires d'exécution **dtExec** et **DTExecUI** . Les configurations applicables sont identifiées par des arguments d'invite de commandes (facultatif).|  
|Pendant l'exécution, les événements qui sont produits par le package sont automatiquement capturés et sont enregistrés dans le catalogue. Vous pouvez interroger ces événements avec des vues Transact-SQL.|Pendant l'exécution, les événements qui sont produits par un package ne sont pas automatiquement capturés. Un module fournisseur d'informations doit être ajouté au package pour capture les événements.|  
|Les packages sont exécutés dans un processus Windows distinct.|Les packages sont exécutés dans un processus Windows distinct.|  
|L'Agent SQL Server est utilisé pour planifier l'exécution du package.|L'Agent SQL Server est utilisé pour planifier l'exécution du package.|  


## <a name="features-of-project-deployment-model"></a>Fonctionnalités du modèle de déploiement de projet  
 Le tableau suivant répertorie les fonctionnalités disponibles pour les projets développés uniquement pour le modèle de déploiement de projet.  
  
|Fonctionnalité|Description|  
|-------------|-----------------|  
|Paramètres|Un paramètre spécifie les données qui seront utilisées par un package. Vous pouvez définir l'étendue des paramètres au niveau du package ou au niveau du projet avec des paramètres de package et des paramètres de projet, respectivement. Des paramètres peuvent être utilisés dans des expressions ou des tâches. Lorsque le projet est déployé dans le catalogue, vous pouvez affecter une valeur littérale pour chaque paramètre ou utiliser la valeur par défaut qui a été affectée au moment de la conception. Au lieu d'une valeur littérale, vous pouvez également référencer une variable d'environnement. Les valeurs de variable d'environnement sont résolues au moment de l'exécution du package.|  
|Environnements|Un environnement est un conteneur de variables qui peuvent être référencées par les projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Chaque projet peut avoir plusieurs références environnementales, mais une instance d'exécution de package unique ne peut faire référence qu'à des variables d'un environnement unique. Les environnements vous permettent d'organiser les valeurs que vous affectez à un package. Par exemple, vous pouvez avoir des environnements nommés « Dev », « test » et « Production ».|  
|Variables d'environnement|Une variable d'environnement définit une valeur littérale qui peut être affectée à un paramètre pendant l'exécution du package. Pour utiliser une variable d'environnement, créez une référence environnementale (dans le projet qui correspond à l'environnement ayant le paramètre), affectez une valeur de paramètre au nom de la variable d'environnement, puis spécifiez la référence environnementale correspondante lorsque vous configurez une instance d'exécution.|  
|Catalogue SSISDB|Tous les objets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sont stockés et gérés sur une instance de SQL Server dans une base de données appelée catalogue SSISDB. Ce catalogue vous permet d'utiliser des dossiers pour organiser vos projets et environnements. Chaque instance de SQL Server ne peut disposer que d'un seul catalogue. Chaque catalogue peut avoir zéro dossier ou plus. Chaque dossier peut avoir zéro projet ou plus et zéro environnement ou plus. Un dossier du catalogue peut également être utilisé comme limite pour les autorisations sur des objets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
|Procédures stockées et vues du catalogue|Un grand nombre de procédures stockées et de vues peuvent être utilisées pour gérer les objets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] du catalogue. Par exemple, vous pouvez spécifier des valeurs pour des paramètres et des variables d'environnement, créer et démarrer des exécutions, et surveiller des opérations de catalogue. Vous pouvez même afficher exactement la valeur qui sera utilisée par un package avant le démarrage de l'exécution.|  
  
## <a name="project-deployment"></a>Déploiement de projet  
 Au centre du modèle de déploiement de projet se trouve le fichier de déploiement de projet (extension .ispac). Le fichier de déploiement de projet est une unité de déploiement autonome qui inclut uniquement les informations essentielles relatives aux packages et aux paramètres du projet. Le fichier de déploiement de projet ne capture pas toutes les informations contenues dans le fichier projet Integration Services (extension .dtproj). Par exemple, les fichiers texte supplémentaires que vous utilisez pour l'écriture de commentaires ne sont pas stockés dans le fichier de déploiement de projet, et ne sont donc pas déployés dans le catalogue.  

## <a name="permissions-required-to-deploy-ssis-projects-and-packages"></a>Autorisations nécessaires pour déployer des projets et des packages SSIS

Si vous utilisez un autre compte de service SSIS que le compte par défaut, vous devrez peut-être accorder des autorisations supplémentaires à ce compte de service pour pouvoir déployer correctement les packages. Si le compte de service qui n’est pas le compte par défaut n’a pas les autorisations appropriées, le message d’erreur suivant peut s’afficher.

`A .NET Framework error occurred during execution of user-defined routine or aggregate "deploy_project_internal":
System.ComponentModel.Win32Exception: A required privilege is not held by the client.`

Cette erreur est généralement le résultat d’autorisations DCOM manquantes. Pour corriger l’erreur, effectuez les actions suivantes :

1.  Ouvrez la console **Services de composants** (ou exécutez Dcomcnfg.exe).
2.  Dans la console **Services de composants**, développez **Services de composants** > **Ordinateurs** > **Poste de travail** > **Configuration DCOM**.
3.  Dans la liste, recherchez **Microsoft SQL Server Integration Services xx.0** pour la version de SQL Server que vous utilisez. Par exemple, SQL Server 2016 correspond à la version 13.
4.  Effectuez un clic droit et sélectionnez **Propriétés**.
5.  Dans la boîte de dialogue **Propriétés de Microsoft SQL Server Integration Services 13.0**, sélectionnez l’onglet **Sécurité**.
6.  Pour chacun des trois ensembles d’autorisations (Lancement et Activation, Accès et Configuration), sélectionnez **Personnaliser**, puis sélectionnez **Modifier** pour ouvrir la boîte de dialogue **Autorisation**.
7.  Dans la boîte de dialogue **Autorisation**, ajoutez le compte de service qui n’est pas le compte par défaut, puis accordez les autorisations **Autoriser** appropriées. En règle générale, un compte a les autorisations **Exécution locale** et **Activation locale**.
8.  Cliquez sur **OK** à deux reprises, puis fermez la console **Services de composants**.

Pour plus d’informations sur l’erreur décrite dans cette section et sur les autorisations nécessaires au compte de service SSIS, consultez le billet de blog suivant :
 
- [System.ComponentModel.Win32Exception : il manque un privilège obligatoire au client pendant le déploiement d’un projet SSIS](/archive/blogs/dataaccesstechnologies/system-componentmodel-win32exception-a-required-privilege-is-not-held-by-the-client-while-deploying-ssis-project)

## <a name="deploy-projects-to-integration-services-server"></a>Déployer des projets sur le serveur Integration Services
  Dans la version actuelle d’ [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vous pouvez déployer vos projets sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vous permet de gérer les packages, d'exécuter les packages et de configurer les valeurs d'exécution des packages à l'aide d'environnements.  
  
> [!NOTE]  
>  À l’instar des versions antérieures d’ [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], la version actuelle vous permet aussi de déployer vos packages sur une instance de SQL Server et d’utiliser le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour exécuter et gérer les packages. Utilisez le modèle de déploiement de package. Pour plus d’informations, consultez [Déploiement de packages hérités &#40;SSIS&#41;](../../integration-services/packages/legacy-package-deployment-ssis.md).  
  
 Pour déployer un projet sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , effectuez les tâches suivantes :  
  
1.  Créez un catalogue SSISDB, si vous ne l’avez pas encore fait. Pour plus d’informations, consultez [Catalogue SSIS](../../integration-services/catalog/ssis-catalog.md).  
  
2.  Convertissez le projet en modèle de déploiement de projet en exécutant l’**Assistant Conversion de projet Integration Services**. Pour plus d’informations, consultez les instructions suivantes : [Pour convertir un projet en modèle de déploiement de projet](#convert)  
  
    -   Si vous avez créé le projet dans [!INCLUDE[ssISversion12](../../includes/ssisversion12-md.md)] ou une version ultérieure, le projet utilise par défaut le modèle de déploiement de projet.  
  
    -   Si vous avez créé le projet dans une version précédente de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], après avoir ouvert le fichier projet dans [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], convertissez le projet en modèle de déploiement de projet.  
  
        > [!NOTE]  
        >  Si le projet contient une ou plusieurs sources de données, les sources de données sont supprimées quand la conversion du projet est terminée. Pour créer une connexion à une source de données que les packages du projet peuvent partager, ajoutez un gestionnaire de connexions au niveau du projet. Pour plus d’informations, consultez [Ajouter, supprimer ou partager un gestionnaire de connexions dans un package](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130)).  
  
         Selon que vous exécutez **l’Assistant Conversion de projet Integration Services** à partir de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] ou à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l’Assistant effectue différentes tâches de conversion.  
  
        -   Si vous exécutez l'Assistant à partir de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], les packages contenus dans le projet sont convertis de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 2005, 2008 ou 2008 R2 vers le format utilisé par la version en cours de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Les fichiers du projet d'origine (.dtproj) et de package (.dtsx) sont mis à niveau.  
  
        -   Si vous exécutez l’Assistant à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l’Assistant génère un fichier de déploiement de projet (.ispac) à partir des packages et des configurations contenus dans le projet. Les fichiers de package d'origine (.dtsx) ne sont pas mis à niveau.  
  
             Vous pouvez sélectionner un fichier existant ou en créer un dans la page **Destination de la sélection** de l’Assistant.  
  
             Pour mettre à niveau des fichiers de package pendant la conversion d’un projet, exécutez **l’Assistant Conversion de projet Integration Services** à partir de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Pour mettre à niveau des fichiers de package en dehors d’une conversion de projet, exécutez **l’Assistant Conversion de projet Integration Services** à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , puis exécutez **l’Assistant Mise à niveau de packages SSIS**. Si vous mettez à niveau les fichiers de package séparément, assurez-vous d'enregistrer les modifications. À défaut, lorsque vous convertissez le projet en modèle de déploiement de projet, les modifications non enregistrées dans le package ne sont pas converties.  
  
     Pour plus d’informations sur la mise à niveau des packages, consultez [Mettre à niveau des packages Integration Services](../../integration-services/install-windows/upgrade-integration-services-packages.md) et [Mettre à niveau des packages Integration Services à l’aide de l’Assistant Mise à niveau de packages SSIS](../../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md).  
  
3.  Déployez le projet sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d’informations, consultez les instructions ci-dessous : [Pour déployer un projet sur le serveur Integration Services](#deploy).  
  
4.  (Facultatif) Créez un environnement pour le projet déployé. 
  
###  <a name="to-convert-a-project-to-the-project-deployment-model"></a><a name="convert"></a> Pour convertir un projet en modèle de déploiement de projet  
  
1.  Ouvrez le projet dans [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], puis dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **Convertir en modèle de déploiement de projet**.  
  
     - ou -  
  
     Depuis l’Explorateur d’objets dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], cliquez avec le bouton droit sur le nœud **Projets** et sélectionnez **Importer les packages**.  
  
2.  Terminez l'Assistant.
  
###  <a name="to-deploy-a-project-to-the-integration-services-server"></a><a name="deploy"></a> Pour déployer un projet sur le serveur Integration Services  
  
1.  Ouvrez le projet dans [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]puis, dans le menu **Projet** , sélectionnez **Déployer** pour lancer **l’Assistant Déploiement d’Integration Services**.  
  
     ou  
  
     Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] > **SSISDB** dans l’Explorateur d’objets, puis recherchez le dossier du projet que vous souhaitez déployer. Cliquez avec le bouton droit sur le dossier des **projets** , puis cliquez sur **Déployer le projet**.  
  
     ou  
  
     À l’invite de commandes, exécutez **isdeploymentwizard.exe** à partir de **%ProgramFiles%\Microsoft SQL Server\130\DTS\Binn**. Sur les ordinateurs 64 bits, il existe également une version 32 bits de l’outil dans **%ProgramFiles(x86)%\Microsoft SQL Server\130\DTS\Binn**.  
  
2.  Dans la page **Sélectionner une source** , cliquez sur **Fichier de déploiement de projet** pour sélectionner le fichier de déploiement du projet.  
  
     ou  
  
     Cliquez sur **Catalogue Integration Services** pour sélectionner un projet qui a déjà été déployé dans le catalogue SSISDB.  
  
3.  Terminez l'Assistant. 

## <a name="deploy-packages-to-integration-services-server"></a>Déployer des packages sur le serveur Integration Services
  La fonctionnalité Déploiement incrémentiel de packages introduite dans  [!INCLUDE[ssISversion13](../../includes/ssisversion13-md.md)] vous permet de déployer un ou plusieurs packages dans un projet existant ou nouveau sans déployer la totalité du projet.  
  
###  <a name="deploy-packages-by-using-the-integration-services-deployment-wizard"></a><a name="DeployWizard"></a> Déployer des packages à l’aide de l’Assistant Déploiement d’Integration Services  
  
1.  À l’invite de commandes, exécutez **isdeploymentwizard.exe** à partir de **%ProgramFiles%\Microsoft SQL Server\130\DTS\Binn**. Sur les ordinateurs 64 bits, il existe également une version 32 bits de l’outil dans **%ProgramFiles(x86)%\Microsoft SQL Server\130\DTS\Binn**.  
  
2.  Dans la page **Sélectionner une source** , accédez à **Modèle de déploiement de package**. Sélectionnez ensuite le dossier qui contient les packages sources et configurez-les.  
  
3.  Terminez l'Assistant. Suivez le reste de la procédure décrite dans [Package Deployment Model](#PackageModel).  
  
###  <a name="deploy-packages-by-using-sql-server-management-studio"></a><a name="SSMS"></a> Déployer des packages à l’aide de SQL Server Management Studio  
  
1.  Dans SQL Server Management Studio, développez le nœud **Catalogues Integration Services** > **SSISDB** dans l’Explorateur d’objets.  
  
2.  Cliquez avec le bouton droit sur le dossier **Projets** , puis cliquez sur **Déployer des projets**.  
  
3.  Si la page **Introduction** s’affiche, cliquez sur **Suivant** pour continuer.  
  
4.  Dans la page **Sélectionner une source** , accédez à **Modèle de déploiement de package**. Sélectionnez ensuite le dossier qui contient les packages sources et configurez-les.  
  
5.  Terminez l'Assistant. Suivez le reste de la procédure décrite dans [Package Deployment Model](#PackageModel).  
  
###  <a name="deploy-packages-by-using-sql-server-data-tools-visual-studio"></a><a name="SSDT"></a> Déployer des packages à l’aide de SQL Server Data Tools (Visual Studio)  
  
1.  Dans Visual Studio, ouvrez un projet Integration Services si ce n’est déjà fait, et sélectionnez le ou les packages que vous souhaitez déployer.  
  
2.  Cliquez avec le bouton droit et sélectionnez **Déployer le package**. l’Assistant Déploiement s’ouvre avec les packages sélectionnés configurés en tant que packages sources.  
  
3.  Terminez l'Assistant. Suivez le reste de la procédure décrite dans [Package Deployment Model](#PackageModel).  
  
###  <a name="deploy-packages-by-using-the-deploy_packages-stored-procedure"></a><a name="StoredProcedure"></a> Déployer des packages à l’aide de la procédure stockée deploy_packages  
 Vous pouvez utiliser la procédure stockée **[catalog].[deploy_packages]** pour déployer un ou plusieurs packages SSIS dans le catalogue SSIS. l’exemple de code suivant montre comment utiliser cette procédure stockée pour déployer des packages sur un serveur SSIS. Pour plus d’informations, consultez [catalog.deploy_packages](../../integration-services/system-stored-procedures/catalog-deploy-packages.md).  
  
```cs
  
private static void Main(string[] args)  
{  
    // Connection string to SSISDB  
    var connectionString = "Data Source=.;Initial Catalog=SSISDB;Integrated Security=True;MultipleActiveResultSets=false";  
  
    using (var sqlConnection = new SqlConnection(connectionString))  
    {  
        sqlConnection.Open();  
  
        var sqlCommand = new SqlCommand  
        {  
            Connection = sqlConnection,  
            CommandType = CommandType.StoredProcedure,  
            CommandText = "[catalog].[deploy_packages]"  
        };  
  
        var packageData = Encoding.UTF8.GetBytes(File.ReadAllText(@"C:\Test\Package.dtsx"));  
  
        // DataTable: name is the package name without extension and package_data is byte array of package.  
        var packageTable = new DataTable();  
        packageTable.Columns.Add("name", typeof(string));  
        packageTable.Columns.Add("package_data", typeof(byte[]));  
        packageTable.Rows.Add("Package", packageData);  
  
        // Set the destination project and folder which is named Folder and Project.  
        sqlCommand.Parameters.Add(new SqlParameter("@folder_name", SqlDbType.NVarChar, ParameterDirection.Input, "Folder", -1));  
        sqlCommand.Parameters.Add(new SqlParameter("@project_name", SqlDbType.NVarChar, ParameterDirection.Input, "Project", -1));  
        sqlCommand.Parameters.Add(new SqlParameter("@packages_table", SqlDbType.Structured, ParameterDirection.Input, packageTable, -1));  
  
        var result = sqlCommand.Parameters.Add("RetVal", SqlDbType.Int);  
        result.Direction = ParameterDirection.ReturnValue;  
  
        sqlCommand.ExecuteNonQuery();  
    }  
}  
  
```  
  
###  <a name="deploy-packages-using-the-management-object-model-api"></a><a name="MOMApi"></a> Déployer des packages à l’aide de l’API Modèle d’objet de gestion  
 l’exemple de code suivant montre comment utiliser l’API Modèle d’objet de gestion pour déployer des packages sur un serveur.  
  
```cs 
  
static void Main()  
 {  
     // Before deploying packages, make sure the destination project exists in SSISDB.  
     var connectionString = "Data Source=.;Integrated Security=True;MultipleActiveResultSets=false";  
     var catalogName = "SSISDB";  
     var folderName = "Folder";  
     var projectName = "Project";  
  
     // Get the folder instance.  
     var sqlConnection = new SqlConnection(connectionString);  
     var store = new Microsoft.SqlServer.Management.IntegrationServices.IntegrationServices(sqlConnection);  
     var folder = store.Catalogs[catalogName].Folders[folderName];  
  
     // Key is package name without extension and value is package binaries.  
     var packageDict = new Dictionary<string, string>();  
  
     var packageData = File.ReadAllText(@"C:\Folder\Package.dtsx");  
     packageDict.Add("Package", packageData);  
  
     // Deploy package to the destination project.  
     folder.DeployPackages(projectName, packageDict);  
 }  
  
```

## <a name="convert-to-package-deployment-model-dialog-box"></a>Convertir en modèle de déploiement de package, boîte de dialogue
  La commande **Convertir en modèle de déploiement de package** vous permet de convertir un package en modèle de déploiement de package après avoir vérifié la compatibilité du projet et de chaque package du projet avec ce modèle. Si un package utilise des fonctionnalités propres au modèle de déploiement de projet, par exemple des paramètres spécifiques, le package ne peut pas être converti.  

 La conversion d'un package en modèle de déploiement de package requiert deux étapes.  
  
1.  Quand vous sélectionnez la commande **Convertir en modèle de déploiement de package** dans le menu **Projet** , le projet et les packages individuels sont examinés dans le but de garantir leur compatibilité avec ce modèle. Les résultats s’affichent dans le tableau **Résultats** .  
  
     Si le projet ou un package ne réussit pas le test de compatibilité, cliquez sur **Échec** dans la colonne **Résultat** pour obtenir des informations supplémentaires. Cliquez sur **Enregistrer le rapport** pour enregistrer une copie de ces informations dans un fichier texte.  
  
2.  Si le projet et tous les packages réussissent le test de compatibilité, cliquez sur **OK** pour convertir le package.  
  
> [!NOTE]
> Pour convertir un projet en modèle de déploiement de projet, utilisez **l’Assistant Conversion de projet Integration Services**. Pour plus d’informations, consultez [Assistant Conversion de projet Integration Services](deploy-integration-services-ssis-projects-and-packages.md).  

## <a name="integration-services-deployment-wizard"></a>Assistant Déploiement d’Integration Services
  L’ **Assistant Déploiement d’Integration Services** prend en charge deux modèles de déploiement :
   - Le modèle de déploiement de projet
   - le modèle de déploiement de package 
   
 Le **modèle de déploiement de projet** vous permet de déployer un projet SQL Server Integration Services (SSIS) comme une seule unité dans le catalogue SSIS.
 
 Le **modèle de déploiement de package** vous permet de déployer des packages que vous avez mis à jour dans le catalogue SSIS sans avoir besoin de déployer l’ensemble du projet. 
 
 > [!NOTE]
 > Le déploiement par défaut de l’Assistant est le modèle de déploiement de projet.  
  
### <a name="launch-the-wizard"></a>Lancer l'Assistant
Lancer l’Assistant en :

 - En tapant **« Assistant déploiement SQL Server »** dans Windows Search 

 ou

 - En recherchant le fichier exécutable **ISDeploymentWizard.exe** sous le dossier d’installation de SQL Server, par exemple : « C:\Program Files (x86)\Microsoft SQL Server\130\DTS\Binn ». 
 
 > **REMARQUE :** cliquez sur **Suivant** sur la page **Introduction** pour afficher la page **Sélectionner une source** . 
 
 Les paramètres de cette page sont différents pour chaque modèle de déploiement. Suivez les étapes de la section [Project Deployment Model](#ProjectModel) ou de la section [Package Deployment Model](#PackageModel) en fonction du modèle que vous avez sélectionné sur cette page.  
  
###  <a name="project-deployment-model"></a><a name="ProjectModel"></a> Project Deployment Model  
  
#### <a name="select-source"></a>Sélectionner la source

 Pour déployer un fichier de déploiement de projet que vous avez créé, sélectionnez **Fichier de déploiement de projet** , puis entrez le chemin d’accès du fichier .ispac. Pour déployer un projet qui réside dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , sélectionnez **Catalogue Integration Services**, puis entrez le nom du serveur et le chemin d'accès au projet au sein du catalogue. Cliquez sur **Suivant** pour afficher la page **Sélectionner la destination** .  
  
#### <a name="select-destination"></a>Sélectionner la destination

 Pour sélectionner le dossier de destination du projet dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , entrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou cliquez sur **Parcourir** pour sélectionner un serveur dans une liste de serveurs. Entrez le chemin d'accès au projet dans SSISDB ou cliquez sur **Parcourir** pour le sélectionner. Cliquez sur **Suivant** pour afficher la page **Vérifier** .  
  
#### <a name="review-and-deploy"></a>Vérifier (et déployer)

 La page vous permet de vérifier les paramètres que vous avez sélectionnés. Vous pouvez modifier vos sélections en cliquant sur **Précédent** ou en cliquant sur l'une des étapes dans le volet gauche. Cliquez sur **Déployer** pour démarrer le processus de déploiement.  
  
#### <a name="results"></a>Résultats

 Une fois le processus de déploiement terminé, la page **Résultats** doit s’afficher. Cette page indique la réussite ou l’échec de chaque action. Si l'action échoue, cliquez sur **Échec** dans la colonne **Résultat** pour afficher une explication de l'erreur. Cliquez sur **Enregistrer le rapport** pour enregistrer les résultats dans un fichier XML, ou cliquez sur **Fermer** pour quitter l’Assistant.
  
###  <a name="package-deployment-model"></a><a name="PackageModel"></a> Package Deployment Model  
  
#### <a name="select-source"></a>Sélectionner la source

 La page **Sélectionner une source** dans l’ **Assistant Déploiement d’Integration Services** affiche les paramètres spécifiques au modèle de déploiement de package si vous avez sélectionné l’option **Déploiement de package** comme **modèle de déploiement**.  
  
 Pour sélectionner les packages sources, cliquez sur le bouton **Parcourir...** pour sélectionner le **dossier** qui contient les packages ou tapez le chemin du dossier dans la zone de texte **Chemin d’accès du dossier des packages**, puis cliquez sur le bouton **Actualiser** en bas de la page. Tous les packages contenus dans le dossier spécifié doivent s’afficher dans la zone de liste. Par défaut, tous les packages sont sélectionnés. Cliquez sur la **case à cocher** dans la première colonne pour choisir les packages à déployer sur le serveur.  
  
 Consultez les colonnes **État** et **Message** pour vérifier l’état du package. Si l’état est défini sur **Prêt** ou **Avertissement**, l’Assistant Déploiement ne bloquera pas le processus de déploiement. Si l’état est défini sur **Erreur**, l’Assistant ne poursuivra pas le déploiement des packages sélectionnés. Pour afficher les messages d’avertissement ou d’erreur détaillés, cliquez sur le lien affiché dans la colonne **Message**.  
  
 Si les données sensibles ou les données de package sont chiffrées avec un mot de passe, tapez le mot de passe dans la colonne **Mot de passe** et cliquez sur le bouton **Actualiser** pour vérifier si le mot de passe est accepté. Si le mot de passe est correct, l’état passe à **Prêt** et le message d’avertissement disparaît. S’il existe plusieurs packages avec le même mot de passe, sélectionnez les packages ayant le même mot de passe de chiffrement, tapez le mot de passe dans la zone de texte **Mot de passe**, puis sélectionnez le bouton **Appliquer**. Le mot de passe est appliqué aux packages sélectionnés.  
  
 Si l’état de tous les packages sélectionnés n’est pas défini sur **Erreur**, le bouton **Suivant** est activé pour vous permettre de poursuivre le processus de déploiement de package.  
  
#### <a name="select-destination"></a>Sélectionner la destination

 Après avoir sélectionné les sources de package, cliquez sur le bouton **Suivant** pour afficher la page **Sélectionner la destination**. Les packages doivent être déployés dans un projet dans le catalogue SSIS (SSISDB). Avant de déployer des packages, vérifiez que le projet de destination existe déjà dans le catalogue SSIS. Créez un projet vide s’il n’existe aucun projet. Dans la page **Sélectionner la destination**, tapez le nom du serveur dans la zone de texte **Nom du serveur** ou cliquez sur le bouton **Parcourir...** pour sélectionner une instance de serveur. Cliquez ensuite sur le bouton **Parcourir...** en regard de la zone de texte **Chemin** pour spécifier le projet de destination. Si le projet n’existe pas, cliquez sur le bouton **Nouveau projet...** pour créer un projet vide comme projet de destination. Le projet doit être créé dans un dossier.  
  
#### <a name="review-and-deploy"></a>Vérifier et déployer

 Cliquez sur **Suivant** sur la page **Sélectionner la destination** pour afficher la page **Vérifier** dans l’ **Assistant Déploiement d’Integration Services**. Sur la page de vérification, passez en revue le rapport de synthèse sur l’action de déploiement. Après la vérification, cliquez sur le bouton **Déployer** pour exécuter l’action de déploiement.  
  
#### <a name="results"></a>Résultats

 Une fois le déploiement terminé, la page **Résultats** doit s’afficher. Sur la page **Résultats**, passez en revue les résultats de chaque étape du processus de déploiement. Cliquez sur **Enregistrer le rapport** pour enregistrer le rapport de déploiement ou sur **Fermer** pour fermer l’Assistant.  

## <a name="create-and-map-a-server-environment"></a>Créer et mapper un environnement serveur

  Vous créez un environnement serveur pour spécifier les valeurs d’exécution des packages contenus dans un projet que vous avez déployé sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Vous pouvez ensuite mapper les variables d'environnement aux paramètres, pour un package spécifique, pour les packages de point d'entrée, ou pour tous les packages dans un projet donné. Un package de point d'entrée est généralement un package parent qui exécute un package enfant.  
  
> [!IMPORTANT]  
>  Pour une exécution données, un package peut s'exécuter uniquement avec les valeurs contenues dans un seul environnement.  
  
 Vous pouvez interroger les affichages afin d'obtenir la liste des environnements serveur, des références environnementales et des variables d'environnement. Vous pouvez également appeler des procédures stockées pour ajouter, supprimer et modifier des environnements, des références environnementales et des variables d’environnement. Pour plus d’informations, consultez la section **Environnements serveur, variables de serveur et références d’environnement serveur** dans le [catalogue SSIS](../../integration-services/catalog/ssis-catalog.md).  
  
### <a name="to-create-and-use-a-server-environment"></a>Pour créer et utiliser un environnement serveur  
  
1.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], développez le nœud **SSISDB** de Catalogues [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans l’Explorateur d’objets, puis recherchez le dossier **Environnements** du projet pour lequel vous voulez créer un environnement.  
  
2.  Cliquez avec le bouton droit sur le dossier **Environnements** , puis cliquez sur **Créer l’environnement**.  
  
3.  Tapez un nom pour l’environnement et ajoutez éventuellement une description. Cliquez sur **OK**.  
  
4.  Cliquez avec le bouton droit sur le nouvel environnement, puis sélectionnez **Propriétés**.  
  
5.  Dans la page **Variables** , procédez comme suit pour ajouter une variable.  
  
    1.  Sélectionnez le **Type** de la variable. Le nom de la variable ne doit pas nécessairement correspondre au nom du paramètre du projet que vous mappez à la variable.  
  
    2.  Entrez une **Description** facultative pour la variable.  
  
    3.  Entrez la **Valeur** de la variable d'environnement.  
  
         Pour plus d'informations sur les règles énoncées pour les noms de variable d'environnement, consultez la section **Variable d'environnement** dans [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md).  
  
    4.  Indiquez si la variable contient une valeur sensible, en activant ou désactivant la case à cocher **Sensible** .  
  
         Si vous sélectionnez **Sensible**, la valeur de la variable ne s'affiche pas dans le champ **Valeur** .  
  
         Les valeurs sensibles sont chiffrées dans le catalogue SSISDB. Pour plus d'informations sur le chiffrement, consultez [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md).  
  
6.  Dans la page **Autorisations** , accordez ou refusez des autorisations pour les rôles et les utilisateurs sélectionnés en procédant comme suit.  
  
    1.  Cliquez sur **Parcourir**, puis sélectionnez un ou plusieurs utilisateurs et rôles dans la boîte de dialogue **Parcourir tous les principaux** .  
  
    2.  Dans la zone **Connexions ou rôles** , sélectionnez l'utilisateur ou le rôle auquel vous souhaitez accorder ou refuser des autorisations.  
  
    3.  Dans la zone **Explicite**, sélectionnez **Accorder** ou sur **Refuser** en regard de chaque autorisation.  
  
7.  Pour générer un script de l'environnement, cliquez sur **Script**. Par défaut, le script s'affiche dans une nouvelle fenêtre de l'Éditeur de requête.  
  
    > [!TIP]  
    >  Vous devez cliquer sur **Script** après avoir apporté des modifications aux propriétés d’environnement, telles que l’ajout d’une variable, et avant de cliquer sur **OK** dans la boîte de dialogue **Propriétés d’environnement**. Sinon, aucun script n'est généré.  
  
8.  Cliquez sur **OK** pour enregistrer les propriétés de l'environnement.  
  
9. Sous le nœud **SSISDB** dans l’Explorateur d’objets, développez le dossier **Projets** , cliquez avec le bouton droit sur le projet, puis cliquez sur **Configurer**.  
  
10. Dans la page **Références** , cliquez sur **Ajouter** pour ajouter un environnement, puis cliquez sur **OK** pour enregistrer la référence dans l'environnement.  
  
11. Recliquez avec le bouton droit sur le projet, puis cliquez sur **Configurer**.  
  
12. Pour mapper la variable d’environnement à un paramètre que vous avez ajouté au package au moment de la conception ou à un paramètre généré lors de la conversion du projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en modèle de déploiement de projet, effectuez les étapes suivantes :
  
    1.  Dans l'onglet **Paramètres** dans la page **Paramètres** , cliquez sur le bouton Parcourir en regard du champ **Valeur** .  
  
    2.  Cliquez sur **Utiliser la variable d'environnement**, puis sélectionnez la variable d'environnement que vous avez créée.  
  
13. Pour mapper la variable d'environnement à une propriété du gestionnaire de connexions, procédez comme suit. Les paramètres sont automatiquement générés sur le serveur SSIS pour les propriétés du gestionnaire de connexions.  
  
    1.  Sous l’onglet **Gestionnaires de connexions** dans la page **Paramètres**, cliquez sur le bouton **Parcourir** en regard du champ **Valeur**.  
  
    2.  Cliquez sur **Utiliser la variable d'environnement**, puis sélectionnez la variable d'environnement que vous avez créée.  
  
14. Cliquez deux fois sur **OK** pour enregistrer vos modifications.  

## <a name="deploy-and-execute-ssis-packages-using-stored-procedures"></a>Déployer et exécuter des packages SSIS à l'aide de procédures stockées

  Lorsque vous configurez un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] afin d'utiliser le modèle de déploiement de projet, vous pouvez utiliser les procédures stockées du catalogue [!INCLUDE[ssIS](../../includes/ssis-md.md)] pour déployer le projet et pour exécuter des packages. Pour plus d’informations sur le modèle de déploiement de projet, consultez [Déploiement de projets et de packages](deploy-integration-services-ssis-projects-and-packages.md#compare-project-deployment-model-and-legacy-package-deployment-model).  
  
 Vous pouvez également utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour déployer le projet et pour exécuter des packages. Pour plus d’informations, consultez les rubriques de la section **Voir aussi** .  
  
> [!TIP]
>  Vous pouvez facilement créer des instructions Transact-SQL pour les procédures stockées répertoriées dans la procédure ci-dessous, à l'exception de catalog.deploy_project, en procédant comme suit :
> 
>  1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez le nœud **Catalogues Integration Services** dans l’Explorateur d’objets et accédez au package à exécuter.  
> 2.  Cliquez avec le bouton droit sur le package, puis cliquez sur **Exécuter**.  
> 3.  Si nécessaire, définissez les valeurs des paramètres, les propriétés du gestionnaire de connexions, ainsi que les options de l’onglet **Avancé** , telles que le niveau de journalisation.  
> 
>      Pour plus d’informations sur les niveaux de journalisation, consultez [Activer la journalisation des exécutions de package sur le serveur SSIS](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).  
> 4.  Avant de cliquer sur **OK** pour exécuter le package, cliquez sur **Script**. Transact-SQL s’affiche dans une fenêtre de l’Éditeur de requête dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="to-deploy-and-execute-a-package-using-stored-procedures"></a>Pour déployer et exécuter un package à l'aide de procédures stockées  
  
1.  Appelez [catalog.deploy_project &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md) pour déployer le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient le package sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
     Pour récupérer les contenus binaires du fichier de déploiement de projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], pour le paramètre _\@project_stream_, utilisez une instruction SELECT avec la fonction OPENROWSET et le fournisseur d’ensembles de lignes BULK. Le fournisseur d'ensembles de lignes BULK vous permet de lire des données dans un fichier. L'argument SINGLE_BLOB du fournisseur d'ensembles de lignes BULK retourne le contenu du fichier de données sous la forme d'un ensemble de lignes à une seule ligne, une seule colonne de type varbinary (max). Pour plus d’informations, consultez [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
     Dans l’exemple suivant, le projet SSISPackages_ProjectDeployment est déployé dans le dossier SSIS Packages sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Les données binaires sont lues à partir du fichier projet (SSISPackage_ProjectDeployment.ispac) et sont stockées dans le paramètre _\@ProjectBinary de type varbinary(max). La valeur du paramètre _\@ProjectBinary_ est affectée au paramètre _\@project_stream_.  
  
    ```sql
    DECLARE @ProjectBinary as varbinary(max)  
    DECLARE @operation_id as bigint  
    Set @ProjectBinary = (SELECT * FROM OPENROWSET(BULK 'C:\MyProjects\ SSISPackage_ProjectDeployment.ispac', SINGLE_BLOB) as BinaryData)  
  
    Exec catalog.deploy_project @folder_name = 'SSIS Packages', @project_name = 'DeployViaStoredProc_SSIS', @Project_Stream = @ProjectBinary, @operation_id = @operation_id out  
  
    ```  
  
2.  Appelez [catalog.create_execution &#40; base de données SSISDB &#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md) pour créer une instance de l’exécution du package et éventuellement [catalog.set_execution_parameter_value &#40;base de données SSISDB &#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md) pour définir les valeurs de paramètres d’exécution.  
  
     Dans l'exemple suivant, catalog.create_execution crée une instance d'exécution pour le fichier package.dtsx contenu dans le projet SSISPackage_ProjectDeployment. Le projet se trouve dans le dossier SSIS Packages. L'execution_id retourné par la procédure stockée est utilisé dans l'appel à catalog.set_execution_parameter_value. Cette deuxième procédure stockée attribue au paramètre LOGGING_LEVEL la valeur 3 (journalisation verbose) et attribue à un paramètre de package nommé Parameter1 la valeur 1.  
  
     Pour les paramètres tels que LOGGING_LEVEL la valeur d'object_type est 50. Pour les paramètres de package la valeur d'object_type est 30.  
  
    ```sql
    Declare @execution_id bigint  
    EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'SSIS Packages', @project_name=N'SSISPackage_ProjectDeployment', @use32bitruntime=False, @reference_id=1  
  
    Select @execution_id  
    DECLARE @var0 smallint = 3  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0  
  
    DECLARE @var1 int = 1  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=30, @parameter_name=N'Parameter1', @parameter_value=@var1  
  
    GO  
  
    ```  
  
3.  Appelez [catalog.start_execution &#40;base de données SSISDB &#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md) pour exécuter le package.  
  
     Dans l'exemple suivant, un appel à catalog.start_execution est ajouté à Transact-SQL pour démarrer l'exécution du package. L'execution_id retourné par la procédure stockée catalog.create_execution est utilisé.  
  
    ```sql
    Declare @execution_id bigint  
    EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'SSIS Packages', @project_name=N'SSISPackage_ProjectDeployment', @use32bitruntime=False, @reference_id=1  
  
    Select @execution_id  
    DECLARE @var0 smallint = 3  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0  
  
    DECLARE @var1 int = 1  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=30, @parameter_name=N'Parameter1', @parameter_value=@var1  
  
    EXEC [SSISDB].[catalog].[start_execution] @execution_id  
    GO  
  
    ```  
  
### <a name="to-deploy-a-project-from-server-to-server-using-stored-procedures"></a>Pour déployer un projet de serveur à serveur à l'aide de procédures stockées

 Vous pouvez déployer un projet de serveur à serveur à l’aide des procédures stockées [catalog.get_project &#40;base de données SSISDB &#41;](../../integration-services/system-stored-procedures/catalog-get-project-ssisdb-database.md) et [catalog.deploy_project &#40;base de données SSISDB &#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md).  
  
 Vous devez effectuer les opérations suivantes avant d'exécuter les procédures stockées.  
  
-   Créez un objet serveur lié. Pour plus d’informations, consultez [Créer des serveurs liés &#40;moteur de base de données SQL Server&#41;](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md).  
  
     Dans la page **Options du serveur** de la boîte de dialogue **Propriétés du serveur lié** , attribuez à **RPC** et à **Sortie RPC** la valeur **True**. Par ailleurs, attribuez à **Activer la promotion des transactions distribuées pour RPC** la valeur **False**.  
  
-   Vérifiez les paramètres dynamiques du fournisseur que vous avez sélectionné pour le serveur lié : développez le nœud **Fournisseurs** sous **Serveurs liés** dans l’Explorateur d’objets, puis cliquez avec le bouton droit sur le fournisseur et cliquez sur **Propriétés**. Sélectionnez **Activer** en regard de **Paramètre dynamique**.  
  
-   Vérifiez que le Coordinateur de transactions distribuées (DTC, Distributed Transaction Coordinator) est démarré sur les deux serveurs.  
  
 Appelez catalog.get_project pour retourner les données binaires du projet, puis appelez catalog.deploy_project. La valeur retournée par catalog.get_project est insérée dans une variable de table de type varbinary (max). Le serveur lié ne peut pas retourner des résultats qui sont varbinary (max).  
  
 Dans l'exemple suivant, catalog.get_project retourne une valeur binaire pour le projet SSISPackages sur le serveur lié. catalog.deploy_project déploie le projet sur le serveur local, dans un dossier nommé DestFolder.  
  
```sql
declare @resultsTableVar table (  
project_binary varbinary(max)  
)  
  
INSERT @resultsTableVar (project_binary)  
EXECUTE [MyLinkedServer].[SSISDB].[catalog].[get_project] 'Packages', 'SSISPackages'  
  
declare @project_binary varbinary(max)  
select @project_binary = project_binary from @resultsTableVar  
  
exec [SSISDB].[CATALOG].[deploy_project] 'DestFolder', 'SSISPackages', @project_binary  
  
```  

## <a name="integration-services-project-conversion-wizard"></a>Assistant Conversion de projet Integration Services
  **L’Assistant Conversion de projet Integration Services** convertit un projet en modèle de déploiement de projet.  
  
> [!NOTE]  
>  Si le projet contient une ou plusieurs sources de données, les sources de données sont supprimées quand la conversion du projet est terminée. Pour créer une connexion à une source de données pouvant être partagée par les packages du projet, ajoutez un gestionnaire de connexions au niveau du projet. Pour plus d’informations, consultez [Ajouter, supprimer ou partager un gestionnaire de connexions dans un package](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130)).  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir l'Assistant Conversion de projet Integration Services](#open_dialog)  
  
-   [Définir les options sur la page Localiser les packages](#locate)  
  
-   [Définir les options sur la page Sélectionner les package](#selectPackages)  
  
-   [Définir les options sur la page Sélectionner la destination](#destination)  
  
-   [Définir les options sur la page Spécifier les propriétés du projet](#projectProperties)  
  
-   [Définir les options sur la page Mettre à jour la tâche d'exécution de package](#executePackage)  
  
-   [Définir les options sur la page Sélectionner les configurations](#configurations)  
  
-   [Définir les options sur la page Créer des paramètres](#createParameters)  
  
-   [Définir les options sur la page Configurer les paramètres](#configureParameters)  
  
-   [Définir les options de la page Vérifier](#review)  
  
-   [Définir les options de la page Effectuer la conversion](#conversion)  
  
###  <a name="open-the-integration-services-project-conversion-wizard"></a><a name="open_dialog"></a> Ouvrir l'Assistant Conversion de projet Integration Services  
 Pour ouvrir l’Assistant **Conversion de projet Integration Services** , effectuez l’une des opérations suivantes.  
  
-   Ouvrez le projet dans [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], puis dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **Convertir en modèle de déploiement de projet**.  
  
-   À partir de l’Explorateur d’objets dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], cliquez avec le bouton droit sur le nœud **Projets** dans le **Catalogue Integration Services**, puis sélectionnez **Importer les packages**.  
  
 Selon que vous exécutez **l’Assistant Conversion de projet Integration Services** à partir de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] ou à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l’Assistant effectue différentes tâches de conversion.   
  
###  <a name="set-options-on-the-locate-packages-page"></a><a name="locate"></a> Définir les options sur la page Localiser les packages  
  
> [!NOTE]  
>  La page **Localiser les packages** est disponible uniquement quand vous exécutez l’Assistant à partir de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 L’option suivante s’affiche dans la page quand vous sélectionnez **Système de fichiers** dans la liste déroulante **Source** . Sélectionnez cette option quand le package se trouve dans le système de fichiers.  
  
 **Folder**  
 Tapez le chemin d’accès du package ou accédez au package en cliquant sur **Parcourir**.  
  
 Les options suivantes s’affichent dans la page quand vous sélectionnez **Magasin de packages SSIS** dans la liste déroulante **Source**. Pour plus d’informations sur le magasin de packages, consultez [Gestion de packages &#40;Service SSIS&#41;](../../integration-services/service/package-management-ssis-service.md).  
  
 **Serveur**  
 Tapez le nom du serveur ou sélectionnez ce dernier.  
  
 **Folder**  
 Tapez le chemin d’accès du package ou accédez au package en cliquant sur **Parcourir**.  
  
 Les options suivantes s’affichent dans la page quand vous sélectionnez **Microsoft SQL Server** dans la liste déroulante **Source** . Sélectionnez cette option quand le package se trouve dans Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Serveur**  
 Tapez le nom du serveur ou sélectionnez ce dernier.  
  
 **Utiliser l’authentification Windows**  
 Le mode d'authentification Microsoft Windows permet à l'utilisateur de se connecter au moyen d'un compte d'utilisateur Windows. Si vous utilisez l'authentification Windows, vous n'avez pas besoin de fournir un nom d'utilisateur ou un mot de passe.  
  
 **Utiliser l'authentification SQL Server**  
 Quand un utilisateur se connecte avec un nom d’accès et un mot de passe spécifiés à partir d’une connexion non autorisée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentifie la connexion en vérifiant si un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été défini et si le mot de passe spécifié correspond à celui enregistré. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne possède pas de compte de connexion, l'authentification échoue et un message d'erreur est envoyé à l'utilisateur.  
  
 **Nom d’utilisateur**  
 Spécifiez un nom d'utilisateur lorsque vous utilisez l'authentification SQL Server.  
  
 **Mot de passe**  
 Tapez le mot de passe lorsque vous utilisez l'authentification SQL Server.  
  
 **Folder**  
 Tapez le chemin d’accès du package ou accédez au package en cliquant sur **Parcourir**.  
  
###  <a name="set-options-on-the-select-packages-page"></a><a name="selectPackages"></a> Définir les options sur la page Sélectionner les package  
 **Nom du package**  
 Indique le fichier de package.  
  
 **État**  
 Indique si un package est prêt à être converti en modèle de déploiement de projet.  
  
 **Message**  
 Affiche un message associé au package.  
  
 **Mot de passe**  
 Affiche un mot de passe associé au package. Le texte du mot de passe est masqué.  
  
 **Appliquer à la sélection**  
 Cliquez pour appliquer le mot de passe de la zone de texte **Mot de passe** au(x) package(s) sélectionné(s).  
  
 **Actualiser**  
 Actualise la liste des packages.  
  
###  <a name="set-options-on-the-select-destination-page"></a><a name="destination"></a> Définir les options sur la page Sélectionner la destination  
 Dans cette page, spécifiez le nom et le chemin d'accès d'un nouveau fichier de déploiement de projet (.ispac) ou sélectionnez un fichier existant.  
  
> [!NOTE]  
>  La page **Sélectionner la destination** est disponible uniquement quand vous exécutez l’Assistant à partir de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 **Chemin de sortie**  
 Tapez le chemin d’accès du fichier de déploiement ou accédez à ce dernier en cliquant sur **Parcourir**.  
  
 **Nom du projet**  
 Tapez le nom du projet.  
  
 **Niveau de protection**  
 Sélectionnez le niveau de protection. Pour plus d'informations, consultez [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 **Description du projet**  
 Tapez une description facultative du projet.  
  
###  <a name="set-options-on-the-specify-project-properties-page"></a><a name="projectProperties"></a> Définir les options sur la page Spécifier les propriétés du projet  
  
> [!NOTE]  
>  La page **Spécifier les propriétés du projet** est disponible uniquement quand vous exécutez l’Assistant à partir de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
 **Nom du projet**  
 Indique le nom du projet.  
  
 **Niveau de protection**  
 Sélectionnez le niveau de protection des packages contenus dans le projet. Pour plus d’informations sur les niveaux de protection, consultez [Contrôle d’accès pour les données sensibles présentes dans les packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 **Description du projet**  
 Si vous le souhaitez, tapez une description du projet.  
  
###  <a name="set-options-on-the-update-execute-package-task-page"></a><a name="executePackage"></a> Définir les options sur la page Mettre à jour la tâche d'exécution de package  
 Mettez à jour les tâches d'exécution de package contenues dans les packages pour utiliser une référence basée sur un projet. Pour plus d'informations, consultez [Execute Package Task Editor](../control-flow/execute-package-task.md).  
  
 **Package parent**  
 Indique le nom du package qui exécute le package enfant à l'aide de la tâche d'exécution de package.  
  
 **Nom de la tâche**  
 Indique le nom de la tâche d'exécution de package.  
  
 **Référence d'origine**  
 Indique le chemin d'accès actuel du package enfant.  
  
 **Affecter une référence**  
 Sélectionnez un package enfant stocké dans le projet.  
  
###  <a name="set-options-on-the-select-configurations-page"></a><a name="configurations"></a> Définir les options sur la page Sélectionner les configurations  
 Sélectionnez les configurations de package que vous souhaitez remplacer par des paramètres.  
  
 **Package**  
 Indique le fichier de package.  
  
 **Type**  
 Indique le type de configuration, par exemple un fichier de configuration XML.  
  
 **Chaîne de configuration**  
 Indique le chemin d'accès du fichier de configuration.  
  
 **État**  
 Affiche un message d'état pour la configuration. Cliquez sur le message pour afficher l'intégralité du texte du message.  
  
 **Ajouter des configurations**  
 Ajoutez les configurations de package contenues dans d'autres projets à la liste des configurations disponibles que vous souhaitez remplacer à l'aide de paramètres. Vous pouvez sélectionner des configurations stockées dans un système de fichiers ou dans SQL Server.  
  
 **Actualiser**  
 Cliquez pour actualiser la liste des configurations.  
  
 **Supprimer les configurations de tous les packages après la conversion**  
 Il est recommandé de supprimer toutes les configurations de projet en sélectionnant cette option.  
  
 Si vous ne sélectionnez pas cette option, seules les configurations que vous avez sélectionnées pour remplacement à l’aide de paramètres sont supprimées.  
  
###  <a name="set-options-on-the-create-parameters-page"></a><a name="createParameters"></a> Définir les options sur la page Créer des paramètres  
 Sélectionnez le nom et l'étendue du paramètre pour chaque propriété de configuration.  
  
 **Package**  
 Indique le fichier de package.  
  
 **Nom du paramètre**  
 Indique le nom du paramètre.  
  
 **Étendue**  
 Sélectionnez l'étendue du paramètre, package ou projet.  
  
###  <a name="set-options-on-the-configure-parameters-page"></a><a name="configureParameters"></a> Définir les options sur la page Configurer les paramètres  
 **Nom**  
 Indique le nom du paramètre.  
  
 **Étendue**  
 Indique l'étendue du paramètre.  
  
 **Valeur**  
 Indique la valeur du paramètre.  
  
 Cliquez sur le bouton de sélection en regard du champ de valeur pour configurer les propriétés du paramètre.  
  
 Dans la boîte de dialogue **Définir les détails du paramètre** , vous pouvez modifier la valeur de paramètre. Vous pouvez également spécifier si la valeur de paramètre doit être fournie lorsque vous exécutez le package.  
  
 Vous pouvez modifier la valeur dans la page **Paramètres** de la boîte de dialogue **Configurer** de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]en cliquant sur le bouton Parcourir en regard du paramètre. La boîte de dialogue **Définir la valeur du paramètre** s’affiche.  
  
 La boîte de dialogue **Définir les détails du paramètre** indique également le type de données de la valeur de paramètre et l’origine du paramètre.  
  
###  <a name="set-the-options-on-the-review-page"></a><a name="review"></a> Définir les options de la page Vérifier  
 Utilisez la page **Vérifier** pour confirmer l’exactitude des options sélectionnées pour la conversion du projet.  
  
 **Précédent**  
 Cliquez pour modifier une option.  
  
 **Convert**  
 Cliquez pour convertir le projet en modèle de déploiement de projet.  
  
###  <a name="set-the-options-on-the-perform-conversion"></a><a name="conversion"></a> Définir les options de la page Effectuer la conversion  
 La page Effectuer la conversion indique l'état de la conversion du projet.  
  
 **Action**  
 Indique une étape de conversion spécifique.  
  
 **Résultat**  
 Indique l'état de chaque étape de conversion. Pour plus d'informations, cliquez sur le message d'état.  
  
 La conversion de projet n'est pas enregistrée tant que le projet n'est pas enregistré dans [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
 **Enregistrer le rapport**  
 Cliquez pour enregistrer un résumé de la conversion du projet dans un fichier .xml.