---
description: Ajouter un serveur de rapports supplémentaire à une batterie (montée en puissance SSRS)
title: Ajouter un serveur de rapports supplémentaire à un pool (montée en puissance SSRS) | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
ms.assetid: c1a6b683-15cf-44ae-ac60-ceee63a60aaf
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: f30aabd3ca217465ce29ca9a4f73a2fb030e155a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067944"
---
# <a name="add-an-additional-report-server-to-a-farm-ssrs-scale-out"></a>Ajouter un serveur de rapports supplémentaire à une batterie (montée en puissance SSRS)

  L'ajout d'un second serveur de rapports en mode SharePoint, ou plus, à votre batterie de serveurs SharePoint peut améliorer les performances et le temps de réponse du traitement du serveur de rapports. Si vous avez trouvé que les performances ralentissaient alors que vous ajoutiez des utilisateurs, des rapports et autres applications au serveur de rapports, l'ajout de serveurs de rapports peut améliorer les performances. Il est également recommandé d'ajouter un second serveur de rapports pour augmenter la disponibilité des serveurs de rapports lorsqu'il existe des problèmes liés au matériel ou lorsque vous effectuez la maintenance générale sur des serveurs individuels de votre environnement. À compter de la version [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , les étapes de la montée en puissance parallèle d'un environnement [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode SharePoint suivent le déploiement standard d'une batterie de serveurs SharePoint et exploitent les fonctionnalités d'équilibrage de charge SharePoint.  
  
> [!IMPORTANT]  
>  Le déploiement avec montée en puissance parallèle de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] n'est pas pris en charge par toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez la section [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de l’article [Fonctionnalités prises en charge par les éditions de SQL Server](~/sql-server/editions-and-components-of-sql-server-2017.md#SSRS).  
  
> [!TIP]  
>  À compter de la version [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], vous n'utilisez pas le Gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour ajouter des serveurs et effectuer un scale-out des serveurs de rapports. Les produits SharePoint gèrent le déploiement avec montée en puissance parallèle de Reporting Services sous forme de serveurs SharePoint avec l'ajout du service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à la batterie de serveurs.  
  
 Pour plus d’informations sur la façon d’effectuer un scale-out de serveurs de rapports en mode natif, consultez [Configurer un déploiement par scale-out de serveurs de rapports en mode natif &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
##  <a name="load-balancing"></a><a name="bkmk_loadbalancing"></a> Équilibrage de charge  
 L'équilibrage de la charge des applications de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sera géré automatiquement par SharePoint à moins que votre environnement n'ait une solution d'équilibrage de charge tierce ou personnalisée. Le comportement d'équilibrage de la charge SharePoint par défaut fait que chaque application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sera équilibrée à travers tous les serveurs d'applications sur lesquels vous avez démarré le service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour vérifier si le service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est installé et démarré, cliquez sur **Gérer les services sur le serveur** dans l'Administration centrale de SharePoint.  
  
##  <a name="prerequisites"></a><a name="bkmk_prerequisites"></a> Conditions préalables  
  
-   Vous devez être administrateur local pour exécuter le programme d'installation de SQL Server.  
  
-   L'ordinateur doit être attaché à un domaine.  
  
-   Vous devez connaître le nom du serveur de base de données existant qui héberge la configuration et les bases de données de contenu SharePoint.  
  
-   Le serveur de base de données doit être configuré pour autoriser les connexions de base de données distantes.  S'il ne l'est pas, vous ne pourrez pas joindre le nouveau serveur à la batterie car il ne pourra pas établir de connexion aux bases de données de configuration SharePoint.  
  
-   Le nouveau serveur devra avoir la même version de SharePoint installée que celle exécutée par les serveurs de batterie actuels. Par exemple, si SharePoint 2013 Service Pack 1 (SP1) est déjà installé sur la batterie de serveurs, vous devez installer aussi le SP1 sur le nouveau serveur pour qu’il puisse rejoindre la batterie de serveurs.  
  
##  <a name="steps"></a><a name="bkmk_steps"></a> Étapes  
 Les étapes de cette rubrique partent du principe qu'un administrateur de batterie de serveurs SharePoint installe et configure le serveur. Le diagramme illustre un environnement à trois niveaux classique et les éléments numérotés sont décrits dans la liste suivante :  
  
-   (1) Plusieurs serveurs Web frontaux. Les serveurs Web frontaux ont besoin du complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour SharePoint 2016.  
  
-   (2) Un seul serveur d'applications exécutant [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et des sites Web, par exemple Administration centrale. Les étapes suivantes ajoutent un deuxième serveur d'applications à cette couche.  
  
-   (3) Deux serveurs de base de données SQL Server.  
  
-   (4) Représente une solution d'équilibrage de la charge réseau matérielle ou logicielle  
  
 ![Ajout d'un serveur d'applications Reporting Services](../../reporting-services/install-windows/media/rs-sharepointscale.gif "Ajout d'un serveur d'applications Reporting Services")  
  
 Les étapes suivantes impliquent qu'un administrateur procède à l'installation et à la configuration du serveur. Le serveur sera installé en tant que nouveau serveur d'applications dans la batterie et ne sera pas utilisé comme serveur Web frontal.  
  
|Étape|Description et lien|  
|----------|--------------------------|  
|Ajoutez un serveur SharePoint à une batterie de serveurs.|Vous devez installer SharePoint pour déployer une autre application Reporting Services.<br/><br/>Pour SharePoint 2013, consultez [Ajouter un serveur SharePoint à une batterie de serveurs dans SharePoint Server 2013](/SharePoint/install/add-web-or-application-server-to-the-farm).<br/><br/>Pour SharePoint 2016, consultez [Ajouter un serveur SharePoint à une batterie de serveurs dans SharePoint Server 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm).|  
|Installez et configurez le mode SharePoint de Reporting Services.|Lancez l’installation de SQL Server. Pour plus d’informations sur l’installation du mode SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Installer le premier serveur de rapports en mode SharePoint](install-the-first-report-server-in-sharepoint-mode.md).<br /><br /> Si le serveur ne doit être utilisé qu’en tant que serveur d’applications et non en tant que serveur Web frontal, vous n’avez pas besoin de sélectionner **Complément Reporting Services pour les produits SharePoint**.<br /><br /> 1) Dans la page **Rôle d’installation** , sélectionnez **Installation de fonctionnalités SQL Server**.<br /><br /> 2) Dans la page **Sélection des fonctionnalités** , sélectionnez **Reporting Services - SharePoint**.<br /><br /> 3) Dans la page **Configuration de Reporting Services**  , vérifiez que l’option **Installer uniquement** est sélectionnée pour **Mode SharePoint de Reporting Services**.|  
|Vérifiez que Reporting Services est opérationnel.|1) Dans l’Administration centrale de SharePoint, cliquez sur **Gérer les serveurs de cette batterie** dans le groupe **Paramètres système** .<br /><br /> 2) Vérifiez le service **SQL Server Reporting Services**.<br /><br />Pour plus d'informations, consultez [Verify a Reporting Services Installation](../../reporting-services/install-windows/verify-a-reporting-services-installation.md)|  
  
##  <a name="additional-configuration"></a><a name="bkmk_additional"></a> Configuration supplémentaire  
 Vous pouvez optimiser des serveurs [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] individuels dans un déploiement avec montée en puissance parallèle afin d'effectuer un traitement en arrière-plan uniquement, de sorte que ces serveurs ne soient pas en concurrence en matière d'utilisation des ressources pour l'exécution de rapports interactifs. Le traitement en arrière-plan comprend les planifications, les abonnements et les alertes de données.  
  
 Pour changer le comportement de serveurs de rapports individuels, affectez la valeur false à **\<IsWebServiceEnable>** dans le fichier de configuration **RSreportServer.config**.  
  
 Par défaut, les serveurs de rapports sont configurés avec \<IsWebServiceEnable>défini à TRUE. Lorsque tous les serveurs sont configurés avec la valeur TRUE, les charges de traitement interactif et de traitement en arrière-plan sont équilibrées sur tous les nœuds de la batterie de serveurs.  
  
 Si vous configurez tous les serveurs de rapports avec \<IsWebServiceEnable> défini à False, un message d’erreur semblable à ce qui suit s’affiche quand vous essayez d’utiliser les fonctionnalités [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
```output
The Reporting Services Web Service is not enabled. Configure at least one instance of the Reporting Services SharePoint Service to have <IsWebServiceEnable> set to true.
```
 
 Pour plus d’informations, consultez [Modifier un fichier de configuration &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  

## <a name="next-steps"></a>Étapes suivantes

[Ajouter un serveur SharePoint à une batterie de serveurs dans SharePoint Server 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm)  
[Ajouter un serveur SharePoint à une batterie de serveurs dans SharePoint Server 2013](/SharePoint/install/add-web-or-application-server-to-the-farm)

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)