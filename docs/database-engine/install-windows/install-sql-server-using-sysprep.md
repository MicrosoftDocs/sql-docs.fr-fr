---
title: Installer SQL Server à l’aide de SysPrep | Microsoft Docs
description: Cet article décrit la préparation et la finalisation des images à l’aide de SysPrep dans l’installation SQL Server.
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 11f4ed8a-aaa9-417b-bdd5-204f551c6bb6
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8bbb1d668efb9a8531d6a93b084a7315e99072ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348308"
---
# <a name="install-sql-server-with-sysprep"></a>Installer SQL Server à l’aide de SysPrep

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep sont accessibles via le Centre d'installation. La Page **Avancé** du **Centre d’installation** a deux options : **Préparation de l’image d’une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** et **Finalisation d’image d’une instance autonome préparée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Les sections [Préparer](#prepare) et [Finaliser](#complete) décrivent la procédure d'installation en détail. Pour plus d'informations, consultez [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md). 
  
Vous pouvez également préparer et finaliser une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de l'invite de commandes ou d'un fichier de configuration. Pour plus d'informations, consultez les pages suivantes :  
  
- [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)  
  
- [Installer SQL Server à l’aide d’un fichier de configuration](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md)  
  
## <a name="prerequisites"></a>Prérequis  
Avant d’installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez les articles dans [Planification d’une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md). 
  
Pour plus d’informations sur les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et sur les configurations matérielle et logicielle requises, consultez [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). 
    
##  <a name="ssnoversion-sysprep-cluster-support"></a><a name="sysprep"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Prise en charge des clusters par SysPrep  
 À compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], SysPrep prend en charge les instances cluster de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans les installations via la ligne de commande. Pour plus d'informations, consultez [Qu'est-ce que Sysprep ?](https://msdn.microsoft.com/library/cc721940\(v=WS.10\).aspx). 
  
### <a name="to-prepare-a-ssnoversion-failover-cluster-unattended"></a>Pour préparer un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (sans assistance)  
  
1. Préparez l’image (comme décrit dans [Considérations relatives à l’installation de SQL Server à l’aide de SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md)) et capturez l’image Windows au moyen de SysPrep Generalization. L'exemple suivant prépare l'image :  
  
    ```  
    Setup.exe /q /ACTION=PrepareImage l /FEATURES=SQLEngine /InstanceID =<MYINST> /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
     Exécutez ensuite Windows SysPrep Generalization. 
  
2. Déployez l'image en exécutant Windows SysPrep Specialize. 
  
3. Créez le cluster de basculement Windows. 
  
4. Exécutez setup.exe avec **/ACTION=PrepareFailoverCluster** sur tous les nœuds. Par exemple :  
  
    ```  
    setup.exe /q /ACTION=PrepareFailoverCluster /InstanceName=<InstanceName> /Features=SQLEngine  /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx"  /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
### <a name="complete-a-ssnoversion-failover-cluster-unattended"></a>Création d’un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (sans assistance)  
  
1. Exécutez setup.exe avec **/ACTION=CompleteFailoverCluster** sur le nœud propriétaire du groupe de stockage disponible :  
  
    ```  
    setup.exe /q /ACTION=CompleteFailoverCluster /InstanceName=<InstanceName>  /FAILOVERCLUSTERDISKS="<Cluster Disk Resource Name - for example, 'Disk S:'>:" /FAILOVERCLUSTERNETWORKNAME="<Insert FOI Network Name>" /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;Cluster Network;xxx.xxx.xxx.x" /FAILOVERCLUSTERGROUP="MSSQLSERVER" /INSTALLSQLDATADIR="<Drive>:\<Path>\MSSQLSERVER" /SQLCOLLATION="SQL_Latin1_General_CP1_CS_AS" /SQLSYSADMINACCOUNTS="<DomainName\UserName>"  
    ```  
  
### <a name="adding-a-node-to-an-existing-ssnoversion-failover-cluster-unattended"></a>Ajout d’un nœud à un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existant (sans assistance)  
  
1. Déployez l'image en exécutant Windows SysPrep Specialize. 
  
2. Joignez le cluster de basculement Windows. 
  
3. Exécutez setup.exe avec **/ACTION=AddNode** sur tous les nœuds :  
  
    ```  
    setup.exe /q /ACTION=AddNode /InstanceName=<InstanceName> /Features=SQLEngine  /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx"  /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
##  <a name="prepare-image"></a><a name="prepare"></a> Préparer l'image  
  
### <a name="prepare-a-stand-alone-instance-of-ssnoversion"></a>Préparez une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
1. Insérez le support d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans le dossier racine, double-cliquez sur Setup.exe. Pour effectuer l'installation à partir d'un partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur Setup.exe. 
  
2. L'Assistant Installation exécute le Centre d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour préparer une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cliquez sur **Préparation de l’image d’une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** dans la page **Avancé**. 
  
3. L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour continuer, cliquez sur **OK**. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**. 
  
4. Dans la page Mises à jour du produit, les dernières mises à jour disponibles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'affichent. Si vous ne souhaitez pas inclure les mises à jour, décochez la case **Inclure les mises à jour du produit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Si aucune mise à jour du produit n'est découverte, le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'affiche pas cette page et passe automatiquement à la page **Installer les fichiers d'installation** . 
  
5. Sur la page Installer les fichiers d'installation, le programme d'installation fournit la progression du téléchargement, de l'extraction et de l'installation des fichiers d'installation. Si une mise à jour de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est trouvée, et est spécifiée pour être incluse, cette mise à jour est également installée. 
  
6. L'outil d'analyse de configuration système vérifie l'état système de votre ordinateur avant que le programme d'installation ne se poursuive. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**. 
  
7. Dans la page **Préparer le type d’image**, sélectionnez **Préparer une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . 
  
     La page **Préparer le type d’image** est affichée uniquement quand vous avez une instance existante, préparée et non configurée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l’ordinateur. Vous pouvez choisir de préparer une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d'ajouter des fonctionnalités prises en charge par sys prep à une instance préparée existante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l'ordinateur. Pour plus d'informations sur l'ajout de fonctionnalités à une instance préparée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Ajouter des fonctionnalités à une instance préparée](#AddFeatures). 
  
8. Dans la page **Termes du contrat de licence** , prenez connaissance du contrat de licence, puis activez la case à cocher indiquant que vous en acceptez les termes et conditions. Pour aider à améliorer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez également activer l'option d'utilisation des fonctionnalités et envoyer des rapports à [!INCLUDE[msCoName](../../includes/msconame-md.md)]. 
  
9. Dans la page **Sélection de fonctionnalités** , sélectionnez les composants que vous voulez installer :  
  
    |Installation|Components|  
    |-|-|  
    |[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] SysPrep|[!INCLUDE[ssDE](../../includes/ssde-md.md)]<br /><br /> Réplication[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> Fonctionnalités de texte intégral<br /><br /> Data Quality Services<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif<br /><br /> [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]<br /><br /> Fonctionnalités redistribuables<br /><br /> Fonctionnalités partagées|  
  
     Une description de chaque groupe de composants apparaît dans le volet droit lorsque vous sélectionnez le nom de la fonctionnalité. Vous pouvez choisir n'importe quelle combinaison de cases à cocher. Pour plus d’informations, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) et [SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).
  
     Les composants requis pour les fonctionnalités sélectionnées sont affichés dans le volet droit. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe les composants requis qui n'ont pas déjà été installés lors de l'étape d'installation décrite plus loin dans cette procédure. 
  
10. Dans la page **Règles de préparation d'image** , l'outil d'analyse de configuration système vérifie l'état système de votre ordinateur avant que le programme d'installation ne se poursuive. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**. 
  
11. Dans la page Configuration de l'instance, spécifiez l'ID d'instance. Cliquez sur **Suivant** pour continuer. 
  
     **ID d’instance** : l’ID d’instance permet d’identifier les répertoires d’installation et les clés de registre de votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tel est le cas pour les instances par défaut et les instances nommées. Si l'instance préparée est finalisée en tant qu'instance par défaut pendant l'étape Finaliser, le nom de l'instance est remplacé par MSSQLSERVER. L'ID d'instance reste identique à celui spécifié. 
  
     **Répertoire racine de l’instance** : par défaut, le répertoire racine de l’instance est [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]. Pour spécifier un répertoire racine non défini par défaut, utilisez le champ fourni ou cliquez sur **Parcourir** pour rechercher un dossier d’installation. Le répertoire spécifié à l'étape de préparation sera utilisé durant la configuration à l'étape Finaliser. 
  
     Tous les Service Packs et mises à niveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'appliqueront à chaque composant d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
     **Instances installées** : la grille affiche les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui se trouvent sur l’ordinateur où le programme d’installation s’exécute. 
  
12. La page **Espace disque nécessaire** calcule l'espace disque nécessaire pour les fonctionnalités que vous spécifiez. Elle compare ensuite cet espace à l'espace disque disponible. 
  
13. L'Outil d'analyse de configuration système exécutera des règles de préparation d'image pour valider la configuration de votre ordinateur avec les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous avez spécifiées. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**. 
  
14. La page **Prêt à préparer l'image** affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Dans cette page, le programme d'installation indique si la fonctionnalité de mise à jour du produit est activée ou désactivée et la version de la mise à jour finale. Pour continuer, cliquez sur **Préparer**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe d'abord les composants requis pour les fonctionnalités sélectionnées, puis installe les fonctionnalités. 
  
15. Au cours de l'installation, la page **Progression de la préparation de l'image** fournit des informations d'état de sorte que vous puissiez contrôler la progression de l'installation au fil de l'exécution du programme d'installation. 
  
16. Après l'installation, la page **Terminé** fournit un lien vers le fichier journal résumé de l'installation et d'autres remarques importantes. Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**. 
  
17. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d'informations, consultez [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
18. L'étape de préparation est maintenant terminée. Vous pouvez finaliser l'image ou déployer l'image préparée comme décrit dans [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md). 
  
##  <a name="complete-image"></a><a name="complete"></a> Finaliser l'image  
  
### <a name="complete-a-prepared-instance-of-ssnoversion"></a>Finaliser une instance préparée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Si vous avez une instance préparée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluse dans l'image de votre ordinateur, un raccourci est affiché dans le menu Démarrer. Vous pouvez également lancer le Centre d’installation et cliquer sur **Finalisation d’image d’une instance autonome préparée** dans la page **Avancé** . 
  
2. L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour continuer, cliquez sur **OK**. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**. 
  
3. Dans la page **Fichiers de support du programme d'installation** , cliquez sur **Installer** pour installer les fichiers de support du programme d'installation. 
  
4. L'outil d'analyse de configuration système vérifie l'état système de votre ordinateur avant que le programme d'installation ne se poursuive. Lorsque la vérification est terminée, cliquez sur **Suivant** pour poursuivre. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**. 
  
5. Dans la page **Clé du produit** , sélectionnez une case d'option pour indiquer si vous installez une édition gratuite de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ou une version de production du produit qui a une clé PID. Pour plus d’informations, consultez [Éditions et fonctionnalités prises en charge de SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md). Si vous installez l'édition Evaluation, la période d'essai de 180 jours démarre lorsque vous terminez cette étape. 
  
6. Dans la page **Termes du contrat de licence** , prenez connaissance du contrat de licence, puis activez la case à cocher indiquant que vous en acceptez les termes et conditions. Pour aider à améliorer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez également activer l'option d'utilisation des fonctionnalités et envoyer des rapports à [!INCLUDE[msCoName](../../includes/msconame-md.md)]. 
  
7. Dans la page **Sélectionner une instance préparée** , sélectionnez l'instance préparée que vous souhaitez finaliser dans la zone de liste déroulante. Sélectionnez l’instance non configurée dans la liste **ID de l’instance** . 
  
     **Instances installées :** cette grille affiche toutes les instances, y compris toute instance préparée sur cet ordinateur. 
  
8. Dans la page **Vérification des fonctionnalités** sont répertoriés les fonctionnalités et composants sélectionnés inclus dans l'installation pendant l'étape de préparation. Si vous souhaitez ajouter davantage de fonctionnalités à votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non incluses dans l'instance préparée, vous devez d'abord effectuer cette étape pour finaliser l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puis ajouter les fonctionnalités à partir de **Ajouter des fonctionnalités** dans le **Centre d'installation**. 
  
    > [!NOTE]  
    >  Vous pouvez ajouter des fonctionnalités qui sont disponibles pour la version du produit que vous installez. Pour plus d’informations, consultez [Éditions et fonctionnalités prises en charge de SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
9. Dans la page Configuration de l'instance, spécifiez le nom de l'instance préparée. Il s'agit du nom de l'instance une fois que vous avez achevé la configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cliquez sur **Suivant** pour continuer. 
  
     **ID d’instance** : l’ID d’instance permet d’identifier les répertoires d’installation et les clés de registre de votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tel est le cas pour les instances par défaut et les instances nommées. Si l'instance préparée est finalisée en tant qu'instance par défaut pendant l'étape Finaliser, le nom de l'instance est remplacé par MSSQLSERVER. L'ID d'instance reste identique à celui spécifié pendant l'étape Préparer. 
  
     **Répertoire racine de l’instance** : le répertoire spécifié à l’étape de préparation sera utilisé et ne peut pas être modifié lors de cette étape. 
  
     Tous les Service Packs et mises à niveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'appliqueront à chaque composant d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
     **Instances installées** : la grille affiche les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui se trouvent sur l’ordinateur où le programme d’installation s’exécute. 
  
10. Le flux de travail du reste de cet article dépend des fonctionnalités sélectionnées durant l’étape de préparation. Il est possible que les pages ne soient pas toutes visibles ; cela dépend des sélections effectuées. 
  
11. Dans la page **Configuration du serveur** - Comptes de service, spécifiez les comptes de connexion des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les services réels configurés dans cette page dépendent des fonctionnalités que vous avez choisi d'installer. 
  
     Vous pouvez attribuer le même compte de connexion à tous les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou configurer chaque compte de service individuellement. Vous pouvez également spécifier si les services démarrent automatiquement, sont démarrés manuellement ou sont désactivés. [!INCLUDE[msCoName](../../includes/msconame-md.md)] vous recommande de configurer les comptes de service individuellement afin d'octroyer le moins de privilèges à chaque service, sachant que les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disposent des autorisations minimales requises pour effectuer leurs tâches. Pour plus d’informations, consultez [Configuration du serveur - Comptes de service](./install-sql-server.md) et [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md). 
  
     Pour spécifier le même compte d'ouverture de session pour tous les comptes de service dans cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], fournissez les informations d'identification dans les champs en bas de page. 
  
     **Note de sécurité** [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
     Lorsque vous avez terminé de spécifier les informations de connexion pour les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez sur **Suivant**. 
  
12. Utilisez l’onglet **Configuration du serveur — Classement** pour spécifier les classements non définis par défaut pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d’informations, consultez [Configuration du serveur - Classement](./install-sql-server.md). 
  
13. Utilisez la page Configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] - Attribution de privilèges d’accès aux comptes pour spécifier les éléments suivants :  
  
    - Mode de sécurité - Sélectionnez l'authentification Windows ou le mode d'authentification mixte pour votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous sélectionnez Mode d'authentification mixte, vous devez fournir un mot de passe fort pour le compte administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégré. 
  
         Lorsque la connexion d'un périphérique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]est établie, le mécanisme de sécurité est le même pour l'authentification Windows et le mode mixte. Pour plus d’informations, consultez [Configuration du moteur de base de données - Configuration du serveur](./install-sql-server.md). 
  
    - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Administrateurs : vous devez spécifier au moins un administrateur système pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour ajouter le compte sous lequel le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute, cliquez sur **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, cliquez sur **Ajouter** ou sur **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposeront des privilèges d'administrateur pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Configuration du moteur de base de données - Configuration du serveur](./install-sql-server.md). 
  
     Lorsque vous avez fini de modifier la liste, cliquez sur **OK**. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Une fois la liste complète, cliquez sur **Suivant**. 
  
14. Utilisez la page Configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] - Répertoires de données pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, cliquez sur **Suivant**. 
  
    > [!IMPORTANT]  
    >  Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
     Pour plus d’informations, consultez [Configuration du moteur de base de données – Répertoires de données](./install-sql-server.md). 
  
15. Utilisez la page Configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] - FILESTREAM pour activer FILESTREAM pour votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Configuration du moteur de base de données - Filestream](./install-sql-server.md). 
  
16. Utilisez la page Configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour spécifier le type d'installation de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à créer. Pour plus d’informations sur les modes de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Options de configuration Reporting Services &#40;SSRS&#41;](./install-sql-server.md). 
  
17. Dans la page **Création de rapports d'erreurs** , spécifiez les informations que vous souhaitez envoyer à [!INCLUDE[msCoName](../../includes/msconame-md.md)] afin d'aider à améliorer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les options de création de rapports d'erreurs sont activées par défaut. 
  
18. Dans la page **Règles de finalisation d'image** , l'outil d'analyse de configuration système exécutera les règles de finalisation d'image pour valider la configuration de votre ordinateur avec les configurations [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous avez spécifiées. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**. 
  
19. La page **Prêt à finaliser l'image** affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Pour continuer, cliquez sur **Installer**. 
  
20. Au cours de l'installation, la page **Progression de la finalisation d'image** fournit des informations d'état de sorte que vous puissiez contrôler la progression de l'installation au fil de l'exécution du programme d'installation. 
  
21. Après l'installation, la page **Terminé** fournit un lien vers le fichier journal résumé de l'installation et d'autres remarques importantes. Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**. 
  
22. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d'informations, consultez [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
23. Cette étape achève la configuration de l'instance préparée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et termine l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
##  <a name="add-features-to-a-prepared-instance"></a><a name="AddFeatures"></a> Add Features to a Prepared Instance  
  
### <a name="add-features-to-a-prepared-instance-of-ssnoversion"></a>Ajouter des fonctionnalités à une instance préparée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Insérez le support d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans le dossier racine, double-cliquez sur Setup.exe. Pour effectuer l'installation à partir d'un partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur Setup.exe. 
  
2. L'Assistant Installation exécute le Centre d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour ajouter des fonctionnalités à une instance préparée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cliquez sur **Préparation de l’image d’une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** dans la page **Avancé**. 
  
3. L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour continuer, cliquez sur **OK**. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**. 
  
4. Sur la page Fichiers de support du programme d'installation, cliquez sur **Installer** pour installer les fichiers de support du programme d'installation. 
  
5. Dans la page **Préparer le type d’image**, sélectionnez l’option **Ajouter des fonctionnalités à une instance préparée existante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Sélectionnez l'instance préparée spécifique à laquelle vous souhaitez ajouter des fonctionnalités dans la zone de liste déroulante d'instances préparées disponibles. 
  
6. Dans la page **Sélection de composant** , spécifiez les fonctionnalités que vous souhaitez ajouter à l'instance préparée spécifiée. 
  
     Les composants requis pour les fonctionnalités sélectionnées sont affichés dans le volet droit. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe les composants requis qui n'ont pas déjà été installés lors de l'étape d'installation décrite plus loin dans cette procédure. 
  
7. Dans la page **Règles de préparation d'image** , l'outil d'analyse de configuration système vérifie l'état système de votre ordinateur avant que le programme d'installation ne se poursuive. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**. 
  
8. La page Espace disque nécessaire calcule l'espace disque nécessaire pour les fonctionnalités que vous spécifiez. Elle compare ensuite cet espace à l'espace disque disponible. 
  
9. Dans la page **Règles de préparation d'image** , l'outil d'analyse de configuration système exécutera des règles de préparation d'image pour valider la configuration de votre ordinateur avec les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous avez spécifiées. Vous pouvez afficher les détails à l'écran en cliquant sur **Afficher les détails** ou sous la forme d'un rapport HTML en cliquant sur **Afficher le rapport détaillé**. 
  
10. La page **Prêt à préparer l'image** affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Pour continuer, cliquez sur **Installer**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe d'abord les composants requis pour les fonctionnalités sélectionnées, puis installe les fonctionnalités. 
  
11. Au cours de l'installation, la page **Progression de la préparation de l'image** fournit des informations d'état de sorte que vous puissiez contrôler la progression de l'installation au fil de l'exécution du programme d'installation. 
  
12. Après l'installation, la page **Terminé** fournit un lien vers le fichier journal résumé de l'installation et d'autres remarques importantes. Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**. 
  
13. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d'informations, consultez [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
##  <a name="remove-features-from-a-prepare-instance"></a><a name="RemoveFeatures"></a> Supprimer des fonctionnalités d'une instance préparée  
  
### <a name="removing-features-from-a-prepared-instance-of-ssnoversion"></a>Suppression de fonctionnalités d'une instance préparée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Pour commencer le processus de désinstallation, dans le menu **Démarrer** , cliquez sur **Panneau de configuration** , puis double-cliquez sur **Programmes et fonctionnalités**. 
  
2. Double-cliquez sur le composant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à désinstaller, puis cliquez sur **Supprimer**. 
  
3. Les règles de support d'installation seront exécutées pour vérifier la configuration de votre ordinateur. Pour continuer, cliquez sur **OK** . 
  
4. Dans la page **Sélectionner une instance** , sélectionnez l'instance préparée que vous souhaitez modifier. Le nom de l'instance préparée sera affiché comme « ID_instance_préparée non configurée » où ID_instance_préparée est l'instance que vous sélectionnez. 
  
5. Dans la page **Sélectionner les fonctionnalités** , spécifiez les fonctionnalités à supprimer de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous avez spécifiée. Cliquez sur **Suivant** pour continuer. 
  
6. Les règles de suppression seront exécutées pour vérifier que l'opération peut s'effectuer correctement. 
  
7. Dans la page **Prêt pour la suppression** , consultez la liste des composants et des fonctionnalités à désinstaller. 
  
8. La page **Progression de la suppression** affiche l'état de l'opération. 
  
9. Dans la page **Finaliser** , vous pouvez examiner l'état d'achèvement de l'opération. Cliquez sur **Fermer** pour quitter l'Assistant Installation. 
  
##  <a name="uninstalling-a-prepared-instance"></a><a name="Uninstall"></a> Désinstallation d'une instance préparée  
  
### <a name="uninstall-a-prepared-instance-of-ssnoversion"></a>Désinstaller une instance préparée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Pour commencer le processus de désinstallation, dans le menu **Démarrer** , cliquez sur **Panneau de configuration** , puis double-cliquez sur **Programmes et fonctionnalités**. 
  
2. Double-cliquez sur le composant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à désinstaller, puis cliquez sur **Supprimer**. 
  
3. Les règles de support d'installation seront exécutées pour vérifier la configuration de votre ordinateur. Pour continuer, cliquez sur **OK** . 
  
4. Dans la page **Sélectionner une instance** , sélectionnez l'instance préparée que vous souhaitez modifier. Le nom de l'instance préparée sera affiché comme « ID_instance_préparée non configurée » où ID_instance_préparée est l'instance que vous sélectionnez. 
  
5. Dans la page **Sélectionner les fonctionnalités** , spécifiez les fonctionnalités à supprimer de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous avez spécifiée. Cliquez sur **Suivant** pour continuer. 
  
6. Dans la page **Règles de suppression** , le programe d'installation exécutera les règles pour vérifier que l'opération peut s'effectuer correctement. 
  
7. Dans la page **Prêt pour la suppression** , consultez la liste des composants et des fonctionnalités à désinstaller. 
  
8. La page **Progression de la suppression** affiche l'état de l'opération. 
  
9. Dans la page **Finaliser** , vous pouvez examiner l'état d'achèvement de l'opération. Cliquez sur **Fermer** pour quitter l'Assistant Installation. 
  
10. Répétez les étapes 1 à 9 jusqu'à ce que tous les composants de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] aient été supprimés. 
  
##  <a name="modifying-or-uninstalling-a-completed-instance-of-ssnoversion"></a><a name="bk_Modifying_Uninstalling"></a> Modification ou désinstallation d'une instance finalisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 
 Le processus d'ajout ou de suppression de fonctionnalités ou de désinstallation d'une instance finalisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est semblable au processus applicable à une instance installée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez les articles suivants :  
  
- [Ajouter des fonctionnalités à une instance de SQL Server &#40;programme d’installation&#41;](./add-features-to-an-instance-of-sql-server-setup.md)  
  
- [Désinstaller une instance existante de SQL Server &#40;programme d’installation&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Qu'est-ce que Sysprep ?](/previous-versions/windows/it-pro/windows-vista/cc721940(v=ws.10))   
 [Fonctionnement de Windows SysPrep](/previous-versions/windows/it-pro/windows-vista/cc766514(v=ws.10))  
  
