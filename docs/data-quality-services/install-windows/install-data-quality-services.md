---
description: Installer Data Quality Services
title: Installer Data Quality Services
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: 486e4216-a946-4c6e-828c-61bc905f7ec1
author: swinarko
ms.author: sawinark
ms.openlocfilehash: c7a91729430078f5913d5f4d83ca75015c50ac15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339902"
---
# <a name="install-data-quality-services"></a>Installer Data Quality Services

[!INCLUDE [SQL Server - Windows only ](../../includes/applies-to-version/sql-windows-only.md)]

  [!INCLUDE[ssDQSnoversionLong](../../includes/ssdqsnoversionlong-md.md)] (DQS) contient les deux composants suivants : **[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]** et **[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]** .  
  
|Composant DQS|Description|  
|-------------------|-----------------|  
|[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]|[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] est installé sur le moteur de base de données [!INCLUDE[ssNoversion](../../includes/ssNoVersion-md.md)] et inclut trois bases de données : DQS_MAIN, DQS_PROJECTS et DQS_STAGING_DATA. DQS_MAIN contient des procédures stockées DQS, le moteur DQS et des bases de connaissances publiées. DQS_PROJECTS contient les informations du projet Data Quality. DQS_STAGING_DATA est la zone de transit où vous pouvez copier vos données sources pour effectuer des opérations DQS, puis exporter vos données traitées.|  
|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] est une application autonome qui vous permet de vous connecter à [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]et vous fournit une interface utilisateur graphique hautement intuitive pour effectuer des opérations de contrôle qualité des données et d’autres tâches administratives associées à DQS.|  
  
> [!IMPORTANT]  
>  Indépendamment des deux composants DQS ci-dessus, vous pouvez également :  
>   
>  Utiliser la Transformation de nettoyage DQS dans Integration Services qui effectue le nettoyage des données dans le processus d'empaquetage Integration Services, et est automatiquement installée lorsque vous installez Integration Services. Pour plus d’informations sur l’installation d’Integration Services, consultez [Installer Integration Services](../../integration-services/install-windows/install-integration-services.md).  
>   
>  Activez l'intégration DQS dans Master Data Services pour utiliser la fonctionnalité DQS correspondante dans le complément Master Data Services pour Excel. Pour plus d'informations, consultez [Activer l'intégration de Data Quality Services avec Master Data Services](../../master-data-services/install-windows/enable-data-quality-services-integration-with-master-data-services.md).  
  
 L'installation DQS est un processus en trois parties :  
  
-   [Tâches de préinstallation](#PreInstallationTasks): vérifiez la configuration requise avant d’installer DQS.  
  
-   [Tâches d'installation de Data Quality Services](#DQSInstallation): installent DQS à l'aide du programme d'installation de SQL Server.  
  
-   [Tâches de post-installation](#PostInstallationTasks): exécutez ces tâches après avoir installé SQL Server pour finir d’installer DQS.  
  
> [!NOTE]  
>  Cette rubrique ne contient pas d'instructions relatives à l'exécution du programme d'installation à partir de la ligne de commande. Pour plus d’informations sur les options de ligne de commande pour l’installation de [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] et du client, consultez [Paramètres de fonctionnalités](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Feature) dans [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
##  <a name="pre-installation-tasks"></a><a name="PreInstallationTasks"></a> Tâches de pré-installation  
 Avant d'installer DQS, assurez-vous que votre ordinateur a la configuration minimale requise. Le tableau ci-dessous fournit des informations sur la configuration minimum requise pour les composants DQS :  
  
|Composant DQS|Configuration minimale requise|  
|-------------------|---------------------------------|  
|[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]|Mémoire (RAM): 2 Go minimum ; mémoire recommandée : 4 Go ou plus<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] . Pour plus d’informations, consultez [Installer le moteur de base de données SQL Server](../../database-engine/install-windows/install-sql-server-database-engine.md).|  
|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]|.NET Framework 4.0 (installé lors de l'installation du [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] , si ce composant n'est pas déjà installé)<br /><br /> Internet Explorer 6.0 SP1 ou version ultérieure.|  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] et [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] peuvent être installés sur le même ordinateur ou sur des ordinateurs distincts. Les deux composants peuvent être installés indépendamment l'un de l'autre, et dans n'importe quel ordre. Toutefois, pour utiliser le [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)], vous avez besoin d'un [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] installé auquel se connecter.  
>   
>  Connectez-vous à la version [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] de [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] en utilisant la version actuelle ou une version antérieure de [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] et la Transformation de nettoyage DQS. Pour plus d’informations sur la mise à niveau de votre version existante de DQS vers [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], consultez [Mettre à niveau Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md).  
>   
>  Bien que Microsoft Excel ne soit pas un composant requis pour installer un client de qualité des données, Microsoft Excel 2003 ou une version ultérieure doit être installé sur l'ordinateur client de qualité des données pour effectuer diverses opérations dans l'application cliente, comme importer des valeurs de domaine à partir d'un fichier Excel ou mapper des données sources dans un fichier Excel pour les activités de découverte des connaissances, de nettoyage ou de correspondance.  
  
 Pour obtenir des informations détaillées sur la configuration minimale requise pour installer [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], consultez [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
##  <a name="data-quality-services-installation-tasks"></a><a name="DQSInstallation"></a> Tâches d'installation de Data Quality Services  
 Vous devez utiliser le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] pour installer les composants de DQS. Lorsque vous exécutez le programme d'installation de SQL Server, vous devez parcourir les pages de l'Assistant Installation pour sélectionner les options appropriées selon votre configuration. Le tableau suivant répertorie uniquement les pages de l'Assistant Installation où les options que vous sélectionnez affectent votre installation de DQS :  
  
|Page|Action|  
|----------|------------|  
|Sélection de caractéristiques|Sélectionnez :<br /><br /> **Data Quality Services** sous **Services Moteur de base de données** pour installer le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]. <br />Si vous activez la case à cocher **Data Quality Services** , l’installation de SQL Server copie un fichier de programme d’installation, DQSInstaller.exe, sous le répertoire de l’instance SQL Server sur votre ordinateur. Vous devez exécuter ce fichier après avoir complété le programme d'installation de SQL Server pour *terminer* l'installation du [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] . De plus, vous devez procéder à quelques étapes supplémentaires pour configurer votre [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] avant de pouvoir l'utiliser. Pour plus d’informations, consultez [Tâches de post-installation](#PostInstallationTasks).<br /><br /> **Client Data Quality** pour installer le [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)].<br /><br /> Recommandations **Outils de gestion-complet** sous **outils de gestion-de base** pour l’installation [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Ils fournissent une interface utilisateur graphique pour gérer votre instance SQL Server et vous aideront à effectuer des tâches supplémentaires après l'installation, comme décrit dans la section suivante.|  
|Configuration du moteur de base de données|Cliquez **Ajouter l'utilisateur actuel** pour ajouter votre compte d'utilisateur Windows au rôle serveur fixe sysadmin. Cela est nécessaire pour être en mesure d'exécuter le fichier DQSInstaller.exe pour terminer l'installation du [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .|  
  
##  <a name="post-installation-tasks"></a><a name="PostInstallationTasks"></a> Tâches consécutives à l’installation  
 Après avoir terminé l'Assistant Installation de SQL Server, vous devez effectuer les étapes supplémentaires indiquées dans cette section pour achever votre installation du [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] , le configurer et l'utiliser.  
  
1.  Pour terminer l’installation du [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] , exécutez le fichier DQSInstaller.exe. Pendant l'exécution du fichier DQSInstaller.exe :  
  
    -   Les bases de données DQS_MAIN, DQS_PROJECTS et DQS_STAGING_DATA sont créées.  
  
    -   Les connexions ##MS_dqs_db_owner_login## et ##MS_dqs_service_login## sont créées.  
  
    -   Les rôles dqs_administrator, dqs_kb_editor et dqs_kb_operator sont créés dans la base de données DQS_MAIN.  
  
    -   La procédure stockée DQInitDQS_MAIN est créée dans la base de données master.  
  
    -   Le fichier DQS_install.log est généralement créé dans le dossier C:\Program Files\Microsoft SQL Server\MSSQL13.*<nom_instance>* \MSSQL\Log. Le fichier contient des informations sur les actions effectuées lors de l'exécution du fichier DQSInstaller.exe.  
  
    -   Si une base de données Master Data Services est présente dans la même instance SQL Server que le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)], un utilisateur mappé dans la connexion Master Data Services est créé et reçoit le rôle dqs_administrator sur la base de données DQS_MAIN.  
  
     L'installation du [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] est terminée.  
  
     Pour plus d’informations, consultez [exécuter DQSInstaller.exe pour terminer l’installation du serveur de qualité des données](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md).  
  
2.  Affectez des rôles DQS aux utilisateurs :  
  
     Pour se connecter à [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] à l’aide de [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)], un utilisateur doit disposer de l’un des trois rôles suivants concernant la base de données DQS_MAIN :  
  
    -   **dqs_administrator**  
  
    -   **dqs_kb_editor**  
  
    -   **dqs_kb_operator**  
  
     Par défaut, si votre compte d'utilisateur est membre du rôle serveur fixe sysadmin, vous pouvez ouvrir une session sur le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] à l'aide du [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] même si aucun des rôles DQS n'est accordé à votre compte d'utilisateur. Pour plus d'informations sur les trois rôles DQS, consultez [Sécurité DQS](../../data-quality-services/dqs-security.md).  
  
     Pour plus d'informations, consultez [Affecter des rôles DQS aux utilisateurs](../../data-quality-services/install-windows/grant-dqs-roles-to-users.md).  
  
    > [!NOTE]  
    >  Les trois rôles DQS ne sont pas disponibles pour les bases de données DQS_PROJECTS et DQS_STAGING_DATA.  
  
3.  Rendez vos données disponibles pour les opérations DQS. Assurez-vous que vous avez accès à vos données sources pour les opérations DQS et que vous pouvez exporter les données traitées vers une table dans une base de données.  
  
     Pour plus d’informations, consultez  
                    [Accéder aux données pour les opérations DQS](../../data-quality-services/install-windows/access-data-for-the-dqs-operations.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vidéo : installer et configurer DQS](/previous-versions/dn912438(v=msdn.10))   
 [Mettre à niveau les assemblys SQLCLR après la mise à jour .NET Framework](../../data-quality-services/install-windows/upgrade-sqlclr-assemblies-after-net-framework-update.md)   
 [Exporter et importer des bases de connaissances DQS à l’aide de DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md)   
 [Mettre à niveau Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md)   
 [Supprimer des objets Data Quality Server](../../sql-server/install/remove-data-quality-server-objects.md)   
 [Installer SQL Server fonctionnalités Business Intelligence](../../sql-server/install/install-sql-server-business-intelligence-features.md)   
 [Désinstaller SQL Server](../../sql-server/install/uninstall-sql-server.md)   
 [Data Quality Services](../../data-quality-services/data-quality-services.md)   
 [Résolution des problèmes d’installation et de configuration dans Data Quality Services (DQS)](https://social.technet.microsoft.com/wiki/contents/articles/3776.aspx)  
  
