---
title: Installation et configuration
description: Apprenez à installer Master Data Services sur un ordinateur Windows Server 2012 R2, à configurer la base de données et le site Web MDS et à déployer les exemples de modèles et de données.
ms.custom: ''
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: quickstart
ms.assetid: f6cd850f-b01b-491f-972c-f966b9fe4190
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ca05529c9b4ce1d9ec603989af81f0d2aeebf76b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236398"
---
# <a name="master-data-services-installation-and-configuration"></a>Installation et configuration de Master Data Services

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Cet article explique comment installer [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] sur un ordinateur Windows Server 2012 R2, configurer la base de données et le site web MDS, et déployer les exemples de modèles et de données. [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) permet à votre organisation de gérer une version approuvée des données.   
  
> [!NOTE] 
> Vous pouvez installer [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] sur une machine Windows 10 quand vous utilisez l’édition Developer qui prend maintenant en charge [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]. 
>>Pour plus d’informations sur la prise en charge des systèmes d’exploitation suivant les différentes éditions de [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)], consultez [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). 

Si vous souhaitez une vue d’ensemble de l’organisation des données dans [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)], consultez [Vue d’ensemble de Master Data Services (MDS)](../master-data-services/master-data-services-overview-mds.md).     
  
 Pour plus d’informations sur les nouvelles fonctionnalités de [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], consultez [Nouveautés de Master Data Services &#40;MDS&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md).  
 
Pour obtenir des liens vers des vidéos et d’autres ressources de formation concernant [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)], consultez [En savoir plus sur Master Data Services](../master-data-services/learn-sql-server-master-data-services.md). 
  
> **Télécharger**  
> -   Pour télécharger [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], accédez au  **[Centre d’évaluation](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-ctp/)** .  
> -   Vous avez un compte Azure ?  Cliquez **[ici](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm)** pour lancer une machine virtuelle avec SQL Server déjà installé.  
> 
> **Vous ne parvenez pas à créer un site web MDS ?**
> >Consultez cet article du support technique Microsoft pour obtenir des instructions sur la manière de résoudre ce problème.
> [Impossible de créer un site web MDS via un compte à faibles privilèges dans SQL Server 2016](https://aka.ms/mdssupport) 

## <a name="internet-explorer-and-silverlight"></a>Internet Explorer et Silverlight
- Quand vous installez [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] sur une machine Windows Server 2012, vous pouvez être amené à configurer la sécurité renforcée d’Internet Explorer pour autoriser le script du site de l’application web. Sinon, la navigation sur le site sur le serveur échoue.
- Pour fonctionner dans l’application web, Silverlight 5 doit être installé sur l’ordinateur client. Si vous n’avez pas la version requise de Silverlight, vous êtes invité à l’installer quand vous accédez à une partie de l’application web qui l’utilise. Vous pouvez installer Silverlight 5 à partir de **[cet emplacement](https://www.microsoft.com/silverlight/)**.

## <a name="ssmdsshort_md-on-an-azure-virtual-machine"></a>[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] sur une machine virtuelle Azure
Par défaut, lorsque vous lancez une machine virtuelle Azure sur laquelle [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)] est déjà installé, [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] est également installé. 

L’étape suivante consiste à installer IIS (Internet Information Services). Consultez la section [Installation et configuration d’IIS](#InstallIIS). 

Si vous souhaitez apporter des modifications à l’installation de [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)], le fichier setup.exe est disponible à l’emplacement par défaut, `<drive>` : \SQLServer_13.0_Full.
  
## <a name="installing-master-data-services"></a><a name="InstallMDS"></a> Installation de Master Data Services  
 Vous installez [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l’aide de l’Assistant Installation du programme d’installation de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]ou d’une invite de commandes.  
  
 **Pour installer [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] à l’aide du programme d’installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un ordinateur Windows Server 2012 R2**  
  
1.  Double-cliquez sur Setup.exe et suivez les étapes de l’Assistant Installation.  
  
2.  Dans la page [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Sélection de fonctionnalités **, sous** Fonctionnalités partagées **, sélectionnez**.  
  
     Cette opération installe [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], des assemblys, un composant logiciel enfichable Windows PowerShell et des dossiers et fichiers pour les applications et services web.  
  
     ![mds_SQLServer2016Setup_FeatureSelection](../master-data-services/media/mds-sqlserver2016setup-featureselection.png "mds_SQLServer2016Setup_FeatureSelection")  
  
3.  Terminez l’Assistant Installation.  

## <a name="installing-and-configuring-iis"></a><a name="InstallIIS"></a> Installation et configuration d’IIS
  
1.  Dans [!INCLUDE[winblue_server_2](../includes/winblue-server-2-md.md)], cliquez sur l’icône **Gestionnaire de serveur** dans la barre des tâches sur le **Bureau**.  
  
     ![Icône de la Gestionnaire de serveur dans la barre des tâches de Windows Server 2012](../master-data-services/media/mds-windowsservertaskbar-servermanagericon.png "Icône de la Gestionnaire de serveur dans la barre des tâches de Windows Server 2012")  
  
5.  Dans le **Gestionnaire de serveur**, cliquez sur **Ajouter des rôles et fonctionnalités** dans le menu **Gérer** .  
   
     ![Dans Server Manage, la commande de menu Ajouter des rôles et des fonctionnalités](../master-data-services/media/mds-servermanagerdashboard-addrolesfeaturesmenu.png "Dans Server Manage, la commande de menu Ajouter des rôles et des fonctionnalités")  
  
6.  Dans la page **Type d’installation** de **l’Assistant Ajout de rôles et de fonctionnalités**, acceptez la valeur par défaut (**Installation basée sur un rôle ou une fonctionnalité**), puis cliquez sur **Suivant**.  
  
7.  Cliquez sur **Sélectionner un serveur du pool de serveurs**, puis cliquez sur le serveur où vous avez installé [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
     ![mds_AddRolesFeaturesWizard_ServerSelectionPage](../master-data-services/media/mds-addrolesfeatureswizard-serverselectionpage.png) 
  
8. Dans la page **Rôles serveur**, cliquez sur **Serveur Web**, puis cliquez sur **Suivant**. 

   ![mds_AddRolesFeaturesWizard_ServerRolesPage](../master-data-services/media/mds-addrolesfeatureswizard-serverrolespage.png)
   
9. Dans la page **Fonctionnalités**, vérifiez que les fonctionnalités suivantes sont sélectionnées, puis cliquez sur **Suivant**. Ces fonctionnalités sont requises pour [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] sur [!INCLUDE[winblue_server_2_md](../includes/winblue-server-2-md.md)].
  
    |Fonctionnalités|Fonctionnalités|  
    |--------------|--------------|  
    |![mds_AddRolesFeaturesWizard_FeaturesPage](../master-data-services/media/mds-addrolesfeatureswizard-featurespage.png)|![mds_AddRolesFeaturesWizard_FeaturesPage_WindowsProcActive](../master-data-services/media/mds-addrolesfeatureswizard-featurespage-windowsprocactive.png)|  

10. Dans le volet gauche, cliquez sur **Rôle Web Server (IIS)**, puis cliquez sur **Services de rôle**.
11. Dans la page **Services de rôle**, vérifiez que les services suivants sont sélectionnés, puis cliquez sur **Suivant**. Ces services sont requis pour [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] sur [!INCLUDE[winblue_server_2](../includes/winblue-server-2-md.md)].

    > [!WARNING]  
    >  N’installez pas le service de rôle Publication WebDAV. Publication WebDAV n’est pas compatible avec [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
     |Services de rôle|Services de rôle|  
    |-----------------------------|-----------------------------|  
    |![mds_AddRolesFeaturesWizard_RoleServicesPage](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage.png)|![mds_AddRolesFeaturesWizard_RoleServicesPage_PerformSecurity](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-performsecurity.png)|  
    |![mds_AddRolesFeaturesWizard_RoleServicesPage_AppDevsection](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-appdevsection.png)|![mds_AddRolesFeaturesWizard_RoleServicesPage_ManageToolssection](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-managetoolssection.png)|  
    |||  
  
     Pour obtenir la liste des fonctionnalités et services de rôle obligatoires sur d’autres systèmes d’exploitation, consultez [Configuration requise pour l’application web &#40;Master Data Services&#41;](../master-data-services/install-windows/web-application-requirements-master-data-services.md).   
  
 Pour plus d’informations sur l’installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l’aide du programme d’installation, consultez [Installer SQL Server 2016 avec l’Assistant Installation &#40;programme d’installation&#41;](../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md).  
  
 Pour plus d’informations sur l’installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l’aide d’une invite de commandes, consultez [Installer SQL Server 2016 à partir de l’invite de commandes](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md). Lorsque vous utilisez une invite de commandes, [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] est disponible comme paramètre de fonctionnalité.  
  
 Pour obtenir une brève description des liens vers des informations supplémentaires sur les tâches de préinstallation, consultez [Installer Master Data Services](../master-data-services/install-windows/install-master-data-services.md).  
  
##  <a name="setting-up-the-database-and-website"></a><a name="SetUpWeb"></a> Configuration de la base de données et du site web  
 **Pour configurer la base de données et le site Web à l’aide de [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]**  

 
> [!WARNING]
>  Vous devez [installer IIS](#InstallIIS) avant de lancer le Gestionnaire de configuration [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]. Sinon,Configuration Manager affiche une erreur IIS, vous empêchant de créer l’application web [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)].  
> 
> **Condition requise pour le navigateur**
> >L’application web [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] fonctionne uniquement dans Internet Explorer 9 ou version ultérieure. Internet Explorer 8 et versions antérieures, Microsoft Edge et Chrome ne sont pas pris en charge.    
  
1.  Dans le volet gauche, lancez le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], puis cliquez sur **Configuration de la base de données** .  
  
2.  Dans **l’Assistant Création d’une base de données**, cliquez sur **Créer une base de données** , puis sur **Suivant**.  
  
3.  Sur la page **serveur de base de données** , spécifiez l’instance SQL Server. 

    >  [!INCLUDE[sqlv15](../includes/sssql19-md.md)] Ajoute la prise en charge de SQL Server Managed Instance. Définissez la valeur de **SQL Server instance** sur l’hôte de l’instance gérée. Par exemple : `xxxxxx.xxxxxx.database.windows.net`.

4. Sélectionnez le **type d’authentification** , puis cliquez sur **tester la connexion** pour confirmer que vous pouvez vous connecter à la base de données à l’aide des informations d’identification pour le type d’authentification que vous avez sélectionné. Cliquez sur **Suivant**.

    >Pour [!INCLUDE[sqlv15](../includes/sssql19-md.md)] , pour vous connecter à l’instance gérée, utilisez l’un des types d’authentification suivants :
    >
    >- Authentification intégrée Azure Active Directory : **utilisateur actuel – Active Directory intégré**
    >- Authentification SQL Server : **compte SQL Server**.
    >
    >Dans SQL Managed Instance, l’utilisateur doit être membre du `sysadmin` rôle serveur fixe.

    > [!NOTE]  
    >  Si vous sélectionnez le type d’authentification **Utilisateur actuel - Sécurité intégrée**, la zone **Nom d’utilisateur** est en lecture seule et affiche le nom du compte d’utilisateur Windows qui a ouvert une session sur l’ordinateur. Si vous exécutez [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)] [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] sur une machine virtuelle Azure, la zone **Nom d’utilisateur** affiche le nom de la machine virtuelle et le nom d’utilisateur associé au compte Administrateur local sur la machine virtuelle. 

    ![mds_2016ConfigManager_CreateDatabaseWizard_ServerPage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-serverpage.png)  
  
4.  Tapez un nom dans le champ **Nom de la base de données** . (Facultatif) Pour sélectionner un classement Windows, décochez la case **Classement par défaut de SQL Server**, puis cliquez sur une ou plusieurs des options disponibles comme **Respecter la casse**. Cliquez sur **Suivant**.

    ![mds_2016ConfigManager_CreateDatabaseWizard_DatabasePage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-databasepage.png)  
  
     Pour plus d’informations sur le classement Windows, consultez [Nom de classement Windows (Transact-SQL)](../t-sql/statements/windows-collation-name-transact-sql.md).  
  
5.  Dans le champ **Nom d’utilisateur** , indiquez le compte Windows de l’utilisateur qui sera le Super utilisateur par défaut pour Master Data Services. Un Super utilisateur a accès à toutes les zones fonctionnelles et peut ajouter, supprimer et mettre à jour tous les modèles.  

    ![mds_2016ConfigManager_CreateDatabaseWizard_AdminPage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-adminpage.png)  
  
6.  Cliquez sur **Suivant** pour afficher un récapitulatif des paramètres de la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , puis cliquez de nouveau sur **Suivant** pour créer la base de données. La page **État d’avancement et fin** s’affiche.

7. Quand la base de données est créée et configurée, cliquez sur **Terminer**.  
  
     Pour plus d’informations sur les paramètres de **l’Assistant Création d’une base de données**, consultez [Assistant Création d’une base de données &#40;Gestionnaire de configuration Master Data Services&#41;](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md).  
  
7.  Dans la page **Configuration de la base de données** dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], cliquez sur **Sélectionner une base de données**.  
  
8.  Cliquez sur **Se connecter**, sélectionnez la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] que vous avez créée à l’étape 7, puis cliquez sur **OK**. 

    ![mds_2016ConfigManager_SelectDatabaseButton_ConnectToDatabaseDialog](../master-data-services/media/mds-2016configmanager-selectdatabasebutton-connecttodatabasedialog.png)  
  
     Vous avez terminé la configuration de la base de données. La page **Configuration de la base de données** affiche désormais l’instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à laquelle vous êtes connecté pour [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], la base de données que vous avez créée et la version actuelle de la base de données.  

    ![mds_2016ConfigManager_DatabaseConfig_Completed](../master-data-services/media/mds-2016configmanager-databaseconfig-completed.png)   
  
9. Dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], cliquez sur **Configuration web** dans le volet gauche.  
  
10. Dans la zone de liste **Site web** , cliquez sur **Site web par défaut**, puis cliquez sur **Créer** pour créer une application web.  
  
    > [!NOTE]  
    >  Lorsque vous sélectionnez **Site web par défaut**, vous devez créer une application web. Si vous sélectionnez **Créer un nouveau site web** dans la zone de liste, l’application est créée automatiquement.  

     ![mds_2016ConfigManager_WebConfig](../master-data-services/media/mds-2016configmanager-webconfig.png)  
  
11. Dans la section **Pool d’applications** , procédez de l’une des manières suivantes.  
  
    -   Entrez le même nom d’utilisateur que celui utilisé à l’étape 5 pour le **Compte administrateur** de la base de données,entrez le mot de passe, puis cliquez sur **OK**.  
  
         **Ni**  
  
    -   Entrez un nom d’utilisateur différent, entrez le mot de passe, puis cliquez sur OK.  
  
         Vous n’êtes pas obligé d’utiliser le même compte lorsque vous créez la base de données et l’application web.  

        ![mds_2016ConfigManager_WebConfig_CreateWebApplication](../master-data-services/media/mds-2016configmanager-webconfig-createwebapplication.png)   
  
     Pour plus d’informations sur la boîte de dialogue **Créer une application web**, consultez [Boîte de dialogue Créer une application web &#40;Gestionnaire de configuration Master Data Services&#41;](../master-data-services/create-web-application-dialog-box-master-data-services-configuration-manager.md).  

    > [!NOTE] 
    >  Si votre domaine implémente la [liaison de canal ldap 2020 et les exigences de signature LDAP pour Windows](https://support.microsoft.com/en-us/help/4520412/2020-ldap-channel-binding-and-ldap-signing-requirements-for-windows). Vous verrez le problème « les informations d’identification n’ont pas pu être vérifiées dans Active Directory ». Lorsque vous utilisez un compte de domaine pour créer un pool d’applications. Pour une solution de contournement, au lieu d’un utilisateur de domaine, utilisez un utilisateur de l' **ordinateur local**. Cela peut contourner la vérification des informations d’identification avec Active Directory. Après avoir créé l’application Web, vous pouvez modifier l’identité en utilisateur de domaine dans le **Gestionnaire de Internet Information Services (IIS)**.
  
12. Dans la page **Configuration web** , dans la zone **Application web** , cliquez sur l’application que vous avez créée, puis cliquez sur **Sélectionner** dans la section  **Associer l’application à la base de données** .  
  
13. Cliquez sur **Se connecter**, sélectionnez la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] que vous souhaitez associer à l’application web, puis cliquez sur **OK**.  
  
     Vous avez terminé la configuration du site web. La page **Configuration Web** affiche maintenant le site web sélectionné, l’application web que vous avez créée et la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] associée à l’application.  

     ![mds_2016ConfigManager_WebConfig_Completed](../master-data-services/media/mds-2016configmanager-webconfig-completed.png)  
 
     
15. Cliquez sur **Appliquer**. La boîte de message **Configuration terminée** s’affiche. Cliquez sur **OK** dans la boîte de message pour lancer l’application web. L’adresse du site Web est https://*Server Name* / /. 


![mds_2016ConfigurationComplete_MessageBox](../master-data-services/media/mds-2016configurationcomplete-messagebox.png) 
  
Pour plus d’informations sur les paramètres de la page Configuration Web, consultez [Page Configuration Web &#40;Gestionnaire de configuration Master Data Services&#41;](../master-data-services/web-configuration-page-master-data-services-configuration-manager.md)  
  
 Vous pouvez également utiliser [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] pour spécifier d’autres paramètres pour les applications et services web associés à la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Par exemple, vous pouvez spécifier la fréquence à laquelle les données sont chargées ou des e-mails de validation envoyés. Pour plus d’informations, consultez [Paramètres système &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
##  <a name="deploying-sample-models-and-data"></a><a name="deploySample"></a> Déploiement des exemples de modèles et de données  
 Les trois packages d’exemples de modèles suivants sont inclus avec  [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].   Ces exemples de modèles incluent des données. **L’emplacement par défaut de ces packages est %programfiles%\Microsoft SQL Server\140\Master Data Services\Samples\Packages.**
  
-   chartofaccounts_en.pkg  
-   customer_en.pkg  
-   product_en.pkg  
  
 Déployez ces packages à l’aide de l’outil MDSModelDeploy. L’emplacement par défaut de l’outil MDSModelDeploy est *lecteur*\Program Files\Microsoft SQL Server\ 140\Master Data Services\Configuration.  
  
 Pour plus d’informations sur la configuration requise pour l’exécution de cet outil, consultez [Déployer un package de déploiement de modèle à l’aide de MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md).  
  
 Pour plus d’informations sur les mises à jour des données visant à assurer la prise en charge des nouvelles fonctionnalités dans [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], consultez [Exemples SQL Server : packages de déploiement de modèles (MDS)](../master-data-services/sql-server-samples-model-deployment-packages-mds.md).  
  
 **Pour déployer les exemples de modèles**  
  
1.  Copiez les packages d’exemples de modèles dans *lecteur*\Program Files\Microsoft SQL Server\140\Master Data Services\Configuration.  
  
2.  Ouvrez une invite de commandes d’administrateur et accédez à MDSModelDeploy.exe en exécutant la commande suivante.  
  
    ```  
    cd c:\Program Files\Microsoft SQL Server\140\Master Data Services\Configuration  
    ```  
  
3.  Déployez chacun des exemples de modèles sur [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] en exécutant les commandes suivantes.  
  
    > [!IMPORTANT]  
    >  Dans les exemples ci-dessous, la valeur de service `MDS1` est spécifiée. Vous utilisez cette valeur si vous avez sélectionné  **Site Web par défaut** lors de la configuration du site web [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  Consultez la section [Configuration de la base de données et du site web](#SetUpWeb) .  
    >   
    >  Si vous avez créé un site web ou sélectionné un autre site web existant, commencez par exécuter la commande suivante afin de déterminer la valeur de service correcte.  
    >   
    >  `MDSModelDeploy listservices`  
    >   
    >  La première valeur de service dans la liste des valeurs renvoyées est celle que vous avez spécifiée pour déployer un modèle.  

    > [!NOTE]
    > Pour en savoir plus sur les informations de métadonnées des exemples de modèles, reportez-vous au fichier lisez-moi disponible à l’emplacement « c:\Program Files\Microsoft SQL Server\140\Master Data Services\Configuration ».
   
     **Pour déployer l’exemple de modèle chartofaccounts_en.pkg**  
  
    ```console
    MDSModelDeploy deploynew -package chartofaccounts_en.pkg -model ChartofAccounts -service MDS1  
    ```  
  
     **Pour déployer l’exemple de modèle customer_en.pkg**  
  
    ```console
    MDSModelDeploy deploynew -package customer_en.pkg -model Customer -service MDS1  
    ```  
  
     **Pour déployer l’exemple de modèle product_en.pkg**  
  
    ```console
    MDSModelDeploy deploynew -package product_en.pkg -model Product -service MDS1  
    ```  
  
     Lorsqu’un modèle est déployé avec succès, le message **Opération MDSModelDeploy terminée** s’affiche.  
  
     L’image suivante montre la commande de déploiement de l’exemple de modèle product_en.pkg.  
  
     ![Ligne de commande pour le déploiement du modèle d’exemple de produit](../master-data-services/media/mds-commandprompt-deployingsamplemodel-product.png "Ligne de commande pour le déploiement du modèle d’exemple de produit")  
  
4.  Pour afficher les exemples de modèles, procédez comme suit.  
  
    1.  Accédez au site web [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] que vous avez configuré. Consultez la section [Configuration de la base de données et du site web](#SetUpWeb) .  
  
         L’adresse du site Web est https://*Server Name* / /.  
  
    2.  Sélectionnez un modèle dans la zone de liste **Modèle** , puis cliquez sur **Explorer**.  
  
         ![Site Web MDS, page d’hébergement.](../master-data-services/media/mds-mdswebsite-homepage-selectsamplemodel.png "Site Web MDS, page d’hébergement.")  
  
## <a name="next-step"></a>étape suivante  
 Créer un nouveau modèle et les entités pour vos données. Consultez [Créer un modèle &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md) et [Créer une entité &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).  
  
 Pour obtenir une vue d’ensemble sur l’utilisation d’un modèle et d’entités en vue de créer une structure pour vos données dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], consultez [Vue d’ensemble de Master Data Services &#40;MDS&#41;](../master-data-services/master-data-services-overview-mds.md).  
    
## <a name="see-also"></a>Voir aussi  
 [Base de données Master Data Services](../master-data-services/master-data-services-database.md)   
 [Application Web Data Manager maître](../master-data-services/master-data-manager-web-application.md)   
 [Page Configuration de la base de données &#40;Gestionnaire de configuration Master Data Services&#41;](../master-data-services/database-configuration-page-master-data-services-configuration-manager.md)   
 [Nouveautés de Master Data Services &#40;MDS&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md)  
  
