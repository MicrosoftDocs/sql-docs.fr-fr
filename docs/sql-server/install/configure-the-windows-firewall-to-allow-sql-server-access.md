---
title: Configurer le pare-feu Windows
description: Découvrez comment configurer le pare-feu Windows pour permettre l’accès à une instance de SQL Server via le pare-feu.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- Windows Firewall ports
- WMI firewall ports
- Windows Firewall [Database Engine]
- firewall systems, configuring
- advfirewall
- firewall systems
- rules firewall
- firewall systems, overview and port list
- 1433 TCP port
- portopening using netsh
- ports [SQL Server], TCP
- netsh to open firewall ports
ms.assetid: f55c6a0e-b6bd-4803-b51a-f3a419803024
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7a4e10b437f60c8f6b1f80ed76017591fc22686d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597259"
---
# <a name="configure-the-windows-firewall-to-allow-sql-server-access"></a>Configure the Windows Firewall to Allow SQL Server Access
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Les systèmes de pare-feu empêchent les accès non autorisés aux ressources de l'ordinateur. Si un pare-feu est activé alors qu'il n'est pas configuré correctement, les tentatives de connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être bloquées.  
  
Pour accéder à une instance du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par le biais d'un pare-feu, vous devez configurer le pare-feu sur l'ordinateur exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le pare-feu est un composant de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Vous pouvez également installer un pare-feu d'une autre société. Cet article explique comment configurer le Pare-feu Windows, mais les principes de base s’appliquent à d’autres programmes de pare-feu.  
  
> [!NOTE]  
>  Cet article fournit une vue d’ensemble de la configuration du pare-feu et résume les informations présentant un intérêt pour un administrateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur le pare-feu et pour obtenir des informations rigoureuses sur le pare-feu, consultez la documentation de pare-feu, par exemple le [Guide de déploiement fu Pare-feu Windows avec fonctions avancées de sécurité](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-deployment-guide).  
  
 Les utilisateurs ayant une bonne connaissance de la gestion du **Pare-feu Windows** et sachant quels paramètres de pare-feu ils veulent configurer peuvent passer directement aux articles plus avancés :  
  
-   [Configurer un pare-feu Windows pour accéder au moteur de base de données](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)    
-   [Configurer le Pare-feu Windows pour autoriser l’accès à Analysis Services](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)    
-   [Configurer un pare-feu pour accéder au serveur de rapports](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)  
  
##  <a name="basic-firewall-information"></a><a name="BKMK_basic"></a> Informations de base sur le pare-feu  
 Les pare-feu fonctionnent en examinant les paquets entrants, et en les comparant à un jeu de règles. Si le paquet respecte les normes dictées par les règles, le pare-feu passe le paquet au protocole TCP/IP pour traitement supplémentaire. Si le paquet ne respecte pas les normes spécifiées par les règles, le pare-feu ignore le paquet et, si la journalisation est activée, crée une entrée dans le fichier journal du pare-feu.  
  
 La liste du trafic autorisé est remplie de l'une des façons suivantes :  

- **Automatiquement** : quand un ordinateur dont le pare-feu est activé a lancé la communication, le pare-feu crée une entrée dans la liste afin que la réponse soit autorisée. Cette réponse est considérée comme du trafic sollicité, et rien ne doit être configuré. 
  
-   **Manuellement** : Un administrateur configure les exceptions du pare-feu. Cela autorise l’accès à des programmes ou des ports spécifiés sur votre ordinateur. Dans ce cas, l'ordinateur accepte le trafic entrant non sollicité lorsqu'il joue le rôle de serveur, d'écouteur ou d'homologue. C'est le type de configuration qui doit être complétée pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Le choix d'une stratégie de pare-feu est plus complexe que le fait de déterminer si un port donné doit être ouvert ou fermé. Lors de la conception d'une stratégie de pare-feu pour votre entreprise, veillez à prendre en considération toutes les règles et les options de configuration disponibles. Cet article n’examine pas toutes les options de pare-feu possibles. Nous vous recommandons de consulter les documents suivants :  
  
 [Guide de déploiement du Pare-feu Windows](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-deployment-guide)    
 [Guide de conception du Pare-feu Windows](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-design-guide)    
 [Introduction à l'isolation de serveur et de domaine](/windows/security/threat-protection/windows-firewall/domain-isolation-policy-design)  
  
##  <a name="default-firewall-settings"></a><a name="BKMK_default"></a> Paramètres du pare-feu par défaut  
 Pour planifier votre configuration de pare-feu, la première étape est de déterminer l'état en cours du pare-feu de votre système d'exploitation. Si le système d'exploitation a été mis à niveau à partir d'une version précédente, les anciens paramètres du pare-feu ont pu être conservés. De même, les paramètres du pare-feu ont peut-être été modifiés par un autre administrateur ou par une Stratégie de groupe dans votre domaine.  
  
> [!NOTE]  
>  L'activation du pare-feu affectera d'autres programmes qui accèdent à cet ordinateur, comme le partage de fichiers et d'imprimantes, ainsi que les connexions Bureau à distance. Les administrateurs doivent tenir compte de toutes les applications qui s'exécutent sur l'ordinateur avant de définir les paramètres du pare-feu.  
  
##  <a name="programs-to-configure-the-firewall"></a><a name="BKMK_programs"></a> Programmes pour configurer le pare-feu  
Configurez les paramètres du Pare-feu Windows avec **Microsoft Management Console** ou **netsh**.  

-  **Microsoft Management Console (MMC)**  
  
     Le composant logiciel enfichable MMC du Pare-feu Windows avec fonctions avancées de sécurité vous permet de configurer des paramètres du pare-feu plus avancés. Ce composant logiciel enfichable présente la plupart des options de pare-feu de façon simple et expose tous les profils de pare-feu. Pour plus d’informations, consultez [Utilisation du composant logiciel Pare-feu Windows avec fonctions avancées de sécurité](#BKMK_WF_msc), plus loin dans cet article.  
  
-   **netsh**  
  
     L’outil **netsh.exe** peut être utilisé par un administrateur pour configurer et surveiller des ordinateurs Windows à partir d’une invite de commandes ou à l’aide d’un fichier de commandes **.** En utilisant l’outil **netsh** , vous pouvez diriger les commandes de contexte que vous entrez dans l’application d’assistance appropriée, et celle-ci exécute ensuite la commande. Une application d’assistance est un fichier de bibliothèque de liens dynamiques (.dll, Dynamic Link Library) qui étend les fonctionnalités de l’outil **netsh** en fournissant la configuration, l’analyse et la prise en charge d’un ou plusieurs services, utilitaires ou protocoles. Tous les systèmes d'exploitation qui prennent en charge [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ont un programme d'assistance de pare-feu. [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] possède également une application d’assistance de pare-feu avancée nommée **advfirewall**. Aucune information détaillée sur l’utilisation de **netsh** n’est fournie dans cet article. Toutefois, un grand nombre des options de configuration décrites peuvent être configurées via **netsh**. Exécutez, par exemple, le script suivant à partir d'une invite de commandes pour ouvrir le port TCP 1433 :  
  
    ```  
    netsh firewall set portopening protocol = TCP port = 1433 name = SQLPort mode = ENABLE scope = SUBNET profile = CURRENT  
    ```  
  
     Voici un exemple semblable qui utilise l'application auxiliaire Pare-feu Windows avec fonctions avancées de sécurité :  
  
    ```  
    netsh advfirewall firewall add rule name = SQLPort dir = in protocol = tcp action = allow localport = 1433 remoteip = localsubnet profile = DOMAIN  
    ```  
  
     Pour plus d’informations sur **netsh**, consultez les liens suivants :  
  
    -   [Syntaxe, contextes et mise en forme des commandes Netsh](/windows-server/networking/technologies/netsh/netsh-contexts)    
    -   [Comment utiliser le contexte « pare-feu netsh advfirewall » à la place du contexte « pare-feu netsh » pour contrôler le comportement du Pare-feu Windows dans Windows Server 2008 et Windows Vista](https://support.microsoft.com/kb/947709)    

    
- **Pour Linux** : Sur Linux, vous devez également ouvrir les ports associés aux services auxquels vous devez accéder. Différentes distributions de Linux et différents pare-feux ont leurs propres procédures. Pour avoir deux exemples, consultez [SQL Server on Red Hat](../../linux/quickstart-install-connect-red-hat.md) et [SQL Server on SUSE](../../linux/quickstart-install-connect-suse.md). 
  
## <a name="ports-used-by-ssnoversion"></a>Ports utilisés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Les tableaux suivants peuvent vous aider à identifier les ports utilisés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="ports-used-by-the-database-engine"></a><a name="BKMK_ssde"></a> Ports Used By the Database Engine  
 

Par défaut, les ports standard utilisés par SQL Server et les services de moteur de base de données associés sont : TCP **1433**, **4022**, **135**, **1434**, UDP **1434**. Le tableau ci-dessous explique ces ports de manière plus détaillée. Une instance nommée utilise des [ports dynamiques](#BKMK_dynamic_ports).
 
 Le tableau suivant répertorie les ports qui sont fréquemment utilisés par le [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
|Scénario|Port|Commentaires|  
|--------------|----------|--------------|  
|Instance par défaut qui s’exécute par le biais de TCP|Port TCP 1433|C'est le port le plus commun autorisé via le pare-feu. Il s'applique aux connexions de routine de l'installation par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)], ou une instance nommée qui est la seule instance qui s'exécute sur l'ordinateur. (Les instances nommées ont des considérations spéciales. Consultez [Ports dynamiques](#BKMK_dynamic_ports) plus loin dans cet article.)|  
|Instances nommées avec port par défaut|Le port TCP est un port dynamique déterminé au moment des démarrages du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|Consultez l'exposé ci-dessous dans la section [Ports dynamiques](#BKMK_dynamic_ports). Le port UDP 1434 peut être requis pour le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser lorsque vous utilisez des instances nommées.|  
| Instances nommées avec port fixe |Le numéro de port configuré par l'administrateur.|Consultez l'exposé ci-dessous dans la section [Ports dynamiques](#BKMK_dynamic_ports).|  
|Connexion administrateur dédiée|Port TCP 1434 pour l'instance par défaut. D'autres ports sont utilisés pour les instances nommées. Recherchez le numéro de port dans le journal des erreurs.|Par défaut, les connexions distantes à la connexion administrateur dédiée ne sont pas activées. Pour activer une DAC distante, utilisez la facette Configuration de la surface d'exposition. Pour plus d'informations, consultez [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser|Port UDP 1434|Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est à l'écoute des connexions entrantes vers une instance nommée et fournit au client le numéro de port TCP qui correspond à cette instance nommée. Normalement le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est démarré toutes les fois que les instances nommées du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sont utilisées. Il n'est pas nécessaire que le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soit démarré si le client est configuré pour se connecter au port spécifique de l'instance nommée.|  
|Instance avec point de terminaison HTTP.|Peut être spécifié lors de la création d'un point de terminaison HTTP. La valeur par défaut est le port TCP 80 pour le trafic CLEAR_PORT et le port 443 pour le trafic SSL_PORT.|Utilisé pour une connexion HTTP via une URL.|  
|Instance par défaut avec point de terminaison HTTPS |Port TCP 443|Utilisé pour une connexion HTTPS via une URL. HTTPS est une connexion HTTP qui utilise le protocole TLS (Transport Layer Security), précédemment appelé SSL (Secure Sockets Layer).|  
|[!INCLUDE[ssSB](../../includes/sssb-md.md)]|Port TCP 4022. Pour vérifier le port utilisé, exécutez la requête suivante :<br /><br /> `SELECT name, protocol_desc, port, state_desc`<br /><br /> `FROM sys.tcp_endpoints`<br /><br /> `WHERE type_desc = 'SERVICE_BROKER'`|Il n’existe aucun port par défaut pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssSB](../../includes/sssb-md.md)], mais il s’agit de la configuration classique utilisée dans les exemples de la documentation en ligne.|  
|Mise en miroir de bases de données|Port choisi par l'administrateur. Pour déterminer le port, exécutez la requête suivante :<br /><br /> `SELECT name, protocol_desc, port, state_desc FROM sys.tcp_endpoints`<br /><br /> `WHERE type_desc = 'DATABASE_MIRRORING'`|Il n’y a aucun port par défaut pour la mise en miroir de bases de données ; cependant, les exemples de la documentation en ligne utilisent le port TCP 5022 ou 7022. Il est important d’éviter d’interrompre un point de terminaison de mise en miroir en cours d’utilisation, en particulier en mode haute sécurité avec basculement automatique. Votre configuration du pare-feu doit éviter de rompre le quorum. Pour plus d’informations, consultez [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).|  
|Réplication|Les connexions de réplication à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent les ports standard type du [!INCLUDE[ssDE](../../includes/ssde-md.md)] (port TCP 1433 pour l'instance par défaut, etc.)<br /><br /> La synchronisation Web et l'accès de FTP/UNC pour l'instantané de réplication requièrent l'ouverture de ports supplémentaires sur le pare-feu. Pour transférer des données initiales et le schéma d'un emplacement à un autre, la réplication peut utiliser FTP (port TCP 21), ou la synchronisation via HTTP (port TCP 80) ou le partage de fichiers. Le partage de fichiers utilise les ports UDP 137 et 138, et le port TCP 139 avec NetBIOS. Le partage de fichiers utilise le port TCP 445.|Pour la synchronisation via HTTP, la réplication utilise le point de terminaison IIS (dont les ports sont configurables, mais qui a par défaut le port 80), mais le processus IIS se connecte au système principal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] via les ports standard (1433 pour l'instance par défaut).<br /><br /> Pendant la synchronisation Web via FTP, le transfert FTP s'effectue entre IIS et le serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , pas entre l'abonné et IIS.|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] Débogueur|Port TCP 135<br /><br /> Consultez [Considérations spéciales relatives au port 135](#BKMK_port_135)<br /><br /> L'exception [IPsec](#BKMK_IPsec) peut également être requise.|Si vous utilisez [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], sur l'ordinateur hôte [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] , vous devez également ajouter **Devenv.exe** à la liste Exceptions et ouvrir le port TCP 135.<br /><br /> Si vous utilisez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], sur l'ordinateur hôte [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , vous devez également ajouter **ssms.exe** à la liste Exceptions et ouvrir le port TCP 135. Pour plus d’informations, consultez [Configurer des règles de pare-feu avant d’exécuter le débogueur TSQL](../../ssms/scripting/configure-firewall-rules-before-running-the-tsql-debugger.md).|  
  
 Pour obtenir des instructions détaillées sur la manière de configurer le Pare-feu Windows pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)], consultez [Configurer un pare-feu Windows pour accéder au moteur de base de données](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md).  
  
####  <a name="dynamic-ports"></a><a name="BKMK_dynamic_ports"></a> Ports dynamiques  
 Par défaut, les instances nommées (y compris [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]) utilisent des ports dynamiques. Cela signifie que chaque fois que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] démarre, il identifie un port disponible et utilise ce numéro de port. Si l'instance nommée est la seule instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] installée, elle utilisera probablement le port TCP 1433. Si d'autres instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sont installées, le port utilisé sera probablement un port TCP différent. Dans la mesure où le port sélectionné peut changer chaque fois que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est démarré, il est difficile de configurer le pare-feu pour qu'il active l'accès au numéro de port correct. Par conséquent, si un pare-feu est utilisé, nous vous recommandons de reconfigurer le [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour qu'il utilise le même numéro de port à chaque fois. Cela s'appelle port fixe ou port statique. Pour plus d’informations, consultez [Configurer un serveur pour écouter un port TCP spécifique &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md).  
  
 L’alternative à la configuration d’une instance nommée pour l’écoute sur un port fixe est de créer une exception dans le pare-feu pour un programme [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tel que **sqlservr.exe** (pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)]). Cette approche peut être pratique, mais le numéro de port n’apparaît pas dans la colonne **Port local** de la page **Règles de trafic entrant** quand vous utilisez le composant logiciel enfichable MMC du Pare-feu Windows avec fonctions avancées de sécurité. Cela peut rendre plus difficile le fait d'auditer quels ports sont ouverts. Un autre élément à prendre en compte est qu’un Service Pack ou une mise à jour cumulative peut modifier le chemin vers le fichier exécutable [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ce qui risque d’invalider la règle de pare-feu.  
  
##### <a name="to-add-a-program-exception-to-the-firewall-using-windows-defender-firewall-with-advanced-security"></a>Pour ajouter une exception de programme au pare-feu à l’aide du pare-feu Windows Defender avec fonctions de sécurité avancée
  
1. Dans le menu Démarrer, tapez *wf.msc*. Appuyez sur Entrée ou sélectionnez le résultat de la recherche wf.msc pour ouvrir le **Pare-feu Windows Defender avec sécurité avancée**.
1. Dans le volet gauche, sélectionnez **Règles de trafic entrant**.
1. Dans le volet droit, sous **Actions**, sélectionnez **Nouvelle règle...** L’**Assistant Nouvelle règle de trafic entrant** s’ouvre.
1. Dans **Type de règle**, sélectionnez **Programme**. Sélectionnez **Suivant**.
1. Dans **Programme**, sélectionnez **Ce chemin d’accès de programme**. Sélectionnez **Parcourir** pour localiser votre instance de SQL Server. Le programme se nomme sqlservr.exe. Il se trouve normalement à l’emplacement suivant :

   `C:\Program Files\Microsoft SQL Server\MSSQL15.<InstanceName>\MSSQL\Binn\sqlservr.exe`

   Sélectionnez **Suivant**.

1. Dans **Action**, sélectionnez **Autoriser la connexion**. Sélectionnez **Suivant**.
1. Dans **Profil**, incluez les trois profils. Sélectionnez **Suivant**.
1. Dans **Nom**, tapez un nom pour la règle. Sélectionnez **Terminer**.

Pour plus d’informations sur les points de terminaison, consultez [Configurer le moteur de base de données de manière à écouter sur plusieurs ports TCP](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md) et [Affichages catalogue de points de terminaison &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md). 
  
###  <a name="ports-used-by-analysis-services"></a><a name="BKMK_ssas"></a> Ports utilisés par Analysis Services  
 
Par défaut, les ports standard utilisés par SQL Server Analysis Services et les services associés sont : TCP **2382**, **2383**, **80**, **443**. Le tableau ci-dessous explique ces ports de manière plus détaillée.  
 
 Le tableau suivant répertorie les ports qui sont fréquemment utilisés par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
|Fonctionnalité|Port|Commentaires|  
|-------------|----------|--------------|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|Port TCP 238 pour l'instance par défaut|Port standard pour l'instance par défaut de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser|Port TCP 2382 uniquement exigé pour une instance nommée [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|Les demandes de connexion des clients à une instance nommée de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui ne définissent pas un numéro de port sont dirigées vers le port 2382, qui est le port écouté par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser redirige ensuite la demande vers le port utilisé par l'instance nommée.|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] configuré pour une utilisation via IIS/HTTP<br /><br /> (Le service PivotTable® utilise HTTP ou HTTPS)|Port TCP 80|Utilisé pour une connexion HTTP via une URL.|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] configuré pour une utilisation via IIS/HTTPS<br /><br /> (Le service PivotTable® utilise HTTP ou HTTPS)|Port TCP 443|Utilisé pour une connexion HTTPS via une URL. HTTPS est une connexion HTTP qui utilise TLS.|  
  
 Si des utilisateurs accèdent à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] via IIS et Internet, vous devez ouvrir le port sur lequel IIS écoute et spécifier ce port dans la chaîne de connexion cliente. Dans ce cas, aucun port ne doit être ouvert pour l'accès direct à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Le port par défaut 2389 et le port 2382, ainsi que tous les ports qui ne sont pas nécessaires, doivent être soumis à des restrictions.  
  
 Pour obtenir des instructions détaillées sur la manière de configurer le Pare-feu Windows pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consultez [Configurer le pare-feu Windows pour autoriser l’accès à Analysis Services](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access).  
  
###  <a name="ports-used-by-reporting-services"></a><a name="BKMK_ssrs"></a> Ports utilisés par Reporting Services  

Par défaut, les ports standard utilisés par SQL Server Reporting Services et les services associés sont : TCP **80**, **443**. Le tableau ci-dessous explique ces ports de manière plus détaillée. 


Le tableau suivant répertorie les ports qui sont fréquemment utilisés par [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
|Fonctionnalité|Port|Commentaires|  
|-------------|----------|--------------|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Services Web|Port TCP 80|Utilisé pour une connexion HTTP à [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] via une URL. Nous vous recommandons de ne pas utiliser la règle préconfigurée **Services World Wide Web (HTTP)** . Pour plus d'informations, consultez ci-dessous la section [Interaction avec d'autres règles de pare-feu](#BKMK_other_rules) .|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] configuré pour une utilisation via HTTPS|Port TCP 443|Utilisé pour une connexion HTTPS via une URL. HTTPS est une connexion HTTP qui utilise TLS. Nous vous recommandons de ne pas utiliser la règle préconfigurée **Services World Wide Web sécurisés (HTTPS)** . Pour plus d'informations, consultez ci-dessous la section [Interaction avec d'autres règles de pare-feu](#BKMK_other_rules) .|  
  
Lorsque [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se connecte à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ou [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vous devez également ouvrir les ports appropriés pour ces services. Pour obtenir des instructions détaillées sur la manière de configurer le Pare-feu Windows pour [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Configurer un pare-feu pour accéder au serveur de rapports](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
###  <a name="ports-used-by-integration-services"></a><a name="BKMK_ssis"></a> Ports utilisés par Integration Services  
 Le tableau suivant répertorie les ports qui sont utilisés par le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
|Fonctionnalité|Port|Commentaires|  
|-------------|----------|--------------|  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Appels de procédure distante (MS RPC)<br /><br /> Utilisé par le runtime [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|Port TCP 135<br /><br /> Consultez [Considérations spéciales relatives au port 135](#BKMK_port_135)|Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise DCOM sur le port 135. Le Gestionnaire de contrôle des services utilise le port 135 pour effectuer des tâches telles que le démarrage et l'arrêt du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ainsi que la transmission des demandes de contrôle au service en exécution. Le numéro de port ne peut pas être modifié.<br /><br /> Ce port ne doit être ouvert que si vous vous connectez à une instance distante du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à partir de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou d'une application personnalisée.|  
  
Pour obtenir des instructions pas à pas pour configurer le Pare-feu Windows pour [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], consultez [Service Integration Services &#40;service SSIS&#41;](/previous-versions/sql/sql-server-2012/ms137861(v=sql.110)).  
  
###  <a name="additional-ports-and-services"></a><a name="BKMK_additional_ports"></a> Ports et services supplémentaires  
Le tableau suivant répertorie les ports et services dont [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut dépendre.  
  
|Scénario|Port|Commentaires|  
|--------------|----------|--------------|  
|Windows Management Instrumentation<br /><br /> Pour plus d'informations sur WMI, consultez [WMI Provider for Configuration Management Concepts](../../relational-databases/wmi-provider-configuration/wmi-provider-for-configuration-management.md).|WMI s'exécute dans le cadre d'un hôte de service partagé avec les ports attribués via DCOM. WMI peut utiliser le port TCP 135.<br /><br /> Consultez [Considérations spéciales relatives au port 135](#BKMK_port_135)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise WMI pour lister et gérer des services. Nous vous recommandons d’utiliser la règle préconfigurée **Windows Management Instrumentation (WMI)** . Pour plus d'informations, consultez ci-dessous la section [Interaction avec d'autres règles de pare-feu](#BKMK_other_rules) .|  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC)|Port TCP 135<br /><br /> Consultez [Considérations spéciales relatives au port 135](#BKMK_port_135)|Si votre application utilise des transactions distribuées, vous devez éventuellement configurer le pare-feu pour autoriser le trafic MS DTC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator) entre des instances MS DTC distinctes, et entre MS DTC et les gestionnaires de ressources tels que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nous vous recommandons d'utiliser le groupe de règles préconfigurées **Distributed Transaction Coordinator** .<br /><br /> Quand un MS DTC partagé unique est configuré pour l’intégralité du cluster dans un groupe de ressources distinct, vous devez ajouter sqlservr.exe comme exception au pare-feu.|  
|Le bouton Parcourir dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] utilise UDP pour se connecter au service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser. Pour plus d’informations, consultez [Service SQL Server Browser &#40;moteur de base de données et SSAS&#41;](../../database-engine/configure-windows/sql-server-browser-service-database-engine-and-ssas.md).|Port UDP 1434|UDP est un protocole sans connexion.<br /><br /> Le pare-feu a un paramètre ([Propriété UnicastResponsesToMulticastBroadcastDisabled de l’interface INetFwProfile](/windows/win32/api/netfw/nf-netfw-inetfwprofile-get_unicastresponsestomulticastbroadcastdisabled)) qui contrôle le comportement du pare-feu par rapport aux réponses de monodiffusion (unicast) à une demande UDP multidiffusion (ou multicast).  Il a deux comportements :<br /><br /> Si le paramètre est TRUE, aucune réponse de monodiffusion à une diffusion n'est autorisée. L'énumération des services échouera.<br /><br /> Si le paramètre est FALSE (valeur par défaut), les réponses de monodiffusion sont autorisées pendant 3 secondes. La durée n'est pas configurable. Dans un réseau encombré ou à latence élevée, ou pour les serveurs très chargés, toute tentative d’énumération des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut retourner une liste partielle, qui peut induire les utilisateurs en erreur.|  
|<a name="BKMK_IPsec"></a> Trafic IPsec|Port UDP 500 et port UDP 4500|Si la stratégie de domaine exige que les communications réseau s'effectuent par le biais du protocole IPsec, vous devez également ajouter les ports UDP 4500 et UDP 500 à la liste des exceptions. IPsec est une option de l’ **Assistant Nouvelle règle de trafic entrant** dans le composant logiciel enfichable Pare-feu Windows. Pour plus d’informations, consultez ci-dessous [Utilisation du composant logiciel enfichable Pare-feu Windows avec fonctions avancées de sécurité](#BKMK_WF_msc) .|  
|Utilisation de l'authentification Windows avec les domaines approuvés|Les pare-feu doivent être configurés pour autoriser des demandes d'authentification.|Pour plus d'informations, consultez [Comment faire pour configurer un pare-feu pour les domaines et les approbations](https://support.microsoft.com/kb/179442/).|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le clustering Windows|Le clustering requiert des ports supplémentaires qui ne sont pas directement en rapport avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Pour plus d'informations, consultez [Activer un réseau pour une utilisation du cluster](/previous-versions/windows/it-pro/windows-server-2003/cc728293(v=ws.10)).|  
|Des espaces de noms réservés de l'URL dans l'API HTTP Server (HTTP.SYS)|Probablement le port TCP 80, mais la configuration d'autres ports est possible. Pour les informations générales, consultez [Configuration de HTTP et HTTPS](/dotnet/framework/wcf/feature-details/configuring-http-and-https).|Pour des informations spécifiques à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en ce qui concerne la réservation d’un point de terminaison HTTP.SYS à l’aide de HttpCfg.exe, consultez [À propos des réservations et de l’inscription d’URL &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md).|  
  
##  <a name="special-considerations-for-port-135"></a><a name="BKMK_port_135"></a> Considérations spéciales relatives au port 135  
 Lorsque vous utilisez RPC avec TCP/IP ou avec UDP/IP comme transport, les ports entrants sont fréquemment attribués de manière dynamique aux services système en fonction des besoins ; les ports TCP/IP et UDP/IP qui sont supérieurs au port 1024 sont utilisés. Ces ports sont souvent appelés de façon informelle « ports RPC aléatoires ». Dans ces cas, les clients RPC comptent sur le mappeur de points de terminaison RPC pour leur indiquer quels ports dynamiques ont été attribués au serveur. Pour certains services basés sur RPC, vous pouvez configurer un port spécifique au lieu de laisser RPC en attribuer un dynamiquement. Vous pouvez également limiter la plage de ports que RPC attribue dynamiquement, indépendamment du service. Étant donné que le port 135 est utilisé pour de nombreux services, il est fréquemment attaqué par des utilisateurs malveillants. Lorsque vous ouvrez le port 135, pensez à restreindre l'étendue de la règle de pare-feu.  
  
 Pour plus d'informations sur le port 135, consultez les rubriques de référence suivantes :  
  
-   [Vue d'ensemble des services et exigences de ports réseau pour le système Windows Server](https://support.microsoft.com/kb/832017)   
-   [Comment faire pour corriger les erreurs du mappeur de points de terminaison RPC à l'aide d'outils de support Windows Server 2003 disponibles sur le CD-ROM du produit](https://support.microsoft.com/kb/839880)  
-   [Appel de procédure distante (RPC)](/previous-versions/ms950395(v=msdn.10))    
-   [Comment faire pour configurer l'allocation de port dynamique RPC avec un pare-feu](https://support.microsoft.com/kb/154596/)  
  
##  <a name="interaction-with-other-firewall-rules"></a><a name="BKMK_other_rules"></a> Interaction avec d'autres règles de pare-feu  
 Le Pare-feu Windows utilise des règles et des groupes de règles pour établir sa configuration. Chaque règle ou groupe de règles est généralement associé à un programme ou service particulier, et ce programme ou service peut modifier ou supprimer qui règle à votre insu. Par exemple, les groupes de règles **Services World Wide Web (HTTP)** et **Services World Wide Web (HTTPS)** sont associés à IIS. L'activation de ces règles ouvrira les ports 80 et 443, et les fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui dépendent des ports 80 et 443 fonctionneront si ces règles sont activées. Toutefois, les administrateurs qui configurent IIS peuvent modifier ou désactiver ces règles. Par conséquent, si vous utilisez le port 80 ou le port 443 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez créer votre propre règle ou groupe de règles qui conserve votre configuration de port souhaitée indépendamment des autres règles IIS.  
  
 Le composant logiciel enfichable MMC du Pare-feu Windows avec fonctions avancées de sécurité autorise tout trafic qui correspond aux règles d'autorisation applicables. S'il existe deux règles qui s'appliquent toutes deux au port 80 (avec des paramètres différents), le trafic qui correspond à l'une ou l'autre règle sera autorisé. Si une règle autorise le trafic sur le port 80 du sous-réseau local et l'autre règle autorise le trafic à partir de n'importe quelle adresse, le résultat fait que tout le trafic vers le port 80 est autorisé indépendamment de la source. Pour gérer efficacement l'accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les administrateurs doivent périodiquement examiner toutes les règles de pare-feu activées sur le serveur.  
  
##  <a name="overview-of-firewall-profiles"></a><a name="BKMK_profiles"></a> Vue d'ensemble des profils de pare-feu  
 Les profils de pare-feu sont utilisés par les systèmes d’exploitation pour identifier et garder en mémoire chacun des réseaux auxquels ils se connectent (connectivité, connexions et catégorie).  
  
 Il existe trois types d'emplacements réseau dans le Pare-feu Windows avec fonctions avancées de sécurité :  
  
-   **Domaine** : Windows peut authentifier l'accès au contrôleur de domaine pour le domaine auquel l'ordinateur est joint.
-   **Public** : En dehors des réseaux de domaine, tous les réseaux sont catégorisés initialement en tant que publics. Les réseaux qui représentent des connexions directes à l'Internet ou qui se trouvent à emplacements publics, tels que les aéroports et les cafés, doivent rester publics.
-   **Privé** : Réseau identifié par un utilisateur ou une application comme privé. Seuls les réseaux de confiance doivent être identifiés en tant que réseaux privés. Les utilisateurs identifient généralement comme privés les réseaux domestiques ou les réseaux pour petites entreprises.  
  
 L'administrateur peut créer un profil pour chaque type d'emplacement réseau, chaque profil contenant des stratégies de pare-feu différentes. Un seul profil est appliqué à la fois. L'ordre des profils est appliqué comme suit :  
  
1.  Si toutes les interfaces sont authentifiées au contrôleur de domaine pour le domaine dans lequel l'ordinateur est membre, le profil de domaine est appliqué.    
2.  Si toutes les interfaces sont authentifiées au contrôleur de domaine ou sont connectées à des réseaux classifiés comme emplacements de réseau privés, le profil privé est appliqué.    
3.  Autrement, le profil public est appliqué.  
  
 Utilisez le composant logiciel enfichable MMC du Pare-feu Windows avec fonctions avancées de sécurité pour afficher et configurer tous les profils de pare-feu. L'élément du **Pare-feu Windows** dans le Panneau de configuration configure seulement le profil actuel.  
  
##  <a name="additional-firewall-settings-using-the-windows-firewall-item-in-control-panel"></a><a name="BKMK_additional_settings"></a> Paramètres de pare-feu supplémentaires utilisant l'élément Pare-feu Windows dans le Panneau de configuration  
 Les exceptions que vous ajoutez au pare-feu peuvent restreindre l'ouverture du port aux connexions entrantes à partir d'ordinateurs spécifiques ou du sous-réseau local. Cette restriction de la portée d'ouverture de port peut réduire la surface d'exposition de votre ordinateur aux utilisateurs malveillants, et elle est recommandée.  
  
> [!NOTE]  
>  L’utilisation de l’élément **Pare-feu Windows** dans le Panneau de configuration configure seulement le profil de pare-feu actuel.  
  
### <a name="to-change-the-scope-of-a-firewall-exception-using-the-windows-firewall-item-in-control-panel"></a>Pour modifier l'étendue d'une exception de pare-feu à l'aide de l'élément Pare-feu Windows dans le Panneau de configuration  
  
1.  Dans **Pare-feu Windows** du Panneau de configuration, sélectionnez un programme ou un port sous l'onglet **Exceptions** , puis cliquez sur **Propriétés** ou **Modifier**.  
  
2.  Dans la boîte de dialogue **Modifier un programme** ou **Modifier un port** , cliquez sur **Modifier l'étendue**.  
  
3.  Choisissez l’une des options suivantes :  
  
    -   **N’importe quel ordinateur (y compris ceux présents sur Internet)**  : Option non recommandée. Cela autorisera tout ordinateur qui peut adresser votre ordinateur à se connecter au programme ou port spécifié. Ce paramètre peut être nécessaire pour autoriser la présentation d'informations à des utilisateurs anonymes sur Internet, mais augmente votre exposition aux utilisateurs malveillants. Votre exposition peut être accrue encore plus si vous activez ce paramètre et également autorisez la traversée NAT (Network Address Translation), comme l'option Autoriser la traversée latérale.  
  
    -   **Uniquement mon réseau (ou sous-réseau)**  : Ce paramètre plus sécurisé que **N'importe quel ordinateur**. Seuls les ordinateurs présents sur le sous-réseau local de votre réseau peuvent se connecter au programme ou port.  
  
    -   **Liste personnalisée** : Seuls les ordinateurs qui correspondent aux adresses IP listées peuvent se connecter. Ce paramètre peut être plus sécurisé que l’option **Uniquement mon réseau (ou sous-réseau)** ; toutefois, les ordinateurs clients utilisant DHCP peuvent parfois modifier leur adresse IP. L'ordinateur prévu ne sera alors pas en mesure de se connecter. Un autre ordinateur, que vous n'aviez pas projeté d'autoriser, peut accepter l'adresse IP répertoriée puis être en mesure de se connecter. L’option **Liste personnalisée** peut être appropriée pour lister d’autres serveurs qui sont configurés pour utiliser une adresse IP fixe ; toutefois, les adresses IP peuvent être usurpées par un intrus. Les règles de pare-feu restrictives ne sont fortes que si votre infrastructure réseau l'est aussi.  
  
##  <a name="using-the-windows-firewall-with-advanced-security-snap-in"></a><a name="BKMK_WF_msc"></a> Utilisation du composant logiciel enfichable Pare-feu Windows avec fonctions avancées de sécurité  
 Des paramètres de pare-feu avancés supplémentaires peuvent être configurés en utilisant le composant logiciel enfichable MMC du Pare-feu Windows avec fonctions avancées de sécurité. Le composant logiciel enfichable inclut un Assistant Règle et expose des paramètres supplémentaires qui ne sont pas disponibles dans l’élément **Pare-feu Windows** du Panneau de configuration. Les paramètres suivants sont inclus :  
  
-   Paramètres de chiffrement  
-   Restrictions de services   
-   Restriction des connexions pour les ordinateurs par nom    
-   Restriction des connexions à des utilisateurs ou profils spécifiques  
-   Traversée latérale autorisant le trafic de contourner les routeurs NAT (Network Address Translation)    
-   Configuration de règles de trafic sortant    
-   Configuration de règles de sécurité    
-   Présence nécessaire d'IPsec pour les connexions entrantes  
  
### <a name="to-create-a-new-firewall-rule-using-the-new-rule-wizard"></a>Pour créer une règle de pare-feu à l'aide de l'Assistant Nouvelle règle  
  
1.  Dans le menu Démarrer, sélectionnez **Exécuter**, tapez **WF.msc**, puis sélectionnez **OK**.    
2.  Dans le **Pare-feu Windows avec fonctions avancées de sécurité**, dans le volet gauche, cliquez avec le bouton droit sur **Règles de trafic entrant**, puis sélectionnez **Nouvelle règle**.   
3.  Exécutez l' **Assistant Nouvelle règle de trafic entrant** à l'aide des paramètres que vous souhaitez.  
  
##  <a name="troubleshooting-firewall-settings"></a><a name="BKMK_troubleshooting"></a> Résolution des problèmes liés aux paramètres du pare-feu  
 Les outils et techniques suivants peuvent être utiles pour résoudre les problèmes liés au pare-feu :  
  
-   L'état effectif du port repose sur l'union de toutes les règles en rapport avec le port. Lors de la tentative de blocage de l’accès via un port, il peut être utile d’examiner toutes les règles qui citent le numéro de port. Pour cela, utilisez le composant logiciel enfichable MMC du Pare-feu Windows avec fonctions avancées de sécurité et triez les règles du trafic entrant et sortant par numéro de port.  
  
-   Examinez les ports qui sont actifs sur l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute. Ce processus de vérification inclut l'identification des ports TCP/IP qui écoutent, ainsi que de l'état des ports.  
  
     Pour identifier les ports qui sont à l’écoute, utilisez l’utilitaire de ligne de commande **netstat** . l’utilitaire **netstat** affiche non seulement les connexions TCP actives, mais également diverses statistiques et informations IP.  
  
    #### <a name="to-list-which-tcpip-ports-are-listening"></a>Pour lister les ports TCP/IP qui sont à l'écoute  
  
    1.  Ouvrez la fenêtre d'invite de commandes.  
  
    2.  À l’invite de commandes, tapez **netstat -n -a**.  
  
         Le commutateur **-n** demande à **netstat** d’afficher l’adresse numérique et le numéro de port des connexions TCP actives. Le commutateur **-a** demande à **netstat** d’afficher les ports TCP et UDP écoutés par l’ordinateur.  
  
-   L’utilitaire **PortQry** peut être utilisé pour signaler l’état des ports TCP/IP comme à l’écoute, pas à l’écoute ou filtré. (Lorsque l'état est filtré, le port peut être à l'écoute ou non ; cet état indique que l'utilitaire n'a pas reçu de réponse du port.) l’utilitaire **PortQry** peut être téléchargé à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=17148).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d'ensemble des services et exigences de ports réseau pour le système Windows Server](https://support.microsoft.com/kb/832017)   
 [Procédure : configurer les paramètres de pare-feu (Azure SQL Database)](/azure/azure-sql/database/firewall-configure)  
  
