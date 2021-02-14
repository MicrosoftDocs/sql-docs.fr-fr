---
title: exécuter des tests unitaires SQL Server en utilisant Team Foundation Build
description: Découvrez comment exécuter des tests unitaires SQL Server en utilisant Team Foundation Build. Découvrez comment créer une définition de build et exécuter des tests unitaires dans une série de tests automatisés.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 24f5b85d-d6f9-415f-b09f-933b78dc0b67
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2b87720ba7a4cbdb531cd19fe23be7d48d9a07ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018129"
---
# <a name="how-to-run-sql-server-unit-tests-from-team-foundation-build"></a>Procédure : exécuter des tests unitaires SQL Server en utilisant Team Foundation Build

Utilisez Team Foundation Build pour exécuter vos tests unitaires SQL Server dans un test de vérification de la génération. Configurez les tests unitaires pour déployer la base de données, générer des données de test, puis exécuter les tests sélectionnés. Si vous n'êtes pas familiarisé avec Team Foundation Build, vous devez vérifier les informations suivantes avant de suivre les procédures de cette rubrique :  
  
-   [Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
  
-   [Procédure : configurer et exécuter des tests planifiés après avoir généré votre application](/previous-versions/visualstudio/visual-studio-2010/ms182465(v=vs.100))  
  
-   [Créer une définition de build de base](/previous-versions/visualstudio/visual-studio-2010/ms181716(v=vs.100))  
  
Avant d'utiliser ces procédures, vous devez d'abord configurer l'environnement de travail en effectuant les tâches suivantes :  
  
-   Installez Team Foundation Build et le contrôle de version Team Foundation. Vous devrez probablement installer Team Foundation Build et le contrôle de version Team Foundation sur des ordinateurs distincts.  
  
-   Installez MicrosoftSQL Server Data Tools Build Utilities sur le même ordinateur que Team Foundation Build. Pour installer SQL Server Data Tools Build Utilities, commencez par créer un point d'installation d'administration. Pour plus d'informations sur un point d'installation d'administration, consultez [Installer Microsoft SQL Server Data Tools](./download-sql-server-data-tools-ssdt.md). Ensuite, installez SSDTBuildUtilties.msi sur le serveur de build à partir de l'emplacement (/location) utilisé pour le point d'installation d'administration.  
  
-   Connectez-vous à une instance de Visual Studio Team Foundation Server.  
  
Après avoir configuré votre environnement de travail, vous devez ensuite suivre ces étapes :  
  
1.  Créer un projet de base de données.  
  
2.  Importer ou créer le schéma et les objets du projet de base de données.  
  
3.  Configurer les propriétés de projet de base de données pour la génération et le déploiement.  
  
4.  Créer un ou plusieurs tests unitaires.  
  
5.  Ajouter la solution qui contient le projet de base de données et le projet de test unitaire au contrôle de version, puis archiver tous les fichiers.  
  
Les procédures de cette rubrique décrivent comment créer une définition de build pour exécuter vos tests unitaires dans une série de tests automatisée :  
  
1.  [Configurer les paramètres de test pour exécuter des tests unitaires de base de données sur un agent de build x64](#ConfigureX64)  
  
2.  [Affecter des tests à une catégorie de test (facultatif)](#CreateATestList)  
  
3.  [Modifier le projet de test](#ModifyTestProject)  
  
4.  [Archiver la solution](#CheckInTheTestList)  
  
5.  [Créer une définition de build](#CreateBuildDef)  
  
6.  [Exécuter la nouvelle définition de build](#RunBuild)  
  
**Exécution de tests unitaires SQL Server sur un ordinateur de build**  
  
Lorsque vous exécutez des tests unitaires sur un ordinateur de build, les tests unitaires peuvent se retrouver dans l'incapacité de trouver les fichiers projet de base de données (.sqlproj). Ce problème est dû au fait que le fichier app.config référence ces fichiers en utilisant des chemins d'accès relatifs. En outre, les tests unitaires peuvent échouer s'ils ne trouvent pas l'instance de SQL Server que vous souhaitez utiliser pour exécuter les tests unitaires. Ce problème peut se produire si les chaînes de connexion stockées dans le fichier app.config ne sont pas valides sur l'ordinateur de build.  
  
Pour résoudre ces problèmes, vous devez spécifier une section de remplacement dans le fichier app.config qui remplace le fichier app.config par un fichier de configuration spécifique à l'environnement Team Foundation Build. Pour plus d'informations, consultez [Modifier le projet de test](#ModifyTestProject) plus loin dans cette rubrique.  
  
## <a name="configure-test-settings-to-run-sql-server-unit-tests-on-an-x64-build-agent"></a><a name="ConfigureX64"></a>Configurer les paramètres de test pour exécuter des tests unitaires SQL Server sur un agent de build x64  
Pour pouvoir exécuter des tests unitaires sur un agent de build x64, vous devez configurer les paramètres de test de façon à modifier la plateforme de processus hôte.  
  
#### <a name="to-specify-the-host-process-platform"></a>Pour spécifier la plateforme de processus hôte  
  
1.  Ouvrez la solution qui contient le projet de test pour lequel vous souhaitez configurer les paramètres.  
  
2.  Dans l'**Explorateur de solutions**, dans le dossier **Éléments de solution**, double-cliquez sur le fichier **Local.testsettings**.  
  
    La boîte de dialogue **Paramètres de test** s'affiche.  
  
3.  Dans la liste, cliquez sur **Hôtes**.  
  
4.  Dans le volet d'informations, dans **Plateforme de processus hôte**, cliquez sur **MSIL** pour configurer les tests de façon à ce qu'ils s'exécutent sur un agent de build x64.  
  
5.  Cliquez sur **Appliquer**.  
  
## <a name="assign-tests-to-a-test-category-optional"></a><a name="CreateATestList"></a>Affecter des tests à une catégorie de test (facultatif)  
En règle générale, lorsque vous créez une définition de build pour effectuer des tests unitaires, vous spécifiez une ou plusieurs catégories de test. Tous les tests des catégories spécifiées sont exécutés lors de l'exécution de la build.  
  
#### <a name="to-assign-tests-to-a-test-category"></a>Pour affecter des tests à une catégorie de test  
  
1.  Ouvrez la fenêtre **Affichage des tests**.  
  
2.  Sélectionnez un test.  
  
3.  Dans le volet des propriétés, cliquez sur **Catégories de test**, puis cliquez sur le bouton de sélection (…) dans la colonne la plus à droite.  
  
4.  Dans la fenêtre **Catégorie de test**, dans la zone **Ajouter une catégorie**, tapez un nom pour la nouvelle catégorie de test.  
  
5.  Cliquez sur **Ajouter**, puis sur **OK**.  
  
    La nouvelle catégorie de test sera affectée à votre test et disponible pour les autres tests par le biais de leurs propriétés.  
  
## <a name="modify-the-test-project"></a><a name="ModifyTestProject"></a>Modifier le projet de test  
Par défaut, Team Foundation Build crée un fichier de configuration à partir du fichier app.config du projet lorsqu'il génère le projet de tests unitaires. Le chemin d'accès au projet de base de données est stocké en tant que chemin d'accès relatif dans le fichier app.config. Les chemins d'accès relatifs qui fonctionnent dans Visual Studio ne fonctionneront pas, car Team Foundation Build place les fichiers créés à différents emplacements relatifs dans lesquels vous exécutez des tests unitaires. En outre, le fichier app.config contient les chaînes de connexion qui spécifient la base de données à tester. Vous avez également besoin d'un fichier app.config distinct pour Team Foundation Build si les tests unitaires doivent se connecter à une base de données différente de celle utilisée lorsque le projet de test a été créé. En apportant les modifications dans la procédure suivante, configurez le projet de test et le serveur de build de façon à ce que Team Foundation Build utilise une autre configuration.  
  
> [!IMPORTANT]  
> Vous devez effectuer cette procédure pour chaque projet de test (.vbproj ou .vsproj).  
  
#### <a name="to-specify-an-appconfig-file-for-team-foundation-build"></a>Pour spécifier un fichier app.config pour Team Foundation Build  
  
1.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le fichier app.config, puis cliquez sur **Copier**.  
  
2.  Cliquez avec le bouton droit sur le projet de test, puis cliquez sur **Coller**.  
  
3.  Cliquez avec le bouton droit sur le fichier nommé **Copie de app.config**, puis cliquez sur Renommer.  
  
4.  Tapez _BuildComputer_ **.sqlunitttest.config**, puis appuyez sur Entrée, où *BuildComputer* est le nom de l’ordinateur sur lequel l’agent de build s’exécute.  
  
5.  Double-cliquez sur *BuildComputer*.sqlunitttest.config.  
  
    Le fichier de configuration s'ouvre dans l'éditeur.  
  
6.  Modifiez le chemin d'accès relatif au fichier .sqlproj en ajoutant un niveau de dossier pour le dossier Sources et un sous-dossier du même nom que la solution. Par exemple, si le fichier de configuration contient initialement l'entrée suivante :  
  
    ```  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database3\Database3.sqlproj"      Configuration="Debug" />  
    ```  
  
    Mettez le fichier à jour comme suit :  
  
    ```  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database3\Database3.sqlproj"      Configuration="Debug" />  
    ```  
  
    Lorsque vous avez terminé, le fichier *BuildComputer*.sqlunitttest.config doit ressembler à l'exemple suivant pour Visual Studio 2010 :  
  
    ```  
    <SqlUnitTesting_VS2010>  
        <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database4\Database4.sqlproj"  
            Configuration="Debug" />  
        <DataGeneration ClearDatabase="true" />  
        <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
            CommandTimeout="30" />  
        <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
            CommandTimeout="30" />  
    </SqlUnitTesting_VS2010>  
    ```  
  
    Ou, si vous utilisez Visual Studio 2012 :  
  
    ```  
    <SqlUnitTesting_VS2012>  
            <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database4\Database4.sqlproj"  
                Configuration="Debug" />  
            <DataGeneration ClearDatabase="true" />  
            <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
                CommandTimeout="30" />  
            <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
                CommandTimeout="30" />  
        </SqlUnitTesting_VS2012>  
    ```  
  
7.  Mettez à jour l'attribut ConnectionString de ExecutionContext et PrivilegedContext pour spécifier les connexions à la base de données cible sur laquelle vous souhaitez effectuer le déploiement.  
  
8.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
9. Dans l'Explorateur de solutions, double-cliquez sur app.config.  
  
10. Dans l’éditeur, pour chaque nœud \<SqlUnitTesting_*VSVersion*>, ajoutez `AllowConfigurationOverride="true"`. Par exemple :  
  
    ```  
    -- Update SqlUnitTesting_VS2010 node to:  
    <SqlUnitTesting_VS2010 AllowConfigurationOverride="true">   
  
    -- Update SqlUnitTesting_VS2012 node to:  
    <SqlUnitTesting_VS2012 AllowConfigurationOverride="true">  
    ```  
  
    En apportant cette modification, vous permettez à Team Foundation Build d'utiliser le fichier de configuration de remplacement créé.  
  
11. Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
    Ensuite, vous devez mettre à jour Local.testsettings pour inclure le fichier de configuration personnalisé.  
  
#### <a name="to-customize-localtestsettings-to-deploy-the-customized-configuration-file"></a>Pour personnaliser Local.testsettings de façon à déployer le fichier de configuration personnalisé  
  
1.  Dans l'Explorateur de solutions, double-cliquez sur Local.testsettings.  
  
    La boîte de dialogue **Paramètres de test** s'affiche.  
  
2.  Dans la liste de catégories, cliquez sur **Déploiement**.  
  
3.  Activez la case à cocher **Activer le déploiement**.  
  
4.  Cliquez sur **Ajouter un fichier**.  
  
5.  Dans la boîte de dialogue **Ajouter des fichiers de déploiement**, spécifiez le fichier *BuildComputer*.sqlunitttest.config créé.  
  
6.  Cliquez sur **Appliquer**.  
  
7.  Cliquez sur **Fermer**.  
  
8.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
    Ensuite, archivez votre solution dans le contrôle de version.  
  
## <a name="check-in-the-solution"></a><a name="CheckInTheTestList"></a>Archiver la solution  
Dans cette procédure, vous archivez tous les fichiers de la solution. Ces fichiers incluent le fichier de métadonnées de test de votre solution, qui contient les associations de catégorie de test et les tests. Chaque fois que vous ajoutez, supprimez, réorganisez ou modifier le contenu des tests, votre fichier de métadonnées de test est automatiquement mis à jour pour refléter ces modifications.  
  
> [!NOTE]  
> Cette procédure décrit les étapes si vous utilisez le contrôle de version Team Foundation. Si vous utilisez un logiciel différent de contrôle de version, vous devez suivre les étapes appropriées pour le logiciel.  
  
#### <a name="to-check-in-the-solution"></a>Pour archiver la solution  
  
1.  Connectez-vous à un ordinateur exécutant Team Foundation Server.  
  
    Pour plus d'informations, consultez [Utilisation de l'Explorateur du contrôle de code source](/previous-versions/visualstudio/visual-studio-2010/ms181370(v=vs.100)).  
  
2.  Si votre solution ne figure pas encore dans le contrôle de code source, ajoutez-la au contrôle de code source.  
  
    Pour plus d'informations, consultez [Ajouter un projet ou une solution au contrôle de version](/previous-versions/visualstudio/visual-studio-2010/ms181374(v=vs.100)).  
  
3.  Cliquez sur **Affichage**, puis sur **Archivages en attente**.  
  
4.  Archivez tous les fichiers de la solution.  
  
    Pour plus d’informations, consultez [Archiver des modifications en attente](/previous-versions/visualstudio/visual-studio-2010/ms181411(v=vs.100)).  
  
    > [!NOTE]  
    > Vous pouvez disposer d'un processus d'équipe spécifique qui détermine la manière dont les tests automatisés sont créés et gérés. Par exemple, le processus peut nécessiter de vérifier la génération localement avant d'avoir archivé ce code avec les tests qui seront exécutés sur celui-ci.  
  
    Dans l'**Explorateur de solutions**, une icône de cadenas apparaît en regard de chaque fichier pour indiquer qu'il est archivé. Pour plus d'informations, consultez [Afficher les propriétés des fichiers et dossiers du contrôle de version](/previous-versions/visualstudio/visual-studio-2010/ms245468(v=vs.100)).  
  
    Les tests sont disponibles dans Team Foundation Build. Vous pouvez à présent créer une définition de build qui contient les tests à exécuter.  
  
## <a name="create-a-build-definition"></a><a name="CreateBuildDef"></a>Créer une définition de build  
  
#### <a name="to-create-a-build-definition"></a>Pour créer une définition de build  
  
1.  Dans Team Explorer, sélectionnez votre projet d'équipe, cliquez avec le bouton droit sur le nœud **Versions**, puis cliquez sur **Nouvelle définition de build**.  
  
    La fenêtre **Nouvelle définition de build** s'affiche.  
  
2.  Dans **Nom de définition de build**, tapez le nom à utiliser pour la définition de build.  
  
3.  Dans la barre de navigation, cliquez sur **Valeurs par défaut des builds**.  
  
4.  Dans **Copier la sortie de la génération vers le dossier de dépôt suivant (chemin UNC, tel que \\\server\share)** , spécifiez un dossier qui contiendra la sortie de la génération.  
  
    Spécifiez un dossier partagé sur votre ordinateur local ou n'importe quel emplacement réseau sur lequel le processus de génération aura des autorisations.  
  
5.  Dans la barre de navigation, cliquez sur **Processus**.  
  
6.  Dans le groupe **Requis**, dans **Éléments à générer**, cliquez sur le bouton Parcourir (…).  
  
7.  Dans la boîte de dialogue **Éditeur de liste des projets de génération**, cliquez sur **Ajouter**.  
  
8.  Spécifiez le fichier solution (.sln) que vous avez ajouté au contrôle de version précédemment dans cette procédure pas à pas, puis cliquez sur **OK**.  
  
    La solution apparaît dans la liste **Fichiers projet ou solution à générer**.  
  
9. Cliquez sur **OK**.  
  
10. Dans le groupe  **De base**, sous **Tests automatisés**, spécifiez les tests à exécuter. Par défaut, les tests contenus dans les fichiers nommés \*test\*.dll de votre solution seront exécutés.  
  
11. Dans le menu **Fichier**, cliquez sur **Enregistrer** *ProjectName*.  
  
    Vous avez créé une définition de build. Ensuite, modifiez le projet de test.  
  
## <a name="run-the-new-build-definition"></a><a name="RunBuild"></a>Exécuter la nouvelle définition de build  
  
#### <a name="to-run-the-new-build-type"></a>Pour exécuter le nouveau type de build  
  
1.  Dans Team Explorer, développez le nœud du projet d'équipe, développez le nœud Builds, cliquez avec le bouton droit sur la définition de build à exécuter, puis cliquez sur Mettre en file d'attente une nouvelle build.  
  
    La boîte de dialogue **Mettre en file d’attente la build {** _TeamProjectName_ **}** s’affiche avec la liste de tous les types de build existants.  
  
2.  Si nécessaire, dans **Définition de build**, cliquez sur la nouvelle définition de build.  
  
3.  Vérifiez que les valeurs des champs **Définition de build**, **Agent de build** et **Dossier de dépôt de cette build** sont appropriées, puis cliquez sur **Mettre en file d'attente**.  
  
    L'onglet **En attente** de l'**Explorateur de builds** s'affiche. Pour plus d'informations, consultez [Gérer et afficher des builds terminées (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms181730(v=vs.100)) ou [Gérer vos builds dans l'Explorateur de builds (Visual Studio 2012)](/previous-versions/ms181732(v=vs.140)).  
  
## <a name="see-also"></a>Voir aussi  
[Exécuter des tests unitaires SQL Server](../ssdt/running-sql-server-unit-tests.md)  
[Créer une définition de build de base](/previous-versions/visualstudio/visual-studio-2010/ms181716(v=vs.100))  
[Mettre une build en file d'attente](/previous-versions/visualstudio/visual-studio-2010/ms181722(v=vs.100))  
[Surveiller la progression d'une build en cours d'exécution](/previous-versions/visualstudio/visual-studio-2010/ms181724(v=vs.100))  
