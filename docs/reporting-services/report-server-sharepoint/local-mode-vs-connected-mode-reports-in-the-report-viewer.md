---
title: Rapports en mode local et rapports en mode connecté dans la Visionneuse de rapports| Microsoft Docs
description: Les rapports SQL Server Reporting Services peuvent être configurés pour être exécutés en mode local ou en mode connecté. Apprenez-en davantage sur les différents modes.
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: beeba27a4186aa8ea19401284b8e1f3c4c7f6990
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597206"
---
# <a name="local-mode-vs-connected-mode-reports-in-the-report-viewer"></a>Rapports en mode local et rapports en mode connecté dans la Visionneuse de rapports

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peuvent être configurés pour être exécutés en *mode local* ou en *mode connecté*, qui utilise un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] repou ent server. À la place, vous pouvez utiliser la Visionneuse de rapports pour effectuer le rendu des rapports directement à partir de SharePoint lorsque l'extension de données prend en charge la création de rapports en mode local. Cette approche est appelée *mode local*. Dans les versions précédentes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], la batterie de serveurs SharePoint devait être connectée à un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] configuré en mode SharePoint pour que le contrôle Visionneuse de rapports puisse effectuer le rendu des rapports. Cette approche est appelée *mode distant* ou *mode connecté*.  

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

 En *mode local* , il n'y a aucun serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Vous devez installer le complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour les produits SharePoint. Toutefois, aucun serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] n’est requis. En mode local, les utilisateurs peuvent afficher des rapports mais n'ont pas accès aux fonctionnalités côté serveur telles que les abonnements et les alertes de données.  

## <a name="local-mode-vs-connected-mode-and-supported-extensions"></a>Comparaison entre mode local et mode connecté, et extensions prises en charge

 **Mode local :** quand une extension de données prend en charge le mode local, la Visionneuse de rapports affiche directement les rapports depuis SharePoint. En *mode local* , il n'y a aucun serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Vous devez installer le complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour les produits SharePoint. Toutefois, aucun serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] n’est requis. En mode local, les utilisateurs peuvent afficher des rapports, mais ils **n'ont pas** accès aux fonctionnalités côté serveur, telles que les abonnements et les alertes de données.  
  
 Le **mode connecté**, également appelé *mode distant* nécessite qu'un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] soit en mode SharePoint et soit connecté à une batterie de serveurs SharePoint pour que le contrôle Visionneuse de rapports puisse effectuer le rendu des rapports.  
  
 Voici une liste des extensions pour le traitement des données qui prennent en charge la création de rapports en mode local :  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access 2010. Pour plus d’informations sur les services Access, consultez [Utiliser Access Services avec SQL Reporting Services : installation du complément SQL Server 2008 R2 Reporting Services (SharePoint Server 2010)](/previous-versions/office/sharepoint-server-2010/ee662542(v=office.14)).  
  
-   Extension de données Liste SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations sur l’extension de données Liste SharePoint, consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  
  
 Les extensions pour le traitement des données personnalisées peuvent également être développées pour prendre en charge le mode local. Pour plus d’informations, consultez [Implémentation d’une extension pour le traitement des données](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md).  
  
 Le mode local prend en charge le rendu de rapports ayant une source de données incorporée ou une source de données partagée à partir d'un fichier .rsds. Toutefois, vous ne pouvez pas gérer le rapport ou sa source de données associée. Si vous essayez de le faire, un message d'erreur s'affiche, indiquant que l'opération n'est pas prise en charge en mode local. La gestion des sources de données sur le site SharePoint est prise en charge uniquement en mode connecté.  
  
> [!NOTE]  
>  Comme avec les versions précédentes, vous ne pouvez pas incorporer des noms d'utilisateurs et des mots de passe dans le fichier .rsds.  
  
## <a name="configure-local-mode-and-access-services-with-sharepoint-2013"></a>Configurer le mode local et Access Services avec SharePoint 2013

 Vous pouvez configurer votre batterie de serveurs SharePoint 2013 afin de prendre en charge les bases de données existantes Web Access 2010 et le mode local [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d'informations, consultez [Installer et configurer Access Services 2010 pour les bases de données Web sur le serveur SharePoint 2013](https://technet.microsoft.com/library/ee748653\(office.15\).aspx).  
  
 Il n'est pas possible de créer de nouvelles bases de données Web Access pour SharePoint 2013. Access 2013 utilise un nouveau type de base de données, l' *application Web Access* , que vous créez dans Access, puis que vous utilisez et partagez en tant qu'application SharePoint dans un navigateur Web.  
  
 Pour plus d'informations, consultez les documents suivants.  
  
-   [Nouveautés d’Access 2013](https://office.microsoft.com/access-help/what-s-new-in-access-2013-HA102809500.aspx) (https://office.microsoft.com/access-help/what-s-new-in-access-2013-HA102809500.aspx).  
  
-   [Tâches de base pour une application Access](https://office.microsoft.com/access-help/basic-tasks-for-an-access-app-HA102840210.aspx?CTT=5&origin=HA102809500) (https://office.microsoft.com/access-help/basic-tasks-for-an-access-app-HA102840210.aspx?CTT=5&origin=HA102809500).  
  
## <a name="configure-local-mode-reporting-with-sharepoint-2010"></a>Configurer la création de rapports en mode local avec SharePoint 2010

 Le mode local requiert l'état de session ASP.NET. L'installation des services Access activera l'état de session ASP.NET. Vous pouvez également l'activer à l'aide de PowerShell.  
  
1.  Ouvrez SharePoint 2010 Management Shell.  
  
2.  Tapez la commande suivante :  
  
    ```  
    - Enable-SPSessionStateService  
    ```  
  
3.  Lorsque vous y êtes invité, tapez le nom de votre base de données.  
  
4.  Effectuez une réinitialisation d'IIS.  
  
 Pour plus d’informations, consultez [Utiliser Access Services avec SQL Reporting Services : installation du complément SQL Server 2008 R2 Reporting Services (SharePoint Server 2010)](/previous-versions/office/sharepoint-server-2010/ee662542(v=office.14)) et [Enable-SPSessionStateService](https://technet.microsoft.com/library/ff607857\(v=office.15\).aspx).  
  
## <a name="connected-mode"></a>mode connecté

 Pour obtenir les dernières informations concernant l’utilisation de l’extension ADS avec le mode connecté de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Access Services Report in SharePoint Site shows error in data extension 'ADS'](https://social.technet.microsoft.com/wiki/contents/articles/25298.access-services-report-in-sharepoint-site-shows-error-in-data-extension-ads.aspx).  
  
## <a name="see-also"></a>Voir aussi

 [Sources de données prises en charge par Reporting Services](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)