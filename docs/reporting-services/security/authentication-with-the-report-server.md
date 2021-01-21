---
description: Authentification avec le serveur de rapports
title: Authentification avec le serveur de rapports | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- connections [Reporting Services], configuring
- connections [Reporting Services], accounts
- Windows authentication [Reporting Services]
- authentication [Reporting Services]
- Forms authentication
ms.assetid: 753c2542-0e97-4d8f-a5dd-4b07a5cd10ab
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8f682e468bffe704df57cb63b811682488909605
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595202"
---
# <a name="authentication-with-the-report-server"></a>Authentification avec le serveur de rapports

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (SSRS) offre plusieurs options configurables pour authentifier les utilisateurs et les applications clientes sur le serveur de rapports. Par défaut, le serveur de rapports utilise l'authentification intégrée Windows et suppose des relations approuvées dans lesquelles le client et les ressources réseau sont dans le même domaine ou dans un domaine approuvé. Selon la topologie de votre réseau et les besoins de votre organisation, vous pouvez personnaliser le protocole d’authentification utilisé pour l’authentification intégrée de Windows, utiliser l’authentification de base ou une extension personnalisée d’authentification basée sur les formulaires que vous fournissez. Chacun de ces types d'authentification peut être activé ou désactivé individuellement. Vous pouvez activer plusieurs types d’authentification si vous voulez que le serveur de rapports accepte des demandes de plusieurs types.
  
 Tous les utilisateurs ou applications qui demandent l'accès au contenu et aux opérations du serveur de rapports doivent être authentifiés avant que l'accès ne soit autorisé.  
  
## <a name="authentication-types"></a>Types d'authentification  
 Tous les utilisateurs ou applications qui demandent l'accès au contenu et aux opérations du serveur de rapports doivent être authentifiés à l'aide du type d'authentification configuré sur le serveur de rapports avant que l'accès ne soit autorisé. Le tableau suivant décrit les types d'authentification pris en charge par [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
|Nom de type d'authentification|Valeur de la couche d'authentification HTTP|Utilisé par défaut|Description|  
|-----------------------------|-------------------------------------|---------------------|-----------------|  
|RSWindowsNegotiate|Negotiate|Oui|Tente d'utiliser Kerberos en premier pour l'authentification intégrée de Windows, mais revient à NTLM si Active Directory ne peut pas accorder de ticket pour la demande du client au serveur de rapports. Negotiate revient à NTLM uniquement si le ticket n'est pas disponible. Si les premières tentatives entraînent une erreur plutôt qu'un ticket manquant, le serveur de rapports n'effectue pas de deuxième tentative.|  
|RSWindowsNTLM|NTLM|Oui|Utilise NTLM pour l'authentification intégrée de Windows.<br /><br /> Les informations d'identification ne seront pas déléguées ou empruntées sur d'autres demandes. Les demandes suivantes suivent une nouvelle séquence de stimulation/réponse. Selon les paramètres de sécurité du réseau, le système peut demander à un utilisateur des informations d'identification ou la demande d'authentification est gérée de façon transparente.|  
|RSWindowsKerberos|Kerberos|Non|Utilise Kerberos pour l'authentification intégrée de Windows. La configuration de Kerberos passe par celle des noms des principes du service (SPN) pour vos comptes de service, lequel requiert des privilèges d'administrateur de domaine. Si vous configurez la délégation d'identité à l'aide de Kerberos, le jeton de l'utilisateur qui demande un rapport peut également être utilisé sur une connexion supplémentaire aux sources de données externes qui fournissent des données aux rapports.<br /><br /> Avant de spécifier RSWindowsKerberos, vérifiez que le type de navigateur que vous utilisez prend bien en charge ce dernier. Si vous utilisez Microsoft Edge ou Internet Explorer, l’authentification Kerberos est prise en charge uniquement par l’intermédiaire de Negotiate. Microsoft Edge ou Internet Explorer ne formule pas de demande d’authentification qui spécifie Kerberos directement.|  
|RSWindowsBasic|De base|Non|L'authentification de base est définie dans le protocole HTTP et peut être utilisée uniquement pour authentifier des requêtes HTTP au serveur de rapports.<br /><br /> Les informations d'identification sont passées dans la requête HTTP à l'aide de l'encodage en base 64. Si vous avez recours à l'authentification de base, utilisez le protocole TLS (Transport Layer Security), précédemment appelé SSL (Secure Sockets Layer) pour chiffrer les informations du compte d'utilisateur avant de les envoyer sur le réseau. Le protocole SSL fournit un canal chiffré pour l'envoi d'une demande de connexion du client au serveur de rapports via une connexion HTTP TCP/IP. Pour plus d’informations, consultez [Using SSL to Encrypt Confidential Data](/previous-versions/windows/it-pro/windows-server-2003/cc738495(v=ws.10)) (Chiffrer les données confidentielles à l’aide de SSL) sur le site web [!INCLUDE[msCoName](../../includes/msconame-md.md)] TechNet.|  
|Custom|(Anonyme)|Non|L'authentification anonyme dirige le serveur de rapports pour ignorer l'en-tête d'authentification dans une requête HTTP. Le serveur de rapports accepte toutes les demandes, mais appelle une authentification par formulaire [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] personnalisée que vous fournissez pour authentifier l'utilisateur.<br /><br /> Spécifiez **Custom** si vous déployez un module d’authentification personnalisé qui gère toutes les demandes d’authentification sur le serveur de rapports. Vous ne pouvez pas utiliser le type d'authentification Personnalisé avec l'extension d'authentification Windows par défaut.|  
  
## <a name="unsupported-authentication-methods"></a>Méthodes d'authentification non prises en charge  
 Les méthodes et demandes d'authentification suivantes ne sont pas prises en charge.  
  
|Méthode d'authentification|Explication|  
|---------------------------|-----------------|  
|Anonyme|Le serveur de rapports n'accepte pas de demandes non authentifiées d'un utilisateur anonyme, à l'exception des déploiements qui incluent une extension d'authentification personnalisée.<br /><br /> Le Générateur de rapports accepte des demandes non authentifiées si vous activez l'accès au Générateur de rapports sur un serveur de rapports configuré pour l'authentification de base.<br /><br /> Pour tous les autres cas, les demandes anonymes sont rejetées avec une erreur État HTTP 401 Accès Refusé avant que la demande ne parvienne à [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. Les clients qui reçoivent cette erreur doivent reformuler la demande avec un type d'authentification valide.|  
|Technologies d'authentification uniques (SSO)|Il n’existe pas de prise en charge native pour les technologies d’authentification unique dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour utiliser cette technologie, vous devez créer une extension d'authentification personnalisée.<br /><br /> L'environnement d'hébergement du serveur de rapports ne prend pas en charge les filtres ISAPI. Si la technologie SSO que vous utilisez est implémentée sous la forme d'un filtre ISAPI, vous pouvez envisager d'utiliser la prise en charge intégrée ISA Server pour RSASecueID ou le protocole RADIUS. Sinon, vous pouvez créer un ISA Server ISAPI ou un HTTPModule pour RS, mais il est recommandé d'utiliser ISA Server directement.|  
|Passport|Non pris en charge dans SQL Server Reporting Services.|  
|Digest|Non pris en charge dans SQL Server Reporting Services.|  
  
## <a name="configuration-of-authentication-settings"></a>Configuration des paramètres d'authentification  
 Les paramètres d'authentification sont configurés pour la sécurité par défaut lorsque l'URL du serveur de rapports est réservée. Si vous modifiez ces paramètres de manière incorrecte, le serveur de rapports retourne des erreurs HTTP 401 Accès Refusé pour les requêtes HTTP qui ne peuvent pas être authentifiées. Le choix d'un type d'authentification nécessite de savoir comment l'authentification Windows est prise en charge sur votre réseau. Vous devez spécifier au moins un type d'authentification. Plusieurs types d'authentification peuvent être spécifiés pour RSWindows. Les types d’authentification RSWindows (à savoir, **RSWindowsBasic**, **RSWindowsNTLM**, **RSWindowsKerberos** et **RSWindowsNegotiate**) s’excluent mutuellement avec Personnalisé.  
  
> [!IMPORTANT]  
>  Reporting Services ne valide pas les paramètres que vous spécifiez pour déterminer s'ils sont corrects pour votre environnement d'informatique. La sécurité par défaut peut ne pas fonctionner pour votre installation, ou et vous pouvez spécifier des paramètres de configuration qui ne sont pas valides pour votre infrastructure de sécurité. Pour cette raison, il est important de tester prudemment votre déploiement de serveur de rapports dans un environnement de test contrôlé avant de le déployer à plus grande échelle dans votre organisation.  
  
 Le service Web Report Server et le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] font toujours appel au même type d’authentification. Vous ne pouvez pas configurer d'autres types d'authentification pour les fonctionnalités du service Report Server. Si vous disposez d'un déploiement par montée en puissance parallèle, veillez à dupliquer toutes vos modifications sur tous les nœuds dans le déploiement. Vous ne pouvez pas configurer d'autres nœuds dans le même déploiement pour utiliser des types d'authentification différents.  
  
 Le traitement en arrière-plan n'accepte pas de demandes d'utilisateurs finaux, cependant il authentifie toutes les demandes à des fins d'exécution sans assistance. Il utilise toujours l'authentification Windows et authentifie des demandes à l'aide du service Report Server ou du compte d'exécution sans assistance s'il est configuré.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Configurer une authentification Windows sur le serveur de rapports](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)  
  
-   [Configurer une authentification de base sur le serveur de rapports](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)  
  
-   [Configurer l'authentification personnalisée ou par formulaire sur le serveur de rapports](../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md)  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Descriptions de tâche|Liens|  
|-----------------------|-----------|  
|Configurez le type d'authentification intégrée de Windows.|[Configurer une authentification Windows sur le serveur de rapports](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)|  
|Configurez le type d'authentification de base.|[Configurer une authentification de base sur le serveur de rapports](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)|  
|Configurez soit l'authentification par formulaire, soit un type d'authentification personnalisé.|[Configurer l'authentification personnalisée ou par formulaire sur le serveur de rapports](../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md)|  
|Pour gérer le scénario d’authentification personnalisé, activez le [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] .|[Configurer le portail Web pour passer des cookies d’authentification personnalisée](configure-the-web-portal-to-pass-custom-authentication-cookies.md)|  

## <a name="next-steps"></a>Étapes suivantes

[Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
[RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Créer et gérer des attributions de rôles](../../reporting-services/security/create-and-manage-role-assignments.md)   
[Spécifier des informations d'identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
[Implémentation d’une extension de sécurité](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)   
[Configurer des connexions TLS sur un serveur de rapports en mode natif](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md)   
[Présentation des extensions de sécurité](../../reporting-services/extensions/security-extension/security-extensions-overview.md)   
[Authentification dans Reporting Services](../../reporting-services/extensions/security-extension/authentication-in-reporting-services.md)   
[Autorisation dans Reporting Services](../../reporting-services/extensions/security-extension/authorization-in-reporting-services.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)