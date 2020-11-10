---
title: Portail web d’un serveur de rapports (mode natif) | Microsoft Docs
ms.date: 01/31/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
description: Le portail web d’un serveur de rapports Reporting Services est une expérience web qui vous permet d’afficher des rapports, des rapports mobiles et des indicateurs de performance clés, ainsi que de parcourir les éléments de votre instance de serveur de rapports.
ms.topic: conceptual
ms.assetid: 7349e626-6ed5-4d21-b05f-cf042ad9ad70
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 301d77a972da39f4e3e046e5e983206e9a1ba5c9
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243756"
---
# <a name="the-web-portal-of-a-report-server-ssrs-native-mode"></a>Portail web d’un serveur de rapports (Mode natif SSRS)

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

Le portail web d’un serveur de rapports Reporting Services est une expérience web. Dans le portail, vous pouvez afficher des rapports, des rapports mobiles et des indicateurs de performance clés, et naviguer parmi les éléments de votre instance de serveur de rapports. Vous pouvez également utiliser le portail web pour administrer une instance unique du serveur de rapports.

![Capture d’écran montrant le portail SQL Server Reporting Services.](../reporting-services/media/ssrsportal.png)

## <a name="what-is-the-web-portal"></a>Qu’est-ce que le portail web ?

Vous pouvez utiliser le portail web pour effectuer les tâches suivantes :

- Afficher, rechercher, imprimer et s'abonner à des rapports,
- Créer, sécuriser et gérer l'arborescence des dossiers pour organiser les éléments sur le serveur.
- configurer la sécurité basée sur les rôles qui définit l'accès aux éléments et aux opérations, Pour plus d’informations, consultez [Définitions de rôles - Rôles prédéfinis](security/role-definitions-predefined-roles.md).
- configurer l'historique, les paramètres et les propriétés d'exécution des rapports,
- créer des planifications partagées et des sources de données partagées pour faciliter la gestion des planifications et des connexions aux sources de données,
- créer des abonnements pilotés par les données qui déploient les rapports à un nombre important de destinataires,
- créer des rapports liés pour réutiliser de différentes manières ou adapter à d'autres fins un rapport existant,
- télécharger les outils courants, notamment le Générateur de rapports et l'Éditeur de rapports mobiles,
- [créer des indicateurs de performance clés](../reporting-services/working-with-kpis-in-reporting-services.md),
- envoyer des commentaires ou effectuer des demandes de fonctionnalités.

Vous pouvez utiliser le portail web pour parcourir les dossiers du serveur de rapports ou rechercher des rapports spécifiques. Vous pouvez afficher des rapports, consulter leurs propriétés générales et leurs anciennes copies capturées dans l'historique de rapport. Selon vos autorisations, vous pouvez aussi vous abonner à ces rapports pour les recevoir dans votre boîte de réception de messagerie électronique ou dans un dossier partagé sur votre système de fichiers.

> [!NOTE]
> Pour plus d'informations sur les versions et les navigateurs pris en charge, consultez [Planification de la prise en charge des navigateurs par Reporting Services](../reporting-services/browser-support-for-reporting-services-and-power-view.md).

Le portail web est uniquement utilisé pour les serveurs de rapports qui s’exécutent en mode natif. Il n'est pas pris en charge sur un serveur de rapports que vous configurez en mode intégré SharePoint.

Certaines fonctionnalités du portail web sont disponibles uniquement dans les éditions spécifiées de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Fonctionnalités Reporting Services prises en charge par les éditions de SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

Dans le cas d'une nouvelle installation, seuls les administrateurs locaux possèdent les autorisations suffisantes pour travailler avec le contenu et les paramètres. Pour octroyer des autorisations à d'autres utilisateurs, un administrateur local doit créer des attributions de rôles permettant d'accéder au serveur de rapports. Les pages et les tâches auxquelles un utilisateur pourra ensuite accéder dépendent des attributions de rôles qui ont été définies pour cet utilisateur. Pour plus d’informations, consultez [Accorder à un utilisateur l’accès à un serveur de rapports](./security/grant-user-access-to-a-report-server.md)

> [!NOTE]
> Si vous accédez au portail web sur l'ordinateur local sur lequel le serveur est en cours d’exécution, il est possible qu’un message indiquant que vous n'êtes pas autorisé à afficher ce dossier s’affiche. Cela est dû au contrôle d'accès universel (UAC) et au fait que vous n'exécutez pas le navigateur en tant qu'administrateur. Vous n'êtes pas en mesure d'exécuter Microsoft Edge en tant qu'administrateur. Vous devez utiliser Internet Explorer. Vous pouvez soit accéder au serveur à distance, soit lancer Internet Explorer en tant qu'administrateur et accéder au portail web. Si vous souhaitez utiliser le portail web à distance, vous devez accorder les droits de gestionnaire du contenu de votre compte sur le dossier.  

## <a name="start-and-use-the-web-portal"></a>Prise en main du portail web

Le portail web est une application web que vous ouvrez en tapant l’URL du [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] dans la barre d’adresses de la fenêtre du navigateur. Quand vous démarrez le [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], les pages, les liens et les options que vous voyez varient en fonction des autorisations dont vous disposez sur le serveur de rapports. Pour effectuer une tâche, vous devez être titulaire d'un rôle qui inclut la tâche.  Un utilisateur doté d'un rôle disposant d'autorisations complètes peut accéder à l'ensemble des menus et des pages de l'application qui sont disponibles pour gérer un serveur de rapports. Un utilisateur doté d'un rôle limité aux autorisations d'affichage et d'exécution des rapports ne peut voir que les menus et les pages concernées par ces activités. Chaque utilisateur peut disposer de rôles divers pour les différents serveurs de rapports qu'il utilise, voire pour les divers rapports et dossiers stockés sur un serveur de rapports unique.

Pour plus d'informations, consultez [Attribution d'autorisations sur un serveur de rapports en mode natif](../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md).

### <a name="start-the-web-portal"></a>Démarrer le portail web

Pour démarrer le portail web à partir d’un navigateur, effectuez les étapes suivantes :

1. Ouvrez votre navigateur web. Pour obtenir la liste des navigateurs web pris en charge, consultez la page [Planification de la prise en charge des navigateurs pour Reporting Services](../reporting-services/browser-support-for-reporting-services-and-power-view.md).

2. Dans la barre d’adresses du navigateur web, tapez l’URL du portail web.

    Par défaut, l’URL est *https://[NomOrdinateur]/reports*.

    Le serveur de rapports peut être configuré pour utiliser un port spécifique. Par exemple, *https://[NomOrdinateur]:80/reports* ou *https://[NomOrdinateur]:8080/reports*.

## <a name="grouping-by-categories"></a>Regroupement par catégories

Le portail web regroupe les éléments en différentes catégories. Les catégories disponibles sont les suivantes.

- Indicateurs de performance clés
- Rapports mobiles
- Rapports paginés
- Rapports Power BI Desktop
- Classeurs Excel
- Groupes de données
- Sources de données
- Ressources

Vous pouvez contrôler ce qui est affiché en sélectionnant **Affichage** dans le coin supérieur droit. Si vous sélectionnez Afficher les éléments masqués, ces éléments s’afficheront dans une couleur plus claire.

![Capture d’écran de la liste déroulante Affichage avec l’option Afficher les éléments masqués sélectionnée.](../reporting-services/media/ssrswebportal-view.png)

![Capture d’écran montrant l’option Rapports paginés indisponible.](../reporting-services/media/ssrswebportal-hidden.png)

### <a name="power-bi-desktop-reports-and-excel-workbooks"></a>Rapports Power BI Desktop et classeurs Excel

Vous pouvez charger, organiser et gérer des autorisations pour les rapports Power BI Desktop et les classeurs Excel. Ils sont regroupés au sein du portail web.

![Capture d’écran montrant la section des rapports Power BI Desktop et la section des classeurs Excel.](../reporting-services/media/ssrswebportal-view-pbi-and-excel.png)

Les fichiers sont stockés dans Reporting Services, de la même façon que les autres fichiers de ressources. Sélectionner un de ces éléments permet de le télécharger localement sur votre bureau. Vous pouvez enregistrer les modifications que vous avez apportées en les rechargeant sur le serveur de rapports.

## <a name="search-for-items"></a>Recherche d’éléments

Entrez un terme de recherche pour voir tout ce à quoi vous pouvez accéder. Les résultats sont classés ainsi : indicateurs de performance clés, rapports, jeux de données et autres éléments. Vous pouvez ensuite interagir avec les résultats et les ajouter à vos favoris.

![Capture d’écran montrant le portail SQL Server Reporting Services avec la zone de texte de recherche mise en évidence.](../reporting-services/media/ssrswebportal-search.png)

## <a name="web-portal-tasks"></a>Tâches du portail web

[Personnalisation du portail web](../reporting-services/branding-the-web-portal.md)

[Utilisation d’indicateurs de performance clés](../reporting-services/working-with-kpis-in-reporting-services.md)

[Utilisation de jeux de données partagés](../reporting-services/work-with-shared-datasets-web-portal.md)

## <a name="see-also"></a>Voir aussi

[Créer des rapports mobiles avec l’Éditeur de rapports mobiles SQL Server](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md)  
[Configurer une URL (Gestionnaire de configuration du serveur de rapports)](../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)  
[Outils de Reporting Services](../reporting-services/tools/reporting-services-tools.md)  
[Planification de la prise en charge des navigateurs par Reporting Services](../reporting-services/browser-support-for-reporting-services-and-power-view.md)  
[Fonctionnalités Reporting Services prises en charge par les éditions de SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)  

D’autres questions ? [Essayez le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)