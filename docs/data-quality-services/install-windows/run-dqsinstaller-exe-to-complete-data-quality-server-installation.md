---
title: Exécuter DQSInstaller.exe pour terminer l’installation du serveur de qualité des données
description: Découvrez comment exécuter le DQSInstaller.exe pour effectuer une installation de SQL Server Data Quality Services (DQS).
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: 7a8c96e0-1328-4f35-97fc-b6d9cb808bae
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 2933803ce4a8bce84468f4ce2e360a18d2444635
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765792"
---
# <a name="run-dqsinstallerexe-to-complete-data-quality-server-installation"></a>Exécuter DQSInstaller.exe pour terminer l'installation du serveur DQS

[!INCLUDE [SQL Server - Windows only ](../../includes/applies-to-version/sql-windows-only.md)]

  Pour terminer l'installation du [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] , vous devez exécuter le fichier DQSInstaller.exe après avoir installé [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Cette rubrique décrit comment exécuter DQSInstaller.exe à partir de l'écran **Démarrer** , du menu **Démarrer** , de l'Explorateur Windows ou d'une invite de commandes ; vous pouvez choisir l'une des manières suivantes pour exécuter le fichier DQSInstaller.exe.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez avoir sélectionné **Data Quality Services** sous **Services Moteur de base de données** sur la page de sélection de fonctionnalités du programme d'installation de SQL Server lors de l'installation de SQL Server, et terminé l'installation de SQL Server. Pour plus d’informations, consultez [Installer Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md).  
  
-   Votre compte d'utilisateur Windows doit être membre du rôle serveur fixe sysadmin dans l'instance du moteur de base de données SQL Server.  
  
-   Vous devez être connecté en tant que membre du groupe Administrateurs sur l'ordinateur où vous exécutez DQSInstaller.exe.  
  
##  <a name="run-dqsinstallerexe-from-start-screen-start-menu-or-windows-explorer"></a><a name="WindowsExplorer"></a> Exécuter DQSInstaller.exe à partir de l’écran d’accueil, du menu Démarrer ou de l’Explorateur Windows  
  
1.  Sur l'ordinateur sur lequel vous avez choisi d'installer le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)], exécutez le fichier DQSInstaller.exe d'une des manières suivantes, selon le cas :  
  
    -   Écran **Démarrer**: dans l'écran **Démarrer** , cliquez sur **Data Quality Server Installer**.  
  
    -   **Menu Démarrer**: dans la barre des tâches, cliquez sur **Démarrer**, pointez sur **Tous les programmes**, puis cliquez sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]. Sous [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], cliquez sur **Data Quality Services**, puis sur **Data Quality Server Installer**.  
  
    -   **Explorateur Windows**: recherchez le fichier DQSInstaller.exe. Si vous avez installé l’instance par défaut de SQL Server, le fichier DQSInstaller.exe sera disponible dans C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn. Double-cliquez sur le fichier DQSInstaller.exe.  
  
2.  Une fenêtre d'invite de commandes s'affiche qui indique l'état de l'installation. Vous remarquerez les trois événements suivants :  
  
    1.  Le programme d'installation crée un fichier journal d'installation, DQS_install.log, qui contient des informations sur les actions effectuées lors de l'exécution du fichier DQSInstaller.exe. Si vous avez installé l’instance par défaut de SQL Server, le fichier DQS_install.log sera disponible dans C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Log.  
  
    2.  Le programme d'installation utilise le classement par défaut du serveur, SQL_Latin1_General_CP1_CI_AS, pour installer le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)].  
  
        > [!IMPORTANT]  
        >  Vous pouvez fournir une valeur différente de classement du serveur pour installer le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] uniquement si vous exécutez DQSInstaller.exe à partir d'une invite de commandes. Pour plus d'informations, consultez la section [Exécuter DQSInstaller.exe à partir d'une invite de commandes](#CommandPrompt) plus loin dans cette rubrique.  
  
    3.  Le programme d'installation vérifie tous les redémarrages en attente sur votre ordinateur dus à toutes les mises à jour qui ont été récemment installées sur votre ordinateur. Si des redémarrages en attente sont trouvés, un message apparaît vous en informant, et vous pouvez choisir de poursuivre ou d'interrompre l'installation en appuyant sur O ou N respectivement. S'il existe des redémarrages en attente, nous vous recommandons d'abandonner l'installation, de redémarrer votre ordinateur, puis d'exécuter à nouveau DQSInstaller.exe.  
  
3.  Vous êtes invité à taper un mot de passe pour la clé principale de base de données. La clé principale de base de données est requise pour chiffrer les clés du fournisseur de services de données de référence qui seront stockées dans la base de données DQS_MAIN lorsque vous installerez des fournisseurs de données de référence dans [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) ultérieurement.  
  
    > [!IMPORTANT]  
    >  Le mot de passe doit comporter au moins 8 caractères et doit contenir des caractères provenant de trois des quatre catégories suivantes : lettre majuscule anglaise (A, B, C,... Z), lettre minuscule anglaise (a, b, c,... z), chiffre (0, 1, 2,... 9) et les caractères non alphanumériques ou spéciaux (~ ! @ # $% ^& * () _-+ = | \\ {} []:;"' <>,. ? /). Par exemple : P@ssword. Le programme d'installation vous invite à entrer un autre mot de passe si le mot de passe actuel ne correspond pas aux conditions.  
  
4.  Fournissez un mot de passe, confirmez le mot de passe, puis appuyez sur ENTRÉE pour continuer l'installation.  
  
    > [!IMPORTANT]  
    >  Vous devez conserver le mot de passe spécifié pour la clé principale de base de données parce que vous en aurez besoin plus tard pour restaurer les sauvegardes des bases de données DQS, le cas échéant. Pour plus d'informations sur la restauration d'une base de données DQS, consultez [Backing Up and Restoring DQS Databases](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
  
5.  Si une base de données Master Data Services est présente dans la même instance SQL Server que le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)], le programme d'installation crée un utilisateur mappé dans la connexion Master Data Services, et lui affecte le rôle dqs_administrator sur la base de données DQS_MAIN. Pour plus d'informations sur l'installation de Master Data Services et la création d'une base de données Master Data Services, consultez [Install Master Data Services](../../master-data-services/install-windows/install-master-data-services.md).  
  
6.  Un message d'achèvement s'affiche une fois l'installation terminée. Appuyez sur n'importe quelle touche pour fermer la fenêtre d'invite de commandes.  
  
##  <a name="run-dqsinstallerexe-from-command-prompt"></a><a name="CommandPrompt"></a> Exécuter DQSInstaller.exe à partir de l’invite de commandes  
 Vous pouvez exécuter DQSInstaller.exe depuis une invite de commandes à l'aide des paramètres de ligne de commande suivants :  
  
|Paramètre DQSInstaller.exe|Description|Exemple de syntaxe|  
|--------------------------------|-----------------|-------------------|  
|-collation|Le classement du serveur à utiliser pour installer le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)].<br /><br /> DQS prend seulement en charge le classement qui respecte la casse. Si vous spécifiez un classement qui respecte la casse, le programme d'installation essaie d'utiliser la version qui ne respecte pas la casse du classement spécifié. S'il n'existe aucune version ne respectant pas la casse, ou si le classement n'est pas pris en charge par SQL, l'installation du [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] échoue.<br /><br /> Si un classement du serveur n'est pas spécifié, le classement par défaut, SQL_Latin1_General_CP1_CI_AS, est utilisé.|`dqsinstaller.exe -collation <collation_name>`|  
|-upgradedlls|Ignore la nouvelle création des bases de données DQS (DQS_MAIN, DQS_PROJECTS et DQS_STAGING_DATA) et met à jour uniquement des assemblys du Common Language Runtime de SQL (SQLCLR) utilisés par DQS dans la base de données [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .<br /><br /> Pour plus d’informations, consultez [Mettre à niveau des assemblys SQLCLR après une mise à jour de .NET Framework](../../data-quality-services/install-windows/upgrade-sqlclr-assemblies-after-net-framework-update.md).|`dqsinstaller.exe -upgradedlls`|  
|-exportkbs|Exportez toutes les bases de connaissances vers un fichier de sauvegarde DQS (.dqsb). Vous devez également spécifier le chemin d'accès complet et le nom du fichier où vous voulez exporter toutes les bases de connaissances.<br /><br /> Pour plus d'informations, consultez [Exporter et importer des bases de connaissances DQS à l'aide de DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md).|`dqsinstaller.exe -exportkbs <path><filename>`<br /><br /> Par exemple : `dqsinstaller.exe -exportkbs c:\DQSBackup.dqsb`|  
|-importkbs|Importez toutes les bases de connaissances à partir d'un fichier de sauvegarde DQS (.dqsb) après l'installation de [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] . Vous devez également spécifier le chemin d'accès complet et le nom du fichier à partir duquel vous voulez importer toutes les bases de connaissances.<br /><br /> Pour plus d'informations, consultez [Exporter et importer des bases de connaissances DQS à l'aide de DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md).|`dqsinstaller.exe -importkbs <path><filename>`<br /><br /> Par exemple : `dqsinstaller.exe -importkbs c:\DQSBackup.dqsb`|  
|-upgrade|Met à niveau le schéma des bases de données DQS. Vous devez utiliser ce paramètre après avoir installé une mise à jour de SQL Server sur une instance DQS précédemment configurée. Pour plus d'informations, consultez [Upgrade DQS Databases Schema After Installing SQL Server Update](../../data-quality-services/install-windows/upgrade-dqs-databases-schema-after-installing-sql-server-update.md).|`dqsinstaller.exe -upgrade`|  
|-uninstall|Désinstalle le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] de l'instance actuelle de SQL Server.<br /><br /> Vous pouvez également exporter toutes les bases de connaissances de l'installation existante du serveur DQS vers un fichier de sauvegarde DQS (.dqsb), avant de désinstaller le serveur DQS. Pour plus d'informations, consultez [Exporter et importer des bases de connaissances DQS à l'aide de DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md).<br /><br /> Important si vous désinstallez à partir d’une instance SQL Server à l’aide du paramètre de ligne de **\* commande, tous les \* objets DQS sont supprimés dans le cadre du processus de désinstallation. \* \*** [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] `-uninstall` Vous n’êtes pas obligé de les supprimer manuellement après la désinstallation de [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] comme indiqué dans [Supprimer les objets serveur DQS](../../sql-server/install/remove-data-quality-server-objects.md).|**Pour désinstaller uniquement le serveur DQS :**<br /><br /> `dqsinstaller.exe -uninstall`<br /><br /> **Pour exporter toutes les bases de connaissances vers un fichier, puis désinstaller le serveur DQS :**<br /><br /> `dqsinstaller.exe -uninstall <path><filename>`<br /><br /> Par exemple : `dqsinstaller.exe -uninstall c:\DQSBackup.dqsb`|  
  
 **Pour exécuter DQSInstaller.exe à partir d'une invite de commandes :**  
  
1.  Démarrez l'invite de commandes.  
  
2.  À l'invite de commandes, remplacez votre répertoire à l'emplacement où DQSInstaller.exe est disponible. Si vous avez installé l’instance par défaut de SQL Server, le fichier DQSInstaller.exe est disponible dans C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn:  
  
    ```  
    cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn  
    ```  
  
3.  À l'invite de commandes, exécutez DQSInstaller.exe avec ou sans paramètres de ligne de commande :  
  
    -   **Sans paramètre de ligne de commande**: tapez `dqsinstaller.exe`, puis appuyez sur Entrée.  
  
    -   **Avec paramètre de ligne de commande**: tapez la commande requise comme indiqué dans le tableau ci-dessus, puis appuyez sur Entrée.  
  
4.  Les actions requises sont exécutées selon la commande spécifiée. Si vous choisissez d’installer uniquement [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] sans aucun paramètre de ligne de commande, le reste des étapes est identique, comme décrit dans les étapes 2 à 6 dans la section précédente, [Exécuter DQSInstaller.exe à partir de l’écran Démarrer, du menu Démarrer ou de l’Explorateur Windows](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md#WindowsExplorer).  
  
## <a name="next-steps"></a>Étapes suivantes  
  
-   Attribuez les rôles DQS appropriés aux utilisateurs selon leur profil de travail. Consultez [Affecter des rôles DQS aux utilisateurs](../../data-quality-services/install-windows/grant-dqs-roles-to-users.md).  
  
-   Si le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] est accessible à distance à partir d'un [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)], activez le protocole TCP/IP à l'aide du gestionnaire de configuration SQL Server sur cet ordinateur.  
  
-   Assurez-vous que vous avez accès à vos données sources pour les opérations DQS et que vous pouvez exporter les données traitées vers une table dans une base de données. Consultez [Accéder aux données pour les opérations DQS](../../data-quality-services/install-windows/access-data-for-the-dqs-operations.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Installer Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [Mettre à niveau les assemblys SQLCLR après la mise à jour .NET Framework](../../data-quality-services/install-windows/upgrade-sqlclr-assemblies-after-net-framework-update.md)   
 [Exporter et importer des bases de connaissances DQS à l'aide de DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md)  
  
  
