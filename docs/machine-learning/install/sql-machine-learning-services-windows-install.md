---
title: Installer sur Windows
description: Découvrez comment installer SQL Server Machine Learning Services sur Windows. Vous pouvez utiliser Machine Learning Services pour exécuter des scripts Python et R en base de données.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/17/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: 44993ae527e99d615a9be620e3d930aa520c8980
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839506"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-windows"></a>Installer SQL Server Machine Learning Services (Python et R) sur Windows

[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

Découvrez comment installer SQL Server Machine Learning Services sur Windows. Vous pouvez utiliser Machine Learning Services pour exécuter des scripts Python et R en base de données.

## <a name="pre-install-checklist"></a><a name="bkmk_prereqs"> </a> Liste de contrôle avant l’installation

+ Une instance du moteur de base de données est nécessaire. Vous ne pouvez pas installer uniquement les fonctionnalités Python ou R, même si vous pouvez les ajouter de façon incrémentielle à une instance existante.

+ Pour assurer la continuité de l’activité, Machine Learning Services prend en charge les [groupes de disponibilité Always On](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). Installez Machine Learning Services et configurez des packages sur chaque nœud.

+ L’installation de Machine Learning Services *n’est pas prise en charge* sur une [instance de cluster de basculement Always On](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md) dans SQL Server 2017. Elle est prise en charge avec SQL Server 2019 et ultérieur.
 
+ N’installez pas Machine Learning Services sur un contrôleur de domaine. La partie du programme d’installation dédiée à Machine Learning Services échouerait.

+ N’installez pas **Fonctionnalités partagées** > **Machine Learning Server (autonome)** sur l’ordinateur exécutant une instance en base de données. Un serveur autonome tentera d’accéder aux mêmes ressources, ce qui réduira les performances des deux installations.

+ L’installation côte à côte avec d’autres versions de Python et R est prise en charge, mais n’est pas recommandée. Elle est prise en charge, car l’instance SQL Server utilise ses propres copies des distributions R et Anaconda open source. Elle n’est pas recommandée, car l’exécution de code utilisant Python et R sur l’ordinateur SQL Server en dehors de SQL Server peut entraîner différents problèmes :
    
  + L’utilisation d’une bibliothèque et de fichiers exécutables différents entraînera des résultats incohérents par rapport à ce que vous exécutez dans SQL Server.
  + Les scripts R et Python exécutés dans des bibliothèques externes ne peuvent pas être gérés par SQL Server, ce qui entraîne un conflit de ressources.

::: moniker range=">=sql-server-ver15"
> [!NOTE]
> Machine Learning Services est installé par défaut sur les **clusters Big Data SQL Server**. Vous n’avez pas besoin de suivre les étapes décrites dans cet article si vous utilisez un **cluster Big Data**. Pour plus d’informations, consultez [Utiliser Machine Learning Services (Python et R) sur Clusters Big Data](../../big-data-cluster/machine-learning-services.md).
::: moniker-end

> [!IMPORTANT]
> Une fois l’installation terminée, veillez à suivre les étapes consécutives à la configuration décrites dans cet article. Ces étapes incluent l’activation de l’utilisation de scripts externes par SQL Server et l’ajout des comptes nécessaires pour que SQL Server exécute les travaux R et Python à votre place. Les modifications de configuration nécessitent généralement un redémarrage de l’instance ou du service Launchpad.

## <a name="get-the-installation-media"></a>Obtenir le média d’installation

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

::: moniker range="=sql-server-2017"
Pour plus d’informations sur les éditions SQL Server qui prennent en charge l’intégration Python et R avec Machine Learning Services, consultez [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).
::: moniker-end

::: moniker range="=sql-server-ver15"
Pour plus d’informations sur les éditions SQL Server qui prennent en charge l’intégration Python et R avec Machine Learning Services, consultez [Éditions et fonctionnalités prises en charge de SQL Server 2019 (15.x)](../../sql-server/editions-and-components-of-sql-server-version-15.md).
::: moniker-end

## <a name="run-setup"></a>Exécuter le programme d’installation

Dans le cas d'une installation locale, vous devez exécuter le programme d'installation en qualité d'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui a les autorisations de lecture et d'exécution sur le partage distant.

1. Démarrez l’Assistant Installation de SQL Server.
  
1. Sous l’onglet **Installation**, sélectionnez **Nouvelle installation autonome de SQL Server ou ajout de fonctionnalités à une installation existante**.

   ::: moniker range="=sql-server-2017"
   ![Nouvelle installation autonome SQL Server](media/2017setup-installation-page-mlsvcs.png)
   ::: moniker-end

   ::: moniker range="=sql-server-ver15"
   ![Nouvelle installation autonome SQL Server](media/2019setup-installation-page-mlsvcs.png)
   ::: moniker-end

1. Dans la page **Sélection de fonctionnalités** , sélectionnez les options suivantes :

   ::: moniker range="=sql-server-2017"

   - **Services Moteur de base de données**
     
     Pour utiliser R et Python avec SQL Server, vous devez installer une instance du moteur de base de données. Vous pouvez utiliser l’instance par défaut ou une instance nommée.

   - **Machine Learning Services (dans la base de données)**
     
     Cette option installe les services de base de données qui prennent en charge l’exécution de scripts R et Python.

   ::: moniker-end

   ::: moniker range="=sql-server-ver15"

   - **Services Moteur de base de données**
     
     Pour utiliser R ou Python avec SQL Server, vous devez installer une instance du moteur de base de données. Vous pouvez utiliser l’instance par défaut ou une instance nommée.

   - **Machine Learning Services (dans la base de données)**
     
     Cette option installe les services de base de données qui prennent en charge l’exécution de scripts R et Python.

   ::: moniker-end

   - **R**
     
     Cochez cette option pour ajouter les packages Microsoft R, l’interpréteur et R open source. 
     
   - **Python**
     
     Cochez cette option pour ajouter les packages Microsoft Python et l’exécutable Python 3.5, puis sélectionnez les bibliothèques à partir de la distribution Anaconda.
     
   ::: moniker range="=sql-server-ver15"
   Pour plus d’informations sur l’installation et l’utilisation de Java, consultez [Installer les extensions de langage SQL Server sur Windows](../../language-extensions/install/windows-java.md).
   ::: moniker-end
   
   ::: moniker range="=sql-server-2017"
   ![Options de fonctionnalité pour R et Python](media/2017setup-features-page-mls-rpy.PNG "Options d’installation pour R et Python")
   ::: moniker-end
   
   ::: moniker range="=sql-server-ver15"
   ![Options de fonctionnalité pour R et Python](media/2019setup-features-page-mls-rpy.png "Options d’installation pour R et Python")
   ::: moniker-end
   
   > [!NOTE]
   > 
   > Ne sélectionnez pas l’option **Machine Learning Server (autonome)** . L’option d’installation de Machine Learning Server sous **Fonctionnalités partagées** est destinée à une utilisation sur un ordinateur distinct.

::: moniker range="=sql-server-2017"

4. Sur la page **Accepter l’installation de Microsoft R Open**, sélectionnez **Accepter**, puis **Suivant**. 

Le contrat de licence couvre :
+ Microsoft R Open
+ Packages de base et outils R open source
+ Packages et fournisseurs de connectivité R améliorés de l’équipe de développement Microsoft.

1. Sur la page **Consentement pour installer Python**, sélectionnez **Accepter**, puis **Suivant**. Le contrat de licence Python open source couvre également Anaconda et les outils associés ainsi que de nouvelles bibliothèques Python de l’équipe de développement Microsoft.

   > [!NOTE]
   >  Si l’ordinateur que vous utilisez n’a pas accès à Internet, vous pouvez suspendre l’installation à ce stade pour télécharger les programmes d’installation séparément. Pour plus d’informations, consultez [Installer des composants de machine learning sans accès à Internet](../install/sql-ml-component-install-without-internet-access.md).

1. Dans la page **Prêt pour l’installation**, vérifiez que ces sélections sont incluses, puis choisissez **Installer**.
  
   + Services Moteur de base de données
   + Machine Learning Services (en base de données)
   + R, Python ou les deux

   Notez l’emplacement du dossier sous le chemin `..\Setup Bootstrap\Log` où sont stockés les fichiers config. Une fois l’installation terminée, vous pouvez passer en revue les composants installés dans le fichier de synthèse.

1. Si vous êtes invité redémarrer l’ordinateur après l’installation, faites-le dès à présent. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d'informations, consultez [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

::: moniker-end

::: moniker range="=sql-server-ver15"

1. Sur la page **Accepter l’installation de Microsoft R Open**, sélectionnez **Accepter**, puis **Suivant**. Ce contrat de licence couvre Microsoft R Open, qui comprend une distribution des packages de base et des outils R open source ainsi que des packages et fournisseurs de connectivité R améliorés de l’équipe de développement Microsoft.

2. Sur la page **Consentement pour installer Python**, sélectionnez **Accepter**, puis **Suivant**. Le contrat de licence Python open source couvre également Anaconda et les outils associés ainsi que de nouvelles bibliothèques Python de l’équipe de développement Microsoft.

3. Dans la page **Prêt pour l’installation**, vérifiez que ces sélections sont incluses, puis choisissez **Installer**.
  
   + Services Moteur de base de données
   + Machine Learning Services (en base de données)
   + R et/ou Python

   Notez l’emplacement du dossier sous le chemin `..\Setup Bootstrap\Log` où sont stockés les fichiers de configuration. Une fois l’installation terminée, vous pouvez passer en revue les composants installés dans le fichier de synthèse.

4. Si vous êtes invité à redémarrer l’ordinateur après l’installation, faites-le dès à présent. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d'informations, consultez [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

::: moniker-end

## <a name="set-environment-variables"></a>Définir des variables d’environnement

Pour l’intégration de fonctionnalités R uniquement, vous devez définir la variable d’environnement **MKL_CBWR** pour [garantir la cohérence de la sortie](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) des calculs d’Intel Math Kernel Library (MKL).

1. Dans le Panneau de configuration, cliquez sur **Système et sécurité** > **Système** > **Paramètres système avancés** > **Variables d’environnement**.

2. Créez une variable Utilisateur ou Système. 

   + Nommez la variable `MKL_CBWR`.
   + Définissez la valeur de la variable sur `AUTO`.

Cette étape nécessite un redémarrage du serveur. Si vous vous apprêtez à activer l’exécution de scripts, vous pouvez reporter le redémarrage jusqu’à ce que le travail de configuration soit complètement terminé.

<a name="bkmk_enableFeature"></a>

## <a name="enable-script-execution"></a>Activer l’exécution de scripts

1. Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. 

    > [!TIP]
    > Vous pouvez télécharger et installer la version appropriée à partir de cette page : [Téléchargez SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
    > 
    > Vous pouvez également utiliser [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md), qui prend en charge les requêtes et les tâches d’administration sur SQL Server.
  
2. Connectez-vous à l’instance sur laquelle vous avez installé Machine Learning Services, cliquez sur **Nouvelle requête** pour ouvrir une fenêtre de requête, puis exécutez la commande suivante :

    ```sql
    sp_configure
    ```

    La propriété `external scripts enabled` a normalement la valeur **0** à ce stade. Cette fonctionnalité est désactivée par défaut. Elle doit être activée de manière explicite par un administrateur pour que vous puissiez exécuter des scripts R ou Python.
    
3.  Pour activer la fonctionnalité d’exécution de scripts externes, exécutez l’instruction suivante :
    
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
    
    Si vous avez déjà activé la fonctionnalité pour le langage R, ne procédez pas à une deuxième configuration pour Python. La plateforme d’extensibilité sous-jacente prend en charge les deux langages.

## <a name="restart-the-service"></a>Redémarrez le service.

Une fois l’installation terminée, redémarrez le moteur de base de données.

Le redémarrage du service entraîne également le redémarrage automatique du service [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] associé.

Pour redémarrer le service, vous pouvez cliquer avec le bouton droit sur la commande **Redémarrer** pour l’instance dans SSMS, utiliser le panneau **Services** dans le Panneau de configuration ou utiliser le [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md).

## <a name="verify-installation"></a>Vérifier l'installation

Effectuez les étapes suivantes pour vérifier que tous les composants utilisés pour lancer un script externe sont en cours d’exécution.

1. Dans SQL Server Management Studio, ouvrez une nouvelle fenêtre de requête et exécutez la commande suivante :
    
   ```sql
   EXECUTE sp_configure  'external scripts enabled'
   ```

   La valeur **run_value** est égale à 1.
    
2. Ouvrez le panneau **Services** ou le Gestionnaire de configuration SQL Server, puis vérifiez que le service **SQL Server Launchpad** est en cours d’exécution. Vous devez disposer d’un service pour chaque instance du moteur de base de données sur laquelle R ou Python est installé. Pour plus d’informations sur le service, consultez [Framework d’extensibilité](../concepts/extensibility-framework.md). 
   
3. Si le service Launchpad est en cours d’exécution, vous pouvez exécuter des scripts Python et R simples pour vérifier que les runtimes de script externes peuvent communiquer avec SQL Server.

   Ouvrez une nouvelle fenêtre **Requête** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis exécutez un script de ce type :
   + Pour R
   
     ```sql
     EXEC sp_execute_external_script  @language =N'R',
     @script=N'
     OutputDataSet <- InputDataSet;
     ',
     @input_data_1 =N'SELECT 1 AS hello'
     WITH RESULT SETS (([hello] int not null));
     GO
     ```
     
   + Pour Python
     
     ```sql
     EXEC sp_execute_external_script  @language =N'Python',
     @script=N'
     OutputDataSet = InputDataSet;
     ',
     @input_data_1 =N'SELECT 1 AS hello'
     WITH RESULT SETS (([hello] int not null));
     GO
     ```
   
   **Résultats**

   L’exécution du script peut prendre un certain temps lors du premier chargement du runtime de script externe. Les résultats doivent se présenter comme suit :

   | hello |
   |----|
   | 1|

> [!NOTE]
> Les colonnes ou les titres utilisés dans le script Python ne sont pas retournés automatiquement. Pour ajouter des noms de colonne à votre sortie, vous devez spécifier le schéma du jeu de données de retour. Pour cela, utilisez le paramètre WITH RESULTS de la procédure stockée en nommant les colonnes et en spécifiant le type de données SQL.
>
> Par exemple, vous pouvez ajouter la ligne suivante pour générer un nom de colonne arbitraire : `WITH RESULT SETS ((Col1 AS int))`

::: moniker range=">=sql-server-2017"

<a name="apply-cu"></a>

## <a name="apply-updates"></a>Appliquer des mises à jour

### <a name="existing-installation"></a>Installation existante

Si vous avez ajouté Machine Learning Services à une instance SQL existante et que vous avez déjà appliqué une mise à jour cumulative (CU), les versions du moteur de base de données et de la fonctionnalité Machine Learning Services peuvent être différentes. Cela peut entraîner des erreurs ou un comportement inattendu. 

Procédez comme suit pour ramener Machine Learning Services à la même version que votre moteur de base de données.

1. Déterminez la mise à jour cumulative (CU) dont vous disposez pour le moteur de base de données. Exécutez cette instruction T-SQL :

   ```sql
   SELECT @@VERSION
   ```
 
   Voici un exemple de sortie de la mise à jour cumulative SQL Server 2019 (CU) 8 :
 
   *Microsoft SQL Server 2019 (RTM-CU8-GDR) (KB4583459) - 15.0.4083.2 (X64)   2 nov 2020 18:35:09   Copyright (C) 2019 Microsoft Corporation  Developer Edition (64 bits) sur Windows 10 Enterprise 10.0 <X64> (Build 19042: ) (Hypervisor)*

   Pour plus d’informations, consultez [Déterminer la version, l’édition et le niveau de mise à jour de SQL Server et de ses composants](https://docs.microsoft.com/troubleshoot/sql/general/determine-version-edition-update-level#machine-learning-services).

1. Téléchargez la [Mise à jour cumulative (Cu)](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) que vous avez installée pour le moteur de base de données.

1. Exécutez l’installation de la Mise à jour cumulative (CU) et suivez les instructions pour installer la CU pour Machine Learning Services.

### <a name="new-installation"></a>Nouvelle installation

Si vous installez Machine Learning Services avec une nouvelle installation du moteur de base de données SQL Server, nous vous recommandons d’appliquer la dernière mise à jour cumulative au moteur de base de données et aux composants du Machine Learning.

Sur les appareils connectés à Internet, les mises à jour cumulatives sont généralement appliquées par le biais de Windows Update. Cependant, vous pouvez également contrôler les mises à jour en effectuant les étapes ci-dessous. Quand vous appliquez la mise à jour au moteur de base de données, le programme d’installation extrait les mises à jour cumulatives pour toutes les fonctionnalités Python ou R que vous avez installées sur la même instance. 

Les serveurs non connectés nécessitent des étapes supplémentaires. Pour plus d’informations, consultez [Installation sur des ordinateurs sans accès à Internet > Appliquer des mises à jour cumulatives](sql-ml-component-install-without-internet-access.md#apply-cu).

1. Commencez avec une instance de base déjà installée : version initiale de SQL Server.

2. Accédez à la liste des mises à jour cumulatives : [Dernières mises à jour pour Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md).

3. Sélectionnez la mise à jour cumulative la plus récente. Un fichier exécutable est téléchargé et extrait automatiquement.

4. Exécutez le programme d'installation. Acceptez les termes du contrat de licence, puis, dans la page Sélection de fonctionnalités, passez en revue les fonctionnalités pour lesquelles des mises à jour cumulatives sont appliquées. Vous devez voir toutes les fonctionnalités installées pour l’instance actuelle, y compris les fonctionnalités de machine learning. Le programme d’installation télécharge les fichiers CAB nécessaires à la mise à jour de toutes les fonctionnalités.

   ![Résumé des fonctionnalités installées](media/cumulative-update-feature-selection.png)

5. Poursuivez avec l’Assistant en acceptant les termes du contrat de licence pour les distributions R et Python. 

::: moniker-end

## <a name="additional-configuration"></a>Configuration supplémentaire

Si l’étape de vérification des scripts externes réussit, vous pouvez exécuter des commandes R ou Python à partir de SQL Server Management Studio, de Visual Studio Code ou de tout autre client capable d’envoyer des instructions T-SQL au serveur.

Si une erreur s’est produite lors de l’exécution de la commande, passez en revue les étapes de configuration supplémentaires de cette section. Vous devrez peut-être apporter des configurations supplémentaires spécifiques au service ou à la base de données.

Au niveau de l’instance, ces configurations supplémentaires peuvent inclure :

* [Configurer le pare-feu pour SQL Server Machine Learning Services](../../machine-learning/security/firewall-configuration.md)
* [Activer des protocoles réseau supplémentaires](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
* [Activer des connexions à distance](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
* [Créer un nom de connexion pour SQLRUserGroup](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)
* [Gestion des quotas de disque](/windows/desktop/fileio/managing-disk-quotas) pour éviter que les scripts externes n’exécutent des tâches qui saturent l’espace disque

::: moniker range=">=sql-server-ver15"
Dans SQL Server 2019 sur Windows, le mécanisme d’isolation a changé. Ce mécanisme affecte **SQLRUserGroup**, les règles de pare-feu, l’autorisation de fichier et l’authentification implicite. Pour plus d’informations, consultez [Modifications de l’isolation pour Machine Learning Services](sql-server-machine-learning-services-2019.md).
::: moniker-end

<a name="bkmk_configureAccounts"></a> 
<a name="permissions-external-script"></a> 

Vous aurez peut-être besoin d’effectuer les mises à jour de configuration suivantes sur la base de données :

* [Accorder des autorisations utilisateur pour SQL Server Machine Learning Services](../../machine-learning/security/user-permission.md)

> [!NOTE]
> Plusieurs éléments déterminent si une configuration supplémentaire est nécessaire : votre schéma de sécurité, l’emplacement d’installation de SQL Server et la façon dont les utilisateurs sont supposés se connecter à la base de données et exécuter des scripts externes.

## <a name="suggested-optimizations"></a>Optimisations suggérées

Maintenant que tout fonctionne, vous souhaitez peut-être optimiser le serveur en vue de la prise en charge du machine learning ou installer un modèle de machine learning préalablement entraîné.

::: moniker range="=sql-server-2017"

### <a name="add-more-worker-accounts"></a>Ajouter des comptes de travail

Si vous pensez que de nombreux utilisateurs exécuteront des scripts simultanément, vous pouvez augmenter le nombre de comptes de travail attribués au service Launchpad. Pour plus d’informations, consultez [Mise à l’échelle de l’exécution simultanée de scripts externes dans SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).
::: moniker-end

### <a name="optimize-the-server-for-script-execution"></a>Optimiser le serveur pour l’exécution de scripts

Les paramètres par défaut pour la configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont destinés à optimiser l’équilibre du serveur pour un éventail de services pris en charge par le moteur de base de données, ce qui peut inclure les processus d’extraction, transformation et chargement (ETL, extract, transform, load), la création de rapports, l’audit et les applications qui utilisent les données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dans les paramètres par défaut, les ressources sont parfois restreintes ou limitées pour le machine learning, en particulier dans les opérations utilisant beaucoup de mémoire.

Pour vous assurer que les travaux de machine learning sont classés par ordre de priorité et correctement ressourcés, nous vous recommandons d’utiliser la fonctionnalité Resource Governor de SQL Server pour configurer un pool de ressources externes. Vous pouvez aussi modifier la quantité de mémoire allouée au moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou augmenter le nombre de comptes s’exécutant sous le service [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)].

- Pour configurer un pool de ressources afin de gérer des ressources externes, consultez [Créer un pool de ressources externes](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
- Pour modifier la quantité de mémoire réservée à la base de données, consultez [Options de configuration de la mémoire du serveur](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
- Pour modifier le nombre de comptes R qui peuvent être démarrés par [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)], consultez [Mise à l’échelle de l’exécution simultanée de scripts externes dans SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

Si vous utilisez l’édition Standard et que vous n’avez pas le composant Resource Governor, vous pouvez utiliser les vues de gestion dynamique, les événements étendus ainsi que la supervision des événements Windows pour mieux gérer les ressources serveur.

### <a name="install-additional-python-and-r-packages"></a>Installer des packages Python et R supplémentaires

Les solutions Python et R que vous créez pour SQL Server peuvent appeler des fonctions de base, des fonctions des packages propriétaires installés avec SQL Server et des packages tiers compatibles avec la version de R et Python open source installée par SQL Server.

Installez les packages que vous souhaitez utiliser à partir de SQL Server dans la bibliothèque par défaut utilisée par l’instance. Si vous avez installé R ou Python séparément sur l’ordinateur ou installé des packages dans des bibliothèques utilisateur, vous ne pouvez pas utiliser ces packages à partir de T-SQL.

Pour installer et gérer des packages supplémentaires, vous pouvez configurer des groupes d’utilisateurs afin de partager des packages individuellement pour chaque base de données ou configurer des rôles de base de données pour permettre aux utilisateurs d’installer leurs propres packages. Pour plus d’informations, consultez [Installer des packages Python](../package-management/install-additional-python-packages-on-sql-server.md) et [Installer de nouveaux packages R](../package-management/install-additional-r-packages-on-sql-server.md).

## <a name="next-steps"></a>Étapes suivantes

Les développeurs Python peuvent apprendre à utiliser Python avec SQL Server en effectuant les didacticiels suivants :

+ [Tutoriel Python : Prédire la location de ski avec régression linéaire dans SQL Server Machine Learning Services](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Tutoriel Python : Catégoriser des clients à l’aide de k-moyennes avec SQL Server Machine Learning Services](../tutorials/python-clustering-model.md)

Les développeurs peuvent démarrer avec quelques exemples simples et découvrir les principes de base du fonctionnement de R avec SQL Server. Pour l’étape suivante, consultez les liens suivants :

+ [Démarrage rapide : Exécuter R dans T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Tutoriel : Analytique dans la base de données pour les développeurs R](../tutorials/r-taxi-classification-introduction.md)
