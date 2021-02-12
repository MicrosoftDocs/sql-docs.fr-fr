---
title: Configurer le compte d’exécution sans assistance (Gestionnaire de configuration) | Microsoft Docs
description: Reporting Services fournit un compte spécial utilisé pour le traitement de rapport sans assistance et pour l'envoi de demandes de connexion par le biais du réseau.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
helpviewer_keywords:
- no credentials option [Reporting Services]
- security [Reporting Services], unattended report processing
- unattended report processing [Reporting Services]
- credentials [Reporting Services]
- external data sources [Reporting Services]
- accounts [Reporting Services]
- reports [Reporting Services], processing
ms.assetid: 4e50733e-bd8c-4bf6-8379-98b1531bb9ca
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 549a1659bcbcf954a4039a152c5b372a758d0c74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014829"
---
# <a name="configure-the-unattended-execution-account-report-server-configuration-manager"></a>Configurer le compte d’exécution sans assistance (Gestionnaire de configuration du serveur de rapports)
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit un compte spécial utilisé pour le traitement de rapport sans assistance et pour l'envoi de demandes de connexion par le biais du réseau. Le compte est utilisé des façons suivantes :  
  
-   Envoyez les demandes de connexion sur le réseau pour les rapports qui utilisent l'authentification de base de données ou connectez-vous aux sources de données de rapport externes qui ne requièrent pas ou n'utilisent pas l'authentification. Pour plus d’informations, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).

-   Extraire des fichiers image externes qui sont utilisés dans un rapport. Si vous souhaitez utiliser un fichier image et que le fichier est inaccessible par le biais de l'accès Anonyme, vous pouvez configurer le compte de traitement de rapport sans surveillance et accorder au compte l'autorisation d'accéder au fichier.  
  
 Le traitement sans surveillance des rapports désigne tout processus d'exécution de rapport déclenché par un événement, d'actualisation de données ou piloté par planification, plutôt que par une demande d'utilisateur. Le serveur de rapports utilise le compte de traitement sans surveillance des rapports pour se connecter à l'ordinateur qui héberge la source de données externe. Ce compte est nécessaire car les informations d'identification du compte de service Report Server ne sont jamais utilisées pour la connexion à d'autres ordinateurs.  
  
> [!IMPORTANT]  
>  La configuration de ce compte est facultative. Cependant, si vous ne le configurez pas, vous limitez vos options pour la connexion à certaines sources de données et vous risquez de ne pas pouvoir récupérer de fichiers image à partir d'ordinateurs distants. Si vous configurez le compte, vous devez le maintenir à jour. Plus spécifiquement, si vous laissez un mot de passe expirer ou si les informations de compte sont modifiées dans Active Directory, vous rencontrerez l'erreur suivante lors du prochain traitement de rapport : « Échec de l'ouverture de session (rsLogonFailed) Échec d'ouverture de session : nom d'utilisateur inconnu ou mot de passe incorrect ». Une maintenance correcte du compte de traitement de rapport sans surveillance est essentielle, même si vous ne récupérez jamais de fichiers image ou si vous n'envoyez jamais de demandes de connexion à des ordinateurs externes. Si vous configurez le compte et constatez ultérieurement que vous ne l'utilisez pas, vous pouvez le supprimer afin d'éliminer des tâches de maintenance de compte routinières.  
  
## <a name="how-to-configure-the-account"></a>Comment configurer le compte  
 Vous devez utiliser un compte utilisateur de domaine. Pour assumer son rôle prévu, ce compte doit être différent de celui utilisé pour exécuter le service Report Server. Veillez à utiliser un compte ayant les autorisations minimales (l'accès en lecture seule avec les autorisations de connexion réseau est suffisant) et un accès limité aux seuls ordinateurs qui fournissent des sources de données et des ressources au serveur de rapports.  
  
 Pour spécifier le compte, vous pouvez utiliser l’outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou l’utilitaire **rsconfig** . La façon la plus simple de configurer le compte d'exécution sans surveillance consiste à exécuter l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et à spécifier les informations d'identification dans la page Compte d'exécution.  
  
1.  Démarrez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis connectez-vous à l'instance du serveur de rapports que vous voulez configurer. Pour obtenir des instructions, consultez [Gestionnaire de configuration du serveur de rapports &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2.  Sur la page Compte d'exécution, activez l'option **Spécifier un compte d'exécution**.  
  
3.  Tapez le compte et le mot de passe, tapez de nouveau le mot de passe, puis cliquez sur **Appliquer**.  
  
### <a name="using-rsconfig-utility"></a>Utilisation de l'utilitaire RSCONFIG.exe  
 Une autre méthode pour définir le compte consiste à recourir à l’utilitaire **rsconfig** . Pour spécifier le compte, utilisez l’argument **-e** de **rsconfig**. La spécification de l’argument **-e** pour **rsconfig** force l’utilitaire à écrire les informations de compte dans le fichier de configuration. Il n'est pas nécessaire de spécifier un chemin d'accès au fichier RSreportserver.config. Suivez ces étapes pour configurer le compte.  
  
1.  Créez ou sélectionnez un compte de domaine qui ne peut accéder qu'aux ordinateurs et aux serveurs qui fournissent des données ou des services à un serveur de rapports. Vous devez utiliser un compte bénéficiant d'autorisations réduites (telles que des autorisations de lecture seule).  
  
2.  Ouvrez une invite de commandes : dans le menu **Démarrer** , cliquez sur **Exécuter**, tapez **cmd**, puis cliquez sur **OK**.  
  
3.  Tapez la commande suivante pour configurer le compte sur une instance de serveur de rapports locale :  
  
     **rsconfig -e -u\<domain/username> -p\<password>**  
  
 **rsconfig -e** prend en charge d’autres arguments. Pour plus d’informations sur la syntaxe et pour obtenir des exemples de commande, consultez [Utilitaire rsconfig &#40;SSRS&#41;](../../reporting-services/tools/rsconfig-utility-ssrs.md).
 
### <a name="how-account-information-is-stored"></a>Comment les informations sur le compte sont stockées  
 Lorsque vous définissez le compte, les paramètres suivants sont spécifiés comme valeurs chiffrées dans le fichier RSreportserver.config sur une instance de serveur de rapports locale ou distante :  
  
```  
<UnattendedExecutionAccount>  
     <UserName></UserName>  
     <Password></Password>  
     <Domain></Domain>  
</UnattendedExecutionAccount>  
```  
  
 Une fois que vous les avez définies, vous ne pouvez pas les déchiffrer pour les consulter sous la forme de texte brut. Si vous n’entrez pas les valeurs correctement ou si vous oubliez les valeurs que vous avez spécifiées, vous devez recommencer la procédure à l’aide de l’outil de configuration de Reporting Services ou de la commande **rsconfig -e** .  
  
## <a name="how-to-use-the-unattended-report-processing-account"></a>Comment utiliser le compte de traitement de rapport sans surveillance  
 Pour extraire les fichiers image, le serveur de rapports utilise automatiquement le compte et aucune action spécifique n'est requise de votre part. Pour utiliser le compte pour vous connecter aux sources de données externes qui fournissent les données aux rapports, vous devez spécifier une option **Type d'informations d'identification** dans la page de propriétés de source de données de la source de données du rapport ou de la source de données partagée :  
  
-   Dans le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] ou sur un site SharePoint, sélectionnez l’option **Informations d’identification non requises** .  

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.
  
 Le compte de traitement des rapports sans assistance est utilisé principalement pour se connecter aux serveurs externes, et non comme connexion aux serveurs de base de données. Si vous souhaitez utiliser les informations d'identification du compte pour vous connecter à une base de données, vous devez spécifier ces informations dans la chaîne de connexion. Vous pouvez spécifier **Integrated Security=SSPI** si le serveur de base de données prend en charge la sécurité intégrée Windows et si le compte utilisé pour le traitement de rapport sans assistance dispose de l’autorisation de lecture dans la base de données. Sinon, vous devez entrer le nom d'utilisateur et le mot de passe dans la chaîne de connexion, où ces informations apparaissent en texte en clair à tout utilisateur qui peut modifier les propriétés de la connexion à la source de données.  
  
 Bien que rien ne vous empêche d'utiliser le compte de traitement de rapport sans assistance pour récupérer les données une fois la connexion établie, il n'est pas recommandé de le faire. Ce compte est prévu pour des fonctions bien spécifiques. Si vous l'utilisez pour récupérer des données, vous allez à l'encontre de sa fonction première.  
  
## <a name="how-to-maintain-the-unattended-report-processing-account"></a>Comment maintenir le compte de traitement de rapport sans surveillance  
 Une fois que vous avez défini le compte, vous devez vous assurer que le compte et le mot de passe sont maintenus à jour. Vous pouvez utiliser l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour mettre à jour les paramètres de configuration qui stockent les informations relatives à ce compte.  
  
1.  Démarrez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis connectez-vous à l'instance du serveur de rapports que vous voulez configurer.  
  
2.  Sur la page Compte d'exécution, vérifiez que l'option **Spécifier un compte d'exécution** est sélectionnée.  
  
3.  Tapez le nouveau compte ou mot de passe, tapez de nouveau le mot de passe, puis cliquez sur **Appliquer**.  
  
## <a name="how-to-delete-the-unattended-report-processing-account"></a>Comment supprimer le compte de traitement de rapport sans surveillance  
 Si vous n'utilisez pas le compte, vous pouvez le supprimer afin d'éliminer des tâches de maintenance de compte routinières.  
  
1.  Démarrez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis connectez-vous à l'instance du serveur de rapports que vous voulez configurer.  
  
2.  Sur la page Compte d'exécution, désactivez l'option **Spécifier un compte d'exécution**.  
  
3.  Cliquez sur **Appliquer**.  
  
 Les informations de compte sont supprimées du fichier RSReportServer.config.  
  
## <a name="see-also"></a>Voir aussi  
 [Gestionnaire de configuration du serveur de rapports (mode natif SSRS)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
  
  
