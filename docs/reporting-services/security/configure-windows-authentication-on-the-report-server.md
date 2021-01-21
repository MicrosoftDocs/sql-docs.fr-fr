---
description: Configurer une authentification Windows sur le serveur de rapports
title: Configurer une authentification Windows sur le serveur de rapports | Microsoft Docs
ms.date: 06/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Windows authentication [Reporting Services]
- Reporting Services, configuration
ms.assetid: 4de9c3dd-0ee7-49b3-88bb-209465ca9d86
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 53e005fcd5c4a11270a5bc2f7c307f521311c548
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596651"
---
# <a name="configure-windows-authentication-on-the-report-server"></a>Configurer une authentification Windows sur le serveur de rapports
  Par défaut, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] accepte les demandes qui spécifient l'authentification Negotiate ou NTLM. Si votre déploiement inclut des applications clientes et des navigateurs clients qui utilisent ces fournisseurs de sécurité, vous pouvez utiliser les valeurs par défaut sans configuration supplémentaire. Si vous voulez utiliser un fournisseur de sécurité différent pour la sécurité intégrée de Windows (par exemple, si vous voulez utiliser Kerberos directement), ou si vous avez modifié les valeurs par défaut et que vous voulez restaurer les paramètres d'origine, vous pouvez utiliser les informations de cette rubrique pour spécifier des paramètres d'authentification sur le serveur de rapports.  
  
 Pour utiliser la sécurité intégrée de Windows, chaque utilisateur qui a besoin d'un accès à un serveur de rapports doit avoir un compte local ou d'utilisateur de domaine Windows valide, ou être membre d'un compte local ou de groupe de domaine Windows. Vous pouvez inclure des comptes d'autres domaines tant que ces domaines sont approuvés. Les comptes doivent avoir accès à l'ordinateur serveur de rapports, et être ensuite attribués à des rôles pour pouvoir accéder à des opérations du serveur de rapports spécifiques.  
  
 Les exigences supplémentaires suivantes doivent également être respectées :  
  
-   La valeur pour **AuthenticationType** dans les fichiers RSReportServer.config doit être **RSWindowsNegotiate**, **RSWindowsKerberos** ou **RSWindowsNTLM**. Par défaut, le fichier RSReportServer.config inclut le paramètre **RSWindowsNegotiate** si le compte de service Report Server est NetworkService ou LocalSystem ; sinon, le paramètre **RSWindowsNTLM** est utilisé. Vous pouvez ajouter **RSWindowsKerberos** si vous possédez des applications qui utilisent uniquement l'authentification Kerberos.  
  
    > [!IMPORTANT]  
    >  L’utilisation de **RSWindowsNegotiate** provoquera une erreur d’authentification Kerberos si vous avez configuré le service Report Server pour qu’il s’exécute sous un compte d’utilisateur de domaine alors que vous n’avez pas inscrit un nom de principal du service (SPN) pour ce compte. Pour plus d'informations, consultez [Résolution des erreurs d'authentification Kerberos lors de la connexion à un serveur de rapports](#proxyfirewallRSWindowsNegotiate) dans cette rubrique.  
  
-   [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] doit être configuré pour l'authentification Windows. Par défaut, les fichiers Web.config du service web Report Server incluent le paramètre \<authentication mode="Windows">. Si vous le remplacez par \<authentication mode="Forms">, l’authentification Windows pour [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] échouera.  
  
-   Les fichiers Web.config du service web Report Server doivent avoir le paramètre \<identity impersonate= "true" />.  
  
-   L'application cliente ou le navigateur client doivent prendre en charge la sécurité intégrée de Windows.  

- Le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] ne nécessite aucune configuration supplémentaire.
  
 Pour modifier les paramètres d'authentification du serveur de rapports, modifiez les éléments et valeurs XML dans le fichier RSReportServer.config. Vous pouvez copier et coller les exemples de cette rubrique pour implémenter des combinaisons spécifiques.  
  
 Les paramètres par défaut sont satisfaisants si tous les ordinateurs clients et serveurs se trouvent dans le même domaine ou dans un domaine approuvé, et si le serveur de rapports est déployé pour l'accès intranet derrière un pare-feu d'entreprise. Des domaines approuvés et uniques sont indispensables pour la transmission d'informations d'identification Windows. Les informations d'identification peuvent être transmises plusieurs fois si vous activez le protocole Kerberos version 5 pour vos serveurs. Dans le cas contraire, elles peuvent être passées une seule fois avant d'expirer. Pour plus d’informations sur la configuration des informations d’identification pour plusieurs connexions d’ordinateurs, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
 Les instructions suivantes sont destinées à un serveur de rapports en mode natif. Si le serveur de rapports est déployé en mode intégré SharePoint, vous devez utiliser les paramètres d'authentification par défaut qui spécifient la sécurité intégrée de Windows. Le serveur de rapports utilise des fonctionnalités internes dans l'extension d'authentification Windows par défaut pour prendre en charge les serveurs de rapports en mode intégré SharePoint.  
  
## <a name="extended-protection-for-authentication"></a>Protection étendue de l'authentification  
 À compter de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], la prise en charge de la protection étendue de l'authentification est disponible. La fonctionnalité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge l'utilisation de la liaison de canal et la liaison de service pour améliorer la protection de l'authentification. Les fonctionnalités [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] doivent être utilisées avec un système d'exploitation qui prend en charge la protection étendue. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour la protection étendue est déterminée par les paramètres contenus dans le fichier RSReportServer.config. Le fichier peut être mis à jour en le modifiant ou à l'aide des API WMI. Pour plus d’informations, consultez [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md).  
  
### <a name="to-configure-a-report-server-to-use-windows-integrated-security"></a>Pour configurer un serveur de rapports afin qu'il utilise la sécurité intégrée de Windows  
  
1.  Ouvrez RSReportServer.config dans un éditeur de texte.  
  
2.  Recherchez \<**Authentication**>.  
  
3.  Copiez, parmi les structures XML suivantes, celle qui répond le mieux à vos besoins. Vous pouvez spécifier **RSWindowsNegotiate**, **RSWindowsNTLM** et **RSWindowsKerberos** dans n'importe quel ordre. Vous devez activer la permanence de l'authentification si vous voulez authentifier la connexion plutôt que chaque demande individuelle. En cas de permanence de l'authentification, toutes les demandes qui requièrent une authentification seront autorisées pendant la durée de la connexion.  
  
     La première structure XML est la configuration par défaut lorsque le compte de service Report Server est NetworkService ou LocalSystem :  
  
    ```  
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsNegotiate />  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    </Authentication>  
    ```  
  
     La deuxième structure XML est la configuration par défaut lorsque le compte de service Report Server n'est ni NetworkService ni LocalSystem :  
  
    ```  
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    ```  
  
     \</Authentication>  
  
     La troisième structure XML spécifie tous les packages de sécurité qui sont utilisés dans la sécurité intégrée de Windows :  
  
    ```  
          <AuthenticationTypes>  
                 <RSWindowsNegotiate />  
                 <RSWindowsKerberos />  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
    ```  
  
     La quatrième structure XML spécifie NTLM uniquement pour les déploiements qui ne prennent pas en charge Kerberos ou pour éviter les erreurs d'authentification Kerberos :  
  
    ```  
          <AuthenticationTypes>  
                 <RSWindowsNTLM />  
          </AuthenticationTypes>  
    ```  
  
4.  Collez-la sur les entrées existantes de \<**Authentication**>.  
  
     Notez que vous ne pouvez pas utiliser **Custom** avec les types **RSWindows** .  
  
5.  Modifiez comme il convient les paramètres de la protection étendue. La protection étendue est désactivée par défaut.  Si ces entrées ne sont pas présentes, l'ordinateur actuel peut ne pas exécuter une version de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui prend en charge la protection étendue. Pour plus d'informations, consultez [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)  
  
    ```  
          <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>  
          <RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionScenario>  
    ```  
  
6.  Enregistrez le fichier .  
  
7.  Si vous avez configuré un déploiement avec montée en puissance parallèle, répétez ces étapes pour d'autres serveurs de rapports du déploiement.  
  
8.  Redémarrez le serveur de rapports pour effacer toutes les sessions qui sont actuellement ouvertes.  
  
##  <a name="resolving-kerberos-authentication-errors-when-connecting-to-a-report-server"></a><a name="proxyfirewallRSWindowsNegotiate"></a> Résolution des erreurs d’authentification Kerberos lors de la connexion à un serveur de rapports  
 Sur un serveur de rapports qui est configuré pour l'authentification Negotiate ou Kerberos, une connexion client au serveur de rapports échouera en cas d'erreur d'authentification Kerberos. Les erreurs d'authentification Kerberos sont connues pour se produire dans les cas suivants :  
  
-   Le service Report Server s'exécute en tant que compte d'utilisateur de domaine Windows et vous n'avez pas inscrit de nom de principal du service (SPN) pour le compte.  
  
-   Le serveur de rapports est configuré avec le paramètre **RSWindowsNegotiate** .  
  
-   Le navigateur choisit Kerberos via NTLM dans l'en-tête d'authentification de la demande qu'il envoie au serveur de rapports.  
  
 Vous pouvez détecter l'erreur si vous avez activé l'enregistrement Kerberos. Le fait que vous soyez invité plusieurs fois à entrer les informations d'identification, puis qu'une fenêtre de navigateur vide s'affiche, constitue un autre symptôme de l'erreur.  
  
 Vous pouvez vérifier que vous rencontrez une erreur d’authentification Kerberos en supprimant < **RSWindowsNegotiate** /> de votre fichier de configuration, puis en tentant une nouvelle connexion.  
  
 Après avoir confirmé le problème, vous pouvez le résoudre de l'une des manières suivantes :  
  
-   Inscrivez un nom de principal du service (SPN) pour le service Report Server sous le compte d'utilisateur de domaine. Pour plus d’informations, consultez [Inscrire un nom de principal du service &#40;SPN&#41; pour un serveur de rapports](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md).  
  
-   Changez de compte de service pour qu'il s'exécute sous un compte intégré tel que le compte de service réseau. Les comptes intégrés mappent le nom de principal du service (SPN) HTTP au nom de principal du service (SPN) hôte, lequel est défini lorsque vous joignez un ordinateur à votre réseau. Pour plus d’informations, consultez [Configurer un compte de service &#40;Gestionnaire de configuration du serveur de rapports&#41;](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).
  
-   Utilisez NTLM. NTLM fonctionnera généralement dans les cas où l'authentification Kerberos échoue. Pour utiliser NTLM, supprimez **RSWindowsNegotiate** du fichier RSReportServer.config et vérifiez que seul **RSWindowsNTLM** est spécifié. Si vous choisissez cette approche, vous pouvez continuer à utiliser un compte d'utilisateur de domaine pour le service Report Server même si vous ne définissez pas de nom de principal du service (SPN) pour ce compte.  
  
#### <a name="logging-information"></a>Informations de journalisation  
 Il existe plusieurs sources d'nformations de journalisation qui peuvent aider à résoudre les problèmes liés à Kerberos.  
  
##### <a name="user-account-control-attribute"></a>Attribut User-Account-Control  
 Déterminez si l'attribut défini pour le compte de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est suffisant dans Active Directory. Examinez le fichier journal de trace des services de création de rapports pour rechercher la valeur enregistrée pour l'attribut UserAccountControl. La valeur enregistrée se présente au format décimal. Vous devez convertir la valeur décimale au format hexadécimal, puis rechercher cette valeur dans la rubrique MSDN qui décrit l'attribut User-Account-Control.  
  
-   L'entrée journal de trace des services de création de rapports doit ressembler à l'exemple suivant :  
  
    ```  
    appdomainmanager!DefaultDomain!8f8!01/14/2010-14:42:28:: i INFO: The UserAccountControl value for the service account is 590336  
    ```  
  
-   Une option de conversion de la valeur décimale au format hexadécimal est pour nous la Calculatrice de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. La Calculatrice de Windows prend en charge plusieurs modes qui affichent les options 'Dec' et 'Hex'. Sélectionnez l'option 'Dec', collez ou tapez la valeur décimale trouvée dans le fichier journal, puis sélectionnez l'option 'Hex'.  
  
-   Reportez-vous ensuite à la rubrique [User-Account-Control Attribute](/windows/win32/adschema/a-useraccountcontrol) (Attribut User-Account-Control) pour dériver l’attribut pour le compte de service.  
  
##### <a name="spns-configured-in-active-directory-for-the-reporting-services-service-account"></a>Noms de principaux du service configurés dans Active Directory pour le compte de service Reporting Services.  
 Pour enregistrer les noms de principaux du service dans le fichier journal de trace du service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vous pouvez activer temporairement la fonctionnalité de protection étendue [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   Modifiez le fichier de configuration **rsreportserver.config** en définissant les éléments suivants :  
  
    ```  
    <RSWindowsExtendedProtectionLevel>Allow</RSWindowsExtendedProtectionLevel>   
    <RSWindowsExtendedProtectionScenario>Any</RSWindowsExtendedProtectionScenario>  
    ```  
  
-   Redémarrez le service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .

 Si vous ne voulez pas continuer à utiliser la protection étendue, rétablissez les valeurs par défaut de configuration et redémarrez le compte de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
```  
<RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>  
<RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionScenario>  
```  
  
 Pour plus d'informations, consultez [Protection étendue de l'authentification avec Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)  
  
#### <a name="how-the-browser-chooses-negotiated-kerberos-or-negotiated-ntlm"></a>Comment le navigateur choisit l'authentification Kerberos par négociation ou l'authentification NTLM par négociation  
 Lorsque vous utilisez Internet Explorer pour vous connecter au serveur de rapports, il spécifie l'authentification Kerberos par négociation ou l'authentification NTLM par négociation sur l'en-tête d'authentification. NTLM est utilisé à la place de Kerberos dans les cas suivants :  
  
-   La demande est envoyée à un serveur de rapports local.  
  
-   La demande est envoyée à une adresse IP du serveur de rapports plutôt qu'à un en-tête d'hôte ou à un nom de serveur.  
  
-   Le logiciel de pare-feu bloque les ports utilisés pour l'authentification Kerberos.  
  
-   Kerberos n'est pas activé pour le système d'exploitation d'un serveur particulier.  
  
-   Le domaine inclut des versions antérieures des systèmes d'exploitation client et serveur Windows qui ne prennent pas en charge la fonctionnalité d'authentification Kerberos intégrée aux versions plus récentes du système d'exploitation.  
  
 En outre, Internet Explorer peut choisir l'authentification Kerberos par négociation ou l'authentification NTLM par négociation en fonction de la configuration des paramètres d'URL, de réseau local et de proxy.  
  
###### <a name="report-server-url"></a>URL du serveur de rapports  
 Si l'URL inclut un nom de domaine complet, Internet Explorer sélectionne NTLM. Si l'URL spécifie l'hôte local (localhost), Internet Explorer sélectionne NTLM. Si l'URL spécifie le nom réseau de l'ordinateur, Internet Explorer sélectionne l'authentification Negotiate, qui réussira ou échouera suivant qu'il existe, ou non, un nom de principal du service (SPN) pour le compte de service Report Server.  
  
###### <a name="lan-and-proxy-settings-on-the-client"></a>Paramètres de réseau local et de proxy sur le client  
 Les paramètres de réseau local et de proxy que vous définissez dans Internet Explorer peuvent déterminer si l'authentification NTLM est préférée à Kerberos. Toutefois, étant donné que les paramètres de réseau local et de proxy varient entre plusieurs organisations, il n'est pas possible de déterminer précisément les paramètres exacts qui contribuent aux erreurs d'authentification Kerberos. Par exemple, votre organisation peut appliquer des paramètres de proxy qui transforment des URL d'intranet en URL de noms de domaine complets qui se résolvent via des connexions Internet. Si des fournisseurs d'authentification différents sont utilisés pour les types différents d'URL, vous constaterez peut-être que certaines connexions réussissent alors que vous vous attendiez à ce qu'elles échouent.  
  
 Si vous rencontrez des erreurs de connexion qui sont, selon vous, dues à des échecs d'authentification, vous pouvez essayer des combinaisons différentes de paramètres de réseau local et de proxy pour isoler le problème. Dans Internet Explorer, les paramètres de réseau local et de proxy se trouvent dans la boîte de dialogue **Paramètres du réseau local** , que vous ouvrez en cliquant sur **Paramètres réseau** sous l’onglet **Connexion** d’ **Options Internet**.  
  
## <a name="external-resources"></a>Ressources externes  
  
-   Pour plus d'informations concernant Kerberos et les serveurs de rapports, consultez [Deploying a Business Intelligence Solution Using SharePoint, Reporting Services, and PerformancePoint Monitoring Server with Kerberos (en anglais).](https://go.microsoft.com/fwlink/?LinkID=177751)  
  
## <a name="see-also"></a>Voir aussi  
 [Authentification avec le serveur de rapports](../../reporting-services/security/authentication-with-the-report-server.md)   
 [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
 [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Configurer une authentification de base sur le serveur de rapports](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)   
 [Configurer l’authentification personnalisée ou par formulaire sur le serveur de rapports](../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md)   
 [Protection étendue de l'authentification avec Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)  
  
