---
title: Écrire des scripts pour les tâches d’administration et de déploiement | Microsoft Docs
description: Découvrez les outils, les technologies et les stratégies permettant d’automatiser l’installation, le déploiement et les tâches administratives de routine pour un serveur de rapports.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- scripts [Reporting Services]
- moving reports
- report servers [Reporting Services], duplicating settings
- deploying [Reporting Services], scripts
- copying report server settings
- administrative tasks [Reporting Services]
- duplicating report server environment
- migrating reports [Reporting Services]
- scripts [Reporting Services], deployments
- transferrng reports
- reports [Reporting Services], migrating
ms.assetid: d0416c9e-e3f9-456d-9870-2cfd2c49039b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 883acc6af22c69b6deda8606c2361e33e55aeb1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077920"
---
# <a name="script-deployment-and-administrative-tasks"></a>Écrire des scripts pour les tâches d'administration et de déploiement

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] prend en charge l'utilisation de scripts pour automatiser les installations courantes, le déploiement et les tâches administratives. Le déploiement d'un serveur de rapports est un processus qui implique plusieurs étapes. Vous devez faire appel à plusieurs outils et processus pour configurer un déploiement ; il n'existe pas un seul programme ou une seule approche pour automatiser l'ensemble des tâches.  
  
 Toutes les étapes ne doivent pas être automatisées. Dans certains cas, la réalisation d'une étape manuellement ou par le biais d'un outil graphique constitue l'approche la plus simple et la plus efficace. Par exemple, si vous souhaitez déployer un grand nombre de rapports et de modèles; il est préférable de copier les bases de données du serveur de rapports plutôt que d'écrire le code qui recrée l'environnement de ce dernier.  
  
 Certaines étapes exigent un code personnalisé. Par exemple, vous pouvez automatiser la configuration des URL pour le service Web et le Gestionnaire de rapports, mais uniquement si vous écrivez un code personnalisé qui soumet des appels au fournisseur WMI (Windows Management Instrumentation) Report Server. Si vous ne souhaitez pas écrire le code, vous devez utiliser l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour effectuer cette étape.  
  
 Pour exécuter un script qui configure un serveur de rapports, vous devez être un administrateur local sur l'ordinateur que vous configurez. Pour plus d’informations, consultez [Configurer un serveur de rapports pour l’administration à distance](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md).  
  
 Cette rubrique décrit les approches recommandées pour l'automatisation de tâches spécifiques. Divers programmes et interfaces de programmation sont mentionnés ; la description de chacun d'eux est fournie plus loin dans cette rubrique.  
  
## <a name="deployment-tasks-and-how-to-automate-them"></a>Tâches de déploiement et mode d'automatisation  
 Le tableau suivant résume les tâches d'installation et de configuration nécessaires au déploiement d'un serveur de rapports. Vous pouvez vous reporter à ce tableau pour associer une tâche spécifique à une approche vous permettant d'automatiser ou d'effectuer la tâche sans assistance.  
  
|Tâche|Approche|  
|----------|--------------|  
|Installez [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].|Vous pouvez exécuter une installation sans assistance à partir de la ligne de commande.<br /><br /> Vous pouvez utiliser le programme d'installation pour installer et configurer un serveur de rapports, mais uniquement si vous spécifiez l'option de configuration par défaut et si votre système répond à toutes les exigences de ce type d'installation. Si l'installation de la configuration par défaut est impossible, procédez à une installation de fichiers uniquement.|  
|Configurez le compte de service.|Le compte de service est configuré initialement à travers l'installation. Pour automatiser les modifications du compte de service comme tâche de post-installation, vous devez écrire le code personnalisé qui soumet les appels au fournisseur WMI Report Server. Il n'existe pas d'utilitaire d'invite de commandes ou de modèle de script pour configurer le compte de service par programme.<br /><br /> Si les exigences du code vous empêchent d'automatiser cette étape, vous pouvez facilement configurer le compte manuellement à l'aide de l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez [Configurer un compte de service &#40;Gestionnaire de configuration du serveur de rapports&#41;](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).|  
|Configurez les URL du service Web Report Server et du Gestionnaire de rapports.|Vous devez écrire le code personnalisé chargé de soumettre les appels au fournisseur WMI Report Server. Il n'existe pas d'utilitaire de ligne de commande ou de modèle de script pour configurer les URL.<br /><br /> Si vous souhaitez éviter d'écrire le code, vous pouvez configurer manuellement les URL en exécutant l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez [Configurer une URL &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md).|  
|Créez la base de données du serveur de rapports.|Vous devez écrire le code personnalisé chargé de soumettre les appels au fournisseur WMI Report Server. Il n'existe pas d'utilitaire d'invite de commandes ou de modèle de script pour créer les bases de données du serveur de rapports et RSExecRole.<br /><br /> Si vous souhaitez éviter d'écrire le code, vous pouvez créer la base de données manuellement en exécutant l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez [Créer une base de données du serveur de rapports en mode natif &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).|  
|Configurez la connexion à la base de données du serveur de rapports.|Si vous modifiez la chaîne de connexion, le compte ou le mot de passe, ou le type d’authentification, exécutez l’utilitaire **rsconfig** pour configurer la connexion. Pour plus d’informations, consultez [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md) et [Utilitaire rsconfig &#40;SSRS&#41;](../../reporting-services/tools/rsconfig-utility-ssrs.md).<br /><br /> L'utilisation de cet utilitaire n'est pas possible pour la création ou la mise à niveau de la base de données. La base de données et RSExecRole doivent déjà exister.|  
|Configurer un déploiement avec montée en puissance parallèle.|Choisissez l'une des approches suivantes pour automatiser un déploiement avec montée en puissance parallèle :<br /><br /> -   Exécutez l’utilitaire rskeymgmt.exe pour joindre les instances du serveur de rapports à une installation existante. Pour plus d’informations, consultez [Ajouter et supprimer des clés de chiffrement pour un déploiement par scale-out &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md).<br />-   Écrivez le code personnalisé à exécuter sur le fournisseur WMI Report Server.|  
|Sauvegarder les clés de chiffrement.|Choisissez l'une des approches suivantes pour automatiser la sauvegarde des clés de chiffrement :<br /><br /> -   Exécutez l’utilitaire rskeymgmt.exe pour sauvegarder les clés. Pour plus d’informations, consultez [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).<br />-   Écrivez le code personnalisé à exécuter sur le fournisseur WMI Report Server.|  
|Configurez la messagerie Report Server.|Écrivez le code personnalisé à exécuter sur le fournisseur WMI de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Le fournisseur prend en charge un sous-ensemble des paramètres de configuration de la messagerie.<br /><br /> Bien que le fichier RSReportServer.config contienne tous les paramètres, n'utilisez pas ce fichier de manière automatisée. En particulier, n'utilisez pas un fichier de commandes pour copier le fichier sur un autre serveur de rapports. Chaque fichier de configuration inclut des valeurs qui sont spécifiques à l'instance active. Ces valeurs ne sont pas valides sur les autres instances du serveur de rapports.<br /><br /> Pour plus d’informations sur les paramètres, consultez [Paramètres d’e-mail : mode natif de Reporting Services (Gestionnaire de configuration)](../install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md).|  
|Configurer le compte d'exécution sans assistance.|Choisissez l'une des approches suivantes pour automatiser la configuration du compte de traitement sans assistance :<br /><br /> -   Exécutez l’utilitaire rsconfig.exe pour configurer le compte. Pour plus d’informations, consultez [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).<br />-   Écrivez le code personnalisé qui effectue les appels au fournisseur WMI Report Server.|  
|Déployez le contenu existant sur un autre serveur de rapports, y compris l'arborescence des dossiers, les attributions de rôles, les rapports, les abonnements, les planifications, les sources de données et les ressources.|Le meilleur moyen de recréer un environnement de serveur de rapports existant est de copier la base de données du serveur de rapports dans une nouvelle instance du serveur de rapports.<br /><br /> Une autre approche consiste à écrire un code personnalisé capable de recréer par programme le contenu du serveur de rapports existant. Soyez toutefois conscient du fait que les abonnements, les instantanés de rapports et l'historique des rapports ne peuvent pas être recréés par programme.<br /><br /> Certains déploiements peuvent tirer parti de l'utilisation conjointe de ces techniques (soit restaurer une base de données du serveur de rapports, puis exécuter un code personnalisé qui modifie cette base de données pour une installation précise).<br /><br /> Pour obtenir un exemple détaillé, consultez [Exemple de script Reporting Services rs.exe pour copier du contenu entre des serveurs de rapports](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).<br /><br /> Pour plus d’informations sur le déplacement d’une base de données du serveur de rapports, consultez [Déplacement des bases de données du serveur de rapports vers un autre ordinateur &#40;mode natif SSRS&#41;](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md). Pour plus d'informations sur la création par programme d'un environnement de serveur de rapports, consultez la section « Utilisation de scripts pour migrer les dossiers et le contenu du Report Server » plus loin dans cette rubrique.|  
  
## <a name="tools-and-technologies-for-automating-server-deployment"></a>Outils et technologies d'automatisation de déploiement de serveur  
 La liste ci-après répertorie les programmes et les interfaces qu'il est possible d'utiliser pour automatiser les tâches de déploiement et de maintenance :  
  
-   Vous pouvez exécuter le programme d'installation en mode sans assistance pour installer et parfois configurer les composants du serveur de rapports. Vous devez choisir l'option d'installation de fichiers uniquement pour que le programme d'installation configure une instance du serveur de rapports.  
  
-   Vous pouvez faire appel au fournisseur WMI de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et aux utilitaires de ligne de commande de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour la configuration des serveurs locaux et distants.  
  
     Le fournisseur WMI de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] expose les classes, les propriétés et les méthodes qui vous permettent de configurer tous les aspects d’une installation de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , notamment les comptes de service, la configuration des URL, la création et la configuration de la base de données du serveur de rapports ou la configuration d’un serveur de rapports pour la remise par e-mail. Vous devez écrire un code personnalisé ou un script pour utiliser le fournisseur WMI. Pour plus d’informations, consultez [Accéder au fournisseur WMI de Reporting Services](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  
  
     Une solution autre que l'écriture de code est le recours aux utilitaires de ligne de commande (rsconfig.exe et rskeymgmt.exe). Vous pouvez créer des fichiers de commandes chargés d'exécuter les utilitaires. Vous pouvez exploiter les utilitaires afin d'automatiser certaines tâches de configuration, mais pas toutes.  
  
-   L’outil hôte de scripts du serveur de rapports (rs.exe) peut exécuter du code [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] personnalisé que vous pouvez écrire pour recréer ou déplacer un contenu existant d’un serveur de rapports vers un autre. En optant pour cette approche, vous pouvez écrire un script dans [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], l'enregistrer en tant que fichier .rss et utiliser l'outil rs.exe pour exécuter le script sur le serveur de rapports cible. Le script que vous écrivez peut appeler l'interface SOAP sur le service Web de Report Server. Cette approche est employée pour l'écriture des scripts de déploiement parce qu'elle vous permet de recréer un espace de noms et le contenu du dossier du serveur de rapports et de recréer la sécurité basée sur les rôles.  
  
-   La version SQL Server 2012 inclut des applets de commande PowerShell pour le mode intégré SharePoint. Vous pouvez utiliser PowerShell pour configurer et administrer l'intégration SharePoint.  Pour plus d’informations, consultez [Applets de commande PowerShell pour le mode SharePoint de Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).  
  
## <a name="use-scripts-to-migrate-report-server-content-and-folders"></a>Utiliser des scripts pour migrer les dossiers et le contenu du serveur de rapports  
 Vous pouvez écrire des scripts capables de dupliquer un environnement de serveur de rapports sur une autre instance du serveur de rapports. En règle générale, les scripts de déploiement sont écrits en [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] , puis traités à l'aide de l'utilitaire de l'environnement d'exécution de scripts du serveur de rapports.  
  
 Pour obtenir un exemple détaillé, consultez [Exemple de script Reporting Services rs.exe pour copier du contenu entre des serveurs de rapports](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).  
  
 Les scripts s'utilisent pour copier des dossiers, des sources de données partagées, des ressources, des rapports, des attributions de rôles et des paramètres d'un serveur sur un autre. Vous écrivez un script pour une instance de serveur de rapports, puis vous l'exécutez sur un autre serveur pour recréer l'espace de noms du serveur de rapports. Si votre déploiement [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] contient plusieurs serveurs de rapports, vous pouvez exécuter le script sur chaque serveur individuellement de façon configurer tous les serveurs de la même manière.  
  
 La liste suivante décrit les étapes nécessaires à la migration de rapports d'un serveur vers un autre.  
  
1.  Définissez votre variable de script sur l'URL du serveur de rapports source.  
  
2.  Utilisez les méthodes <xref:ReportService2010.ReportingService2010.GetItemDefinition%2A> et <xref:ReportService2010.ReportingService2010.GetProperties%2A> pour récupérer la définition et les propriétés du rapport.  
  
3.  Définissez l'URL de manière à pointer vers le serveur de destination.  
  
4.  Utilisez la méthode <xref:ReportService2010.ReportingService2010.CreateCatalogItem%2A> , en passant les propriétés retournées par <xref:ReportService2010.ReportingService2010.GetProperties%2A> et la définition de rapport retournée par <xref:ReportService2010.ReportingService2010.GetItemDefinition%2A>.  
  
 En utilisant une combinaison de méthodes « get » et « create », vous pouvez suivre une procédure analogue pour la migration des paramètres, des dossiers, des sources de données partagées et des ressources. Pour plus d’informations sur les méthodes disponibles, consultez [Informations techniques de référence &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md).  
  
> [!NOTE]  
>  Les scripts s’exécutent avec les informations d’identification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows de l’utilisateur qui exécute le script, sauf si les informations d’identification sont définies explicitement.  
  
 Pour plus d’informations la mise en forme et l’exécution d’un fichier de script, consultez [Écrire des scripts avec l’utilitaire rs.exe et le service Web](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md).  
  
## <a name="using-scripts-to-set-server-properties"></a>Utilisation de scripts pour définir les propriétés du serveur  
 Vous pouvez écrire un script qui définit les propriétés système du serveur de rapports. Le script [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET suivant présente une méthode de définition des propriétés. Cet exemple désactive le contrôle RSClientPrint ActiveX, mais vous pouvez remplacer **EnableClientPrinting** et **False** par le nom et la valeur d'une propriété valide. Pour afficher la liste complète des propriétés de serveur, consultez [Report Server System Properties](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md).  
  
 Pour utiliser le script, enregistrez-le dans un fichier doté d'une extension .rss, puis utilisez l'utilitaire de ligne de commande rs.exe pour exécuter le fichier sur le serveur de rapports. Comme le script n'est pas compilé, il n'est pas nécessaire de disposer d'une installation de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. Cet exemple part du principe que vous disposez d'autorisations sur l'ordinateur local qui héberge le serveur de rapports. Si vous n'êtes pas connecté sous un compte possédant des autorisations, vous devez spécifier des informations de compte par le biais d'arguments de ligne de commande supplémentaires. Pour plus d’informations, consultez [Utilitaire RS.exe &#40;SSRS&#41;](../../reporting-services/tools/rs-exe-utility-ssrs.md).  
  
> [!TIP]  
>  Pour obtenir un exemple détaillé, consultez [Exemple de script Reporting Services rs.exe pour copier du contenu entre des serveurs de rapports](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).  
  
```  
Public Sub Main()  
        Dim props(0) As [Property]  
        Dim setProp As New [Property]  
        setProp.Name = "EnableClientPrinting"  
        setProp.Value = "False"   
        props(0) = setProp  
        Try  
            rs.SetSystemProperties(props)  
        Catch ex As System.Web.Services.Protocols.SoapException  
            Console.Write(ex.Detail.InnerXml)  
        Catch e as Exception  
            Console.Write(e.Message)  
        End Try  
End Sub  
```

## <a name="next-steps"></a>Étapes suivantes

[Méthode GenerateDatabaseCreationScript &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabasecreationscript.md)   
[Méthode GenerateDatabaseRightsScript &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaserightsscript.md)   
[Méthode GenerateDatabaseUpgradeScript &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaseupgradescript.md)   
[Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
[Installer le serveur de rapports Reporting Services en mode natif](~/reporting-services/install-windows/install-reporting-services-native-mode-report-server.md)   
[Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
[Utilitaires d’invite de commandes du serveur de rapports &#40;SSRS&#41;](../../reporting-services/tools/report-server-command-prompt-utilities-ssrs.md)   
[Planification de la prise en charge des navigateurs pour Reporting Services et Power View (Reporting Services 2014)](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)   
[Outils de Reporting Services](../../reporting-services/tools/reporting-services-tools.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
