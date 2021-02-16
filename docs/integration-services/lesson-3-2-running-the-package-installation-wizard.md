---
description: 'Leçon 3-2 : Exécution de l’Assistant Installation de package'
title: 'Étape 2 : Exécution de l’Assistant Installation de package | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: f91fbb89-4626-4c47-b96d-56052dc45861
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 66895007a304ee10878b54f1761b5e72792e1d57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354942"
---
# <a name="lesson-3-2---running-the-package-installation-wizard"></a>Leçon 3-2 : Exécution de l’Assistant Installation de package

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Au cours de cette tâche, vous allez exécuter l'Assistant Installation de package pour déployer les packages du projet Didacticiel de déploiement dans une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Seuls des packages peuvent être installés dans la table sysssispackages de la base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] msdb, les fichiers de support inclus dans l'application de déploiement seront déployés dans le système de fichiers.  
  
L'Assistant Installation de package va vous guider tout au long de l'installation et de la configuration des packages. Vous allez installer les packages dans une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur l'ordinateur de destination (ordinateur où vous avez copié l'application de déploiement). Vous allez aussi créer un dossier, C:\DeploymentTutorialInstall, où l'Assistant va installer les fichiers non-package.  
  
Dans une leçon précédente, vous avez modifié les packages dans le didacticiel pour utiliser des configurations. À l'aide de l'Assistant Installation de package, vous allez modifier ces configurations pour permettre l'exécution des packages dans l'environnement d'installation.  
  
### <a name="to-install-the-packages"></a>Pour installer les packages  
  
1.  Sur l'ordinateur de destination, recherchez l'application de déploiement.  
  
    Si vous avez utilisé la valeur par défaut, bin\Deployment, comme emplacement de l’utilitaire de déploiement, l’application de déploiement correspond au dossier Deployment dans le projet Didacticiel de déploiement.  
  
2.  Dans le dossier Déploiement, double-cliquez sur le fichier de manifeste, Didacticiel de déploiement.SSISDeploymentManifest.  
  
3.  Dans la page d’accueil de l’Assistant Installation de package, cliquez sur **Suivant**.  
  
4.  Dans la page Déployer les packages SSIS, sélectionnez l’option **Déploiement sur SQL Server** , cochez la case **Valider les packages après l’installation** et cliquez sur **Suivant**.  
  
5.  Dans la page Spécifier le serveur SQL cible, spécifiez **(local)** dans la zone **Nom du serveur** .  
  
6.  Si l’instance de SQL Server prend en charge l’authentification Windows, sélectionnez **Utiliser l’authentification Windows**; sinon, sélectionnez **Utiliser l’authentification SQL Server** et entrez un nom d’utilisateur et un mot de passe.  
  
7.  Vérifiez que la case **Se fier au serveur pour le chiffrement** est décochée.  
  
8.  Cliquez sur **Suivant**.  
  
9. Dans la page Sélectionner le dossier d’installation, cliquez sur **Parcourir**.  
  
10. Dans la boîte de dialogue **Rechercher un dossier** , développez **Poste de travail** et cliquez sur **Disque local (C:)**.  
  
11. Cliquez sur **Créer un nouveau dossier** et remplacez le nom par défaut du nouveau dossier, **Nouveau dossier**, par **DeploymentTutorialInstall**.  
  
    > [!IMPORTANT]  
    > Ce nom est référencé dans la valeur des variables d'environnement utilisées par les configurations. Le nom du dossier et la référence doivent correspondre. Dans le cas contraire, le package ne pourra pas être exécuté.  
  
12. Cliquez sur **OK**.  
  
13. Dans la page Sélectionner le dossier d’installation, vérifiez que la zone Dossier contient **C:\DeploymentTutorialInstall** et cliquez sur **Suivant**.  
  
14. Dans la page Confirmer l’installation, cliquez sur **Suivant**.  
  
    L'Assistant installe les packages. Une fois l'installation terminée, la page Configurer les packages s'ouvre.  
  
15. Dans la page Configurer les packages, vérifiez que la zone **Fichier de configuration** répertorie datatransferconfig.dtsconfig et loadxmldataconfig.dtsconfig.  
  
16. Dans la liste **Fichier de configuration** , cliquez sur **datatransferconfig.dtsconfig**, développez Property dans la colonne **Path** de la zone **Configurations** , et mettez à jour la colonne **Value** avec les valeurs suivantes :  
  
    |Propriété|Valeur|Valeur mise à jour|  
    |------------|---------|-----------------|  
    |\Package.Connections[Deployment Tutorial Log].Properties[ConnectionString]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Completed Packages\Deployment Tutorial Log|C:\DeploymentTutorialInstall\Deployment Tutorial Log|  
    |\Package.Connections[NewCustomers].Properties[ConnectionString]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\NewCustomers.txt|C:\DeploymentTutorialInstall\NewCustomers.txt|  
  
17. Dans la liste **Fichier de configuration** , cliquez sur loadxmldataconfig.dtsconfig, développez Property dans la colonne **Path** de la zone **Configurations** et mettez à jour la colonne **Value** avec les valeurs suivantes :  
  
    |Propriété|Valeur|Valeur mise à jour|  
    |------------|---------|-----------------|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLData]]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xml|C:\DeploymentTutorialInstall\orders.xml|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLSchemaDefinition]]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xsd|C:\DeploymentTutorialInstall\orders.xsd|  
  
18. Dans la page Validation des packages, affichez les résultats de validation de chaque package installé, puis cliquez sur **Suivant**.  
  
    Étant donné que les valeurs des variables d'environnement sur l'ordinateur de destination sont différentes des valeurs des variables d'environnement sur l'ordinateur de développement, plusieurs avertissements s'affichent dans la page Validation des packages. Attendez-vous à quatre avertissements :  
  
    -   Le fichier de configuration : « C:\DeploymentTutorial\DataTransferConfig.dtsConfig » n'est pas valide. Vérifiez le nom du fichier de configuration.  
  
    -   Impossible de charger au moins une des entrées de configuration du package. Recherchez dans les entrées de configuration et les précédents avertissements les descriptions de la configuration qui a échoué.  
  
    -   Le fichier de configuration : « C:\DeploymentTutorial\LoadXMLDataConfig.dtsConfig » n'est pas valide. Vérifiez le nom du fichier de configuration.  
  
    -   Impossible de charger au moins une des entrées de configuration du package. Recherchez dans les entrées de configuration et les précédents avertissements les descriptions de la configuration qui a échoué.  
  
    Ces avertissements n'affectent pas l'installation du package.  
  
    Si vous n’avez pas sélectionné l’option **Valider les packages après l’installation** dans la page Déployer les packages SSIS, la page Validation des packages ne s’ouvre pas et l’Assistant n’affiche pas les informations postinstallation sur la validation.  
  
19. Dans la page Fin de l’Assistant Installation de package, lisez le résumé de l’installation, puis cliquez sur **Terminer**.  
  
    > [!NOTE]  
    > Un fichier journal temporaire est créé pour être utilisé dans la validation de package. Ce fichier n'est pas utilisé lorsque le package s'exécute.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 3 : Test des packages déployés](../integration-services/lesson-3-3-testing-the-deployed-packages.md)  
  
## <a name="see-also"></a>Voir aussi  
[Service Integration Services &#40;Service SSIS&#41;](../integration-services/service/integration-services-service-ssis-service.md)  
