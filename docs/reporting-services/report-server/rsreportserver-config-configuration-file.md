---
title: Fichier de configuration RSReportServer.config | Microsoft Docs
description: Apprenez-en davantage sur le fichier de configuration qui stocke les paramètres utilisés par le service web Report Server et le traitement en arrière-plan.
ms.date: 05/01/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: 60e0a0b2-8a47-4eda-a5df-3e5e403dbdbc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7566e003e9b93377f54a3775ae8ca6ecf119c1f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024223"
---
# <a name="rsreportserverconfig-configuration-file"></a>Fichier de configuration RSReportServer.config
Le fichier [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]**RsReportServer.config** stocke les paramètres utilisés par le service Web Report Server et le traitement en arrière-plan. Toutes les applications [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] s'exécutent au sein d'un processus unique qui lit les paramètres de configuration stockés dans le fichier RSReportServer.config. Les serveurs de rapports en mode natif et en mode SharePoint utilisent le fichier RSReportServer.config. Toutefois, les deux modes n'utilisent pas les mêmes paramètres dans le fichier de configuration. La version en mode SharePoint du fichier est moins volumineuse car de nombreux paramètres du mode SharePoint sont stockés dans des bases de données de configuration SharePoint plutôt que dans le fichier. Cette rubrique décrit le fichier de configuration par défaut installé en mode natif ou en mode SharePoint, et certains paramètres et comportements importants qui sont contrôlés par le fichier de configuration.  

En mode SharePoint, le fichier de configuration contient les paramètres qui s'appliquent à toutes les instances d'application de service exécutées sur cet ordinateur. La base de données de configuration SharePoint contient les paramètres de configuration qui s'appliquent aux applications de service spécifiques. Les paramètres qui sont stockés dans la base de données de configuration et qui sont gérés par les pages de gestion SharePoint, peuvent être différents pour chaque application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Les paramètres sont présentés ci-dessous dans l'ordre dans lequel ils apparaissent dans le fichier de configuration qui est installé par défaut. Pour obtenir des instructions sur la modification de ce fichier, consultez [Modifier un fichier de configuration Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
 
##  <a name="file-location"></a><a name="bkmk_file_location"></a> Emplacement du fichier  

RSReportServer.config se trouve dans les dossiers suivants, selon le mode du serveur de rapports :  


  
### <a name="native-mode-report-server"></a>Serveur de rapports en mode natif 

 
**[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)]** [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)]

```  
C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer  
```

**[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)]** [!INCLUDE[ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)]

```  
C:\Program Files\Microsoft SQL Server Reporting Services\SSRS\ReportServer
```  

**[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)]** [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

```  
C:\Program Files\Microsoft Power BI Report Server\PBIRS\ReportServer
```  
  
### <a name="sharepoint-mode-report-server"></a>Serveur de rapports en mode SharePoint 

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.
  
```  
C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting  
```  
 
Pour plus d’informations sur la modification du fichier, consultez [Modifier un fichier de configuration Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
##  <a name="general-configuration-settings-rsreportserverconfig"></a><a name="bkmk_generalconfiguration"></a> Paramètres de configuration généraux (rsreportserver.config)  
 Le tableau suivant fournit des informations sur les paramètres de configuration généraux qui apparaissent dans la première partie du fichier. Les paramètres sont présentés dans l'ordre dans lequel ils apparaissent dans le fichier de configuration. La dernière colonne du tableau indique si le paramètre s’applique à un serveur de rapports en mode natif **(N)** , à un serveur de rapports en mode SharePoint **(S)** ou aux deux.  
  
> [!NOTE]  
>  Dans cette rubrique, « entier maximal » désigne la valeur INT_MAX de 2147483647.  Pour plus d'informations, consultez [Limites des ressources](https://msdn.microsoft.com/library/296az74e\(v=vs.110\).aspx) (https://msdn.microsoft.com/library/296az74e(v=vs.110).aspx).  
  
|Paramètre|Description|Mode|  
|-------------|-----------------|----------|  
|**Dsn**|Spécifie la chaîne de connexion au serveur de base de données qui héberge la base de données du serveur de rapports. Cette valeur est chiffrée et ajoutée au fichier de configuration lorsque vous créez la base de données du serveur de rapports. Pour SharePoint, les informations de connexion de la base de données sont issues de la base de données de configuration SharePoint.|N,S|  
|**ConnectionType**|Détermine le type d'informations d'identification utilisé par le serveur de rapports pour se connecter à la base de données de serveur de rapports. Les valeurs possibles sont **Default** et **Impersonate**. **Default** est spécifiée si le serveur de rapports est configuré pour utiliser un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou le compte de service pour se connecter à la base de données du serveur de rapports. **Impersonate** est spécifiée si le serveur de rapports utilise un compte Windows pour se connecter à la base de données du serveur de rapports.|N|  
|**LogonUser, LogonDomain, LogonCred**|Stocke le domaine, le nom d'utilisateur et le mot de passe d'un compte de domaine utilisé par un serveur de rapports pour se connecter à une base de données du serveur de rapports. Des valeurs sont créées pour **LogonUser**, **LogonDomain** et **LogonCred** quand la configuration de la connexion du serveur de rapports prévoit l’utilisation d’un compte de domaine. Pour plus d’informations sur la connexion d’une base de données à un serveur de rapports, consultez [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).|N|  
|**InstanceID**|Identificateur de l'instance du serveur de rapports. Les noms des instances du serveur de rapports sont basés sur les noms des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette valeur indique un nom d'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par défaut, cette valeur est **MSRS12** _\<instancename>_ . Ne modifiez pas ce paramètre. Voici un exemple de la valeur complète : `<InstanceId>MSRS13.MSSQLSERVER</InstanceId>`<br /><br /> Voici un exemple en mode SharePoint :<br /><br /> `<InstanceId>MSRS12.@Sharepoint</InstanceId>`|N,S|  
|**InstallationID**|Identificateur de l'installation du serveur de rapports que crée le programme d'installation. Cette valeur est définie sur un GUID. Ne modifiez pas ce paramètre.|N|  
|**SecureConnectionLevel**|Spécifie le degré auquel les appels de service web doivent utiliser le protocole TLS (Transport Layer Security), anciennement SSL (Secure Sockets Layer). Ce paramètre est utilisé à la fois pour le service Web Report Server et le portail web. Cette valeur est définie lorsque vous configurez une URL pour utiliser le protocole HTTP ou HTTPS dans l'outil de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Dans SQL Server 2008 R2, SecureConnectionLevel est un commutateur d’activation ou de désactivation. Pour les versions antérieures à SQL Server 2008 R2 la plage de valeurs valides va de 0 à 3, où 0 est le moins sécurisé. Pour plus d’informations, consultez [Méthode ConfigurationSetting - SetSecureConnectionLevel](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setsecureconnectionlevel.md), [Utilisation des méthodes de service Web sécurisées](../../reporting-services/report-server-web-service/net-framework/using-secure-web-service-methods.md) et [Configurer des connexions TLS sur un serveur de rapports en mode natif](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).|N,S|
|**DisableSecureFormsAuthenticationCookie**|La valeur par défaut est False.<br /><br /> Spécifie s'il faut désactiver l'utilisation forcée du cookie employé pour que l'authentification par formulaire et personnalisée soit considérée comme sécurisée. Depuis SQL Server 2012, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] marque automatiquement les cookies d'authentification par formulaire utilisés avec les extensions d'authentification personnalisée comme cookies sécurisés lors de l'envoi au client. En modifiant cette propriété, les administrateurs de serveurs de rapports et les auteurs d'extensions de sécurité personnalisées peuvent revenir au comportement précédent, lequel autorise l'auteur d'une extension de sécurité personnalisée à déterminer s'il faut marquer le cookie en tant que cookie sécurisé. Il est recommandé d'utiliser des cookies sécurisés pour l'authentification par formulaire afin d'éviter les attaques par relecture et les renifleurs réseau.|N|  
|**CleanupCycleMinutes**|Spécifie au bout de combien de minutes les anciennes sessions et les instantanés expirés sont supprimés des bases de données du serveur de rapports. La plage de valeurs valides s’étend de 1 à l’entier maximal. La valeur par défaut est de 10. |N,S|  
|**MaxActiveReqForOneUser**|Spécifie le nombre maximal de rapports qu'un utilisateur peut traiter en même temps. Une fois la limite atteinte, les demandes de traitement de rapport supplémentaires sont refusées. Les valeurs valides vont de 1 à un entier maximal. Valeur par défaut : 20.<br /><br /> Notez que le traitement de la plupart des requêtes étant extrêmement rapide, il est très improbable qu'un seul utilisateur puisse cumuler plus de 20 connexions ouvertes à un moment donné. Si les utilisateurs ouvrent simultanément plus de 15 rapports nécessitant un traitement intensif, vous serez amené à augmenter cette valeur.<br /><br /> Ce paramètre est ignoré pour les serveurs de rapports qui s'exécutent en mode intégré SharePoint.|N,S|  
|**MaxActiveReqForAnonymous**|Spécifie le nombre maximal de demandes anonymes qui peuvent être en cours de traitement simultanément. Une fois la limite atteinte, les demandes de traitement supplémentaires sont refusées. Les valeurs valides vont de 1 à un entier maximal. Par défaut, la valeur est 200.
|**DatabaseQueryTimeout**|Spécifie au bout de combien de secondes une connexion à la base de données du serveur de rapports expire. Cette valeur est transmise à la propriété System.Data.SQLClient.SQLCommand.CommandTimeout. Les valeurs valides sont comprises entre 0 et 2147483647. La valeur par défaut est 120. La valeur 0 spécifie un temps d'attente illimité et, par conséquent, n'est pas recommandée.|N|  
|**AlertingCleanupCycleMinutes**|Valeur par défaut : 20.<br /><br /> Détermine la fréquence à laquelle le nettoyage des données temporaires stockées dans la base de données d’alertes doit avoir lieu.|S|  
|**AlertingDataCleanupMinutes**|La valeur par défaut est 360.<br /><br /> Détermine la durée pendant laquelle les données de session utilisées pour la création et la modification d'une définition d'alerte sont conservées dans la base de données d'alerte. L'intervalle par défaut est de 6 heures.|S|  
|**AlertingExecutionLogCleanup** Minutes|La valeur par défaut est 10080.<br /><br /> Détermine la durée de conservation des valeurs du journal d'exécution des alertes. La valeur par défaut est 7 jours.|S|  
|**AlertingMaxDataRetentionDays**|La valeur par défaut est 180.<br /><br /> Détermine la durée de conservation des données d'alerte requises pour éviter les messages d'alerte dupliqués lorsque les données de l'alerte n'ont pas changé.|S|  
|**RunningRequestsScavengerCycle**|Spécifie la fréquence à laquelle sont annulées les demandes orphelines et expirées. Cette valeur est exprimée en secondes. La plage de valeurs valides s'étend de 0 à un entier maximal. La valeur par défaut est 60.|N,S|  
|**RunningRequestsDbCycle**|Spécifie d’une part la fréquence d’évaluation des travaux en cours par le serveur de rapports (qui vérifie si les délais d’exécution prévus pour les rapports ont été dépassés) et, d’autre part, le moment auquel les informations sur les travaux en cours doivent être présentées dans la page Gérer les travaux du portail web. Cette valeur est exprimée en secondes. Les valeurs valides sont comprises entre 0 et 2147483647. La valeur par défaut est 60.|N,S|  
|**RunningRequestsAge**|Spécifie l'intervalle en secondes au bout duquel l'état d'un travail en cours d'exécution passe de l'état Nouveau à En cours d'exécution. Les valeurs valides sont comprises entre 0 et 2147483647. La valeur par défaut est 30.|N,S|  
|**MaxScheduleWait**|Spécifie la durée en secondes pendant laquelle le service Windows Report Server attend que le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ait mis à jour une planification lorsque l' **Heure de la prochaine exécution** est demandée. Les valeurs valides sont comprises entre 1 et 60.<br /><br /> Dans le fichier de configuration par défaut, MaxScheduleWait a la valeur **5**.<br /><br /> Si le serveur de rapports ne trouve pas ou n'arrive pas à lire le fichier de configuration, le serveur attribue à MaxScheduleWait la valeur par défaut « 1 ».|N,S|  
|**DisplayErrorLink**|Indique si un lien vers le site Aide et support de [!INCLUDE[msCoName](../../includes/msconame-md.md)] s'affiche en cas d'erreur. Ce lien apparaît dans les messages d'erreur. Les utilisateurs peuvent cliquer sur le lien afin d'afficher le contenu mis à jour du message d'erreur sur le site. Les valeurs valides sont **True** (valeur par défaut) et **False**.|N,S|  
|**WebServiceuseFileShareStorage**|Indique si les instantanés temporaires et les rapports mis en cache (créés par le service Web Report Server pour la durée d'une session utilisateur) doivent être stockés dans le système de fichiers. Les valeurs possibles sont **True** et **False** (valeur par défaut). Si la valeur est false, les données temporaires sont stockées dans la base de données reportservertempdb.|N,S|  
|**ProcessTimeout**|Spécifie le nombre de secondes pendant lesquelles le contrôle de processus du serveur de rapports doit attendre la fin de toute opération d’activité de service avant d’arrêter le service. La plage de valeurs valides s'étend de 0 à un entier maximal. La valeur par défaut est 150. Ce paramètre est désactivé par défaut et peut être activé en supprimant la syntaxe de commentaire (```<!-- and -->```).|N|  
|**ProcessTimeoutGcExtension**|Spécifie le nombre de secondes pendant lesquelles le contrôle de processus du serveur de rapports doit attendre la fin d’une opération d’activité de service avant d’arrêter le service. Ce paramètre s’applique uniquement si un nettoyage de la mémoire .NET est en cours et que la valeur ProcessTimeout a été atteinte. La plage de valeurs valides s'étend de 0 à un entier maximal. La valeur par défaut est 30. Ce paramètre est désactivé par défaut et peut être activé en supprimant la syntaxe de commentaire (```<!-- and -->```).|N|  
|**WatsonFlags**|Précise la quantité d'informations consignées dans un journal pour les conditions d'erreur signalées à [!INCLUDE[msCoName](../../includes/msconame-md.md)].<br /><br /> 0x0430 = vidage complet<br /><br /> 0x0428 = vidage minimal<br /><br /> 0x0002 = aucun vidage|N,S|  
|**WatsonDumpOnExceptions**|Spécifie une liste des exceptions que vous souhaitez signaler dans un journal des erreurs. Cela est utile lorsque vous avez un problème récurrent et que vous souhaitez créer un vidage avec des informations à envoyer à [!INCLUDE[msCoName](../../includes/msconame-md.md)] à des fins d'analyse. La création de vidages affecte les performances ; vous devez donc modifier ce paramètre uniquement lorsque vous diagnostiquez un problème.|N,S|  
|**WatsonDumpExcludeIfContainsExceptions**|Spécifie une liste des exceptions que vous ne souhaitez pas signaler dans un journal des erreurs. Cela est utile lorsque vous diagnostiquez un problème et que vous ne souhaitez pas que le serveur crée des vidages pour une exception spécifique.|N,S|  
  
##  <a name="urlreservations-rsreportserverconfig-file"></a><a name="bkmk_URLReservations"></a> URLReservations (fichier RSReportServer.config)  
 **URLReservations** définit l’accès HTTP au service Web Report Server et au portail web pour l’instance actuelle. Les URL sont réservées et stockées dans HTTP.SYS lorsque vous configurez le serveur de rapports.  
  
> [!WARNING]  
>  Pour le mode SharePoint, les réservations d'URL sont configurées dans l'Administration centrale de SharePoint. Pour plus d’informations, consultez [Configurer le mappage des accès de substitution (https://technet.microsoft.com/library/cc263208(office.12).aspx)](https://technet.microsoft.com/library/cc263208\(office.12\).aspx).  
  
 Ne modifiez pas directement les réservations d'URL dans le fichier de configuration. Utilisez toujours le Gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou le fournisseur WMI de Report Server pour créer ou modifier des réservations d'URL dans le cadre d'un serveur de rapports en mode natif. Si vous modifiez les valeurs du fichier de configuration, vous risquez d'endommager la réservation, ce qui provoquera des erreurs sur le serveur au moment de l'exécution ou laissera des réservations orphelines dans HTTP.SYS, car ces dernières ne seront pas supprimées si vous désinstallez le logiciel. Pour plus d’informations, consultez [Configurer des URL de serveurs de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md) et [URL des fichiers de configuration &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/urls-in-configuration-files-ssrs-configuration-manager.md).  
  
 **URLReservations** est un élément facultatif. S'il n'est pas présent dans le fichier RSReportServer.config, le serveur peut ne pas être configuré. S’il est spécifié, tous les éléments enfants sauf **AccountName** sont requis.  
  
 La dernière colonne du tableau indique si le paramètre s'applique à un serveur de rapports en mode natif (N), à un serveur de rapports en mode SharePoint (S) ou aux deux cas de figure.  
  
|Paramètre|Description|Mode|  
|-------------|-----------------|----------|  
|**Application**|Contient les paramètres des applications [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|N|  
|**Nom**|Spécifie les applications [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Les valeurs valides sont ReportServerWebService ou ReportManager.|N|  
|**VirtualDirectory**|Spécifie le nom du répertoire virtuel de l'application.|N|  
|**URLs, URL**|Contient une ou plusieurs réservations d'URL pour l'application.|N|  
|**UrlString**|Spécifie la syntaxe d'URL valide pour HTTP.SYS. Pour plus d’informations sur la syntaxe, consultez [Syntaxe de réservation d’URL &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/url-reservation-syntax-ssrs-configuration-manager.md).|N|  
|**AccountSid**|Spécifie l'identificateur de sécurité (SID) du compte pour lequel la réservation d'URL a été créée. Il doit s'agir du compte sous lequel le service Report Server s'exécute. Si le SID ne correspond pas au compte de service, le serveur de rapports peut ne pas être en mesure d'écouter les demandes sur cette URL.|N|  
|**AccountName**|Spécifie un nom de compte lisible qui correspond à **AccountSid**. Il n'est pas utilisé, mais il apparaît dans le fichier afin que vous puissiez facilement identifier le compte de service pour le compte utilisé pour la réservation d'URL.|N|  
  
##  <a name="authentication-rsreportserverconfig-file"></a><a name="bkmk_Authentication"></a> Authentication (fichier RSReportServer.config)  
 **Authentication** spécifie un ou plusieurs types d’authentification acceptés par le serveur de rapports. Les paramètres et valeurs par défaut sont un sous-ensemble des paramètres et valeurs possibles pour cette section. Seuls les paramètres par défaut sont ajoutés automatiquement. Si vous souhaitez ajouter d'autres paramètres, vous devez utiliser un éditeur de texte pour ajouter la structure de l'élément au fichier RSReportServer.config et définir les valeurs.  
  
 Les valeurs par défaut sont **RSWindowsNegotiate** et **RSWindowsNTLM** avec **EnableAuthPersistance** défini sur la valeur **True**:  
  
```  
   <Authentication>  
      <AuthenticationTypes>  
         <RSWindowsNegotiate/>  
         <RSWindowsNTLM/>  
      </AuthenticationTypes>  
      <EnableAuthPersistence>true</EnableAuthPersistence>  
   </Authentication>  
```  
  
 Toutes les autres valeurs doivent être ajoutées manuellement. Pour obtenir des informations et des exemples supplémentaires, consultez [Authentification avec le serveur de rapports](../../reporting-services/security/authentication-with-the-report-server.md).  
  
 La dernière colonne du tableau suivant indique si le paramètre s'applique à un serveur de rapports en mode natif (N), à un serveur de rapports en mode SharePoint (S) ou aux deux cas de figure.  
  
|Paramètre|Description|Mode|  
|-------------|-----------------|----------|  
|**AuthenticationTypes**|Spécifie un ou plusieurs types d'authentification. Les valeurs autorisées sont : **RSWindowsNegotiate**, **RSWindowsKerberos**, **RSWindowsNTLM**, **RSWindowsBasic** et **Custom**.<br /><br /> Les types **RSWindows** et **Custom** s’excluent mutuellement.<br /><br /> **RSWindowsNegotiate**, **RSWindowsKerberos**, **RSWindowsNTLM** et **RSWindowsBasic** sont cumulatifs et peuvent être utilisés ensemble, comme illustré dans l’exemple de valeur par défaut plus haut dans cette section.<br /><br /> La spécification de plusieurs types d'authentification est nécessaire si vous vous attendez à recevoir des requêtes de diverses applications ou navigateurs clients, qui utilisent des types d'authentification distincts.<br /><br /> Ne supprimez pas **RSWindowsNTLM**; sinon, vous limiterez la navigation à une partie des types de navigateurs pris en charge. Pour plus d’informations, consultez [Prise en charge des navigateurs pour Reporting Services et Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).|N|  
|**RSWindowsNegotiate**|Le serveur de rapports accepte les jetons de sécurité NTLM ou Kerberos. Il s'agit du paramètre par défaut lorsque le serveur de rapports s'exécute en mode natif et que le compte de service est Service réseau. Ce paramètre est omis lorsque le serveur de rapports s'exécute en mode natif et que le compte de service est configuré en tant que compte d'utilisateur de domaine.<br /><br /> Si un compte de domaine est configuré pour le compte de service Report Server et qu'aucun Nom de principal du service (SPN) n'est configuré pour le serveur de rapports, ce paramètre peut empêcher des utilisateurs de se connecter au serveur.|N|  
|**RSWindowsNTLM**|Le serveur accepte les jetons de sécurité NTLM.<br /><br /> Si vous supprimez ce paramètre, la prise en charge de navigation sera limitée pour certains des types de navigateurs pris en charge. Pour plus d’informations, consultez [Prise en charge des navigateurs pour Reporting Services et Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).|N, S|  
|**RSWindowsKerberos**|Le serveur accepte les jetons de sécurité Kerberos.<br /><br /> Utilisez ce paramètre ou RSWindowsNegotiate lorsque vous utilisez l'authentification Kerberos dans un modèle d'authentification de délégation contrainte.|N|  
|**RSWindowsBasic**|Le serveur accepte les informations d'identification de base et émet une stimulation/réponse lorsqu'une connexion est établie sans information d'identification.<br /><br /> L'authentification de base passe les informations d'identification dans les requêtes HTTP en texte clair. Si vous utilisez l'authentification de base, utilisez le protocole TLS pour chiffrer le trafic réseau vers et en provenance du serveur de rapports. Pour obtenir un exemple de syntaxe de configuration de l’authentification de base dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Authentification avec le serveur de rapports](../../reporting-services/security/authentication-with-the-report-server.md).|N|  
|**Personnalisée**|Spécifiez cette valeur si vous avez déployé une extension de sécurité personnalisée sur le serveur de rapports. Pour plus d’informations, consultez [Implémentation d’une extension de sécurité](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).|N|  
|**LogonMethod**|Cette valeur spécifie le type d'ouverture de session pour **RSWindowsBasic**. Si vous spécifiez **RSWindowsBasic**, cette valeur est requise. Les valeurs valides sont 2 ou 3, où chaque valeur représente ce qui suit :<br /><br /> **2** = serveurs hautes performances d’ouverture de session réseau pour l’authentification des mots de passe en clair<br /><br /> **3** = ouverture de session basée sur du texte en clair ; les informations d’identification d’ouverture de session sont conservées dans le package d’authentification envoyé avec chaque requête HTTP, ce qui permet au serveur d’emprunter l’identité de l’utilisateur quand il s’agit de se connecter aux autres serveurs du réseau.<br /><br /> <br /><br /> Remarque : Les valeurs 0 (pour l'ouverture de session interactive) et 1 (pour l'ouverture de session par lot) ne sont pas prises en charge dans [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)].|N|  
|**Realm**|Cette valeur est utilisée pour **RSWindowsBasic**. Elle spécifie une partition de ressource qui inclut les fonctionnalités d'autorisation et d'authentification permettant de contrôler l'accès aux ressources protégées de votre organisation.|N|  
|**DefaultDomain**|Cette valeur est utilisée pour **RSWindowsBasic**. Elle permet de déterminer le domaine utilisé par le serveur pour authentifier l'utilisateur. Cette valeur est facultative, mais si vous l'omettez, le serveur de rapports utilise le nom d'ordinateur comme domaine. Si vous avez installé le serveur de rapports sur un contrôleur de domaine, le domaine utilisé est celui contrôlé par l'ordinateur.|N|  
|**RSWindowsExtendedProtectionLevel**|La valeur par défaut est **off**. Pour plus d'informations, consultez [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)|N|  
|**RSWindowsExtendedProtectionScenario**|La valeur par défaut est **Proxy**.|N|  
|**EnableAuthPersistence**|Détermine si l'authentification est effectuée sur la connexion ou pour chaque requête.<br /><br /> Les valeurs possibles sont **True** (par défaut) ou **False**. Si la valeur est **True**, les requêtes suivantes qui proviennent de la même connexion prennent en compte par défaut le contexte d’emprunt d’identité de la première requête.<br /><br /> Cette valeur doit être **False** si vous utilisez un logiciel de serveur proxy (par exemple ISA Server) pour accéder au serveur de rapports. Un serveur proxy permet l'utilisation d'une connexion unique par plusieurs utilisateurs. Pour ce scénario, vous devez désactiver la persistance de l'authentification afin que chaque requête d'utilisateur puisse être authentifiée séparément. Si vous n’affectez pas la valeur **False** à **EnableAuthPersistence**, tous les utilisateurs se connecteront à l’aide du contexte d’emprunt d’identité de la première requête.|N,S|  
  
##  <a name="service-rsreportserverconfig-file"></a><a name="bkmk_service"></a> Service (fichier RSReportServer.config)  
 **Service** spécifie les paramètres de l’application qui s’appliquent à l’ensemble du service.  
  
 La dernière colonne du tableau suivant indique si le paramètre s’applique à un serveur de rapports en mode natif (N), à un serveur de rapports en mode SharePoint (S) ou à Power BI Report Server (P).  

  
|Paramètre|Description|Mode|  
|-------------|-----------------|----------|  
|**IsSchedulingService**|Spécifie si le serveur de rapports maintient un jeu de travaux de l'agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui correspondent aux planifications et aux abonnements créés par les utilisateurs [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Les valeurs valides sont **True** (valeur par défaut) et **False**.<br /><br /> Ce paramètre est affecté quand vous activez ou désactivez les fonctionnalités [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à l’aide de la facette Configuration de la surface d’exposition pour Reporting Services de la Gestion basée sur des stratégies. Pour plus d’informations, consultez [Démarrer et arrêter le service Report Server](../../reporting-services/report-server/start-and-stop-the-report-server-service.md).|N,S,P|  
|**IsNotificationService**|Spécifie si le serveur de rapports traite les notifications et les remises. Les valeurs valides sont **True** (valeur par défaut) et **False**. Quand la valeur est **False**, les abonnements ne sont pas remis.<br /><br /> Ce paramètre est affecté quand vous activez ou désactivez les fonctionnalités [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à l’aide de la facette Configuration de la surface d’exposition pour Reporting Services de la Gestion basée sur des stratégies. Pour plus d’informations, consultez [Démarrer et arrêter le service Report Server](../../reporting-services/report-server/start-and-stop-the-report-server-service.md).|N,S,P|  
|**IsEventService**|Spécifie si le service traite les événements de la file d'attente des événements. Les valeurs valides sont **True** (valeur par défaut) et **False**. Quand la valeur est **False**, le serveur de rapports n’effectue pas d’opérations pour les planifications ni les abonnements.<br /><br /> Ce paramètre est affecté quand vous activez ou désactivez les fonctionnalités [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à l’aide de la facette Configuration de la surface d’exposition pour Reporting Services de la Gestion basée sur des stratégies. Pour plus d’informations, consultez [Démarrer et arrêter le service Report Server](../../reporting-services/report-server/start-and-stop-the-report-server-service.md).|N,S,P|  
|**IsAlertingService**|La valeur par défaut est **True**.|S|  
|**PollingInterval**|Spécifie l'intervalle, en secondes, entre deux interrogations de la table d'événements par le serveur de rapports. La plage de valeurs valides s'étend de 0 à un entier maximal. La valeur par défaut est de 10.|N,S,P|  
|**IsDataModelRefreshService**|Spécifie si le service traite les événements d’actualisation planifiée du modèle de données pour les rapports Power BI. Les valeurs valides sont **True** (valeur par défaut) et **False**. Quand la valeur est **False**, le serveur de rapports n’effectue pas d’opérations pour l’actualisation planifiée du modèle de données.|N|  
|**WindowsServiceUseFileShareStorage**|Indique si les instantanés temporaires et les rapports mis en cache (créés par le service Report Server pour la durée d'une session utilisateur) doivent être stockés dans le système de fichiers. Les valeurs possibles sont **True** et **False** (valeur par défaut).|N,S,P|  
|**MemorySafetyMargin**|Spécifie un pourcentage de **WorkingSetMaximum** qui définit la limite entre les scénarios correspondant à une sollicitation moyenne et basse. La valeur par défaut est 80. Pour plus d’informations sur **WorkingSetMaximum** et la configuration de la mémoire disponible, consultez [Configurer la mémoire disponible pour les applications du serveur de rapports](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N,S,P|  
|**MemoryThreshold**|Spécifie un pourcentage de **WorkingSetMaximum** qui définit la limite entre les scénarios correspondant à une sollicitation élevée et moyenne. La valeur par défaut est **90**. Elle doit être supérieure à la valeur définie pour **MemorySafetyMargin**. Pour plus d’informations, consultez [Configurer la mémoire disponible pour les applications du serveur de rapports](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N,S,P|  
|**WorkingSetMaximum**|Spécifie un seuil de mémoire au-delà duquel aucune nouvelle requête d’allocation de mémoire n’est accordée aux applications du serveur de rapports. Par défaut, le serveur de rapports affecte à WorkingSetMaximum la quantité de mémoire disponible sur l’ordinateur. Cette valeur est détectée lorsque le service démarre. Ce paramètre n'apparaît pas dans le fichier RSReportServer.config à moins que vous ne l'ajoutiez manuellement. La plage de valeurs valides s'étend de 0 à un entier maximal. Cette valeur est exprimée en kilo-octets. Pour plus d’informations, consultez [Configurer la mémoire disponible pour les applications du serveur de rapports](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N|  
|**WorkingSetMinimum**|Spécifie une limite inférieure pour la consommation de mémoire. Le serveur de rapports ne libère pas de mémoire si la quantité totale de mémoire utilisée est inférieure à cette limite. Par défaut, la valeur est calculée au démarrage du service et la demande d’allocation de mémoire initiale représente 60 pour cent de **WorkingSetMaximum**. Ce paramètre n'apparaît pas dans le fichier RSReportServer.config à moins que vous ne l'ajoutiez manuellement. Si vous souhaitez personnaliser cette valeur, vous devez ajouter l’élément **WorkingSetMaximum** au fichier RSReportServer.config. La plage de valeurs valides s'étend de 0 à un entier maximal. Cette valeur est exprimée en kilo-octets. Pour plus d’informations, consultez [Configurer la mémoire disponible pour les applications du serveur de rapports](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N| 
|**RecycleTime**|Spécifie une durée de recyclage, en minutes, pour le domaine d'application. La plage de valeurs valides s'étend de 0 à un entier maximal. La valeur par défaut est 720.|N,S|  
|**MaxAppDomainUnloadTime**|Spécifie un intervalle pendant lequel le domaine d'application est autorisé à se décharger au cours d'une opération de recyclage. Si le recyclage n'est pas achevé à la fin du temps alloué, tout le traitement dans le domaine d'application est arrêté. Pour plus d'informations, consultez [Application Domains for Report Server Applications](../../reporting-services/report-server/application-domains-for-report-server-applications.md).<br /><br /> Cette valeur est exprimée en minutes. La plage de valeurs valides s'étend de 0 à un entier maximal. La valeur par défaut est **30**.|N,S,P|  
|**MaxQueueThreads**|Spécifie le nombre de threads utilisés par le service Windows Report Server pour le traitement simultané des abonnements et des notifications. La plage de valeurs valides s'étend de 0 à un entier maximal. La valeur par défaut est 0. Si vous choisissez 0, le serveur de rapports détermine le nombre maximal de threads. Si vous spécifiez un nombre entier, la valeur que vous spécifiez définit la limite maximale sur les threads pouvant être créés simultanément. Pour plus d'informations sur la manière dont le service Windows Report Server gère la mémoire pour les processus en cours d’exécution, consultez [Configurer la mémoire disponible pour les applications du serveur de rapports](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N,S,P|  
|**UrlRoot**|Employé par les extensions de remise de serveur de rapports pour composer les URL utilisées par les rapports remis dans les abonnements par e-mail et dans un partage de fichiers ; également employé par le traitement des rapports lors de la résolution d’expressions avec Globals!ReportServerUrl. La valeur doit être une adresse URL valide menant au serveur de rapports qui permet d'accéder au rapport publié. Utilisé par le serveur de rapports pour générer des URL pour l'accès hors connexion ou sans assistance. Ces URL sont utilisées dans les rapports exportés et par les extensions de remise pour composer une URL incluse dans les messages de remise tels que les liens dans les messages électroniques. Le serveur de rapports détermine les URL dans les rapports selon le comportement suivant :<br /><br /> Quand **UrlRoot** est vide (par défaut) et qu’il y a des réservations d’URL, le serveur de rapports détermine automatiquement les URL de la même façon que les URL sont générées pour la méthode ListReportServerUrls. La première URL retournée par la méthode ListReportServerUrls est utilisée. Ou, si SecureConnectionLevel est supérieur à zéro (0), la première URL TLS est utilisée.<br /><br /> Lorsque **UrlRoot** est défini sur une valeur spécifique, la valeur explicite est utilisée.<br /><br /> Quand **UrlRoot** est vide et qu’aucune réservation d’URL n’est configurée, les URL dans les rapports affichés et dans les liens de messagerie électronique sont incorrectes.|N,S,P|  
|**UnattendedExecutionAccount**|Spécifie un nom d’utilisateur, un mot de passe et un domaine utilisés par le serveur de rapports pour exécuter un rapport. Ces valeurs sont chiffrées. Utilisez l’outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou l’utilitaire **rsconfig** pour définir ces valeurs. Pour plus d’informations, consultez [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).<br /><br /> Pour le mode SharePoint, vous définissez le compte d'exécution d'une application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à l'aide de l'Administration centrale de SharePoint. Pour plus d’informations, consultez [Gérer une application de service SharePoint Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).|N,P|  
|**PolicyLevel**|Spécifie le fichier de configuration de la stratégie de sécurité. La valeur valide est Rssrvrpolicy.config. Pour plus d'informations, consultez [Using Reporting Services Security Policy Files](../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md).|N,S,P|  
|**IsWebServiceEnabled**|Spécifie si le service Web Report Server répond aux demandes d'accès URL et SOAP. Cette valeur est définie lorsque vous activez ou désactivez le service à l'aide de la facette Configuration de la surface d'exposition pour Reporting Services de la Gestion basée sur des stratégies.|N,S|  
|**IsReportManagerEnabled**|Ce paramètre est déprécié à compter de SQL Server 2016 Reporting Services Cumulative Update 2. Le portail web sera toujours activé.|N|  
|**FileShareStorageLocation**|Indique un dossier de stockage unique pour les instantanés temporaires dans le système de fichiers. Même s'il est possible de spécifier un chemin d'accès UNC pour ce dossier, il est déconseillé de faire ce choix. La valeur par défaut est vide.<br /><br /> `<FileShareStorageLocation>`<br /><br /> `<Path>`<br /><br /> `</Path>`<br /><br /> `</FileShareStorageLocation>`|N,S,P|  
|**IsRdceEnabled**|Spécifie si l'extension RDCE (Report Definition Customization Extension) est activée. Les valeurs valides sont **True** et **False**.|N,S,P|
|**IsDataModelRefreshService**|Spécifie si le serveur doit traiter l’actualisation des rapports Power BI. Les valeurs valides sont **True** et **False**.|P|
|**MaxCatalogConnectionPoolSizePerProcess**|Spécifie la taille maximale du pool de connexions lors de la connexion au catalogue du serveur. La valeur par défaut est 0. Si vous choisissez 0, le serveur de rapports détermine le nombre maximal de connexions pour le processus reportingservices.exe ; pour les autres processus, il s’agit de .[SqlClientDefault](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.maxpoolsize).|P|  

  
##  <a name="ui-rsreportserverconfig-file"></a><a name="bkmk_UI"></a> UI (fichier RSReportServer.config)  
 **UI** spécifie des paramètres de configuration qui s’appliquent à l’application du portail web.  
  
 La dernière colonne du tableau suivant indique si le paramètre s'applique à un serveur de rapports en mode natif (N), à un serveur de rapports en mode SharePoint (S) ou aux deux cas de figure.  
  
|Paramètre|Description|Mode|  
|-------------|-----------------|----------|  
|**ReportServerUrl**|Spécifie l’URL du serveur de rapports auquel se connecte le portail web. Ne modifiez cette valeur que si vous configurez le portail web pour qu’il se connecte à un serveur de rapports dans une autre instance ou sur un ordinateur distant.|N,S|  
|**ReportBuilderTrustLevel**|Ne modifiez pas cette valeur ; elle n'est pas configurable. Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et versions ultérieures, le Générateur de rapports s’exécute uniquement en **FullTrust**. Pour plus d’informations sur la suppression du mode de confiance partielle, consultez [Fonctionnalités supprimées de SQL Server Reporting Services dans SQL Server 2016](../../reporting-services/discontinued-functionality-to-sql-server-reporting-services-in-sql-server.md).|N,S|  
|**PageCountMode**|Pour le portail web uniquement, ce paramètre spécifie si le serveur de rapports calcule le nombre de pages avant le rendu du rapport ou pendant l’affichage de ce dernier. Les valeurs possibles sont **Estimate** (par défaut) et **Actual**. Utilisez **Estimate** pour calculer le nombre de pages du rapport pendant que l’utilisateur le consulte. Initialement, le nombre de pages est défini à 2 (pour la page actuelle plus une page supplémentaire), mais cette valeur s'ajuste au fur et à mesure que l'utilisateur navigue parmi les pages du rapport. Utilisez **Actual** si vous souhaitez calculer le nombre de pages à l’avance avant que le rapport ne s’affiche. **Actual** est fourni pour la compatibilité descendante. Notez que, si vous affectez **Actual** à **PageCountMode**, le rapport entier doit être traité pour permettre l’obtention d’un nombre de pages valide, ce qui accroît le temps d’attente avant que le rapport ne soit affiché.|N,S|  
  
##  <a name="extensions-rsreportserverconfig-file-native-mode"></a><a name="bkmk_extensions"></a> Extensions (fichier RSReportServer.config) en mode natif  
 La section Extensions apparaît dans le fichier rsreportserver.config **uniquement pour les serveurs de rapports en mode natif** . Les informations relatives aux extensions des serveurs de rapports en mode SharePoint sont stockées dans la base de données de configuration SharePoint et sont configurées en fonction de chaque application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 **Extensions** spécifie les paramètres de configuration des modules extensibles suivants pour une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   Extensions de remise  
  
-   Extensions DeliveryUI  
  
-   Extensions de rendu  
  
-   Extensions pour le traitement des données  
  
-   Extensions de requêtes sémantiques (interne uniquement)  
  
-   Extensions de génération de modèle (interne uniquement)  
  
-   Extensions de sécurité  
  
-   Extensions d'authentification  
  
-   Extensions pour le traitement des événements (interne uniquement)  
  
-   Extensions de la personnalisation de définition de rapport  
  
 Quelques-unes de ces extensions sont à usage strictement interne par le serveur de rapports. Les paramètres de configuration pour les extensions à usage interne uniquement ne sont pas documentés. Les sections suivantes décrivent les paramètres de configuration des extensions par défaut. Si vous utilisez un serveur de rapports qui a des extensions personnalisées, vos fichiers de configuration peuvent contenir des paramètres qui ne sont pas décrits ici. Cette section liste les extensions dans l'ordre dans lequel elles apparaissent. Les paramètres qui se répètent pour plusieurs instances du même type d'extension ne sont décrits qu'une seule fois.  
  
###  <a name="delivery-extensions-general-configuration"></a><a name="bkmk_extensionsgeneral"></a> Configuration générale des extensions de remise  
 Spécifie les extensions de remise par défaut (et éventuellement personnalisée) qui sont utilisées pour remettre les rapports par le biais des abonnements. Le fichier RSReportServer.config inclut des paramètres d'application pour quatre extensions de remise :  
  
1.  messagerie électronique du serveur de rapports ;  
  
2.  remise par partage de fichiers ;  
  
3.  bibliothèque de documents du serveur de rapports utilisée pour un serveur de rapports s'exécutant en mode intégré SharePoint ;  
  
4.  fournisseur de remise Null utilisé pour le préchargement du cache de rapports.  
  
 Pour plus d’informations sur les extensions de remise, consultez [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md).  
  
 Toutes les extensions de remise ont **Extension Name**, **MaxRetries**, **SecondsBeforeRetry** et **Configuration**. Ces paramètres partagés sont documentés en premier. Les descriptions des paramètres spécifiques aux extensions figurent dans un deuxième tableau.  
  
|Paramètre|Description|  
|-------------|-----------------|  
|**Extension Name**|Spécifie le nom convivial et l'assembly de l'extension de remise. Ne modifiez pas cette valeur.|  
|**MaxRetries**|Indique le nombre de tentatives de remise que le serveur de rapports effectuera en cas d'échec de la première tentative. La valeur par défaut est 3.|  
|**SecondsBeforeRetry**|Précise la durée (en secondes) qui s'écoule entre deux tentatives. La valeur par défaut est 900.|  
|**Configuration**|Contient les paramètres de configuration spécifiques à chaque extension de remise.|  
  
####  <a name="file-share-delivery-extension-configuration-settings"></a><a name="bkmk_fileshare_extension"></a> Paramètres de configuration spécifiques à l'extension de remise par partage de fichiers  
 La remise par partage de fichiers envoie un rapport exporté dans un format de fichier d'application vers un dossier partagé sur le réseau. Pour plus d'informations, consultez [File Share Delivery in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md).  
  
|Paramètre|Description|  
|-------------|-----------------|  
|**ExcludedRenderFormats**, **RenderingExtension**|Ces paramètres sont utilisés pour exclure volontairement les formats d'exportation qui ne fonctionnent pas correctement avec la remise par partage de fichiers. Ces formats sont utilisés en général pour la création de rapports interactifs, l'aperçu ou le préchargement du cache de rapports. Ils ne produisent pas de fichiers d'application qui peuvent être affichés facilement à partir d'une application bureautique.<br /><br /> HTMLOWC<br /><br /> RGDI<br /><br /> Null|  
  
####  <a name="report-server-e-mail-extension-configuration-settings"></a><a name="bkmk_email_extension"></a> Paramètres de configuration de l'extension de messagerie électronique du serveur de rapports  
 La messagerie électronique Report Server utilise un périphérique réseau SMTP pour envoyer des rapports à des adresses de messagerie. Cette extension de remise doit être configurée avant de pouvoir être utilisée. Pour plus d’informations, consultez [Remise par courrier électronique dans Reporting Services](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md).  
  
|Paramètre|Description|  
|-------------|-----------------|  
|**SMTPServer**|Spécifie une valeur de chaîne indiquant l'adresse d'un redirecteur ou d'un serveur SMTP distant. Cette valeur est obligatoire pour le service SMTP distant. Il peut s'agir d'une adresse IP, du nom UNC d'un ordinateur sur l'intranet de votre entreprise ou d'un nom de domaine complet.|  
|**SMTPServerPort**|Spécifie une valeur d'entier indiquant le port utilisé par le service SMTP pour envoyer le courrier sortant. Le port 25 est généralement employé pour envoyer le courrier électronique.|  
|**SMTPAccountName**|Contient une valeur de chaîne qui désigne le nom du compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Outlook Express. Vous pouvez définir cette valeur si votre serveur SMTP est configuré de manière à l'utiliser d'une manière ou d'une autre. Si tel n'est pas le cas, vous pouvez la laisser vide. Utilisez **From** pour spécifier le compte de messagerie utilisé pour envoyer les rapports.|  
|**SMTPConnectionTimeout**|Spécifie une valeur d'entier indiquant le nombre de secondes d'attente d'une connexion de sockets valide avec le service SMTP avant l'expiration du délai. La valeur par défaut est 30 secondes, mais elle est ignorée si la valeur de **SendUsing** est 2.|  
|**SMTPServerPickupDirectory**|Spécifie une valeur de chaîne indiquant le répertoire de collecte pour le service SMTP local. Cette valeur doit être un chemin d'accès complet à un dossier local (par exemple, d:\rs-emails).|  
|**SMTPUseSSL**|Spécifie une valeur booléenne pouvant être définie pour utiliser le protocole TLS (Transport Layer Security), anciennement SSL (Secure Sockets Layer), en cas d'envoi de message SMTP via le réseau. La valeur par défaut est 0 (ou False). Ce paramètre peut être utilisé lorsque l'élément **SendUsing** est défini sur 2.|  
|**SendUsing**|Spécifie quelle méthode utiliser pour envoyer des messages. Les valeurs autorisées sont :<br /><br /> 1= Envoie un message à partir du répertoire de collecte du service SMTP local.<br /><br /> 2= Envoie le message à partir du service SMTP du réseau.|  
|**SMTPAuthenticate**|Spécifie une valeur d'entier qui indique quel type d'authentification utiliser lorsque vous envoyez des messages à un service SMTP via une connexion TCP/IP. Les valeurs autorisées sont :<br /><br /> 0= Pas d'authentification.<br /><br /> 1= (Non pris en charge.)<br /><br /> 2= Authentification NTLM (NT LanMan). Le contexte de sécurité du service Windows Report Server est utilisé pour la connexion au serveur SMTP du réseau.|  
|**From**|Spécifie l’adresse de messagerie électronique d’où sont envoyés les rapports au format *abc@host.xyz* . L’adresse apparaît dans la ligne **De** d’un message électronique sortant. Cette valeur est obligatoire si vous utilisez un serveur SMTP distant. Elle doit correspondre à un compte de messagerie valide ayant l'autorisation d'envoyer des messages.|  
|**EmbeddedRenderFormats, RenderingExtension**|Spécifie le format de rendu utilisé pour encapsuler un rapport dans le corps d'un message électronique. Les images du rapport sont ensuite incorporées dans le rapport. Les valeurs valides sont MHTML et HTML4.0.|  
|**PrivilegedUserRenderFormats**|Spécifie les formats de rendu que l'utilisateur peut sélectionner pour un abonnement de rapport lorsque l'option d'abonnement est activée au moyen de la tâche « Gérer tous les abonnements ». Si cette valeur n'est pas définie, tous les formats de rendu qui ne sont pas spécifiquement exclus peuvent être sélectionnés.|  
|**ExcludedRenderFormats, RenderingExtension**|Exclut spécifiquement les formats qui ne fonctionnent pas bien avec une extension de remise donnée. Vous ne pouvez pas exclure plusieurs instances de la même extension de rendu. L'exclusion de plusieurs instances entraîne une erreur lorsque le serveur de rapports lit le fichier de configuration. Par défaut, les extensions suivantes sont exclues de la remise par messagerie électronique :<br /><br /> HTMLOWC<br /><br /> Null<br /><br /> RGDI|  
|**SendEmailToUserAlias**|Cette valeur fonctionne avec **DefaultHostName**.<br /><br /> Quand l’option **SendEmailToUserAlias** a la valeur **True**, les utilisateurs qui définissent des abonnements individuels sont automatiquement spécifiés comme destinataires du rapport. Le champ **À** est masqué. Si cette valeur est **False**, le champ **À** est visible. Choisissez la valeur **True** si vous voulez un contrôle maximal de la distribution des rapports. Les valeurs valides sont les suivantes :<br /><br /> **True**= l’adresse de messagerie de l’utilisateur qui crée l’abonnement est utilisée. Il s’agit de la valeur par défaut.<br /><br /> **False**= n’importe quelle adresse de messagerie peut être spécifiée.|  
|**DefaultHostName**|Cette valeur fonctionne avec **SendEmailToUserAlias**.<br /><br /> Spécifie une valeur de chaîne indiquant le nom d'hôte à ajouter à l'alias d'utilisateur lorsque l'option **SendEmailToUserAlias** est définie sur true (vrai). Cette valeur peut être un nom DNS (Domain Name System) ou une adresse IP.|  
|**PermittedHosts**|Limite la distribution des rapports en spécifiant quels hôtes peuvent recevoir la livraison de courriers électroniques. Dans l'option **PermittedHosts**, chaque hôte est spécifié en tant qu'élément **HostName** , sachant que la valeur est soit une adresse IP, soit un nom DNS.<br /><br /> Seuls les comptes de messagerie définis pour l'hôte sont des destinataires valides. Si vous avez spécifié l'option **DefaultHostName**, assurez-vous d'inclure cet hôte en tant qu'élément **HostName** de l'option **PermittedHosts**. Cette valeur doit correspondre à un ou plusieurs noms DNS ou adresses IP. Par défaut, cette valeur n'est pas définie. Si la valeur n'est pas définie, il n'y a aucune restriction en matière de réception des rapports envoyés par messagerie.|  
  
####  <a name="report-server-sharepoint-document-library-extension-configuration"></a><a name="bkmk_documentlibrary_extension"></a> Configuration de l'extension de bibliothèque de documents SharePoint du serveur de rapports  
 L'extension de remise par bibliothèque de documents Report Server envoie un rapport exporté dans un format de fichier d'application vers une bibliothèque de documents. Cette extension de remise ne peut être utilisée que par un serveur de rapports configuré pour s'exécuter en mode intégré SharePoint. Pour plus d'informations, consultez [SharePoint Library Delivery in Reporting Services](../../reporting-services/subscriptions/sharepoint-library-delivery-in-reporting-services.md).  
  
|Paramètre|Description|  
|-------------|-----------------|  
|**ExcludedRenderFormats, RenderingExtension**|Ces paramètres sont utilisés pour exclure volontairement les formats d'exportation qui ne fonctionnent pas correctement avec la remise à une bibliothèque de documents. Les extensions de remise HTMLOWC, RGDI et Null sont exclues. Ces formats sont utilisés en général pour la création de rapports interactifs, l'aperçu ou le préchargement du cache de rapports. Ils ne produisent pas de fichiers d'application qui peuvent être affichés facilement à partir d'une application bureautique.|  
  
####  <a name="null-delivery-extension-configuration"></a><a name="bkmk_null_extension"></a> Configuration de l'extension de remise Null  
 Le fournisseur de remise NULL sert à précharger le cache avec les rapports générés au préalable pour des utilisateurs spécifiques. Il n'y a pas de paramètres de configuration pour cette extension de remise. Pour plus d’informations, consultez [Mise en cache de rapports &#40;SSRS&#41;](../../reporting-services/report-server/caching-reports-ssrs.md).  
  
###  <a name="delivery-ui-extensions-general-configuration"></a><a name="bkmk_ui"></a> Configuration générale des extensions de l'interface utilisateur de remise  
 Spécifie les extensions de remise qui contiennent un composant d’interface utilisateur apparaissant dans les pages de définition d’abonnement ; ces dernières permettent de définir des abonnements individuels dans le portail web. Si vous créez et déployez une extension de remise personnalisée qui dispose d’options définies par l’utilisateur et que vous souhaitez utiliser le portail web, vous devez inscrire l’extension de remise dans cette section. Par défaut, il existe des paramètres de configuration pour la messagerie électronique Report Server et le partage de fichiers Report Server. Les extensions de remise utilisées uniquement dans les abonnements pilotés par les données ou les pages d'application SharePoint n'ont pas de paramètres dans cette section.  
  
|Paramètre|Description|  
|-------------|-----------------|  
|**DefaultDeliveryExtension**|Ce paramètre détermine l'extension de remise qui apparaît en premier dans la liste des types de remises de la page de définition d'abonnement. Une seule extension de remise peut contenir ce paramètre. Les valeurs valides sont **True** ou **False**. Quand la valeur est **True**, cette extension est sélectionnée par défaut.|  
|**Configuration**|Spécifie les options de configuration d'une extension de remise. Vous pouvez définir un format de rendu par défaut pour chaque extension de remise. Les valeurs valides sont les noms d'extensions de rendu de la section de rendu du fichier rsreportserver.config.|  
|**DefaultRenderingExtension**|Spécifie si une extension de remise est celle par défaut. La messagerie électronique Report Server est l'extension de remise par défaut. Les valeurs valides sont **True** ou **False**. Si plusieurs extensions contiennent la valeur **True**, la première extension est considérée comme l’extension par défaut.|  
  
###  <a name="rendering-extensions-general-configuration"></a><a name="bkmk_rendering"></a> Configuration générale des extensions de rendu  
 Spécifie les extensions de rendu par défaut (et éventuellement personnalisé) utilisées pour la présentation des rapports.  
  
 Ne modifiez pas cette section sauf si vous déployez une extension de rendu personnalisée. Pour plus d’informations, consultez [Implémentation d’une extension de rendu](../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md).  
  
 Les extensions de rendu par défaut sont les suivantes :  
  
-   XML  
  
-   Null  
  
-   CSV  
  
-   PDF  
  
-   RGDI  
  
-   HTML4.0  
  
-   MHTML  
  
-   EXCEL  
  
-   RPL  
  
-   IMAGE  
  
 À partir de la version [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , les rendus MHTML et HTML 4.0 contiennent par défaut le paramètre d'informations de périphérique suivant pour contrôler le comportement du dimensionnement des visualisations de données.  
  
```  
<DeviceInfo><DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing></DeviceInfo>  
```  
  
 Pour plus d'informations sur les paramètres DeviceInfo, consultez :  
  
-   [Paramètres d'informations de périphérique pour le format de rendu MHTML](../../reporting-services/mhtml-device-information-settings.md)  
  
-   [Paramètres d'informations de périphérique HTML](../../reporting-services/html-device-information-settings.md)  
  
-   [Paramètres d’informations de périphérique pour les extensions de rendu &#40;Reporting Services&#41;](../../reporting-services/device-information-settings-for-rendering-extensions-reporting-services.md)  
  
 Pour plus d’informations sur les attributs de l’élément **\<Extension>** enfant sous **\<Render>** , consultez :  
  
-   [Personnaliser les paramètres d'extension de rendu dans RSReportServer.Config](../../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)  
  
-   [Déploiement d'une extension de rendu](../../reporting-services/extensions/rendering-extension/deploying-a-rendering-extension.md)  
  
 Ne modifiez pas cette section sauf si vous déployez une extension de rendu personnalisée. Pour plus d’informations, consultez [Implémentation d’une extension de rendu](../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md).  
  
###  <a name="data-extensions-general-configuration"></a><a name="bkmk_data"></a> Configuration générale des extensions de données  
 Spécifie les extensions pour le traitement des données par défaut (et éventuellement personnalisé) utilisées pour traiter les requêtes. Les extensions de traitement de données par défaut sont les suivantes :  
  
-   SQL  
  
-   SQLAZURE  
  
-   SQLPDW  
  
-   OLEDB  
  
-   OLEDB-MD  
  
-   ORACLE  
  
-   ODBC  
  
-   XML  
  
-   SHAREPOINTLIST  
  
-   SAPBW  
  
-   ESSBASE  
  
-   TERADATA  
  
 Ne modifiez pas cette section sauf si vous ajoutez des extensions pour le traitement des données personnalisées. Pour plus d’informations, consultez [Implémentation d’une extension pour le traitement des données](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md).  
  
###  <a name="semantic-query-extensions-general-configuration"></a><a name="bkmk_semantic"></a> Configuration générale des extensions de requêtes sémantiques  
 Indique l'extension pour le traitement des requêtes sémantiques utilisée pour traiter les modèles de rapport. Les extensions pour le traitement des requêtes sémantiques incluses dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] permettent la prise en charge des données relationnelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , des données Oracle et des données multidimensionnelles [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Ne modifiez pas cette section. Le traitement des requêtes n'est pas extensible.  
  
###  <a name="model-generation-configuration"></a><a name="bkmk_model"></a> Configuration de la génération de modèle  
 Spécifie une extension de génération de modèle servant à créer des modèles de rapports à partir d'une source de données partagée déjà publiée sur un serveur de rapports. Vous pouvez générer des modèles pour des données relationnelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , des données Oracle et des sources de données multidimensionnelles [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Ne modifiez pas cette section. La génération de modèle n'est pas extensible.  
  
###  <a name="security-extension-configuration"></a><a name="bkmk_security"></a> Configuration de l'extension de sécurité  
 Spécifie le composant d'autorisation utilisé par [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Ce composant est utilisé par l’extension d’authentification inscrite dans l’élément **Authentication** du fichier RSReportServer.config. Ne modifiez pas cette section sauf si vous implémentez une extension d'authentification personnalisée. Pour plus d'informations sur l'ajout de fonctionnalités de sécurité personnalisées, consultez [Implémentation d’une extension de sécurité](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md). Pour plus d'informations sur les autorisations, consultez [Autorisation dans Reporting Services](../../reporting-services/extensions/security-extension/authorization-in-reporting-services.md).  
  
###  <a name="authentication-extension-configuration"></a><a name="bkmk_authentication"></a> Configuration de l'extension d'authentification  
 Spécifie les extensions d'authentification personnalisée et par défaut qui sont utilisées par le serveur de rapports. L'extension par défaut repose sur l'authentification Windows. Ne modifiez pas cette section sauf si vous implémentez une extension d'authentification personnalisée. Pour plus d’informations sur l’authentification dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Authentification dans Reporting Services](../../reporting-services/extensions/security-extension/authentication-in-reporting-services.md) et [Authentification avec le serveur de rapports](../../reporting-services/security/authentication-with-the-report-server.md). Pour plus d'informations sur l'ajout de fonctionnalités de sécurité personnalisées, consultez [Implémentation d’une extension de sécurité](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).  
  
###  <a name="event-processing"></a><a name="bkmk_eventprocessing"></a> Traitement des événements  
 Spécifie les gestionnaires d'événements par défaut. Ne modifiez pas cette section. Cette section n'est pas extensible.  
  
###  <a name="report-definition-customization"></a><a name="bkmk_reportdefinition"></a> Personnalisation de la définition de rapport  
 Spécifie le nom et le type d'une extension personnalisée qui modifie une définition de rapport.  
  
###  <a name="rdlsandboxing"></a><a name="bkmk_rdlsandboxing"></a> RDLSandboxing  
 Spécifie un mode RDL (Report Definition Language) qui vous permet de détecter et restreindre l'utilisation de types spécifiques de ressources de rapport par les locataires individuels dans un scénario où plusieurs locataires partagent une batterie Web unique de serveurs de rapports. Pour plus d’informations, consultez [Enable and Disable RDL Sandboxing](../../reporting-services/report-server-sharepoint/enable-and-disable-rdl-sandboxing.md).  
  
##  <a name="maptileserverconfiguration-rsreportserverconfig-file"></a><a name="bkmk_MapTileServer"></a> MapTileServerConfiguration (fichier RSReportServer.config)  
 **MapTileServerConfiguration** définit des paramètres de configuration pour les Services Web Bing Maps de [!INCLUDE[msCoName](../../includes/msconame-md.md)] qui fournissent un arrière-plan de mosaïques pour un élément de rapport cartographique dans un rapport publié sur un serveur de rapports. Tous les éléments enfants sont obligatoires.  
  
|Paramètre|Description|  
|-------------|-----------------|  
|**MaxConnections**|Spécifie le nombre maximal de connexions aux services Web Bing Maps.|  
|**Délai d'expiration**|Spécifie le délai d'attente, en secondes, d'une réponse des services Web Bing Maps.|  
|**AppID**|Spécifie l'identificateur d'application (AppID) à utiliser pour les services Web Bing Maps. **(Default)** spécifie l’AppID par défaut de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .<br /><br /> Pour plus d'informations sur l'utilisation de mosaïques Bing dans votre rapport, consultez [Conditions supplémentaires d'utilisation](https://go.microsoft.com/fwlink/?LinkId=151371).<br /><br /> Ne modifiez pas cette valeur sauf si vous devez spécifier un AppID personnalisé pour votre propre contrat de licence Bing Maps. Lorsque vous modifiez l'AppID, vous n'avez pas à redémarrer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour que la modification prenne effet.|  
|**CacheLevel**|Spécifie une valeur de l'énumération HttpRequestCacheLevel de System.Net.Cache. La valeur par défaut est **Default**. Pour plus d'informations, consultez [HttpRequestCacheLevel, énumération](/dotnet/api/system.net.cache.httprequestcachelevel).|  
  
##  <a name="default-configuration-file-for-a-native-mode-report-server"></a><a name="bkmk_nativedefaultfile"></a> Fichier de configuration par défaut d'un serveur de rapports en mode natif  
 Le fichier rsreportserver.config est installé à l'emplacement par défaut suivant :  
  
 **C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer**  
  
```  
<Configuration>
    <Dsn>AQAAANCMnd8BFdERjHoAwE/Cl+sBAAAAR58DMGebHUeMvyR6HR04kQQAAAAiAAAAUgBlAHAAbwBy
AHQAaQBuAGcAIABTAGUAcgB2AGUAcgAAAANmAADAAAAAEAAAADczfLRgZ4GF44iBHkLrKY4AAAAA
BIAAAKAAAAAQAAAAJ9wQOmDNauH+LS30rboJ2OAAAAAp0kiFFBrc3r3ypKaldZJtjCORX9LTZRzt
0/JCSVIZc4GXx0peGKqd+f85UyrY/KOyUSHogOC/XoBp9Ppxv6ITbdunsS/LXEcMUBVqEdQD4ylh
x6K1NTC/u8hl9v0MgK+xMQKaiV7BuNYbgGgkaViABcNH0xVzcc5rMTHUkrABbGDFGKyAFniGQ1qu
/rqHibNNyvYbP/2uiqvgC0tQl6u8VkVbXpWrkvO+bFCqxlaJlCoDc2f3rIO321SZEvoFbsYNgPLd
+mIAkSCnH3Z3gm/bI8bqVkFaHblKyQuSfFsi6RQAAACb87b26dV0GjHmMJnE0Tk8CzNmhg==</Dsn>
    <ConnectionType>Default</ConnectionType>
    <LogonUser></LogonUser>
    <LogonDomain></LogonDomain>
    <LogonCred></LogonCred>
    <InstanceId>MSRS13.MSSQLSERVER</InstanceId>
    <InstallationID>{cd920604-a5c7-4554-b2a0-aadc04312fe5}</InstallationID>
    <Add Key="SecureConnectionLevel" Value="0"/>
    <Add Key="DisableSecureFormsAuthenticationCookie" Value="false"/>
    <Add Key="CleanupCycleMinutes" Value="10"/>
    <Add Key="MaxActiveReqForOneUser" Value="20"/>
    <Add Key="DatabaseQueryTimeout" Value="120"/>
    <Add Key="RunningRequestsScavengerCycle" Value="60"/>
    <Add Key="RunningRequestsDbCycle" Value="60"/>
    <Add Key="RunningRequestsAge" Value="30"/>
    <Add Key="MaxScheduleWait" Value="5"/>
    <Add Key="DisplayErrorLink" Value="true"/>
    <Add Key="WebServiceUseFileShareStorage" Value="false"/>
    <!--  <Add Key="ProcessTimeout" Value="150" /> -->
    <!--  <Add Key="ProcessTimeoutGcExtension" Value="30" /> -->
    <!--  <Add Key="WatsonFlags" Value="0x0430" /> full dump-->
    <!--  <Add Key="WatsonFlags" Value="0x0428" /> minidump -->
    <!--  <Add Key="WatsonFlags" Value="0x0002" /> no dump-->
    <Add Key="WatsonFlags" Value="0x0428"/>
    <Add Key="WatsonDumpOnExceptions" Value="Microsoft.ReportingServices.Diagnostics.Utilities.InternalCatalogException,Microsoft.ReportingServices.Modeling.InternalModelingException,Microsoft.ReportingServices.ReportProcessing.UnhandledReportRenderingException"/>
    <Add Key="WatsonDumpExcludeIfContainsExceptions" Value="System.Threading.ThreadAbortException,System.Web.UI.ViewStateException,System.OutOfMemoryException,System.Web.HttpException,System.IO.IOException,System.IO.FileLoadException,Microsoft.SharePoint.SPException,Microsoft.ReportingServices.WmiProvider.WMIProviderException,System.AppDomainUnloadedException"/>
    <URLReservations>
        <Application>
            <Name>ReportServerWebService</Name>
            <VirtualDirectory>ReportServer</VirtualDirectory>
            <URLs>
                <URL>
                    <UrlString>https://+:80</UrlString>
                    <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>
                    <AccountName>NT SERVICE\ReportServer</AccountName>
                </URL>
            </URLs>
        </Application>
        <Application>
            <Name>ReportServerWebApp</Name>
            <VirtualDirectory>Reports</VirtualDirectory>
            <URLs>
                <URL>
                    <UrlString>https://+:80</UrlString>
                    <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>
                    <AccountName>NT SERVICE\ReportServer</AccountName>
                </URL>
            </URLs>
        </Application>
    </URLReservations>
    <Authentication>
        <AuthenticationTypes>
            <RSWindowsNTLM/>
        </AuthenticationTypes>
        <RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>
        <RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionScenario>
        <EnableAuthPersistence>true</EnableAuthPersistence>
    </Authentication>
    <Service>
        <IsSchedulingService>True</IsSchedulingService>
        <IsNotificationService>True</IsNotificationService>
        <IsEventService>True</IsEventService>
        <PollingInterval>10</PollingInterval>
        <WindowsServiceUseFileShareStorage>False</WindowsServiceUseFileShareStorage>
        <MemorySafetyMargin>80</MemorySafetyMargin>
        <MemoryThreshold>90</MemoryThreshold>
        <RecycleTime>720</RecycleTime>
        <MaxAppDomainUnloadTime>30</MaxAppDomainUnloadTime>
        <MaxQueueThreads>0</MaxQueueThreads>
        <UrlRoot>
        </UrlRoot>
        <UnattendedExecutionAccount>
            <UserName></UserName>
            <Password></Password>
            <Domain></Domain>
        </UnattendedExecutionAccount>
        <PolicyLevel>rssrvpolicy.config</PolicyLevel>
        <IsWebServiceEnabled>True</IsWebServiceEnabled>
        <IsReportManagerEnabled>True</IsReportManagerEnabled>
        <FileShareStorageLocation>
            <Path>
            </Path>
        </FileShareStorageLocation>
        <DefaultFileShareAccount>
            <Domain></Domain>
            <UserName></UserName>
            <Password></Password>
        </DefaultFileShareAccount>
    </Service>
    <UI>
        <ReportServerUrl>
        </ReportServerUrl>
        <PageCountMode>Estimate</PageCountMode>
    </UI>
    <Extensions>
        <Delivery>
            <Extension Name="Report Server FileShare" Type="Microsoft.ReportingServices.FileShareDeliveryProvider.FileShareProvider,ReportingServicesFileShareDeliveryProvider">
                <MaxRetries>3</MaxRetries>
                <SecondsBeforeRetry>900</SecondsBeforeRetry>
                <Configuration>
                    <FileShareConfiguration>
                        <ExcludedRenderFormats>
                            <RenderingExtension>HTMLOWC</RenderingExtension>
                            <RenderingExtension>NULL</RenderingExtension>
                            <RenderingExtension>RGDI</RenderingExtension>
                        </ExcludedRenderFormats>
                    </FileShareConfiguration>
                </Configuration>
            </Extension>
            <Extension Name="Report Server Email" Type="Microsoft.ReportingServices.EmailDeliveryProvider.EmailProvider,ReportingServicesEmailDeliveryProvider">
                <MaxRetries>3</MaxRetries>
                <SecondsBeforeRetry>900</SecondsBeforeRetry>
                <Configuration>
                    <RSEmailDPConfiguration>
                        <SMTPServer></SMTPServer>
                        <SMTPServerPort>
                        </SMTPServerPort>
                        <SMTPAccountName>
                        </SMTPAccountName>
                        <SMTPConnectionTimeout>
                        </SMTPConnectionTimeout>
                        <SMTPServerPickupDirectory>
                        </SMTPServerPickupDirectory>
                        <SMTPUseSSL>False</SMTPUseSSL>
                        <SendUsing>2</SendUsing>
                        <SMTPAuthenticate>0</SMTPAuthenticate>
                        <SendUserName></SendUserName>
                        <SendPassword></SendPassword>
                        <From></From>
                        <EmbeddedRenderFormats>
                            <RenderingExtension>MHTML</RenderingExtension>
                        </EmbeddedRenderFormats>
                        <PrivilegedUserRenderFormats>
                        </PrivilegedUserRenderFormats>
                        <ExcludedRenderFormats>
                            <RenderingExtension>HTMLOWC</RenderingExtension>
                            <RenderingExtension>NULL</RenderingExtension>
                            <RenderingExtension>RGDI</RenderingExtension>
                        </ExcludedRenderFormats>
                        <SendEmailToUserAlias>True</SendEmailToUserAlias>
                        <DefaultHostName>
                        </DefaultHostName>
                        <PermittedHosts>
                        </PermittedHosts>
                    </RSEmailDPConfiguration>
                </Configuration>
            </Extension>
            <Extension Name="Report Server DocumentLibrary" Type="Microsoft.ReportingServices.SharePoint.SharePointDeliveryExtension.DocumentLibraryProvider,ReportingServicesSharePointDeliveryExtension">
                <MaxRetries>3</MaxRetries>
                <SecondsBeforeRetry>900</SecondsBeforeRetry>
                <Configuration>
                    <DocumentLibraryConfiguration>
                        <ExcludedRenderFormats>
                            <RenderingExtension>HTMLOWC</RenderingExtension>
                            <RenderingExtension>NULL</RenderingExtension>
                            <RenderingExtension>RGDI</RenderingExtension>
                        </ExcludedRenderFormats>
                    </DocumentLibraryConfiguration>
                </Configuration>
            </Extension>
            <Extension Name="NULL" Type="Microsoft.ReportingServices.NullDeliveryProvider.NullProvider,ReportingServicesNullDeliveryProvider"/>
            <Extension Name="Report Server PowerBI" Type="Microsoft.ReportingServices.PowerBIDeliveryProvider.PowerBIDeliveryProvider,ReportingServicesPowerBIDeliveryProvider">
                <MaxRetries>3</MaxRetries>
                <SecondsBeforeRetry>900</SecondsBeforeRetry>
                <Configuration>
                    <PowerBIDeliveryConfiguration>
                    </PowerBIDeliveryConfiguration>
                </Configuration>
            </Extension>
        </Delivery>
        <DeliveryUI>
            <Extension Name="Report Server Email" Type="Microsoft.ReportingServices.EmailDeliveryProvider.EmailDeliveryProviderControl,ReportingServicesEmailDeliveryProvider">
                <DefaultDeliveryExtension>True</DefaultDeliveryExtension>
                <Configuration>
                    <RSEmailDPConfiguration>
                        <DefaultRenderingExtension>MHTML</DefaultRenderingExtension>
                    </RSEmailDPConfiguration>
                </Configuration>
            </Extension>
            <Extension Name="Report Server FileShare" Type="Microsoft.ReportingServices.FileShareDeliveryProvider.FileShareUIControl,ReportingServicesFileShareDeliveryProvider"/>
            <Extension Name="Report Server PowerBI" Type="Microsoft.ReportingServices.PowerBIDeliveryProvider.PowerBIDeliveryUIControl,ReportingServicesPowerBIDeliveryProvider"/>
        </DeliveryUI>
        <Render>
            <Extension Name="WORDOPENXML" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordOpenXmlRenderer.WordOpenXmlDocumentRenderer,Microsoft.ReportingServices.WordRendering"/>
            <Extension Name="WORD" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordDocumentRenderer,Microsoft.ReportingServices.WordRendering" Visible="false"/>
            <Extension Name="EXCELOPENXML" Type="Microsoft.ReportingServices.Rendering.ExcelOpenXmlRenderer.ExcelOpenXmlRenderer,Microsoft.ReportingServices.ExcelRendering"/>
            <Extension Name="EXCEL" Type="Microsoft.ReportingServices.Rendering.ExcelRenderer.ExcelRenderer,Microsoft.ReportingServices.ExcelRendering" Visible="false"/>
            <Extension Name="PPTX" Type="Microsoft.ReportingServices.Rendering.PowerPointRendering.PptxRenderingExtension,Microsoft.ReportingServices.PowerPointRendering"/>
            <Extension Name="PDF" Type="Microsoft.ReportingServices.Rendering.ImageRenderer.PDFRenderer,Microsoft.ReportingServices.ImageRendering"/>
            <Extension Name="IMAGE" Type="Microsoft.ReportingServices.Rendering.ImageRenderer.ImageRenderer,Microsoft.ReportingServices.ImageRendering"/>
            <Extension Name="MHTML" Type="Microsoft.ReportingServices.Rendering.HtmlRenderer.MHtmlRenderingExtension,Microsoft.ReportingServices.HtmlRendering">
                <Configuration>
                    <DeviceInfo>
                        <DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing>
                    </DeviceInfo>
                </Configuration>
            </Extension>
            <Extension Name="CSV" Type="Microsoft.ReportingServices.Rendering.DataRenderer.CsvReport,Microsoft.ReportingServices.DataRendering"/>
            <Extension Name="XML" Type="Microsoft.ReportingServices.Rendering.DataRenderer.XmlDataReport,Microsoft.ReportingServices.DataRendering"/>
            <Extension Name="ATOM" Type="Microsoft.ReportingServices.Rendering.DataRenderer.AtomDataReport,Microsoft.ReportingServices.DataRendering"/>
            <Extension Name="NULL" Type="Microsoft.ReportingServices.Rendering.NullRenderer.NullReport,Microsoft.ReportingServices.NullRendering" Visible="false"/>
            <Extension Name="RGDI" Type="Microsoft.ReportingServices.Rendering.ImageRenderer.RGDIRenderer,Microsoft.ReportingServices.ImageRendering" Visible="false"/>
            <Extension Name="HTML4.0" Type="Microsoft.ReportingServices.Rendering.HtmlRenderer.Html40RenderingExtension,Microsoft.ReportingServices.HtmlRendering" Visible="false">
                <Configuration>
                    <DeviceInfo>
                        <DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing>
                    </DeviceInfo>
                </Configuration>
            </Extension>
            <Extension Name="HTML5" Type="Microsoft.ReportingServices.Rendering.HtmlRenderer.Html5RenderingExtension,Microsoft.ReportingServices.HtmlRendering" Visible="false">
                <Configuration>
                    <DeviceInfo>
                        <DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing>
                    </DeviceInfo>
                </Configuration>
            </Extension>
            <Extension Name="RPL" Type="Microsoft.ReportingServices.Rendering.RPLRendering.RPLRenderer,Microsoft.ReportingServices.RPLRendering" Visible="false" LogAllExecutionRequests="false"/>
        </Render>
        <!--
        For the SQLPDW extension to work, install the SQL Server PDW Client Tools on the report server.
        NOTE: The SQLPDW extension is deprecated. It supports old versions of SQL Server Parallel Data Warehouse (PDW).        
        To connect to Analytics Platform System, use the SQL (SQL Server) extension.        
        For the ORACLE extension to work, install the Oracle Data Provider for NET (ODP.NET) on the report server.
        For TERADATA extension to work, install the .NET Provider for Teradata on the report server.
      -->
        <Data>
            <Extension Name="SQL" Type="Microsoft.ReportingServices.DataExtensions.SqlConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="SQLAZURE" Type="Microsoft.ReportingServices.DataExtensions.SqlAzureConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="SQLPDW" Type="Microsoft.ReportingServices.DataExtensions.SqlDwConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="OLEDB-MD" Type="Microsoft.ReportingServices.DataExtensions.AdoMdConnection,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="SHAREPOINTLIST" Type="Microsoft.ReportingServices.DataExtensions.SharePointList.SPListConnection,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="ORACLE" Type="Microsoft.ReportingServices.DataExtensions.OracleClientConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="ESSBASE" Type="Microsoft.ReportingServices.DataExtensions.Essbase.EssbaseConnection,Microsoft.ReportingServices.DataExtensions.Essbase"/>
            <Extension Name="SAPBW" Type="Microsoft.ReportingServices.DataExtensions.SapBw.SapBwConnection,Microsoft.ReportingServices.DataExtensions.SapBw"/>
            <Extension Name="TERADATA" Type="Microsoft.ReportingServices.DataExtensions.TeradataConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="OLEDB" Type="Microsoft.ReportingServices.DataExtensions.OleDbConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="ODBC" Type="Microsoft.ReportingServices.DataExtensions.OdbcConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="XML" Type="Microsoft.ReportingServices.DataExtensions.XmlDPConnection,Microsoft.ReportingServices.DataExtensions"/>
        </Data>
        <SemanticQuery>
            <Extension Name="SQL" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MSSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
                <Configuration>
                    <EnableMathOpCasting>False</EnableMathOpCasting>
                </Configuration>
            </Extension>
            <Extension Name="SQLAZURE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MSSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
                <Configuration>
                    <EnableMathOpCasting>False</EnableMathOpCasting>
                </Configuration>
            </Extension>
            <Extension Name="SQLPDW" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQLADW.MSSqlAdwSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
                <Configuration>
                    <EnableMathOpCasting>False</EnableMathOpCasting>
                </Configuration>
            </Extension>
            <Extension Name="ORACLE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Oracle.OraSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
                <Configuration>
                    <EnableMathOpCasting>True</EnableMathOpCasting>
                    <DisableNO_MERGEInLeftOuters>False</DisableNO_MERGEInLeftOuters>
                    <EnableUnistr>False</EnableUnistr>
                    <DisableTSTruncation>False</DisableTSTruncation>
                </Configuration>
            </Extension>
            <Extension Name="TERADATA" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Teradata.TdSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
                <Configuration>
                    <EnableMathOpCasting>True</EnableMathOpCasting>
                    <ReplaceFunctionName>oREPLACE</ReplaceFunctionName>
                </Configuration>
            </Extension>
            <Extension Name="OLEDB-MD" Type="Microsoft.AnalysisServices.Modeling.QueryExecution.ASSemanticQueryCommand,Microsoft.AnalysisServices.Modeling"/>
        </SemanticQuery>
        <ModelGeneration>
            <Extension Name="SQL" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MsSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
            <Extension Name="SQLAZURE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MsSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
            <Extension Name="ORACLE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Oracle.OraSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
            <Extension Name="TERADATA" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Teradata.TdSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
            <Extension Name="OLEDB-MD" Type="Microsoft.AnalysisServices.Modeling.Generation.ModelGeneratorExtention,Microsoft.AnalysisServices.Modeling"/>
        </ModelGeneration>
        <Security>
            <Extension Name="Windows" Type="Microsoft.ReportingServices.Authorization.WindowsAuthorization, Microsoft.ReportingServices.Authorization"/>
        </Security>
        <Authentication>
            <Extension Name="Windows" Type="Microsoft.ReportingServices.Authentication.WindowsAuthentication, Microsoft.ReportingServices.Authorization"/>
        </Authentication>
        <EventProcessing>
            <Extension Name="SnapShot Extension" Type="Microsoft.ReportingServices.Library.HistorySnapShotCreatedHandler,ReportingServicesLibrary">
                <Event>
                    <Type>ReportHistorySnapshotCreated</Type>
                </Event>
            </Extension>
            <Extension Name="Timed Subscription Extension" Type="Microsoft.ReportingServices.Library.TimedSubscriptionHandler,ReportingServicesLibrary">
                <Event>
                    <Type>TimedSubscription</Type>
                </Event>
            </Extension>
            <Extension Name="Cache Refresh Plan Extension" Type="Microsoft.ReportingServices.Library.CacheRefreshPlanHandler,ReportingServicesLibrary">
                <Event>
                    <Type>RefreshCache</Type>
                </Event>
            </Extension>
            <Extension Name="Shared Dataset Cache Update Extension" Type="Microsoft.ReportingServices.Library.SharedDatasetCacheUpdatePlanHandler,ReportingServicesLibrary">
                <Event>
                    <Type>SharedDatasetCacheUpdate</Type>
                </Event>
            </Extension>
            <Extension Name="Cache Update Extension" Type="Microsoft.ReportingServices.Library.ReportExecutionSnapshotUpdateEventHandler,ReportingServicesLibrary">
                <Event>
                    <Type>SnapshotUpdated</Type>
                </Event>
            </Extension>
        </EventProcessing>
    </Extensions>
    <MapTileServerConfiguration>
        <MaxConnections>2</MaxConnections>
        <Timeout>10</Timeout>
        <AppID>(Default)</AppID>
        <CacheLevel>Default</CacheLevel>
    </MapTileServerConfiguration>
</Configuration> 
```  
  
##  <a name="default-configuration-file-for-a-sharepoint-mode-report-server"></a><a name="bkmk_sharepointdefaultfile"></a> Fichier de configuration par défaut d'un serveur de rapports en mode SharePoint  
 Le fichier rsreportserver.config est installé à l'emplacement par défaut suivant :  
  
 **C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting**  
  
```  
<Configuration>  
  <Dsn />  
  <ConnectionType>Default</ConnectionType>  
  <LogonUser>  
  </LogonUser>  
  <LogonDomain>  
  </LogonDomain>  
  <LogonCred>  
  </LogonCred>  
  <InstanceId>MSRS12.@Sharepoint</InstanceId>  
  <Add Key="SecureConnectionLevel" Value="0" />  
  <Add Key="CleanupCycleMinutes" Value="10" />  
  <Add Key="MaxActiveReqForOneUser" Value="20" />  
  <Add Key="AlertingCleanupCycleMinutes" Value="20" />  
  <Add Key="AlertingDataCleanupMinutes" Value="360" />  
  <Add Key="AlertingExecutionLogCleanupMinutes" Value="10080" />  
  <Add Key="AlertingMaxDataRetentionDays" Value="180" />  
  <Add Key="RunningRequestsScavengerCycle" Value="60" />  
  <Add Key="RunningRequestsDbCycle" Value="60" />  
  <Add Key="RunningRequestsAge" Value="30" />  
  <Add Key="MaxScheduleWait" Value="5" />  
  <Add Key="DisplayErrorLink" Value="true" />  
  <Add Key="WebServiceUseFileShareStorage" Value="false" />  
  <!--  <Add Key="ProcessTimeout" Value="150" /> -->  
  <!--  <Add Key="ProcessTimeoutGcExtension" Value="30" /> -->  
  <!--  <Add Key="WatsonFlags" Value="0x0430" /> full dump-->  
  <!--  <Add Key="WatsonFlags" Value="0x0428" /> minidump -->  
  <!--  <Add Key="WatsonFlags" Value="0x0002" /> no dump-->  
  <Add Key="WatsonFlags" Value="0x0428" />  
  <Add Key="WatsonDumpOnExceptions" Value="Microsoft.ReportingServices.Diagnostics.Utilities.InternalCatalogException,Microsoft.ReportingServices.Modeling.InternalModelingException,Microsoft.ReportingServices.ReportProcessing.UnhandledReportRenderingException" />  
  <Add Key="WatsonDumpExcludeIfContainsExceptions" Value="System.Threading.ThreadAbortException,System.Web.UI.ViewStateException,System.OutOfMemoryException,System.Web.HttpException,System.IO.IOException,System.IO.FileLoadException,Microsoft.SharePoint.SPException,Microsoft.ReportingServices.WmiProvider.WMIProviderException" />  
  <RStrace>  
    <add name="FileName" value="ReportServerService" />  
    <add name="FileSizeLimitMb" value="32" />  
    <add name="KeepFilesForDays" value="14" />  
    <add name="Prefix" value="tid, time" />  
    <add name="TraceListeners" value="file" />  
    <add name="TraceFileMode" value="unique" />  
    <add name="Components" value="all:3" />  
  </RStrace>  
  <URLReservations>  
    <Application>  
      <Name>ReportServerWebService</Name>  
      <VirtualDirectory>ReportServer</VirtualDirectory>  
      <URLs>  
        <URL>  
          <UrlString>https://+:80</UrlString>  
          <AccountSid>  
          </AccountSid>  
          <AccountName>  
          </AccountName>  
        </URL>  
      </URLs>  
    </Application>  
    <Application>  
      <Name>ReportManager</Name>  
      <VirtualDirectory>Reports</VirtualDirectory>  
      <URLs>  
        <URL>  
          <UrlString>https://+:80</UrlString>  
          <AccountSid>  
          </AccountSid>  
          <AccountName>  
          </AccountName>  
        </URL>  
      </URLs>  
    </Application>  
  </URLReservations>  
  <Authentication>  
    <AuthenticationTypes>  
      <RSWindowsNTLM />  
    </AuthenticationTypes>  
    <EnableAuthPersistence>true</EnableAuthPersistence>  
  </Authentication>  
  <Service>  
    <IsSchedulingService>True</IsSchedulingService>  
    <IsNotificationService>True</IsNotificationService>  
    <IsEventService>True</IsEventService>  
    <IsAlertingService>True</IsAlertingService>  
    <PollingInterval>10</PollingInterval>  
    <WindowsServiceUseFileShareStorage>False</WindowsServiceUseFileShareStorage>  
    <MemorySafetyMargin>80</MemorySafetyMargin>  
    <MemoryThreshold>90</MemoryThreshold>  
    <RecycleTime>720</RecycleTime>  
    <MaxAppDomainUnloadTime>30</MaxAppDomainUnloadTime>  
    <MaxQueueThreads>0</MaxQueueThreads>  
    <UrlRoot>  
    </UrlRoot>  
    <PolicyLevel>rssrvpolicy.config</PolicyLevel>  
    <IsWebServiceEnabled>True</IsWebServiceEnabled>    
    <FileShareStorageLocation>  
      <Path>  
      </Path>  
    </FileShareStorageLocation>  
  </Service>  
  <UI>  
    <ReportServerUrl>  
    </ReportServerUrl>  
    <PageCountMode>Estimate</PageCountMode>  
  </UI>  
  <MapTileServerConfiguration>  
    <MaxConnections>2</MaxConnections>  
    <Timeout>10</Timeout>  
    <AppID>(Default)</AppID>  
    <CacheLevel>Default</CacheLevel>  
  </MapTileServerConfiguration>  
</Configuration>  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Modifier un fichier de configuration Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)   
 [Configurer la mémoire disponible pour les applications du serveur de rapports](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)   
 [Fichiers de configuration de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Initialiser un serveur de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md)   
 [Stocker des données chiffrées du serveur de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md)   
 [Gestionnaire de configuration du serveur de rapports &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  

 D’autres questions ? [Essayez le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
  
