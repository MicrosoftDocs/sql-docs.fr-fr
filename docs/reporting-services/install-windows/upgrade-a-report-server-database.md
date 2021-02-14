---
description: Mettre à niveau une base de données du serveur de rapports
title: Mettre à niveau une base de données du serveur de rapports | Microsoft Docs
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- upgrading databases
- report server database
- upgrading Reporting Services
ms.assetid: 4091cf87-9d97-4048-a393-67f1f9207401
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: cf6cc7dc672e14a0fd988354ba4900fb49c206e3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075900"
---
# <a name="upgrade-a-report-server-database"></a>Mettre à niveau une base de données du serveur de rapports

La base de données du serveur de rapports offre un espace de stockage pour une ou plusieurs instances du serveur de rapports. Comme le schéma de base de données du serveur de rapports peut changer à chaque nouvelle version de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], il est nécessaire que la version de la base de données corresponde à la version de l'instance du serveur de rapports que vous utilisez. Dans la plupart des cas, une base de données du serveur de rapports peut être mise à niveau automatiquement sans aucune intervention de votre part.  
  
 **Mode natif** : En mode natif [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], la base de données de serveur de rapports est composée de deux bases de données qui portent les noms par défaut ReportServer et ReportServerTempDB.  

::: moniker range="=sql-server-2016"
  
 **Mode SharePoint :** en mode SharePoint SQL Server 2016 Reporting Services, la base de données du serveur de rapports est une collection de bases de données créée pour chaque instance de l’application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  

::: moniker-end

## <a name="ways-to-upgrade-a-native-mode-report-server-database"></a>Méthodes de mise à niveau d'une base de données de serveur de rapports en mode natif

 La liste suivante identifie toutes les conditions selon lesquelles une base de données du serveur de rapports est mise à niveau :  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] met à niveau une instance unique d'un serveur de rapports. Le schéma de base de données du serveur de rapports est mis à niveau automatiquement après le démarrage de service et le serveur de rapports détermine que la version du schéma de base de données ne correspond pas à la version du serveur.  
  
     Au démarrage du service, le serveur de rapports vérifie la version du schéma de base de données pour s'assurer qu'elle correspond à la version du serveur. Si la version du schéma de base de données est antérieure, elle est automatiquement mise à niveau vers la version du schéma requise par le serveur de rapports. La mise à niveau automatique est particulièrement utile si vous avez restauré ou joint une base de données du serveur de rapports plus ancienne. Un message est entré dans le fichier journal de suivi du serveur de rapports, indiquant que la version du schéma de base de données a été mise à niveau.  
  
-   Le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] met à niveau une base de données locale ou distante du serveur de rapports lorsque vous sélectionnez une version antérieure à utiliser avec une instance plus récente du serveur de rapports. Dans ce cas, vous devez confirmer l'action de mise à niveau avant qu'elle ne se produise.  
  
     Le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne fournit plus de bouton Mettre à niveau distinct ni de script de mise à niveau. Ces fonctionnalités sont obsolètes à compter de la version [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] en raison de la fonction de mise à niveau automatique du service Report Server.  
  
 Une fois le schéma mis à niveau, vous ne pouvez pas restaurer la mise à niveau dans une version antérieure. Pensez toujours à sauvegarder la base de données de serveur de rapports, au cas où vous devriez recréer une précédente installation.  
  
## <a name="how-the-schema-metadata-and-report-server-content-is-updated"></a>Comment le schéma, les métadonnées et le serveur de rapports sont mis à jour  
 La mise à niveau de la base de données de serveur de rapports se déroule en trois étapes :  
  
1.  Le schéma est mis à niveau automatiquement après l'installation et le démarrage du service, ou lorsque vous sélectionnez une base de données du serveur de rapports en mode natif [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , qui est une ancienne version. De plus, le service Report Server vérifie la version de la base de données au démarrage. Si le serveur de rapports est connecté à une base de données qui est une version antérieure, il met à jour la base de données lors du démarrage.  
  
2.  Les descripteurs de sécurité sont mis à niveau lors la première utilisation de la base de données de serveur de rapports après la mise à jour du schéma.  
  
3.  Les rapports publiés et les instantanés de rapports compilés sont mis à jour lors de la première utilisation. Pour plus d'informations, consultez [Mettre à niveau des rapports](../../reporting-services/install-windows/upgrade-reports.md).  
  
 Outre la base de données de serveur de rapports, un serveur de rapports utilise également une base de données temporaire. La base de données temporaire est mise à niveau automatiquement lors de la mise à niveau de la base de données de serveur de rapports.  
  
## <a name="permissions-required-to-upgrade-a-report-server-database"></a>Autorisations obligatoires pour la mise à niveau d'une base de données du serveur de rapports  
 Si vous mettez à niveau une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui inclut une base de données de serveur de rapports, vous pouvez obtenir un message d'erreur si la mise à niveau de base de données est effectuée avec des autorisations insuffisantes. Par défaut, le programme d'installation utilise le jeton de sécurité de l'utilisateur qui exécute le programme d'installation pour se connecter à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] distante et mettre à jour le schéma. Si vous disposez d’autorisations de niveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **sysadmin** sur le serveur de base de données qui héberge les bases de données du serveur de rapports, la mise à niveau de la base de données aboutira. De même, si vous lancez le programme d’installation à partir de l’invite de commandes et spécifiez les arguments RSUPGRADEDATABASEACCOUNT et RSUPGRADEPASSWORD pour un compte qui possède l’autorisation **sysadmin** permettant de modifier le schéma sur l’ordinateur distant, la mise à niveau de la base de données s’effectuera sans problème.  
  
 En revanche, si vous ne bénéficiez pas d’une autorisation **sysadmin** concernant la base de données de l’ordinateur distant, la connexion sera refusée et un message d’erreur s’affichera, indiquant que :  
  
 `"Setup was not able to upgrade the report server database schema. You must update the database schema manually after setup is finished. To update the schema, run the Report Server Configuration Manager, open the Database Setup page, re-select the database, and click Apply. The database will be upgraded automatically."`  
  
 À ce stade, les fichiers programme du serveur de rapports sont mis à niveau, mais la base de données du serveur de rapports reste au format de la version précédente. Le serveur de rapports n'est pas disponible tant que vous n'avez pas fini le processus de mise à niveau en mettant à niveau la base de données manuellement.  
  
#### <a name="to-upgrade-a-native-mode-database-with-scripts"></a>Pour mettre à niveau une base de données en mode natif avec des scripts  
 Vous pouvez utiliser des scripts WMI pour mettre à niveau une base de données du serveur de rapports. Pour plus d’informations, consultez [méthode GenerateDatabaseUpgradeScript &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaseupgradescript.md)  
  
## <a name="next-steps"></a>Étapes suivantes

[Gestionnaire de configuration du serveur de rapports](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
[Créer une base de données du serveur de rapports](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
[Mettre à niveau et migrer Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[Migrer une installation Reporting Services](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
