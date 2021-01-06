---
title: Avant l’installation du clustering de basculement | Microsoft Docs
description: Cet article aborde la préparation de l’installation d’un cluster de basculement SQL Server, y compris le matériel, le système d’exploitation et la configuration.
ms.custom: ''
ms.date: 08/24/2016
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], preinstallation checklist
- installing failover clusters
- failover clustering [SQL Server], preinstallation checklist
ms.assetid: a655225d-8c54-4b30-95fd-31f588167899
author: cawrites
ms.author: chadam
ms.openlocfilehash: a59216090f680492bf41aec72711c2e38f62034d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642905"
---
# <a name="before-installing-failover-clustering"></a>Avant l'installation du clustering de basculement
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Avant d’installer un cluster de basculement SQL Server, vous devez sélectionner le matériel et le système d’exploitation que SQL Server utilisera. Vous devez aussi configurer le clustering de basculement Windows Server (WSFC) et examiner le réseau, la sécurité ainsi que les points importants à prendre en compte pour les autres logiciels qui seront exécutés sur votre cluster de basculement.  
  
 Si un cluster Windows dispose d'un lecteur de disque local et que la même lettre de lecteur est aussi utilisée sur un ou plusieurs nœuds de cluster en tant que lecteur partagé, vous ne pouvez pas installer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur ce lecteur. Cette restriction s’applique à la fois aux instances de cluster de basculement SQL Server et aux instances autonomes sur un serveur qui fait partie d’une instance de cluster de basculement Windows.
  
 Vous pouvez également examiner les rubriques suivantes pour en savoir plus sur les concepts, les fonctionnalités et les tâches de clustering de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
|Description des rubriques|Rubrique|  
|-----------------------|-----------|  
|Décrit les concepts de clustering de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et fournit des liens vers les contenus et tâches associés.|[Instances de cluster de basculement Always On &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)|  
|Décrit les concepts de stratégie de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et fournit des liens pour la configuration de la stratégie de basculement selon les besoins de votre organisation.|[Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)|  
|Décrit comment gérer votre cluster de basculement existant [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Administration et maintenance de l'instance de cluster de basculement](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)|  
|Explique comment installer [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] sur un cluster de basculement Windows Server (WSFC).|[Procédure : mettre en cluster SQL Server Analysis Services](https://go.microsoft.com/fwlink/p/?LinkId=396548)|  
  
 
  
##  <a name="best-practices"></a><a name="BestPractices"></a> Bonnes pratiques  
  
-   Passez en revue les [Notes de publication](https://go.microsoft.com/fwlink/?LinkId=296445) de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]  
  
-   Installez les logiciels requis. Avant d'exécuter l'installation ou la mise à niveau vers [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], installez les composants requis suivants pour réduire la durée d'installation. Vous pouvez installer les logiciels requis sur chaque nœud de cluster de basculement, puis redémarrer les nœuds une fois avant d'exécuter le programme d'installation.  
  
    -   Windows PowerShell n'est plus installé par le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Windows PowerShell est un composant requis pour l’installation des composants du [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)][!INCLUDE[ssDE](../../../includes/ssde-md.md)] et de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Si Windows PowerShell n’est pas présent sur votre ordinateur, vous pouvez l’activer en suivant les instructions de la page [Windows Management Framework](https://go.microsoft.com/fwlink/?LinkId=186214) .  
  
    -   Le .NET Framework 3.5 SP1 n'est plus installé par le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], mais il peut être requis lors de l'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur des systèmes d'exploitation Windows plus anciens. Pour plus d’informations, consultez les [notes de publication](https://go.microsoft.com/fwlink/?LinkId=296445) de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
    -   **Mise à jour [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Update :** Pour éviter le redémarrage de l'ordinateur suite à l'installation de .NET Framework 4 pendant l'installation, le programme d'installation [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] requiert l'installation de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] sur l'ordinateur.  Si vous installez [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] sur Windows 7 SP1 ou [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] SP2, cette mise à jour est incluse. Si vous installez sur un système d'exploitation Windows plus ancien, vous pouvez la télécharger à partir de la [Mise à niveau Microsoft pour le .NET Framework 4.0 sur Windows Vista et Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=198093).  
  
    -   .NET Framework 4 : Le programme d'installation installe le .NET Framework 4 sur un système d'exploitation en cluster. Pour réduire le temps d'installation, vous pouvez envisager d'installer le .NET Framework 4 avant d'exécuter le programme d'installation.  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Vous pouvez installer ces fichiers en exécutant SqlSupport.msi qui se trouve sur le support d'installation de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] .  
  
-   Vérifiez qu'aucun logiciel antivirus n'est installé sur votre cluster WSFC. Pour plus d’informations, consultez l’article [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Le logiciel antivirus peut être à l’origine de problèmes avec les services de cluster [de la Base de connaissances](https://go.microsoft.com/fwlink/?LinkId=116986).  
  
-   Lorsque vous nommez un groupe de clusters pour votre installation de clusters de basculement, vous ne devez pas utiliser les caractères suivants dans le nom du groupe de clusters :  
  
    -   Opérateur Inférieur à (\<)  
  
    -   Opérateur Supérieur à (>)  
  
    -   Guillemet (")  
  
    -   Apostrophe (')  
  
    -   Esperluette (&)  
  
     Vérifiez également que les noms de groupes de clusters existants ne contiennent pas de caractères non pris en charge.  
  
-   Vérifiez que tous les nœuds du cluster sont configurés de la même manière, y compris COM+, les lettres de lecteur de disque et les utilisateurs dans le groupe Administrateurs.  
  
-   Vérifiez que vous avez effacé les journaux système dans tous les nœuds, puis affiché de nouveau les journaux système. Vérifiez que les journaux ne contiennent aucun message d'erreur avant de continuer.  
  
-   Avant d'installer ou de mettre à jour un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , désactivez l'ensemble des applications et des services susceptibles d'utiliser les composants [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] au cours de l'installation, mais maintenez en ligne les ressources de disque.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] définit automatiquement les dépendances entre le groupe de clusters [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et les disques qui figurent dans le cluster de basculement. Ne définissez pas de dépendances pour les disques avant l'installation.  
  
    -   Pendant l'installation du cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , un objet ordinateur (compte d'ordinateur Active Directory) pour la ressource de nom réseau [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est créé. Dans un cluster [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] , le nom de compte du cluster (compte d'ordinateur du cluster lui-même) doit disposer d'autorisations pour créer des objets ordinateur. Pour plus d'informations, consultez [Configuration de comptes dans Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx).  
  
    -   Si vous utilisez le partage de fichiers SMB comme option de stockage, le compte d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit avoir les privilèges SeSecurityPrivilege sur le serveur de fichiers. Pour ce faire, dans la console de stratégie de sécurité locale du serveur de fichiers, ajoutez le compte utilisé pour l'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aux droits **Gérer le journal d'audit et de la sécurité** .  
  
##  <a name="verify-your-hardware-solution"></a><a name="Hardware"></a> Vérifiez votre solution matérielle  
  
-   Si la solution de cluster inclut des nœuds de cluster dispersés géographiquement, il est important de vérifier d'autres éléments, tels que la prise en charge de disques partagés et le temps de réponse du réseau.  
  
    -   Pour plus d'informations sur [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] et [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)], consultez [Validation matérielle d’un cluster de basculement](https://go.microsoft.com/fwlink/?LinkId=196817) et [Politique de prise en charge pour les clusters de basculement Windows](https://go.microsoft.com/fwlink/?LinkId=196818).  
  
-   Vérifiez que le disque sur lequel [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit être installé n'est pas compressé ni chiffré. Si vous tentez d'installer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur un lecteur compressé ou chiffré, l'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] échoue.  
  
-   Les configurations SAN sont également prises en charge sous [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] et les éditions Advanced Server et Datacenter Server de [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] . La catégorie « Périphérique cluster/multi-cluster » du Catalogue Windows et de la liste des matériels compatibles avec Windows répertorie les unités de stockage compatibles SAN qui ont été testées et qui sont prises en charge en tant qu'unités de stockage SAN avec plusieurs clusters WSFC associés. Exécutez la validation de cluster après avoir trouvé les composants certifiés.  
  
-   Le partage de fichiers SMB est également pris en charge pour l'installation des fichiers de données. Pour plus d'informations, consultez [Storage Types for Data Files](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes).  
  
    > [!WARNING]  
    >  Si vous utilisez un serveur de fichiers Windows comme stockage de partage de fichiers SMB, le compte d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit avoir les privilèges SeSecurityPrivilege sur le serveur de fichiers. Pour ce faire, dans la console de stratégie de sécurité locale du serveur de fichiers, ajoutez le compte utilisé pour l'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aux droits **Gérer le journal d'audit et de la sécurité** .  
    >   
    >  Si vous utilisez un stockage de partage de fichiers SMB autre que le serveur de fichiers Windows, consultez le fournisseur de stockage pour une installation équivalente du côté du serveur de fichiers.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge des points de montage.  
  
     Un volume monté, ou point de montage, vous permet d'utiliser une seule lettre de lecteur pour faire référence à de nombreux disques ou volumes. Si vous disposez d'une lettre de lecteur D: qui fait référence à un disque ou volume traditionnel, vous pouvez connecter ou « monter » d'autres disques ou volumes comme répertoires sous la lettre de lecteur D: sans que les disques ou volumes supplémentaires ne nécessitent des lettres de lecteurs qui leur sont propres.  
  
     Remarques supplémentaires relatives aux points de montage pour le clustering de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , le lecteur de base d'un lecteur monté doit posséder une lettre de lecteur associée. Pour les installations de clusters de basculement, ce lecteur de base doit être un lecteur en cluster. Les GUID du volume ne sont pas pris en charge dans cette version.  
  
    -   Le lecteur de base (celui doté de la lettre de lecteur) ne peut pas être partagé entre des instances de clusters de basculement. Cette restriction est normale pour les clusters de basculement, mais elle ne s'applique pas aux serveurs autonomes à plusieurs instances.  
  
    -   Les installations en cluster de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont limitées au nombre de lettres de lecteurs disponibles. Si l'on part du principe que vous utilisez une seule lettre de lecteur pour le système d'exploitation et que toutes les autres lettres de lecteurs peuvent être utilisées comme lecteurs en cluster standard ou lecteurs en cluster hébergeant des points de montage, vous êtes limité à un maximum de 25 instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] par cluster de basculement.  
  
        > [!TIP]  
        >  La limite de 25 instances peut être évitée à l'aide de l'option de partage de fichiers SMB. Si vous utilisez le partage de fichiers SMB comme option de stockage, vous pouvez installer jusqu'à 50 instances de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    -   Le formatage d'un lecteur après montage de lecteurs supplémentaires n'est pas pris en charge.  
  
-   L'installation du cluster de basculement[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge uniquement le disque local pour l'installation des fichiers tempdb. Assurez-vous que le chemin d'accès spécifié pour les données tempdb et les fichiers journaux sont valides sur tous les nœuds du cluster. Pendant le basculement, si les répertoires tempdb ne sont pas disponibles sur le nœud de basculement cible, la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne sera pas en ligne. Pour plus d’informations, consultez [Types de stockage pour les fichiers de données](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes) et [Configuration du moteur de base de données – Répertoires de données](https://msdn.microsoft.com/library/9b1fa0fc-623b-479a-afc3-4f13bd850487).  
  
-   Si vous déployez un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur des composants de la technologie iSCSI, nous vous recommandons de prendre les précautions qui s'imposent. Pour plus d'informations, consultez [Prise en charge de SQL Server sur des composants de la technologie iSCSI](https://go.microsoft.com/fwlink/?LinkId=116960).  
  
-   Pour plus d'informations, consultez [Politique de prise en charge SQL Server pour le clustering Microsoft](https://go.microsoft.com/fwlink/?LinkId=116958).  
  
-   Pour plus d'informations sur la configuration appropriée d'un lecteur quorum, consultez [Informations concernant la configuration d'un lecteur quorum](https://go.microsoft.com/fwlink/?LinkId=196816).  
  
-   Pour installer un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] lorsque les fichiers d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sources et le cluster se trouvent dans des domaines différents, copiez les fichiers d'installation sur le domaine actuellement disponible sur le cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
##  <a name="review-security-considerations"></a><a name="Security"></a> Passez en revue les considérations sur la sécurité  
  
-   Pour utiliser le chiffrement, installez le certificat du serveur avec le nom DNS complet du cluster WSFC sur tous les nœuds du cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Par exemple, si vous disposez d'un cluster à deux nœuds appelés « Test1.NomDomaine.com » et « Test2.NomDomaine.com » et d'une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] appelée « Virtsql », vous devez vous procurer un certificat pour « Virtsql.NomDomaine.com » et installer le certificat sur les nœuds test1 et test2. Vous pouvez ensuite activer la case à cocher **Forcer le chiffrement du protocole** dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour configurer votre cluster de basculement à des fins de chiffrement.  
  
    > [!IMPORTANT]  
    >  Ne cochez pas la case **Forcer le chiffrement du protocole** tant que les certificats n’ont pas été installés sur tous les nœuds participants de votre instance de cluster de basculement.  
  
-   Pour les installations [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans des configurations côte à côte avec des versions antérieures, les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doivent utiliser des comptes figurant uniquement dans le groupe global des domaines. En outre, les comptes utilisés par les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne doivent pas figurer dans le groupe local Administrateurs. Ne pas se conformer à cette consigne entraînera un comportement de sécurité inattendu.  
  
-   Pour créer un cluster de basculement, vous devez être un administrateur local autorisé à se connecter en tant que service et à agir dans le cadre du système d'exploitation sur tous les nœuds de l'instance de cluster de basculement.  
  
-   Sur [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)], les SID de service sont générés automatiquement pour une utilisation avec les services [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] . Pour les instances de cluster de basculement [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] mises à niveau à partir d'anciennes versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], les groupes de domaines et configurations des listes de contrôle d'accès existants seront conservés.  
  
-   Les groupes de domaines doivent se trouver dans le même domaine que les comptes d'ordinateurs. Si, par exemple, l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sera installé se trouve dans le domaine SQLSVR, enfant de MYDOMAIN, vous devez spécifier un groupe dans le domaine SQLSVR. Le domaine SQLSVR peut contenir des comptes d'utilisateurs de MYDOMAIN.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut pas être installé lorsque les nœuds du cluster sont des contrôleurs de domaine.  
  
-   Consultez le contenu de [Security Considerations for a SQL Server Installation](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
-   Pour activer l'authentification Kerberos avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez l'article [Utiliser l'authentification Kerberos dans SQL Server](https://support.microsoft.com/kb/319723) de la Base de connaissances [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .  

-   L’instance de cluster de basculement (FCI) SQL Server exige que les nœuds de cluster soient joints à un domaine. Les configurations suivantes ne sont **pas prises en charge** : 
    *   instance de cluster de basculement SQL sur des clusters de groupe de travail ; 
    *   instance de cluster de basculement SQL sur un cluster multidomaine ;   
    *   instance de cluster de basculement SQL sur des clusters de domaine + de groupe de travail. 

  
##  <a name="review-network-port-and-firewall-considerations"></a><a name="Network"></a> Passez en revue les considérations relatives aux réseau, port et pare-feu  
  
-   Vérifiez que vous avez désactivé NetBIOS pour toutes les cartes réseau privées avant d'entamer la procédure d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Le nom réseau et l'adresse IP de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne doivent pas être utilisés à d'autres fins, telles que le partage de fichiers. Si vous souhaitez créer une ressource de partage de fichier, utilisez un nom réseau et une adresse IP différents et uniques pour la ressource.  
  
    > [!IMPORTANT]  
    >  Nous vous déconseillons d'utiliser des partages de fichiers sur des lecteurs de données, car ils ont une incidence sur les performances et le comportement de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Bien que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prenne en charge les canaux nommés et les sockets TCP/IP sur TCP/IP au sein d'un cluster, nous vous recommandons d'utiliser des sockets TCP/IP dans une configuration en cluster.  
  
-   Notez que le serveur ISA Server n'est pas pris en charge sur le clustering Windows ; par conséquent, il n'est pas non plus pris en charge sur les clusters de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Le service Registre distant doit être activé et en cours d'exécution.  
  
-   L'administration à distance doit être activée.  
  
- Pour les instances SQL Server utilisant un port autre que le port par défaut, utilisez la configuration réseau du Gestionnaire de configuration SQL Server pour déterminer le port utilisé par l’instance SQL Server que vous voulez débloquer. Activez le port TCP pour IPALL dans le pare-feu si vous voulez vous connecter à votre instance SQL Server en utilisant le [service SQL Server Browser](../../../tools/configuration-manager/sql-server-browser-service.md), qui utilise une adresse IP différente de celle de l’instance clusterisée et le port UDP 1434. 
  
-   Les opérations d'installation du cluster de basculement incluent une règle qui vérifie l'ordre de liaison réseau. Bien que les ordres de liaison semblent corrects, il se peut que vous ayez désactivé ou créé des configurations de carte réseau « fantômes » sur le système. Les configurations de carte réseau « fantômes » peuvent affecter l'ordre de liaison et provoquer l'émission d'un avertissement par ce dernier. Pour éviter cela, effectuez les opérations suivantes pour identifier et supprimer les cartes réseau désactivées :  
  
    1.  À une invite de commandes, entrez : set devmgr_Show_Nonpersistent_Devices=1.  
  
    2.  Tapez et exécutez : start Devmgmt.msc.  
  
    3.  Développez la liste des cartes réseau. Seuls les adaptateurs physiques doivent figurer dans la liste. Si une carte réseau est désactivée, le programme d'installation signalera un échec pour la règle de l'ordre de liaison réseau. Le Panneau de configuration/Connexions réseau indiquera également que l'adaptateur a été désactivé. Assurez-vous que les Paramètres réseau du Panneau de configuration affichent la même liste d'adaptateurs physiques actifs que devmgmt.msc.  
  
    4.  Supprimez les cartes réseau désactivées avant d’exécuter l’installation de SQL Server.  
  
    5.  À l'issue de l'installation, retournez dans Connexions réseau dans le Panneau de configuration et désactivez les cartes réseau qui ne sont pas actuellement utilisées.  
  
##  <a name="verify-your-operating-system"></a><a name="OS_Support"></a> Vérifiez votre système d'exploitation  
 Assurez-vous que votre système d'exploitation est installé correctement et qu'il prend en charge le clustering de basculement. Le tableau suivant comporte une liste des éditions [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et des systèmes d'exploitation qui les prennent en charge.  
  
|Édition de[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|[!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] Enterprise|[!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] Datacenter Server|[!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] Enterprise|[!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] Datacenter Server|  
|---------------------------------------|------------------------------------------------|-------------------------------------------------------|----------------------------------------------|-----------------------------------------------------|  
|[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Enterprise (64 bits) x64*|Oui|Oui|Oui**|Oui**|  
|[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Enterprise (32 bits)|Oui|Oui|||  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] bits) Developer (64|Oui|Oui|Oui**|Oui**|  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] Developer (32 bits)|Oui|Oui|||  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] Standard (64 bits)|Oui|Oui|Oui|Oui|  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] Standard (32 bits)|Oui|Oui|||  
  
 *[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne sont pas pris en charge en mode WOW. Cela inclut les mises à niveau de versions précédentes des clusters de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installés à l'origine dans WOW. Pour ceux-là, la seule option de mise à niveau est d'installer la nouvelle version côte à côte et de migrer.  
  
 **Pris en charge pour le clustering de basculement de plusieurs sous-réseaux [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
##  <a name="additional-considerations-for-multi-subnet-configurations"></a><a name="MultiSubnet"></a> Remarques supplémentaires concernant les configurations de sous-réseaux multiples  
 Les sections ci-dessous décrivent les points à garder à l'esprit lors de l'installation d'un cluster de basculement de sous-réseaux multiples [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Une configuration de sous-réseaux multiples implique le clustering de plusieurs sous-réseaux et, par conséquent, l'utilisation de plusieurs adresses IP et la modification des dépendances de ressource d'adresse IP.  
  
### <a name="ssnoversion-edition-and-operating-system-considerations"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Considérations relatives au système d’exploitation et à l’édition  
  
-   Pour plus d’informations sur les éditions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui prennent en charge un cluster de basculement de plusieurs sous-réseaux [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Pour créer un cluster de basculement de plusieurs sous-réseaux [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vous devez d'abord créer le cluster de basculement multisite [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] sur plusieurs sous-réseaux.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dépend du cluster de basculement Windows Server pour s'assurer que les conditions de la dépendance d'IP est valide en cas de basculement.  
  
-   [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] requiert que tous les serveurs de clusters soient dans le même domaine Active Directory. Par conséquent, le cluster de basculement de sous-réseaux multiples [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] requiert que tous les nœuds de cluster soient dans le même domaine Active Directory même s'ils se trouvent dans des sous-réseaux différents.  
  
#### <a name="ip-address-and-ip-address-resource-dependencies"></a>Adresse IP et dépendances de ressource d'adresse IP  
  
1.  La dépendance de ressource d'adresse IP est définie à OR dans une configuration de sous-réseaux multiples. Pour plus d’informations, consultez [Créer un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
  
2.  Les dépendances d'adresse IP mixtes AND-OR ne sont pas prises en charge. Par exemple, \<IP1> AND \<IP2> OR \<IP3> n’est pas pris en charge.  
  
3.  La présence de plusieurs adresses IP par sous-réseau n'est pas prise en charge.  
  
     Si vous décidez d'utiliser plusieurs adresses IP configurées pour le même sous-réseau, vous pouvez rencontrer des échecs de connexion client au démarrage de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
#### <a name="related-content"></a>Contenu associé  
 Pour plus d’informations sur le basculement multisite [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] , consultez [Site de clustering de basculement Windows Server 2008 R2](https://technet.microsoft.com/library/ff182338\(v=WS.10\).aspx) et [Concevoir un service ou une application en cluster dans un clustering de basculement multisite](https://go.microsoft.com/fwlink/?LinkId=177873).  
  
##  <a name="configure-windows-server-failover-cluster"></a><a name="WSFC"></a> Configurer le cluster de basculement Windows Server  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Le logiciel WSFC (service de cluster) doit être configuré sur au moins l’un des nœuds de votre cluster de serveurs. Vous devez également exécuter [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Enterprise, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Business Intelligence ou [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Standard conjointement à WSFC. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Enterprise prend en charge les clusters de basculement contenant jusqu'à 16 nœuds. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Business Intelligence et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Standard prennent en charge les clusters de basculement à deux nœuds.  
  
-   La DLL de ressource pour le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] exporte deux fonctions utilisées par le gestionnaire de cluster WSFC pour vérifier la disponibilité de la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Stratégie de basculement pour les instances de cluster de basculement](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md).  
  
-   WSFC doit être en mesure de vérifier que l'instance de cluster de basculement s'exécute à l'aide de la vérification IsAlive. Cela signifie qu'il convient de se connecter au serveur à l'aide d'une connexion approuvée. Par défaut, le compte qui exécute le service de cluster n'est pas configuré en tant qu'administrateur sur les nœuds du cluster, et le groupe BUILTIN\Administrateurs n'a pas l'autorisation de se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ces paramètres changent uniquement si vous modifiez les autorisations définies sur les nœuds du cluster.  
  
-   Configurez le service DNS (Domain Name Service) ou le service WINS (Windows Internet Name Service). Un serveur DNS ou un serveur WINS doit s'exécuter dans l'environnement où votre cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sera installé. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nécessite un enregistrement DNS dynamique de la référence virtuelle de l'interface IP [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . La configuration du serveur DNS doit permettre aux nœuds de cluster d'inscrire dynamiquement une table d'adresses IP en ligne sur le nom du réseau. Si l'enregistrement dynamique ne peut pas être effectué, le programme d'installation échoue et l'installation est restaurée. Pour plus d'informations, consultez [cet article de la base de connaissances](https://support.microsoft.com/kb/947048)  
  
##  <a name="install-msconame-distributed-transaction-coordinator"></a><a name="MSDTC"></a> Installer [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Distributed Transaction Coordinator  
 Avant d'installer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur un cluster de basculement, déterminez si la ressource de cluster [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Distributed Transaction Coordinator (MSDTC) doit être créée. Si vous installez uniquement le [!INCLUDE[ssDE](../../../includes/ssde-md.md)], la ressource de cluster MSDTC n'est pas nécessaire. Si vous installez le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] et SSIS, les composants de station de travail, ou si vous comptez utiliser les transactions distribuées, vous devez installer MSDTC. Notez que MSDTC n'est pas requis pour les instances d' [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]uniquement.  
  
 Sur [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] et [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)], vous pouvez installer plusieurs instances de MSDTC sur un cluster de basculement unique. La première instance de MSDTC installée sera l'instance de cluster par défaut de MSDTC. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tirera parti d'une instance de MSDTC installée automatiquement sur le groupe de ressources de cluster local [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en utilisant automatiquement l'instance de MSDTC. Toutefois, les applications peuvent être mappées individuellement à toute instance de MSDTC sur le cluster.  
  
 Les règles suivantes sont appliquées pour une instance de MSDTC devant être choisie par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:  
  
-   Utilisation de MSDTC installé dans le groupe local, sinon  
  
-   Utilisation de l'instance mappée de MSDTC, sinon  
  
-   Utilisation de l'instance par défaut du cluster de MSDTC, sinon  
  
-   Utiliser l’instance de MSDTC installée sur l’ordinateur local  
  
> [!IMPORTANT]  
>  Si l'instance MSDTC installée dans le groupe de clusters local de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] échoue, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'essaie pas automatiquement d'utiliser l'instance de cluster par défaut ou l'instance de MSDTC de l'ordinateur local. Vous devrez supprimer complètement l'instance non réussie de MSDTC du groupe [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour utiliser une autre instance de MSDTC. De même, si vous créez un mappage pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et si l'instance mappée de MSDTC échoue, vos transactions distribuées échoueront également. Si vous souhaitez que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilise une autre instance de MSDTC, vous devez ajouter une instance de MSDTC au groupe de clusters local de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou supprimer le mappage.  
  
### <a name="configure-msconame-distributed-transaction-coordinator"></a>Configurer le coordinateur de transactions distribuées [!INCLUDE[msCoName](../../../includes/msconame-md.md)] (MSDTC)  
 Une fois que vous avez installé le système d'exploitation et configuré votre cluster, vous devez configurer MSDTC pour qu'il fonctionne dans un cluster à l'aide de l'Administrateur de cluster. L'échec de la mise en cluster de MSDTC ne bloquera pas l'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , mais les fonctionnalités des applications [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peuvent être affectées si MSDTC n'est pas configuré correctement.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Paramètres de l'outil d'analyse de configuration système](../../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)   
 [Administration et maintenance de l'instance de cluster de basculement](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)  
  
  

