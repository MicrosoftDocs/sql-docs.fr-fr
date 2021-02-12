---
description: Configurer une URL (Gestionnaire de configuration du serveur de rapports)
title: Configurer une URL (Configuration Manager) | Microsoft Docs
ms.date: 05/26/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- URL access [Reporting Services], syntax
ms.assetid: 851e163a-ad2a-491e-bc1e-4df92327092f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bd4d8deaa9be114ffb2ce806f67be3b96babb8b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015279"
---
# <a name="configure-a-url--report-server-configuration-manager"></a>Configurer une URL (Gestionnaire de configuration du serveur de rapports)
  Avant de pouvoir utiliser le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] ou le service web Report Server, vous devez configurer au moins une URL pour chaque application. La configuration des URL est obligatoire si vous avez installé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode « fichiers uniquement » (autrement dit, en sélectionnant l’option **Installer mais ne pas configurer le serveur** dans la page Options d’installation du serveur de rapports dans l’Assistant Installation). Si vous avez installé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans la configuration par défaut, les URL sont déjà configurées pour chaque application.  
  
 Utilisez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour configurer les URL. Toutes les parties de l'URL sont définies dans cet outil. Contrairement aux versions précédentes, les sites Web IIS (Internet Information Services) ne fournissent plus l'accès aux applications [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit les valeurs par défaut qui fonctionnent bien dans la plupart des scénarios de déploiement, y compris les déploiements côte à côte avec les autres applications et services web. Les URL par défaut incorporent les noms d'instance, réduisant le risque de conflits d'URL si vous exécutez plusieurs instances de serveur de rapports sur le même ordinateur.  
  
 Cette rubrique fournit des instructions relatives aux tâches suivantes :  
  
-   Créer une URL pour le service Web Report Server.  
  
-   Créer une URL pour le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)].  
  
-   Définir les propriétés URL avancées pour définir des URL supplémentaires.  
  
 Pour plus d’informations sur le stockage et la gestion des URL, ou sur les problèmes d’interopérabilité, consultez [À propos des réservations et de l’inscription d’URL &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md) et [Installer côte à côte Reporting Services et Internet Information Services &#40;SSRS en mode natif&#41;](../../reporting-services/install-windows/install-reporting-and-internet-information-services-side-by-side.md). Pour obtenir des exemples d'URL souvent utilisées dans une installation Reporting Services, consultez [Exemples d'URL](#URLExamples) dans cette rubrique.  
  
## <a name="prerequisites"></a>Prérequis  
 Avant de créer ou de modifier une URL, souvenez-vous des points suivants :  
  
-   Vous devez être membre du groupe Administrateurs local sur le serveur de rapports.  
  
-   Si IIS est installé sur le même ordinateur, vérifiez les noms des répertoires virtuels sur les sites web qui utilisent le port 80. Si vous consultez les répertoires virtuels qui utilisent les noms de répertoire virtuel Reporting Services par défaut (autrement dit, Reports et ReportServer), choisissez des noms de répertoire virtuel différents pour les URL [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que vous configurez.  
  
-   Vous devez utiliser l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour configurer l'URL : N'utilisez pas d'utilitaire système. Ne modifiez jamais directement les réservations d'URL dans la section **URLReservations** du fichier RSReportServer.config. L'utilisation de l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est nécessaire pour mettre à jour la réservation d'URL sous-jacente stockée en interne et pour synchroniser les paramètres URL stockés dans le fichier RSReportServer.config.  
  
-   Choisissez une heure qui présente une activité de rapport basse. À chaque modification de la réservation d’URL, vous pouvez vous attendre à ce que les domaines d’application pour le service web Report Server et le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] soient recyclés.  
  
-   Pour obtenir une présentation de la création et de l’utilisation des URL dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Configurer des URL de serveurs de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md).  
  
### <a name="to-configure-a-url-for-the-report-server-web-service"></a>Pour configurer une URL pour le service Web Report Server  
  
1.  Démarrez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis connectez-vous à l'instance du serveur de rapports locale.  
  
2.  Cliquez sur **URL du service Web**.  
  
3.  Spécifiez le répertoire virtuel. Le nom de répertoire virtuel identifie l'application qui reçoit la demande. Comme une adresse IP et un port peuvent être partagés par plusieurs applications, le nom de répertoire virtuel spécifie quelle application reçoit la demande.  
  
     Cette valeur doit être unique pour garantir que la demande atteint sa destination prévue. Cette valeur est requise. Elle ne respecte pas la casse. Il existe une correspondance univoque entre un nom de répertoire virtuel et une instance d'une application [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Si vous créez plusieurs URL vers la même instance d'application, vous devez utiliser le même nom de répertoire virtuel dans toutes les URL que vous définissez pour cette instance d'application.  
  
     Pour le service Web Report Server, le nom du répertoire virtuel par défaut est **ReportServer**.  
  
4.  Spécifiez l'adresse IP qui identifie de façon unique le serveur de rapports sur le réseau. Si vous souhaitez spécifier un en-tête de l'hôte ou définir des URL supplémentaires pour la même instance d'application, vous devez cliquer sur **Avancé**. Pour obtenir des instructions sur la façon de définir les propriétés avancées sur l'URL, consultez les instructions plus loin dans cette rubrique. Sinon, utilisez la page **URL du service Web** pour effectuer une sélection parmi les valeurs suivantes :  
  
    -   **Assigné** spécifie que chacune des adresses IP assignées à l'ordinateur peut être utilisée dans une URL qui pointe sur une application du serveur de rapports. Cette valeur comprend également les noms d'hôte conviviaux (tels que les noms d'ordinateur) qui peuvent être résolus par un serveur de noms de domaine en une adresse IP assignée à l'ordinateur. Il s'agit de la valeur par défaut pour une URL [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
    -   **Non assigné** spécifie que le serveur de rapports reçoit une demande qui n'a pas été gérée par une autre application. Il est recommandé d'éviter cette option. Si vous sélectionnez cette option, il devient possible pour une autre application qui possède une réservation d'URL plus forte d'intercepter les demandes prévues pour le serveur de rapports.  
  
    -   **127.0.0.1** est l'adresse IPv4 utilisée pour l'accès à localhost. Cette valeur prend en charge l'administration locale sur le serveur de rapports. Si vous sélectionnez uniquement cette valeur, seuls les utilisateurs qui se connectent localement au serveur de rapports ont accès à l'application.  
  
    -   **::1** désigne l'adresse de bouclage au format IPv6.  
  
    -   Les adresses IP spécifiques apparaissent également dans cette liste. Les adresses IP peuvent être aux formats IPv4 et IPv6. *Nnn.nnn.nnn.nnn* est l’adresse IPv4 32 bits d’une carte réseau sur votre ordinateur. Les adresses IPv6 ont une taille de 128 bits et sont formées de huit champs de 4 octets séparés par le signe deux-points : \<prefix>:*nnnn:nnnn:nnnn:nnnn:nnnn:nnnn*  
  
         Si vous avez plusieurs cartes ou si votre réseau prend en charge les adresses IPv4 et les adresses IPv6, vous visualisez plusieurs adresses IP. Si vous sélectionnez une seule adresse IP, elle limitera l'accès de l'application à la seule adresse IP (et à tout nom d'hôte qu'un serveur de noms de domaine mappe sur cette adresse). Vous ne pouvez pas utiliser localhost pour accéder à un serveur de rapports, et vous ne pouvez pas utiliser les adresses IP des autres cartes réseau installées sur le serveur de rapports. En général, si vous sélectionnez cette valeur, c'est parce que vous configurez plusieurs réservations d'URL qui spécifient aussi des adresses IP explicites ou des noms d'hôte (par exemple, un premier pour une carte réseau utilisée pour les connexions intranet et un second utilisé pour les connexions extranet).  
  
5.  Spécifiez le port. Le port 80 est le port par défaut, car il peut être partagé avec d’autres applications. Si vous souhaitez utiliser un numéro de port personnalisé, souvenez-vous que vous devrez toujours le spécifier dans l'URL utilisée pour accéder au serveur de rapports. Vous pouvez utiliser les techniques suivantes pour rechercher un port disponible :  
  
    -   À partir d'une invite de commandes, tapez la commande suivante pour retourner la liste des ports TCP utilisés :  
  
         `netstat -anp tcp`  
  
    -   Lisez l’article du support Microsoft [Informations relatives aux affectations de ports TCP/IP](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/174904)pour comprendre les attributions de port TCP et les différences entre les ports bien identifiés (0 à 1023), les ports inscrits (1024 à 49151) et les ports dynamiques ou privés (49152 à 65535).  
  
    -   Si vous utilisez le Pare-feu Windows, vous devez ouvrir le port. Pour obtenir des instructions, consultez [Configure a Firewall for Report Server Access](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
6.  Si vous ne l'avez pas déjà fait, vérifiez qu'IIS (s'il est installé) n'a pas de répertoire virtuel ayant le même nom que celui que vous projetez d'utiliser.  
  
7.  Si vous avez installé un certificat TLS/SSL, vous pouvez le sélectionner pour lier l'URL au certificat TLS/SSL installé sur votre ordinateur.  
  
8.  Le cas échéant, si vous sélectionnez un certificat TLS/SSL, vous pouvez spécifier un port personnalisé. La valeur par défaut est 443, mais vous pouvez utiliser tout port disponible.  
  
9. Cliquez sur **Appliquer** pour créer l'URL.  
  
10. Testez l'URL en cliquant sur le lien dans la section **URL** de la page. Notez que la base de données du serveur de rapports doit être créée et configurée avant que vous puissiez tester l'URL. Pour obtenir des instructions, consultez [Créer une base de données du serveur de rapports en mode natif &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  

> [!NOTE]
>  Si vous disposez de liaisons TLS et de réservations d'URL existantes et souhaitez modifier la liaison TLS, vous pouvez utiliser un certificat ou un en-tête d'hôte différent. Il est recommandé de réaliser les étapes suivantes dans l'ordre indiqué :  
> 
>  1.  Commencez par supprimer toutes les réservations d'URL.  
> 2.  Puis, supprimez toutes les liaisons TLS.  
> 3.  puis de recréer les URL et les liaisons TLS.  
> 
>  Les étapes précédentes peuvent être effectuées à partir du Gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
> 
>  Microsoft Windows prend en charge une liaison pour chaque combinaison d'adresse IP/port. Si vous configurez un serveur de rapports de façon à utiliser une valeur d'en-tête d'hôte spécifique et le certificat sur la combinaison port/adresse IP est également émis à une valeur d'en-tête d'hôte différente, un avertissement indiquant que le certificat ne correspond pas à l'URL utilisée s'affiche dans le navigateur.  
> 
>  Pour corriger le problème, supprimez toutes les liaisons, puis créez des liaisons possédant des paramètres uniques, ou configurez l'inscription d'URL Reporting Services à l'aide de caractères génériques.
  
### <a name="to-create-a-url-reservation-for-the-ssrswebportal"></a>Pour créer une réservation d’URL pour le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]  
  
1.  Démarrez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis connectez-vous à l'instance du serveur de rapports.  
  
2.  Cliquez sur **URL du portail web**.  
  
3.  Spécifiez le répertoire virtuel. Le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] écoute sur la même adresse IP et le même port que le service web Report Server. Si vous avez configuré le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] pour pointer sur un service web Report Server différent, vous devez modifier les paramètres d’URL du [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] dans le fichier RSReportServer.config.  
  
4.  Si vous avez installé un certificat TLS/SSL, vous pouvez le sélectionner pour exiger que toutes les demandes adressées au [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] soient acheminées via le protocole HTTPS.  
  
     Le cas échéant, si vous sélectionnez un certificat TLS/SSL, vous pouvez spécifier un port personnalisé. La valeur par défaut est 443, mais vous pouvez utiliser tout port disponible.  
  
5.  Cliquez sur **Appliquer** pour créer l'URL.  
  
6.  Testez l'URL en cliquant sur le lien dans la section **URL** de la page.  
  
## <a name="setting-advanced-properties-to-specify-additional-urls"></a>Définition de propriétés avancées pour spécifier les URL supplémentaires  
 Vous pouvez réserver plusieurs URL pour le service web Report Server ou le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] en spécifiant des ports ou des noms d’hôte différents (une adresse IP ou un nom d’en-tête de l’hôte qu’un serveur de noms de domaine peut résoudre en une adresse IP affectée à l’ordinateur). En créant plusieurs URL, vous pouvez définir différents chemins d'accès à la même instance de serveur de rapports. Par exemple, pour activer l'accès intranet et extranet à un serveur de rapports, vous pouvez utiliser l'URL par défaut pour l'accès intranet et un nom d'hôte complet supplémentaire pour l'accès extranet :  
  
-   `https://myserver01/reportserver`  
  
-   `https://www.adventure-works.com/reportserver`  
  
 Vous ne pouvez pas définir plusieurs noms de répertoire virtuel pour la même instance d'application. Chaque instance d'application de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est mappée sur un nom de répertoire virtuel unique. Si vous avez plusieurs instances de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur le même ordinateur, le nom de répertoire virtuel d'une application doit inclure le nom de l'instance pour garantir que chaque demande atteint sa cible prévue.  
 
  **En-tête de l'hôte**  
 Si vous avez déjà un en-tête de l'hôte défini sur un serveur de noms de domaine qui se résout sur votre ordinateur, vous pouvez spécifier cet en-tête de l'hôte dans une URL que vous configurez pour l'accès au serveur de rapports.  
  
 Un en-tête de l'hôte est un nom unique qui permet à plusieurs sites Web de partager une même adresse IP et un même port. Les noms d'en-tête de l'hôte sont plus faciles à mémoriser et à taper que les adresses IP et les numéros de port. Un exemple de nom d'en-tête de l'hôte peut être www.adventure-works.com.  
  
 **Port SSL**  
 Spécifie le port pour les connexions TLS/SSL. Le numéro de port TLS par défaut est 443.  
  
 **Certificat SSL**  
 Spécifie le nom de certificat d'un certificat TLS/SSL que vous avez installé sur cet ordinateur. Si le certificat est mappé sur un caractère générique, vous pouvez l'utiliser pour une connexion de serveur de rapports.  
  
 Spécifie le nom complet de l'ordinateur pour lequel le certificat est enregistré. Le nom que vous spécifiez doit être identique au nom pour lequel le certificat est enregistré.  
  
 Un certificat doit être installé pour utiliser cette option. Vous devez également modifier le paramètre de configuration UrlRoot dans le fichier RSReportServer.config, afin qu'il spécifie le nom complet de l'ordinateur pour lequel le certificat est enregistré. Pour plus d’informations, consultez [Configurer des connexions TLS sur un serveur de rapports en mode natif](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
### <a name="to-set-advanced-properties-on-a-url"></a>Pour définir les propriétés avancées sur une URL  
  
1.  Dans la page **URL du service web** ou **URL du Gestionnaire de rapports** , cliquez sur **Avancé**.  
  
2.  Cliquez sur **Add**.  
  
3.  Cliquez sur l'adresse IP ou le nom d'en-tête de l'hôte. Si vous spécifiez un en-tête de l'hôte, veillez bien à spécifier un nom que le service DNS peut résoudre. Si vous spécifiez un nom de domaine disponible publiquement, indiquez la totalité de l’URL, y compris `https://www`.  
  
4.  Spécifiez le port. Si vous spécifiez un port personnalisé, l'URL de l'application doit toujours inclure le numéro de port.  
  
5.  Cliquez sur **OK**.  
  
6.  Testez l'URL en ouvrant une fenêtre de navigateur et en entrant l'URL.  
  
## <a name="urls-for-multiple-report-server-instances-on-the-same-computer"></a>URL de plusieurs instances de Report Server sur le même ordinateur  
 Si vous réservez les URL pour plusieurs instances de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vous devez suivre les conventions d'affectation de noms afin d'éviter les conflits de noms. Pour plus d’informations, consultez [Réservations d’URL pour les déploiements de serveur de rapports multi-instance &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/url-reservations-for-multi-instance-report-server-deployments.md).  
  
##  <a name="examples-of-url-configurations"></a><a name="URLExamples"></a> Exemples de configurations d'URL  
 La liste suivante présente quelques exemples d'URL de serveur de rapports :  
  
-   `https://localhost/reportserver`  
  
-   `https://localhost/reportserver_SQLEXPRESS`  
  
-   `https://sales01/reportserver`  
  
-   `https://sales01:8080/reportserver`  
  
-   `https://sales.adventure-works.com/reportserver`  
  
-   `https://www.adventure-works.com:8080/reportserver01`  
  
 Les URL que vous utilisez pour accéder au [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] partagent le même format et sont généralement créées sous le site web qui héberge le serveur de rapports. La seule différence est le nom du répertoire virtuel (dans ce cas, le nom est **rapports** , mais vous pouvez configurer le répertoire virtuel pour qu’il utilise le nom de votre choix) :  
  
-   `https://localhost/reports`  
  
-   `https://localhost/reports_SQLEXPRESS`  
  
-   `https://sales01/reports`  
  
-   `https://sales01:8080/reports`  
  
-   `https://sales.adventure-works.com/reports`  
  
-   `https://www.adventure-works.com:8080/reports`  
  
## <a name="see-also"></a>Voir aussi

 [Configurer des URL de serveurs de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md) [Gestionnaire de configuration du serveur de rapports &#40;Mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
