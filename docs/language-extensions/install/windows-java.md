---
title: Installation de l’extension de langage Java sur Windows
titleSuffix: SQL Server Language Extensions
description: Découvrez comment installer la fonctionnalité Extension de langage Java SQL Server sur Windows.
author: dphansen
ms.author: davidph
ms.date: 11/11/2020
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 6974a44210f008807de5498d70083530d9b40757
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096859"
---
# <a name="install-sql-server-java-language-extension-on-windows"></a>Installation de l’extension de langage Java SQL Server sur Windows

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Découvrez comment installer le composant [Extension de langage Java](../java-overview.md) pour SQL Server sur Windows. L’extension de langage Java fait partie des [Extensions de langage SQL Server](../language-extensions-overview.md).

> [!NOTE]
> Cet article concerne l’installation de l’extension de langage Java pour SQL Server sur Windows. Pour Linux, consultez [Installation de l’extension de langage Java SQL Server sur Linux](../../linux/sql-server-linux-setup-language-extensions-java.md).

<a name="prerequisites"></a>

## <a name="pre-install-checklist"></a>Liste de contrôle avant l’installation

+ Le programme d’installation de SQL Server 2019 est nécessaire si vous souhaitez installer la prise en charge de l’extension de langage Java.

+ Une instance du moteur de base de données est nécessaire. Il n’est pas possible d’installer uniquement les fonctionnalités Extension de langage Java. Cependant, vous pouvez les ajouter de façon incrémentielle à une instance existante.

+ Pour assurer la continuité de l’activité, les extensions de langage prennent en charge les [groupes de disponibilité Always On](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). Vous devez installer les extensions de langage et configurer des packages sur chaque nœud.

+ L’installation de l’extension de langage Java est prise en charge sur un cluster de basculement dans SQL Server 2019.

+ N’installez pas les extensions de langage SQL Server ni l’extension de langage Java sur un contrôleur de domaine. La partie Extensions de langage de l’installation échouerait.

+ Les extensions de langage et [Machine Learning Services](../../machine-learning/index.yml) sont installés par défaut sur Clusters Big Data SQL Server. Si vous utilisez Clusters Big Data, vous n’avez pas besoin de suivre les étapes décrites dans cet article. Pour plus d’informations, consultez [Utiliser Machine Learning Services (Python et R) sur Clusters Big Data](../../big-data-cluster/machine-learning-services.md).

> [!IMPORTANT]
> Une fois l’installation terminée, veillez à suivre les étapes consécutives à la configuration décrites dans cet article. Vous devez notamment autoriser SQL Server à utiliser du code externe et ajouter les comptes nécessaires pour que SQL Server exécute du code Java en votre nom. Les modifications de configuration nécessitent généralement un redémarrage de l’instance ou du service Launchpad.

<a name="java-jre-jdk"></a>

## <a name="java-jre-or-jdk"></a>Java JRE ou JDK

Il existe deux façons d’installer et d’utiliser Java avec SQL Server :

1. Utiliser le runtime Java par défaut, Zulu Open JRE version 11.0.3. Ce runtime est pris en charge et inclus dans l’installation de SQL Server.

1. Utilisez votre distribution Java par défaut au lieu du runtime Java par défaut.

    Java 11 est actuellement la version prise en charge sur Windows. Le JRE (Java Runtime Environment) est le minimum nécessaire, mais le JDK (Java Development Kit) est utile si vous avez besoin du compilateur Java et de packages de développement. Le JDK étant « tout compris », son installation rend le JRE inutile. Sur Windows, nous vous recommandons d’installer le JDK dans le dossier par défaut `/Program Files/` si possible. Sinon, une configuration supplémentaire est nécessaire pour accorder des autorisations aux exécutables. Pour plus d’informations, consultez la section sur l’[octroi d’autorisations (Windows)](#perms-nonwindows) dans ce document.

    > [!NOTE]
    > Étant donné que Java est à compatibilité descendante, il est possible que les versions antérieures fonctionnent. Cependant, la version prise en charge et testée pour SQL Server 2019 est Java 11.

## <a name="get-the-installation-media"></a>Obtenir le média d’installation

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

## <a name="run-setup"></a>Exécuter le programme d’installation

Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui a les autorisations de lecture et d'exécution sur le partage distant.

1. Démarrez l’Assistant Installation de SQL Server 2019.
  
1. Sous l’onglet **Installation**, sélectionnez **Nouvelle installation autonome de SQL Server ou ajout de fonctionnalités à une installation existante**.

    ![Installation de SQL Server 2019](../media/sql-install.png) 

1. Dans la page **Sélection de fonctionnalités** , sélectionnez les options suivantes :
  
    - **Services Moteur de base de données**
  
        Pour utiliser les extensions de langage avec SQL Server, vous devez installer une instance du moteur de base de données. Vous pouvez utiliser l’instance par défaut ou une instance nommée.
  
    - **Machine Learning Services et extensions de langage**
  
        Cette option installe le composant Extensions de langage qui prend en charge l’exécution de code Java.

        - Si vous souhaitez installer le runtime Java par défaut, Zulu Open JRE 11.0.3, sélectionnez **Machine Learning Services et extensions de langage** et **Java**.

        - Si vous souhaitez utiliser votre propre runtime Java, sélectionnez **Machine Learning Services et extensions de langage**. Ne sélectionnez pas **Java**.

        Si vous souhaitez utiliser R et Python, consultez [Installer SQL Server Machine Learning Services sur Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md).

    ![Options des fonctionnalités pour les extensions de langage](../media/sql-install-feature-selection.png)

1. Si vous avez choisi **Java** à l’étape précédente pour installer le runtime Java par défaut, la page **Emplacement d’installation de Java** s’affiche.

    Sélectionnez **Installer Open JRE 11.0.3 inclus dans cette installation**.

    ![Choisir l’emplacement d’installation de Java](../media/sql-install-openjdk.png)

    > [!NOTE]
    > L’option **Indiquer l’emplacement d’une autre version installée sur cet ordinateur** n’est pas utilisée pour les extensions de langage.

1. Dans la page **Prêt pour l’installation**, vérifiez que ces sélections sont incluses, puis choisissez **Installer**.
  
    + Services Moteur de base de données
    + Machine Learning Services et extensions de langage

    Notez l’emplacement du dossier sous le chemin `..\Setup Bootstrap\Log` où sont stockés les fichiers config. Une fois l’installation terminée, vous pouvez passer en revue les composants installés dans le fichier de synthèse.

6. Si vous êtes invité redémarrer l’ordinateur après l’installation, faites-le dès à présent. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d'informations, consultez [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="add-the-jre_home-variable"></a>Ajouter la variable JRE_HOME

`JRE_HOME` est une variable d’environnement système qui spécifie l’emplacement de l’interpréteur Java. Dans cette étape, vous allez créer une variable d’environnement système sur Windows.

1. Recherchez et copiez le chemin au dossier de base de JRE.

    Par exemple, le chemin au dossier de base de JRE pour le runtime Java par défaut Zulu JRE 11.0.3 est `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Binn\AZUL-OpenJDK-JRE\`.

    Selon le chemin d’installation de SQL Server ou la sélection d’un autre runtime Java, l’emplacement du JDK ou du JRE peut être différent de celui de l’exemple ci-dessus. Même si vous avez installé un JDK, vous obtiendrez souvent un sous-dossier JRE dans le cadre de cette installation. Dans ce cas, pointez vers le dossier JRE. L’extension Java tente de charger le fichier `jvm.dll` à partir du chemin `%JRE_HOME%\bin\server`.

1. Dans le Panneau de configuration, ouvrez **Système et sécurité**, ouvrez **Système**, puis sélectionnez **Propriétés système avancées**.

1. Sélectionnez **Variables d’environnement**.

1. Créez une variable système pour `JRE_HOME` avec la valeur du chemin JDK/JRE (disponible à l’étape 1).

1. Redémarrez [Launchpad](../concepts/extensibility-framework.md#launchpad).

    1. Ouvrez le [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md).

    1. Sous Services SQL Server, cliquez avec le bouton droit sur SSQL Server Launchpad et sélectionnez **Redémarrer**.

<a name="perms-nonwindows"></a>

## <a name="grant-access-to-non-default-jre-folder"></a>Accorder l’accès à un dossier JRE autre que celui par défaut

Si vous n’avez pas installé le JRE Zulu Open par défaut inclus avec SQL Server et que vous n’avez pas installé le JDK ou le JRE sous Program Files, vous devez effectuer les étapes suivantes. Exécutez les commandes **icacls** à partir d’une ligne *avec des privilèges élevés* pour accorder l’accès aux comptes de service **SQLRUsergroup** et SQL Server (dans **ALL_APPLICATION_PACKAGES**) afin d’accéder au JRE. Les commandes accordent l’accès de manière récursive à tous les fichiers et dossiers sous le chemin d’un répertoire donné.

1. Accorder des autorisations SQLRUserGroup

    Pour une instance nommée, ajoutez le nom de l’instance à SQLRUsergroup (par exemple, `SQLRUsergroupINSTANCENAME`).

    ```cmd
    icacls "<PATH to JRE>" /grant "SQLRUsergroup":(OI)(CI)RX /T
    ```
    
    Vous pouvez ignorer cette étape si vous avez installé le JDK/JRE dans le dossier par défaut sous Program Files sur Windows.

1. Accorder des autorisations AppContainer

    ```cmd
    icacls “<PATH to JRE>” /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    > [!NOTE]
    > La commande ci-dessus accorde des autorisations au SID de l’ordinateur **S-1-15-2-1**, ce qui équivaut à **ALL APPLICATION PACKAGES** sur une version en anglais de Windows. Vous pouvez également utiliser `icacls "<PATH to JRE>" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` sur une version en anglais de Windows.

## <a name="restart-the-service"></a>Redémarrez le service.

Une fois l’installation terminée, redémarrez le moteur de base de données avant de passer à l’étape suivante pour activer l’exécution de scripts.

Le redémarrage du service entraîne également le redémarrage automatique du service SQL Server Launchpad associé.

Pour redémarrer le service, vous pouvez cliquer avec le bouton droit sur la commande **Redémarrer** pour l’instance dans SSMS, utiliser le panneau **Services** dans le Panneau de configuration ou utiliser le [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md).

## <a name="enable-script-execution"></a>Activer l’exécution de scripts

1. Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. 

1. Connectez-vous à l’instance sur laquelle vous avez installé les extensions de langage, cliquez sur **Nouvelle requête** pour ouvrir une fenêtre de requête, puis exécutez la commande suivante :

    ```sql
    sp_configure
    ```

    La propriété `external scripts enabled` a normalement la valeur **0** à ce stade. La fonctionnalité est désactivée par défaut et doit être explicitement activée par un administrateur avant que vous puissiez exécuter du code Java.

1. Pour activer la fonctionnalité d’exécution de scripts externes, exécutez l’instruction suivante :

    ```sql
    EXEC sp_configure 'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```

    Si vous avez déjà activé la fonctionnalité pour Machine Learning Services, ne procédez pas à une deuxième configuration les extensions de langage. La plateforme d’extensibilité sous-jacente prend en charge les deux.

<a name="register_external_language"></a>

## <a name="register-external-language"></a>Inscrire le langage externe

Pour chaque base de données dans laquelle vous souhaitez utiliser des extensions de langage, vous devez inscrire le langage externe avec [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md).

L’exemple suivant ajoute un langage externe appelé Java à une base de données sur SQL Server sur Windows.

```SQL
CREATE EXTERNAL LANGUAGE Java
FROM (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

Pour plus d’informations, consultez [CRÉER UN LANGAGE EXTERNE](../../t-sql/statements/create-external-language-transact-sql.md).

## <a name="verify-installation"></a>Vérifier l'installation

Vérifiez l’état d’installation de l’instance dans les journaux d’installation.

Effectuez les étapes suivantes pour vérifier que tous les composants utilisés pour lancer un script externe sont en cours d’exécution.

1. Dans SQL Server Management Studio ou Azure Data Studio, ouvrez une nouvelle fenêtre de requête et exécutez l’instruction suivante :

    ```sql
    EXEC sp_configure 'external scripts enabled'
    ```

    La valeur **run_value** est maintenant égale à 1.

1. Ouvrez le panneau **Services** ou le Gestionnaire de configuration SQL Server, puis vérifiez que le service **SQL Server Launchpad** est en cours d’exécution. Vous devez disposer d’un service pour chaque instance du moteur de base de données sur laquelle les extensions de langage sont installées. Pour plus d’informations sur le service, consultez [Framework d’extensibilité](../concepts/extensibility-framework.md).

## <a name="additional-configuration"></a>Configuration supplémentaire

Si l’étape de vérification réussit, vous pouvez exécuter du code Java à partir de SQL Server Management Studio, d’Azure Data Studio, de Visual Studio Code ou de tout autre client capable d’envoyer des instructions T-SQL au serveur.

Si une erreur s’est produite lors de l’exécution de la commande, passez en revue les étapes de configuration supplémentaires de cette section. Vous devrez peut-être apporter des configurations supplémentaires spécifiques au service ou à la base de données.

Au niveau de l’instance, ces configurations supplémentaires peuvent inclure :

+ [Configurer le pare-feu pour SQL Server Machine Learning Services](../../machine-learning/security/firewall-configuration.md)
+ [Activer des protocoles réseau supplémentaires](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
+ [Activer des connexions à distance](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
+ [Créer un nom de connexion pour SQLRUserGroup](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)

<a name="bkmk_configureAccounts"></a>
<a name="permissions-external-script"></a>

Vous aurez peut-être besoin d’effectuer les mises à jour de configuration suivantes sur la base de données :

+ [Accorder des autorisations utilisateur pour SQL Server Machine Learning Services](../../machine-learning/security/user-permission.md)
+ [Accorder aux utilisateurs l’autorisation d’exécuter un langage spécifique](../../t-sql/statements/create-external-language-transact-sql.md#permissions)

> [!NOTE]
> Plusieurs éléments déterminent si une configuration supplémentaire est nécessaire : votre schéma de sécurité, l’emplacement d’installation de SQL Server et la façon dont les utilisateurs sont supposés se connecter à la base de données et exécuter des scripts externes.

## <a name="suggested-optimizations"></a>Optimisations suggérées

Maintenant que tout fonctionne, il peut être intéressant d’optimiser le serveur pour prendre en charge l’extension de langage Java.

### <a name="optimize-the-server-for-java-language-extension"></a>Optimisation du serveur pour l’extension de langage Java

Les paramètres par défaut pour la configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont destinés à optimiser l’équilibre du serveur pour un éventail de services pris en charge par le moteur de base de données, ce qui peut inclure les processus d’extraction, transformation et chargement (ETL, extract, transform, load), la création de rapports, l’audit et les applications qui utilisent les données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cela explique pourquoi, dans les paramètres par défaut, les ressources sont parfois restreintes ou limitées pour les extensions de langage, en particulier dans les opérations utilisant beaucoup de mémoire.

Pour vous assurer que les travaux d’extensions de langage sont classés par ordre de priorité et correctement ressourcées, nous vous recommandons d’utiliser la fonctionnalité Resource Governor de SQL Server pour configurer un pool de ressources externes. Vous pouvez aussi modifier la quantité de mémoire allouée au moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou augmenter le nombre de comptes s’exécutant sous le service [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)].

+ Pour configurer un pool de ressources afin de gérer des ressources externes, consultez [Créer un pool de ressources externes](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
+ Pour modifier la quantité de mémoire réservée à la base de données, consultez [Options de configuration de la mémoire du serveur](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
Si vous utilisez l’édition Standard et que vous n’avez pas le composant Resource Governor, vous pouvez utiliser les vues de gestion dynamique, les événements étendus ainsi que la supervision des événements Windows pour mieux gérer les ressources serveur.

## <a name="next-steps"></a>Étapes suivantes

Les développeurs peuvent démarrer avec quelques exemples simples et découvrir les principes de base du fonctionnement de Java avec SQL Server. Pour accéder à l’étape suivante, cliquez sur le lien suivant :

+ [Tutoriel : Expressions régulières avec Java](../tutorials/search-for-string-using-regular-expressions-in-java.md)