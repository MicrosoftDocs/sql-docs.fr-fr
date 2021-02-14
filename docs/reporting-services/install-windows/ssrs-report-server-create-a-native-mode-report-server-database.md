---
title: Créer une base de données du serveur de rapports en mode natif (Gestionnaire de configuration) | Microsoft Docs
description: Reporting Services en mode natif utilise une base de données SQL Server pour le stockage interne. La base de données est un composant nécessaire et elle permet de stocker les rapports publiés, les modèles, les sources de données partagées, les données de session, les ressources et les métadonnées du serveur.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
helpviewer_keywords:
- report servers [Reporting Services], databases
- databases [Reporting Services], creating
ms.assetid: 81b9f4ad-800b-4688-8b47-a5a83dc8ff10
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: dab7b82e27ba57974a4bdb8e679164f6316ecd4a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067124"
---
# <a name="create-a-native-mode-report-server-database-report-server-configuration-manager"></a>Créer une base de données du serveur de rapports en mode natif (Gestionnaire de configuration du serveur de rapports)

[!INCLUDE[ssrs-appliesto-sql2016-preview](../../includes/ssrs-appliesto-sql2016-preview.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif utilise une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le stockage interne. La base de données est un composant nécessaire et elle permet de stocker les rapports publiés, les modèles, les sources de données partagées, les données de session, les ressources et les métadonnées du serveur.  

Pour créer une base de données du serveur de rapports ou modifier la chaîne de connexion ou les informations d'identification, utilisez les options de la page Base de données du Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
## <a name="when-to-create-or-configure-the-report-server-databases"></a>Quand créer ou configurer les bases de données de serveur de rapports  
 Vous devez créer et configurer la base de données du serveur de rapports si vous avez installé le serveur de rapports en mode fichiers uniquement.  
  
 Si vous avez installé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans la configuration par défaut pour le mode natif, la base de données du serveur de rapports a été créée et configurée automatiquement lorsque l’instance du serveur de rapports a été installée. Vous pouvez utiliser le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour afficher ou modifier les paramètres que le programme d'installation a configurés automatiquement.  
  
##  <a name="before-you-start"></a><a name="rsdbrequirements"></a> Avant de commencer  
 La création ou la configuration d'une base de données de serveur de rapports est un processus comportant plusieurs étapes. Avant de créer la base de données du serveur de rapports, pensez à la façon dont vous souhaitez spécifier les éléments suivants :  
  
 **Sélectionner un serveur de base de données**  
 Consultez les versions prises en charge du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] dans [Créer une base de données du serveur de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md).  
  
 **Activer les connexions TCP/IP**  
 Activez les connexions TCP/IP pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Certaines éditions du [!INCLUDE[ssDE](../../includes/ssde-md.md)] n’activent pas TCP/IP par défaut. Les instructions sont fournies dans cette rubrique.  
  
 **Ouvrir le port pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
 Pour un serveur distant, si vous utilisez le logiciel de pare-feu, vous devez ouvrir le port sur lequel le [!INCLUDE[ssDE](../../includes/ssde-md.md)] écoute.  
  
 **Choisir les informations d'identification du serveur de rapports**  
 Décidez comment le serveur de rapports se connectera aux bases de données de serveur de rapports. Les types d'informations d'identification incluent le compte d'utilisateur de domaine, le compte d'utilisateur de la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou le compte de service Report Server.  
  
 Ces informations d'identification sont chiffrées et stockées dans le fichier RSReportServer.config. Le serveur de rapports utilise ces informations d'identification pour les connexions continues à la base de données du serveur de rapports. Si vous souhaitez utiliser un compte d'utilisateur Windows ou un compte d'utilisateur de base de données, veillez bien à en spécifier un qui existe déjà. Bien que le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] crée une connexion et définisse les autorisations nécessaires, il ne crée pas de compte automatiquement. Pour plus d’informations, consultez [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
 **Choisir une langue du serveur de rapports**  
 Choisissez une langue à spécifier pour le serveur de rapports. Les noms de rôle prédéfinis, les descriptions et les dossiers Mes rapports n'apparaissent pas en différentes langues lorsque les utilisateurs se connectent au serveur à l'aide de différentes versions linguistiques d'un navigateur.  
  
 **Vérifier les informations d'identification pour créer et mettre en service la base de données**  
 Vérifiez que les informations d’identification de votre compte ont l’autorisation de créer des bases de données sur l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Ces informations d’identification sont utilisées pour une connexion unique afin de créer la base de données du serveur de rapports et **RSExecRole**. Si une connexion n'existe pas déjà, une connexion d'utilisateur de la base de données est créée pour le compte utilisé par le serveur de rapports pour se connecter à la base de données. Vous pouvez vous connecter sous le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows avec lequel vous êtes connecté ou vous pouvez entrer une connexion de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="to-enable-access-to-a-remote-report-server-database"></a>Pour activer l'accès à une base de données distante du serveur de rapports  
  
1.  Si vous utilisez une instance distante du [!INCLUDE[ssDE](../../includes/ssde-md.md)] , ouvrez une session sur le serveur de base de données pour vérifier ou activer les connexions TCP/IP.  
  
2.  Dans le menu **Démarrer**, pointez successivement sur **Tous les programmes**, **Microsoft SQL Server** et **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
3.  Ouvrez **Configuration du réseau SQL Server**.  
  
4.  Sélectionnez l’instance de base de données.  
  
5.  Cliquez avec le bouton droit sur **TCP/IP** , puis sélectionnez **Activer**.  
  
6.  Redémarrez le service.  
  
7.  Ouvrez votre logiciel de pare-feu, ainsi que le port sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] écoute. Pour l'instance par défaut, il s'agit généralement du port 1433 pour les connexions TCP/IP. Pour plus d’informations, consultez [Configurer un pare-feu Windows pour accéder au moteur de base de données](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md).  
  
### <a name="to-create-a-local-report-server-database"></a>Pour créer une base de données locale du serveur de rapports  
  
1.  Démarrez le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis connectez-vous à l'instance du serveur de rapports pour laquelle vous créez la base de données. Pour plus d’informations, consultez [Gestionnaire de configuration du serveur de rapports &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2.  Dans la page Base de données, sélectionnez **Modifier la base de données**.  
  
3.  Sélectionnez **Créer une nouvelle base de données de serveur de rapports**, puis sélectionnez **Suivant**.  
  
4.  Connectez-vous à l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] que vous allez utiliser pour créer et héberger la base de données du serveur de rapports :  
  
    1.  Tapez l'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] que vous souhaitez utiliser. L'Assistant affiche un [!INCLUDE[ssDE](../../includes/ssde-md.md)] local qui s'exécute comme instance par défaut en cas de disponibilité. Sinon, vous devez entrer le serveur et l'instance à utiliser. Les instances nommées sont spécifiées au format suivant : \<servername>\\<nom_instance\>.  
  
    2.  Entrez les informations d’identification utilisées pour une connexion unique au [!INCLUDE[ssDE](../../includes/ssde-md.md)] en vue de créer les bases de données du serveur de rapports. Pour plus d'informations sur la façon dont ces informations d'identification sont utilisées, consultez [Avant de commencer](#rsdbrequirements) dans cette rubrique.  
  
    3.  Pour valider la connexion au serveur, cliquez sur **Tester la connexion** .  
  
    4.  Sélectionnez **Suivant**.  
  
5.  Spécifiez les propriétés utilisées pour créer la base de données. Pour plus d'informations sur la façon dont ces propriétés sont utilisées, consultez [Avant de commencer](#rsdbrequirements) dans cette rubrique :  
  
    1.  Tapez le nom de la base de données du serveur de rapports. Une base de données temporaire est créée parallèlement à la base de données primaire. Pensez à utiliser un nom descriptif qui vous aidera à vous rappeler comment la base de données est utilisée. Notez que le nom que vous spécifiez sera utilisé pendant toute la durée de vie de la base de données. Vous ne pouvez pas renommer une base de données du serveur de rapports après sa création.  
  
    2.  Sélectionnez la langue dans laquelle vous souhaitez que les définitions de rôle et Mes rapports apparaissent.  
  
    3.  Le mode du serveur de rapports est toujours défini à **Natif**.  
  
    4.  Sélectionnez **Suivant**.  
  
6.  Spécifiez les informations d'identification que le serveur de rapports doit utiliser pour se connecter à la base de données du serveur de rapports.  
  
    1.  Spécifiez le type d'authentification :  
  
         Sélectionnez **Informations d'identification de la base de données** pour vous connecter à l'aide d'une connexion de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] déjà définie. L'utilisation d'informations d'identification de base de données est recommandée si le serveur de rapports se trouve sur un ordinateur qui est dans un domaine différent, un domaine non approuvé ou derrière un pare-feu.  
  
         Sélectionnez **Informations d’identification Windows** si vous avez un compte d’utilisateur de domaine avec les privilèges les plus faibles qui a l’autorisation de se connecter à l’ordinateur et au serveur de base de données.  
  
         Sélectionnez **Informations d'identification du service** si vous souhaitez que le serveur de rapports se connecte à l'aide de son compte de service. Avec cette option, le serveur se connecte en utilisant la sécurité intégrée ; les informations d'identification ne sont pas chiffrées ou stockées.  
  
    2.  Sélectionnez **Suivant**.  
  
7.  Examinez les informations de la page Résumé pour vérifier que les paramètres sont corrects, puis cliquez sur **Suivant**.  
  
8.  Vérifiez la connexion en sélectionnant une URL de la page URL de Report Server. Les URL doivent être définies dans l'ordre pour que ce test fonctionne. Si la connexion à la base de données du serveur de rapports est valide, vous pouvez voir la hiérarchie des dossiers du serveur de rapports. Pour plus d’informations, consultez [Vérifier une installation de Reporting Services](../../reporting-services/install-windows/verify-a-reporting-services-installation.md).  

## <a name="change-database-credentials"></a>Modifier les informations d’identification d’une base de données

Le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit l'Assistant Modification des informations d'identification pour vous guider à travers les étapes de reconfiguration du compte que le serveur de rapports utilise pour se connecter à la base de données du serveur de rapports. Lorsque vous modifiez les informations d'identification, le Gestionnaire de configuration met à jour toutes les autorisations et informations de connexion de base de données sur le serveur de base de données pour la base de données du serveur de rapports en cours d'utilisation par le serveur de rapports. 

1.  Démarrez le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis connectez-vous à l'instance du serveur de rapports pour laquelle vous créez la base de données. Pour plus d’informations, consultez [Gestionnaire de configuration du serveur de rapports &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2.  Dans la page Base de données, sélectionnez **Modifier les informations d’identification**. 

3.  Connectez-vous à l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] que vous allez utiliser pour créer et héberger la base de données du serveur de rapports :  
  
    1.  Entrez les informations d’identification utilisées pour une connexion unique au [!INCLUDE[ssDE](../../includes/ssde-md.md)] en vue de créer les bases de données du serveur de rapports. Pour plus d'informations sur la façon dont ces informations d'identification sont utilisées, consultez [Avant de commencer](#rsdbrequirements) dans cette rubrique.  
  
    2.  Pour valider la connexion au serveur, cliquez sur **Tester la connexion** .  
  
    3.  Sélectionnez **Suivant**.  

4.  Spécifiez les informations d'identification que le serveur de rapports doit utiliser pour se connecter à la base de données du serveur de rapports.  
  
    1.  Spécifiez le type d'authentification :  
  
         Sélectionnez **Informations d'identification de la base de données** pour vous connecter à l'aide d'une connexion de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] déjà définie. L'utilisation d'informations d'identification de base de données est recommandée si le serveur de rapports se trouve sur un ordinateur qui est dans un domaine différent, un domaine non approuvé ou derrière un pare-feu.  
  
         Sélectionnez **Informations d’identification Windows** si vous avez un compte d’utilisateur de domaine avec les privilèges les plus faibles qui a l’autorisation de se connecter à l’ordinateur et au serveur de base de données.  
  
         Sélectionnez **Informations d'identification du service** si vous souhaitez que le serveur de rapports se connecte à l'aide de son compte de service. Avec cette option, le serveur se connecte en utilisant la sécurité intégrée ; les informations d'identification ne sont pas chiffrées ou stockées.  
  
    2.  Sélectionnez **Suivant**. 

5. Passez en revue les paramètres, puis sélectionnez **Suivant**.

6. Une fois les modifications effectuées, sélectionnez **Terminer**.

## <a name="next-steps"></a>Étapes suivantes

[Configurer une connexion à la base de données du serveur de rapports](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)   
[Gérer un serveur de rapports Reporting Services (SSRS) en mode natif](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
[Gestionnaire de configuration service Web Report Server](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
