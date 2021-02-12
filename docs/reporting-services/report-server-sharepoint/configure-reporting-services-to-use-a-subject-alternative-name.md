---
title: Configurer Reporting Services pour utiliser un autre nom d’objet (SAN) | Microsoft Docs
description: Découvrez comment configurer SQL Server Reporting Services et Power BI Report Server pour utiliser un autre nom d’objet en modifiant le fichier rsreportserver.config et en utilisant l’outil Netsh.exe.
ms.date: 09/27/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 25e5fce47d86ea4d544b43f297fcb793d8167a66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065154"
---
# <a name="configure-reporting-services-to-use-a-subject-alternative-name-san"></a>Configurer Reporting Services pour utiliser un autre nom d’objet (SAN)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

Cette rubrique explique comment configurer Reporting Services (SSRS) et Power BI Report Server pour utiliser un autre nom d’objet (SAN, Subject Alternative Name) en modifiant le fichier rsreportserver.config et en utilisant l’outil Netsh.exe.

Les instructions s’appliquent à l’URL du service web ainsi qu’à l’URL du portail web dans l’outil Gestionnaire de configuration du serveur de rapports.

Pour utiliser un nom SAN, le certificat TLS/SSLdoit être inscrit sur le serveur, être signé et posséder la clé privée. Vous ne pouvez pas utiliser un certificat auto-signé.

Dans Reporting Services Power BI Report Server, les URL peuvent être configurées de sorte qu’elles utilisent un certificat TLS/SSL. Normalement, un certificat a juste un nom d'objet qui n'autorise qu'une seule URL pour une session TLS (Transport Layer Security), anciennement SSL (Secure Sockets Layer). Le nom SAN est un champ supplémentaire dans le certificat qui permet à un service TLS d’être à l’écoute de plusieurs URL. Il permet également au service TLS de partager le port TLS avec d’autres applications. Par exemple, un nom SAN peut ressembler à `www.myreports.com`.

Pour plus d’informations sur l’activation de TLS pour Reporting Services, consultez [Configurer des connexions TLS sur un serveur de rapports en mode natif](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
## <a name="configure-to-use-a-subject-alternative-name-for-web-service-url"></a>Configurer l’utilisation d’un autre nom de l’objet pour l’URL du service web
  
1.  Démarrez le Gestionnaire de configuration du serveur de rapports.  
  
     Pour plus d’informations, consultez [Gestionnaire de configuration du serveur de rapports &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2.  Dans la page **URL du service web** , sélectionnez un port TLS/SSL et un certificat TLS/SSL.  
  
     ![Gestionnaire de configuration service Web Report Server](../../reporting-services/report-server-sharepoint/media/reportingservices-configurationmanager.png "Gestionnaire de configuration service Web Report Server")  
  
     Le gestionnaire de configuration inscrit le certificat TLS/SSL pour le port.  
  
3.  Ouvrez le fichier rsreportserver.config.  
  
     Pour SSRS 2016 en mode natif, le fichier se trouve par défaut dans le dossier suivant :  
  
    ```  
    \Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer  
    ```  
  
     Pour SSRS 2017 et versions ultérieures, le fichier se trouve par défaut dans le dossier suivant :  
  
    ```  
    \Program Files\Microsoft SQL Server Reporting Services\SSRS\ReportServer  
    ```  
    
     Pour Power BI Report Server, le fichier se trouve par défaut dans le dossier suivant :  
  
    ```  
    \Program Files\Microsoft Power BI Report Server\PBIRS\ReportServer  
    ```  
  
4.  Copiez la section URL de l’application **ReportServerWebService**.
  
     Par exemple, voici la section URL d’origine :  
  
    ```  
        <URL>  
         <UrlString>https://+:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
  
    ```  
  
     Et la section URL modifiée :
  
    ```  
    <URL>  
         <UrlString>https://+:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
        <URL>  
         <UrlString>https://www.myreports.com:443</UrlString>  
         <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051/AccountSid>  
         <AccountName>NT Service\ReportServer</AccountName>  
        </URL>  
  
    ```  
  
  > [!TIP]  
>  * Pour SSRS 2017 et versions ultérieures, la valeur `AccountSid` est `S-1-5-80-4050220999-2730734961-1537482082-519850261-379003301` et la valeur `AccountName` est `NT SERVICE\SQLServerReportingServices`.
>  * Pour Power BI Report Server, la valeur `AccountSid` est `S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663` et la valeur `AccountName` est `NT SERVICE\PowerBIReportServer`.
  
5.  Enregistrez le fichier rsreportserver.config.  
  
6.  Démarrez une invite de commandes en utilisant **Exécuter en tant qu’administrateur**, puis exécutez l’outil Netsh.exe.  
  
    ```  
    C:\windows\system32\netsh  
    ```  
  
7.  Passez au contexte http en tapant ce qui suit.  
  
    ```  
    Netsh>http  
    ```  
  
8.  Affichez les urlacl existantes en tapant ce qui suit :
  
    ```  
    Netsh http>show urlacl  
    ```  
  
     Une entrée semblable à ce qui suit s'affiche.  
  
    ```  
    Reserved URL            : https://+:443/ReportServer/  
        User: NT SERVICE\ReportServer  
            Listen: Yes  
            Delegate: No  
            SDDL: D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
    ```  
  
     Une urlacl est une liste de contrôle d'accès discrétionnaire (DACL, Discretionary Access Control List) pour une URL réservée.  
  
9. Créez une entrée pour l’autre nom de l’objet, (SAN) avec le même utilisateur et le même SDDL que l’entrée existante, en tapant ce qui suit :  
  
    ```  
    netsh http>add urlacl  url=https://www.myreports.com:443/ReportServer    
    user="NT Service\ReportServer" sddl=D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
  
    ```  
  
10. Pour l’**URL du portail web**, créez une entrée pour l’autre nom de l’objet (SAN) en tapant ce qui suit :

    ```  
    netsh http>add urlacl  url=https://www.myreports.com:443/Reports  
    user="NT Service\ReportServer" sddl=D:(A;;GX;;;S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051)  
  
    ```  
> [!TIP]  
>  * Pour SSRS 2017 et versions ultérieures, la valeur `user` est `NT SERVICE\SQLServerReportingServices` et la valeur `sddl` est `D:(A;;GX;;;S-1-5-80-4050220999-2730734961-1537482082-519850261-379003301)`.
>  * Pour Power BI Report Server, la valeur `user` est `NT SERVICE\PowerBIReportServer` et la valeur `sddl` est `S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663`

> [!NOTE]  
> Pour Power BI Report Server, vous devez créer deux entrées supplémentaires pour l’autre nom de l’objet (SAN) en tapant ce qui suit :
>  * `add urlacl url=https://www.myreports.com:443/PowerBI user="NT SERVICE\PowerBIReportServer" sddl=D:(A;;GX;;;S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663)`
>  * `add urlacl url=https://www.myreports.com:443/wopi user="NT SERVICE\PowerBIReportServer" sddl=D:(A;;GX;;;S-1-5-80-1730998386-2757299892-37364343-1607169425-3512908663)`

11. Dans la page **État de Report Server** du Gestionnaire de configuration du serveur de rapports, cliquez sur **Arrêter**, puis sur **Démarrer** pour redémarrer le serveur de rapports.  
  
## <a name="see-also"></a>Voir aussi

 [Fichier de configuration RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Gestionnaire de configuration du serveur de rapports](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Modifier un fichier de configuration Reporting Services](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)   
 [Configurer des URL de serveurs de rapports](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
