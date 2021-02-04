---
title: Installer Distributed Replay
titleSuffix: SQL Server Distributed Replay
description: 'Cet article décrit les différentes façons d’installer Distributed Replay : à partir de l’Assistant Installation, de la fenêtre d’invite de commandes ou d’un fichier de configuration.'
ms.prod: sql
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ea1171da-f50e-4f16-bedc-5e468a46477f
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 4909975e83ea4438d969685ed9b76ae9f02a5631
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077077"
---
# <a name="install-distributed-replay"></a>Installer Distributed Replay

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Vous pouvez installer Distributed Replay de trois manières :  
  
-   [Installer Distributed Replay à partir de l’Assistant Installation](#bkmk_wizard)  
  
-   [Installer Distributed Replay à partir de l’invite de commandes](#bkmk_command_prompt)  
  
-   [Installer Distributed Replay à l’aide d’un fichier de configuration](#bkmk_configuration_file)  
  
##  <a name="install-distributed-replay-from-the-installation-wizard"></a><a name="bkmk_wizard"></a> Installer Distributed Replay à partir de l’Assistant Installation  
 Installer les fonctionnalités [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay avec l’Assistant Installation de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . Pour décider de l'emplacement où installer ces fonctionnalités, tenez compte des éléments suivants :  
  
-   Vous pouvez installer l'outil d'administration sur le même ordinateur que le contrôleur Distributed Replay ou sur d'autres ordinateurs.  
  
-   Chaque environnement Distributed Replay ne doit contenir qu'un seul contrôleur.  
  
-   Vous pouvez installer le service client sur 16 ordinateurs (physiques ou virtuels) au maximum.  
  
-   Une seule instance du service client peut être installée sur l'ordinateur du contrôleur Distributed Replay. Si votre environnement Distributed Replay doit avoir plusieurs clients, il n'est pas recommandé d'installer le service client sur le même ordinateur que le contrôleur. Cela risquerait de réduire la vitesse globale de Distributed Replay.  
  
-   Pour les scénarios de test des performances, nous déconseillons d'installer l'outil d'administration, le service du contrôleur ou le service client Distributed Replay sur l'instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'installation de toutes ces fonctionnalités sur le serveur cible doit être limitée au test fonctionnel de compatibilité des applications.  
  
-   Après l'installation, le service contrôleur, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller, doit s'exécuter avant le démarrage du service Distributed Replay Client sur les clients.  
  
> [!NOTE]  
>  Pour supprimer ou modifier des fonctionnalités Distributed Replay, utilisez la fenêtre **Programmes et fonctionnalités** du **Panneau de configuration** Windows. Sélectionnez [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] dans la fenêtre **Désinstaller ou modifier un programme** , puis cliquez sur **Supprimer** pour ouvrir l'Assistant Installation de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . Dans la page **Sélectionner les composants** , sélectionnez les fonctionnalités Distributed Replay à supprimer.  
  
 **Configuration requise :**  
  
-   Assurez-vous que les ordinateurs que vous voulez utiliser présentent la configuration décrite dans la rubrique [Distributed Replay Requirements](../../tools/distributed-replay/distributed-replay-requirements.md).  
  
-   Avant d'entamer cette procédure, créez les comptes d'utilisateur de domaine sous lesquels s'exécuteront les services du contrôleur et du client. Il est préférable que ces comptes ne soient pas membres du groupe Administrateurs Windows. Pour plus d'informations, consultez la section relative aux comptes d'utilisateur et de service de la rubrique [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md) .  
  
    > [!NOTE]  
    >  Vous pouvez utiliser des comptes d'utilisateur locaux si vous exécutez l'outil d'administration, le service du contrôleur et le service du client sur le même ordinateur.  
  
 **Emplacements d'installation**  
  
 En supposant que vous utilisez les emplacements de fichiers et l'installation par défaut, le répertoire de base se trouve dans C:\Program Files\Microsoft SQL Server. Dans ce répertoire, les emplacements d'installation des binaires et des assemblys sont les suivants :  
  
-   Sur un système 32 bits :  
  
     [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]Outils  
  
     \- OR -  
  
     \<Share Feature Directory>\Outils\\(répertoire de fonctionnalités partagées alternatives fourni par l’utilisateur)  
  
-   Sur un système 64 bits :  
  
     C:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (x86)\130\Tools  
  
     \- OR -  
  
     \<Share Feature Directory (x86)>\Outils\\(répertoire (x86) de fonctionnalités partagées alternatives fourni par l’utilisateur)  
  
#### <a name="to-install-distributed-replay-features"></a>Pour installer les fonctionnalités Distributed Replay  
  
1.  Pour démarrer l'installation d'une fonctionnalité Distributed Replay, démarrez l'Assistant Installation de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
2.  La page **Règles de support du programme d'installation** identifie les problèmes susceptibles de se présenter lors de l'installation des fichiers de support d'installation de SQL Server. Vous devez corriger toute erreur de support du programme d'installation avant de poursuivre l'installation.  
  
3.  Dans la page **Clé du produit** , sélectionnez une case d'option pour indiquer si vous installez une édition gratuite de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ou une version de production du produit qui a une clé PID. Pour plus d’informations, consultez [Éditions et composants de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
4.  Dans la page **Termes du contrat de licence** , prenez connaissance du contrat de licence, puis activez la case à cocher indiquant que vous en acceptez les termes et conditions. Pour aider à améliorer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez également activer l'option d'utilisation des fonctionnalités et envoyer des rapports à [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
5.  Dans la page **Fichiers de support du programme d'installation** , cliquez sur **Installer** pour installer ou mettre à jour les fichiers de support du programme d'installation pour [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
6.  Dans la page **Rôle d’installation**, sélectionnez **Installation de fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** , puis cliquez sur **Suivant** pour passer à la page **Sélection des fonctionnalités**.  
  
7.  Dans la page **Sélection de fonctionnalités** , configurez les fonctionnalités à installer.  
  
    -   Pour installer l’outil d’administration, sélectionnez **Outils de gestion - De base**.  
  
    -   Pour installer le service du contrôleur, sélectionnez **Distributed Replay Controller**.  
  
    -   Pour installer le service client, sélectionnez **Distributed Replay Client**.  
  
     **Important !** lorsque vous configurez le contrôleur Distributed Replay, vous pouvez spécifier un ou plusieurs comptes d'utilisateurs qui seront utilisés pour exécuter les services client Distributed Replay. Vous trouverez ci-dessous la liste des comptes pris en charge :  
  
    -   Compte d’utilisateur de domaine  
  
    -   Compte d'utilisateur local créé par l'utilisateur  
  
    -   Administrateur  
  
    -   Compte virtuel et Compte de service administré (MSA)  
  
    -   Services réseau, Services locaux et Système  
  
     Les comptes de groupe (locaux ou de domaine) et autres comptes intégrés (comme Tout le monde) ne sont pas acceptés.  
  
8.  Éventuellement, cliquez sur le bouton de sélection (...) pour modifier le chemin du répertoire des fonctionnalités partagées.  
  
    1.  Sur les ordinateurs 32 bits, le chemin d’installation par défaut est **C:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\**  
  
    2.  Sur les ordinateurs 64 bits, le chemin d’installation par défaut est **C:\Program Files (x86)\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\**  
  
9. Quand vous avez terminé, cliquez sur **Suivant**.  
  
10. Dans la page **Règles d'installation** , le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide la configuration de votre ordinateur. Une fois que le processus de validation est terminé, cliquez sur **Suivant**.  
  
11. La page **Espace disque nécessaire** calcule l'espace disque nécessaire pour les fonctionnalités que vous spécifiez. Elle compare ensuite cet espace à l'espace disque disponible.  
  
12. Dans la page **Création de rapports d'erreurs** , spécifiez les informations que vous souhaitez envoyer à [!INCLUDE[msCoName](../../includes/msconame-md.md)] afin d'aider à améliorer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'option de création de rapports d'erreurs est activée par défaut.  
  
13. Dans la page **Règles de configuration de l'installation** , l'Outil d'analyse de configuration système exécutera un autre ensemble de règles pour valider la configuration de votre ordinateur avec les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous avez spécifiées.  
  
14. Dans la page **Prêt à installer le programme** , cliquez sur **Installer**.  
  
    > [!IMPORTANT]  
    >  Une fois que vous avez installé Distributed Replay, vous devez créer des règles de pare-feu sur le contrôleur et les ordinateurs clients, et accorder des autorisations à chaque ordinateur client sur le serveur cible. Pour plus d’informations, consultez [Suivre les étapes consécutives à l’installation](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
### <a name="net-framework-security"></a>Sécurité du .NET Framework  
 Vous devez posséder des autorisations administratives pour installer les fonctionnalités de Distributed Replay. Seule une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disposant d’autorisations sysadmin peut ajouter des comptes de services clients au rôle serveur sysadmin du serveur de test. Pour plus d'informations sur les questions de sécurité de Distributed Replay, consultez [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
##  <a name="install-distributed-replay-from-the-command-prompt"></a><a name="bkmk_command_prompt"></a> Installer Distributed Replay à partir de l'invite de commandes  
 L'installation d'une nouvelle instance de Distributed Replay à partir de l'invite de commandes vous permet de spécifier les composants à installer et la façon dont ils doivent être configurés. L'installation par l'invite de commandes prend en charge l'installation, la réparation, la mise à niveau et la désinstallation des composants Distributed Replay. Lors de l'installation à partir de l'invite de commandes, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le mode silencieux complet via le paramètre /Q.  
  
> [!NOTE]  
>  Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui a les autorisations de lecture et d'exécution sur le partage distant.  
  
### <a name="installation-parameters"></a>Paramètres d'installation  
 La liste des fonctionnalités prioritaires comprend [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]et Tools. La fonctionnalité Tools installera les outils d'administration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ainsi que d'autres composants partagés. Pour installer les composants Distributed Replay, spécifiez les paramètres suivants :  
  
|Composant|Paramètre|  
|---------------|---------------|  
|Distributed Replay Controller|**DREPLAY_CTLR**|  
|Distributed Replay Client|**DREPLAY_CLT**|  
|Outil d'administration|**outils**|  
  
> [!IMPORTANT]  
>  Une fois que vous avez installé Distributed Replay, vous devez créer des règles de pare-feu sur le contrôleur et les ordinateurs clients, et accorder des autorisations à chaque ordinateur client sur le serveur cible. Pour plus d’informations, consultez [Suivre les étapes consécutives à l’installation](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
 Utilisez les paramètres répertoriés dans le tableau ci-dessous pour développer des scripts d'installation en ligne de commande.  
  
|Paramètre|Description|Valeurs prises en charge|  
|---------------|-----------------|----------------------|  
|/CTLRSVCACCOUNT<br /><br /> **Facultatif**|Compte de service Distributed Replay Controller.|Vérifie le compte et le mot de passe|  
|/CTLRSVCPASSWORD<br /><br /> **Facultatif**|Mot de passe du compte de service Distributed Replay Controller.|Vérifie le compte et le mot de passe|  
|/CTLRSTARTUPTYPE<br /><br /> **Facultatif**|Type de démarrage pour le compte de service Distributed Replay Controller.|Automatique<br /><br /> Désactivé<br /><br /> Manuel|  
|/CTLRUSERS<br /><br /> **Facultatif**|Spécifiez les utilisateurs qui disposent d'autorisations pour le service Distributed Replay Controller.|Ensemble de chaînes de compte d'utilisateur utilisant «   » (espace) comme séparateur<br /><br /> **Important !** Quand vous configurez le service Distributed Replay Controller, vous pouvez spécifier le ou les comptes d’utilisateurs qui seront utilisés pour exécuter les services Distributed Replay Client. Vous trouverez ci-dessous la liste des comptes pris en charge :<br /><br /> Compte d’utilisateur de domaine<br /><br /> Compte d'utilisateur local créé par l'utilisateur<br /><br /> Administrateur<br /><br /> Administrateur<br /><br /> Compte virtuel et Compte de service administré (MSA)<br /><br /> Services réseau, Services locaux et Système<br /><br /> <br /><br /> Remarque : Les comptes de groupe (locaux ou de domaine) et autres comptes intégrés (comme Tout le monde) ne sont pas acceptés.|  
|/CLTSVCACCOUNT<br /><br /> **Facultatif**|Compte de service Distributed Replay Client.|Vérifie le compte et le mot de passe|  
|/CLTSVCPASSWORD<br /><br /> **Facultatif**|Mot de passe du compte du service Distributed Replay Client.|Vérifie le compte et le mot de passe|  
|/CLTSTARTUPTYPE<br /><br /> **Facultatif**|Type de démarrage du compte du service Distributed Replay Client.|Automatique<br /><br /> Désactivé<br /><br /> Manuel|  
|/CLTCTLRNAME<br /><br /> **Facultatif**|Nom de l'ordinateur avec lequel le client communique pour le service Distributed Replay Controller.||  
|/CLTWORKINGDIR<br /><br /> **Facultatif**|Répertoire de travail du service Distributed Replay Client.|Chemin d'accès valide|  
|/CLTRESULTDIR<br /><br /> **Facultatif**|Répertoire des résultats du service Distributed Replay Client.|Chemin d'accès valide|  
  
### <a name="sample-syntax"></a>Exemple de syntaxe :  
 **Pour installer le composant contrôleur de Distributed Replay**  
  
```  
setup /q /ACTION=Install /FEATURES=DREPLAY_CTLR /IAcceptSQLServerLicenseTerms /CTLRUSERS="domain\user1" "domain\user2" /CTLRSVCACCOUNT="domain\svcuser" /CTLRSVCPASSWORD="password" /CTLRSTARTUPTYPE=Automatic  
```  
  
 **Pour installer le composant client de Distributed Replay**  
  
```  
setup /q /ACTION=Install /FEATURES=DREPLAY_CLT /IAcceptSQLServerLicenseTerms /CLTSVCACCOUNT="domain\svcuser" /CLTSVCPASSWORD="password" /CLTSTARTUPTYPE=Automatic /CLTCTLRNAME=ControllerMachineName /CLTWORKINGDIR="C:\WorkingDir" /CLTRESULTDIR="C:\ResultDir  
```  
  
##  <a name="install-distributed-replay-using-a-configuration-file"></a><a name="bkmk_configuration_file"></a> Installer Distributed Replay à l'aide d'un fichier de configuration  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet de générer un fichier de configuration basé sur les entrées utilisateur et les entrées système par défaut. Si vous spécifiez que vous souhaitez que les outils d'administration soient installés, vous pouvez utiliser le fichier de configuration pour déployer les trois composants Distributed Replay (outil d'administration, Distributed Replay Controller et Distributed Replay Client). Il prend en charge l'installation, la réparation et la désinstallation des composants Distributed Replay.  
  
 Le programme d'installation ne prend en charge l'utilisation du fichier de configuration qu'à l'aide de la ligne de commande. L'ordre de traitement des paramètres lors de l'utilisation du fichier de configuration est décrit ci-dessous :  
  
-   Le fichier de configuration remplace les valeurs par défaut d'un package.  
  
-   Les valeurs de la ligne de commande remplacent les valeurs du fichier de configuration.  
  
 Pour plus d’informations sur l’utilisation d’un fichier de configuration, consultez [Installer SQL Server 2016 à l’aide d’un fichier de configuration](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md).  
  
> [!IMPORTANT]  
>  Une fois que vous avez installé Distributed Replay, vous devez créer des règles de pare-feu sur le contrôleur et les ordinateurs clients, et accorder des autorisations à chaque ordinateur client sur le serveur cible. Pour plus d’informations, consultez [Suivre les étapes consécutives à l’installation](../../tools/distributed-replay/complete-the-post-installation-steps.md).  
  
#### <a name="to-generate-a-configuration-file"></a>Pour générer un fichier de configuration  
  
1.  Suivez le déroulement des étapes de l'Assistant Installation jusqu'à la page **Prêt pour l'installation** . Le chemin d'accès au fichier de configuration est spécifié dans la page **Prêt pour l'installation** , dans la section relative au chemin d'accès du fichier de configuration.  
  
2.  Annulez l'exécution du programme d'installation sans réellement terminer l'installation afin de générer le fichier INI.  
  
#### <a name="to-install-distributed-replay-using-the-configuration-file"></a>Pour installer Distributed Replay à l'aide du fichier de configuration  
  
-   Exécutez l'installation à partir de l'invite de commandes et spécifiez le fichier ConfigurationFile.ini à l'aide du paramètre ConfigurationFile.  
  
 **Exemple de syntaxe**  
  
 Voici un exemple montrant comment spécifier le fichier de configuration lors de l'invite de commandes :  
  
```
Setup.exe /CTLRSVCPASSWORD="ctlrsvcpswd" /CLTSVCPASSWORD="cltsvcpswd" / ConfigurationFile=ConfigurationFile.INI\  
```

> [!NOTE]
> Vous devez spécifier les deux mots de passe dans la ligne de commande car il n'est pas possible de les configurer dans le fichier de configuration.  

## <a name="see-also"></a>Voir aussi

- [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)
- [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)
- [Conditions requises par Distributed Replay](../../tools/distributed-replay/distributed-replay-requirements.md)
- [Options de ligne de commande de l’outil d’administration &#40;Distributed Replay Utility&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md)
- [Configurer Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md)