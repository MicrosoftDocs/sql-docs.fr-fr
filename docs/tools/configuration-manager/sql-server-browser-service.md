---
title: SQL Server Browser Service
description: Découvrez comment utiliser SQL Server Browser, un service qui écoute les demandes ayant trait aux ressources SQL Server et fournit des informations sur les instances SQL installées.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
f1_keywords:
- sql13.swb.browseservers.local.f1
- sql13.swb.browseservers.network.f1
helpviewer_keywords:
- services [SQL Server], security
- SQL Browser service (See SQL Server Browser Service)
- Browser Service
- SQL Server Browser service
ms.assetid: 3cc00d3a-487c-4cd9-a155-655f02485fa0
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a4c58894434c503f92c8bcf4df4f74cf9fa2c688
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345196"
---
# <a name="sql-server-browser-service"></a>SQL Server Browser Service
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Le programme [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Browser s’exécute en tant que service Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est à l’écoute des demandes entrantes pour les ressources [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et fournit des informations sur les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installées sur l’ordinateur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser contribue aux actions suivantes :  
  
-   Affichage de la liste des serveurs disponibles  
  
-   Connexion à l'instance correcte du serveur  
  
-   Connexion aux points de terminaison d'une connexion administrateur dédiée  
  
 Pour chaque instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] et de [!INCLUDE[ssAS](../../includes/ssas-md.md)], le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser (sqlbrowser) fournit le nom de l’instance et le numéro de version. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est installé avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser peut être configuré pendant l’installation ou en utilisant le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par défaut, le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser démarre automatiquement :  
  
-   lors de la mise à niveau d'une installation ;  
  
-   lors d'une installation sur un cluster ;  
  
-   lors de l’installation d’une instance nommée de [!INCLUDE[ssDE](../../includes/ssde-md.md)] , notamment toutes les instances de SQL Server Express ;  
  
-   lors de l'installation d'une instance nommée du service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
## <a name="background"></a>Arrière-plan  
 Avant [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], une seule instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pouvait être installée sur un ordinateur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] était à l’écoute des demandes entrantes sur le port 1433, attribué à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par l’IANA (Internet Assigned Numbers Authority). L'utilisation d'un port étant limitée à une seule instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , lorsque [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] a introduit la prise en charge de plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le protocole SSRP ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Resolution Protocol) a été développé afin de permettre une écoute sur le port UDP 1434. Ce service d'écoute répondait aux demandes des clients avec les noms des instances installées et les ports ou les canaux nommés utilisés par l'instance. Pour parer aux limites du système SSRP, [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] introduit le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser en remplacement de SSRP.  
  
## <a name="how-sql-server-browser-works"></a>Fonctionnement de SQL Server Browser  
 Au moment du démarrage d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , si le protocole TCP/IP est activé pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un port TCP/IP est attribué au serveur. Si le protocole des canaux nommés est activé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est à l'écoute d'un canal nommé spécifique. Ce port, ou « canal de communication », est utilisé par cette instance spécifique pour l'échange de données avec les applications clientes. Au cours de l'installation, le port TCP 1433 et le canal de communication `\sql\query` sont affectés à l'instance par défaut, mais ceux-ci peuvent être modifiés ultérieurement par l'administrateur du serveur via le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans la mesure où un port ou un canal de communication ne peut être utilisé que par une seule instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , différents numéros de ports et noms de canaux de communication sont attribués pour les instances nommées, y compris [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]. Par défaut, lorsque les instances nommées et [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] sont activés, ils sont configurés pour utiliser des ports dynamiques. En d'autres termes, un port disponible est attribué lors du démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si nécessaire, un port spécifique peut être attribué à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Au moment de la connexion, les clients peuvent désigner un port spécifique. Toutefois, si le port est attribué dynamiquement, le numéro de port peut changer lors de chaque redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , auquel cas le numéro de port correct est inconnu du client.  
  
 Lors du démarrage, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser réclame le port UDP 1434. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser lit le Registre, identifie toutes les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l’ordinateur, puis note les ports et autres canaux nommés qu’elles utilisent. Lorsqu'un serveur est équipé de deux cartes réseau ou plus, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser renvoie le premier port activé qu'il détecte pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser pend en charge les protocoles ipv6 et ipv4.  
  
 Lorsque les clients [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] demandent des ressources [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la bibliothèque réseau cliente envoie un message UDP au serveur à l'aide du port 1434. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser répond avec le port TCP/IP ou canal nommé de l’instance demandée. La bibliothèque réseau de l'application cliente établit alors la connexion en envoyant une demande au serveur en utilisant le port ou le canal nommé de l'instance souhaitée.  
  
 Pour plus d'informations sur le démarrage et l'arrêt du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser, consultez la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="using-sql-server-browser"></a>Utilisation de SQL Server Browser  
 Si le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser n'est pas en cours d'exécution, vous pouvez quand même vous connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , à condition d'indiquer le numéro de port ou le canal de communication nommé correct. Par exemple, vous pouvez vous connecter à l'instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] via le protocole TCP/IP si elle s'exécute sur le port 1433.  
  
 Toutefois, si le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser n'est pas en cours d'exécution, les connexions suivantes ne fonctionnent pas :  
  
-   tout composant essayant de se connecter à une instance nommée sans spécifier intégralement tous les paramètres (tels que le port TCP/IP ou le canal nommé) ;  
  
-   tout composant générant ou transmettant des informations serveur/instance qui pourraient être utilisées ultérieurement par d'autres composants pour se reconnecter ;  
  
-   connexion à une instance nommée sans fournir le numéro de port ou le canal de communication ;  
  
-   connexion administrateur dédiée (DAC) à une instance nommée si le port TCP/IP 1433 n'est pas utilisé ;  
  
-   service redirecteur OLAP ;  
  
-   énumération de serveurs dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], Enterprise Manager ou dans l'Analyseur de requêtes.  
  
 Si vous utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un scénario client-serveur (par exemple, lorsque votre application accède à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à travers un réseau) et que vous arrêtez ou désactivez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser, vous devez attribuer un numéro de port spécifique à chaque instance et écrire le code de votre application cliente afin de toujours utiliser ce numéro de port. Cette approche présente les inconvénients suivants :  
  
-   Vous devez mettre à jour et maintenir le code de l'application cliente pour vous assurer qu'elle se connecte au bon port.  
  
-   Le port que vous choisissez pour chaque instance peut être utilisé par un autre service ou une autre application présents sur le serveur, ce qui entraîne la non-disponibilité de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="clustering"></a>Clustering  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser n’est pas une ressource cluster et ne prend pas en charge le basculement d’un nœud de cluster à l’autre. Par conséquent, dans le cas d'un cluster, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser doit être installé et activé pour chaque nœud du cluster. Dans les clusters, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est à l'écoute de IP_ANY.  
  
> [!NOTE]  
>  Dans ce cas, si vous activez l'écoute sur des ports IP spécifiques, l'utilisateur doit configurer le même port TCP sur chaque adresse IP, car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser renvoie la première paire IP/port qu'il détecte.  
  
## <a name="installing-uninstalling-and-running-from-the-command-line"></a>Installation, désinstallation et exécution à partir de la ligne de commande  
 Par défaut, le programme [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est installé dans C:\Program Files (x86)\Microsoft SQL Server\90\Shared\sqlbrowser.exe.  
  
 Le service de navigateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est désinstallé lorsque la dernière instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est supprimée.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser peut être démarré à partir de l’invite de commandes à des fins de dépannage au moyen du commutateur **-c** :  
  
```  
<drive>\<path>\sqlbrowser.exe -c  
```  
  
## <a name="security"></a>Sécurité  
  
### <a name="account-privileges"></a>Privilèges de compte  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est à l’écoute d’un port UDP et accepte les demandes non authentifiées via le protocole SSRP ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Resolution Protocol). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser doit être exécuté dans le contexte de sécurité d’un utilisateur doté de faibles privilèges afin de limiter les risques d’attaque malveillante. Le compte de connexion peut être modifié à partir du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les droits d'utilisateur minimaux pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser sont les suivants :  
  
-   Refuser l'accès à cet ordinateur à partir du réseau  
  
-   Refuser les ouvertures de session locales  
  
-   Refuser l'ouverture de session en tant que programme de traitement par lots  
  
-   Interdire l'ouverture de session par les services Terminal Server  
  
-   Ouvrir une session en tant que service  
  
-   Lire et écrire les clés de Registre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] relatives à la communication réseau (ports et canaux de communication)  
  
### <a name="default-account"></a>Compte par défaut  
 Le programme d'installation configure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser de sorte qu'il utilise le compte sélectionné pour les services au cours de l'installation. Les autres comptes possibles sont les suivants :  
  
-   Tout compte **domaine\local**  
  
-   Le compte de **service local**  
  
-   Le compte **système local** (non recommandé car doté de privilèges inutiles)  
  
### <a name="hiding-sql-server"></a>Masquage de SQL Server  
 Les instances masquées sont des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne prennent en charge que les connexions de mémoire partagée. Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], définissez l'indicateur `HideInstance` pour indiquer que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser ne doit pas répondre par des informations sur cette instance de serveur.  
  
### <a name="using-a-firewall"></a>Utilisation d'un pare-feu  
 Pour communiquer avec le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser sur un serveur situé derrière un pare-feu, ouvrez le port UDP 1434 en plus du port TCP utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (par ex., le port 1433). Pour plus d’informations sur l’utilisation d’un pare-feu, consultez « Guide pratique pour configurer un pare-feu pour accéder à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Protocoles réseau et bibliothèques réseau](../../sql-server/install/network-protocols-and-network-libraries.md)  
  
  
