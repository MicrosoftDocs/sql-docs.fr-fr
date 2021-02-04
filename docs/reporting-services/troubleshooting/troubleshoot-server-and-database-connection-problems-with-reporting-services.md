---
title: Résoudre les problèmes de connexion à un serveur et à une base de données
description: Dans cet article, vous allez diagnostiquer et résoudre les problèmes qui se produisent lors de la connexion à un serveur de rapports. Vous allez également en savoir plus sur les messages de type « Erreur inattendue ».
ms.date: 12/16/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
ms.assetid: 8bbb88df-72fd-4c27-91b7-b255afedd345
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 74bf6f0da0daa0df248243ffbce505c75c55db5e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076321"
---
# <a name="troubleshoot-server--database-connection-problems-with-reporting-services"></a>Résoudre les problèmes de connexion à un serveur et à une base de données avec Reporting Server
Utilisez cette rubrique pour résoudre les problèmes que vous rencontrez lors de la connexion à un serveur de rapports. En outre, elle fournit des informations sur les messages de type « Erreur inattendue ». Pour plus d’informations sur la configuration de la source de données et des informations de connexion au serveur de rapports, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md) et [Configurer une connexion à la base de données du serveur de rapports (Gestionnaire de configuration du serveur de rapports)](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
## <a name="cannot-create-a-connection-to-data-source-datasourcename-rserroropeningconnection"></a>Impossible de créer une connexion à la source de données 'nom_source_données'. (rsErrorOpeningConnection)  
Ceci est une erreur générique qui se produit lorsque le serveur de rapports ne peut pas ouvrir une connexion à une source de données externe qui fournit des données à un rapport. Cette erreur apparaît avec un second message d'erreur qui indique la cause sous-jacente. Les erreurs supplémentaires suivantes peuvent apparaître avec **rsErrorOpeningConnection**.  
  
### <a name="login-failed-for-user-username"></a>Échec de la connexion pour l'utilisateur 'nom_utilisateur'  
L'utilisateur n'est pas autorisé à accéder à la source de données. Si vous utilisez une base de données SQL Server, vérifiez que l’utilisateur dispose d’une connexion d’utilisateur de base de données valide. Pour plus d’informations sur la création d’un utilisateur de base de données ou d’une connexion SQL Server, voir [Créer un utilisateur de base de données](../../relational-databases/security/authentication-access/create-a-database-user.md) et [Créer une connexion SQL Server](../../relational-databases/security/authentication-access/create-a-login.md).  
  
### <a name="login-failed-for-user-nt-authorityanonymous-logon"></a>Échec de la connexion pour l'utilisateur 'NT AUTHORITY\ANONYMOUS LOGON'  
Cette situation se produit lorsque des informations d'identification sont transmises via plusieurs connexions d'ordinateurs. Si vous utilisez l'authentification Windows, et que le protocole Kerberos version 5 n'est pas activé, cette erreur se produit lorsque les informations d'identification sont transmises via plusieurs connexions d'ordinateurs. Pour contourner cette erreur, envisagez d'utiliser des informations d'identification stockées ou des informations d'identification demandées. Pour plus d’informations sur la façon de contourner ce problème, voir [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
### <a name="an-error-has-occurred-while-establishing-a-connection-to-the-server"></a>Une erreur s'est produite lors de l'établissement d'une connexion au serveur.  
Lors de la connexion à SQL Server, cet échec peut être dû au fait que les paramètres par défaut de SQL Server n'autorisent pas les connexions à distance. (fournisseur : Fournisseur de canaux nommés, erreur : 40 - Impossible d’ouvrir une connexion à SQL Server). Cette erreur est retournée par l’instance du moteur de base de données qui héberge la base de données du serveur de rapports. Dans la plupart des cas, cette erreur se produit à la suite d’un arrêt du service SQL Server. Si vous utilisez SQL Server Express with Advanced Services ou une instance nommée, cette erreur se produit si l’URL du serveur de rapports ou la chaîne de connexion à la base de données du serveur de rapports ne sont pas correctes. Pour résoudre ces problèmes, procédez comme suit :  
  
* Vérifiez que le service SQL Server (**MSSQLSERVER**) a démarré. Sur l’ordinateur qui héberge l’instance du moteur de base de données, cliquez sur Démarrer, Outils d’administration, Services, puis faites défiler la liste jusqu’à SQL Server (**MSSQLSERVER**). Si le service n’a pas démarré, cliquez dessus avec le bouton droit, puis sélectionnez Propriétés. Dans Type de démarrage, sélectionnez Automatique, puis cliquez successivement sur Appliquer, Démarrer, puis OK.   
* Assurez-vous que l'URL du serveur de rapports et la chaîne de connexion à la base de données du serveur de rapports sont correctes. Si les services Reporting Services ou le moteur de base de données ont été installés en tant qu’instance nommée, la chaîne de connexion par défaut créée pendant l’installation comprend le nom d’instance. Par exemple, si vous avez installé une instance par défaut de SQL Server Express with Advanced Services sur un serveur nommé DEVSRV01, l’URL du portail web est DEVSRV01\Reports$SQLEXPRESS. En outre, le nom du serveur de base de données dans la chaîne de connexion ressemble à DEVSRV01\SQLEXPRESS. Pour plus d’informations sur les URL et les chaînes de connexion à la source de données pour SQL Server Express, voir [Reporting Services dans SQL Server Express with Advanced Services](/previous-versions/sql/sql-server-2008-r2/ms365166(v=sql.105)). Pour vérifier la chaîne de connexion de la base de données du serveur de rapports, démarrez l’outil de configuration de Reporting Services, puis consultez la page Installation de la base de données.  
  
### <a name="a-connection-cannot-be-made-ensure-that-the-server-is-running"></a>Impossible d'établir une connexion. Vérifiez que le serveur fonctionne.  
Cette erreur est retournée par le fournisseur ADOMD.NET. Cette erreur peut se produire pour différentes raisons. Si vous avez spécifié le serveur comme hôte local (« localhost »), essayez de spécifier le nom du serveur à la place. Cette erreur peut également se produire s'il est impossible d'allouer de la mémoire à la nouvelle connexion. Pour plus d’informations, voir l’ [l’Article 912017 de Base de connaissances CORRECTIF, Message d’erreur lorsque vous vous connectez à une instance de SQL Server 2005 Analysis Services :](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/912017).  
  
Si l’erreur inclut également « Hôte inconnu », cela indique que le serveur Analysis Services n’est pas disponible ou qu’il refuse la connexion. Si le serveur Analysis Services est installé en tant qu’instance nommée sur un ordinateur distant, vous pouvez être amené à exécuter le service SQL Server Browser pour obtenir le numéro de port que cette instance utilise.  
  
### <a name="report-services-soap-proxy-source"></a>(Source de proxy SOAP de Reporting Services)  
Si vous obtenez cette erreur lors de la génération d'un modèle de rapport et si la section des informations supplémentaires inclut « SQL Server n'existe pas ou l'accès est refusé », il est possible que vous rencontriez les conditions suivantes :  
* La chaîne de connexion pour les sources de données inclut « localhost ».  
* TCP/IP est désactivé pour le service SQL Server.  
  
Pour résoudre cette erreur, vous pouvez modifier la chaîne de connexion de manière à utiliser le nom du serveur ou vous pouvez activer TCP/IP pour le service. Procédez comme suit pour activer TCP/IP :  
  
1. Démarrez le Gestionnaire de configuration SQL Server.  
2. Développez **Configuration du réseau SQL Server**.  
3. Sélectionnez **Protocoles pour MSSQLSERVER**.  
4. Cliquez avec le bouton droit sur **TCP/IP**, puis sélectionnez **Activer**.  
5. Sélectionnez **Services SQL Server**.  
6. Cliquez avec le bouton droit sur **SQL Server (MSSQLSERVER)** , puis sélectionnez **Redémarrer**.  
  
## <a name="wmi-error-when-connecting-to-a-report-server-in-management-studio"></a>Erreur WMI lors de la connexion à un serveur de rapports dans Management Studio  
Par défaut, Management Studio utilise le fournisseur WMI (Windows Management Instrumentation) de Reporting Services pour établir une connexion au serveur de rapports. Si le fournisseur WMI n'est pas installé correctement, vous obtiendrez l'erreur suivante lors de la tentative de connexion au serveur de rapports :  
  
Impossible de se connecter à \<your server name>. Soit le fournisseur WMI de Reporting Services n'est pas installé, soit il est configuré de manière incorrecte (Microsoft.SqlServer.Management.UI.RSClient).  
  
Pour résoudre cette erreur, vous devez réinstaller le logiciel. Dans tous les autres cas, en guise de solution temporaire, vous pouvez vous connecter au serveur de rapports via le point de terminaison SOAP :  
  
* Dans la boîte de dialogue **Se connecter au serveur** de **, dans** Nom du serveur, tapez l’URL du serveur de rapports. Par défaut, il s’agit de `https://<your server name>/reportserver`. Ou si vous utilisez SQL Server 2008 Express with Advanced Services, il s’agit de `https://<your server name>/reportserver$sqlexpress`.  
  
Pour résoudre l’erreur et vous connecter à l’aide du fournisseur WMI, vous devez soit exécuter le programme d’installation afin de réparer Reporting Services, soit réinstaller l’application.  
  
## <a name="connection-error-where-login-failed-due-to-unknown-user-name-or-bad-password"></a>Erreur de connexion en raison d'un nom d'utilisateur inconnu ou d'un mot de passe incorrect  
Une erreur **rsReportServerDatabaseLogonFailed** peut se produire si, pour la connexion du serveur de rapports à la base de données du serveur de rapports, vous utilisez un compte de domaine dont le mot de passe a été modifié.   
  
Le texte complet de l'erreur est : « Le serveur de rapports ne peut pas ouvrir une connexion à la base de données du serveur de rapports. Échec de l’ouverture de session (**rsReportServerDatabaseLogonFailed**). Échec d'ouverture de session : nom d'utilisateur inconnu ou mot de passe incorrect. »  
  
Si vous redéfinissez le mot de passe, vous devez mettre à jour la connexion. Pour plus d’informations, consultez [Configurer une connexion à la base de données du serveur de rapports (Gestionnaire de configuration du serveur de rapports)](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
## <a name="the-report-server-cannot-open-a-connection-to-the-report-server-database-rsreportserverdatabaseunavailable"></a>Le serveur de rapports ne peut pas ouvrir une connexion à la base de données du serveur de rapports. (rsReportServerDatabaseUnavailable).  
Message complet : Le serveur de rapports ne peut pas ouvrir une connexion à la base de données du serveur de rapports. Une connexion à la base de données est requise pour toutes les demandes et le traitement. (rsReportServerDatabaseUnavailable)  
Cette erreur se produit lorsque le serveur de rapports ne peut pas se connecter à la base de données relationnelle SQL Server qui procure un stockage interne au serveur. La connexion à la base de données du serveur de rapports est gérée via l’outil de configuration de Reporting Services. Vous pouvez exécuter l'outil, ouvrir la page Installation de la base de données et corriger les informations de connexion. L'utilisation de l'outil pour mettre à jour les informations de connexion correspond à une méthode recommandée ; l'outil garantit que les paramètres dépendants sont mis à jour et que les services sont redémarrés. Pour plus d’informations, voir [Configurer une connexion à la base de données du serveur de rapports](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md) et [Configurer le compte de service Report Server](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  
  
Cette erreur peut également se produire si l’instance du moteur de base de données qui héberge la base de données du serveur de rapports n’est pas configurée pour les connexions à distance. La connexion a distance est activée par défaut dans certaines éditions de SQL Server. Pour vérifier si elle est activée sur l’instance du moteur de base de données SQL Server que vous utilisez, exécutez l’outil Gestionnaire de Configuration SQL Server. Vous devez activer à la fois TCP/IP et les canaux nommés. Un serveur de rapports utilise les deux protocoles. Pour obtenir des instructions sur la manière d’activer des connexions distantes, voir la section « Comment configurer des connexions distantes à la base de données du serveur de rapports » dans [Configurer un serveur de rapports pour l’administration à distance](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md).  
  
Si l'erreur inclut le texte supplémentaire ci-dessous, le mot de passe a expiré sur le compte utilisé pour exécuter l'instance du moteur de base de données : « Une erreur s'est produite lors de l'établissement d'une connexion au serveur. Lors de la connexion à SQL Server, cet échec peut être dû au fait que les paramètres par défaut de SQL Server n’autorisent pas les connexions à distance. (**fournisseur : interfaces réseau SQL Server, erreur : 26 - Erreur lors de la localisation du serveur/de l’instance spécifiés**. » Pour résoudre cette erreur, réinitialisez le mot de passe.   
  
## <a name="rpc-server-is-not-listening"></a>« Le serveur RPC n'est pas à l'écoute »  
Le service Report Server utilise le serveur RPC (Remote Procedure Call) pour certaines opérations. Si vous obtenez le message d'erreur « Le serveur RPC n'est pas à l'écoute », assurez-vous que le service Report Server est en cours d'exécution.  
  
## <a name="unexpected-error-general-network-error"></a>Erreur inattendue (erreur réseau générale)  
Cette erreur signale un problème de connexion à la source de données. Vérifiez la chaîne de connexion et assurez-vous également de disposer du droit d'accès à la source de données. Si vous utilisez l'authentification Windows pour accéder à une source de données, vous devez être autorisé à accéder à l'ordinateur qui héberge la source de données.  
  
## <a name="unable-to-grant-database-access-in-sharepoint-central-administration"></a>Impossible d'accorder l'accès à la base de données dans l'Administration centrale SharePoint  
Lorsque vous avez configuré le service Reporting Services pour l’intégrer à un produit ou à une technologie SharePoint sur Windows Vista ou Windows Server 2008, il se peut que vous receviez le message d’erreur suivant lorsque vous essayez d’accorder l’accès via la page **Accorder l’accès à la base de données** dans l’Administration centrale de SharePoint : « Impossible d’établir une connexion à l’ordinateur. »  
  
Cela est dû au fait que le contrôle de compte d’utilisateur (UAC) dans Windows Vista et Windows Server 2008 nécessite l’acceptation explicite d’un administrateur pour élever et utiliser le jeton d’administrateur lors de tâches nécessitant des autorisations d’administrateur. Toutefois, sans ce cas, le service Administration de Windows SharePoint Services ne peut pas être élevé pour accorder l’accès du ou des comptes de service Reporting Services aux bases de données de configuration et de contenu SharePoint.  
  
Dans SQL Server 2008 Reporting Services, seul le compte de service Report Server nécessite l’accès à la base de données. Dans SQL Server 2005 Reporting Services SP2, le compte de service Windows Report Server et le compte de service web Report Server nécessitent tous deux l’accès à la base de données. Pour plus d’informations sur le compte de service du serveur de rapports dans SQL Server 2008, voir Compte de service (configuration de Reporting Services).  
  
Il existe deux solutions de contournement pour ce problème.   
1.  Dans une solution de contournement, vous pouvez désactiver temporairement le contrôle de compte d'utilisateur et utiliser l'Administration centrale de SharePoint pour accorder l'accès.  
> [!IMPORTANT]  
> Soyez vigilant si vous désactivez le contrôle de compte d'utilisateur pour contourner ce problème, et activez immédiatement le contrôle de compte d'utilisateur après avoir accordé l'accès à la base de données dans l'Administration centrale de SharePoint. Si vous ne souhaitez pas désactiver le contrôle de compte d'utilisateur, utilisez la deuxième solution de contournement fournie dans cette section. Pour plus d'informations sur l’utilisation du contrôle de compte d'utilisateur, consultez la documentation produit de Windows.  
2. Dans l’autre solution de contournement, vous pouvez accorder manuellement l’accès à la base de données aux comptes de service Reporting Services. Pour accorder l’accès en ajoutant les comptes de service Reporting Services au groupe Windows et aux rôles de base de données appropriés, vous pouvez procéder comme suit. Cette procédure s’applique au compte de service du serveur de rapports dans SQL Server 2008 Reporting Services. Si vous exécutez SQL Server 2005 Reporting Services, effectuez la procédure pour le compte de service Windows du serveur de rapports et le compte de service web Report Server.   
  
### <a name="to-manually-grant-database-access"></a>Pour accorder manuellement l'accès à la base de données  
  
1. Ajoutez le compte de service du serveur de rapports au groupe Windows WSS_WPG sur l’ordinateur exécutant Reporting Services.  
2. Connectez-vous à l'instance de base de données qui héberge les bases de données de configuration et de contenu SharePoint, puis créez une connexion de base de données SQL pour le compte de service Report Server.  
3. Ajoutez la connexion de base de données SQL aux rôles de base de données suivants :  
  
* Rôle db_owner dans la base de données de contenu WSS  
* Rôle WSS_Content_Application_Pools dans la base de données SharePoint_Config  
  
## <a name="unable-to-connect-to-the-reports-and-reportserver-directories-when-the-report-server-databases-are-created-on-a-virtual-sql-server-that-runs-in-a-microsoft-cluster-services-mscs-cluster"></a>Impossible de se connecter aux répertoires /reports et /reportserver lorsque les bases de données du serveur de rapports sont créées sur un serveur SQL Server virtuel qui s'exécute dans un cluster Microsoft Cluster Services (MSCS)  
Lorsque vous créez les bases de données du serveur de rapports, **ReportServer** et **ReportServerTempDB**, sur un serveur SQL Server virtuel s’exécutant dans un cluster MSCS, il se peut que le nom distant au format `<domain>\<computer_name>$` ne soit pas inscrit dans SQL Server en tant que connexion. Si vous avez configuré le compte de service du serveur de rapports en tant que compte nécessitant ce nom distant pour les connexions, les utilisateurs ne peuvent pas se connecter aux répertoires /reports et /reportserver dans Reporting Services. Par exemple, le compte Windows intégré NetworkService requiert ce nom distant. Pour éviter ce problème, utilisez un compte de domaine explicite ou un compte de connexion SQL Server pour vous connecter aux bases de données du serveur de rapports.  
    
  ## <a name="see-also"></a>Voir aussi  
[Prise en charge des navigateurs pour Reporting Services et Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)  
[Erreurs et événements (Reporting Services)](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
[Résoudre les problèmes de récupération des données avec des rapports Reporting Services](../../reporting-services/troubleshooting/troubleshoot-data-retrieval-issues-with-reporting-services-reports.md)  
[Résoudre les problèmes d’abonnements et de remise de Reporting Services](../../reporting-services/troubleshooting/troubleshoot-reporting-services-subscriptions-and-delivery.md)  
  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect-md.md)]
