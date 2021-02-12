---
title: Se connecter à un serveur de rapports dans Management Studio | Microsoft Docs
description: Découvrez comment vous connecter à n’importe quel serveur de la famille SQL Server, et comment parcourir graphiquement son contenu à l’aide de l’Explorateur d’objets dans SQL Server Management Studio.
ms.date: 05/07/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
f1_keywords:
- sql13.swb.connecttors.connectionproperties.f1
- sql13.swb.connecttors.login.f1
- sql13.swb.connection.login.reportserver.f1
helpviewer_keywords:
- report servers [Reporting Services], connections
- connections [Reporting Services], report server
- registering report servers
- report servers [Reporting Services], registering
- Connect to Server dialog box, Reporting Services
ms.assetid: c875ff87-ee7d-443a-a702-bdb4b6c27c6e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 735c7805d1deba9861a6e51249acf9928d97db7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076780"
---
# <a name="connect-to-a-report-server-in-management-studio"></a>Se connecter à un serveur de rapports dans Management Studio

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] fournit l’Explorateur d’objets, qui vous permet de vous connecter à n’importe quel serveur de la famille [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de parcourir son contenu sous forme graphique. Pour Reporting Services, vous pouvez utiliser l’Explorateur d’objets afin d’effectuer les tâches suivantes :

- activer les fonctionnalités du serveur de rapports ;

- définir les valeurs par défaut du serveur et configurer des définitions de rôles ;

- gérer les travaux en cours d'exécution ;

- Gérer les planifications du travail.

 Vous pouvez vous connecter à un serveur de rapports qui s'exécute en mode natif ou en mode intégré SharePoint. La syntaxe de connexion et les types d’opérations que vous pouvez effectuer varient en fonction du mode du serveur de rapports et de vos autorisations. Si vous ne pouvez pas vous connecter au serveur de rapports ou si vous avez des problèmes pour effectuer des tâches spécifiques, vous n’avez probablement pas les autorisations suffisantes ou bien le nom du serveur de rapports spécifié est incorrect. Pour plus d’informations sur les autorisations et sur la syntaxe de connexion, consultez le tableau à la fin de cet article.

 Vous ne pouvez pas utiliser l’Explorateur d’objets pour afficher ou gérer le contenu du serveur de rapports. La gestion du contenu s’effectue par le biais du portail web si le serveur de rapports s’exécute en mode natif, ou par le biais d’un site SharePoint s’il s’exécute en mode intégré SharePoint.

 L'Explorateur d'objets vous permet d'ouvrir des connexions vers plusieurs instances de serveur dans le même espace de travail, à condition que les serveurs soient inscrits dans le même groupe de serveurs. Avant de pouvoir vous connecter à une instance de serveur de rapports dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], le serveur doit être inscrit. Si le serveur de rapports est déjà inscrit, vous pouvez ignorer cette étape. Les instructions relatives à l’inscription des serveurs de rapports sont fournies à la fin de cet article.

### <a name="to-connect-to-a-native-mode-report-server"></a>Pour se connecter à un serveur de rapports en mode natif

1. Si l’Explorateur d’objets n’est pas ouvert, sélectionnez-le dans le menu **Affichage**.

2. Sélectionnez **Se connecter** pour afficher la liste des types de serveurs, puis sélectionnez **Reporting Services**.

3. Dans la boîte de dialogue **Se connecter au serveur** , entrez le nom de l’instance du serveur de rapports. Les noms des instances du serveur de rapports sont basés sur les noms des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par défaut, le nom d'instance d'une instance de serveur de rapports locale est simplement le nom de l'ordinateur. Si vous avez installé le serveur de rapports en tant qu’instance nommée, utilisez la syntaxe suivante pour spécifier le serveur : *\<servername>[\\<nom_instance\>]* .

4. Sélectionnez le **Type d’authentification**. Si vous utilisez l’authentification Windows, vous vous connectez à l’aide de vos informations d’identification. Si vous sélectionnez l'authentification de base ou l'authentification par formulaire, tapez les informations relatives au compte et au mot de passe.  
  
5. Sélectionnez **Connecter**. Le serveur de rapports apparaît dans l'Explorateur d'objets.  

6. Cliquez avec le bouton droit sur le nœud du serveur afin de définir les propriétés système et les paramètres par défaut du serveur. Pour plus d’informations, consultez [Définir les propriétés du serveur de rapports &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md).

### <a name="to-connect-to-a-sharepoint-integrated-mode-report-server"></a>Pour se connecter à un serveur de rapports en mode intégré SharePoint  

1. Si l’Explorateur d’objets n’est pas ouvert, sélectionnez-le dans le menu **Affichage**.

2. Sélectionnez **Se connecter** pour afficher la liste des types de serveurs, puis sélectionnez **Reporting Services**.

3. Dans la boîte de dialogue **Se connecter au serveur** , entrez une URL d’un site SharePoint. L’exemple suivant illustre la syntaxe : `https://<web server>/sites/<site>`.

4. Sélectionnez le **Type d’authentification**. Si vous utilisez l'authentification Windows, vous devez vous connecter à l'aide de vos informations d'identification. Si vous sélectionnez l'authentification de base ou l'authentification par formulaire, tapez les informations relatives au compte et au mot de passe.

5. Sélectionnez **Connecter**. Le serveur de rapports apparaît dans l'Explorateur d'objets.

6. Cliquez avec le bouton droit sur le nœud du serveur afin de définir les propriétés système et les paramètres par défaut du serveur. Pour plus d’informations, consultez [Définir les propriétés du serveur de rapports &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md).

### <a name="to-register-a-report-server"></a>Pour inscrire un serveur de rapports

1. Si vous ne pouvez pas vous connecter à un serveur de rapports, cela signifie que vous n’avez pas l’autorisation d’y accéder ou qu’il n’est pas inscrit. Pour inscrire le serveur, sélectionnez le menu **Affichage** > **Serveurs inscrits**.

2. Sélectionnez l’icône **Reporting Services**.

3. Cliquez avec le bouton droit sur **Reporting Services** > **Nouveau** > **Inscription du serveur**. La boîte de dialogue **Nouvelle inscription de serveur** s’affiche.

4. Dans la zone **Nom du serveur**, entrez une valeur. Vous spécifiez la valeur en fonction du mode du serveur :

    - Pour un serveur de rapports en mode natif, tapez le nom de l'instance du serveur de rapports. Les noms des instances du serveur de rapports sont basés sur les noms des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par défaut, le nom d'instance d'une instance de serveur de rapports locale est simplement le nom de l'ordinateur. Si vous avez installé le serveur de rapports en tant qu’instance nommée, utilisez la syntaxe suivante pour spécifier le serveur : *\<servername>[\\<nom_instance\>]* .

    - Pour un serveur de rapports qui s'exécute en mode intégré SharePoint, le serveur de connexion est le site SharePoint auquel le serveur de rapports est connecté. Connectez-vous au site SharePoint afin de pouvoir voir les niveaux d’autorisation. Les autorisations contrôlent l’accès au contenu du serveur de rapports et aux opérations. Vous pouvez spécifier n'importe quel site dans la collection de sites. L’exemple suivant illustre la syntaxe : `https://mysharepointsite`.

5. Pour **Authentification**, sélectionnez le mode d’authentification que le serveur de rapports utilise déjà.

   - Si vous utilisez la sécurité par défaut, choisissez **Authentification Windows**.
   - Si vous avez installé et déployé une extension de sécurité personnalisée, choisissez **Authentification par formulaires**.
   - Si vous avez configuré le serveur de rapports pour l’utilisation de l’authentification de base, choisissez **Authentification de base**.
   - Si le serveur de rapports est configuré pour le mode intégré SharePoint, choisissez **Authentification Windows**.

6. Sélectionnez **Tester** pour vérifier la connexion.

7. À l’invite, sélectionnez **OK**, puis **Enregistrer**.

## <a name="connection-syntax-and-permissions"></a>Syntaxe de connexion et autorisations

 Le tableau suivant résume la syntaxe de connexion, les étapes et les autorisations nécessaires pour effectuer des tâches spécifiques.

 Quand vous indiquez [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en tant que type de serveur dans la boîte de dialogue **Se connecter au serveur** , vous pouvez spécifier un nom de serveur de rapports ou un point de terminaison au service web.

|Se connecter à|   Tâches   |   Autorisations   |
|----------|-----------|-----------------|  
|Serveur de rapports en mode natif, connecté en tant qu’instance par défaut ou nommée :<br /><br /> \<server name>\<_instance><br /><br /> La connexion au serveur de rapports est établie à travers le fournisseur WMI de Report Server.|Consultez et définissez les propriétés du serveur, ainsi que les valeurs par défaut.<br /><br /> Consultez et annulez les travaux.<br /><br /> Créez et gérez les planifications partagées.<br /><br /> Créez, modifiez ou supprimez les définitions de rôles.|Assignées au rôle Administrateur système.|  
|Serveur de rapports en mode natif, connecté en tant qu’instance par défaut ou nommée, par le biais du point de terminaison au service web Report Server :<br /><br /> `https://<servername>/reportserver`<br /><br /> La spécification de l'URL du serveur de rapports offre un autre moyen de se connecter au serveur de rapports.|Consultez et définissez les propriétés du serveur, ainsi que les valeurs par défaut.<br /><br /> Consultez et annulez les travaux.<br /><br /> Créez et gérez les planifications partagées.<br /><br /> Créez, modifiez ou supprimez les définitions de rôles.|Assignées au rôle Administrateur système.|  
|Serveur de rapports en mode intégré SharePoint, connecté via le site SharePoint :<br /><br /> `https://<webserver>/<SharePointSite>`|Consultez et définissez les propriétés du serveur, ainsi que les valeurs par défaut.<br /><br /> Consultez et annulez les travaux.<br /><br /> Créez et gérez des planifications partagées définies pour le site auquel vous êtes connecté.<br /><br /> Consultez les niveaux d’autorisation définis pour le site auquel vous êtes connecté.|Niveau d’autorisation Contrôle total pour le site SharePoint auquel vous êtes connecté.|
|Serveur de rapports en mode intégré SharePoint, connecté via le nom de l'instance du serveur de rapports :<br /><br /> \<server name>\<_instance>|Consultez et définissez les propriétés du serveur, ainsi que les valeurs par défaut.<br /><br /> Consultez et annulez les travaux.|Niveau d'autorisation Contrôle total pour le site SharePoint intégré au serveur de rapports.<br /><br /> Notez que quand vous vous connectez au serveur de rapports plutôt qu’au site SharePoint, le nombre des tâches que vous pouvez effectuer est réduit. En effet, le serveur de rapports peut retourner uniquement les données d’application stockées ou gérées dans la base de données du serveur de rapports, et non dans les bases de données de contenu et de configuration SharePoint.|

## <a name="see-also"></a>Voir aussi

 [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)  
 [Reporting Services pour SQL Server Management Studio &#40;SSRS&#41;](../../reporting-services/tools/reporting-services-in-sql-server-management-studio-ssrs.md)
