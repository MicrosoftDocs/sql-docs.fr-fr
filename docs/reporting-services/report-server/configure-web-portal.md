---
title: Configurer le portail web | Microsoft Docs
description: Découvrez comment configurer l’application de portail web, utilisée pour afficher des rapports, gérer le contenu d’un serveur de rapports et accorder aux utilisateurs l’accès à un serveur de rapports en mode natif.
ms.date: 05/10/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- the web portal [Reporting Services], configuring
ms.assetid: e918986c-af15-48f6-8178-256aed829c6a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ebbce2708647e15b68ec497cfadd0af2c7c5b154
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074690"
---
# <a name="configure-the-web-portal"></a>Configurer le portail web

Le portail web est une application web frontale utilisée pour afficher des rapports, gérer le contenu d’un serveur de rapports et accorder aux utilisateurs l’accès à un serveur de rapports en mode natif. Le portail web est installé avec le service web Report Server dans la même instance du serveur de rapports et est configuré si vous sélectionnez l’option **Installer la configuration par défaut en mode Natif** dans le programme d’installation. Vous pouvez également configurer le portail web après son installation. Cette rubrique fournit des informations sur les scénarios de configuration suivants du portail web :

## <a name="prerequisites"></a>Prérequis

Pour utiliser le portail web, vous devez satisfaire aux prérequis suivants :

- Vous devez disposer d'un serveur de rapports doté d'une configuration minimale. Pour plus d’informations sur la configuration minimale d’un serveur de rapports, consultez [Configurer un serveur de rapports](../../reporting-services/report-server/configure-a-report-server-reporting-services-native-mode.md).

- Votre serveur de rapports doit s'exécuter en mode natif. Vous ne pouvez pas utiliser le portail web avec un serveur de rapports configuré pour le mode intégré SharePoint. Dans SQL Server 2012, vous ne pouvez pas basculer un serveur de rapports d'un mode à un autre. Si vous souhaitez modifier le type de serveur de rapports que votre environnement utilise, vous devez installer le mode souhaité du serveur de rapports, puis copier ou déplacer les éléments de rapport vers le nouveau serveur de rapports. Ce processus est habituellement désigné sous le terme de « migration ». Les étapes nécessaires pour la migration dépendent du mode vers lequel vous effectuez la migration et de la version d'origine de la migration. Pour plus d'informations, consultez [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).

- Internet Explorer 11 ou ultérieur avec activation des scripts doit également être installé. Pour plus d’informations, consultez [Prise en charge des navigateurs pour Reporting Services et Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).

## <a name="configure-the-web-portal-to-use-the-default-url"></a>Configurer le portail web pour utiliser l’URL par défaut

Le portail web est une application web à laquelle les utilisateurs accèdent via un navigateur web. Vous devez au minimum définir l'URL utilisée pour ouvrir l'application dans une fenêtre de navigateur. L'URL est formée d'un nom d'hôte, d'un port et d'un répertoire virtuel. Les valeurs par défaut de cette URL utilisent les valeurs de nom d'hôte et de port que vous avez définies pour l'URL du service Web Report Server, plus le nom du répertoire virtuel **rapports** . Si vous disposez d’une instance nommée, le répertoire virtuel est **reports_instance**, où **instance** est le nom de votre instance [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .

Par défaut, l’URL du portail web est formée d’un nom de répertoire virtuel unique, plus le port et le nom d’hôte définis pour le service web Report Server qui s’exécute dans la même instance. Dans la plupart des cas, le nom d'hôte est le nom de réseau du serveur de rapports, mais il peut également s'agir d'une adresse IP ou d'un en-tête d'hôte qui résout l'ordinateur. Pour configurer le portail web de sorte qu’il utilise l’URL par défaut, utilisez la page **URL du portail web** dans l’outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].

> [!TIP]
> Si vous essayez d’accéder au portail web sur un ordinateur distant et que vous obtenez des erreurs de connexion dans votre navigateur, les paramètres du pare-feu sont généralement en cause. Pour plus d’informations, consultez [Configurer un pare-feu pour accéder au serveur de rapports](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).

#### <a name="to-configure-the-default-the-web-portal-url-and-virtual-directory"></a>Pour configurer l’URL et le répertoire virtuel par défaut du portail web

1. Démarrez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis connectez-vous à l'instance du serveur de rapports.

2. Dans l’outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], sélectionnez **URL du portail web** pour ouvrir la page permettant de configurer l’URL.

3. Entrez un nom de répertoire virtuel unique pour le portail web.

4. Cliquez sur **Appliquer**.

5. Si vous utilisez [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)] ou Windows Server 2008, des étapes supplémentaires peuvent être requises pour pouvoir ouvrir le portail web. Pour plus d’informations, consultez [Configurer un serveur de rapports en mode natif pour l’administration locale &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).

## <a name="configure-the-web-portal-to-use-a-specific-report-server-url"></a>Configurer le portail web de sorte qu’il utilise une URL de serveur de rapports spécifique

Par défaut, le portail web se connecte au service web Report Server qui s’exécute dans le même service Report Server. Le portail web utilise l’URL du service web Report Server pour établir cette connexion. Si vous définissez plusieurs URL pour le service web Report Server, le portail web utilise la dernière que vous avez définie. Toutefois, pour certains déploiements, vous souhaiterez peut-être que le portail web se connecte toujours au service web via une URL statique. En effet, si vous avez configuré le filtrage des paquets sur un port ou une adresse IP spécifique et que vous souhaitez que toutes les connexions au serveur de rapports se soumettent aux règles de filtrage que vous avez définies, vous utiliserez une URL statique.

Lorsque vous configurez des URL dans l’outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], le portail web détecte et utilise automatiquement toute nouvelle URL ou URL mise à jour pour le serveur de rapports qui s’exécute dans la même instance du serveur. Si votre déploiement exige que vous utilisiez une URL statique unique pour toutes les demandes de serveur de rapports, vous pouvez spécifier cette URL dans le fichier RSReportServer.config.

#### <a name="to-configure-a-static-report-server-url"></a>Pour configurer une URL de serveur de rapports statique

1. Ouvrez le fichier **RSReportServer.config** dans un éditeur de texte. Par défaut, celui-ci se trouve dans le dossier \Program Files\Microsoft SQL Server\MSRS12.\<*instancename*>\Reporting Services\ReportServer.  

2. Recherchez **ReportServerURL**.

3. Remplacez-le par l'URL de l'instance du serveur de rapports.

4. Enregistrer vos modifications et fermez le fichier.

Pour plus d’informations sur le fichier de configuration, consultez [Modifier un fichier de configuration Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md) et [Fichier de configuration RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).

## <a name="customize-styles-or-application-title"></a>Personnaliser les styles ou le titre de l'application

Vous pouvez créer un package de marque personnalisé pour modifier les couleurs utilisées pour le portail web. Pour plus d’informations, consultez [Personnalisation du portail web](../branding-the-web-portal.md).

#### <a name="to-modify-application-title"></a>Pour modifier le titre de l'application

1. Connectez-vous à l'aide d'un compte auquel les autorisations **Administrateur système** sont assignées sur le serveur de rapports.

2. Ouvrez Internet Explorer.

3. Configurez l’URL du portail web. Par défaut, il s’agit de https://\<**your-server-name**>/reports, mais si vous avez installé Reporting Services sous la forme d’une instance nommée, l’URL par défaut sera https://\<**your-server-name**>/reports\<**_instancename**>.

4. Sélectionnez **Paramètres du site**.

5. Sous l'onglet **Général** , dans **Nom**, remplacez **SQL Server Reporting Services** par un nom différent.

6. Sélectionnez **Appliquer**.

## <a name="next-steps"></a>Étapes suivantes

[Portail Web](../../reporting-services/web-portal-ssrs-native-mode.md)  
[Prise en charge des navigateurs pour Reporting Services](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)
[Configurer une URL](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)   
[Vérifier une installation de Reporting Services](../../reporting-services/install-windows/verify-a-reporting-services-installation.md)   
[Activer ou désactiver les fonctionnalités Reporting Services](../../reporting-services/report-server/turn-reporting-services-features-on-or-off.md)   
[Gérer un serveur de rapports Reporting Services (SSRS) en mode natif](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
[RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Configurer un serveur de rapports en mode natif pour l’administration locale](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md)

 D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
