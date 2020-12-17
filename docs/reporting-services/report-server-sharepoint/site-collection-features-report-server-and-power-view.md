---
title: Activer les fonctionnalités d’intégration du serveur de rapports et de Power View dans SharePoint | Microsoft Docs
description: Les fonctionnalités du complément SQL Server Reporting Services pour SharePoint sont généralement activées automatiquement. Utilisez ces instructions si vous devez les activer manuellement.
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 2f79af39e2ba871a0ee4acf9f33c23f54f2bfd3b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461460"
---
# <a name="activate-the-report-server-and-power-view-integration-features-in-sharepoint"></a>Activer les fonctionnalités d’intégration du serveur de rapports et de Power View dans SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  Les fonctionnalités de collection de sites de Reporting Services sont généralement activées par défaut après l’installation du complément [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] pour les produits SharePoint. Dans certaines circonstances, vous devrez activer ces fonctionnalités manuellement.  

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

 Si vous installez le complément Reporting Services pour les produits SharePoint 2010 après l’installation du produit SharePoint, la fonctionnalité d’intégration du serveur de rapports et la fonctionnalité d’intégration de Power View sont uniquement activées pour les collections de sites racines. Pour les autres collections de sites, vous devez activer manuellement ces fonctionnalités. Par exemple, si vous avez une collection de sites **https://[nom de mon serveur]/sites/[nom de collection de sites]**, vous devez activer manuellement les fonctionnalités de collection de sites de Reporting Services.  
  
 S’il n’y a pas de collection de sites racine, le complément Reporting Services journalise un message semblable au suivant.  
  
 « L'application Web SharePoint 80 n'a pas de collection de sites racine »  
  
 Ce message se trouve dans le journal d’installation du complément, nommé « RS_SP_#.log », où # est un nombre incrémentiel. Le fichier journal se trouve dans le dossier Temp de l’utilisateur actuel, par exemple C:\Users\\[nom utilisateur]\AppData\Local\Temp. Pour plus d’informations sur les options de journalisation du complément, consultez [Installer ou désinstaller le complément Reporting Services pour SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md).  

## <a name="activate-the-report-server-and-power-view-integration-site-collection-features"></a>Activer les fonctionnalités de collection de sites d’intégration du serveur de rapports et d’intégration de Power View
  
1.  Ouvrez votre navigateur sur le site où vous souhaitez activer les fonctionnalités de Reporting Services.  
  
2.  Cliquez sur **Actions du site**.  
  
3.  Cliquez sur **Paramètres du site**.  
  
4.  Cliquez sur **Fonctionnalités de la collection de sites** dans le groupe Administration de la collection de sites.  
  
5.  Recherchez **Fonctionnalité d'intégration Report Server** ou **Fonctionnalité d'intégration de Power View** dans la liste.  
  
6.  Cliquez sur **Activer**.  
  
 Pour désactiver les fonctionnalités, vous pouvez utiliser la même procédure en cliquant sur **Désactiver** au lieu d' **Activer**.  
  
## <a name="activate-or-deactivate-reporting-services-central-administration-site-collection-feature"></a>Activer ou désactiver la fonctionnalité de collection de sites d’administration centrale Reporting Services
  
1.  Ouvrez votre navigateur dans l'Administration centrale de SharePoint.  
  
2.  Cliquez sur **Actions du site**.  
  
3.  Cliquez sur **Paramètres du site**.  
  
4.  Cliquez sur **Fonctionnalités de la collection de sites** dans le groupe Administration de la collection de sites.  
  
5.  Recherchez **Fonctionnalité d'administration centrale du serveur de rapports** dans la liste.  
  
6.  Cliquez sur **Activer**.  
  
 Pour désactiver la fonctionnalité, vous pouvez utiliser la même procédure en cliquant sur **Désactiver** au lieu d' **Activer**.  
  
## <a name="next-steps"></a>Étapes suivantes

Une fois la fonctionnalité activée, vous pouvez continuer l'intégration de serveur.

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
