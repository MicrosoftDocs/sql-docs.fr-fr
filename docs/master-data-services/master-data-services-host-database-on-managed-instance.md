---
title: Héberger une base de données sur une instance gérée
description: Découvrez comment créer et configurer une base de données Master Data Services (MDS) et l’héberger sur un Managed Instance Azure SQL.
ms.custom: ''
ms.date: 07/01/2019
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 19519697-c219-44a8-9339-ee1b02545445
author: v-redu
ms.author: lle
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 0ae7d5ad7bdb7874e4f6a99a1a99ea08c228fced
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339481"
---
# <a name="host-an-mds-database-on-a-managed-instance"></a>Héberger une base de données MDS sur une instance gérée

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Cet article explique comment configurer une base de données Master Data Services (MDS) sur une instance gérée.
  
## <a name="preparation"></a>Préparation

Pour vous préparer, vous devez créer et configurer un Managed Instance SQL Azure et configurer votre ordinateur d’application Web.

### <a name="create-and-configure-the-database"></a>Créer et configurer la base de données

1. Créer une instance gérée avec un réseau virtuel. Pour plus d’informations, consultez [démarrage rapide : créer un Managed instance SQL](/azure/sql-database/sql-database-managed-instance-get-started) .

1. Configurez une connexion de point à site. Pour obtenir des instructions, consultez [configurer une connexion de point à site à un réseau virtuel à l’aide de l’authentification par certificat Azure Native : portail Azure](/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) .

1. Configurez Azure Active Directory l’authentification avec SQL Managed Instance. Pour plus d’informations [, consultez configurer et gérer l’authentification Azure Active Directory avec SQL](/azure/sql-database/sql-database-aad-authentication-configure) .

### <a name="configure-web-application-machine"></a>Configurer un ordinateur d’application Web

1. Installez un certificat de connexion de point à site et un VPN pour vous assurer que l’ordinateur peut accéder à l’instance gérée. Pour obtenir des instructions, consultez [configurer une connexion de point à site à un réseau virtuel à l’aide de l’authentification par certificat Azure Native : portail Azure](/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) .

1. Installez les rôles et fonctionnalités suivants :
   - Rôles :
     - Services Internet (IIS)
     - Outils d’administration Web
     - Console de gestion d’IIS
     - Services World Wide Web
     - Développement d'applications
     - Extensibilité .NET 3.5
     - Extensibilité .NET 4.5
     - ASP.NET 3.5
     - ASP.NET 4.5
     - Extensions ISAPI
     - Filtres ISAPI
     - Fonctionnalités HTTP communes
     - Document par défaut
     - Exploration des répertoires
     - Erreurs HTTP
     - Contenu statique
     - Intégrité et diagnostics
     - Journalisation HTTP
     - Observateur de demandes
     - Performances
     - Compression de contenu statique
     - Sécurité
     - Filtrage des demandes
     - Authentification Windows
       > [!NOTE]
       > Ne pas installer la publication WebDAV

   - Fonctionnalités :
     - .NET Framework 3.5 (inclut .NET 2.0 et 3.0)
     - .NET Framework 4.5 Advanced Services
     - ASP.NET 4.5
     - Services WCF
     - Activation HTTP (obligatoire)
     - Partage de port TCP
     - Service d’activation des processus Windows
     - Modèle de processus
     - Environnement .NET
     - API de configuration
     - compression du contenu dynamique

## <a name="install-and-configure-an-mds-web-application"></a>Installer et configurer une application Web MDS

Ensuite, vous installez et configurez [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] .

### <a name="install-sql-server-2019"></a>Installer SQL Server 2019

Utilisez l’Assistant Installation de SQL Server ou une invite de commandes pour installer [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] .

1. Ouvrez `Setup.exe` et suivez les étapes de l’Assistant installation.

2. Dans la page [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Sélection de fonctionnalités **, sous** Fonctionnalités partagées **, sélectionnez**.
Cette action installe :
   - [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]
   - Assemblys
   - Un composant logiciel enfichable Windows PowerShell
   - Dossiers et fichiers pour les applications et services Web.

   ![Capture d’écran montrant la page de sélection de fonctionnalités.](../master-data-services/media/mds-sqlserver2019-config-mi-sqlfeatureselection.png "MDS-SQLServer2019-config-MI_SQLFeatureSelection")  

### <a name="set-up-the-database-and-website"></a>Configurer la base de données et le site Web

1. Connectez le réseau virtuel Azure pour vous assurer que vous pouvez vous connecter à l’instance gérée.

   ![Capture d’écran du VPN test MI se connectant au réseau virtuel Azure.](../master-data-services/media/mds-sqlserver2019-config-mi-p2svpnconnect.png "MDS-SQLServer2019-config-MI_P2SVPNConnect")

1. Ouvrez la [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] , puis sélectionnez **configuration de la base de données** dans le volet gauche.

1. Sélectionnez **créer une base de données** pour ouvrir l' **Assistant Création d’une base de données**. Sélectionnez **Suivant**.

1. Dans la page **serveur de base de données** , renseignez le champ **instance de SQL Server** , puis choisissez le **type d’authentification**. Sélectionnez **tester la connexion** pour confirmer que vous pouvez utiliser vos informations d’identification pour vous connecter à la base de données via le type d’authentification choisi. Sélectionnez **Suivant**.

   > [!NOTE]
   > - Une instance de SQL Server se présente comme suit `xxxxxxx.xxxxxxx.database.windows.net` :.
   > - Pour une instance gérée, choisissez parmi les types d’authentification **« compte SQL Server »** et **« utilisateur actuel-Active Directory intégré »** .
   > - Si vous sélectionnez **utilisateur actuel – Active Directory intégré** comme type d’authentification, le champ **nom d’utilisateur** est en lecture seule et affiche le compte d’utilisateur Windows actuellement connecté. Si vous exécutez SQL Server 2019 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] sur une machine virtuelle Azure, le champ **nom d’utilisateur** affiche le nom de la machine virtuelle et le nom d’utilisateur du compte d’administrateur local sur la machine virtuelle.

   Votre authentification doit contenir la règle **« sysadmin »** pour les instances gérées.

   ![Capture d’écran de la page serveur de base de données de l’Assistant Création d’une base de données.](../master-data-services/media/mds-sqlserver2019-config-mi-createdbconnect.png "MDS-SQLServer2019-config-MI_CreateDBConnect")  

1. Tapez un nom dans le champ **Nom de la base de données** . Si vous le souhaitez, pour sélectionner un classement Windows, désactivez la case à cocher **SQL Server classement par défaut** , puis sélectionnez une ou plusieurs des options disponibles. Par exemple, respect de la **casse**. Sélectionnez **Suivant**.

   ![Capture d’écran de la page base de données de l’Assistant Création d’une base de données.](../master-data-services/media/mds-sqlserver2019-config-mi-createddbname.png "MDS-SQLServer2019-config-MI_CreatedDBName")

1. Dans le champ **nom d’utilisateur** , spécifiez le compte Windows du super utilisateur par défaut pour [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] . Un super utilisateur a accès à toutes les zones fonctionnelles et peut ajouter, supprimer et mettre à jour tous les modèles.

   ![Capture d’écran de la page compte d’administrateur de l’Assistant Création d’une base de données.](../master-data-services/media/mds-sqlserver2019-config-mi-createdbusername.png "MDS-SQLServer2019-config-MI_createDBUserName")

1. Sélectionnez **suivant** pour afficher un résumé des paramètres de la [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] base de données. Sélectionnez de nouveau  **suivant** pour créer la base de données. La page **progression et fin** s’affiche.

1. Une fois la base de données créée et configurée, sélectionnez **Terminer**.

   Pour plus d’informations sur les paramètres de l' **Assistant Création d’une base de données**, consultez [Assistant Création de base de données &#40;[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Configuration Manager&#41;](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md).

1. Dans la page **configuration de la base de données** du [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] , choisissez **Sélectionner une base de données**.

1. Sélectionnez **se connecter**, choisissez la [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] base de données, puis cliquez sur **OK**.

   ![Capture d’écran de la boîte de dialogue se connecter à la base de données.](../master-data-services/media/mds-sqlserver2019-config-mi-connectdbname.png "MDS-SQLServer2019-config-MI_connectDBName")

1. Dans [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] , sélectionnez **configuration Web** dans le volet gauche.

1. Dans la zone de liste **site** Web, choisissez **site Web par défaut**, puis sélectionnez **créer** pour créer une application Web.

   ![Capture d’écran de la boîte de dialogue Gestionnaire de configuration Master Data Services.](../master-data-services/media/mds-sqlserver2019-config-mi-webconfiguration.png "MDS-SQLServer2019-config-MI_WebConfiguration")

   > [!NOTE]
   > Si vous sélectionnez **site Web par défaut**, vous devez créer séparément une application Web. Si vous choisissez **créer un nouveau site Web** dans la zone de liste, l’application est automatiquement créée.

1. Dans la section **pool d’applications** , entrez un autre nom d’utilisateur, entrez le mot de passe, puis sélectionnez **OK**.

   ![Capture d’écran de la boîte de dialogue gestion des applications.](../master-data-services/media/mds-sqlserver2019-config-mi-createwebapplication.png "MDS-SQLServer2019-config-MI_CreateWebApplication")

   > [!NOTE]
   > Assurez-vous que l’utilisateur peut accéder à la base de données avec l’authentification intégrée Active Directory que vous avez créée récemment. Vous pouvez également modifier la connexion dans `web.config` ultérieurement.

   Pour plus d’informations sur la boîte de dialogue **créer une application Web** , consultez boîte de [dialogue créer une application web &#40;[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Configuration Manager&#41;](../master-data-services/create-web-application-dialog-box-master-data-services-configuration-manager.md).

1. Dans le volet **configuration Web** de la fenêtre **application Web** , sélectionnez l’application que vous avez créée, puis choisissez **Sélectionner** dans la section **associer une application à une base de données** .

1. Sélectionnez **se connecter** , puis choisissez la [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] base de données que vous souhaitez associer à l’application Web. Sélectionnez **OK**.

   Vous avez terminé la configuration du site Web. La page **configuration Web** affiche maintenant le site Web que vous avez sélectionné, l’application Web que vous avez créée et la [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] base de données associée à l’application.

   ![Capture d’écran de la section de configuration Web.](../master-data-services/media/mds-sqlserver2019-config-mi-webconfigselectdb.png "MDS-SQLServer2019-config-MI_WebConfigSelectDB")

1. Sélectionnez **Appliquer**. Le message **Configuration terminée** s’affiche. Sélectionnez **OK** dans la boîte de message pour lancer l’application Web. L’adresse du site web est `http://server name/web application/`.

## <a name="configure-authentication"></a>configurer l’authentification ;

Pour connecter la base de données Managed instance à l’application Web, vous devez modifier l’autre type d’authentification.

Recherchez le `web.config` fichier sous `C:\Program Files\Microsoft SQL Server\150\Master Data Services\WebApplication` . Modifiez la chaîne de connexion pour modifier l’autre type d’authentification pour la connexion à la base de données d’instance gérée.

Le type d’authentification par défaut est `Active Directory Integrated` tel qu’indiqué dans l’exemple de chaîne de connexion suivant :

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;Authentication=&quot;Active Directory Integrated&quot;" />
   ```

MDS prend également en charge l’authentification par mot de passe Active Directory et l’authentification SQL Server, comme indiqué dans les exemples de chaînes de connexion suivants :

- Authentification par mot de passe Active Directory

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;Authentication=&quot;Active Directory Password&quot; ; UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!" />
   ```

- Authentification SQL Server

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;User ID=UserName;Password=MyPassword!;" />
   ```

## <a name="upgrade-ssmdsshort_md-and-sql-database-version"></a>Mettre à niveau [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] et SQL Database version

### <a name="upgrade-ssmdsshort_md"></a>Installation [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]

Installez la **mise à jour cumulative de SQL Server 2019**. [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] sera automatiquement mis à jour.

### <a name="upgrade-sql-server"></a>Mettre à niveau SQL Server

Vous pouvez recevoir l’erreur suivante : `The client version is incompatible with the database version` après l’installation de la **mise à jour Cumulative de SQL Server 2019**.
![Capture d’écran de l’erreur de Master Data Services.](../master-data-services/media/mds-sqlserver2019-config-mi-upgradedbpage.png "MDS-SQLServer2019-config-MI_UpgradeDBPage")

Pour résoudre ce problème, vous devez mettre à niveau la version de la base de données :

1. Ouvrez la [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] , puis sélectionnez  **configuration de la base de données** dans le volet gauche.

1. Dans la page **configuration de la base de données** du [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] , choisissez **Sélectionner une base de données**.

1. Choisissez la [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] base de données que vous avez associée à l’application Web. Sélectionnez **se connecter**, puis cliquez sur **OK**.

   ![Capture d’écran de la boîte de dialogue Connexion à une base de données Master Data Service.](../master-data-services/media/mds-sqlserver2019-config-mi-connectdbname.png "MDS-SQLServer2019-config-MI_ConnectDBName")

1. Sélectionnez **mettre à niveau la base de données...** .

   ![Capture d’écran de l’option de mise à niveau de la base de données.](../master-data-services/media/mds-sqlserver2019-config-mi-selectupgradedb.png "MDS-SQLServer2019-config-MI_SelectUpgradeDB")

1. Dans l’Assistant Mise à niveau de la base de données, sélectionnez **suivant** sur la page d' **Accueil** et sur la page révision de la  **mise à niveau** .

   ![Capture d’écran de la page révision de mise à niveau de l’Assistant Mise à niveau de base de données.](../master-data-services/media/mds-sqlserver2019-config-mi-upgradedbwizard.png "MDS-SQLServer2019-config-MI_UpgradeDBWizard")

1. Sélectionnez **Terminer** une fois toutes les tâches terminées.

## <a name="see-also"></a>Voir aussi

- [Base de données Master Data Services](../master-data-services/master-data-services-database.md)
- [Application Web Master Data Manager](../master-data-services/master-data-manager-web-application.md)
- [Page Configuration de base de données &#40;Gestionnaire de configuration Master Data Services&#41;](../master-data-services/database-configuration-page-master-data-services-configuration-manager.md)
- [Nouveautés de Master Data Services &#40;MDS&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md)