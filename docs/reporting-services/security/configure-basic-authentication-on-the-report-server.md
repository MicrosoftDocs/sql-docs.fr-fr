---
description: Configurer l’authentification de base sur le serveur de rapports
title: Configurer une authentification de base sur le serveur de rapports | Microsoft Docs
ms.date: 02/10/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, configuration
- Basic authentication
ms.assetid: 8faf2938-b71b-4e61-a172-46da2209ff55
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d3b875eafb10e4234df2cde35bb61bbecfebad94
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489323"
---
# <a name="configure-basic-authentication-on-the-report-server"></a>Configurer l’authentification de base sur le serveur de rapports
  Par défaut, Reporting Services accepte les demandes qui spécifient l'authentification Negotiate et NTLM. Si votre déploiement inclut des applications clientes ou des navigateurs qui utilisent l'authentification de base, vous devez l'ajouter à la liste des types pris en charge. De plus, si vous voulez utiliser le Générateur de rapports, vous devez activer l'accès anonyme aux fichiers Générateur de rapports.  
  
 Pour configurer l'authentification de base sur le serveur de rapports, modifiez les éléments et les valeurs XML dans le fichier RSReportServer.config. Vous pouvez copier et coller les exemples de cette rubrique pour remplacer les valeurs par défaut.  
  
 Avant d'activer l'authentification de base, vérifiez que votre infrastructure de sécurité la prend en charge. Sous l'authentification de base, le service Web Report Server passe des informations d'identification à l'autorité de sécurité locale. Si ces informations spécifient un compte d'utilisateur local, l'utilisateur est authentifié par l'autorité de sécurité locale sur le serveur de rapports et l'utilisateur obtient un jeton de sécurité qui est valide pour les ressources locales. Les informations d'identification pour les comptes d'utilisateur de domaine sont transférées et sont authentifiées par un contrôleur de domaine. Le ticket résultant est valide pour les ressources réseau.  
  
 Le chiffrement du canal, tel que TLS (Transport Layer Security), précédemment appelé SSL (Secure Sockets Layer), est requis si vous souhaitez réduire le risque d'interception des informations d'identification lors de leur transit vers un contrôleur de domaine sur votre réseau. L'authentification de base transmet en texte en clair le nom d'utilisateur et le mot de passe en encodage base 64. L'ajout du chiffrement de canal rend le paquet illisible. Pour plus d’informations, consultez [Configurer des connexions TLS sur un serveur de rapports en mode natif](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
 Après avoir activé l'authentification de base, n'oubliez pas que les utilisateurs ne peuvent pas sélectionner l'option **Sécurité intégrée de Windows** lors de la définition de propriétés de connexion sur une source de données externe qui fournit des données à un rapport. L'option est grisée dans les pages de propriétés de source de données.  
  
> [!NOTE]  
>  Les instructions suivantes sont destinées à un serveur de rapports en mode natif. Si le serveur de rapports est déployé en mode intégré SharePoint, vous devez utiliser les paramètres d'authentification par défaut qui spécifient la sécurité intégrée de Windows. Le serveur de rapports utilise des fonctionnalités internes dans l'extension d'authentification Windows par défaut pour prendre en charge le serveur de rapports en mode intégré SharePoint.  
  
### <a name="to-configure-a-report-server-to-use-basic-authentication"></a>Pour configurer un serveur de rapports pour utiliser l'authentification de base  
  
1. Ouvrez RSReportServer.config dans un éditeur de texte.  
  
     Pour trouver le fichier config, consultez la section [Emplacement du fichier](../report-server/rsreportserver-config-configuration-file.md#bkmk_file_location) dans l’article « Fichier de configuration RsReportServer.config ».
  
2. Recherchez \<**Authentication**>.  
  
3. Copiez, parmi les structures XML suivantes, celle qui répond le mieux à vos besoins. La première structure XML fournit des espaces réservés pour spécifier tous les éléments décrits dans la section suivante :  

    [!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)]

    ```xml
    <Authentication>  
          <AuthenticationTypes>  
                 <RSWindowsBasic>  
                       <LogonMethod>3</LogonMethod>  
                       <Realm></Realm>  
                       <DefaultDomain></DefaultDomain>  
                 </RSWindowsBasic>  
          </AuthenticationTypes>  
          <EnableAuthPersistence>true</EnableAuthPersistence>  
    </Authentication>  
    ```  
  
    Si vous utilisez des valeurs par défaut, vous pouvez copier la structure de l'élément minimale :  
  
    ```xml
          <AuthenticationTypes>  
                 <RSWindowsBasic/>  
          </AuthenticationTypes>  
    ```  

    [!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)] [!INCLUDE [ssrs-appliesto-pbirs](../../includes/ssrs-appliesto-pbirs.md)]

    ```xml
      <Authentication>
          <AuthenticationTypes>
                      <RSWindowsBasic/>
          </AuthenticationTypes>
          <EnableAuthPersistence>true</EnableAuthPersistence>
      <RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>
      <RSWindowsExtendedProtectionScenario>Any</RSWindowsExtendedProtectionScenario>
      </Authentication>
    ```

4. Collez-la sur les entrées existantes de \<**Authentication**>.  
  
     Si vous utilisez plusieurs types d'authentification, ajoutez juste l'élément **RSWindowsBasic** mais ne supprimez pas les entrées pour **RSWindowsNegotiate**, **RSWindowsNTLM** ou **RSWindowsKerberos**.  
  
     Notez que vous ne pouvez pas utiliser **Custom** avec d'autres types d'authentification.  
  
5. Remplacez des valeurs vides pour \<**Realm**> ou\<**DefaultDomain**> par des valeurs valides pour votre environnement.  
  
6. Enregistrez le fichier .  
  
7. Si vous avez configuré un déploiement avec montée en puissance parallèle, répétez ces étapes pour d'autres serveurs de rapports du déploiement.  
  
8. Redémarrez le serveur de rapports pour effacer toutes les sessions qui sont actuellement ouvertes.  
  
## <a name="rswindowsbasic-reference"></a>Référence RSWindowsBasic  
 Les éléments suivants peuvent être spécifiés lors de la configuration de l'authentification de base.  
  
|Élément|Obligatoire|Valeurs valides|  
|-------------|--------------|------------------|  
|LogonMethod|Oui<br /><br /> Si vous ne spécifiez pas de valeur, 3 est utilisé.|**2** = ouverture de session réseau, destinée aux serveurs hautes performances pour l’authentification des mots de passe en texte brut.<br /><br /> **3** = ouverture de session basée sur du texte en clair ; les informations d’identification d’ouverture de session sont conservées dans le package d’authentification envoyé avec chaque requête HTTP, ce qui permet au serveur d’emprunter l’identité de l’utilisateur quand il s’agit de se connecter aux autres serveurs du réseau. (Par défaut)<br /><br /> Remarque : Les valeurs 0 (pour l'ouverture de session interactive) et 1 (pour l'ouverture de session par lot) ne sont **PAS** prises en charge dans [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)].|  
|Realm|Facultatif|Spécifie une partition de ressource qui inclut les fonctionnalités d'autorisation et d'authentification permettant de contrôler l'accès aux ressources protégées de votre organisation.|  
|DefaultDomain|Facultatif|Spécifie le domaine utilisé par le serveur pour authentifier l'utilisateur. Cette valeur est facultative, mais si vous l'omettez, le serveur de rapports utilise le nom d'ordinateur comme domaine. Si l'ordinateur est membre du domaine, ce domaine est le domaine par défaut. Si vous avez installé le serveur de rapports sur un contrôleur de domaine, le domaine utilisé est celui contrôlé par l'ordinateur.|  
  
## <a name="see-also"></a>Voir aussi  
 [Domaines d'application des applications du serveur de rapports](../../reporting-services/report-server/application-domains-for-report-server-applications.md)   
 [Sécurité et protection de Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md)  
  
  
