---
description: Upgrade and Migrate Reporting Services
title: Mettre à niveau et migrer Reporting Services | Microsoft Docs
ms.prod: reporting-services
ms.prod_service: reporting-services-native
helpviewer_keywords:
- SSRS, upgrading
- Reporting Services, upgrades
- SQL Server Reporting Services, upgrading
- upgrading Reporting Services
author: maggiesMSFT
ms.author: maggies
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 2515a93b025392735135cd13cbd734975f8ed221
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596829"
---
# <a name="upgrade-and-migrate-reporting-services"></a>Upgrade and Migrate Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  Cette rubrique propose une vue d’ensemble des options de mise à niveau et de migration pour SQL Server Reporting Services. Voici les approches générales à suivre pour mettre à niveau un déploiement de SQL Server Reporting Services :  
 
- **Mise à niveau *vers* Reporting Services 2016 et antérieur *à partir de* Reporting Services 2016 et antérieur :** vous mettez à niveau les composants [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur les serveurs et instances où ils sont installés. C’est ce que l’on appelle communément une mise à niveau « sur place ». La mise à niveau sur place n'est pas prise en charge d'un mode de serveur [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à un autre. Par exemple, vous ne pouvez pas mettre à niveau un serveur de rapports en mode natif vers un serveur de rapports en mode SharePoint. vous pouvez migrer vos éléments de rapport d'un mode à l'autre. Pour plus d’informations, consultez la section [Scénarios de mise à niveau et de migration en mode SharePoint](#bkmk_sharePoint_scenarios) plus loin dans ce document.  

- **Mise à niveau *vers* Reporting Services 2017 et ultérieur *à partir de* Reporting Services 2016 et antérieur** : il ne s’agit pas du même scénario de mise à niveau que dans les versions précédentes. Lors de la mise à niveau *vers* Reporting Services 2016 et antérieur, vous pouviez suivre un processus de mise à niveau sur place en utilisant le support d’installation de SQL Server. Lors de la mise à niveau *vers* Reporting Services 2017 et ultérieur *à partir de* Reporting Services 2016 et antérieur, vous ne pouvez pas suivre les mêmes étapes puisque la nouvelle installation de Reporting Services est un produit autonome qui ne figure plus sur le support d’installation de SQL Server. 

    Pour procéder à la mise à niveau de Reporting Services 2016 et antérieur vers Reporting Services 2017 et ultérieur, suivez l’article [Migrer une installation Reporting Services (mode natif)](migrate-a-reporting-services-installation-native-mode.md) avec Reporting Services 2017 ou ultérieur comme instance de destination. 

- **Mise à niveau *à partir de* Reporting Services 2017 vers les versions futures** : il s’agit encore d’un scénario de mise à niveau sur place, car les GUID d’installation du produit sont les mêmes. Exécutez le fichier d’installation SQLServerReportingServices.exe pour commencer la mise à niveau sur place sur le serveur où Reporting Services est actuellement installé.
  
- **Migration** : vous installez et configurez un nouvel environnement SharePoint, copiez vos éléments de rapport et ressources dans le nouvel environnement et configurez le nouvel environnement de façon à utiliser le contenu existant. une forme de migration de niveau inférieur consiste à copier les bases de données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , les fichiers de configuration, et si vous utilisez le mode SharePoint, les bases de données de contenu SharePoint.  


> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est pas disponible après SQL Server 2016.
   
##  <a name="known-upgrade-issues-and-best-practices"></a><a name="bkmk_known_issues"></a> Problèmes connus de mise à niveau et meilleures pratiques  
 Pour obtenir la liste des éditions et versions prises en charge que vous pouvez mettre à niveau, consultez [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
> [!TIP]  
>  Pour obtenir les dernières informations concernant les problèmes relatifs à SQL Server, consultez les [notes de publication de SQL Server 2016](../../sql-server/sql-server-2016-release-notes.md).  
  
  
##  <a name="side-by-side-installations"></a><a name="bkmk_side_by_side"></a> Installations côte à côte  
 SQL Server Reporting Services en mode natif peut être installé côte à côte avec un déploiement [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] en mode natif.  
  
 Il n’existe aucune prise en charge des déploiements côte à côte de SQL Server Reporting Services en mode SharePoint et des versions antérieures des composants du mode SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
  
##  <a name="in-place-upgrade"></a><a name="bkmk_inplace_upgrade"></a> Mise à niveau sur place  
 La mise à niveau est effectuée par le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être utilisé pour mettre à niveau tout ou partie des composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , y compris [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Le programme d'installation détecte les instances existantes et vous invite à procéder à la mise à niveau. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] L'installation fournit des options de mise à niveau que vous pouvez spécifier comme argument de ligne de commande ou dans l'Assistant Installation.  
  
 Lorsque vous exécutez le programme d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez sélectionner l’option de mise à niveau de l’une des versions suivantes ou installer une nouvelle instance de SQL Server Reporting Services qui s’exécute côte à côte avec les installations existantes :  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
-   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
 Pour plus d'informations sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez les documents suivants :  

* [Mettre à niveau vers SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server.md)
* [Effectuer une mise à niveau vers SQL Server 2016 à l’aide de l’Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
* [Installer SQL Server 2016 à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)
  
  
##  <a name="pre-upgrade-checklist"></a><a name="bkmk_upgrade_checklist"></a> Liste de contrôle préalable à la mise à niveau  
 Avant la mise à niveau vers SQL Server Reporting Services, passez en revue les éléments suivants :  
  
-   Passez en revue la configuration requise afin de déterminer si votre matériel et vos logiciels peuvent prendre en charge [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]. Pour plus d’informations, consultez [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
-   Utilisez l’outil d’analyse de configuration système (SCC, System Configuration Checker) pour analyser le serveur de rapports afin d’y déceler d’éventuels défauts pouvant empêcher la réussite de l’installation de SQL Server Reporting Services. Pour plus d'informations, consultez [Check Parameters for the System Configuration Checker](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md).  
  
-   Prenez connaissance des meilleures pratiques recommandées et de l'aide pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Security Considerations for a SQL Server Installation](../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
-   Sauvegardez votre clé symétrique. Pour plus d’informations, consultez [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
-   Sauvegardez les fichiers de configuration et bases de données du serveur de rapports. Pour plus d'informations, consultez [Backup and Restore Operations for Reporting Services](../../reporting-services/install-windows/backup-and-restore-operations-for-reporting-services.md).  
  
-   Sauvegardez toutes les personnalisations effectuées dans les répertoires virtuels [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans IIS.  
  
-   Supprimez les certificats TLS/SSL non valides.  Cela inclut les certificats expirés et que vous ne projetez pas de mettre à jour avant la mise à niveau de Reporting Services.  Les certificats non valides provoqueront l'échec de la mise à niveau et l'écriture d'un message d'erreur dans le fichier journal Reporting Services similaire au message suivant : **Microsoft.ReportingServices.WmiProvider.WMIProviderException : Un certificat SSL (Secure Sockets Layer) n'est pas configuré sur le site web.** .  
  
 Avant de mettre à niveau un environnement de production, veillez à toujours exécuter une mise à niveau de test dans un environnement de préproduction qui a la même configuration que votre environnement de production.  
  
  
## <a name="overview-of-migration-scenarios"></a>Présentation des scénarios de migration  
 Si vous effectuez une mise à niveau à partir d'une version prise en charge de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous exécutez généralement l'Assistant Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour mettre à niveau les fichiers programme du serveur de rapports, la base de données et toutes les données d'application.  
  
 Toutefois, la **migration** manuelle d'une installation du serveur de rapports est requise si vous rencontrez l'une des conditions suivantes :  
  
-   Vous souhaitez modifier le type de serveur de rapports utilisé dans votre déploiement. Par exemple, vous ne pouvez pas mettre à niveau ou convertir un serveur de rapports en mode natif vers le mode SharePoint. Pour plus d’informations, consultez [Migration du mode natif au mode SharePoint &#40;SSRS&#41;](../../reporting-services/install-windows/native-to-sharepoint-migration-ssrs.md).  
  
-   Vous souhaitez réduire la durée pendant laquelle le serveur de rapports est mis hors connexion lors du processus de mise à niveau. Votre installation actuelle reste en ligne pendant que vous copiez des données de contenu vers une nouvelle instance du serveur de rapports et testez l'installation sans modifier l'état de votre installation existante du serveur de rapports.  
  
-   Vous souhaitez migrer un déploiement SharePoint 2010 de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vers SharePoint 2013/2016. SharePoint 2013/2016 ne prend pas en charge la mise à niveau sur place à partir de SharePoint 2010. Pour plus d’informations, consultez [Migrer une installation Reporting Services &#40;mode SharePoint&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md).  
  
  
##  <a name="native-mode-upgrade-and-migration-scenarios"></a><a name="bkmk_native_scenarios"></a> Scénarios de mise à niveau et de migration en mode natif  
 **Mise à niveau :** la mise à niveau sur place pour le mode natif est identique pour chacune des versions prises en charge répertoriées plus haut dans cette rubrique. Exécutez l'Assistant Installation de SQL Server ou une installation à partir de la ligne de commande. L'installation suivante dans la base de données du serveur de rapports effectuera automatiquement la mise à niveau vers le nouveau schéma de base de données du serveur de rapports. Pour plus d’informations, consultez la section [Mise à niveau sur place](#bkmk_inplace_upgrade) de cette rubrique.  
  
 Le processus de mise à niveau commence lorsque vous sélectionnez une instance de serveur de rapports existante à mettre à niveau.  
  
1.  Si la base de données de serveur de rapports se trouve sur un ordinateur distant et que vous n'avez pas l'autorisation de mettre à jour cette base de données, le programme d'installation vous invite à fournir les informations d'identification pour mettre à jour la base de données de serveur de rapports distante. Soyez sûr de fournir les informations d'identification qui ont **sysadmin** ou les autorisations de mise à jour de la base de données.  
  
2.  L'installation vérifie les conditions ou paramètres qui empêchent la mise à niveau et lit les paramètres de configuration. Les exemples incluent les extensions personnalisées déployées sur le serveur de rapports. Si la mise à niveau est bloquée, vous devez modifier votre installation afin que la mise à niveau ne soit plus bloquée, ou migrer vers une nouvelle instance de SQL Server Reporting Services. Pour plus d'informations, consultez la documentation relative au Conseiller de mise à niveau.  
  
3.  Si la mise à niveau peut se poursuivre, le programme d'installation vous invite à la continuer.  
  
4.  Le programme d’installation crée de nouveaux dossiers pour les fichiers programme de SQL Server Reporting Services. Les dossiers de programme pour une installation de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] incluent MSRS13.\<*instance name*>.  
  
5.  L’installation ajoute les fichiers programme du serveur de rapports SQL Server Reporting Services, les outils de configuration et les utilitaires de ligne de commande qui font partie des fonctionnalités du serveur de rapports.  
  
    1.  Les fichiers programme de la version antérieure sont supprimés.  
  
    2.  Les outils de configuration et utilitaires du serveur de rapports mis à niveau vers la nouvelle version incluent l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif, les utilitaires en ligne de commande, tels que RS.exe et le Générateur de rapports.  
  
    3.  Les autres outils clients tels que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] font l’objet d’un téléchargement distinct et doivent être mis à niveau. Pour plus d’informations, consultez la page [Télécharger SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
  
    4.  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] est disponible en téléchargement séparé. Pour plus d’informations, consultez [SQL Server Data Tools dans Visual Studio 2015](/previous-versions/mt186501(v=msdn.10)).  
  
6.  Le programme d’installation réutilise l’entrée de service du Gestionnaire de services de contrôle pour le service Report Server de SQL Server Reporting Services. Cette entrée de service inclut le compte de service Windows Report Server.  
  
7.  L'installation réserve les nouvelles URL en fonction des paramètres de répertoire virtuel existants dans IIS. Comme le programme d'installation ne supprime pas toujours les répertoires virtuels dans IIS, veillez à les supprimer manuellement après la mise à niveau.  
  
8.  L'installation fusionne les paramètres dans les fichiers de configuration. En utilisant comme base les fichiers de configuration de l'installation actuelle, les nouvelles entrées sont ajoutées. Les entrées obsolètes ne sont pas supprimées, mais ne seront plus lues après la mise à niveau par le serveur de rapports. La mise à niveau ne supprime pas les anciens fichiers journaux, le fichier RSWebApplication.config obsolète ou les paramètres de répertoire virtuel dans IIS. La mise à niveau ne supprime pas les anciennes versions du Concepteur de rapports, de Management Studio ou d’autres outils clients. Si vous n'en avez plus besoin, veillez à supprimer ces fichiers et outils après la mise à niveau.  
  
 **Migration :** la migration à partir d’une version précédente d’une installation en mode natif vers SQL Server Reporting Services est identique à celle de toutes les versions prises en charge listées plus haut dans cette rubrique. Pour plus d’informations, consultez [Migrer une installation Reporting Services &#40;mode natif&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md).  
  
  
##  <a name="upgrade-a-reporting-services-native-mode-scale-out-deployment"></a><a name="bkmk_native_scaleout"></a> Mettre à niveau un déploiement par montée en puissance parallèle en mode natif Reporting Services  
 Voici un récapitulatif de la procédure de mise à niveau d’un déploiement en mode natif [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui fait l’objet d’un scale-out sur plusieurs serveurs de rapports. Ce processus implique des temps morts du déploiement de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
1.  Sauvegardez les bases de données et les clés de chiffrement du serveur de rapports. Pour plus d’informations, consultez [Opérations de sauvegarde et de restauration pour Reporting Services](../../reporting-services/install-windows/backup-and-restore-operations-for-reporting-services.md) et [Ajouter et supprimer des clés de chiffrement pour un déploiement par scale-out &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md).  
  
2.  Utilisez le gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour supprimer tous les serveurs de rapports du déploiement avec montée en puissance parallèle. Pour plus d’informations, consultez [Configurer un déploiement par scale-out de serveur de rapports en mode natif &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
3.  Mettez à niveau l’un des serveurs de rapports vers SQL Server Reporting Services.  
  
4.  Utilisez le gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour rajouter les serveurs de rapports au déploiement avec montée en puissance parallèle. Pour plus d’informations, consultez [Configurer un déploiement par scale-out de serveur de rapports en mode natif &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
     Pour chaque serveur, répétez les étapes de mise à niveau et montée en puissance parallèle.  
  
##  <a name="sharepoint-mode-upgrade-and-migration-scenarios"></a><a name="bkmk_sharePoint_scenarios"></a> Scénarios de mise à niveau et de migration en mode SharePoint  
 Les sections suivantes décrivent les problèmes et les étapes de base nécessaires pour mettre à niveau ou migrer des versions spécifiques du mode SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vers le mode SharePoint de SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Il existe deux composants d'installation pour mettre à niveau un déploiement en mode SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
    > [!TIP]  
    >  Utilisez l'applet de commande SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]`Get-SPRSServiceApplicationServers` pour déterminer quels serveurs de la batterie de serveurs SharePoint exécutent le Service partagé SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et, par conséquent, doivent être mis à niveau.  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Complément pour les produits SharePoint. Pour plus d’informations, consultez [Installer ou désinstaller le complément Reporting Services pour SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md).  
  
 Pour plus d’informations sur la migration d’une installation en mode SharePoint, consultez [Migrer une installation Reporting Services &#40;mode SharePoint&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md).  
  
> [!IMPORTANT]  
>  Certains des scénarios suivants requièrent l'arrêt de l'environnement SharePoint en raison des différentes technologies devant être mises à niveau. Si votre situation ne permet pas de temps d'arrêt, vous devez effectuer une migration complète au lieu d'une mise à niveau sur place.  
  
### <a name="sssql14-to-sql-server-reporting-services"></a>[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] vers SQL Server Reporting Services  
 **Environnement de départ :** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP1, SharePoint 2010 ou SharePoint 2013.  
  
 **Environnement final :** SQL Server Reporting Services, SharePoint 2013 ou SharePoint 2016.   
  
-   **SharePoint 2013/2016 :** SharePoint 2013/2016 ne prend pas en charge la mise à niveau sur place à partir de SharePoint 2010. Cependant, la procédure de **mise à niveau avec liaison des bases de données est prise en charge**  .
  
     Si vous avez une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée à SharePoint 2010, vous ne pouvez pas effectuer une mise à niveau sur place du serveur SharePoint. Toutefois, vous pouvez migrer les bases de données de contenu et les bases de données d’application de service de la batterie de serveurs SharePoint 2010 vers une batterie de serveurs SharePoint 2013/2016.  
  
### <a name="sssql11-to-sql-server-reporting-services"></a>[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] vers SQL Server Reporting Services  
 **Environnement de départ :** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou [!INCLUDE[ssSQL11SP1](../../includes/sssql11sp1-md.md)], SharePoint 2010.  
  
 **Environnement final :** SQL Server Reporting Services, SharePoint 2013 ou SharePoint 2016.   
  
-   **SharePoint 2013/2016 :** SharePoint 2013/2016 ne prend pas en charge la mise à niveau sur place à partir de SharePoint 2010. Cependant, la procédure de **mise à niveau avec liaison des bases de données est prise en charge**  .
  
     Si vous avez une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée à SharePoint 2010, vous ne pouvez pas effectuer une mise à niveau sur place du serveur SharePoint. Toutefois, vous pouvez migrer les bases de données de contenu et les bases de données d’application de service de la batterie de serveurs SharePoint 2010 vers une batterie de serveurs SharePoint 2013/2016.  
  
### <a name="sskilimanjaro-to-sql-server-reporting-services"></a>[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] vers SQL Server Reporting Services  
 **Environnement de départ :** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], SharePoint 2010.  
  
 **Environnement final :** SQL Server Reporting Services, SharePoint 2013 ou SharePoint 2016.  
 
-   **SharePoint 2013/2016 :** SharePoint 2013/2016 ne prend pas en charge la mise à niveau sur place à partir de SharePoint 2010. Cependant, la procédure de **mise à niveau avec liaison des bases de données est prise en charge**  .

    SharePoint doit être migré avant de pouvoir mettre à niveau Reporting Services.
  
-   Installez la version SQL Server Reporting Services du complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour SharePoint sur chaque serveur web frontal de la batterie de serveurs. Vous pouvez installer le complément à l’aide de l’Assistant Installation de SQL Server Reporting Services ou en le téléchargeant.  
  
-   Exécutez l’installation de SQL Server Reporting Services pour mettre à niveau le mode SharePoint pour chaque serveur de rapports. L’Assistant installation de SQL Server installe le service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et crée une nouvelle application Service. 
  
  
##  <a name="considerations-for-a-migration"></a><a name="bkmk_migration_considerations"></a> Considérations relatives à une migration  
 Lorsque vous déplacez des données d'application, tenez compte des problèmes et restrictions suivants :  
  
-   La protection de la clé de chiffrement inclut un hachage qui incorpore l'identité de l'ordinateur.  
  
-   Les noms de bases de données du serveur de rapports sont fixes et ne peuvent pas être renommés sur le nouvel ordinateur.  
  
### <a name="encryption-key-considerations"></a>Considérations sur la clé de chiffrement  
 Effectuez toujours une sauvegarde des clés de chiffrement avant de déplacer une base de données du serveur de rapports vers un nouvel ordinateur.  
  
 Le déplacement d'une installation du serveur de rapports vers un autre ordinateur invalidera le hachage qui protège les clés de chiffrement utilisées pour aider à sécuriser des données sensibles stockées dans la base de données du serveur de rapports. Chaque instance du serveur de rapports qui utilise la base de données a sa copie de la clé de chiffrement, laquelle est chiffrée avec l'identité du compte de service telle qu'elle est définie sur l'ordinateur actuel. Si vous changez d'ordinateurs, le service n'aura plus accès à sa clé, même si vous utilisez le même nom de compte sur le nouvel ordinateur.  
  
 Pour rétablir le chiffrement réversible sur le nouveau serveur de rapports, vous devez restaurer la clé que vous avez précédemment sauvegardée. L'ensemble complet de clés qui est stocké dans la base de données du serveur de rapports est composé d'une valeur de clé symétrique ainsi que des informations d'identité de service utilisées pour restreindre l'accès à la clé afin qu'elle puisse être utilisée uniquement par l'instance du serveur de rapports qui l'a stockée. Pendant la restauration de la clé, le serveur de rapports remplace les copies existantes de la clé par les nouvelles versions. La nouvelle version inclut les valeurs d'identité du service et de l'ordinateur, telles que définies sur l'ordinateur actuel. Pour plus d'informations, voir les rubriques suivantes :  
  
-   Mode SharePoint : Consultez la section « Gestion des clés » de l’article [Gérer une application de service Reporting Services SharePoint](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)  
  
-   Mode natif : Consultez [Sauvegarder et restaurer les clés de chiffrement Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)  
  
  
### <a name="fixed-database-name"></a>Nom de base de données fixe  
 Vous ne pouvez pas renommer la base de données du serveur de rapports. L'identité de la base de données est enregistrée dans des procédures stockées du serveur de rapports lors de la création de la base de données. Renommer les bases de données primaires ou temporaires du serveur de rapports provoquera des erreurs lors de l'exécution des procédures, invalidant alors votre installation du serveur de rapports.  
  
 Si le nom de la base de données de l'installation existante ne convient pas pour la nouvelle installation, vous devez envisager de créer une base de données portant le nom souhaité, puis de charger les données d'application existantes à l'aide des techniques énumérées ci-dessous :  
  
-   Écrivez un script [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] qui appelle des méthodes SOAP du service Web Report Server pour copier des données entre des bases de données. Vous pouvez utiliser l'utilitaire RS.exe pour exécuter le script. Pour plus d’informations sur cette approche, consultez [Scripts et PowerShell avec Reporting Services](../../reporting-services/tools/scripting-and-powershell-with-reporting-services.md).  
  
-   Écrivez du code qui appelle le fournisseur WMI pour copier des données entre des bases de données. Pour plus d’informations sur cette approche, consultez [Accès au fournisseur WMI de Reporting Services](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  
  
-   Si vous avez seulement quelques éléments, vous pouvez republier des rapports et des sources de données partagées à partir du Concepteur de rapports, du Générateur de modèles et du Générateur de rapports vers le nouveau serveur de rapports. Vous devez recréer les attributions de rôles, les abonnements, les planifications partagées, les planifications d'instantanés de rapports, les propriétés personnalisées que vous définissez sur les rapports ou d'autres éléments, la sécurité des éléments de modèle et les propriétés que vous définissez sur le serveur de rapports. L'historique de rapport et les données du journal des exécutions des rapports seront perdus.  
  
  
##  <a name="additional-resources"></a><a name="bkmk_additional_resources"></a> Ressources supplémentaires  
  
> [!NOTE]  
>  Pour plus d'informations sur la mise à niveau avec liaison des bases de données SharePoint, consultez les rubriques suivantes :  
  
-   [Présentation de la procédure de mise à niveau vers SharePoint 2016](https://technet.microsoft.com/library/cc262483\(v=office.16\)).

-   [Présentation de la procédure de mise à niveau vers SharePoint 2013](/SharePoint/upgrade-and-update/overview-of-the-upgrade-process-from-sharepoint-2010-to-sharepoint-2013).
  
-   [Nettoyer les préparations avant la mise à niveau vers SharePoint 2013](/SharePoint/upgrade-and-update/clean-up-an-environment-before-an-upgrade-to-sharepoint-2013).  
  
-   [Mettre à niveau les bases de données de SharePoint 2013 vers SharePoint 2016](https://technet.microsoft.com/library/cc303436\(v=office.16\)).

-   [Mettre à niveau les bases de données de SharePoint 2010 vers SharePoint 2013](/SharePoint/upgrade-and-update/upgrade-content-databases-from-sharepoint-2010-to-sharepoint-2013).  

## <a name="next-steps"></a>Étapes suivantes

[Rapports de mise à niveau](../../reporting-services/install-windows/upgrade-reports.md)   
[Effectuer une mise à niveau vers SQL Server 2016 à l’aide de l’Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)