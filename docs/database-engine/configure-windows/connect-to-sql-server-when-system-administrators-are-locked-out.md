---
title: Se connecter à SQL Server quand les administrateurs système n’y ont plus accès | Microsoft Docs
description: Découvrez comment accéder à nouveau à SQL Server en tant qu’administrateur système si vous avez été verrouillé par erreur.
ms.custom: contperf-fy20q4
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- sa account
- connecting when locked out [SQL Server]
- locked out [SQL Server]
ms.assetid: c0c0082e-b867-480f-a54b-79f2a94ceb67
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3b4cf5946fba4eb3c79150b29ef44bd232f9c155
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878943"
---
# <a name="connect-to-sql-server-when-system-administrators-are-locked-out"></a>Se connecter à SQL Server lorsque les administrateurs système n'y ont plus accès 
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
Cette article explique comment avoir à nouveau accès à [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] en tant qu’administrateur système si vous avez été verrouillé.  Un administrateur système peut perdre l'accès à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'une des raisons suivantes :  
  
-   Toutes les connexions qui sont membres du rôle serveur fixe sysadmin ont été supprimées par erreur.  
  
-   Tous les groupes Windows qui sont membres du rôle serveur fixe sysadmin ont été supprimés par erreur.  
  
-   Les connexions membres du rôle serveur fixe sysadmin sont destinées aux personnes qui ont quitté la société ou qui ne sont pas disponibles.  
  
-   Le compte d'administrateur système (sa) est désactivé ou personne ne connaît le mot de passe.  
  
## <a name="resolution"></a>Résolution

Pour résoudre votre problème d’accès, nous vous recommandons de démarrer l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur. Ce mode empêche les autres connexions de se produire lorsque vous essayez de récupérer l’accès. À partir de là, vous pouvez vous connecter à votre instance de SQL Server et ajouter votre connexion au rôle de serveur **sysadmin**. La procédure détaillée pour cette solution est fournie dans la section [instructions pas à pas](#step-by-step-instructions).


Vous pouvez démarrer une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur avec les options `-m` ou `-f` à partir de la ligne de commande. Tout membre du groupe Administrateurs local de l'ordinateur peut se connecter ensuite à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comme membre du rôle serveur fixe **sysadmin**.  

Lorsque vous démarrez une instance en mode mono-utilisateur, arrêtez au préalable le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Dans le cas contraire, l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut se connecter en premier, en utilisant la seule connexion disponible au serveur et en vous empêchant de vous connecter.

Il est également possible qu’une application cliente inconnue prenne la seule connexion disponible avant que vous puissiez vous connecter. Pour éviter ce problème, vous pouvez utiliser l’option `-m` suivie d’un nom d’application pour limiter les connexions à une connexion unique à partir de l’application spécifiée. Par exemple, démarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec `-mSQLCMD` limite les connexions à une connexion unique, laquelle doit s’identifier en tant que programme client **sqlcmd**. Pour établir une connexion par le biais de l'Éditeur de requêtes dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], utilisez `-m"Microsoft SQL Server Management Studio - Query"`.  


> [!IMPORTANT]  
> N’utilisez pas `-m` avec un nom d’application comme fonctionnalité de sécurité. Les applications clientes spécifient le nom de l’application par le biais des paramètres de chaîne de connexion.de sorte qu'elle peut facilement être usurpée avec un faux nom.

Le tableau suivant résume les différentes façons de démarrer votre instance en mode mono-utilisateur dans la ligne de commande.

| Option | Description | Quand l’utiliser |
|:---|:---|:---|
|`-m` | Limite les connexions à une connexion unique | Quand aucun autre utilisateur ne tente de se connecter à l’instance ou si vous n’êtes pas sûr du nom de l’application que vous utilisez pour vous connecter à l’instance. |
|`-mSQLCMD`| Limite les connexions à une connexion unique, laquelle doit s’identifier en tant que programme client **sqlcmd**| Lorsque vous envisagez de vous connecter à l’instance avec **sqlcmd** et que vous souhaitez empêcher d’autres applications d’utiliser la seule connexion disponible. |
|`-m"Microsoft SQL Server Management Studio - Query"`| Limite les connexions à une connexion unique qui doit s’identifier en tant qu’application **Microsoft SQL Server Management Studio - Requête**.| Lorsque vous envisagez de vous connecter à l’instance avec l’Éditeur de requête dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et que vous souhaitez empêcher d’autres applications d’utiliser la seule connexion disponible. |
|`-f`| Limite les connexions à une connexion unique et démarre l’instance dans une configuration minimale | Quand une autre configuration vous empêche de démarrer. |
| &nbsp; | &nbsp; | &nbsp; |
  
Pour obtenir des instructions détaillées sur le démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur, consultez .[Démarrer SQL Server en mode mono-utilisateur](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md).

## <a name="step-by-step-instructions"></a>Instructions pas à pas

Les instructions pas à pas suivantes décrivent comment accorder des autorisations d’administrateur système à une connexion SQL Server qui a été bloquée par erreur.

Ces instructions partent des prérequis suivants :

* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est exécuté sur Windows 8 ou version ultérieure. De légers ajustements pour les versions antérieures de SQL Server ou de Windows sont fournis le cas échéant.

* Assurez-vous que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est installé sur l'ordinateur.  

Suivez ces instructions lorsque vous êtes connecté à Windows en tant que membre du groupe Administrateurs local.

1.  Dans le menu Démarrer de Windows, cliquez avec le bouton droit sur l’icône de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager et choisissez **Exécuter en tant qu’administrateur** pour transmettre vos informations d’identification d’administrateur à Configuration Manager.  
  
2.  Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans le volet gauche, sélectionnez **Services SQL Server**. Dans le volet droit, recherchez votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. (L’instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclut **(MSSQLSERVER)** après le nom de l’ordinateur. Les instances nommées sont affichées en majuscules et portent le même nom que dans la zone Serveurs inscrits.) Cliquez avec le bouton droit sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis cliquez sur **Propriétés**.  
  
3.  Sous l’onglet **Paramètres de démarrage**, dans la zone **Spécifiez un paramètre de démarrage**, tapez `-m`, puis cliquez sur **Ajouter**. (Il s'agit d'un trait d'union suivi d'un m minuscule)  
  
    > [!NOTE]  
    >  Certaines versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'ont pas d'onglet **Paramètres de démarrage** . Dans ce cas, sous l’onglet **Avancé** , double-cliquez sur **Paramètres de démarrage**. Les paramètres s'ouvrent dans une fenêtre très petite. Veillez à ne pas modifier les paramètres existants. Tout en bas, ajoutez un nouveau paramètre `;-m` , puis cliquez sur **OK**. (Il s'agit d'un point-virgule, suivi d'un trait d'union et d'un m minuscule.)  
  
4.  Cliquez sur **OK** puis, après le message de redémarrage, cliquez avec le bouton droit sur le nom de votre serveur et cliquez sur **Redémarrer**.  
  
5.  Après le redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , votre serveur passe en mode mono-utilisateur. Vérifiez que l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas en cours d’exécution. S'il est démarré, il utilise votre unique connexion.  
  
6.  Dans le menu Démarrer de Windows, cliquez avec le bouton droit sur l’icône de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis sélectionnez **Exécuter en tant qu’administrateur**. Cette opération transfère vos informations d'identification d'administrateur à SSMS.
  
    > [!NOTE]  
    >  Pour les versions antérieures de Windows, l’option **Exécuter en tant qu’administrateur** apparaît comme sous-menu.  
  
     Dans certaines configurations, SSMS essaie d'établir plusieurs connexions. Les connexions multiples échouent car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en mode mono-utilisateur. Selon votre scénario, choisissez l’une des actions suivantes :  
  
    1.  Connectez-vous avec l'Explorateur d'objets en utilisant l'authentification Windows (qui contient vos informations d'identification d'administrateur). Développez **Sécurité**, **Connexions**, puis double-cliquez sur votre propre connexion. Sur la page **Rôles serveur** , sélectionnez **sysadmin**, puis cliquez sur **OK**.  
  
    2.  Au lieu de vous connecter avec l'Explorateur d'objets, connectez-vous avec une fenêtre de requête en utilisant l'authentification Windows (qui contient vos informations d'identification d'administrateur). (Vous pouvez vous connecter de cette manière uniquement si vous ne vous êtes pas connecté avec l'Explorateur d'objets.) Exécutez le code (semblable au suivant) permettant d'ajouter une connexion d'authentification Windows qui est membre du rôle serveur fixe **sysadmin** . L’exemple suivant ajoute un utilisateur de domaine nommé `CONTOSO\PatK`.  
  
        ```  
        CREATE LOGIN [CONTOSO\PatK] FROM WINDOWS;  
        ALTER SERVER ROLE sysadmin ADD MEMBER [CONTOSO\PatK];  
        ```  
  
    3.  Si votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute en mode d'authentification mixte, connectez-vous avec une fenêtre de requête en utilisant l'authentification Windows (qui inclut vos informations d'identification d'administrateur). Exécutez le code (semblable au suivant) permettant de créer une connexion d'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est membre du rôle serveur fixe **sysadmin** .  
  
        ```  
        CREATE LOGIN TempLogin WITH PASSWORD = '************';  
        ALTER SERVER ROLE sysadmin ADD MEMBER TempLogin;  
        ```  
  
        > [!WARNING]  
        >  Remplacez ************ par un mot de passe fort.  
  
    4.  Si votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute en mode d’authentification mixte et que vous souhaitez réinitialiser le mot de passe du compte **sa** , connectez-vous avec une fenêtre de requête en utilisant l’authentification Windows (qui inclut vos informations d’identification d’administrateur). Modifiez le mot de passe du compte **sa** en respectant la syntaxe suivante.  
  
        ```  
        ALTER LOGIN sa WITH PASSWORD = '************';  
        ```  
  
        > [!WARNING]  
        >  Remplacez ************ par un mot de passe fort.  

7. Fermez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
8. Les étapes suivantes repassent SQL Server en mode multi-utilisateur. Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans le volet gauche, sélectionnez **Services SQL Server**.

9. Dans le volet droit, cliquez avec le bouton droit sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis cliquez sur **Propriétés**.  
  
10. Sous l’onglet **Paramètres de démarrage** , dans la zone **Paramètres existants** , sélectionnez `-m` , puis cliquez sur **Supprimer**.  
  
    > [!NOTE]  
    >  Certaines versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'ont pas d'onglet **Paramètres de démarrage** . Dans ce cas, sous l’onglet **Avancé** , double-cliquez sur **Paramètres de démarrage**. Les paramètres s'ouvrent dans une fenêtre très petite. Supprimez `;-m` que vous avez ajouté précédemment, puis cliquez sur **OK**.  
  
11. Cliquez avec le bouton droit sur le nom de votre serveur, puis cliquez sur **Redémarrer**. Veillez à redémarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent si vous l’avez arrêté avant de démarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur.
  
Vous devez désormais être en mesure de vous connecter normalement avec l'un des comptes qui est maintenant membre du rôle serveur fixe **sysadmin** .  
  
## <a name="see-also"></a>Voir aussi  

* [Configurer les options de démarrage de serveur](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)
* [Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md)  
