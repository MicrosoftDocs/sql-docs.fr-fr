---
title: Créer une nouvelle instance de cluster de basculement
description: Cet article explique comment utiliser le programme d’installation pour installer ou mettre à niveau une instance de cluster de basculement Always On SQL Server ou ajouter un nœud à une instance de cluster de basculement existante.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- adding nodes
- failover clustering [SQL Server], creating clusters
- nodes [Faillover Clustering], removing
- nodes [Faillover Clustering], adding
- clusters [SQL Server], creating
- removing nodes
ms.assetid: 30e06a7d-75e9-44e2-bca3-b3b0c4a33f61
author: cawrites
ms.author: chadam
ms.openlocfilehash: c987fdf269db9787392caa5f228f97155cf673a8
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597147"
---
# <a name="create-a-new-always-on-failover-cluster-instance-setup"></a>Créer une nouvelle instance de cluster de basculement Always On (Installation)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Pour installer ou mettre à niveau une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous devez exécuter le programme d’installation sur chaque nœud du cluster de basculement Windows Server sous-jacent. Pour ajouter un nœud à une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existante, vous devez exécuter la Configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur le nœud destiné à être ajouté à l'instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. N'exécutez pas le programme d'installation sur le nœud actif pour gérer les autres nœuds.  
  
 Selon la façon dont les nœuds sont mis en cluster, l’instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est configurée des façons suivantes :  
  
-   Noeuds se trouvant sur le même sous-réseau ou ensemble de sous-réseaux - La dépendance de ressource d'adresse IP est définie sur AND pour ces types de configurations.  
  
-   Noeuds se trouvant sur des sous-réseaux différents : la dépendance de ressource d'adresse IP est définie sur OR et cette configuration est appelée une configuration d’instance de cluster de basculement de sous-réseaux multiples [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Clustering de sous-réseaux multiples SQL Server &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md).  
  
 Les options suivantes sont disponibles pour l'installation d'un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
 **Option 1 : Installation Intégration avec ajout de nœud**   
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] L’installation intégrée de cluster de basculement comprend les étapes suivantes :  
  
-   Créez et configurez une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à un seul nœud. Une fois la configuration du nœud réussie, vous disposez d'une instance de cluster de basculement entièrement fonctionnelle. Celle-ci n'offre pas une haute disponibilité pour l'instant, car il n'y a qu'un seul nœud dans l’instance de cluster de basculement.  
  
-   Sur chaque nœud à ajouter à l’instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , exécutez la Configuration en utilisant la fonctionnalité d'ajout de nœud pour ajouter ce nœud.  
  
    -   Si le nœud que vous ajoutez a des sous-réseaux supplémentaires ou différents, le programme d'installation vous permet de spécifier des adresses IP supplémentaires. Si le nœud que vous ajoutez se trouve sur un sous-réseau différent, vous devez également confirmer la modification de dépendance de ressource d'adresse IP sur OR. Pour plus d’informations sur les différents scénarios possibles pendant les opérations d’ajout de nœuds, consultez [Ajouter ou supprimer des nœuds dans une instance de cluster de basculement Always On &#40;Configuration&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
 **Option n°2 : Installation avancée/entreprise**  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] L’installation avancée/entreprise de cluster de basculement comprend les étapes suivantes :  
  
-   Sur chaque nœud potentiellement propriétaire du nouveau cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que vous créez, suivez les étapes de préparation de l’installation du cluster de basculement dans la [section Préparer](#prepare). Après avoir effectué la préparation du cluster de basculement sur un nœud, le programme d'installation crée le fichier Configuration.ini qui répertorie tous les paramètres que vous spécifiez. Sur les autres nœuds à préparer, au lieu de suivre ces étapes, vous pouvez fournir le fichier Configuration.ini généré automatiquement à partir du premier nœud comme entrée pour la ligne de commande de la Configuration. Pour plus d’informations, consultez [Installer SQL Server 2016 à l’aide d’un fichier de configuration](../../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md). Cette étape prépare les nœuds pour le clustering ; toutefois, aucune instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'est opérationnelle à la fin de cette étape.  
  
-   Une fois les nœuds préparés pour le clustering, exécutez l'installation sur l'un de ces nœuds. Cette étape permet de configurer et de finaliser l'instance de cluster de basculement. À la fin de cette étape, vous disposerez d'une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] opérationnelle et tous les nœuds préparés précédemment pour cette instance seront des propriétaires potentiels de l’instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nouvellement créée.  
  
     Si vous créez une instance de cluster de basculement qui s’étend sur plusieurs sous-réseaux, la Configuration détecte l’union de tous les sous-réseaux sur tous les nœuds ayant une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] préparée. Vous serez en mesure de spécifier plusieurs adresses IP pour les sous-réseaux. Chaque nœud préparé doit être le propriétaire possible d'au moins une adresse IP.  
  
     Si chacune des adresses IP spécifiées pour les sous-réseaux est prise en charge sur tous les nœuds préparés, la dépendance est définie sur AND.  
  
     Si aucune des adresses IP spécifiées pour les sous-réseaux n'est prise en charge sur tous les nœuds préparés, la dépendance est définie sur OR. Pour plus d’informations, consultez [Clustering de sous-réseaux multiples SQL Server &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md).  
  
    > [!NOTE]  
    >  Le cluster de basculement complet requiert l'existence du cluster de basculement Windows sous-jacent. Si le cluster de basculement Windows Server n'existe pas, la Configuration signale une erreur et s'arrête.  
  
 Pour plus d’informations sur l’ajout ou la suppression de nœuds dans une instance de cluster de basculement existante, consultez [Ajouter ou supprimer des nœuds dans une instance de cluster de basculement Always On &#40;Configuration&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
 Pour plus d’informations sur une installation distante, consultez [Mises à niveau de la version et de l’édition prises en charge](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
 Pour plus d’informations sur l’installation de[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] dans un WSFC, consultez [Comment mettre en cluster SQL Server Analysis Services](/previous-versions/sql/sql-server-2012/dn736073(v=msdn.10)).  
  
## <a name="prerequisites"></a>Prérequis  
 Avant de commencer, consultez les rubriques suivantes dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   [Planification d'une installation SQL Server](../../../sql-server/install/planning-a-sql-server-installation.md)  
  
-   [Avant l'installation du clustering de basculement](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)  
  
-   [Considérations sur la sécurité pour une installation SQL Server](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
  
-   [Clustering de sous-réseaux multiples SQL Server &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md)  
  
> [!NOTE]  
>  Souvenez-vous de l'emplacement du lecteur partagé dans l'Administrateur de cluster avant d'exécuter le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Vous aurez besoin de ces informations pour créer une nouvelle instance de cluster de basculement.  
  
### <a name="to-install-a-new-ssnoversion-failover-cluster-instance-using-integrated-install-with-add-node"></a>Pour installer une nouvelle instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en utilisant l'installation intégrée avec ajout de nœud.  
  
1.  Insérez le support d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et, dans le dossier racine, double-cliquez sur Setup.exe. Pour effectuer l'installation à partir d'un partage réseau, accédez au dossier racine sur le partage, puis double-cliquez sur Setup.exe. Pour plus d’informations sur l’installation des composants requis, consultez [Avant l’installation du clustering de basculement](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
2.  L'Assistant d'installation démarre le Centre d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour créer une installation de cluster de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], cliquez sur **Installation d’un nouveau cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** dans la page d’installation.  
  
3.  L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour continuer, [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**.  
  
4.  Pour continuer, cliquez sur **Suivant**.  
  
5.  Sur la page Fichiers de support du programme d'installation, cliquez sur **Installer** pour installer les fichiers de support du programme d'installation.  
  
6.  L'outil d'analyse de configuration système vérifie l'état système de votre ordinateur avant que le programme d'installation ne se poursuive. Lorsque la vérification est terminée, cliquez sur **Suivant** pour poursuivre. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**.  
  
7.  Dans la page Clé du produit, indiquez si vous installez une édition gratuite de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]ou si vous disposez d'une clé PID pour une version de production du produit. Pour plus d’informations, consultez [Éditions et composants de SQL Server 2016](../../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
8.  Dans la page Termes du contrat de licence, prenez connaissance du contrat de licence, puis activez la case à cocher indiquant que vous en acceptez les termes et conditions. Pour aider à améliorer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous pouvez également activer l'option d'utilisation des fonctionnalités et envoyer des rapports à [!INCLUDE[msCoName](../../../includes/msconame-md.md)]. Cliquez sur **Suivant** pour continuer. Pour mettre fin au programme d'installation, cliquez sur **Annuler**.  
  
9. Dans la page Sélection de fonctionnalités, sélectionnez les composants que vous voulez installer. Une description de chaque groupe de composants apparaît dans le volet droit après que vous avez sélectionné le nom de la fonctionnalité. Vous pouvez choisir n'importe quelle combinaison de cases à cocher, mais seuls le [!INCLUDE[ssDE](../../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode tabulaire et [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode multidimensionnel prennent en charge le clustering de basculement. Les autres composants sélectionnés s'exécuteront sous la forme d'une fonctionnalité autonome sans basculement sur le nœud actuel sur lequel vous exécutez le programme d'installation. Pour plus d’informations sur les modes [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] , consultez [Déterminer le mode serveur d’une instance Analysis Services](/analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance).  
  
     Les composants requis pour les fonctionnalités sélectionnées sont affichés dans le volet droit. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installe les composants requis qui n'ont pas déjà été installés lors de l'étape d'installation décrite plus loin dans cette procédure.  
  
     Vous pouvez spécifier un répertoire personnalisé pour les composants partagés en utilisant le champ situé au bas de cette page. Pour modifier le chemin d'installation des composants partagés, mettez à jour le nom du chemin d'accès dans le champ fourni en bas de la boîte de dialogue ou cliquez sur le bouton de sélection pour accéder à un répertoire d'installation. Le chemin d’installation par défaut est C:\Program Files\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] \\.  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend également en charge l’installation des bases de données système (MASTER, Model, MSDB et TempDB) et des bases de données utilisateur du [!INCLUDE[ssDE](../../../includes/ssde-md.md)] sur un partage de fichiers SMB (Server Message Block). Pour plus d’informations sur l’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec le partage de fichiers SMB comme stockage, consultez [Installer SQL Server avec le partage de fichiers SMB en tant qu’option de stockage](../../../database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option.md).  
  
     Le chemin d'accès spécifié pour les composants partagés doit être un chemin d'accès absolu. Le dossier ne doit pas être compressé ni chiffré. Les lecteurs mappés ne sont pas pris en charge. Si vous installez [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur un système d'exploitation 64 bits, vous verrez les options suivantes :  
  
    1.  Répertoire des fonctionnalités partagées  
  
    2.  Répertoire des fonctionnalités partagées (x86)  
  
     Le chemin d'accès spécifié pour chacune des options ci-dessus doit être différent.  
  
    > [!NOTE]  
    >  Lorsque vous sélectionnez la fonctionnalité Services [!INCLUDE[ssDE](../../../includes/ssde-md.md)] , la réplication et la recherche en texte intégral sont sélectionnées automatiquement. Data Quality Services (DQS) est également sélectionné lorsque vous sélectionnez la fonctionnalité des services du [!INCLUDE[ssDE](../../../includes/ssde-md.md)] . Si vous désélectionnez l'une de ces sous-fonctionnalités, la fonctionnalité Services [!INCLUDE[ssDE](../../../includes/ssde-md.md)] est également désélectionnée.  
  
10. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Le programme d’installation exécute un autre ensemble de règles qui sont basées sur les fonctionnalités sélectionnées pour valider votre configuration.  
  
11. Dans la page Configuration de l'instance, spécifiez s'il faut installer une instance par défaut ou une instance nommée. Pour plus d'informations, consultez [Instance Configuration](../../install/instance-configuration.md).  
  
     **[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Nom réseau** : indiquez un nom réseau pour la nouvelle instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il s'agit du nom qui est utilisé pour identifier votre instance de cluster de basculement sur le réseau.  
  
    > [!NOTE]  
    >  Il est connu comme le nom [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] virtuel des versions antérieures des instances de clusters de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
     **ID d’instance** : Par défaut, le nom de l’instance est utilisé comme ID d’instance. Il permet d'identifier les répertoires d'installation et les clés de Registre de votre instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Tel est le cas pour les instances par défaut et les instances nommées. Pour une instance par défaut, le nom de l'instance et l'ID d'instance sont MSSQLSERVER. Pour utiliser un ID d’instance non défini par défaut, cochez la case **ID d’instance** et entrez une valeur.  
  
    > [!NOTE]  
    >  Les instances autonomes classiques de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], qu’il s’agisse d’instances par défaut ou d’instances nommées, n’utilisent pas de valeur non définie par défaut pour la case à cocher **ID d’instance** .  
  
     **Répertoire racine de l’instance** : par défaut, le répertoire racine de l’instance est C:\Program Files\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]\\. Pour spécifier un répertoire racine non défini par défaut, utilisez le champ fourni ou cliquez sur le bouton de sélection afin de rechercher un dossier d'installation.  
  
     **Fonctionnalités et instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] détectées sur cet ordinateur** : la grille affiche les instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui se trouvent sur l’ordinateur où le programme d’installation s’exécute. Si une instance par défaut est déjà installée sur l'ordinateur, vous devez installer une instance nommée de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Cliquez sur **Suivant** pour continuer.  
  
12. Utilisez la page Groupe de ressources de cluster pour spécifier le nom ou rôle du groupe de ressources de cluster où les ressources du serveur virtuel [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] seront hébergées. Pour spécifier le nom du groupe de ressources de cluster [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vous avez deux options :  
  
    -   Utilisez la zone de liste déroulante pour spécifier un groupe existant à utiliser.  
  
    -   Tapez le nom d'un groupe à créer. Notez que le nom « Stockage disponible » n'est pas un nom de groupe valide.  
  
13. Dans la page Sélection du disque du cluster, sélectionnez la ressource disque de cluster partagée pour votre instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le disque de cluster est l'emplacement où les données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] seront placées. Plusieurs disques peuvent être spécifiés. La grille des disques partagés disponibles affiche une liste de disques disponibles, indique si chacun d'eux est qualifié en tant que disque partagé et fournit une description de chaque ressource de disque. Cliquez sur **Suivant** pour continuer.  
  
    > [!NOTE]  
    >  Le premier lecteur est utilisé comme lecteur par défaut pour toutes les bases de données, mais peut être changé sur les pages de configuration [!INCLUDE[ssDE](../../../includes/ssde-md.md)] ou [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] .  
    >   
    >  Dans la page Sélection du disque du cluster, vous pouvez éventuellement ignorer la sélection d'un disque partagé si vous souhaitez utiliser le partage de fichiers SMB comme dossier de données.  
  
14. Dans la page Configuration du réseau de cluster, spécifiez les ressources réseau de votre instance de cluster de basculement :  
  
    -   **Paramètres réseau** : spécifiez le type IP et l’adresse IP de votre instance de cluster de basculement.  
  
     Cliquez sur **Suivant** pour continuer.  
  
15. Utilisez cette page pour spécifier la stratégie de sécurité du cluster.  
  
    -   [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] et versions ultérieures. Les SID de service (ID de sécurité du serveur) sont les paramètres par défaut recommandés. Il n'est pas possible de remplacer cela par des groupes de sécurité. Pour plus d’informations sur les fonctionnalités des SID de service sur [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)], consultez [Configurer les comptes de service Windows et les autorisations](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md). Cela a été testé dans les configurations autonome et de cluster sur [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)].  
  
    -   Dans [!INCLUDE[winxpsvr](../../../includes/winxpsvr-md.md)], spécifiez des groupes de domaines pour les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Toutes les autorisations de ressources sont gérées par des groupes au niveau du domaine qui incluent des comptes de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en tant que membres de groupe.  
  
     Cliquez sur **Suivant** pour continuer.  
  
16. Le flux de travail du reste de cette rubrique dépend des fonctionnalités que vous avez spécifiées pour votre installation. Il est possible que les pages ne soient pas toutes visibles, en fonction de vos sélections ([!INCLUDE[ssDE](../../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]et [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]).  
  
17. Dans la page Configuration du serveur - Comptes de service, spécifiez les comptes de connexion des services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les services réels configurés dans cette page dépendent des fonctionnalités que vous avez choisi d'installer.  
  
     Vous pouvez attribuer le même compte de connexion à tous les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou configurer chaque compte de service individuellement. Le type de démarrage est défini sur manuel pour tous les services prenant en charge les clusters, notamment la recherche en texte intégral et l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , et ne peut pas être modifié pendant l'installation. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] vous recommande de configurer les comptes de service individuellement afin de fournir des privilèges moindres pour chaque service, sachant que les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] disposent des autorisations minimales requises pour effectuer leurs tâches. Pour plus d’informations, consultez [Configuration du serveur - Comptes de service](../../../database-engine/install-windows/install-sql-server.md) et [Configurer les comptes de service Windows et les autorisations](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Pour spécifier le même compte d'ouverture de session pour tous les comptes de service dans cette instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], fournissez les informations d'identification dans les champs en bas de page.  
  
     **Note de sécurité** [!INCLUDE[ssNoteStrongPass](../../../includes/ssnotestrongpass-md.md)]  
  
     Lorsque vous avez terminé de spécifier les informations de connexion pour les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cliquez sur **Suivant**.  
  
18. Utilisez l’onglet **Configuration du serveur — Classement** pour spécifier les classements non définis par défaut pour le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
19. Utilisez la page Configuration du [!INCLUDE[ssDE](../../../includes/ssde-md.md)] - Attribution de privilèges d’accès aux comptes pour spécifier les éléments suivants :  
  
    -   Mode de sécurité - sélectionnez l'authentification Windows ou le mode d'authentification mixte pour votre instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si vous sélectionnez Mode d'authentification mixte, vous devez fournir un mot de passe fort pour le compte administrateur système [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] intégré.  
  
         Lorsque la connexion d'un périphérique à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]est établie, le mécanisme de sécurité est le même pour l'authentification Windows et le mode mixte.  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Administrateurs : vous devez spécifier au moins un administrateur système pour l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour ajouter le compte sous lequel le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute, cliquez sur **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, cliquez sur **Ajouter** ou sur **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposeront des privilèges d'administrateur pour l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
     Lorsque vous avez terminé de modifier la liste, [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Une fois la liste complète, cliquez sur **Suivant**.  
  
20. Utilisez la page Configuration du [!INCLUDE[ssDE](../../../includes/ssde-md.md)] - Répertoires de données pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, cliquez sur **Suivant**.  
  
    > [!IMPORTANT]  
    >  Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques à cette instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les répertoires de données doivent se trouver sur le disque de cluster partagé pour l’instance de cluster de basculement.  
  
    > [!NOTE]  
    >  Pour spécifier un serveur de fichiers SMB comme répertoire de données, définissez le **répertoire de données racine par défaut** sur le partage de fichiers au format \\\NomServeur\NomPartage\\...  
   
21. Utilisez la page Configuration du [!INCLUDE[ssDE](../../../includes/ssde-md.md)] - FILESTREAM pour activer FILESTREAM pour votre instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Cliquez sur **Suivant** pour continuer.  
  
22. Utilisez la page Configuration d'[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] - Attribution de privilèges d'accès aux comptes pour spécifier les utilisateurs ou comptes qui ont les autorisations d'administrateur pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Vous devez spécifier au moins un administrateur système pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Pour ajouter le compte sous lequel le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute, cliquez sur **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, cliquez sur **Ajouter** ou **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposeront des privilèges d'administrateur pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].
  
     Lorsque vous avez terminé de modifier la liste, [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Une fois la liste complète, cliquez sur **Suivant**.  
  
23. Utilisez la page Configuration du [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] - Répertoires de données pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, cliquez sur **Suivant**.  
  
    > [!IMPORTANT]  
    >  Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques à cette instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les répertoires de données doivent se trouver sur le disque de cluster partagé pour l’instance de cluster de basculement.  
   
24. Utilisez la page Configuration de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] pour spécifier le type d'installation de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] à créer. Pour les installations d'une instance de cluster de basculement, cette option est définie sur l’installation [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] non configurée. Vous devez configurer les services [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] une fois l'installation terminée.  
  
 
25. L'Outil d'analyse de configuration système exécute un autre ensemble de règles pour valider la configuration avec les fonctionnalités [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que vous avez spécifiées.  
  
26. La page Prêt pour l'installation affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Pour continuer, cliquez sur **Installer**. installe d'abord les composants requis pour les fonctionnalités sélectionnées, puis installe les fonctionnalités.  
  
27. Au cours de l'installation, la page Progression de l'installation fournit des informations d'état de sorte que vous puissiez contrôler la progression de l'installation au fil de l'exécution du programme d'installation.  
  
28. Après l'installation, la page **Terminé** fournit un lien vers le fichier journal résumé de l'installation et d'autres remarques importantes. Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**.  
  
29. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d’informations sur les fichiers journaux d’installation, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
30. Pour ajouter des nœuds au cluster de basculement à un seul nœud que vous venez de créer, exécutez le programme d'installation sur chacun des nœuds supplémentaires et suivez les étapes pour l'opération AddNode. Pour plus d’informations, consultez [Ajouter ou supprimer des nœuds dans une instance de cluster de basculement ](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md) &#40;Configuration&#41; Always On.  
  
    > [!NOTE]  
    >  Si vous ajoutez plusieurs nœuds, vous pouvez utiliser le fichier de configuration pour déployer les installations. Pour plus d’informations, consultez [Installer SQL Server 2016 à l’aide d’un fichier de configuration](../../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md).  
    >   
    >  L'édition [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que vous installez doit être la même sur tous les nœuds d'une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Lorsque vous ajoutez un nouveau nœud à une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existante, vous devez être certain de spécifier que l'édition est identique à celle de l’instance de cluster de basculement existante.  
  
##  <a name="prepare"></a><a name="prepare"></a> Préparation  
  
#### <a name="advancedenterprise-failover-cluster-instance-install-step-1-prepare"></a>Étape 1 de l’installation avancée/entreprise sur une instance de cluster de basculement : Préparation  
  
1.  Insérez le support d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et, dans le dossier racine, double-cliquez sur Setup.exe. Pour effectuer l'installation à partir d'un partage réseau, accédez au dossier racine sur le partage, puis double-cliquez sur Setup.exe. Pour plus d’informations sur l’installation des composants requis, consultez [Avant l’installation du clustering de basculement](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md). Il se peut que vous deviez installer les composants requis si ceux-ci ne sont pas déjà présents sur l'ordinateur.  
  
2.  Windows Installer 4.5 est requis et peut être installé par l'Assistant Installation. Si vous êtes invité à redémarrer votre ordinateur, redémarrez-le, puis redémarrez le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
3.  Lorsque les composants requis sont installés, l'Assistant Installation démarre le Centre d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour préparer le nœud pour le clustering, passez à la page **Avancé** et cliquez sur **Préparation de cluster avancée**.  
  
4.  L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour continuer, [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**.  
  
5.  Dans la page Fichiers de support du programme d’installation, cliquez sur **Installer** pour installer les fichiers de support du programme d’installation.  
  
6.  L'outil d'analyse de configuration système vérifie l'état système de votre ordinateur avant que le programme d'installation ne se poursuive. Lorsque la vérification est terminée, cliquez sur **Suivant** pour poursuivre. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**.  
  
7.  Dans la page Sélection de la langue, vous pouvez spécifier la langue de votre instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si vous effectuez l'installation sur un système d'exploitation localisé et que le support d'installation inclut des modules linguistiques pour l'anglais et pour la langue qui correspond au système d'exploitation. Pour plus d’informations sur la prise en charge des langues multiples et sur les considérations relatives à l’installation, consultez [Versions linguistiques locales dans SQL Server](../../../sql-server/install/local-language-versions-in-sql-server.md).  
  
     Pour continuer, cliquez sur **Suivant**.  
  
8.  Dans la page Clé du produit, cliquez pour indiquer si vous installez une édition gratuite de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]ou si vous disposez d'une clé PID pour une version de production du produit. Pour plus d’informations, consultez [Éditions et composants de SQL Server 2016](../../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
    > [!NOTE]  
    >  Vous devez spécifier la même clé de produit sur tous les nœuds que vous préparez pour la même instance de cluster de basculement.  
  
9. Dans la page Termes du contrat de licence, prenez connaissance du contrat de licence, puis activez la case à cocher indiquant que vous en acceptez les termes et conditions. Pour aider à améliorer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous pouvez également activer l'option d'utilisation des fonctionnalités et envoyer des rapports à [!INCLUDE[msCoName](../../../includes/msconame-md.md)]. Cliquez sur **Suivant** pour continuer. Pour mettre fin au programme d'installation, cliquez sur **Annuler**.  
  
10. Dans la page Sélection de fonctionnalités, sélectionnez les composants que vous voulez installer. Une description de chaque groupe de composants apparaît dans le volet droit après que vous avez sélectionné le nom de la fonctionnalité. Vous pouvez choisir n'importe quelle combinaison de cases à cocher, mais seuls le [!INCLUDE[ssDE](../../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode tabulaire et [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en mode multidimensionnel prennent en charge le clustering de basculement. Les autres composants sélectionnés s'exécuteront sous la forme d'une fonctionnalité autonome sans basculement sur le nœud actuel sur lequel vous exécutez le programme d'installation. Pour plus d’informations sur les modes [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] , consultez [Déterminer le mode serveur d’une instance Analysis Services](/analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance).  
  
     Les composants requis pour les fonctionnalités sélectionnées sont affichés dans le volet droit. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installe les composants requis qui n'ont pas déjà été installés lors de l'étape d'installation décrite plus loin dans cette procédure.  
  
     Vous pouvez spécifier un répertoire personnalisé pour les composants partagés en utilisant le champ situé au bas de cette page. Pour modifier le chemin d'installation des composants partagés, mettez à jour le nom du chemin d'accès dans le champ fourni en bas de la boîte de dialogue ou cliquez sur le bouton de sélection pour accéder à un répertoire d'installation. Le chemin d’installation par défaut est C:\Program Files\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]\\.  
  
    > [!NOTE]  
    >  Lorsque vous sélectionnez la fonctionnalité Services [!INCLUDE[ssDE](../../../includes/ssde-md.md)] , la réplication et la recherche en texte intégral sont sélectionnées automatiquement. Si vous désélectionnez l'une de ces sous-fonctionnalités, la fonctionnalité Services [!INCLUDE[ssDE](../../../includes/ssde-md.md)] est également désélectionnée.  
  
11. Dans la page Configuration de l'instance, spécifiez s'il faut installer une instance par défaut ou une instance nommée.
  
     **ID d’instance** : Par défaut, le nom de l’instance est utilisé comme ID d’instance. Il permet d'identifier les répertoires d'installation et les clés de Registre de votre instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Tel est le cas pour les instances par défaut et les instances nommées. Pour une instance par défaut, le nom de l'instance et l'ID d'instance sont MSSQLSERVER. Pour utiliser un ID d’instance non défini par défaut, sélectionnez la zone de texte **ID d’instance** et entrez une valeur.  
  
    > [!NOTE]  
    >  Les instances autonomes classiques de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], qu’il s’agisse d’instances par défaut ou d’instances nommées, n’utilisent pas de valeur non définie par défaut pour la zone de texte **ID d’instance** .  
  
    > [!IMPORTANT]  
    >  Utilisez le même ID d'instance pour tous les nœuds préparés pour l’instance de cluster de basculement  
  
     **Répertoire racine de l’instance** : par défaut, le répertoire racine de l’instance est C:\Program Files\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]\\. Pour spécifier un répertoire racine non défini par défaut, utilisez le champ fourni ou cliquez sur le bouton de sélection afin de rechercher un dossier d'installation.  
  
     **Instances installées** : la grille affiche les instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui se trouvent sur l’ordinateur où le programme d’installation s’exécute. Si une instance par défaut est déjà installée sur l'ordinateur, vous devez installer une instance nommée de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Cliquez sur **Suivant** pour continuer.  
  
12. La page Espace disque nécessaire calcule l'espace disque nécessaire pour les fonctionnalités que vous spécifiez et compare cet espace à l'espace disque disponible sur l'ordinateur où le programme d'installation s'exécute.  
  
13. Utilisez cette page pour spécifier la stratégie de sécurité du cluster.  
  
    -   [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] et versions ultérieures. Les SID de service (ID de sécurité du serveur) sont les paramètres par défaut recommandés. Il n'est pas possible de remplacer cela par des groupes de sécurité. Pour plus d’informations sur les fonctionnalités des SID de service sur [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)], consultez [Configurer les comptes de service Windows et les autorisations](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md). Cela a été testé dans les configurations autonome et de cluster sur [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)].  
  
    -   Dans [!INCLUDE[winxpsvr](../../../includes/winxpsvr-md.md)], spécifiez des groupes de domaines pour les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Toutes les autorisations de ressources sont gérées par des groupes au niveau du domaine qui incluent des comptes de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en tant que membres de groupe.  
  
     Cliquez sur **Suivant** pour continuer.  
  
14. Le flux de travail du reste de cette rubrique dépend des fonctionnalités que vous avez spécifiées pour votre installation. Il est possible que les pages ne soient pas toutes visibles, en fonction de vos sélections.  
  
15. Dans la page Configuration du serveur - Comptes de service, spécifiez les comptes de connexion des services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les services réels configurés dans cette page dépendent des fonctionnalités que vous avez choisi d'installer.  
  
     Vous pouvez attribuer le même compte de connexion à tous les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou configurer chaque compte de service individuellement. Le type de démarrage est défini sur manuel pour tous les services prenant en charge les clusters, notamment la recherche en texte intégral et l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , et ne peut pas être modifié pendant l'installation. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] vous recommande de configurer les comptes de service individuellement afin de fournir des privilèges moindres pour chaque service, sachant que les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] disposent des autorisations minimales requises pour effectuer leurs tâches. Pour plus d’informations, consultez [Configurer les comptes de service Windows et les autorisations](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Pour spécifier le même compte d'ouverture de session pour tous les comptes de service dans cette instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], fournissez les informations d'identification dans les champs en bas de page.  
  
     **Note de sécurité** [!INCLUDE[ssNoteStrongPass](../../../includes/ssnotestrongpass-md.md)]  
  
     Lorsque vous avez terminé de spécifier les informations de connexion pour les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cliquez sur **Suivant**.  
  
16. Utilisez l’onglet **Configuration du serveur — Classement** pour spécifier les classements non définis par défaut pour le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
17. Utilisez la page **Configuration du serveur - Filestream** pour activer FILESTREAM pour votre instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  Cliquez sur **Suivant** pour continuer.  
  
18. Utilisez la page Configuration de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] pour spécifier le type d'installation de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] à créer. Pour les installations d'une instance de cluster de basculement, cette option est définie sur l’installation [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] non configurée. Vous devez configurer les services [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] une fois l'installation terminée.  
   
19. Dans la page Création de rapports d'erreurs, spécifiez les informations que vous souhaitez envoyer à [!INCLUDE[msCoName](../../../includes/msconame-md.md)] afin d'aider à améliorer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les options de création de rapports d'erreurs sont activées par défaut.  
  
20. L'Outil d'analyse de configuration système exécute un autre ensemble de règles pour valider la configuration avec les fonctionnalités [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que vous avez spécifiées.  
  
21. La page Prêt pour l'installation affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Pour continuer, cliquez sur **Installer**. installe d'abord les composants requis pour les fonctionnalités sélectionnées, puis installe les fonctionnalités.  
  
     Au cours de l'installation, la page Progression de l'installation fournit des informations d'état de sorte que vous puissiez contrôler la progression de l'installation au fil de l'exécution du programme d'installation. Après l'installation, la page **Terminé** fournit un lien vers le fichier journal résumé de l'installation et d'autres remarques importantes.  
  
22. Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**.  
  
23. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d’informations sur les fichiers journaux d’installation, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
24. Répétez les étapes précédentes pour préparer les autres nœuds pour l’instance de cluster de basculement. Vous pouvez également utiliser le fichier de configuration généré automatiquement pour exécuter la préparation sur les autres nœuds. Pour plus d’informations, consultez [Installer SQL Server 2016 à l’aide d’un fichier de configuration](../../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md).  
  
## <a name="complete"></a>Terminé  
  
#### <a name="advancedenterprise-failover-cluster-instance-install-step-2-complete"></a>Étape 2 de l’installation avancée/entreprise sur une instance de cluster de basculement : Terminé  
  
1.  Une fois tous les nœuds préparés de la façon décrite dans [l’étape de préparation](#prepare), lancez le programme d’installation sur l’un des nœuds préparés, de préférence sur le nœud propriétaire du disque partagé. Dans la page **Avancé** du Centre d’installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cliquez sur **Création de cluster avancée**.  
  
2.  L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour continuer, [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**.  
  
3.  Sur la page Fichiers de support du programme d'installation, cliquez sur **Installer** pour installer les fichiers de support du programme d'installation.  
  
4.  L'outil d'analyse de configuration système vérifie l'état système de votre ordinateur avant que le programme d'installation ne se poursuive. Lorsque la vérification est terminée, cliquez sur **Suivant** pour poursuivre. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**.  
  
5.  Dans la page Sélection de la langue, vous pouvez spécifier la langue de votre instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si vous effectuez l'installation sur un système d'exploitation localisé et que le support d'installation inclut des modules linguistiques pour l'anglais et pour la langue qui correspond au système d'exploitation. Pour plus d’informations sur la prise en charge des langues multiples et sur les considérations relatives à l’installation, consultez [Versions linguistiques locales dans SQL Server](../../../sql-server/install/local-language-versions-in-sql-server.md).  
  
     Pour continuer, cliquez sur **Suivant**.  
  
6.  Utilisez la page Configuration du nœud de clusters pour sélectionner le nom de l'instance préparée pour le clustering et spécifier le nom réseau pour la nouvelle instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Il s'agit du nom qui est utilisé pour identifier votre instance de cluster de basculement sur le réseau.  
  
    > [!NOTE]  
    >  Il est connu comme le nom [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] virtuel des versions antérieures des instances de clusters de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
7.  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Le programme d’installation exécute un autre ensemble de règles qui sont basées sur les fonctionnalités sélectionnées pour valider votre configuration.  
  
8.  Utilisez la page Groupe de ressources de cluster pour spécifier le nom du groupe de ressources de cluster où les ressources du serveur virtuel [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] seront hébergées. Pour spécifier le nom du groupe de ressources de cluster [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , Deux options s'offrent à vous :  
  
    -   Utilisez la liste pour spécifier un groupe existant à utiliser.  
  
    -   Tapez le nom d'un groupe à créer. Notez que le nom « Stockage disponible » n'est pas un nom de groupe valide.  
  
9. Dans la page Sélection du disque du cluster, sélectionnez la ressource disque de cluster partagée pour votre instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le disque de cluster est l'emplacement où les données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] seront placées. Plusieurs disques peuvent être spécifiés. La grille des disques partagés disponibles affiche une liste de disques disponibles, indique si chacun d'eux est qualifié en tant que disque partagé et fournit une description de chaque ressource de disque. Cliquez sur **Suivant** pour continuer.  
  
    > [!NOTE]  
    >  Le premier lecteur est utilisé comme lecteur par défaut pour toutes les bases de données, mais peut être changé sur les pages de configuration [!INCLUDE[ssDE](../../../includes/ssde-md.md)] ou [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] .  
  
10. Dans la page Configuration du réseau de cluster, spécifiez les ressources réseau de votre instance de cluster de basculement :  
  
    -   **Paramètres réseau** : spécifiez le type IP et l’adresse IP pour tous les nœuds et sous-réseaux de votre instance de cluster de basculement. Vous pouvez spécifier plusieurs adresses IP pour une instance de cluster de basculement de sous-réseaux multiples, mais une seule adresse IP par sous-réseau est prise en charge. Chaque nœud préparé doit être le propriétaire d'au moins une adresse IP. Si vous avez plusieurs sous-réseaux dans votre instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous serez invité à définir la dépendance de ressource d'adresse IP sur OR.  
  
     Cliquez sur **Suivant** pour continuer.  
  
11. Le flux de travail du reste de cette rubrique dépend des fonctionnalités que vous avez spécifiées pour votre installation. Il est possible que les pages ne soient pas toutes visibles, en fonction de vos sélections.  
  
12. Utilisez la page Configuration du [!INCLUDE[ssDE](../../../includes/ssde-md.md)] - Attribution de privilèges d’accès aux comptes pour spécifier les éléments suivants :  
  
    -   Mode de sécurité - sélectionnez l'authentification Windows ou le mode d'authentification mixte pour votre instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si vous sélectionnez Mode d'authentification mixte, vous devez fournir un mot de passe fort pour le compte administrateur système [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] intégré.  
  
         Lorsque la connexion d'un périphérique à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]est établie, le mécanisme de sécurité est le même pour l'authentification Windows et le mode mixte.  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Administrateurs : vous devez spécifier au moins un administrateur système pour l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour ajouter le compte sous lequel le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute, cliquez sur **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, cliquez sur **Ajouter** ou sur **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposeront des privilèges d'administrateur pour l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
     Lorsque vous avez terminé de modifier la liste, [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Une fois la liste complète, cliquez sur **Suivant**.  
  
13. Utilisez la page Configuration du [!INCLUDE[ssDE](../../../includes/ssde-md.md)] - Répertoires de données pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, cliquez sur **Suivant**.  
  
    > [!IMPORTANT]  
    >  Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques à cette instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les répertoires de données doivent se trouver sur le disque de cluster partagé pour l’instance de cluster de basculement.  
  
  
14. Utilisez la page Configuration d'[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] - Attribution de privilèges d'accès aux comptes pour spécifier les utilisateurs ou comptes qui ont les autorisations d'administrateur pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Vous devez spécifier au moins un administrateur système pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Pour ajouter le compte sous lequel le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute, cliquez sur **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, cliquez sur **Ajouter** ou **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposeront des privilèges d'administrateur pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
     Lorsque vous avez terminé de modifier la liste, [!INCLUDE[clickOK](../../../includes/clickok-md.md)]. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Une fois la liste complète, cliquez sur **Suivant**.  
  
15. Utilisez la page Configuration du [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] - Répertoires de données pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, cliquez sur **Suivant**.  
  
    > [!IMPORTANT]  
    >  Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques à cette instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les répertoires de données doivent se trouver sur le disque de cluster partagé pour le cluster de basculement.  
  
  
16. L'Outil d'analyse de configuration système exécute un autre ensemble de règles pour valider la configuration avec les fonctionnalités [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que vous avez spécifiées.  
  
17. La page Prêt pour l'installation affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Pour continuer, cliquez sur **Installer**. installe d'abord les composants requis pour les fonctionnalités sélectionnées, puis installe les fonctionnalités.  
  
18. Au cours de l'installation, la page Progression de l'installation fournit des informations d'état de sorte que vous puissiez contrôler la progression de l'installation au fil de l'exécution du programme d'installation.  
  
19. Après l'installation, la page **Terminé** fournit un lien vers le fichier journal résumé de l'installation et d'autres remarques importantes. Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**. Avec cette procédure, tous les nœuds préparés pour la même instance de cluster de basculement font partie de l’instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] terminée.  
  
## <a name="next-steps"></a>Étapes suivantes  
 **Configurer votre nouvelle installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** - Pour réduire la surface d’exposition vulnérable aux attaques d’un système, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installe et active de manière sélective les services et fonctionnalités clés. Pour plus d'informations, consultez [Surface Area Configuration](../../../relational-databases/security/surface-area-configuration.md).  
  
 Pour plus d’informations sur l’emplacement des fichiers journaux, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Installer SQL Server 2016 à partir de l’invite de commandes](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)  
  
