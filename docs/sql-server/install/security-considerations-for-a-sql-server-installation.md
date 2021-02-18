---
title: Considérations relatives à la sécurité
description: Cet article traite des meilleures pratiques de sécurité qu’il est conseillé de suivre avant et après l’installation de SQL Server.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- firewall systems [SQL Server]
- server message blocks [SQL Server]
- disabling protocols
- security [SQL Server], installations
- protocols [SQL Server], disabling
- NetBIOS [SQL Server]
- services [SQL Server], security
- SMB [SQL Server]
- isolating services [SQL Server]
- Setup [SQL Server], security
- low SQL Server installation privileges
- NTFS [SQL Server]
- physical security [SQL Server]
- file system security [SQL Server]
- installing SQL Server, security
ms.assetid: cf96155f-30a8-48b7-8d6b-24ce90dafdc7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5f8a540cf1f740ad2f9cb712db4bdd51f6c1a623
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336114"
---
# <a name="security-considerations-for-a-sql-server-installation"></a>Considérations sur la sécurité pour une installation SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

 La sécurité est importante pour chaque produit et chaque activité. En respectant les recommandations les plus simples, vous pouvez éviter de nombreuses failles de sécurité. Cet article traite des recommandations de sécurité qu’il est conseillé de suivre avant d’installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et après avoir installé [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Des conseils en matière de sécurité concernant des fonctionnalités spécifiques sont inclus dans les articles de référence de ces fonctionnalités.  
  
## <a name="before-installing-ssnoversion"></a>Avant d’effectuer l’installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Suivez ces recommandations lors de la définition de l'environnement du serveur :  
  
-   [Renforcez la sécurité physique.](#physical_security)  
  
-   [Utilisez des pare-feu.](#firewalls)  
  
-   [Isolez les services.](#isolated_services)  
  
-   [Configurez un système de fichiers sécurisé](#sa_with_least_privileges)  
  
-   [Désactivation des protocoles NetBIOS et SMB (Server Message Block)](#disabled_protocols)  
  
-   [Installation de SQL Server sur un contrôleur de domaine](../../sql-server/install/security-considerations-for-a-sql-server-installation.md#Install_DC)  
  
###  <a name="enhance-physical-security"></a><a name="physical_security"></a> Enhance Physical Security  
 L'isolement au niveau physique et logique est à la base de la sécurité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour améliorer la sécurité de l'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au niveau physique, procédez comme suit :  
  
-   Placez le serveur dans une pièce à laquelle seules les personnes autorisées peuvent accéder.  
  
-   Placez les ordinateurs qui hébergent une base de données dans un lieu protégé physiquement, idéalement dans une salle fermée à clé, équipée de systèmes pour la détection et la lutte contre les inondations et les incendies.  
  
-   Installez les bases de données dans une zone sécurisée de l'intranet d'entreprise, sans connecter vos serveurs SQL Server directement à Internet.  
  
-   Sauvegardez régulièrement toutes les données et conservez les copies de sauvegarde dans un lieu sécurisé situé en dehors de l'entreprise.  
  
###  <a name="use-firewalls"></a><a name="firewalls"></a> Use Firewalls  
 Les pare-feu sont importants afin d'aider à sécuriser l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les pare-feu seront d'autant plus efficaces que vous suivrez ces directives :  
  
-   Placez un pare-feu entre le serveur et Internet. Activez votre pare-feu. Si votre pare-feu est désactivé, activez-le. Si votre pare-feu est activé, ne le désactivez pas.  
  
-   Divisez le réseau en zones de sécurité séparées par des pare-feu. Bloquez tout le trafic, puis sélectionnez uniquement ce qui est obligatoire.  
  
-   Dans un environnement à plusieurs niveaux, utilisez plusieurs pare-feu pour créer des sous-réseaux filtrés.  
  
-   Lorsque vous installez le serveur à l'intérieur d'un domaine Windows, configurez des pare-feu internes pour autoriser l'authentification Windows.  
  
-   Si votre application utilise des transactions distribuées, vous devrez éventuellement configurer le pare-feu pour autoriser le trafic MS DTC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator) entre des instances MS DTC distinctes. Vous devrez configurer également le pare-feu afin de permettre au trafic de s'écouler entre les instances MS DTC et les gestionnaires des ressources tels que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Pour plus d’informations sur les paramètres par défaut du Pare-feu Windows et pour obtenir une description des ports TCP qui affectent le [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]et [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], consultez [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
###  <a name="isolate-services"></a><a name="isolated_services"></a> Isolate Services  
 Le fait d'isoler les services réduit le risque qu'un service compromis soit utilisé pour en compromettre d'autres. Pour isoler des services, tenez compte des consignes suivantes :  
  
-   Exécutez chaque service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sous des comptes Windows distincts. Si possible, utilisez des comptes d'utilisateurs locaux ou Windows distincts et à faibles droits pour chaque service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
###  <a name="configure-a-secure-file-system"></a><a name="sa_with_least_privileges"></a> Configure a Secure File System  
 L'utilisation du système de fichiers correct accroît la sécurité. Pour les installations [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez effectuer les tâches suivantes :  
  
-   Utiliser le système de fichiers NTFS. NTFS est le système de fichiers par défaut pour les installations de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] car il est plus stable et récupérable que les systèmes de fichiers FAT. NTFS active également des options de sécurité telles que les listes de contrôle d'accès de fichiers et de répertoires et le chiffrement de fichiers EFS (Encrypting File System). Pendant l'installation, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit les listes de contrôle d'accès appropriées sur les clés de Registre et les fichiers s'il détecte NTFS. Ces autorisations ne doivent pas être modifiées. Il se peut que les versions futures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prennent pas en charge l'installation sur des ordinateurs dotés de systèmes de fichiers FAT.  
  
    > [!NOTE]  
    >  Si vous utilisez le chiffrement EFS, les fichiers de base de données seront chiffrés sous l'identité du compte qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Seul ce compte sera en mesure de déchiffrer les fichiers. Si vous devez modifier le compte qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez d'abord déchiffrer les fichiers sous l'ancien compte, puis les rechiffrer sous le nouveau compte.  
  
-   Utilisez la technologie RAID pour les fichiers de données critiques.  
  
###  <a name="disable-netbios-and-server-message-block"></a><a name="disabled_protocols"></a> Disable NetBIOS and Server Message Block  
 Les protocoles non nécessaires doivent tous être désactivés sur les serveurs du réseau de périmètre, y compris les protocoles NetBIOS et SMB.  
  
 Le protocole NetBIOS utilise les ports suivants :  
  
-   UDP/137 (service de noms NetBIOS)  
  
-   UDP/138 (service de datagrammes NetBIOS)  
  
-   TCP/139 (service de sessions NetBIOS)  
  
 Le protocole SMB utilise les ports suivants :  
  
-   TCP/139  
  
-   TCP/445  
  
 Les serveurs Web et les serveurs DNS (Domain Name System) ne nécessitent pas l'utilisation du protocole NetBIOS ou SMB. Désactivez ces protocoles sur ces serveurs afin de réduire le risque lié à l'énumération utilisateur.  
  
###  <a name="installing-ssnoversion-on-a-domain-controller"></a><a name="Install_DC"></a> Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un contrôleur de domaine  
 Pour des raisons de sécurité, nous recommandons de ne pas installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un contrôleur de domaine. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne bloquera pas l'installation sur un ordinateur qui est contrôleur de domaine, mais les limitations suivantes s'appliquent :  
  
-   Vous ne pouvez pas exécuter les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un contrôleur de domaine sous un compte de service local.  
  
-   Après avoir installé sur un ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous ne pouvez pas modifier l'ordinateur d'un membre de domaine en un contrôleur de domaine. Vous devez désinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant de modifier l'ordinateur hôte en un contrôleur de domaine.  
  
-   Après avoir installé sur un ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous ne pouvez pas modifier l'ordinateur d'un contrôleur de domaine en un membre de domaine. Vous devez désinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant de modifier l'ordinateur hôte en un membre de domaine.  
  
-   Les instances de cluster de basculement[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sont pas prises en charge quand les nœuds du cluster sont des contrôleurs de domaine.  
  
-   Le programme d'installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas créer de groupes de sécurité ni configurer de comptes de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un contrôleur de domaine en lecture seule. Dans ce scénario, le programme d'installation échoue.  
  
## <a name="during-or-after-installation-of-ssnoversion"></a>Pendant ou après l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Après l'installation, vous pouvez renforcer la sécurité de l'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en suivant les recommandations suivantes pour les comptes et les modes d'authentification :  
  
 **Comptes de service**  
  
-   Exécutez les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant les autorisations les plus basses possibles.  
  
-   Associez les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à des comptes d'utilisateurs locaux Windows à faibles privilèges ou à des comptes d'utilisateurs de domaine.  
  
-   Pour plus d’informations, consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
 **Mode d'authentification**  
  
-   Exigez l'authentification Windows pour les connexions à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Utilisez l'authentification Kerberos. Pour plus d’informations, consultez [Inscrire un nom de principal du service pour les connexions Kerberos](../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).  
  
 **Mots de passe forts**  
  
-   Affectez toujours un mot de passe fort au compte **sa** .  
  
-   Toujours activez la vérification de la stratégie de mot de passe pour vérifier la robustesse et l'expiration du mot de passe.  
  
-   Utilisez toujours des mots de passe forts pour toutes les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Lors de l'installation de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] une connexion est ajoutée pour le groupe BUILTIN\Users. Cela permet à tous les utilisateurs authentifiés sur l'ordinateur d'accéder à l'instance de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] en tant que membres du rôle public. La connexion BUILTIN\Users peut être supprimée sans risque pour restreindre l'accès au [!INCLUDE[ssDE](../../includes/ssde-md.md)] aux utilisateurs de l'ordinateur qui disposent de connexions ou qui sont membres d'autres groupes Windows avec des connexions.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Protocoles réseau et bibliothèques réseau](../../sql-server/install/network-protocols-and-network-libraries.md)   
 [Inscrire un nom de principal du service pour les connexions Kerberos](../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)  
  
  
