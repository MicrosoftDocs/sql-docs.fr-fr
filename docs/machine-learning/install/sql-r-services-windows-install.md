---
title: Installer SQL Server 2016 R Services
titleSuffix: ''
description: Découvrez comment installer SQL Server 2016 R Services sur Windows. R Services vous permet d’exécuter des scripts R dans la base de données.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: contperf-fy20q4
monikerRange: =sql-server-2016
ms.openlocfilehash: 5a4aefa522486eda1a955254d33ca7dd312c6cc6
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878872"
---
# <a name="install-sql-server-2016-r-services"></a>Installer SQL Server 2016 R Services

[!INCLUDE[SQL Server 2016 only](../../includes/applies-to-version/sqlserver2016-only.md)]

Découvrez comment installer SQL Server 2016 R Services sur Windows. R Services vous permet d’exécuter des scripts R dans la base de données.

> [!NOTE]
> Dans SQL Server 2017 et les versions ultérieures, R est inclus dans [Machine Learning Services](../sql-server-machine-learning-services.md) avec Python. Si vous souhaitez intégrer R et que vous disposez de SQL Server 2017 ou d’une version ultérieure, consultez [Installation de SQL Server Machine Learning Services](sql-machine-learning-services-windows-install.md) pour ajouter la fonctionnalité.

<a name="bkmk_prereqs"></a>

## <a name="pre-install-checklist"></a>Liste de contrôle avant l’installation

+ Une instance du moteur de base de données est nécessaire. Il n’est pas possible d’installer uniquement R, seulement de l’ajouter de façon incrémentielle à une instance existante.

+ Pour assurer la continuité de l’activité, R Services prend en charge les [groupes de disponibilité Always On](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). Vous devez installer R Services et configurer des packages sur chaque nœud.

+ N’installez pas R Services sur une instance de cluster de basculement Always On SQL Server. Le mécanisme de sécurité servant à isoler les processus R n’est pas compatible avec un environnement d’instance de cluster de basculement Always On SQL Server.

+ N’installez pas R Services sur un contrôleur de domaine. La partie du programme d’installation dédiée à R échouerait.

+ N’installez pas **Fonctionnalités partagées** > **R Server (autonome)** sur l’ordinateur exécutant une instance en base de données.

+ L’installation côte à côte avec d’autres versions de R est prise en charge, mais non recommandée. En effet, l’instance SQL Server utilise ses propres copies de la distribution R open source. Cependant, l’exécution de code utilisant R sur l’ordinateur SQL Server en dehors de SQL Server peut provoquer différents problèmes :

  + La bibliothèque et le fichier exécutable utilisés et les résultats obtenus ne sont pas les mêmes qu’avec une exécution dans SQL Server.
  + SQL Server ne peut pas gérer les scripts R exécutés dans des bibliothèques externes, ce qui entraîne une contention de ressources.
  
> [!IMPORTANT]
> Une fois l’installation terminée, veillez à suivre les étapes supplémentaires consécutives à la configuration décrites dans cet article. Ces étapes incluent l’activation de l’utilisation de scripts externes par SQL Server et l’ajout des comptes nécessaires pour que SQL Server exécute les travaux R à votre place. Les modifications de configuration nécessitent généralement un redémarrage de l’instance ou du service Launchpad.

## <a name="get-the-installation-media"></a>Obtenir le média d’installation

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

<a name="bkmk_ga_instalpatch"></a>

### <a name="install-patch-requirement"></a>Installer le correctif obligatoire

Microsoft a identifié un problème avec la version spécifique des fichiers binaires Microsoft VC++ 2013 Runtime qui sont installés en tant que composants requis par SQL Server. Si cette mise à jour des fichiers binaires du runtime VC n’est pas installée, SQL Server risque de rencontrer des problèmes de stabilité dans certains scénarios. Avant d’installer SQL Server, suivez les instructions données dans [Notes de publication de SQL Server](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch) pour voir si votre ordinateur nécessite un correctif pour les fichiers binaires du runtime VC.  

<a name="bkmk2016top"></a>

## <a name="run-setup"></a>Exécuter le programme d’installation

Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui a les autorisations de lecture et d'exécution sur le partage distant.

1. Démarrez l’Assistant Installation de SQL Server 2016.

1. Sous l’onglet **Installation**, sélectionnez **Nouvelle installation autonome de SQL Server ou ajout de fonctionnalités à une installation existante**.

    ![Installer R Services (dans la base de données)](media/2016-setup-installation-rsvcs.png "Démarrer l’installation du moteur de base de données avec R Services")

1. Sur la page **Sélection des fonctionnalités**, sélectionnez les options suivantes :

    + Sélectionnez **Services Moteur de base de données**. Le moteur de base de données est requis dans chaque instance qui utilise le Machine Learning.
    + Sélectionnez **R Services (dans la base de données)** . Installe la prise en charge de l’utilisation de R dans la base de données.

    ![Sélection de fonctionnalités R Services](media/2016setup-rsvcs-features.png "Sélectionner ces fonctionnalités pour R Services dans la base de données")

    > [!IMPORTANT]
    > N’installez pas R Server et R Services en même temps. 

1. Sur la page **Consentement à l’installation de Microsoft R Open**, cliquez sur **Accepter**.
  
    Vous devez accepter le contrat de licence pour télécharger Microsoft R Open, qui comprend une distribution des packages de base et des outils R open source ainsi que des packages et fournisseurs de connectivité R améliorés de l’équipe de développement R de Microsoft.
  
1. Une fois que vous avez accepté le contrat de licence, le programme d’installation est préparé. Cliquez sur **Suivant** lorsque le bouton devient disponible.

1. Sur la page **Prêt pour l’installation**, vérifiez que les éléments suivants sont inclus, puis choisissez **Installer**.

    + Services Moteur de base de données
    + R Services (dans la base de données)

1. Si vous êtes invité redémarrer l’ordinateur après l’installation, faites-le dès à présent. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d'informations, consultez [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="set-environment-variables"></a>Définir des variables d’environnement

Pour l’intégration de fonctionnalités R uniquement, vous devez définir la variable d’environnement **MKL_CBWR** pour [garantir la cohérence de la sortie](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) des calculs d’Intel Math Kernel Library (MKL).

1. Dans le Panneau de configuration, cliquez sur **Système et sécurité** > **Système** > **Paramètres système avancés** > **Variables d’environnement**.

1. Créez une variable Utilisateur ou Système.

    + Nommez la variable `MKL_CBWR`.
    + Définissez la valeur de la variable sur `AUTO`.

Cette étape nécessite un redémarrage du serveur. Vous pouvez reporter le redémarrage jusqu’à ce que le travail de configuration soit complètement terminé.

<a name="bkmk_enableFeature"></a>

##  <a name="enable-script-execution"></a>Activer l’exécution de scripts

1. Ouvrez [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) ou [Azure Data Studio](../../azure-data-studio/what-is.md).

1. Connectez-vous à l’instance sur laquelle vous avez installé R Services, cliquez sur **Nouvelle requête** pour ouvrir une fenêtre Requête, puis exécutez la commande suivante :

   ```sql
   sp_configure
   ```

    La propriété `external scripts enabled` a normalement la valeur **0** à ce stade. En effet, la fonctionnalité est désactivée par défaut. Elle doit être activée explicitement par un administrateur pour que vous puissiez exécuter des scripts R.

1. Pour activer la fonctionnalité d’exécution de scripts externes, exécutez l’instruction suivante :
  
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
  
## <a name="restart-the-service"></a>Redémarrez le service.

Une fois l’installation terminée, redémarrez le moteur de base de données avant de poursuivre pour activer l’exécution de scripts.

Le redémarrage du service entraîne également le redémarrage automatique du service [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] associé.

Pour redémarrer le service, vous pouvez cliquer avec le bouton droit sur la commande **Redémarrer** pour l’instance dans SSMS ou utiliser le [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md).

## <a name="verify-installation"></a>Vérifier l'installation

Effectuez les étapes suivantes pour vérifier que tous les composants utilisés pour lancer un script externe sont en cours d’exécution.

1. Dans SQL Server Management Studio, ouvrez une nouvelle fenêtre de requête et exécutez la commande suivante :

    ```sql
    EXEC sp_configure 'external scripts enabled'
    ```

    La valeur **run_value** doit maintenant être définie sur 1.

1. Ouvrez le Gestionnaire de configuration SQL Server, puis vérifiez que le **service SQL Server Launchpad** est en cours d’exécution. Vous devez disposer d’un service pour chacune des instances du moteur de base de données sur lesquelles R est installé. Pour plus d’informations sur le service, consultez [Framework d’extensibilité](../concepts/extensibility-framework.md).

1. Si le service Launchpad est en cours d’exécution, vous devez être en mesure d’exécuter des scripts R simples pour vérifier que les runtimes de script externes peuvent communiquer avec SQL Server.

    Ouvrez une nouvelle fenêtre **Requête** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou Azure Data Studio, puis exécutez un script de ce type :

    ```sql
    EXEC sp_execute_external_script  @language =N'R',
    @script=N'
    OutputDataSet <- InputDataSet;
    ',
    @input_data_1 =N'SELECT 1 AS hello'
    WITH RESULT SETS (([hello] int not null));
    GO
    ```

    L’exécution du script peut prendre un certain temps lors du premier chargement du runtime de script externe. Les résultats doivent se présenter comme suit :

    | hello |
    |----|
    | 1|

<a name="apply-cu"></a>

## <a name="apply-updates"></a>Appliquer des mises à jour

Nous vous recommandons d’appliquer le dernier Service Pack et la dernière mise à jour cumulative au moteur de base de données et aux composants de Machine Learning.

Sur les appareils connectés à Internet, les mises à jour cumulatives sont généralement appliquées par le biais de Windows Update. Cependant, vous pouvez également contrôler les mises à jour en effectuant les étapes ci-dessous. Quand vous appliquez la mise à jour au moteur de base de données, le programme d’installation extrait les mises à jour cumulatives pour toutes les bibliothèques R que vous avez installées sur la même instance. 

Sur les serveurs non connectés, des étapes supplémentaires sont nécessaires. Pour plus d’informations, consultez [Installation sur des ordinateurs sans accès à Internet > Appliquer des mises à jour cumulatives](sql-ml-component-install-without-internet-access.md#apply-cu).

1. Commencez avec une instance de base déjà installée : Version initiale de SQL Server 2016, SQL Server 2016 SP 1 ou SQL Server 2016 SP 2.

1. Accédez à la liste des mises à jour cumulatives : [Dernières mises à jour pour Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

1. Sélectionnez le dernier Service Pack (s’il n’est pas déjà installé en tant qu’instance de référence) et la dernière mise à jour cumulative. Un fichier exécutable est téléchargé et extrait automatiquement.

1. Exécutez le programme d'installation. Acceptez les termes du contrat de licence, puis, dans la page Sélection de fonctionnalités, passez en revue les fonctionnalités pour lesquelles des mises à jour cumulatives sont appliquées. Vous devez voir toutes les fonctionnalités installées pour l’instance actuelle, y compris R Services. Le programme d’installation télécharge les fichiers CAB nécessaires à la mise à jour de toutes les fonctionnalités.

1. Poursuivez avec l’Assistant en acceptant les termes du contrat de licence pour la distribution R.

> [!NOTE]
> La mise à jour cumulative (CU) 14 et les versions ultérieures pour SQL Server 2016 SP2 incluent une version plus récente du runtime R. Pour plus d’informations, consultez [Modifier la version du runtime de langage par défaut](change-default-language-runtime-version.md).

<a name="bkmk_FollowUp"></a>

## <a name="additional-configuration"></a>Configuration supplémentaire

Si l’étape de vérification des scripts externes réussit, vous pouvez exécuter des commandes R dans SQL Server Management Studio, Azure Data Studio ou tout autre client permettant d’envoyer des instructions T-SQL au serveur.

Si une erreur s’est produite lors de l’exécution de la commande, passez en revue les étapes de configuration supplémentaires de cette section. Vous devrez peut-être apporter des configurations supplémentaires spécifiques au service ou à la base de données.

Au niveau de l’instance, ces configurations supplémentaires peuvent inclure :

* [Configuration du pare-feu pour SQL Server Machine Learning Services](../../machine-learning/security/firewall-configuration.md)
* [Activation de protocoles réseau supplémentaires](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
* [Activation des connexions à distance](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
* [Gestion des quotas de disque](/windows/desktop/fileio/managing-disk-quotas) pour éviter que les scripts externes n’exécutent des tâches qui saturent l’espace disque

<a name="bkmk_configureAccounts"></a>
<a name="bkmk_AllowLogon"></a>

Vous aurez peut-être besoin d’effectuer les mises à jour de configuration suivantes sur la base de données :

* [Accorder des autorisations utilisateur pour SQL Server Machine Learning Services](../../machine-learning/security/user-permission.md)
* [Ajouter SQLRUserGroup comme utilisateur de base de données](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)

> [!NOTE]
> Toutes les modifications répertoriées ne sont pas requises, et il se peut qu’aucune ne le soit. Les exigences dépendent de votre schéma de sécurité, de l’emplacement d’installation de SQL Server et de la façon dont les utilisateurs sont supposés se connecter à la base de données et exécuter des scripts externes. Vous trouverez des conseils d’installation supplémentaires ici : [Installer SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md)

## <a name="suggested-optimizations"></a>Optimisations suggérées

Il peut également être intéressant d’optimiser le serveur de façon à prendre en charge le Machine Learning avec R ou d’installer des modèles préentraînés.

### <a name="add-more-worker-accounts"></a>Ajouter des comptes de travail

Si vous pensez utiliser R intensément, ou que de nombreux utilisateurs exécuteront des scripts simultanément, vous pouvez augmenter le nombre de comptes de travail affectés au service Launchpad. Pour plus d’informations, consultez [Mise à l’échelle de l’exécution simultanée de scripts externes dans SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

<a name="bkmk_optimize"></a>

### <a name="optimize-the-server-for-external-script-execution"></a>Optimiser le serveur pour l’exécution de scripts externes

Les paramètres par défaut pour la configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont destinés à optimiser l’équilibre du serveur pour un éventail de services pris en charge par le moteur de base de données, ce qui peut inclure les processus d’extraction, transformation et chargement (ETL, extract, transform, load), la création de rapports, l’audit et les applications qui utilisent les données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ceci explique pourquoi, dans les paramètres par défaut, les ressources sont parfois restreintes ou limitées pour le machine learning, en particulier dans les opérations utilisant beaucoup de mémoire.

Pour vous assurer que les travaux de machine learning sont classés par ordre de priorité et correctement ressourcés, nous vous recommandons d’utiliser la fonctionnalité Resource Governor de SQL Server pour configurer un pool de ressources externes. Vous pouvez aussi modifier la quantité de mémoire allouée au moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou augmenter le nombre de comptes s’exécutant sous le service [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)].

- Pour configurer un pool de ressources afin de gérer des ressources externes, consultez [Créer un pool de ressources externes](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
- Pour modifier la quantité de mémoire réservée à la base de données, consultez [Options de configuration de la mémoire du serveur](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
- Pour modifier le nombre de comptes R qui peuvent être démarrés par [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)], consultez [Mise à l’échelle de l’exécution simultanée de scripts externes dans SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

Si vous utilisez l’édition Standard et que vous n’avez pas le composant Resource Governor, vous pouvez utiliser les vues de gestion dynamique, les événements étendus ainsi que la supervision des événements Windows pour mieux gérer les ressources serveur qui sont gérées par R.

### <a name="install-additional-r-packages"></a>Installer des packages R supplémentaires

Les solutions R que vous créez pour SQL Server peuvent appeler des fonctions R de base, des fonctions des packages propriétaires installés avec SQL Server et des packages R tiers compatibles avec la version de R open source installée par SQL Server.

Installez les packages que vous souhaitez utiliser à partir de SQL Server dans la bibliothèque par défaut qui est utilisée par l’instance. Si vous avez installé R séparément sur l’ordinateur, ou installé des packages dans des bibliothèques utilisateur, vous ne pourrez pas utiliser ces packages à partir de T-SQL.

Le processus d’installation et de gestion des packages R est différent dans SQL Server 2016 et SQL Server 2017. Dans SQL Server 2016, un administrateur de base de données doit installer les packages R dont les utilisateurs ont besoin. Dans SQL Server 2017, vous pouvez configurer des groupes d’utilisateurs afin de partager des packages individuellement pour chaque base de données ou configurer des rôles de base de données pour permettre aux utilisateurs d’installer leurs propres packages. Pour plus d’informations, consultez la page [Installer des packages avec les outils R](../package-management/install-r-packages-standard-tools.md).

## <a name="next-steps"></a>Étapes suivantes

Les développeurs peuvent démarrer avec quelques exemples simples et découvrir les principes de base du fonctionnement de R avec SQL Server. Pour l’étape suivante, consultez les liens suivants :

+ [Démarrage rapide : Exécuter R dans T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Tutoriels R pour l’apprentissage automatique SQL](../tutorials/r-tutorials.md)
+ [Documentation sur SQL Machine Learning](../index.yml)