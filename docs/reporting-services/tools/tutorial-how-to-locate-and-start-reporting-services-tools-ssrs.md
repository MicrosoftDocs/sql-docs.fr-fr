---
title: 'Tutoriel : Localiser et démarrer les outils Reporting Services | Microsoft Docs'
description: Découvrez les outils permettant de configurer un serveur de rapports, de gérer le contenu et les opérations d’un serveur de rapports, ainsi que de créer et de publier des rapports Reporting Services paginés et mobiles.
ms.date: 12/09/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.custom: seodec18
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, tutorials
- Reporting Services, tools
- Reporting Services Configuration tool
- Business Intelligence Development Studio, locating and starting tool
- Model Designer [Reporting Services], locating and starting tool
- Report Designer [Reporting Services], tutorials
- tools [Reporting Services]
- tutorials [Reporting Services]
- Report Builder
ms.assetid: 51ad69d8-fe92-4662-a7cd-d235692f0c03
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 60d621e9bb833615aaed5e6f622afb9591916037
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597062"
---
# <a name="tutorial-how-to-locate-and-start-reporting-services-tools-ssrs"></a>Tutoriel : Comment localiser et démarrer les outils Reporting Services (SSRS)

Ce didacticiel présente les outils permettant de configurer un serveur de rapports, de gérer le contenu et les opérations d’un serveur de rapports, et de créer et publier des rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] paginés et mobiles. Si vous connaissez déjà les différents outils, vous pouvez passer aux autres didacticiels pour savoir comment tirer pleinement parti de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour obtenir d’autres didacticiels, consultez [Didacticiels sur Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md).

## <a name="report-server-configuration-manager-native-mode"></a><a name="bkmk_configuration_manager"></a> Gestionnaire de configuration du serveur de rapports (mode natif)
Utilisez le Gestionnaire de configuration en mode natif pour effectuer les opérations suivantes :

- Spécifiez le compte de service.
- Créez ou mettez à niveau la base de données de serveur de rapports.
- Modifiez les propriétés de connexion.
- Spécifiez des URL.
- Gérez les clés de chiffrement.
- Configurez le traitement des rapports sans assistance et la remise de rapports par courrier électronique.

**Installation :** Le Gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est installé en même temps que le mode natif de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour plus d’informations, consultez [Installer le serveur de rapports Reporting Services en mode natif](../../reporting-services/install-windows/install-reporting-services-native-mode-report-server.md).

### <a name="to-start-the-report-server-configuration-manager"></a>Pour démarrer le Gestionnaire de configuration du serveur de rapports

1. Dans l’écran de démarrage de Windows, tapez **reporting** et, dans les résultats de la recherche **Applications**, cliquez sur **Gestionnaire de configuration du serveur de rapports**.

    ![Gestionnaire de configuration du serveur de rapports au démarrage](../../reporting-services/tools/media/bi-ssrs-configmanager-win8-startscreen.gif "Gestionnaire de configuration du serveur de rapports au démarrage")

    **Ou**

    Cliquez sur **Démarrer**, sur **Programmes**, sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], sur **Outils de configuration**, puis sur **Gestionnaire de configuration du serveur de rapports**.

    La boîte de dialogue **Sélection de l'instance d'installation de Report Server** s'affiche pour que vous spécifiiez l'instance de serveur de rapports à configurer.

2. Dans **Nom du serveur**, spécifiez le nom de l'ordinateur sur lequel est installée l'instance de serveur de rapports. Le nom de l'ordinateur local est spécifié par défaut, mais vous pouvez également taper le nom d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] distante.

    Si vous spécifiez un ordinateur distant, cliquez sur **Rechercher** pour établir une connexion. Le serveur de rapports doit être configuré à l'avance pour l'administration à distance. Pour plus d’informations, consultez [Configurer un serveur de rapports pour l’administration à distance](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md).

3. Dans **Nom de l’instance**, choisissez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que vous souhaitez configurer. Seules les instances de serveur de rapports SQL Server 2008 et ultérieur figurent dans la liste. Vous ne pouvez pas configurer d'instances de versions antérieures de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].

4. Cliquez sur **Connecter**.

5. Pour vérifier si l'outil a été lancé, comparez vos résultats à l'image suivante :

    ![Outil de configuration de Reporting Services](../../reporting-services/tools/media/rs-ui-reportserverconfigkatmai.png "Outil de configuration de Reporting Services")

 **Étapes suivantes :** [Configurer et administrer un serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md) et [Gestionnaire de configuration du serveur de rapports &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).

## <a name="web-portal-native-mode"></a>Portail web (mode natif)

Utilisez le [Web portal (SSRS Native Mode)](../../reporting-services/web-portal-ssrs-native-mode.md) pour définir les autorisations, gérer les abonnements et les planifications, et travailler sur les rapports. Vous pouvez également utiliser le portail web pour afficher les rapports.

**Installation :** Le portail web est installé en même temps que le mode natif de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] : [Installer le serveur de rapports Reporting Services en mode natif](../../reporting-services/install-windows/install-reporting-services-native-mode-report-server.md)

Avant de pouvoir ouvrir le portail web, vous devez avoir les autorisations suffisantes (initialement, seuls les membres du groupe local Administrateurs ont les autorisations nécessaires pour accéder aux fonctionnalités du portail web). Le portail web propose des pages et options différentes en fonctions des attributions de rôle de l’utilisateur en cours. Les utilisateurs qui ne bénéficient pas d'autorisations obtiennent une page vide. Les utilisateurs avec l'autorisation d'afficher les rapports obtiennent des liens sur lesquels ils peuvent cliquer pour ouvrir les rapports. Pour en savoir plus sur les autorisations, consultez [Rôles et autorisations &#40;Reporting Services&#41;](../../reporting-services/security/roles-and-permissions-reporting-services.md).

### <a name="to-start-the-web-portal"></a>Pour démarrer le portail web

1. Ouvrez votre navigateur. Pour plus d’informations sur les versions et les navigateurs pris en charge, consultez [Planification de la prise en charge des navigateurs pour Reporting Services et Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).

2. Dans la barre d’adresses du navigateur web, tapez l’URL du portail web. Par défaut, l’URL est `https://<serverName>/reports`. Vous pouvez utiliser l'outil de configuration de Reporting Services pour confirmer le nom du serveur et l'URL. Pour plus d’informations sur les URL utilisées dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Configurer des URL de serveurs de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md).

3. Le portail web s’ouvre dans la fenêtre du navigateur. La page de démarrage est constituée du dossier Home. En fonction des autorisations, vous verrez au sein de la page de démarrage des dossiers supplémentaires, des liens vers les rapports et des fichiers de ressources. Il se peut aussi que la barre d'outils contienne des commandes et des boutons supplémentaires.

4. Si vous exécutez le portail web sur le serveur de rapports local, consultez [Configurer un serveur de rapports en mode natif pour l’administration locale &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).

## <a name="management-studio"></a><a name="bkmk_managements_studio"></a> Management Studio

Les administrateurs du serveur de rapports peuvent utiliser [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour gérer le serveur de rapports en même temps que d'autres composants serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez le tutoriel de [SQL Server Management Studio](../../ssms/quickstarts/ssms-connect-query-sql-server.md).

### <a name="to-start-sql-server-management-studio"></a>Pour démarrer SQL Server Management Studio

1. Dans l’écran de démarrage de Windows, tapez **sql server** et dans les résultats de la recherche **Applications** , cliquez sur **SQL Server Management Studio**.

    ![Management Studio sur l'écran de démarrage de Windows](../../reporting-services/tools/media/bi-ssms-win8-startscreen.gif "Management Studio sur l'écran de démarrage de Windows")

    **Ou**

    Cliquez sur **Démarrer**, puis sur **Tous les programmes**, sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]et sur **SQL Server Management Studio**. La boîte de dialogue **Se connecter au serveur** s’affiche.

2. Si la boîte de dialogue **Se connecter au serveur** n'apparaît pas, dans la liste **Explorateur d'objets**, cliquez sur **Se connecter** , puis sélectionnez **Reporting Services**.

3. Dans la liste **Type de serveur** , sélectionnez **Reporting Services**. Si [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne figure pas dans la liste, c'est qu'il n'est pas installé.

4. Dans la liste **Nom du serveur** , sélectionnez une instance de serveur de rapports. Les instances locales figurent dans la liste. Vous pouvez aussi taper le nom d'une instance distante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

5. Cliquez sur **Connecter**. Vous pouvez développer le nœud racine pour définir les propriétés du serveur, modifier les définitions de rôle ou désactiver des fonctionnalités du serveur de rapports.

## <a name="sql-server-data-tools-with-report-designer-and-report-wizard"></a><a name="bkmk_ssdt"></a> SQL Server Data Tools avec le Concepteur de rapports et l'Assistant Rapport

Pour créer des rapports paginés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous avez le choix entre deux outils : le Concepteur de rapports et le [Générateur de rapports](#bkmk_report_builder).

Le Concepteur de rapports est disponible dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] -Visual Studio. L’aire de conception du Concepteur de rapports inclut des fenêtres avec onglets, des Assistants et des menus permettant d’accéder aux fonctionnalités de création de rapports. L’outil Concepteur de rapports devient disponible lorsque vous choisissez un projet Report Server ou un modèle de l’Assistant Report Server dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Pour en savoir plus, consultez [Reporting Services dans les outils de données SQL Server &#40;SSDT&#41;](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md).

Téléchargez [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md).

### <a name="to-start-report-designer"></a>Pour démarrer le Concepteur de rapports

1. Ouvrez **SQL Server Data Tools**.

2. Dans le menu **Fichier** , pointez sur **Nouveau**, puis cliquez sur **Projet**.

3. Dans la liste **Types de projets** , cliquez sur **Projets Business Intelligence**.

4. Dans la liste **Modèles** , cliquez sur **Projet Report Server**. Le schéma suivant montre comment les modèles de projet s'affichent dans la boîte de dialogue :

    ![Boîte de dialogue Nouveau modèle de projet](../../reporting-services/tools/media/rs-ui-newrsproject.gif "Boîte de dialogue Nouveau modèle de projet")

5. Tapez un nom et un emplacement pour le projet. Vous pouvez également cliquer sur **Parcourir** , puis sélectionner un emplacement.

6. [!INCLUDE[clickOK](../../includes/clickok-md.md)] [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] s'ouvre avec la page de démarrage de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. L'Explorateur de solutions fournit des catégories pour la création de rapports et de sources de données. Vous pouvez utiliser ces catégories pour créer de nouveaux rapports et de nouvelles sources de données. Les fenêtres avec onglets s'affichent quand vous créez une définition de rapport. Les fenêtres avec onglets sont les suivantes : Données, Mise en page et Aperçu.

Pour commencer votre premier rapport, consultez [Créer un rapport de table de base &#40;didacticiel SSRS&#41;](../../reporting-services/create-a-basic-table-report-ssrs-tutorial.md). Pour en savoir plus sur les concepteurs de requêtes que vous pouvez utiliser dans le Concepteur de rapports, consultez [Outils de création de requêtes &#40;SSRS&#41;](../../reporting-services/report-data/query-design-tools-ssrs.md).

## <a name="ssrbnoversion"></a><a name="bkmk_report_builder"></a> [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]

Le[Générateur de rapports de SQL Server](../../reporting-services/report-builder/report-builder-in-sql-server-2016.md) est une application autonome que vous pouvez utiliser pour créer des rapports paginés en dehors de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez personnaliser et mettre à jour tous les rapports existants, qu’ils aient été créés dans le Concepteur de rapports ou dans des versions précédentes du [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]. Installez-le à partir du portail web de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à partir du Centre de téléchargement Microsoft.

Quand votre rapport paginé est prêt, publiez-le sur un serveur de rapports ou [enregistrez-le dans le service Power BI](/power-bi/paginated-reports-save-to-power-bi-service).\
[Téléchargez le Générateur de rapports](https://go.microsoft.com/fwlink/?LinkID=219138) depuis le Centre de téléchargement Microsoft.

### <a name="to-start-ssrbnoversion"></a>Pour démarrer le [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]

1. Dans le portail web de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cliquez sur le menu **Nouveau**, puis sélectionnez **Rapport paginé**.

    ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png "PBI_SSMRP_NewMenu")

2. Si [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] n’est pas encore installé sur cet ordinateur, sélectionnez **Obtenir l’[!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]** .

    ou

    [Téléchargez le Générateur de rapports](https://go.microsoft.com/fwlink/?LinkID=219138) depuis le Centre de téléchargement Microsoft.

3. [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] s’ouvre et vous pouvez créer ou ouvrir un rapport paginé.

## <a name="ss_mobilereptpub_long"></a><a name="bkmk_mobile_report_pub"></a> [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)]

Utilisez l’[Éditeur de rapports mobiles SQL Server](../mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md) pour créer des rapports mobiles consultables dans le portail web de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et sur des appareils mobiles tels que des iPad et des iPhone.\
Vous pouvez l’installer depuis le portail web de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou le Centre de téléchargement Microsoft.

[Téléchargez l’Éditeur de rapports mobiles SQL Server](https://go.microsoft.com/fwlink/?LinkID=733527) depuis le Centre de téléchargement Microsoft.

### <a name="to-start-ss_mobilereptpub_short"></a>Pour démarrer le [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)]

1. Dans le portail web de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cliquez sur le menu **Nouveau**, puis sélectionnez **Rapport mobile**.

     ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png "PBI_SSMRP_NewMenu")

2. Si [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] n’est pas encore installé sur cet ordinateur, sélectionnez **Obtenir l’[!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)]** .

    ou

    [Téléchargez l’Éditeur de rapports mobiles SQL Server](https://go.microsoft.com/fwlink/?LinkID=733527) depuis le Centre de téléchargement Microsoft.

3. [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] s’ouvre et vous pouvez créer ou ouvrir un rapport mobile.

## <a name="next-steps"></a>Étapes suivantes

[Téléchargez l’Éditeur de rapports mobiles SQL Server](https://go.microsoft.com/fwlink/?LinkID=733527)  
[Téléchargez le Générateur de rapports](https://go.microsoft.com/fwlink/?LinkID=219138)  
[Télécharger SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)  
[Installer le mode SharePoint de Reporting Services](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)  
[Serveur de rapports Reporting Services](../../reporting-services/report-server-sharepoint/reporting-services-report-server.md)  
[Outils de création de requêtes](../../reporting-services/report-data/query-design-tools-ssrs.md)  
[Didacticiels sur Reporting Services](../../reporting-services/reporting-services-tutorials-ssrs.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)