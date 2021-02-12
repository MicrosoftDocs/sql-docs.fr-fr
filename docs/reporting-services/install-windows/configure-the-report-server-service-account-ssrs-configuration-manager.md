---
title: Configurer le compte de service Report Server (Gestionnaire de configuration) | Microsoft Docs
description: Reporting Services est implémenté en tant que service unique qui contient un service web Report Server, un portail web et une application de traitement en arrière-plan utilisée pour le traitement des rapports planifié et la remise d'abonnement.
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.custom: seo-lt-2019, seo-mmd-2019
ms.date: 06/09/2020
ms.openlocfilehash: 530478c30885e603d73a5bd35fe3a759b607b836
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014909"
---
# <a name="configure-the-report-server-service-account-report-server-configuration-manager"></a>Configurer le compte de service de serveurs de rapports (Gestionnaire de configuration du serveur de rapports)

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est implémenté en tant que service unique qui contient un service Web Report Server, le [!INCLUDE[ssRSWebPortal-Non-Markdown](../../includes/ssrswebportal-non-markdown-md.md)]et une application de traitement en arrière-plan utilisée pour le traitement des rapports planifié et la remise d’abonnement. Cette rubrique explique comment le compte de service est configuré initialement et comment modifier le compte ou le mot de passe à l'aide de l'outil de configuration de Reporting Services.  
  
## <a name="initial-configuration"></a>Configuration initiale

 Le compte de service Report Server est défini pendant l'installation. Vous pouvez exécuter le service au moyen d’un compte d’utilisateur de domaine ou d’un compte intégré tel que le **compte de service virtuel**. Il n’y a pas de compte par défaut ; le compte que vous spécifiez dans la page **Comptes de service** de l’Assistant Installation devient le compte initial du service Report Server.  
  
> [!IMPORTANT]  
> Bien que le service Web Report Server et [!INCLUDE[ssRSWebPortal-Non-Markdown](../../includes/ssrswebportal-non-markdown-md.md)] soient des applications [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] distinctes, ils s’exécutent sous une architecture de service unique dans la même identité de processus Report Server.

> [!NOTE]  
> Les comptes de service Windows intégrés (Service local ou Service réseau) ne sont pas pris en charge comme comptes de service du serveur de rapports sur un ordinateur qui est contrôleur de domaine.
  
## <a name="changing-the-service-account"></a>Modification du compte de service

 Pour afficher et reconfigurer les informations sur le compte de service, utilisez toujours le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Les informations sur l'identité du service sont stockées en interne dans plusieurs emplacements. L'utilisation de l'outil garantit que toutes les références sont mises à jour en conséquence chaque fois que vous modifiez le compte ou le mot de passe. Le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] exécute les étapes supplémentaires suivantes pour s’assurer que le serveur de rapports reste disponible :  
  
- Il ajoute automatiquement le nouveau compte au groupe de serveurs de rapports créés sur l'ordinateur local. Ce groupe est spécifié dans les listes de contrôle d'accès qui assurent la sécurité des fichiers de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
- Il met à jour automatiquement les autorisations de connexion sur l’instance du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilisée pour héberger la base de données du serveur de rapports. Le nouveau compte est ajouté au rôle **RSExecRole**.  
  
     La connexion de base de données pour l’ancien compte n’est pas supprimée automatiquement. Assurez-vous de supprimer les comptes qui ne sont plus utilisés. Pour plus d’informations, consultez [Administrer une base de données du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md).  
  
     L’accord des autorisations de base de données à un nouveau compte de service a lieu uniquement si vous avez configuré en premier lieu la connexion de base de données du serveur de rapports de façon à utiliser le compte de service. Si vous avez configuré la connexion de base de données du serveur de rapports de façon à utiliser un compte d'utilisateur de domaine ou une connexion de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les informations de connexion ne sont pas affectées par la mise à jour du compte de service.  
  
- Il met à jour automatiquement la clé de chiffrement pour inclure les informations de profil du nouveau compte.  
  
    > [!NOTE]  
    > Si le serveur de rapports est intégré au déploiement avec montée en puissance parallèle, seul le serveur de rapports en cours de mise à jour est affecté. Il n'y a aucune incidence sur les clés de chiffrement des autres serveurs de rapports de ce déploiement.  

## <a name="to-configure-the-report-server-service-account"></a>Pour configurer le compte de service Report Server  
  
1. Démarrez le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puis connectez-vous au serveur de rapports.  
  
2. Dans la page Compte de service, sélectionnez l'option qui décrit le type de compte que vous souhaitez utiliser.  
  
3. Si vous avez sélectionné un compte d'utilisateur Windows, spécifiez le nouveau compte et le nouveau mot de passe. Le nom du compte ne peut pas comporter plus de 20 caractères ni les caractères spéciaux " / \ [ ] : ; | = , + * ? < > ' conformément aux règles de nommage des comptes d’utilisateur Windows.  
  
     Si le serveur de rapports est déployé dans un réseau qui prend en charge l’authentification Kerberos, vous devez inscrire le nom de principal du service du serveur de rapports avec le compte d’utilisateur de domaine que vous avez spécifié. Pour plus d’informations, consultez [Inscrire un nom de principal du service &#40;SPN&#41; pour un serveur de rapports](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md).  
  
4. Cliquez sur **Appliquer**.  
  
5. Lorsque vous êtes invité à sauvegarder la clé symétrique, tapez un nom de fichier et un emplacement pour la sauvegarde de la clé symétrique, entrez un mot de passe pour verrouiller et déverrouiller le fichier, puis cliquez sur **OK**.  
  
6. Si le serveur de rapports utilise le compte de service pour se connecter à la base de données du serveur de rapports, les informations de connexion sont mises à jour pour utiliser le nouveau compte ou le nouveau mot de passe. La mise à jour des informations de connexion nécessite que vous vous connectiez à la base de données. Si la boîte de dialogue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Connexion à la base de données** s’affiche, entrez les informations d’identification ayant l’autorisation de se connecter à la base de données, puis cliquez sur **OK**.  
  
7. Lorsque vous êtes invité à restaurer la clé symétrique, tapez le mot de passe spécifié à l’étape 5, puis cliquez sur **OK**.  
  
8. Examinez les messages d'état dans le volet Résultats pour vérifier que toutes les tâches se sont terminées avec succès.  
  
## <a name="choosing-an-account"></a>Choix d'un compte

 Pour de meilleurs résultats, spécifiez un compte qui a des autorisations de connexion réseau, avec un accès aux contrôleurs de domaine réseau et aux passerelles ou serveurs SMTP d'entreprise. Le tableau suivant fournit une synthèse des comptes et des recommandations pour leur utilisation.  
  
|Compte|Explication|  
|-------------|-----------------|  
|Comptes d'utilisateurs de domaine|Si vous avez un compte d'utilisateur de domaine Windows qui possède les autorisations minimales requises pour les opérations de serveur de rapports, vous devez l'utiliser.<br /><br /> Un compte d'utilisateur de domaine est recommandé car il isole le service Report Server des autres applications. L'exécution de plusieurs applications sous un compte partagé, tel que Service réseau, accroît le risque qu'un utilisateur malveillant prenne le contrôle du serveur de rapports car une violation de la sécurité pour l'une des applications peut facilement s'étendre à toutes les applications qui s'exécutent sous le même compte.<br /><br /> Si vous utilisez un compte d’utilisateur de domaine, vous devez changer régulièrement le mot de passe si votre organisation impose une stratégie d’expiration des mots de passe. Vous devrez peut-être également inscrire le service avec le compte d'utilisateur. Pour plus d’informations, consultez [Inscrire un nom de principal du service &#40;SPN&#41; pour un serveur de rapports](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md).<br /><br /> Évitez d'utiliser un compte d'utilisateur Windows local. En général, les comptes locaux ne disposent pas des autorisations suffisantes pour accéder aux ressources sur d’autres ordinateurs. Pour plus d’informations sur la manière dont l’utilisation d’un compte local limite la fonctionnalité du serveur de rapports, consultez [Considérations relatives à l’utilisation des comptes locaux](#localaccounts) dans cette rubrique.| 
| **Compte de service administré du groupe (gMSA)** | Les comptes de service administrés autonomes ont été introduits dans Windows Server 2008 R2 et Windows 7. Ce sont des comptes de domaine administrés qui fournissent une gestion automatique des mots de passe et une gestion simplifiée du nom de principal du service ainsi que la délégation de la gestion à d’autres administrateurs. Le **compte de service administré de groupe** fournit les mêmes fonctionnalités dans le domaine, mais les étend également sur plusieurs serveurs. |
|**Compte de service virtuel**|**Compte de service virtuel** représente le service Windows. Il s’agit d’un compte intégré doté des privilèges minimaux, qui a les autorisations d’ouverture de session sur le réseau. Ce compte est recommandé si vous n’avez pas de compte d’utilisateur de domaine disponible ou si vous voulez éviter toute interruption de service à cause des stratégies d’expiration de mot de passe.|  
|**Service réseau**|**Service réseau** est un compte intégré doté des privilèges minimaux, qui a les autorisations d’ouverture de session sur le réseau. <br /><br /> Si vous sélectionnez **Service réseau**, essayez de réduire le nombre d’autres services qui s’exécutent sous le même compte. Une violation de la sécurité pour une des applications compromet la sécurité de toutes les autres applications qui s’exécutent sous le même compte.|  
|**Service local**|**Service local** est un compte prédéfini qui s’apparente à un compte d’utilisateur Windows local authentifié. Les services qui s’exécutent au moyen du compte **Service local** accèdent aux ressources réseau dans le cadre d’une session nulle, sans informations d’identification. Ce compte ne convient pas aux scénarios de déploiement intranet où le serveur de rapports doit se connecter à une base de données de serveur de rapports distante ou à un contrôleur de domaine réseau afin d'authentifier un utilisateur avant d'ouvrir un rapport ou de traiter un abonnement.|  
|**Système local**|**Système local** est un compte doté de privilèges élevés qui n’est pas obligatoire pour l’exécution d’un serveur de rapports. Évitez ce compte pour les installations de serveur de rapports. Choisissez un compte de domaine ou **Service réseau** à la place.|  
  
## <a name="considerations-for-using-local-accounts"></a><a name="localaccounts"></a> Considérations relatives à l’utilisation des comptes locaux

 La principale considération liée à l'utilisation des comptes locaux concerne l'obligation, pour le serveur de rapports, de pouvoir accéder aux serveurs de base de données, serveurs de messagerie et contrôleurs de domaine distants. Si vous configurez le serveur de rapports pour qu'il s'exécute en tant que compte d'utilisateur Windows local, Service local ou Système Local, vous introduisez des considérations qui doivent être prises en compte lors de la définition d'autres paramètres de configuration et de la création et remise d'abonnement :  
  
- L’exécution du service sous un compte local limite ultérieurement vos options si vous configurez une connexion à une base de données de serveur de rapports distante. En particulier, si vous utilisez une base de données de serveur de rapports distante, vous devez configurer la connexion afin d’utiliser un compte d’utilisateur de domaine ou un utilisateur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a l’autorisation de connexion à l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] distante.  
  
- L’exécution du service sous un compte local introduit de nouvelles conditions requises en matière de création d’abonnement. Le serveur de rapports stocke des informations sur l'utilisateur qui crée l'abonnement. Si l’utilisateur crée l’abonnement alors qu’il est connecté sous un compte de domaine, le service Report Server tente de se connecter à un contrôleur de domaine pour authentifier l’utilisateur lors du traitement de l’abonnement. Si le service s’exécute sous un compte local, la demande d’authentification échoue quand le serveur de rapports tente d’envoyer la demande à un contrôleur de domaine distant. Pour contourner cette limitation, vous pouvez utiliser une extension personnalisée d'authentification basée sur les formulaires ou faire en sorte que tous les utilisateurs se connectent à un serveur de rapports sous un compte d'utilisateur local.  
  
- L’exécution du service sous un compte local introduit de nouvelles conditions requises en matière de remise des abonnements. Certaines extensions de remise ont les informations du compte d'utilisateur dans la définition de l'abonnement. Si vous envoyez des rapports à des adresses e-mail basées sur des comptes d’utilisateur de domaine et que vous exécutez le service Report Server sous un compte local, il ne peut pas accéder à un contrôleur de domaine distant pour résoudre le compte e-mail cible.  
  
- Les comptes de service Windows intégrés (Service local ou Service réseau) ne sont pas pris en charge comme comptes de service du serveur de rapports sur un ordinateur qui est contrôleur de domaine.  
  
Les instructions et les liens suivants de cette section peuvent vous aider à choisir l'approche la plus appropriée pour votre déploiement.  
  
- [Configurez les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
- [Guide de planification de la sécurité des services et des comptes de service](https://www.zubairalexander.com/blog/services-and-service-accounts-security-planning-guide/).  
  
## <a name="updating-an-expired-password"></a>Mise à jour d'un mot de passe  expiré

 Si le service Report Server s’exécute sous un compte de domaine et que le mot de passe expire avant que vous puissiez le mettre à jour dans le Gestionnaire de configuration du serveur de rapports, le service ne démarre pas tant que vous n’avez pas spécifié un nouveau mot de passe.  
  
 Si le mot de passe du compte de service pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)] expire, l’erreur **rsReportServerDatabaseUnavailable** se produit si vous tentez de vous connecter au serveur de rapports. La réinitialisation du mot de passe résout cette erreur.  
  
## <a name="troubleshooting-service-identity-update-errors"></a>Dépannage des erreurs de mise à jour de l'identité du service

 La modification de l’identité du service déclenche une série d’événements qui incluent le redémarrage du service, la mise à jour de la clé de chiffrement protégée par mot de passe, la mise à jour des réservations d’URL et la mise à jour des informations de connexion à la base de données du serveur de rapports si vous utilisez le compte de service pour vous connecter à la base de données du serveur de rapports. Vous pouvez surveiller l'état de ces événements en consultant les notifications dans le volet Résultats en bas de page. Si des erreurs se produisent pendant ce processus, vous pouvez essayer de les résoudre à l'aide des techniques suivantes :  
  
- Si la clé symétrique ne peut pas être restaurée, vous pouvez essayer de la restaurer manuellement en utilisant **Restaurer** dans la page Clés de chiffrement. Si cela ne fonctionne pas, envisagez de supprimer le contenu chiffré. Vous devez recréer les abonnements et les informations de connexion à la source de données, mais le reste du contenu reste disponible. Pour plus d’informations, consultez [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
- Si le service ne démarre pas, redémarrez-le manuellement en utilisant l’application console Services dans Outils d’administrateur.  
  
- Les erreurs de réservation d'URL peuvent se produire lorsque vous mettez à jour le compte de service. Chaque réservation d'URL comporte un descripteur de sécurité incluant une liste de contrôle d'accès discrétionnaire (DACL) qui accorde l'autorisation au compte de service d'accepter les demandes sur l'URL. Lorsque vous mettez à jour le compte, l'URL doit être recréée pour mettre à jour la liste DACL avec les nouvelles informations de compte. Si la réservation d’URL ne peut pas être recréée et que vous savez que le compte est valide, essayez de redémarrer l’ordinateur. Si l'erreur persiste, essayez d'utiliser un autre compte.  
  
## <a name="next-steps"></a>Étapes suivantes

 [Configurer des URL de serveurs de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md) [Gestionnaire de configuration du serveur de rapports &#40;Mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
