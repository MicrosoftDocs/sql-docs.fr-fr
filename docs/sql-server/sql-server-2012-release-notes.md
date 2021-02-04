---
title: Notes de publication de SQL Server 2012 | Microsoft Docs
description: Ce document Notes de publication décrit les problèmes connus dont vous devez prendre connaissance avant d’installer ou de dépanner Microsoft SQL Server 2012.
ms.prod: sql
ms.technology: release-landing
ms.custom: ''
ms.date: 07/22/2020
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Release Notes, SQL Server
ms.assetid: 9ccb390a-67a9-4593-85ea-2b4c41c4620f
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: 06294cb2dbf588ed5f7588d05ee5aa1c5af971bc
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076071"
---
# <a name="sql-server-2012-release-notes"></a>Notes de publication de SQL Server 2012
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]
Ce document Notes de publication décrit les problèmes connus dont vous devez prendre connaissance avant d’installer ou de dépanner [Microsoft SQL Server 2012](https://go.microsoft.com/fwlink/?LinkId=238647)). Ce document Notes de publication, uniquement disponible en ligne (absent du support d'installation), est régulièrement mis à jour.  
  
Pour plus d'informations sur le démarrage et l'installation de SQL Server 2012, consultez le fichier Lisez-moi de SQL Server 2012. Le document Lisez-moi est disponible sur le support d'installation et sur la page de téléchargement du fichier [Lisez-moi](https://download.microsoft.com/download/3/B/D/3BD9DD65-D3E3-43C3-BB50-0ED850A82AD5/ENU/Readme.htm) . Vous pouvez également trouver des informations supplémentaires dans la [documentation en ligne de SQL Server](/previous-versions/sql/sql-server-2012/ms130214(v=sql.110)) et sur les [forums SQL Server](https://go.microsoft.com/fwlink/?LinkId=213599).  
  
## <a name="10-before-you-install"></a><a name="Install"></a>1.0 Avant l’installation  
Lisez les informations répertoriées ci-dessous avant d'installer [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)].  
  
### <a name="11-rules-documentation-for-sql-server-2012-setup"></a>1.1 Documentation relative aux règles concernant l'installation de SQL Server 2012  
**Problème :** le programme d’installation de SQL Server valide la configuration de votre ordinateur avant la fin de l’opération d’installation. Les différentes règles qui sont exécutées au cours de la procédure d'installation de SQL Server sont capturées à l'aide du rapport d'analyse de configuration système (SCC). La documentation relative à ces règles d'installation n'est plus disponible dans MSDN Library.  
  
**Solution de contournement :** reportez-vous au rapport d’analyse de configuration système pour en savoir plus sur ces règles d’installation. L'analyse de configuration système génère un rapport qui contient une brève description de chaque règle exécutée et de l'état d'exécution. Le rapport de contrôle de configuration du système se trouve à l’emplacement %programfiles%\Microsoft SQL Server\110\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\.  
  
### <a name="12-adding-a-local-user-account-for-the-distributed-replay-controller-service-might-terminate-setup-unexpectedly"></a>1.2 L'ajout d'un compte d'utilisateur local pour le service Distributed Replay Controller peut interrompre l'installation de façon inattendue  
**Problème :** Dans la page **Distributed Replay Controller** de l’installation de SQL Server, si vous essayez d’ajouter un compte d’utilisateur local pour le service Distributed Replay Controller, le programme d’installation s’interrompt de façon inattendue avec le message d’erreur « Erreur du programme d’installation de SQL Server ».  
  
**Solution de contournement :** Pendant l’installation de SQL, n’ajoutez pas de comptes d’utilisateurs locaux à l’aide des options « Ajouter l’utilisateur actuel » ou « Ajouter… ». Après l'installation, ajoutez un compte d'utilisateur local manuellement en procédant comme suit :  
  
1.  Arrêter le service SQL Server Distributed Replay Controller  
  
2.  Sur l'ordinateur contrôleur sur lequel le service contrôleur est installé, depuis l'invite de commandes, tapez dcomcnfg.  
  
3.  Dans la fenêtre Services de composants, accédez à **Racine de la console** -> **Services de composants** -> **Ordinateurs** -> **Poste de travail** -> **Dconfig** ->**DReplayController**.  
  
4.  Cliquez avec le bouton droit sur **DReplayController**, puis cliquez sur **Propriétés**.  
  
5.  Dans la fenêtre **Propriétés de DReplayController** , sous l'onglet **Sécurité** , cliquez sur **Modifier** dans la section **Autorisations d'exécution et d'activation** .  
  
6.  Accordez au compte d'utilisateur local des autorisations **Activation locale et Activation à distance** , puis cliquez sur **OK**.  
  
7.  Dans la section Autorisations d'accès, cliquez sur **Modifier** et accordez des autorisations **Accès local et Accès à distance** au compte d'utilisateur local, puis cliquez sur **OK**.  
  
8.  Cliquez sur **OK** pour fermer la fenêtre **Propriétés de DReplayController** .  
  
9. Sur l'ordinateur contrôleur, ajoutez le compte d'utilisateur local au groupe **Utilisateurs du modèle COM distribué** .  
  
10. Démarrez le service SQL Server Distributed Replay Controller.  
  
### <a name="13-sql-server-setup-might-fail-while-trying-to-start-the-sql-server-browser-service"></a>1.3 Le programme d'installation de SQL Server risque d'échouer pendant la tentative de démarrage du service SQL Server Browser  
**Problème :** il est possible que le programme d’installation de SQL Server échoue pendant la tentative de démarrage du service SQL Server Browser et retourne un message d’erreur similaire à celui-ci :  
  
```
The following error has occurred:  
Service 'SQLBrowser' start request failed. Click 'Retry' to retry the failed action, or click 'Cancel' to cancel this action and continue setup.
```
  
or  
  
```
The following error has occurred:  
SQL Server Browser configuration for feature 'SQL_Browser_Redist_SqlBrowser_Cpu32' was cancelled by user after a previous installation failure. The last attempted step: Starting the SQL Server Browser service 'SQLBrowser', and waiting for up to '900' seconds for the process to complete.
```
  
**Solution de contournement :** ceci peut se produire en cas d’échec de l’installation du moteur SQL Server ou d’Analysis Services. Pour résoudre ce problème, reportez-vous aux journaux du programme d'installation SQL Server et corrigez les erreurs du moteur SQL Server Engine et d'Analysis Services. Pour plus d'informations, consultez Afficher et lire les fichiers journaux d'installation de SQL Server. Pour plus d'informations, consultez [View and Read SQL Server Setup Log Files](../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
### <a name="14-sql-server-2008-2008-r2-analysis-services-failover-cluster-upgrade-to-sql-server-2012-might-fail-after-renaming-the-network-name"></a>1.4 La mise à niveau vers SQL Server 2012 d'un cluster de basculement SQL Server 2008, 2008 R2 Analysis Services peut échouer après un changement de nom du réseau  
**Problème :** après avoir changé le nom réseau d’une instance de cluster de basculement Microsoft SQL Server 2008 ou 2008 R2 Analysis Services à l’aide de l’outil Administrateur de cluster Windows, l’opération de mise à niveau peut échouer.  
  
**Solution de contournement :** Pour résoudre ce problème, mettez à jour l’entrée de Registre ClusterName en suivant les instructions figurant dans la section de résolution de [cet article de la base de connaissances](https://support.microsoft.com/kb/955784).  
  
### <a name="15-installing-sql-server-2012-on-windows-server-2008-r2-server-core-service-pack-1"></a>1.5 Installation de SQL Server 2012 sous Windows Server 2008 R2 Server Core Service Pack 1  
Vous pouvez installer SQL Server sur Windows Server 2008 R2 Server Core SP1, avec les restrictions suivantes :  
  
-   Microsoft SQL Server 2012 ne prend pas en charge l'installation avec l'Assistant d'installation sur le système d'exploitation Server Core. Lors de l'installation sous Server Core, le programme d'installation SQL Server prend en charge le mode silencieux complet via le paramètre /Q ou le mode silencieux simple via le paramètre /QS.  
  
-   La mise à niveau d'une version antérieure de SQL Server vers Microsoft SQL Server 2012 n'est pas prise en charge sur un ordinateur qui exécute Windows Server 2008 R2 Server Core SP1.  
  
-   L'installation d'une version 32 bits de Microsoft SQL Server 2012 n'est pas prise en charge sur un ordinateur exécutant Windows Server 2008 R2 Server Core SP1.  
  
-   Microsoft SQL Server 2012 ne peut pas être installé côte à côte avec des versions antérieures de SQL Server sur un ordinateur qui exécute Windows Server 2008 R2 Server Core SP1.  
  
-   Toutes les fonctionnalités de SQL Server 2012 ne sont pas prises en charge sur le système d'exploitation Server Core. Pour plus d'informations sur les fonctionnalités prises en charge et sur l'installation de SQL Server 2012 sur Server Core, consultez [Installer SQL Server 2012 sur Server Core](/previous-versions/sql/sql-server-2012/hh231669(v=sql.110)).  
  
### <a name="16-semantic-search-requires-you-to-install-an-additional-dependency"></a>1.6 La recherche sémantique exige l'installation d'une dépendance supplémentaire  
**Problème :** la recherche sémantique statistique présente une condition préalable supplémentaire, à savoir la base de données des statistiques linguistiques de sémantique, qui n'est pas installée par le programme d'installation SQL Server.  
  
**Solution de contournement :** pour installer la base de données des statistiques linguistiques de sémantique comme condition préalable à l’indexation sémantique, effectuez les étapes suivantes :  
  
1.  Localisez et exécutez le package Windows Installer nommé SemanticLanguageDatabase.msi sur le support d'installation de SQL Server pour extraire la base de données. Pour SQL Server 2012 Express, téléchargez la base de données des statistiques linguistiques de sémantique à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=52681) (https://www.microsoft.com/download/details.aspx?id=52681), puis exécutez le package Windows Installer.  
  
2.  Déplacez la base de données vers un dossier de données approprié. Si vous laissez la base de données dans l'emplacement par défaut, vous devez modifier les droits avant de pouvoir l'attacher.  
  
3.  Attachez la base de données extraite.  
  
4.  Enregistrez la base de données en appelant la procédure stockée **sp_fulltext_semantic_register_language_statistics_db** et en fournissant le nom attribué à la base de données lors de son attachement.  
  
Si ces tâches ne sont pas menées à bien, le message d'erreur suivant s'affichera lorsque vous tenterez de créer un index sémantique :  
  
```
Msg 41209, Level 16, State 3, Line 1  
A semantic language statistics database is not registered. Full-text indexes using 'STATISTICAL_SEMANTICS' cannot be created or populated.
```
  
### <a name="17-installation-prerequisite-handling-during-sql-server-2012-setup"></a>1.7 Gestion des composants requis pendant l'installation de SQL Server 2012  
Les points suivants décrivent le comportement d'installation des composants requis pendant l'installation de SQL Server 2012 :  
  
-   L'installation de SQL Server 2012 est prise en charge uniquement sous Windows 7 SP1 ou Windows Server 2008 R2 SP1. Toutefois, le programme d'installation ne bloque pas l'installation de SQL Server 2012 sur Windows 7 ou Windows Server 2008 R2.  
  
-   .NET Framework 3.5 SP1 est requis pour SQL Server 2012 lorsque vous sélectionnez les services Database Engine, Replication, Master Data Services, Reporting Services, Data Quality Services (DQS) ou SQL Server Management Studio, mais l'infrastructure n'est plus installée par le programme d'installation de SQL Server.  
  
    -   Si vous exécutez le programme d'installation sur un ordinateur avec le système d'exploitation Windows Vista SP2 ou Windows Server 2008 SP2 et que .NET Framework 3.5 SP1 n'y est pas installé, le programme d'installation de SQL Server exige que .NET Framework 3.5 SP1 soit téléchargé et installé avant de vous laisser continuer l'installation de SQL Server. Téléchargez .NET Framework 3.5 SP1 depuis Windows Update ou directement à partir d' [ici](https://www.microsoft.com/download/details.aspx?id=25150). Pour éviter toute interruption pendant l'installation de SQL Server, téléchargez et installez .NET Framework 3.5 SP1 avant d'exécuter le programme d'installation de SQL Server.  
  
    -   Si vous exécutez le programme d'installation sur un ordinateur disposant du système d'exploitation Windows 7 SP1 ou de Windows Server 2008 R2 SP1, vous devez activer .NET Framework 3.5 SP1 avant d'installer SQL Server 2012.  
  
        **Utilisez l'une des méthodes suivantes pour activer .NET Framework 3.5 SP1 sur Windows Server 2008 R2 SP1 :**  
  
        Méthode 1 : Utiliser le Gestionnaire de serveur  
  
        1.  Dans le Gestionnaire de serveur, cliquez sur **Ajouter des fonctionnalités** pour afficher la liste des fonctionnalités possibles.  
  
        2.  Dans l'interface de **sélection des fonctionnalités** , développez l'entrée **Fonctionnalités .NET Framework 3.5.1** .  
  
        3.  Après avoir développé l'entrée **Fonctionnalités .NET Framework 3.5.1**, vous avez accès à deux cases à cocher. L'une de ces cases à cocher correspond à .NET Framework 3.5.1 et l'autre à Activation de Windows Communication Foundation. Sélectionnez **.NET Framework 3.5.1**, puis cliquez sur **Suivant**. Vous ne pouvez pas installer les fonctionnalités .NET Framework 3.5.1 si les services de rôle et les fonctionnalités requis ne sont pas également installés.  
  
        4.  Dans **Confirmer les sélections pour l’installation**, passez les sélections en revue, puis cliquez sur Installer.  
  
        5.  Attendez que le processus d'installation se termine, puis cliquez sur **Fermer**.  
  
        Méthode 2 : Utiliser Windows PowerShell  
  
        1.  Cliquez sur **Démarrer** | **Tous les programmes** | **Accessoires**.  
  
        2.  Développez **Windows PowerShell**, cliquez avec le bouton droit sur **Windows PowerShell**, puis cliquez sur **Exécuter en tant qu'administrateur**. Cliquez sur **Oui** dans la boîte de dialogue **Contrôle de compte d'utilisateur** .  
  
        3.  À l'invite de commandes PowerShell, saisissez les commandes suivantes, puis appuyez sur Entrée après chaque commande :  
  
            ```  
            Import-Module ServerManager  
            Add-WindowsFeature as-net-framework  
            ```  
  
        **Utilisez la méthode suivante pour activer .NET Framework 3.5 SP1 sur Windows 7 SP1 :**  
  
        1.  Cliquez sur **Démarrer** | **Panneau de configuration** | **Programmes**, puis cliquez sur **Activer ou désactiver les fonctionnalités Windows**. Si vous êtes invité à entrer un mot de passe administrateur ou à fournir une confirmation, tapez le mot de passe ou fournissez la confirmation.  
  
        2.  Pour activer la fonctionnalité **Microsoft .NET Framework 3.5.1**, activez la case à cocher qui se trouve en regard de son nom. Pour désactiver une fonctionnalité Windows, désactivez la case à cocher correspondante.  
  
        3.  Cliquez sur **OK**.  
  
        **Utilisez Deployment Image Servicing and Management (DISM.exe) pour activer .NET Framework 3.5 SP1 :**  
  
        Vous pouvez également activer .NET Framework 3.5 SP1 à l'aide de Deployment Image Servicing and Management (DISM.exe). Pour plus d'informations sur l'activation des fonctionnalités Windows en ligne, consultez [Activer ou désactiver les fonctionnalités Windows en ligne](/previous-versions/windows/it-pro/windows-7/dd744582(v=ws.10)). Voici les instructions permettant d'activer .NET Framework 3.5 SP1 :  
  
        1.  À l'invite de commandes, tapez la commande suivante pour répertorier l'ensemble des fonctionnalités disponibles dans le système d'exploitation.  
  
            ```  
            sm /online /Get-Features  
            ```  
  
        2.  Facultatif : à l’invite de commandes, tapez la commande suivante pour afficher des informations sur la fonctionnalité spécifique qui vous intéresse.  
  
            ```  
            Dism /online /Get-FeatureInfo /FeatureName:NetFx3  
            ```  
  
        3.  Tapez la commande suivante pour activer Microsoft .NET Framework 3.5.1.  
  
            ```  
            Dism /online /Enable-Feature /FeatureName:NetFx3  
            ```  
  
-   .NET Framework 4 est un élément requis pour SQL Server 2012. Le programme d'installation de SQL Server installe .NET Framework 4 pendant l'étape d'installation des fonctionnalités.  
  
    SQL Server 2012 Express n'installe pas .NET Framework 4 lors de l'installation sur le système d'exploitation Windows Server 2008 R2 SP1 Server Core. Lors de l'installation de SQL Server 2012 Express (Database uniquement), .NET Framework 4 n'est pas nécessaire si .NET Framework 3.5 SP1 est présent. Lorsque .NET Framework 3.5 SP1 n'est pas présent ou lors de l'installation de SQL Server 2012 Management Studio Express, SQL Server 2012 Express with Tools ou SQL Server 2012 Express with Advanced Services, vous devez installer .NET Framework 4 avant d'installer SQL Server 2012 Express sur un système d'exploitation Windows Server 2008 R2 SP1 Server Core.  
  
-   Pour garantir l'installation correcte du composant Visual Studio, SQL Server vous oblige à installer une mise à jour. Le programme d'installation de SQL Server vérifie la présence de cette mise à jour, puis vous demande de télécharger et d'installer la mise à jour pour que l'installation puisse se poursuivre. Pour éviter l'interruption pendant l'installation de SQL Server, téléchargez et installez la mise à jour avant d'exécuter le programme d'installation de SQL Server, tel que décrit ci-dessous (ou installez toutes les mises à jour de .NET Framework 3.5 SP1 disponibles sur Windows Update) :  
  
    -   Si vous installez SQL Server 2012 sur un ordinateur équipé du système d'exploitation Windows Vista SP2 ou Windows Server 2008 SP2, obtenez la mise à jour requise [ici](https://support.microsoft.com/?kbid=956250).  
  
    -   Si vous installez SQL Server 2012 sur un ordinateur équipé du système d'exploitation Windows 7 SP1 ou de Windows Server 2008 R2 SP1, cette mise à jour est déjà installée sur l'ordinateur.  
  
-   Windows PowerShell 2.0 est un élément prérequis pour l'installation des composants de moteur de base de données SQL Server 2012 et de SQL Server Management Studio, mais Windows PowerShell n'est plus installé par le programme d'installation SQL Server. Si PowerShell 2.0 n'est pas présent sur votre ordinateur, activez-le en suivant les instructions de la page [Infrastructure de gestion Windows](https://mskb.pkisolutions.com/kb/968929) . La manière d'obtenir Windows PowerShell 2.0 dépend du système d'exploitation que vous exécutez :  
  
    -   Windows Server 2008 - Windows PowerShell 1.0 est une fonctionnalité qui peut être ajoutée. Les versions Windows PowerShell 2.0 sont téléchargées et installées (efficacement en tant que correctif de système d'exploitation).  
  
    -   Windows 7/Windows Server 2008 R2 - Windows PowerShell 2.0 sont installés par défaut.  
  
-   Si vous envisagez d'utiliser les fonctionnalités SQL Server 2012 dans un environnement SharePoint, SharePoint Server 2010 Service Pack 1 (SP1) et la mise à jour cumulative d'août de SharePoint sont requis. Vous devez installer SP1 et la [mise à jour cumulative d'août](https://blogs.technet.com/b/stefan_gossner/archive/2010/09/02/august-2010-cumulative-update-for-sharepoint-has-been-released.aspx)de SharePoint, puis corriger intégralement la batterie de serveurs avant de lui ajouter les fonctionnalités SQL Server 2012. Cette condition requise concerne les fonctionnalités SQL Server 2012 suivantes : l'utilisation d'une instance du moteur de base de données en tant que serveur de base de données de la batterie de serveurs, la configuration de PowerPivot pour SharePoint ou le déploiement de Reporting Services en mode SharePoint.  
  
### <a name="18-supported-operating-systems-for-sql-server-2012"></a>1.8 Systèmes d'exploitation pris en charge pour SQL Server 2012  
SQL Server 2012 est pris en charge sur les systèmes d'exploitation Windows Vista SP2, Windows Server 2008 SP2, Windows 2008 R2 SP1 et Windows 7 SP1.  
  
### <a name="19-sync-framework-is-not-included-in-the-installation-package"></a>1.9 Sync Framework n'est pas inclus dans le package d'installation  
**Problème :** Sync Framework n’est pas inclus dans le package d’installation de SQL Server 2012.  
  
**Solution de contournement :** Téléchargez la version appropriée de Sync Framework à partir de [cette page du Centre de téléchargement Microsoft](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=23217).  
  
### <a name="110-if-visual-studio-2010-service-pack-1-is-uninstalled-the-sql-server-2012-instance-must-be-repaired-to-restore-certain-components"></a>1.10 Si Visual Studio 2010 Service Pack 1 n'est pas installé, l'instance de SQL Server 2012 doit être réparée pour restaurer certains composants  
**Problème :** l'installation de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] est dépendante de certains composants de Visual Studio 2010 Service Pack 1. Si vous désinstallez le Service Pack 1, certains composants partagés sont rétrogradés à leur version originale, et d'autres sont complètement supprimés de l'ordinateur.  
  
**Solution de contournement :** Réparez l’instance de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] à partir du support source d’origine ou de l’emplacement d’installation réseau.  
  
1.  Lancez le programme d'installation de SQL Server (setup.exe) à partir du support d'installation de SQL Server.  
  
2.  Après avoir vérifié les composants requis et le système, le programme d'installation affiche la page **Centre d'installation SQL Server** .  
  
3.  Cliquez sur **Maintenance** dans la zone de navigation gauche, puis sur **Réparer** pour démarrer la réparation. Si le Centre d'installation a été lancé à l'aide du menu **Démarrer** , vous devez fournir l'emplacement actuel du support d'installation.  
  
4.  La règle de support d'installation et les routines de fichiers sont exécutées pour garantir que les composants requis sont installés sur votre système et que les règles de validation du programme d'installation ont été correctement exécutées sur l'ordinateur. Cliquez sur **OK** ou sur **Installer** pour continuer.  
  
5.  Dans la page **Sélectionner une instance** , sélectionnez l’instance à réparer, puis cliquez sur **Suivant** pour continuer.  
  
6.  Les règles de réparation sont exécutées pour valider l'opération. Pour continuer, cliquez sur **Suivant**.  
  
7.  La page **Prêt à réparer** indique que l'opération peut se poursuivre. Pour continuer, cliquez sur **Réparer**.  
  
8.  La page **Progression de la réparation** indique l'état de l'opération de réparation. La page **Terminé** indique que l'opération est terminée.  
  
Pour plus d'informations sur la réparation d'une instance de SQL Server, consultez [Réparer une installation défectueuse de SQL Server 2012](../database-engine/install-windows/repair-a-failed-sql-server-installation.md).  
  
### <a name="111-an-instance-of-sql-server-2012-might-fail-after-an-os-upgrade"></a>1.11 Une instance de SQL Server 2012 risque d'échouer après une mise à niveau du système d'exploitation  
**Problème :** il est possible qu’une instance de SQL Server 2012 échoue et retourne l’erreur suivante si vous mettez à niveau le système d’exploitation de Windows Vista vers Windows 7 SP1.  
  
`Setup has detected that the .NET Framework version 4 needs to be repaired. Do not restart your computer until Setup is complete.`  
  
**Solution de contournement** : réparez l’installation du .NET Framework 4 après avoir mis à niveau le système d’exploitation. Pour plus d'informations, consultez [Comment réparer une installation existante de .NET Framework](https://support.microsoft.com/kb/306160).  
  
### <a name="112-sql-server-edition-upgrade-requires-a-restart"></a>1.12 La mise à niveau de l'édition de SQL Server nécessite un redémarrage  
**Problème** : quand vous mettez à niveau l’édition d’une instance de SQL Server 2012, il est possible que certaines des fonctionnalités associées à la nouvelle édition ne soient pas activées immédiatement.  
  
**Solution de contournement** : redémarrez l’ordinateur après la mise à niveau de l’édition d’une instance de SQL Server 2012. Pour plus d'informations sur les mises à niveau prises en charge dans SQL Server 2012, consultez [Mises à niveau de version et d'édition prises en charge](../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md).  
  
### <a name="113-database-with-read-only-filegroup-or-files-cannot-be-upgraded"></a>1.13 Une base de données avec des fichiers ou un groupe de fichiers en lecture seule ne peut pas être mise à niveau.  
**Problème** :  Vous ne pouvez pas mettre à niveau une base de données en attachant une base de données ou en restaurant une à partir d’une sauvegarde si la base de données ou ses fichiers/groupes de fichiers sont accessibles en lecture seule.  L'erreur 3415 est retournée.  Ce problème s'applique également lors d'une mise à niveau sur place d'une instance de SQL Server. C'est-à-dire, si vous tentez de remplacer une instance existante de SQL Server en installant SQL Server 2012 et une ou plusieurs bases de données existantes sont en lecture seule.  
  
**Solution de contournement :** avant de procéder à la mise à niveau, vérifiez que la base de données et ses fichiers/groupes de fichiers sont accessibles en lecture/écriture.  
  
### <a name="114-reinstalling-an-instance-of-sql-server-failover-custer-fails-if-you-use-the-same-ip-address"></a>1.14 Échec de la réinstallation d'une instance de cluster de basculement SQL Server si vous utilisez la même adresse IP  
**Problème :** si vous spécifiez une adresse IP incorrecte lors de l’installation d’une instance de cluster de basculement SQL Server, l’installation échoue. Après avoir désinstallé l'instance en échec, si vous tentez de réinstaller l'instance de cluster de basculement SQL Server avec le même nom d'instance et une adresse IP correcte, l'installation échoue. Cet échec est dû au groupe de ressources dupliqué conservé par l'installation précédente.  
  
**Solution de contournement :** pour résoudre ce problème, utilisez un autre nom d’instance durant la réinstallation, ou supprimez manuellement le groupe de ressources avant la réinstallation. Pour plus d'informations, consultez [Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server](failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="20-analysis-services"></a><a name="AS"></a>2.0 Analysis Services  
  
### <a name="21-sql-editor-and-as-editor-cannot-connect-to-their-respective-server-instances-in-the-same-ssms-instance"></a>2.1 L'éditeur SQL et l'éditeur AS ne peuvent pas se connecter à leur instance de serveur respective dans la même instance SSMS  
**Problème :** impossible de se connecter à un serveur Analysis Services à l’aide de l’éditeur MDX/DMX quand l’éditeur SQL est déjà connecté.  
  
Lors de l'utilisation de SQL Server Management Studio 2012 (SSMS), si un fichier .sql est ouvert dans l'éditeur et est connecté à une instance de SQL Server, un fichier MDX ou DMX, s'il est ouvert dans la même instance de SSMS, ne peut pas se connecter à une instance du serveur AS. De la même manière, si un fichier MDX ou DMX est déjà ouvert dans l'éditeur dans SSMS et connecté à une instance du serveur AS, un fichier .sql, s'il est ouvert dans la même instance de SSMS, ne peut pas se connecter à une instance de SQL Server.  
  
**Solution de contournement** :  pour résoudre ce problème, choisissez l’une des méthodes suivantes.  
  
-   Démarrez une autre instance de SSMS pour ouvrir le fichier MDX/DMX.  
  
-   Déconnectez l'éditeur SQL, puis connectez l'éditeur MDX/DMX à un serveur AS.  
  
### <a name="22-cannot-create-or-open-tabular-projects-when-builtinadministrators-group-name-cannot-be-resolved"></a>2.2 Impossible de créer ou d'ouvrir des projets tabulaires lorsque le nom de groupe BUILTIN\Administrateurs ne peut pas être résolu  
**Problème :** Pour pouvoir créer ou ouvrir des projets tabulaires, vous devez être administrateur de la base de données d’espace de travail. Un utilisateur peut être ajouté au groupe d'administrateurs du serveur en ajoutant le nom de l'utilisateur ou du groupe. Si vous appartenez au groupe BUILTIN\Administrateur, vous ne pouvez pas créer ni éditer les fichiers BIM, à moins que le serveur de base de données d'espace de travail soit joint au domaine à partir duquel il a été initialement configuré. Si vous ouvrez ou créez un fichier BIM, ce dernier échouera et le message suivant s'affichera :  
  
`"The BIM file cannot be opened. The server connected to is not valid. Reason: You are not an administrator of server [server name]."`  
  
**Solutions de contournement :**  
  
-   Joignez de nouveau le serveur de base de données d'espace de travail et l'ordinateur SQL Server Data Tools (SSDT) au domaine.  
  
-   Si le serveur de base de données d'espace de travail et/ou les ordinateurs SSDT ne sont pas joints au domaine de manière permanente, ajoutez des noms d'utilisateur individuels à la place du groupe BUILTIN\Administrateurs comme administrateurs du serveur de base de données d'espace de travail.  
  
### <a name="23-ssis-components-for-as-tabular-models-do-not-work-as-expected"></a>2.3 Les composants SSIS des modèles tabulaires AS ne fonctionnent pas comme prévu  
Les composants SQL Server Integration Services (SSIS) pour Analysis Services (AS) ne fonctionnent pas comme prévu pour les modèles tabulaires. Voici quelques problèmes connus qui peuvent survenir lorsque vous essayez d'écrire un package SSIS en vue d'une utilisation avec des modèles tabulaires.  
  
**Problème :** le gestionnaire de connexions AS ne peut pas utiliser de modèle tabulaire dans la même solution que la source de données.  
  
**Solution de contournement :** vous devez vous connecter explicitement au serveur AS avant de configurer la tâche de traitement AS ou la tâche DDL d’exécution AS.  
  
Il existe un certain nombre de problèmes avec la tâche de traitement AS lorsque vous travaillez avec des modèles tabulaires :  
  
**Problème :** à la place des bases de données, tableaux et partitions, figurent des cubes, des groupes de mesures et des dimensions. Il s'agit d'une limitation de la tâche.  
  
**Solution de contournement :** vous pouvez toujours traiter votre modèle tabulaire à l’aide de la structure cube/groupe de mesures/dimension.  
  
**Problème :** certaines options de traitement prises en charge par AS en mode tabulaire ne sont pas disponibles dans la tâche de traitement AS, telle que Traiter la défragmentation.  
  
**Solution de contournement :** utilisez à la place la tâche DDL d’exécution Analysis Services pour exécuter un script XMLA qui contient la commande ProcessDefrag.  
  
**Problème :** certaines options de configuration de l’outil ne sont pas applicables. Par exemple, l'option « Traiter les objets connexes » ne doit pas être utilisée lors du traitement des partitions, et l'option de configuration « Traitement parallèle » contient un message d'erreur non valide mentionnant que le traitement parallèle n'est pas pris en charge sur la référence standard.  
  
**Solution de contournement :** None  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="30-books-online"></a><a name="BOL"></a>3.0 Documentation en ligne  
  
### <a name="31-help-viewer-for-sql-server-crashes-in-environments-configured-to-run-only-ipv6"></a>3.1 La visionneuse de l'aide pour SQL Server se bloque dans les environnements configurés pour exécuter uniquement IPv6  
**Problème** : si votre environnement est configuré pour exécuter uniquement IPv6, la visionneuse de l’aide pour SQL Server 2012 se bloque et vous obtenez le message d’erreur suivant :  
  
`HelpLibAgent.exe has stopped working.`  
  
> [!IMPORTANT]  
> Cela s'applique à tous les environnements qui s'exécutent uniquement avec IPv6. Les environnements activés pour IPv4 (et IPv4 avec IPv6) ne sont pas concernés.  
  
**Solution de contournement** : pour résoudre ce problème, activez IPv4 ou suivez les étapes qui suivent pour ajouter une entrée de Registre et créer une liste de contrôle d’accès (ACL) afin d’activer la visionneuse de l’aide pour IPv6 :  
  
1.  Créez une clé de registre ayant pour nom « IPv6 » et pour valeur « 1 (DWORD(32 bit)) » sous HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Help\v1.0.  
  
2.  Définissez l’ACL de sécurité pour le port d’IPv6, en exécutant la commande qui suit à partir d’une fenêtre CMD d’administration :  
  
    ```  
    netsh http add urlacl url=https://[::1]:47873/help/ sddl=D:(A;;GX;;;WD)  
    ```  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="40-data-quality-services"></a><a name="DQS"></a>4.0 Data Quality Services  
  
### <a name="41-dqs-not-supported-in-a-cluster"></a>4.1 DQS non pris en charge dans un cluster  
**Problème :** DQS n’est pas pris en charge dans une installation de cluster SQL Server. Si vous installez une instance de cluster de SQL Server, vous ne devez pas sélectionner les cases à cocher **Data Quality Services** et **Data Quality Client** sur la page **Sélection des fonctionnalités** . Si ces cases à cocher sont sélectionnées au cours de l'installation de l'instance de cluster (et que vous avez terminé l'installation de Data Quality Server en exécutant le fichier DQSInstaller.exe), DQS est installé sur ce nœud, mais n’est pas disponible sur les nœuds supplémentaires lorsque vous ajoutez d'autres nœuds au cluster. De ce fait, il ne fonctionne pas sur ces nœuds supplémentaires.  
  
**Solution de contournement :** pour résoudre ce problème, installez la mise à jour cumulative 1 de SQL Server 2012. Pour obtenir des instructions, consultez [https://support.microsoft.com/kb/2674817](https://support.microsoft.com/kb/2674817).  
  
### <a name="42-to-reinstall-data-quality-server-delete-the-dqs-objects-after-uninstalling-data-quality-server"></a>4.2 Pour réinstaller Data Quality Server, supprimez les objets DQS après avoir désinstallé Data Quality Server  
**Problème :** si vous désinstallez Data Quality Server, les objets DQS (bases de données DQS, connexions DQS et une procédure stockée DQS) ne sont pas supprimés de l’instance SQL Server.  
  
**Solution de contournement :** pour réinstaller Data Quality Server sur le même ordinateur et dans la même instance SQL Server, vous devez supprimer manuellement les objets DQS de l’instance SQL Server. Par ailleurs, vous devez également supprimer les fichiers de bases de données DQS (DQS_MAIN, DQS_PROJECTS et DQS_STAGING_DATA) du dossier C:\Program Files\Microsoft SQL Server\MSSQL11.<SQL_Server_Instance>\MSSQL\DATA de votre ordinateur avant de réinstaller Data Quality Server. Sinon, l'installation de Data Quality Server échoue. Déplacez les fichiers de base de données au lieu de les supprimer si vous souhaitez conserver des données, telles que les bases de connaissances ou les projets de qualité des données. Pour plus d'informations sur la suppression des objets DQS une fois la désinstallation terminée, consultez [Supprimer les objets serveur DQS](./install/remove-data-quality-server-objects.md).  
  
### <a name="43-indication-of-a-terminated-knowledge-discovery-or-interactive-cleansing-activity-is-delayed"></a>4.3 Indication d'une découverte de connaissances terminée ou retard d'une activité de nettoyage interactif  
**Problème :** si un administrateur met fin à une activité dans l’écran Analyse des activités, un utilisateur interactif exécutant la découverte des connaissances, la gestion de domaine ou une activité de nettoyage interactif ne reçoit aucune indication selon laquelle son activité est terminée tant qu’il n’a pas effectué l’opération suivante.  
  
**Solution de contournement :** None  
  
### <a name="44-a-cancel-operation-discards-work-from-multiple-activities"></a>4.4 Une opération d'annulation entraine l'abandon de plusieurs activités  
**Problème :** Si vous cliquez sur **Annuler** pour une activité de découverte des connaissances ou de gestion de domaine en cours d’exécution et que d’autres activités ont été terminées précédemment sans publication pendant l’exécution de l’activité, le travail effectué par toutes les activités depuis la dernière publication est abandonné, et pas uniquement le travail actuel.  
  
**Solution de contournement :** pour éviter ce problème, publiez le travail que vous souhaitez conserver dans la base de connaissances avant de démarrer une nouvelle activité.  
  
### <a name="45-controls-do-not-scale-properly-on-large-font-sizes"></a>4.5 Les contrôles ne sont pas mis à l'échelle correctement sur les polices de grande taille  
**Problème :** Si vous agrandissez la taille du texte à 150 % ou plus (dans Windows Server 2008 ou Windows 7) ou si vous changez le paramètre Échelle personnalisée à 200 % (dans Windows 7), les boutons **Annuler** et **Créer** de la page **Nouvelle Base de connaissances** ne sont plus accessibles.  
  
**Solution de contournement :** Pour résoudre ce problème, attribuez une valeur plus petite à la police.  
  
### <a name="46-screen-resolution-of-800x600-is-not-supported"></a>4.6 La résolution d'écran 800x600 n'est pas prise en charge  
**Problème :** l’application Data Quality Client ne s’affiche pas correctement si la résolution d’écran est de 800x600.  
  
**Solution de contournement :** pour résoudre ce problème, définissez la résolution d’écran avec une valeur plus élevée.  
  
### <a name="47-map-bigint-column-in-the-source-data-to-a-decimal-domain-to-prevent-data-loss"></a>4.7 Mapper la colonne Bigint dans les données source vers un domaine décimal pour éviter la perte de données  
**Problème :** Si une colonne de vos données sources appartient au type de données **bigint**, vous devez mapper la colonne à un domaine du type de données **decimal** plutôt que du type de données **integer** dans DQS. La raison en est que le type de données **décimal** représente une plus grande plage de valeurs que le type de données **int** et, par conséquent, peut contenir des valeurs plus élevées.  
  
### <a name="48-nvarcharmax-and-varcharmax-data-types-are-not-supported-in-the-dqs-cleansing-component-in-integration-services"></a>4.8 Types de données NVARCHAR(MAX) et VARCHAR(MAX) non pris en charge dans le composant de nettoyage DQS d'Integration Services  
**Problème :** Les colonnes de données des types de données **nvarchar(max)** et **varchar(max)** ne sont pas prises en charge dans le composant Nettoyage DQS d’Integration Services. Par conséquent, ces colonnes de données ne sont pas disponibles pour le mappage dans l'onglet Mappage de l'Éditeur de transformation de nettoyage DQS et ne peuvent donc pas être nettoyées.  
  
**Solution de contournement :** Avant de traiter ces colonnes de données à l’aide du composant Nettoyage DQS, vous devez les convertir en type de données **DT_STR** ou **DT_WSTR** à l’aide de la transformation de conversion des données.  
  
### <a name="49-the-item-to-run-dqsinstallerexe-on-the-start-menu-is-overwritten-on-new-sql-server-instance-installation"></a>4.9 L'élément qui permet d'exécuter DQSInstaller.exe dans le menu Démarrer est remplacé dans l'installation de la nouvelle instance de SQL Server  
**Problème :** Si vous choisissez d’installer Data Quality Services dans une instance SQL Server, un élément est créé dans le menu **Démarrer** sous le groupe de programmes **Data Quality Services** appelé **Data Quality Server Installer** une fois que vous avez terminé l’installation de SQL Server. Toutefois, si vous installez plusieurs instances SQL Server sur le même ordinateur, un seul élément **Data Quality Server Installer** est présent dans le menu **Démarrer** . Si vous cliquez sur cet élément, le fichier DQSInstaller.exe est exécuté dans l'instance SQL Server installée en dernier.  
  
### <a name="410-activity-monitoring-displays-incorrect-status-for-failed-integration-services-cleansing-activities"></a>4.10 L'analyse des activités affiche un état incorrect pour les activités de nettoyage Integration Services ayant échoué  
L'écran Analyse des activités indique à tort l'état **Opération réussie** dans la colonne **État actuel** , même pour les activités de nettoyage Integration Services ayant échoué.  
  
### <a name="411-schema-name-is-not-displayed-as-part-of-tableview-name"></a>4.11 Le nom du schéma n'est pas affiché en tant que partie du nom de la table/de la vue  
Lors de la sélection d'une source de données SQL Server dans l'une des activités DQS pendant l'étape de mappage dans Data Quality Client, la liste des tables et des vues est affichée sans le nom du schéma. Par conséquent, si plusieurs tables/vues portent le même nom, mais des schémas différents, il est uniquement possible de les différencier en consultant l'aperçu des données, ou en les sélectionnant, puis en consultant les champs disponibles pour le mappage.  
  
### <a name="412-issue-with-cleansing-output-and-export-if-data-source-is-mapped-to-a-composite-domain-containing-a-child-domain-of-date-type"></a>4.12 Problème avec la sortie du nettoyage et l'exportation si la source de données est mappée à un domaine composite contenant un domaine enfant de type Date  
Dans un projet de qualité de nettoyage des données, si vous avez mappé un champ de vos données source à un domaine composite comportant un domaine enfant de type de données Date, le format de date de la sortie du domaine enfant dans les résultats de nettoyage n'est pas correct, et l'opération d'exportation vers la base de données échoue.  
  
### <a name="413-error-when-mapping-to-an-excel-sheet-that-contains-a--semicolon-in-its-name"></a>4.13 Erreur lors du mappage à une feuille Excel dont le nom contient un ; (point-virgule)  
**Problème :** Dans la page **Mappage** de toutes les activités DQS de Data Quality Client, si vous effectuez un mappage à une feuille source Excel dont le nom contient un point-virgule (;), un message d’exception non prise en charge s’affiche quand vous cliquez sur **Suivant** dans la page **Mappage**.  
  
**Solution de contournement :** supprimez le point-virgule (;) du nom de la feuille dans le fichier Excel qui contient les données source à mapper, puis réessayez.  
  
### <a name="414-issue-with-date-or-datetime-values-in-unmapped-source-fields-in-excel-during-cleansing-and-matching"></a>4.14 Problème avec les valeurs Date ou DateTime dans des champs sources non mappés dans Excel pendant le nettoyage et la mise en correspondance  
**Problème** : Si les données sources sont dans Excel et que vous n’avez pas mappé les champs sources contenant les valeurs de type de données **Date** ou **DateTime**, les événements suivants se produisent pendant les activités de nettoyage et de mise en correspondance :  
  
-   Les valeurs **Date** non mappées sont affichées et exportées au format aaaa-mm-jj.  
  
-   La valeur de temps est perdue pour les valeurs **DateTime** non mappées, qui sont affichées et exportées au format aaaa-mm-jj.  
  
**Solution de contournement :** Vous pouvez consulter les valeurs des champs non mappés dans le volet inférieur droit de la page **Gérer et afficher les résultats** de l’activité de nettoyage, et dans la page **Correspondance** de l’activité de correspondance.  
  
### <a name="415-cannot-import-domain-values-from-an-excel-file-xls-containing-more-than-255-columns-of-data"></a>4.15 Impossible d'importer des valeurs de domaine à partir d'un fichier Excel (.xls) contenant plus de 255 colonnes de données  
**Problème :** si vous importez des valeurs vers un domaine à partir d’un fichier Excel 97-2003 (.xls) contenant plus de 255 colonnes de données, un message d’exception s’affiche et l’importation échoue.  
  
**Solution de contournement :** pour résoudre ce problème, effectuez l’une des opérations suivantes :  
  
-   Enregistrer le fichier .xls en tant que fichier .xlsx, puis importer les valeurs du fichier .xlsx vers un domaine.  
  
-   Supprimer les données de toutes les colonnes au-delà de la colonne 255 dans le fichier .xls, enregistrer le fichier, puis importer les valeurs du fichier .xls vers un domaine.  
  
### <a name="416-activity-monitoring-feature-is-unavailable-for-roles-other-than-dqs_administrator"></a>4.16 La fonctionnalité de surveillance de l'activité n'est pas disponible pour les rôles autres que dqs_administrator  
La fonctionnalité de surveillance de l'activité est disponible uniquement pour les utilisateurs disposant du rôle dqs_administrator. Si votre compte d'utilisateur possède le rôle dqs_kb_editor ou dqs_kb_operator, la fonctionnalité de surveillance de l'activité n'est pas disponible dans l'application Data Quality Client.  
  
### <a name="417-error-on-opening-a-knowledge-base-in-the-recent-knowledge-base-list-for-domain-management"></a>4.17 Erreur lors de l'ouverture d'une base de connaissances dans la liste Base de connaissances récente pour la gestion de domaines  
Problème : Vous risquez de recevoir l’erreur suivante si vous ouvrez une base de connaissances de la liste **Base de connaissances récente** pour l’activité de gestion de domaines dans l’écran d’accueil de Data Quality Client :  
  
`"A configuration with name 'RecentList:KB:<domain>\<username>' already exists in the database."`  
  
Cela se produit en raison de la différence dans la manière dont DQS compare les chaînes dans C# et la base de données SQL Server. La comparaison des chaînes dans la base de données SQL Server ne respecte pas la casse, alors qu'elle le fait dans C#.  
  
Prenons un exemple pour illustrer ce propos. L'utilisateur Domaine\user1 L’utilisateur se connecte à l’ordinateur Data Quality Client à l’aide du compte « user1 » et travaille sur une base de connaissances. DQS stocke la base de connaissances récente pour chaque utilisateur sous forme d'un enregistrement dans la table A_CONFIGURATION de la base de données DQS_MAIN. Dans ce cas, l’enregistrement est stocké avec le nom suivant : RecentList:KB:Domain\user1. Par la suite, l’utilisateur se connecte à l’ordinateur Data Quality Client en tant que « User1 » (notez le U majuscule) et essaie d’ouvrir la base de connaissances dans la liste **Base de connaissances récente** pour l’activité de gestion de domaines. Le code sous-jacent dans DQS compare les deux chaînes, RecentList:KB:DOMAINE\user1 et DOMAINE\User1. Compte tenu du respect de la casse dans la comparaison des chaînes dans C#, les chaînes ne correspondent pas. Par conséquent, DQS essaie d’insérer un nouvel enregistrement pour l’utilisateur (User1) dans la table A_CONFIGURATION de la base de données DQS_MAIN. Toutefois, la comparaison des chaînes ne respectant pas la casse dans la base de données SQL, cette chaîne existe déjà dans la table A_CONFIGURATION de la base de données DQS_MAIN, ce qui fait que l'opération d'insertion échoue.  
  
**Solution de contournement :** pour résoudre ce problème, effectuez l’une des opérations suivantes :  
  
-   Vérifiez qu'il existe des entrées dupliquées en exécutant l'instruction suivante :  
  
    ```  
    SELECT * FROM DQS_MAIN.dbo.A_CONFIGURATION WHERE NAME like 'RecentList%'  
    ```  
  
    Exécutez ensuite l'instruction suivante pour supprimer l'enregistrement uniquement pour l'utilisateur concerné, en remplaçant la valeur dans la clause WHERE afin qu'elle corresponde au domaine et au nom d'utilisateur en question.  
  
    ```  
    DELETE DQS_MAIN.dbo.A_Configuration WHERE NAME LIKE 'RecentList%<domain>\<username>'  
    ```  
  
    Vous pouvez également supprimez tous les éléments récents pour tous les utilisateurs dans DQS :  
  
    ```  
    DELETE DQS_MAIN.dbo.A_Configuration WHERE NAME LIKE 'RecentList%'  
    ```  
  
-   Utilisez la même mise en majuscules que la dernière fois pour spécifier votre compte d'utilisateur lors de la connexion à l'ordinateur Data Quality Client.  
  
> [!NOTE]  
> Pour éviter ce problème, suivez des règles cohérentes concernant l'emploi des majuscules pour spécifier votre compte d'utilisateur lors de la connexion à l'ordinateur Data Quality Client.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="50-database-engine"></a><a name="DE"></a>5.0 Moteur de base de données  
  
### <a name="51-use-of-distributed-replay-controller-and-distributed-replay-client-features"></a>5.1 Utilisation des fonctionnalités Distributed Replay Controller et Distributed Replay Client  
**Problème :** les fonctionnalités Distributed Replay Controller et Distributed Replay Client sont mises à disposition dans la référence SKU Server Core de Windows Server 2008, Windows Server 2008 R2 et Windows Server 7, même si ces deux fonctionnalités ne sont pas prises en charge dans la référence SKU Server Core.  
  
**Solution de contournement :** n’installez pas et n’utilisez pas ces deux fonctionnalités dans la référence SKU Server Core de Windows Server 2008, Windows Server 2008 R2 et Windows Server 7.  
  
### <a name="52-sql-server-management-studio-depends-on-visual-studio-2010-sp1"></a>5.2 SQL Server Management Studio dépend de Visual Studio 2010 SP1  
**Problème** :  SQL Server 2012 Management Studio dépend de Visual Studio 2010 SP1 pour fonctionner correctement. La désinstallation de Visual Studio 2010 SP1 peut entraîner une perte de fonctionnalité dans SQL Server Management Studio et laisser Management Studio dans un état non pris en charge. Les problèmes suivants ont été constatés dans ce cas :  
  
-   Les paramètres de ligne de commande de ssms.exe ne fonctionnent pas correctement.  
  
-   Les informations d'aide affichées lorsque vous essayez d'exécuter ssms.exe avec le commutateur /? sont incorrectes.  
  
-   Pour chaque fichier ouvert par double-clic dans l'Explorateur Windows, une nouvelle instance de SSMS est lancée pour ouvrir le fichier.  
  
-   Il n'est pas possible de dépanner les requêtes en mode utilisateur normal.  
  
**Solution de contournement** : installez à nouveau Visual Studio 2010 SP1 et redémarrez Management Studio.  
  
### <a name="53-x64-operating-systems-require-64-bit-powershell-20"></a>5.3 Les systèmes d'exploitation x64 requièrent PowerShell 2.0 64 bits  
**Problème :** les installations 32 bits de Windows PowerShell Extensions pour SQL Server ne sont pas prises en charge pour les instances de SQL Server 2012 sur les systèmes d’exploitation 64 bits.  
  
**Solutions de contournement :**  
  
-   installez SQL Server 2012 64 bits avec les outils de gestion 64 bits et Windows PowerShell Extensions pour SQL Server 64 bits.  
  
-   Vous pouvez également importer le module SQLPS depuis l'invite Windows PowerShell 2.0 32 bits.  
  
### <a name="54-an-error-might-occur-when-navigating-in-the-generate-script-wizard"></a>5.4 Une erreur peut survenir lors de la navigation dans l'Assistant Générer un script  
**Problème :** Après avoir généré un script dans l’Assistant Générer un script en cliquant sur **Enregistrer ou publier des scripts**, puis navigué en cliquant sur **Choisir des options** ou **Définir les options de script**, le fait de recliquer sur **Enregistrer ou publier des scripts** peut entraîner l’erreur suivante :  
  
```
An exception occurred while executing a Transact-SQL statement or batch. (Microsoft.SqlServer.ConnectionInfo)  
------------------------------  
ADDITIONAL INFORMATION:  
Invalid object name 'sys.federations'. (Microsoft SQL Server, Error: 208)
```
  
**Solution de contournement :** fermez l’Assistant Génération de scripts, puis rouvrez-le.  
  
### <a name="55-new-maintenance-plan-layout-not-compatible-with-earlier-sql-server-tools"></a>5.5 La nouvelle mise en page des plans de maintenance n'est pas compatible avec les versions antérieures des outils SQL Server  
**Problème :** quand les outils de gestion de SQL Server 2012 sont utilisés pour modifier un plan de maintenance existant créé dans une version précédente des outils de gestion de SQL Server (SQL Server 2008 R2, SQL Server 2008 ou SQL Server 2005), le plan de maintenance est enregistré sous un nouveau format. Les versions précédentes des outils de gestion SQL Server ne prennent pas en charge ce nouveau format.  
  
**Solution de contournement** : None  
  
### <a name="56-intellisense-has-limitations-when-logged-in-to-a-contained-database"></a>5.6 Intellisense présente des restrictions lors de la connexion à une base de données autonome  
Problème : Intellisense dans SQL Server Management Studio (SSMS) et SQL Server Data Tools (SSDT) ne fonctionne pas comme attendu quand des utilisateurs contenus sont connectés à des bases de données autonomes. Il est possible d'observer le comportement suivant dans de tels cas :  
  
1.  Le soulignement des objets non valides n'apparaît pas.  
  
2.  La liste de saisie semi-automatique n'apparaît pas.  
  
3.  L'aide des info-bulles ne s'affiche pas dans les fonctions intégrées.  
  
**Solution de contournement** :  None  
  
### <a name="57-alwayson-availability-groups"></a>5.7 Groupes de disponibilité AlwaysOn  
Avant de créer un groupe de disponibilité, consultez [Conditions préalables, restrictions et recommandations pour les groupes de disponibilité AlwaysOn (SQL Server)](https://go.microsoft.com/?linkid=9753168) dans la documentation en ligne. Pour une introduction aux groupes de disponibilité AlwaysOn, consultez [Groupes de disponibilité AlwaysOn (SQL Server)](https://go.microsoft.com/?linkid=9753166)dans la documentation en ligne.  
  
#### <a name="571-client-connectivity-for-alwayson-availability-groups"></a>5.7.1 Connectivité du client pour les groupes de disponibilité AlwaysOn  
**Mise à jour le :** 13 août 2012  
  
Cette section décrit la prise en charge du pilote pour les groupes de disponibilité AlwaysOn et les solutions disponibles pour les pilotes non pris en charge.  
  
**Prise en charge de pilote**  
  
Le tableau suivant récapitule les pilotes pris en charge pour les groupes de disponibilité AlwaysOn :  
  
|Pilote|Basculement de sous-réseaux multiples|Intention de l'application|Routage en lecture seule|Basculement de sous-réseaux multiples : basculement plus rapide du point de terminaison d’un sous-réseau unique|Basculement de sous-réseaux multiples : résolution d’instance nommée pour les instances cluster SQL|  
|----------|--------------------------|----------------------|----------------------|------------------------------------------------------------------|---------------------------------------------------------------------------------|  
|SQL Native Client 11.0 ODBC|Oui|Oui|Oui|Oui|Oui|  
|SQL Native Client 11.0 OLEDB|Non|Oui|Oui|Non|Non|  
|ADO.NET avec .NET Framework 4.0 et correctif logiciel de connectivité **\&#42;**|Oui|Oui|Oui|Oui|Oui|  
|ADO.NET avec .NET Framework 3.5 SP1 et correctif logiciel de connectivité **\&#42;\&#42;**|Oui|Oui|Oui|Oui|Oui|  
|Microsoft JDBC Driver 4.0 pour SQL Server|Oui|Oui|Oui|Oui|Oui|  
  
**\&#42;** Télécharger le correctif logiciel de connectivité pour ADO .NET avec .NET Framework 4.0 : [https://support.microsoft.com/kb/2600211](https://support.microsoft.com/kb/2600211).  
  
**\&#42;\&#42;** Télécharger le correctif logiciel de connectivité pour ADO.NET avec .NET Framework 3.5 SP1 : [https://support.microsoft.com/kb/2654347](https://support.microsoft.com/kb/2654347).  
  
**Mot clé MultiSubnetFailover et fonctionnalités associées**  
  
MultiSubnetFailover est un nouveau mot clé de chaîne de connexion utilisé pour permettre un basculement plus rapide avec les groupes de disponibilité AlwaysOn et les instances de cluster de basculement AlwaysOn dans SQL Server 2012. Les trois sous-fonctionnalités suivantes sont activées lorsque MultiSubnetFailover=True est défini dans la chaîne de connexion :  
  
-   Basculement plus rapide de sous-réseaux multiples vers un écouteur de sous-réseaux multiples pour un groupe de disponibilité AlwaysOn ou des instances de cluster de basculement.  
  
    -   Résolution d'instance nommée pour une instance de cluster de basculement AlwaysOn de sous-réseaux multiples.  
  
-   Basculement plus rapide d'un sous-réseau unique vers un écouteur de sous-réseau unique pour un groupe de disponibilité AlwaysOn ou des instances de cluster de basculement.  
  
    -   Cette fonctionnalité est utilisée pour la connexion à un écouteur comportant une IP unique dans un seul sous-réseau. Elle permet d'exécuter des tentatives de connexion TCP plus agressives afin d'accélérer les basculements au sein d'un sous-réseau unique.  
  
-   Résolution d'instance nommée pour une instance de cluster de basculement AlwaysOn de sous-réseaux multiples.  
  
    -   Elle permet de prendre en charge la résolution d'instance nommée pour des instances de cluster de basculement AlwaysOn avec des points de terminaison de sous-réseaux multiples.  
  
**MultiSubnetFailover=True non pris en charge par NET Framework 3.5 ou OLEDB**  
  
**Problème :** Si votre groupe de disponibilité ou votre instance de cluster de basculement comporte un nom d’écouteur (également appelé « nom de réseau » ou « point d’accès client » dans le gestionnaire de cluster WSFC) qui dépend de plusieurs adresses IP de plusieurs sous-réseaux, et si vous utilisez soit ADO.NET avec le .NET Framework 3.5 SP1, soit SQL Native Client 11.0 OLEDB, 50 % de vos demandes de connexion client à l’écouteur du groupe de disponibilité sont susceptibles de dépasser le délai de connexion.  
  
**Solutions de contournement :** nous vous recommandons d’effectuer l’une des tâches suivantes.  
  
-   Si vous n'êtes pas autorisé à manipuler les ressources de cluster, définissez le délai de connexion à 30 secondes (cette valeur correspond au délai TCP de 20 secondes plus un tampon de 10 secondes).  
  
    **Avantages** : en cas de basculement de sous-réseaux croisé, la durée de récupération du client est courte.  
  
    **Inconvénients** : la moitié des connexions du client prendront plus de 20 secondes.  
  
-   Si vous disposez d'une autorisation vous permettant de manipuler les ressources du cluster, l'approche privilégiée consiste à définir le nom du réseau de votre écouteur de groupe de disponibilité en tant que **RegisterAllProvidersIP**=0. Pour plus d'informations, consultez la section « Exemple de script PowerShell pour désactiver RegisterAllProvidersIP et réduire TTL », plus loin dans cette rubrique.  
  
    **Avantages** : vous n’avez pas besoin d’augmenter la valeur du délai de connexion du client.  
  
    **Inconvénients :** en cas de basculement entre sous-réseaux, la durée de récupération du client peut être de 15 minutes ou plus, en fonction de votre paramètre HostRecordTTL et du paramètre de votre planification de la réplication DNS/AD intersites.  
  
**Exemple de script PowerShell pour désactiver RegisterAllProvidersIP et réduire TTL**  
  
L'exemple de script PowerShell suivant montre comment désactiver `RegisterAllProvidersIP` et réduire TTL. Remplacez `yourListenerName` par le nom de l'écouteur que vous modifiez.  
  
```  
Import-Module FailoverClusters  
Get-ClusterResource yourListenerName|Set-ClusterParameter RegisterAllProvidersIP 0  
Get-ClusterResource yourListenerName|Set-ClusterParameter HostRecordTTL 300  
```  
  
#### <a name="572-upgrading-from-ctp3-with-availability-group-configured-is-not-supported"></a>5.7.2 La mise à niveau à partir de CTP3 avec configuration du groupe de disponibilité n'est pas prise en charge  
Supprimez le groupe de disponibilité et recréez-le avant d'effectuer la mise à niveau. Il s'agit d'une limitation de la version CTP3. Les versions ultérieures ne comporteront pas cette limitation.  
  
#### <a name="573-side-by-side-installation-of-ctp3-with-later-versions-is-not-supported-if-you-have-an-availability-group-configured-in-your-instance"></a>5.7.3 L'installation côte à côte de CTP3 et de versions ultérieures n'est pas prise en charge si un groupe de disponibilité est configuré dans votre instance.  
Il s'agit d'une limitation de la version CTP3. Les versions ultérieures ne comporteront pas cette limitation.  
  
#### <a name="574-side-by-side-installation-of-ctp3-with-later-versions-of-failover-cluster-instances-is-not-supported"></a>5.7.4 L'installation côte à côte de CTP3 et de versions ultérieures d'instances de cluster de basculement n'est pas prise en charge.  
Il s'agit d'une limitation de la version CTP3. Les versions ultérieures ne comporteront pas cette limitation. Pour procéder à la mise à niveau d'instances de cluster de basculement à partir de CTP3, vous devez impérativement procéder à la mise à niveau simultanée de toutes les instances d'un nœud.  
  
#### <a name="575--timeouts-may-occur-when-using-multi-ips-in-the-same-subnet-with-alwayson"></a>5.7.5 Des délais d'attente peuvent survenir lors de l'utilisation de plusieurs adresses IP dans le même sous-réseau avec AlwaysOn  
**Problème :** lors de l’utilisation de plusieurs adresses IP dans le même sous-réseau avec AlwaysOn, les clients peuvent parfois constater un délai d’attente. Cela se produit si l'adresse IP figurant en haut de la liste est incorrecte.  
  
**Solution de contournement :** utilisez 'multisubnetfailover = true' dans la chaîne de connexion.  
  
#### <a name="576-failure-to-create-new-availability-group-listeners-because-of-active-directory-quotas"></a>5.7.6 Échec de création des écouteurs de groupe de disponibilité en raison de quotas Active Directory  
**Problème :** La création d'un nouvel écouteur de groupe de disponibilité peut échouer parce que vous avez atteint un quota Active Directory pour le compte d'ordinateur participant du nœud de cluster. Pour plus d'informations, consultez [Comment faire pour dépanner le compte de service de cluster lorsqu'il modifie des objets ordinateur](https://support.microsoft.com/kb/307532) et [Quotas Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc904295(v=ws.10)).  
  
#### <a name="577-netbios-conflicts-because-availability-group-listener-names-use-an-identical-15-character-prefix"></a>5.7.7 Conflit avec NetBIOS du fait que les noms d'écouteur de groupe de disponibilité utilisent un préfixe de 15 caractères identique  
Si vous avez deux clusters WSFC qui sont contrôlés par le même annuaire Active Directory et que vous tentez de créer des écouteurs de groupe de disponibilité dans les deux clusters à l'aide de noms contenant plus de 15 caractères et un préfixe identique de 15 caractères, vous obtenez une erreur signalant que la ressource de nom de réseau virtuel ne peut pas être mise en ligne. Pour plus d'informations sur les règles de préfixe des noms DNS, consultez [Attribution de noms de domaine](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731265(v=ws.10)).  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="60-integration-services"></a><a name="IS"></a>6.0 Integration Services  
  
### <a name="61-the-change-data-capture-service-for-oracle-and-the-change-data-capture-designer-console-for-oracle"></a>6.1 Service de capture de données modifiées pour Oracle et console du concepteur de capture de données modifiées pour Oracle  
Le service de capture de données modifiées pour Oracle est un service Windows qui analyse les journaux des transactions Oracle et capture les modifications des tables d'intérêt Oracle dans des tables de modifications SQL Server. La console du concepteur CDC est utilisée pour développer et maintenir les instances Oracle CDC. Il s'agit d'un composant logiciel enfichable MMC (Microsoft Management Console).  
  
#### <a name="611-install-the-cdc-service-for-oracle-and-the-cdc-designer-for-oracle"></a>6.1.1 Installer le service CDC pour Oracle et le concepteur CDC pour Oracle  
**Problème :** le service CDC et le concepteur CDC ne sont pas installés dans le cadre de l’installation de SQL Server. Vous devez les installer manuellement sur un ordinateur qui satisfait aux conditions préalables et à la configuration requise décrites dans les fichiers d'aide à jour.  
  
**Solution de contournement :** Pour installer le service CDC pour Oracle, exécutez manuellement AttunityOracleCdcService.msi à partir du support d’installation de SQL Server. Pour installer la console du concepteur de capture de données modifiées, exécutez manuellement AttunityOracleCdcDesigner.msi à partir du support d'installation de SQL Server.  Les packages d'installation pour les versions x86 et x64 se trouvent dans .\Tools\AttunityCDCOracle\ sur le support d'installation de SQL Server.  
  
#### <a name="612-f1-help-functionality-points-to-incorrect-documentation-files"></a>6.1.2 La fonctionnalité d'aide avec la touche F1 renvoie à des fichiers de documentation incorrects  
**Problème :** vous ne pouvez pas accéder à la documentation d’aide correcte à partir de la liste déroulante accessible avec la touche F1 ou en cliquant sur le point d’interrogation (« ? ») dans les consoles Attunity. Ces méthodes renvoient à des fichiers chm incorrects.  
  
**Solution de contournement :** les fichiers chm corrects sont installés en même temps que le service CDC pour Oracle et le concepteur CDC pour Oracle. Pour visualiser le contenu correct de l'aide, lancez les fichiers chm directement depuis l'emplacement suivant : `%Program Files%\Change Data Capture for Oracle by Attunity\*.chm`.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="70-master-data-services"></a><a name="MDS"></a>7.0 Master Data Services  
  
### <a name="71-fixing-an-mds-installation-in-a-cluster"></a>7.1 Résolution des erreurs d'installation MDS dans un cluster  
**Problème :** Si vous installez une instance en cluster de la version RTM de SQL Server 2012 avec la case **Master Data Services** cochée, MDS est installé sur un nœud unique, mais n’est pas disponible et ne fonctionne donc pas sur les nœuds supplémentaires que vous ajoutez au cluster.  
  
**Solution de contournement** : pour résoudre ce problème, vous devez installer SQL Server 2012 Cumulative Release 1 (CU1) en procédant comme suit :  
  
1.  Assurez-vous qu'il n'existe aucune installation SQL/MDS.  
  
2.  téléchargez SQL Server 2012 CU1 dans un répertoire local.  
  
3.  Installez SQL Server 2012 avec la fonctionnalité MDS sur le nœud de cluster principal, puis installez SQL Server 2012 avec la fonctionnalité MDS sur des nœuds de cluster supplémentaires.  
  
Pour plus d’informations sur les problèmes et la réalisation des étapes ci-dessus, consultez [https://support.microsoft.com/kb/2683467](https://support.microsoft.com/kb/2683467).  
  
### <a name="72-microsoft-silverlight-5-required"></a>7.2 Microsoft Silverlight 5 est requis  
Pour travailler dans l'application Web Master Data Manager, Silverlight 5.0 doit être installé sur l'ordinateur client. Si vous n'avez pas la version requise de Silverlight, vous êtes invité à l'installer lorsque vous accédez à une partie de l'application Web qui l'utilise. Vous pouvez installer Silverlight 5 à partir de [https://go.microsoft.com/fwlink/?LinkId=243096](https://go.microsoft.com/fwlink/?LinkId=243096).  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="80-reporting-services"></a><a name="RS"></a>8.0 Reporting Services  
  
### <a name="81-reporting-services-connectivity-to-sql-server-pdw-requires-updated-drivers"></a>8.1 La connectivité Reporting Services à SQL Server PDW nécessite la mise à jour des pilotes  
La connectivité de SQL Server 2012 Reporting Services vers Microsoft SQL Server PDW Appliance Update 2 et ultérieur nécessite une mise à jour des pilotes de connectivité PDW. Pour plus d'informations, les utilisateurs de SQL Server PDW sont priés de contacter l'assistance technique de Microsoft.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="90-streaminsight"></a><a name="SI"></a>9.0 StreamInsight  
StreamInsight 2.1 est inclus dans SQL Server 2012. Une licence Microsoft SQL Server 2012 et .NET Framework 4.0 sont requis par StreamInsight 2.1. Il inclut un certain nombre de résolutions de bogues et d'amélioration des performances. Pour plus d’informations, consultez [Notes de publication de Microsoft StreamInsight 2.1](https://social.technet.microsoft.com/wiki/contents/articles/6539.aspx). Pour télécharger StreamInsight 2.1 séparément, consultez la [page de téléchargement de Microsoft StreamInsight 2.1](https://www.microsoft.com/download/details.aspx?id=30149) sur le Centre de téléchargement Microsoft.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="100-upgrade-advisor"></a><a name="UA"></a>10.0 Conseiller de mise à niveau  
  
### <a name="101-link-to-install-upgrade-advisor-is-not-enabled-on-chinese-hk-operating-systems"></a>10.1 Le lien permettant d'installer le Conseiller de mise à niveau n'est pas activé sur les systèmes d'exploitation chinois (HK)  
Problème : Quand vous essayez d’installer le Conseiller de mise à niveau sur des systèmes d’exploitation prenant en charge une version Windows en chinois (Hong-Kong SAR), le lien permettant l’installation du Conseiller de mise à niveau risque de ne pas être activé.  
  
**Solution de contournement** : Recherchez le fichier **SQLUA.msi** sur votre support SQL Server 2012 dans `\1028_CHT_LP\x64\redist\Upgrade Advisor` ou `\1028_CHT_LP\x86\redist\Upgrade Advisor`, selon l’architecture de votre système d’exploitation.  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")
