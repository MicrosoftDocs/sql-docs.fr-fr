---
title: Installer le service à l’aide de l’interface utilisateur graphique
description: Cet article fournit une procédure d’installation pas à pas d’une nouvelle instance de SQL Server avec l’Assistant Installation de configuration SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- installing SQL Server, steps
- Setup [SQL Server], steps
- SQL Server, installing
ms.assetid: 6ad23de1-2bab-4933-9122-c09f5565028d
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8bb4b3b0e70850702c34ff1c69b9c2fcd5f3c85b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342322"
---
# <a name="install-sql-server-from-the-installation-wizard-setup"></a>Installer SQL Server à partir de l’Assistant Installation (programme d’installation)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Cet article explique comment installer SQL Server avec l’Assistant Installation. Il s’applique à [!INCLUDE[SQLServer2016](../../includes/sssql16-md.md)] et [!INCLUDE[SQLServer2017](../../includes/sssql17-md.md)].

Cet article fournit une procédure d’installation pas à pas d’une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec l’Assistant Installation de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'Assistant Installation fournit une seule arborescence de fonctionnalités pour l'installation de tous les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin d’éviter de les installer individuellement. Pour installer les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] individuellement, consultez [Installer SQL Server](../../database-engine/install-windows/install-sql-server.md#individual-component-installation).  

Pour les autres façons d’installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez :  

* [Installer SQL Server à partir de l’invite de commandes](./install-sql-server-from-the-command-prompt.md)  
* [Installer SQL Server à l’aide d’un fichier de configuration](./install-sql-server-using-a-configuration-file.md)  
* [Installer SQL Server à l’aide de SysPrep](../../database-engine/install-windows/install-sql-server-using-sysprep.md)  
* [Créer un nouveau cluster de basculement SQL Server &#40;Installation&#41;](../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
* [Effectuer une mise à niveau de SQL Server à l’aide de l’Assistant Installation &#40;Installation&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)

## <a name="get-the-installation-media"></a>Obtenir le média d’installation

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]
  
## <a name="prerequisites"></a>Prérequis

Avant d’installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Planification d’une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md).  
  
> [!NOTE]  
> Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui a les autorisations de lecture et d'exécution sur le partage distant.  

::: moniker range=">=sql-server-2016 <=sql-server-2017"

###  <a name="install-patch-requirement"></a><a name="bkmk_ga_instalpatch"></a> Installer le correctif obligatoire

Microsoft a identifié un problème qui affecte les fichiers binaires du runtime Microsoft Visual C++ 2013 installés en tant que composants requis par SQL Server 2016 et 2017. Une mise à jour est disponible pour résoudre ce problème. Si cette mise à jour des fichiers binaires du runtime Visual C++ n’est pas installée, SQL Server risque de rencontrer des problèmes de stabilité dans certains scénarios. Avant d’installer SQL Server, suivez les instructions données dans les [notes de publication SQL Server](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch) pour voir si votre ordinateur nécessite un correctif pour les fichiers binaires du runtime Visual C++. 

Cela ne s’applique pas à [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)].

## <a name="to-install-sql-server-2016-and-2017"></a>Pour installer SQL Server 2016 et 2017  

1. Insérez le support d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans le dossier racine, double-cliquez sur **Setup.exe**. Pour effectuer l'installation à partir d'un partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur **Setup.exe**.  
  
1. L'Assistant Installation exécute le Centre d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour créer une nouvelle installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sélectionnez **Installation** dans la zone de navigation de gauche, puis sélectionnez **Nouvelle installation autonome [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou ajoutez des fonctionnalités à une installation existante**.  

1. Sur la page **Clé de produit (Product Key)** sélectionnez une option pour indiquer si vous installez une édition gratuite de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou une version de production avec une clé PID. Pour plus d’informations, consultez [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
   Pour continuer, sélectionnez **suivant**.

1. Sur la page **Termes du contrat de licence**, consultez le contrat de licence. Si vous acceptez, cochez la case **J’accepte les termes du contrat de licence**, puis sélectionnez **Suivant**.  
    
   > [!NOTE]
   > SQL Server transmet des informations sur votre expérience d’installation, ainsi que d’autres données de performances et d’utilisation pour aider Microsoft à améliorer le produit. Pour plus d’informations sur le traitement de données SQL Server et les contrôles de confidentialité, consultez la [déclaration de confidentialité](https://privacy.microsoft.com/privacystatement) et [Configurer SQL Server pour envoyer des commentaires à Microsoft](../../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md).

1. Dans la page **Règles globales**, l’installation passera automatiquement à la page **Mises à jour du produit** s'il n'existe aucune erreur de règle.  
  
1. La page **Microsoft Update** s'affiche si la case **Microsoft Update** dans **Panneau de configuration** > **Tous les éléments du Panneau de configuration** > **Windows Update** > **Modifier les paramètres** n’est pas cochée. En cochant la case **Microsoft Update** vous modifiez les paramètres de l’ordinateur pour inclure les dernières mises à jour pour tous les produits Microsoft lors de l’analyse des mises à jour de Windows.  

1. Dans la page **Mises à jour du produit**, les dernières mises à jour du produit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponibles s'affichent. Si aucune mise à jour du produit n'est découverte, l'installation n'affiche pas cette page et passe automatiquement à la page **Installer les fichiers d'installation**.  

1. Sur la page **Installer les fichiers d'installation**, l'installation fournit la progression du téléchargement, de l'extraction et de l'installation des fichiers d'installation. Si une mise à jour de l'installation est trouvée et est spécifiée à inclure, cette mise à jour est également installée. Si aucune mise à jour n’est trouvée, le programme d’installation progresse automatiquement.
  
1. Dans la page **Règles d’installation**, l’installation vérifie si des problèmes potentiels peuvent affecter l’exécution de l’installation. En cas d’échec, sélectionnez un élément dans la colonne **État** pour plus d’informations. Sinon, sélectionnez **Suivant**.

1. S’il s’agit de la première installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur la machine, l’installation ignore la page **Type d’installation** et passe directement à la page **Sélection de fonctionnalités**. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est déjà installé sur le système, vous pouvez utiliser la page **Type d’installation** pour choisir d’exécuter une nouvelle installation ou d’ajouter des fonctionnalités à une installation existante. Pour continuer, sélectionnez **suivant**.
  
1. Sur la page **Sélection de fonctionnalités**, sélectionnez les composants que vous voulez installer. Par exemple, pour installer une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)], sélectionnez **Services Moteur de base de données**.

    Une description de chaque groupe de composants apparaît dans le volet **Description du composant** une fois que vous avez sélectionné le nom de la fonctionnalité. Vous pouvez choisir n'importe quelle combinaison de cases à cocher. Pour plus d’informations, consultez [Éditions et composants de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) et [Éditions et composants de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).
  
     La configuration requise pour les fonctionnalités sélectionnées est affichée dans le volet **Configuration requise pour les composants sélectionnés** . L’installation installe les composants requis qui n'ont pas déjà été installés lors de l'étape d'installation décrite plus loin dans cette procédure.  
  
     Vous pouvez également spécifier un répertoire personnalisé pour les composants partagés en utilisant le champ situé au bas de la page **Sélection de fonctionnalités**. Pour modifier le chemin d’accès à l'installation des composants partagés, mettez à jour le chemin d'accès dans le champ situé en bas de la boîte de dialogue ou sélectionnez **Parcourir** afin d'accéder à un répertoire d'installation. Le chemin d'installation par défaut est [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)].  
  
     > [!NOTE]
     > Le chemin d'accès spécifié pour les composants partagés doit être un chemin d'accès absolu. Le dossier ne doit pas être compressé ni chiffré. Les lecteurs mappés ne sont pas pris en charge.  
  
     SQL Server utilise deux répertoires pour les fonctionnalités partagées :
  
     * Répertoire des fonctionnalités partagées  
     * Répertoire des fonctionnalités partagées (x86)  
  
     > [!NOTE]
     > Le chemin d'accès spécifié pour chacune des options ci-dessus doit être différent.  
  
1. La page **Règles de fonctionnalité** avancera automatiquement si toutes les règles passent.  
  
1. Dans la page **Configuration de l'instance**, spécifiez s'il faut installer une instance par défaut ou une instance nommée. Pour plus d’informations, consultez [Configuration d’instance](../../sql-server/install/instance-configuration.md#instance-configuration-page).  
  
     * **ID de l'instance** : Par défaut, le nom de l'instance est utilisé comme ID d'instance. Cet ID permet d'identifier les répertoires d'installation et les clés de Registre de votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le même comportement se produit pour les instances par défaut et les instances nommées. Pour une instance par défaut, le nom de l'instance et l'ID d'instance sont MSSQLSERVER. Pour utiliser un ID d’instance non défini par défaut, spécifiez une autre valeur dans la zone de texte **ID d’instance**.  
  
       > [!NOTE]  
       > Les instances autonomes classiques de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], qu’il s’agisse d’instances par défaut ou d’instances nommées, n’utilisent pas de valeur non définie par défaut pour l’ID d’instance.  
  
       Tous les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Services Pack et mises à niveau s'appliquent à chaque composant d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     * **Instances installées** : La grille affiche toutes les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui se trouvent sur l'ordinateur où l'installation s'exécute. Si une instance par défaut est déjà installée sur l'ordinateur, vous devez installer une instance nommée de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)].  
  
     Le workflow du reste de l’installation dépend des fonctionnalités que vous avez spécifiées pour votre installation. En fonction de vos sélections, il est possible que les pages ne soient pas toutes visibles. 

1. Si vous choisissez d’installer la fonctionnalité Polybase, la page **Configuration de PolyBase** est ajoutée à la configuration de SQL Server, affichée après la page **Configuration de l’instance**. PolyBase requiert la mise à jour 51 d’Oracle JRE 7 (au moins) et si elle n’a pas déjà été installée, votre installation sera bloquée. Sur la page **Configuration de PolyBase**, vous pouvez choisir d’utiliser le SQL Server comme une instance autonome de PolyBase ou vous pouvez utiliser ce SQL Server dans le cadre d’un groupe de scale-out PolyBase. Si vous choisissez d’utiliser le groupe de scale-out, vous devez spécifier une plage de ports allant jusqu’à 6 ports ou plus. 


1. Utilisez la page **Configuration du serveur - Comptes de service** pour spécifier les comptes de connexion des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les services réels configurés dans cette page dépendent des fonctionnalités que vous avez choisi d'installer. Pour plus d’informations sur les paramètres de configuration, consultez [Aide de l’assistant Installation](../../sql-server/install/instance-configuration.md#serverconfig).
  
     Vous pouvez attribuer le même compte de connexion à tous les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou configurer chaque compte de service individuellement. Vous pouvez également spécifier si les services démarrent automatiquement, sont démarrés manuellement ou sont désactivés. Nous vous recommandons de configurer des comptes de service individuellement afin de fournir les privilèges minimum pour chaque service. Assurez-vous que les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disposent des autorisations minimales qu’ils doivent avoir pour exécuter leurs tâches. Pour plus d’informations, consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Pour spécifier le même compte d'ouverture de session pour tous les comptes de service dans cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], fournissez les informations d'identification dans les champs en bas de page.  
  
    > [!IMPORTANT]  
    > [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  

    > [!NOTE]
    > À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], cochez la case **Accorder le privilège Effectuer une tâche de maintenance en volume au service Moteur de base de données SQL Server** pour permettre au compte de service [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] d’utiliser [l’initialisation instantanée de fichiers de base de données](../../relational-databases/databases/database-instant-file-initialization.md).
  
1. Utilisez la page **Configuration du serveur - Classement** pour spécifier les classements non définis par défaut pour [!INCLUDE[ssDE](../../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].    

   Le paramètre d’installation par défaut est déterminé par les paramètres régionaux du système d’exploitation. Le classement au niveau du serveur peut être modifié pendant l’installation, ou en modifiant les paramètres régionaux du système d’exploitation avant l’installation. Le classement par défaut est défini d’après la version disponible la plus ancienne associée à chaque ensemble de paramètres régionaux spécifiques. Cette approche est due à des raisons de compatibilité descendante. Par conséquent, il ne s’agit pas toujours du classement recommandé. Pour tirer pleinement parti des fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], modifiez les paramètres d’installation par défaut de façon à utiliser les classements Windows. Par exemple, pour les paramètres régionaux du système d’exploitation **Anglais (États-Unis)** (page de codes 1252), le classement par défaut lors de l’installation est **SQL_Latin1_General_CP1_CI_AS** et il peut être remplacé par le classement Windows le plus proche **Latin1_General_100_CI_AS_SC** équivalent.

   Pour plus d’informations, consultez [Classement et support Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
1. Utilisez la page **Configuration du moteur de base de données - Configuration du serveur** pour spécifier les options suivantes :  
  
    * **Mode de sécurité** : Sélectionnez **Authentification Windows** ou **Authentification en mode mixte** pour votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous sélectionnez **Mode d'authentification mixte**, vous devez fournir un mot de passe fort pour le compte administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégré.  
  
       Lorsque la connexion réussie d'un périphérique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est établie, le mécanisme de sécurité est le même pour l'authentification Windows et l’authentification en mode mixte. Pour plus d’informations, consultez la [page Configuration du moteur de base de données - Configuration du serveur](../../sql-server/install/instance-configuration.md#serverconfig).
  
    * **Administrateurs SQL Server** : Vous devez spécifier au moins un administrateur système pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour ajouter le compte sous lequel l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute, sélectionnez **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, sélectionnez **Ajouter** ou **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposent des privilèges d'administrateur pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Utilisez la page **Configuration du moteur de base de données - Répertoires de données** pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, sélectionnez **Suivant**.  
  
    > [!IMPORTANT]  
    > Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Pour plus d’informations, consultez la [page Configuration du moteur de base de données – Répertoires de données](../../sql-server/install/instance-configuration.md#datadir).

     Utilisez la page **Configuration du moteur de base de données - TempDB** pour configurer la taille des fichiers, le nombre de fichiers, les répertoires d’installation autres que par défaut et les paramètres de croissance des fichiers pour **tempDB**. Pour plus d’informations, consultez la [page Configuration du moteur de base de données - TempDB](../../sql-server/install/instance-configuration.md#tempdb). 

 
     Utilisez la page **Configuration du moteur de base de données - FILESTREAM** pour activer FILESTREAM pour votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez la [page Configuration du moteur de base de données - FILESTREAM](../../sql-server/install/instance-configuration.md#database-engine-configuration---filestream-page).  
  
1. Utilisez la page **Configuration de Analysis Services - Approvisionnement du compte** pour spécifier le mode serveur et les utilisateurs ou comptes qui ont les autorisations d’administrateur pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Le mode serveur détermine quel sous-systèmes de mémoire et de stockage sont utilisés sur le serveur. Différents types de solution s'exécutent dans différents modes serveur. Si vous envisagez d'exécuter les bases de données multidimensionnelles de cube sur le serveur, choisissez l'option par défaut du mode serveur **Exploration multidimensionnelle et de données**.

    Vous devez spécifier au moins un administrateur système pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] :

    * Pour ajouter le compte sous lequel l'installation de SQL Server s'exécute, sélectionnez **Ajouter l'utilisateur actuel**.

    * Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, sélectionnez **Ajouter** ou **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposent des privilèges d'administrateur pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].

    Pour plus d’informations sur le mode serveur et les autorisations d’administrateur, consultez la [page Configuration Analysis Services – Approvisionnement du compte](../../sql-server/install/instance-configuration.md#analysis-services-configuration---account-provisioning-page).

    Lorsque vous avez fini de modifier la liste, sélectionnez **OK**. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Une fois la liste complète, sélectionnez **Suivant**.

    Utilisez la page **Configuration de Analysis Services - Répertoires de données** pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, sélectionnez **Suivant**.  

    > [!IMPORTANT]  
    > Quand vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si vous spécifiez le même chemin d’accès au répertoire pour INSTANCEDIR et SQLUSERDBDIR, SQL Server Agent et la recherche en texte intégral ne démarrent pas en raison d’autorisations manquantes.  
    >  
    > Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

    Pour plus d’informations, consultez la [page Configuration Analysis Services – Répertoires de données](../../sql-server/install/instance-configuration.md#analysis-services-configuration---data-directories-page).  

1. Utilisez la page **Configuration de Distributed Replay Controller** pour spécifier les utilisateurs auxquels vous voulez octroyer des autorisations administratives sur le service Distributed Replay Controller. Les utilisateurs qui disposent d'autorisations administratives ont un accès illimité au service Distributed Replay Controller.  
  
     * Pour octroyer des autorisations d'accès au service Distributed Replay Controller à l’utilisateur qui exécute l’installation SQL Server, sélectionnez le bouton **Ajouter l'utilisateur actuel**.

     * Pour octroyer des autorisations d'accès au service Distributed Replay Controller à d’autres utilisateurs, sélectionnez le bouton **Ajouter**.

     * Pour supprimer des autorisations d'accès du service Distributed Replay Controller, sélectionnez le bouton **Supprimer**.

     * Pour continuer, sélectionnez **suivant**.  
  
1. Utilisez la page **Configuration de Distributed Replay Client** pour spécifier les utilisateurs auxquels vous voulez octroyer des autorisations administratives sur le service Distributed Replay Client. Les utilisateurs qui disposent d'autorisations administratives ont un accès illimité au service Distributed Replay Client.  
  
     * **Nom du contrôleur** est facultatif. La valeur par défaut est \<*blank*>. Entrez le nom du contrôleur avec lequel l'ordinateur client communiquera pour le service Distributed Replay Client :  
  
       * Si vous avez déjà configuré un contrôleur, entrez son nom lors de la configuration de chaque client.  
  
       * Si vous n'avez pas encore configuré de contrôleur, vous pouvez laisser le nom du contrôleur vide. Toutefois, vous devez entrer manuellement le nom du contrôleur dans le fichier de **configuration du client** .  
  
     * Spécifiez le **Répertoire de travail** du service Distributed Replay Client. Le répertoire de travail par défaut est \<*drive letter*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\WorkingDir\\.  
  
     * Spécifiez le **Répertoire des résultats** du service Distributed Replay Client. Le répertoire des résultats par défaut est \<*drive letter*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\ResultDir\\.  
  
     * Pour continuer, sélectionnez **suivant**.  
  
1. La page **Prêt pour l'installation** affiche une arborescence des options d'installation spécifiées durant l'installation. Dans cette page, l’installation indique si la fonctionnalité **Mise à jour du produit** est activée ou désactivée et la version de la mise à jour finale.  
  
     Sélectionnez **Installer** pour continuer. L’installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe d'abord les composants requis pour les fonctionnalités sélectionnées, puis installe les fonctionnalités sélectionnées.  
  
1. Au cours de l'installation, la page **Progression de l'installation** fournit des mises à jour de l'état de sorte que vous puissiez surveiller la progression de l'installation au fil de l'installation.  
  
1. Après l'installation, la page **Terminé** fournit un lien vers le fichier journal résumé de l'installation et d'autres remarques importantes.
  
    > [!IMPORTANT]
    > Assurez-vous de lire le message affiché de l'Assistant Installation à la fin de l'installation. Pour plus d'informations, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

    Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sélectionnez **Fermer**.  
  
1. Redémarrez l'ordinateur maintenant si vous êtes invité à le faire.

::: moniker-end

::: moniker range=">=sql-server-ver15"
## <a name="to-install-sql-server-2019"></a>Pour installer SQL Server 2019 
  
1. Insérez le support d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans le dossier racine, double-cliquez sur **Setup.exe**. Pour effectuer l'installation à partir d'un partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur **Setup.exe**.  
  
1. L'Assistant Installation exécute le Centre d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour créer une nouvelle installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sélectionnez **Installation** dans la zone de navigation de gauche, puis sélectionnez **Nouvelle installation autonome [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou ajoutez des fonctionnalités à une installation existante**.  

1. Sur la page **Clé de produit (Product Key)** sélectionnez une option pour indiquer si vous installez une édition gratuite de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou une version de production avec une clé PID. Pour plus d’informations, consultez [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-version-15.md).  
  
   Pour continuer, sélectionnez **suivant**.
  
1. Sur la page **Termes du contrat de licence**, consultez le contrat de licence. Si vous acceptez, cochez la case **J’accepte les termes du contrat de licence et la [déclaration de confidentialité](https://privacy.microsoft.com/privacystatement)** , puis sélectionnez **Suivant**.  

   > [!NOTE]
   > Si une clé de produit de licence Enterprise Server/CAL est entrée et que l’ordinateur a plus de 20 cœurs physiques, ou 40 cœurs logiques lorsque l’hyper-threading est activé, un avertissement s’affiche lors de l’installation. Vous pouvez quand même continuer l’installation en activant la case à cocher **Cochez cette case pour confirmer cette limite et continuer ou cliquez sur Précédent/Annuler pour entrer une licence de produit Enterprise Core prenant en charge la valeur maximale du système d'exploitation** ou cliquez sur **Précédent** et entrez une clé de licence prenant en charge le nombre maximal de processeurs du système d’exploitation.

   > [!NOTE]
   > SQL Server transmet des informations sur votre expérience d’installation, ainsi que d’autres données de performances et d’utilisation pour aider Microsoft à améliorer le produit. Pour plus d’informations sur le traitement de données SQL Server et les contrôles de confidentialité, consultez la [déclaration de confidentialité](https://privacy.microsoft.com/privacystatement) et [Configurer SQL Server pour envoyer des commentaires à Microsoft](../../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md).

1. Dans la page **Règles globales**, l’installation passera automatiquement à la page **Mises à jour du produit** s'il n'existe aucune erreur de règle.  
  
1. La page **Microsoft Update** s'affiche si la case **Microsoft Update** dans **Panneau de configuration** > **Tous les éléments du Panneau de configuration** > **Windows Update** > **Modifier les paramètres** n’est pas cochée. En cochant la case **Microsoft Update** vous modifiez les paramètres de l’ordinateur pour inclure les dernières mises à jour pour tous les produits Microsoft lors de l’analyse des mises à jour de Windows.  

1. Dans la page **Mises à jour du produit**, les dernières mises à jour du produit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponibles s'affichent. Si aucune mise à jour du produit n'est découverte, l'installation n'affiche pas cette page et passe automatiquement à la page **Installer les fichiers d'installation**.  

1. Sur la page **Installer les fichiers d'installation**, l'installation fournit la progression du téléchargement, de l'extraction et de l'installation des fichiers d'installation. Si une mise à jour de l'installation est trouvée et est spécifiée à inclure, cette mise à jour est également installée. Si aucune mise à jour n’est trouvée, le programme d’installation progresse automatiquement.
  
1. Dans la page **Règles d’installation**, l’installation vérifie si des problèmes potentiels peuvent affecter l’exécution de l’installation. En cas d’échec, sélectionnez un élément dans la colonne **État** pour plus d’informations. Sinon, sélectionnez **Suivant**.

1. S’il s’agit de la première installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur la machine, l’installation ignore la page **Type d’installation** et passe directement à la page **Sélection de fonctionnalités**. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est déjà installé sur le système, vous pouvez utiliser la page **Type d’installation** pour choisir d’exécuter une nouvelle installation ou d’ajouter des fonctionnalités à une installation existante. Pour continuer, sélectionnez **suivant**.
  
1. Sur la page **Sélection de fonctionnalités**, sélectionnez les composants que vous voulez installer. Par exemple, pour installer une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)], sélectionnez **Services Moteur de base de données**.

    Une description de chaque groupe de composants apparaît dans le volet **Description du composant** une fois que vous avez sélectionné le nom de la fonctionnalité. Vous pouvez choisir n'importe quelle combinaison de cases à cocher. Pour plus d’informations, consultez [Éditions et composants de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) et [Éditions et composants de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).
  
     La configuration requise pour les fonctionnalités sélectionnées est affichée dans le volet **Configuration requise pour les composants sélectionnés** . L’installation installe les composants requis qui n'ont pas déjà été installés lors de l'étape d'installation décrite plus loin dans cette procédure.  
  
     Vous pouvez également spécifier un répertoire personnalisé pour les composants partagés en utilisant le champ situé au bas de la page **Sélection de fonctionnalités**. Pour modifier le chemin d’accès à l'installation des composants partagés, mettez à jour le chemin d'accès dans le champ situé en bas de la boîte de dialogue ou sélectionnez **Parcourir** afin d'accéder à un répertoire d'installation. Le chemin d'installation par défaut est [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)].  
  
     > [!NOTE]
     > Le chemin d'accès spécifié pour les composants partagés doit être un chemin d'accès absolu. Le dossier ne doit pas être compressé ni chiffré. Les lecteurs mappés ne sont pas pris en charge.  
  
     SQL Server utilise deux répertoires pour les fonctionnalités partagées :
  
     * Répertoire des fonctionnalités partagées  
     * Répertoire des fonctionnalités partagées (x86)  
  
     > [!NOTE]
     > Le chemin d'accès spécifié pour chacune des options ci-dessus doit être différent.  
  
1. La page **Règles de fonctionnalité** avancera automatiquement si toutes les règles passent.  
  
1. Dans la page **Configuration de l'instance**, spécifiez s'il faut installer une instance par défaut ou une instance nommée. Pour plus d’informations, consultez [Configuration d’instance](../../sql-server/install/instance-configuration.md#instance-configuration-page).  
  
     * **ID de l'instance** : Par défaut, le nom de l'instance est utilisé comme ID d'instance. Cet ID permet d'identifier les répertoires d'installation et les clés de Registre de votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le même comportement se produit pour les instances par défaut et les instances nommées. Pour une instance par défaut, le nom de l'instance et l'ID d'instance sont MSSQLSERVER. Pour utiliser un ID d’instance non défini par défaut, spécifiez une autre valeur dans la zone de texte **ID d’instance**.  
  
       > [!NOTE]  
       > Les instances autonomes classiques de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], qu’il s’agisse d’instances par défaut ou d’instances nommées, n’utilisent pas de valeur non définie par défaut pour l’ID d’instance.  
  
       Tous les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Services Pack et mises à niveau s'appliquent à chaque composant d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     * **Instances installées** : La grille affiche toutes les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui se trouvent sur l'ordinateur où l'installation s'exécute. Si une instance par défaut est déjà installée sur l'ordinateur, vous devez installer une instance nommée de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)].  
  
     Le workflow du reste de l’installation dépend des fonctionnalités que vous avez spécifiées pour votre installation. En fonction de vos sélections, il est possible que les pages ne soient pas toutes visibles. 

1. À compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], Polybase ne requiert plus qu’Oracle JRE 7 Update 51 (au moins) soit préinstallé sur l’ordinateur avant l’installation de la fonctionnalité. Si vous choisissez d’installer la fonctionnalité Polybase, la page **Emplacement d’installation Java** est ajoutée à la configuration de SQL Server affichée après la page **Configuration de l’instance**. Sur la page d’emplacement de l’installation Java, vous pouvez choisir d’installer Azul Zulu Open JRE fourni avec l’installation de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ou de fournir l’emplacement d’un autre JRE ou JDK qui a déjà été installé sur l’ordinateur.

1. À compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], Java a été ajouté avec des extensions de langage. Si vous choisissez d’installer la fonctionnalité Java, la page **Emplacement d’installation de Java** est ajoutée à la fenêtre de dialogue de configuration de SQL Server, affichée après la page **Configuration de l’instance**. Sur la page d’**emplacement de l’installation Java**, vous pouvez choisir d’installer Zulu Open JRE fourni avec l’installation de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ou de fournir l’emplacement d’un autre JRE ou JDK qui a déjà été installé sur l’ordinateur.

1. Utilisez la page **Configuration du serveur - Comptes de service** pour spécifier les comptes de connexion des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les services réels configurés dans cette page dépendent des fonctionnalités que vous avez choisi d'installer. Pour plus d’informations sur les paramètres de configuration, consultez [Aide de l’assistant Installation](../../sql-server/install/instance-configuration.md#serverconfig).
  
     Vous pouvez attribuer le même compte de connexion à tous les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou configurer chaque compte de service individuellement. Vous pouvez également spécifier si les services démarrent automatiquement, sont démarrés manuellement ou sont désactivés. Nous vous recommandons de configurer des comptes de service individuellement afin de fournir les privilèges minimum pour chaque service. Assurez-vous que les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disposent des autorisations minimales qu’ils doivent avoir pour exécuter leurs tâches. Pour plus d’informations, consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Pour spécifier le même compte d'ouverture de session pour tous les comptes de service dans cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], fournissez les informations d'identification dans les champs en bas de page.  
  
    > [!IMPORTANT]  
    > [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  

    > [!NOTE]
    > À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], cochez la case **Accorder le privilège Effectuer une tâche de maintenance en volume au service Moteur de base de données SQL Server** pour permettre au compte de service [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] d’utiliser [l’initialisation instantanée de fichiers de base de données](../../relational-databases/databases/database-instant-file-initialization.md).
  
     Utilisez la page **Configuration du serveur - Classement** pour spécifier les classements non définis par défaut pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d’informations, consultez [Classement et support Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
1. Utilisez la page **Configuration du moteur de base de données - Configuration du serveur** pour spécifier les options suivantes :  
  
    * **Mode de sécurité** : Sélectionnez **Authentification Windows** ou **Authentification en mode mixte** pour votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous sélectionnez **Mode d’authentification mixte**, vous devez fournir un mot de passe fort pour le compte administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (sa) intégré.  
  
       Lorsque la connexion réussie d'un périphérique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est établie, le mécanisme de sécurité est le même pour l'authentification Windows et l’authentification en mode mixte. Pour plus d’informations, consultez la [page Configuration du moteur de base de données - Configuration du serveur](../../sql-server/install/instance-configuration.md#serverconfig).
  
    * **Administrateurs SQL Server** : Vous devez spécifier au moins un administrateur système pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour ajouter le compte sous lequel l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute, sélectionnez **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, sélectionnez **Ajouter** ou **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposent des privilèges d'administrateur pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Vous pouvez également ajouter un groupe de domaine Windows pour établir un groupe d’administrateurs SQL partagé dans Active Directory avec un accès sysadmin à SQL Server. 
  
     Utilisez la page **Configuration du moteur de base de données - Répertoires de données** pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, sélectionnez **Suivant**.  
  
    > [!IMPORTANT]  
    > Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Pour plus d’informations, consultez la [page Configuration du moteur de base de données – Répertoires de données](../../sql-server/install/instance-configuration.md#datadir).

     Utilisez la page **Configuration du moteur de base de données - TempDB** pour configurer la taille des fichiers, le nombre de fichiers, les répertoires d’installation autres que par défaut et les paramètres de croissance des fichiers pour **tempDB**. Pour plus d’informations, consultez la [page Configuration du moteur de base de données - TempDB](../../sql-server/install/instance-configuration.md#tempdb).

     Utilisez la page de **configuration [!INCLUDE[ssDE](../../includes/ssde-md.md)] - MaxDOP** pour spécifier votre degré maximal de parallélisme. Ce paramètre détermine le nombre de processeurs une seule instruction peut utiliser pendant l’exécution. La valeur recommandée est calculée automatiquement lors de l’installation. 
     
    > [!NOTE]  
    > Cette page est uniquement disponible dans le programme d’installation à compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]. 
    
    Pour plus d’informations, consultez la [page de configuration du moteur de base de données - MaxDOP](../../sql-server/install/instance-configuration.md?view=sql-server-ver15&preserve-view=true#maxdop). 

     Utilisez la page de **configuration du moteur de base de données - Mémoire** pour spécifier les valeurs **min server memory** and **max server memory** utilisées par cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] après le démarrage. Vous pouvez utiliser les valeurs par défaut, les valeurs recommandées calculées ou spécifiez manuellement vos propres valeurs une fois que vous avez choisi l’option **Recommandé**.
     
    > [!NOTE]  
    > Cette page est uniquement disponible dans le programme d’installation à compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]. 
    
    Pour plus d’informations, consultez la [page de configuration du moteur de base de données - Mémoire](../../sql-server/install/instance-configuration.md?view=sql-server-ver15&preserve-view=true#memory). 

     Utilisez la page **Configuration du moteur de base de données - FILESTREAM** pour activer FILESTREAM pour votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez la [page Configuration du moteur de base de données - FILESTREAM](../../sql-server/install/instance-configuration.md#database-engine-configuration---filestream-page).  
  
1. Utilisez la page **Configuration de Analysis Services - Approvisionnement du compte** pour spécifier le mode serveur et les utilisateurs ou comptes qui ont les autorisations d’administrateur pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Le mode serveur détermine quel sous-systèmes de mémoire et de stockage sont utilisés sur le serveur. Différents types de solution s'exécutent dans différents modes serveur. Si vous envisagez d'exécuter les bases de données multidimensionnelles de cube sur le serveur, choisissez l'option par défaut du mode serveur **Exploration multidimensionnelle et de données**.

    Vous devez spécifier au moins un administrateur système pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] :

    * Pour ajouter le compte sous lequel l'installation de SQL Server s'exécute, sélectionnez **Ajouter l'utilisateur actuel**.

    * Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, sélectionnez **Ajouter** ou **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposent des privilèges d'administrateur pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].

    Pour plus d’informations sur le mode serveur et les autorisations d’administrateur, consultez la [page Configuration Analysis Services – Approvisionnement du compte](../../sql-server/install/instance-configuration.md#analysis-services-configuration---account-provisioning-page).

    Lorsque vous avez fini de modifier la liste, sélectionnez **OK**. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Une fois la liste complète, sélectionnez **Suivant**.

    Utilisez la page **Configuration de Analysis Services - Répertoires de données** pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, sélectionnez **Suivant**.  

    > [!IMPORTANT]  
    > Quand vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si vous spécifiez le même chemin d’accès au répertoire pour INSTANCEDIR et SQLUSERDBDIR, SQL Server Agent et la recherche en texte intégral ne démarrent pas en raison d’autorisations manquantes.  
    >  
    > Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

    Pour plus d’informations, consultez la [page Configuration Analysis Services – Répertoires de données](../../sql-server/install/instance-configuration.md#analysis-services-configuration---data-directories-page).  

1. Utilisez la page **Configuration de Distributed Replay Controller** pour spécifier les utilisateurs auxquels vous voulez octroyer des autorisations administratives sur le service Distributed Replay Controller. Les utilisateurs qui disposent d'autorisations administratives ont un accès illimité au service Distributed Replay Controller.  
  
     * Pour octroyer des autorisations d'accès au service Distributed Replay Controller à l’utilisateur qui exécute l’installation SQL Server, sélectionnez le bouton **Ajouter l'utilisateur actuel**.

     * Pour octroyer des autorisations d'accès au service Distributed Replay Controller à d’autres utilisateurs, sélectionnez le bouton **Ajouter**.

     * Pour supprimer des autorisations d'accès du service Distributed Replay Controller, sélectionnez le bouton **Supprimer**.

     * Pour continuer, sélectionnez **suivant**.  
  
1. Utilisez la page **Configuration de Distributed Replay Client** pour spécifier les utilisateurs auxquels vous voulez octroyer des autorisations administratives sur le service Distributed Replay Client. Les utilisateurs qui disposent d'autorisations administratives ont un accès illimité au service Distributed Replay Client.  
  
     * **Nom du contrôleur** est facultatif. La valeur par défaut est \<*blank*>. Entrez le nom du contrôleur avec lequel l'ordinateur client communiquera pour le service Distributed Replay Client :  
  
       * Si vous avez déjà configuré un contrôleur, entrez son nom lors de la configuration de chaque client.  
  
       * Si vous n'avez pas encore configuré de contrôleur, vous pouvez laisser le nom du contrôleur vide. Toutefois, vous devez entrer manuellement le nom du contrôleur dans le fichier de **configuration du client** .  
  
     * Spécifiez le **Répertoire de travail** du service Distributed Replay Client. Le répertoire de travail par défaut est \<*drive letter*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\WorkingDir\\.  
  
     * Spécifiez le **Répertoire des résultats** du service Distributed Replay Client. Le répertoire des résultats par défaut est \<*drive letter*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\ResultDir\\.  
  
     * Pour continuer, sélectionnez **suivant**.  
  
1. La page **Prêt pour l'installation** affiche une arborescence des options d'installation spécifiées durant l'installation. Dans cette page, l’installation indique si la fonctionnalité **Mise à jour du produit** est activée ou désactivée et la version de la mise à jour finale.  
  
     Sélectionnez **Installer** pour continuer. L’installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe d'abord les composants requis pour les fonctionnalités sélectionnées, puis installe les fonctionnalités sélectionnées.  
  
1. Au cours de l'installation, la page **Progression de l'installation** fournit des mises à jour de l'état de sorte que vous puissiez surveiller la progression de l'installation au fil de l'installation.  
  
1. Après l'installation, la page **Terminé** fournit un lien vers le fichier journal résumé de l'installation et d'autres remarques importantes.
  
    > [!IMPORTANT]
    > Assurez-vous de lire le message affiché de l'Assistant Installation à la fin de l'installation. Pour plus d'informations, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

    Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sélectionnez **Fermer**.  
  
1. Redémarrez l'ordinateur maintenant si vous êtes invité à le faire.

::: moniker-end

## <a name="next-steps"></a>Étapes suivantes

[Configurez votre nouvelle[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installation](../../database-engine/configure-windows/database-engine-instances-sql-server.md).  
  
Pour réduire la surface d'exposition d'un système, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe et active de façon sélective les services et fonctionnalités clés. Pour plus d'informations, consultez [Configuration de la surface d'exposition](../../relational-databases/security/surface-area-configuration.md).  
  
## <a name="see-also"></a>Voir aussi
  
* [Valider une installation SQL Server](../../database-engine/install-windows/validate-a-sql-server-installation.md)  
* [Réparer une installation défectueuse de SQL Server](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)
* [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
* [Effectuer une mise à niveau de SQL Server à l’aide de l’Assistant Installation &#40;Installation&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
* [Installer SQL Server à partir de l’invite de commandes](./install-sql-server-from-the-command-prompt.md)