---
title: Créer une application Web Master Data Manager
description: L’application Web Data Manager principale fournit une interface permettant aux utilisateurs de travailler avec des données de référence et aux administrateurs de configurer et d’administrer MDS.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 241d46d7-8008-47f6-bebd-0dfff1cc856a
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: df266fd09cfdd586d5f2fc438791104bf7097938
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344822"
---
# <a name="create-a-master-data-manager-web-application-master-data-services"></a>Créer une application Web Master Data Manager (Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  L’application web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] offre une interface permettant aux utilisateurs de travailler avec des données de référence et aux administrateurs de configurer et de gérer MDS.  
  
 Une application Web doit toujours être contenue par un site Web. Pour créer une application Web, vous devez au choix :  
  
-   Utiliser le site Web par défaut puis créer l'application Web,  
  
-   Utiliser un site Web existant puis créer l'application Web, ou  
  
-   Créer un site web, qui crée automatiquement une application Web.  
  
 Après avoir créé l'application Web, vous l'associez à la base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .  
  
## <a name="prerequisites"></a>Prérequis  
  
-   Pour plus d’informations sur la configuration requise pour l’ordinateur qui héberge l’application web, consultez [Configuration requise pour l’application Web &#40;Master Data Services&#41;](../../master-data-services/install-windows/web-application-requirements-master-data-services.md).  
  
## <a name="to-create-a-master-data-manager-web-application-in-a-new-website"></a>Pour créer une application Web Master Data Manager dans un nouveau site Web  
 Quand vous créez un site web, l’application web racine est l’application web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] . L'application Web est également ajoutée à un nouveau pool d'applications.  
  
> [!NOTE]  
>  Si vous appliquez cette procédure, vous ne pouvez pas spécifier de chemin virtuel ni d’alias de l’application web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] . Si vous souhaitez spécifier un chemin virtuel et un alias pour [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], vous devez créer une application web dans un site web existant qui n’est pas encore configuré comme application web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] .  
  
 En outre, [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] prend uniquement en charge la création de sites avec des liaisons HTTP. Pour ajouter une liaison HTTPS, créez un site et une application dans [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] , puis consultez [Sécuriser une application Web Master Data Manager](../../master-data-services/install-windows/secure-a-master-data-manager-web-application.md) pour plus d’informations.  
  
#### <a name="to-create-a-master-data-manager-web-application-in-a-new-website"></a>Pour créer une application Web Master Data Manager dans un nouveau site Web  
  
1.  Ouvrez [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
2.  Dans le volet gauche, cliquez sur **Configuration Web**.  
  
3.  Dans la page **Configuration Web** , dans la liste des site web, sélectionnez **Créer un site Web**.  
  
4.  Dans la boîte de dialogue **Créer un site Web** , spécifiez les informations pour un nouveau site web. Pour plus d’informations sur les options d’interface utilisateur dans la boîte de dialogue, consultez [Boîte de dialogue Créer un site Web &#40;Gestionnaire de configuration Master Data Services&#41;](../../master-data-services/create-website-dialog-box-master-data-services-configuration-manager.md).  
  
5.  Cliquez sur **OK**.  
  
## <a name="to-create-a-master-data-manager-web-application-in-an-existing-website"></a>Pour créer une application Web Master Data Manager dans un site Web existant  
 Lorsque vous créez une application Web dans un site Web existant, vous pouvez choisir son chemin d'accès virtuel et son alias. L'application Web est ajoutée à un nouveau pool d'applications.  
  
#### <a name="to-create-a-master-data-manager-web-application-in-an-existing-website"></a>Pour créer une application Web Master Data Manager dans un site Web existant  
  
1.  Ouvrez [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
2.  Dans le volet gauche, cliquez sur **Configuration Web**.  
  
3.  Dans la page **Configuration Web** , dans la liste **Site Web** , sélectionnez le site web dans lequel vous souhaitez créer l’application web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] .  
  
4.  Cliquez sur **Créer une application**.  
  
5.  Dans la boîte de dialogue **Créer une application Web** , spécifiez les informations pour une nouvelle application web. Pour plus d’informations sur les options d’interface utilisateur dans la boîte de dialogue, consultez [Boîte de dialogue Créer une application Web &#40;Gestionnaire de configuration Master Data Services&#41;](../../master-data-services/create-web-application-dialog-box-master-data-services-configuration-manager.md).  
  
6.  Cliquez sur **OK**.  
  
## <a name="next-steps"></a>Étapes suivantes  
  
-   Associez l'application Web à une base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . Pour plus d’informations, consultez [Associer une base de données Master Data Services et une application Web](../../master-data-services/install-windows/associate-a-master-data-services-database-and-web-application.md).  
  
-   Éventuellement, configurez le site Web qui héberge l' [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] application Web pour utiliser une liaison HTTPS si vous souhaitez chiffrer le contenu à l’aide du protocole TLS (Transport Layer Security), précédemment appelé SSL (Secure Sockets Layer) (SSL). Vous devez utiliser un outil Internet Information Services (IIS), tel que le gestionnaire des services Internet, pour configurer le certificat de serveur pour le serveur Web, et pour configurer une liaison HTTPs et les paramètres TLS pour le site. Pour plus d’informations, consultez [Sécuriser une application Web Master Data Manager](../../master-data-services/install-windows/secure-a-master-data-manager-web-application.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Installer Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
  
