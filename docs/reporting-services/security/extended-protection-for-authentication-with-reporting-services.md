---
description: Protection étendue de l'authentification avec Reporting Services
title: Protection étendue de l’authentification avec Reporting Services | Microsoft Docs
ms.date: 06/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
ms.assetid: eb5c6f4a-3ed5-430b-a712-d5ed4b6b9b2b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 72e29a5be172bdf00f9c57358cbbaa1e9bae5a09
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594819"
---
# <a name="extended-protection-for-authentication-with-reporting-services"></a>Protection étendue de l'authentification avec Reporting Services

  La protection étendue est un ensemble d'améliorations apportées aux dernières versions du système d'exploitation [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. La protection étendue améliore la manière dont les applications protègent les informations d'identification et l'authentification. La fonctionnalité ne fournit pas directement de protection contre des attaques spécifiques telles que le transfert d'informations d'identification, mais offre une infrastructure aux applications telles que [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] afin d'appliquer la protection étendue de l'authentification.  
  
 La liaison de service et la liaison de canal figurent parmi les améliorations d'authentification les plus importantes de la protection étendue. La liaison de canal utilise un jeton de liaison de canal (FAO) afin de vérifier que le canal établi entre deux points d'arrêt n'a pas été compromis. La liaison de service utilise les noms de principaux du service (SPN) pour valider la destination prévue de jetons d'authentification. Pour plus d’informations générales sur la protection étendue, consultez [Integrated Windows Authentication with Extended Protection](/previous-versions/visualstudio/visual-studio-2008/dd639324(v=vs.90))(Authentification Windows intégrée avec protection étendue).  
  
SQL Server Reporting Services (SSRS) prend en charge et applique la protection étendue qui a été activée dans le système d’exploitation et configurée dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Par défaut, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] accepte les demandes qui spécifient l'authentification Negotiate ou NTLM et peut donc tirer parti de la prise en charge de la protection étendue dans le système d'exploitation et des fonctionnalités de la protection étendue de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Par défaut, Windows n'active pas la protection étendue. Pour plus d'informations sur l'activation de la protection étendue dans Windows, consultez [Protection étendue de l'authentification](https://go.microsoft.com/fwlink/?LinkID=178431). Le système d'exploitation et la pile d'authentification du client doivent tous les deux prendre en charge la protection étendue pour que l'authentification aboutisse. Vous devrez peut-être installer plusieurs mises à jour sur les systèmes d'exploitation plus anciens pour bénéficier d'une protection étendue à jour et prête à l'emploi sur l'ordinateur. Pour plus d’informations sur les développements les plus récents de la protection étendue, consultez les [informations mises à jour relatives à la protection étendue](/previous-versions/sql/sql-server-2008/dd146365(v=sql.100)).  

## <a name="reporting-services-extended-protection-overview"></a>Vue d'ensemble de la protection étendue Reporting Services

SSRS prend en charge et applique la protection étendue qui a été activée dans le système d’exploitation. Si le système d'exploitation ne prend pas en charge la protection étendue ou si la fonctionnalité n'a pas été activée dans le système d'exploitation, la fonctionnalité de protection étendue de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne parviendra pas à effectuer l'authentification. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] requiert également un certificat TLS/SSL. Pour plus d’informations, consultez [Configurer des connexions TLS sur un serveur de rapports en mode natif](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md)  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] n'active pas la protection étendue par défaut. La fonctionnalité peut être activée en modifiant le fichier de configuration **rsreportserver.config** ou en utilisant les API WMI pour mettre le fichier de configuration à jour. SSRS ne propose pas d’interface utilisateur pour modifier ou visualiser les paramètres de protection étendus. Pour plus d'informations, consultez la section des [paramètres de configuration](#ConfigurationSettings) dans cette rubrique.  
  
 Les problèmes courants qui se produisent à cause des modifications des paramètres de protection étendue ou de paramètres mal configurés ne décrivent pas les messages d'erreur ou les boîtes de dialogue habituelles. Les problèmes sur la configuration et la compatibilité de la protection étendue aboutissent à des échecs et des erreurs d'authentification dans les journaux de trace [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
> [!IMPORTANT]
>  Certaines technologies d'accès aux données peuvent ne pas prendre en charge la protection étendue. Une technologie d'accès aux données est utilisée pour se connecter aux sources de données SQL Server et à la base de données du catalogue [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . L'échec d'une technologie d'accès aux données dans la prise en charge de la protection étendue a les conséquences suivantes sur [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
> 
>  -   La protection étendue ne peut être activée sur le serveur SQL Server qui exécute la base de données du catalogue [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et le serveur de rapports ne parvient pas à se connecter à la base de données de catalogue et renvoie des erreurs d'authentification.  
> -   Les serveurs SQL Server utilisés comme sources de données de rapport [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne peuvent bénéficier de la protection étendue ou essayant, par le biais du serveur de rapports, de se connecter à la source de données de rapport échouent et retournent des erreurs d'authentification.  
> 
>  La documentation pour une technologie d'accès aux données doit disposer d'informations sur la prise en charge de la protection étendue.  
  
### <a name="upgrade"></a>Mettre à niveau  
  
-   La mise à niveau d’un serveur [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vers SQL Server 2016 ajoute des paramètres de configuration avec les valeurs par défaut au fichier **rsreportserver.config**. Si les paramètres y figurent déjà, l’installation de SQL Server 2016 les laisse dans le fichier **rsreportserver.config**.  
  
-   Quand les paramètres de configuration sont ajoutés au fichier de configuration **rsreportserver.config** , le comportement par défaut de la fonctionnalité de protection étendue de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est désactivé et vous devez activer la fonction comme indiqué dans cette rubrique. Pour plus d'informations, consultez la section des [paramètres de configuration](#ConfigurationSettings) dans cette rubrique.  
  
-   La valeur par défaut du paramètre **RSWindowsExtendedProtectionLevel** est **Off**.  
  
-   La valeur par défaut du paramètre **RSWindowsExtendedProtectionScenario** est **Proxy**.  
  
-   ne vérifie pas que la protection étendue est activée sur le système d'exploitation ou l'installation actuelle de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
### <a name="what-reporting-services-extended-protection-does-not-cover"></a>Champs non couverts par la protection étendue de Reporting Services  
 Les fonctionnalités et scénarios suivants ne sont pas pris en charge par la fonctionnalité de protection étendue [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   Les auteurs des extensions de sécurité personnalisées [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] doivent insérer la prise en charge de la protection étendue à leur extension de sécurité personnalisée.  
  
-   Les composants tiers ajoutés ou utilisés durant une installation de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] doivent être mis à jour par le fournisseur tiers en vue de la prise en charge de la protection étendue. Pour plus d'informations, contactez le fournisseur tiers.  
  
## <a name="deployment-scenarios-and-recommendations"></a>Recommandations et scénarios de déploiement  
 Les scénarios suivants illustrent plusieurs déploiements et topologies ainsi que la configuration recommandée afin de les sécuriser par la protection étendue [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
### <a name="direct"></a>Direct  
 Ce scénario décrit la connexion directe à un serveur de rapports, par exemple, un environnement d'intranet.  
  
|Scénario|Schéma du scénario|Procédure de sécurisation|  
|--------------|----------------------|-------------------|  
|Communication TLS directe.<br /><br /> Le serveur de rapports applique la liaison du canal entre le client et le serveur de rapports.|![RS_ExtendedProtection_DirectSSL](../../reporting-services/security/media/rs-extendedprotection-directssl.gif "RS_ExtendedProtection_DirectSSL")<br /><br /> 1) Application cliente<br /><br /> 2) Serveur de rapports|Définissez **RSWindowsExtendedProtectionLevel** sur **Allow** ou **Require**.<br /><br /> Définissez **RSWindowsExtendedProtectionScenario** sur **Direct**.<br /><br /> <br /><br /> -La liaison de service n’est pas nécessaire, car le canal TLS est utilisé pour la liaison de canal.|  
|Communication HTTP directe. Le serveur de rapports applique la liaison de service entre le client et le serveur de rapports.|![RS_ExtendedProtection_Direct](../../reporting-services/security/media/rs-extendedprotection-direct.gif "RS_ExtendedProtection_Direct")<br /><br /> 1) Application cliente<br /><br /> 2) Serveur de rapports|Définissez **RSWindowsExtendedProtectionLevel** sur **Allow** ou **Require**.<br /><br /> Définissez **RSWindowsExtendedProtectionScenario** sur **Any**.<br /><br /> <br /><br /> -Il n’existe aucun canal TLS et, par conséquent, toute application de la liaison de canal est impossible.<br /><br /> -La liaison de service peut être validée, mais ne constitue pas une ligne de défense parfaite sans la liaison de canal ; la liaison de service à elle seule se contente de bloquer les menaces de base.|  
  
### <a name="proxy-and-network-load-balancing"></a>Équilibrage de la charge réseau et proxy  
 Les applications clientes se connectent à un périphérique ou un logiciel qui effectue une connexion TLS et transmet les informations d'identification au serveur en vue de l'authentification, par exemple, Internet, un réseau extranet ou intranet sécurisé. Le client se connecte à un serveur proxy ou à tous les clients utilisant un proxy.  
  
 La situation est la même lorsque vous utilisez un périphérique d'équilibrage de la charge réseau.  
  
|Scénario|Schéma du scénario|Procédure de sécurisation|  
|--------------|----------------------|-------------------|  
|Communications HTTP. Le serveur de rapports applique la liaison de service entre le client et le serveur de rapports.|![RS_ExtendedProtection_Indirect](../../reporting-services/security/media/rs-extendedprotection-indirect.gif "RS_ExtendedProtection_Indirect")<br /><br /> 1) Application cliente<br /><br /> 2) Serveur de rapports<br /><br /> 3) Proxy|Définissez **RSWindowsExtendedProtectionLevel** sur **Allow** ou **Require**.<br /><br /> Définissez **RSWindowsExtendedProtectionScenario** sur **Any**.<br /><br /> <br /><br /> -Il n’existe aucun canal TLS et, par conséquent, toute application de la liaison de canal est impossible.<br /><br /> -Le serveur de rapports doit être configuré de façon à identifier le serveur proxy pour s’assurer que la liaison de service est correctement appliquée.|  
|Communications HTTP.<br /><br /> Le serveur de rapports applique la liaison de canal entre le client et le proxy et la liaison de service entre le client et le serveur de rapports.|![RS_ExtendedProtection_Indirect_SSL](../../reporting-services/security/media/rs-extendedprotection-indirect-ssl.gif "RS_ExtendedProtection_Indirect_SSL")<br /><br /> 1) Application cliente<br /><br /> 2) Serveur de rapports<br /><br /> 3) Proxy|Définissez <br />                    **RSWindowsExtendedProtectionLevel** sur **Allow** ou **Require**.<br /><br /> Définissez **RSWindowsExtendedProtectionScenario** sur **Proxy**.<br /><br /> <br /><br /> -Le canal TLS vers le proxy est disponible ; par conséquent, la liaison de canal au proxy peut être appliquée.<br /><br /> -La liaison de service est également applicable.<br /><br /> -Le nom du proxy doit être connu du serveur de rapports et l’administrateur du serveur de rapports doit lui créer une réservation d’URL, avec un en-tête d’hôte ou encore configurer le nom de proxy dans l’entrée **BackConnectionHostNames** du Registre Windows.|  
|Communications HTTPS indirectes avec un proxy sécurisé. Le serveur de rapports applique la liaison de canal entre le client et le proxy et la liaison de service entre le client et le serveur de rapports.|![RS_ExtendedProtection_IndirectSSLandHTTPS](../../reporting-services/security/media/rs-extendedprotection-indirectsslandhttps.gif "RS_ExtendedProtection_IndirectSSLandHTTPS")<br /><br /> 1) Application cliente<br /><br /> 2) Serveur de rapports<br /><br /> 3) Proxy|Définissez <br />                    **RSWindowsExtendedProtectionLevel** sur **Allow** ou **Require**.<br /><br /> Définissez **RSWindowsExtendedProtectionScenario** sur **Proxy**.<br /><br /> <br /><br /> -Le canal TLS vers le proxy est disponible ; par conséquent, la liaison de canal au proxy peut être appliquée.<br /><br /> -La liaison de service est également applicable.<br /><br /> -Le nom du proxy doit être connu du serveur de rapports et l’administrateur du serveur de rapports doit lui créer une réservation d’URL, avec un en-tête d’hôte ou encore configurer le nom de proxy dans l’entrée **BackConnectionHostNames** du Registre Windows.|  
  
### <a name="gateway"></a>Passerelle  
 Ce scénario décrit des applications clientes qui se connectent à un dispositif ou à un logiciel qui effectue la connexion TLS et authentifie l'utilisateur. Le périphérique ou le logiciel emprunte l'identité du contexte de l'utilisateur ou d'un utilisateur différent avant que ce dernier n'émette une demande au serveur de rapports.  
  
|Scénario|Schéma du scénario|Procédure de sécurisation|  
|--------------|----------------------|-------------------|  
|Communications HTTP indirectes.<br /><br /> La passerelle applique la liaison de canal entre le client et la passerelle. Il existe une passerelle menant à la liaison de service du serveur de rapports.|![RS_ExtendedProtection_Indirect_SSL](../../reporting-services/security/media/rs-extendedprotection-indirect-ssl.gif "RS_ExtendedProtection_Indirect_SSL")<br /><br /> 1) Application cliente<br /><br /> 2) Serveur de rapports<br /><br /> 3) Périphérique de passerelle|Définissez **RSWindowsExtendedProtectionLevel** sur **Allow** ou **Require**.<br /><br /> Définissez **RSWindowsExtendedProtectionScenario** sur **Any**.<br /><br /> <br /><br /> -La liaison de canal entre le client et le serveur de rapports est impossible, car la passerelle emprunte l’identité d’un contexte, créant ainsi un autre jeton NTLM.<br /><br /> -Il n’existe aucune connexion TLS entre la passerelle et le serveur de rapports ; la liaison de canal ne peut donc pas être appliquée.<br /><br /> -La liaison de service est applicable.<br /><br /> -Le périphérique de passerelle doit être configuré par votre administrateur pour l’application de la liaison de canal.|  
|Communications HTTPS indirectes avec une passerelle sécurisée. La passerelle applique la liaison de canal entre le client et la passerelle et le serveur de rapports applique la liaison de canal entre la passerelle et le serveur de rapports.|![RS_ExtendedProtection_IndirectSSLandHTTPS](../../reporting-services/security/media/rs-extendedprotection-indirectsslandhttps.gif "RS_ExtendedProtection_IndirectSSLandHTTPS")<br /><br /> 1) Application cliente<br /><br /> 2) Serveur de rapports<br /><br /> 3) Périphérique de passerelle|Définissez **RSWindowsExtendedProtectionLevel** sur **Allow** ou **Require**.<br /><br /> Définissez **RSWindowsExtendedProtectionScenario** sur **Direct**.<br /><br /> <br /><br /> -La liaison de canal entre le client et le serveur de rapports est impossible, car la passerelle emprunte l’identité d’un contexte, créant ainsi un autre jeton NTLM.<br /><br /> -La connexion TLS entre la passerelle et le serveur de rapports signifie qu’une liaison de canal est possible.<br /><br /> -La liaison de service n’est pas obligatoire.<br /><br /> -Le périphérique de passerelle doit être configuré par votre administrateur pour l’application de la liaison de canal.|  
  
### <a name="combination"></a>Combinaison  
 Ce scénario décrit des environnements Internet ou Extranet dans lesquels le client se connecte à un proxy. Il fait intervenir un intranet dans lequel un client se connecte au serveur de rapports.  
  
|Scénario|Schéma du scénario|Procédure de sécurisation|  
|--------------|----------------------|-------------------|  
|Les accès indirects et directs du client au serveur de rapports fonctionnent sans TLS pour les connexions entre le client et le proxy ou le client et le serveur de rapports.|1) Application cliente<br /><br /> 2) Serveur de rapports<br /><br /> 3) Proxy<br /><br /> 4) Application cliente|Définissez **RSWindowsExtendedProtectionLevel** sur **Allow** ou **Require**.<br /><br /> Définissez **RSWindowsExtendedProtectionScenario** sur **Any**.<br /><br /> <br /><br /> -La liaison de service entre le client et le serveur de rapports est applicable.<br /><br /> -Le nom du proxy doit être connu du serveur de rapports et l’administrateur du serveur de rapports doit lui créer une réservation d’URL, avec un en-tête d’hôte ou encore configurer le nom de proxy dans l’entrée **BackConnectionHostNames** du Registre Windows.|  
|Accès indirects et directs du client au serveur de rapports sur lequel le client établit une connexion TLS vers le proxy ou le serveur de rapports.|![RS_ExtendedProtection_CombinationSSL](../../reporting-services/security/media/rs-extendedprotection-combinationssl.gif "RS_ExtendedProtection_CombinationSSL")<br /><br /> 1) Application cliente<br /><br /> 2) Serveur de rapports<br /><br /> 3) Proxy<br /><br /> 4) Application cliente|Définissez **RSWindowsExtendedProtectionLevel** sur **Allow** ou **Require**.<br /><br /> Définissez **RSWindowsExtendedProtectionScenario** sur **Proxy**.<br /><br /> <br /><br /> -La liaison de canal peut être utilisée.<br /><br /> -Le nom du proxy doit être connu du serveur de rapports et l’administrateur du serveur de rapports doit lui créer une réservation d’URL, avec un en-tête d’hôte ou encore configurer le nom de proxy dans une entrée **BackConnectionHostNames** du Registre Windows.|  
  
## <a name="configuring-reporting-services-extended-protection"></a>Configuration de la protection étendue de Reporting Services  
 Le fichier **rsreportserver.config** contient les valeurs de configuration qui contrôlent le comportement de la protection étendue de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Pour plus d’informations sur l’utilisation et la modification du fichier **rsreportserver.config** , consultez [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md). Les paramètres de protection étendue peuvent également être modifiés et examinés à l'aide d'API WMI. Pour plus d’informations, consultez [Méthode SetExtendedProtectionSettings &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setextendedprotectionsettings.md)(Authentification Windows intégrée avec protection étendue).  
  
 Si la validation des paramètres de configuration échoue, les types d'authentification **RSWindowsNTLM**, **RSWindowsKerberos** et **RSWindowsNegotiate** sont désactivés sur le serveur de rapports.  
  
###  <a name="configuration-settings-for-reporting-services-extended-protection"></a><a name="ConfigurationSettings"></a> Paramètres de configuration pour la protection étendue de Reporting Services  
 Le tableau suivant donne des informations sur les paramètres de configuration qui figurent dans le fichier **rsreportserver.config** de protection étendue.  
  
|Paramètre|Description|  
|-------------|-----------------|  
|**RSWindowsExtendedProtectionLevel**|Indique le niveau d'application de la protection étendue. Les valeurs autorisées sont :<br /><br /> **Off** : Par défaut. Indique l’absence de vérification de la liaison de canal ou de service.<br /><br /> **Allow** prend en charge la protection étendue, mais ne l’impose pas.  Spécifie les points suivants :<br /><br /> -La protection étendue est appliquée aux applications clientes qui s’exécutent sur les systèmes d’exploitation prenant en charge la protection étendue. La manière dont la protection s’applique dépend du paramètre **RsWindowsExtendedProtectionScenario**.<br /><br /> -L’authentification est autorisée pour les applications fonctionnant sur des systèmes d’exploitation qui ne prennent pas en charge la protection étendue.<br /><br /> **Require** spécifie les points suivants :<br /><br /> -La protection étendue est appliquée aux applications clientes qui s’exécutent sur les systèmes d’exploitation prenant en charge la protection étendue.<br /><br /> -L’authentification **n’est pas** autorisée pour les applications fonctionnant sur des systèmes d’exploitation qui ne prennent pas en charge la protection étendue.|  
|**RsWindowsExtendedProtectionScenario**|Spécifie quelles formes de protection étendue sont validées : Liaison de canal, liaison de service ou les deux. Les valeurs autorisées sont :<br /><br /> **Proxy** : Par défaut. Spécifie les points suivants :<br /><br /> -L’authentification Windows NTLM, Kerberos et Negotiate a lieu lorsqu’un jeton de liaison de canal est présent.<br /><br /> -La liaison de service est appliquée.<br /><br /> **Any** spécifie les points suivants :<br /><br /> -L’authentification Windows NTLM, Kerberos et Negotiate, ainsi qu’une liaison de canal, ne sont pas obligatoires.<br /><br /> -La liaison de service est appliquée.<br /><br /> **Direct** spécifie les points suivants :<br /><br /> -L’authentification Windows NTLM, Kerberos et Negotiate a lieu lorsqu’il existe un jeton de liaison de canal et une connexion TLS au service actuel, et lorsque le jeton de liaison de canal pour la connexion TLS correspond à celui du jeton NTLM, Kerberos ou Negotiate.<br /><br /> -La liaison de service n’est pas appliquée.<br /><br /> <br /><br /> Remarque : Le paramètre **RsWindowsExtendedProtectionScenario** est ignoré si **RsWindowsExtendedProtectionLevel** est défini sur **OFF**.|  
  
 Exemples d'entrées dans le fichier de configuration **rsreportserver.config** :  
  
```  
<Authentication>  
         <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>  
         <RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionLevel>  
</Authentication>  
```  
  
## <a name="service-binding-and-included-spns"></a>Liaison de service et SPN inclus  
 La liaison de service utilise les noms principaux du service (SPN) pour valider la destination prévue des jetons d'authentification. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise les informations de réservation d'URL existantes pour générer la liste des SPN considérés comme valides. L'utilisation des informations de réservation d'URL pour la validation des réservations SPN et URL permet aux administrateurs système de gérer les deux à la fois à partir d'un seul emplacement.  
  
 La liste des SPN valides est mise à jour au démarrage du serveur de rapports démarre, à la modification des paramètres de configuration de la protection étendue ou au recyclage du domaine d'application.  
  
 La liste valide des SPN est spécifique à chaque application. Par exemple, le Gestionnaire de rapports et le serveur de rapports auront chacun leur propre liste de SPN.  
  
 La liste de SPN valides générée pour une application est déterminée par les facteurs suivants :  
  
-   Réservations d'URL.  
  
-   Ensemble des SPN extraits du contrôleur de domaine pour le compte de service Reporting Services.  
  
-   Si une réservation d'URL inclut des caractères génériques (« * » ou « + »), le serveur de rapports ajoutera chaque entrée de la collection d'hôtes.  
  
### <a name="hosts-collection-sources"></a>Sources de collection d'hôtes.  
 Le tableau suivant répertorie les sources potentielles de la collection d'hôtes.  
  
|Type de la source|Description|  
|--------------------|-----------------|  
|ComputerNameDnsDomain|Nom du domaine DNS affecté à l'ordinateur local. Si l'ordinateur local est un nœud dans un cluster, le nom de domaine DNS du serveur virtuel de cluster est utilisé.|  
|ComputerNameDnsFullyQualified|Nom DNS complet qui identifie de manière unique l'ordinateur local. Ce nom est une combinaison du nom d'hôte DNS et du nom de domaine DNS présentée sous la forme *Nom d'hôte*.*Nom de domaine*. Si l'ordinateur local est un nœud dans un cluster, le nom DNS complet du serveur virtuel de cluster est utilisé.|  
|ComputerNameDnsHostname|Nom d'hôte DNS de l'ordinateur local. Si l'ordinateur local est un nœud dans un cluster, le nom d'hôte DNS du serveur virtuel de cluster est utilisé.|  
|ComputerNameNetBIOS|Nom NetBIOS de l'ordinateur local. Si l'ordinateur local est un nœud dans un cluster, le nom NetBIOS du serveur virtuel de cluster est utilisé.|  
|ComputerNamePhysicalDnsDomain|Nom du domaine DNS affecté à l'ordinateur local. Si l'ordinateur local est un nœud dans un cluster, le nom de domaine DNS de l'ordinateur local est utilisé, mais pas le nom du serveur virtuel de cluster.|  
|ComputerNamePhysicalDnsFullyQualified|Nom DNS complet qui identifie de manière unique l'ordinateur. Si l'ordinateur local est un nœud dans un cluster, le nom DNS complet de l'ordinateur local est utilisé, mais pas le nom du serveur virtuel de cluster.<br /><br /> Le nom DNS complet est une combinaison du nom d'hôte DNS et du nom de domaine DNS présentée sous la forme *Nom d'hôte*.*Nom de domaine*.|  
|ComputerNamePhysicalDnsHostname|Nom d'hôte DNS de l'ordinateur local. Si l'ordinateur local est un nœud dans un cluster, le nom d'hôte DNS de l'ordinateur local est utilisé, mais pas le nom du serveur virtuel de cluster.|  
|ComputerNamePhysicalNetBIOS|Nom NetBIOS de l'ordinateur local. Si l'ordinateur local est un nœud dans un cluster, le nom NetBIOS de l'ordinateur local est utilisé, mais pas le nom du serveur virtuel de cluster.|  
  
Pour plus d’informations, consultez [Inscrire un nom de principal du service &#40;SPN&#41; pour un serveur de rapports](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md) et [À propos des réservations et de l’inscription d’URL &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md).  
  
## <a name="next-steps"></a>Étapes suivantes

[Se connecter au moteur de base de données à l'aide de la protection étendue](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)   
[Extended Protection for Authentication Overview (en anglais)](/previous-versions/dotnet/netframework-3.5/dd767318(v=vs.90))   
[Integrated Windows Authentication with Extended Protection](/previous-versions/visualstudio/visual-studio-2008/dd639324(v=vs.90))   
[Avis de sécurité Microsoft : Protection étendue de l’authentification](https://go.microsoft.com/fwlink/?LinkId=179923)   
[Report Server Service Trace Log](../../reporting-services/report-server/report-server-service-trace-log.md)   
[RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Méthode SetExtendedProtectionSettings &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setextendedprotectionsettings.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)