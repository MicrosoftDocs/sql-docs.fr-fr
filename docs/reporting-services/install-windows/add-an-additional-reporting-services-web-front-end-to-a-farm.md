---
description: Ajouter un serveur Web frontal Reporting Services supplémentaire à une batterie
title: Ajouter un serveur web frontal Reporting Services supplémentaire à une batterie de serveurs | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
ms.assetid: d7a11bda-ae26-49ac-b071-37d83cae5afe
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 50bee52344ee56d83e1dc86cf9d4f8208c4c8c68
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061114"
---
# <a name="add-an-additional-reporting-services-web-front-end-to-a-farm"></a>Ajouter un serveur Web frontal Reporting Services supplémentaire à une batterie
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Le mode SharePoint inclut les composants nécessaires pour les serveurs d’applications et les serveurs web frontaux. Cette rubrique traite de l'installation des composants [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] requis pour un serveur Web frontal, y compris les pages d'application utilisées par les fonctionnalités [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] telles que les abonnements, les alertes de données et [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]. L’installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] principale nécessaire pour un serveur web frontal consiste à installer le complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour les produits SharePoint 2016.  
  
## <a name="prerequisites"></a>Prérequis  
  
-   Vous devez être administrateur local pour exécuter le programme d'installation de SQL Server.  
  
-   L'ordinateur doit être attaché à un domaine.  
  
-   Vous devez connaître le nom du serveur de base de données existant qui héberge la configuration et les bases de données de contenu SharePoint.  
  
-   Le serveur de base de données doit être configuré pour autoriser les connexions de base de données distantes.  S'il ne l'est pas, vous ne pourrez pas joindre le nouveau serveur à la batterie car il ne pourra pas établir de connexion aux bases de données de configuration SharePoint.  
  
-   Le nouveau serveur devra avoir la même version de SharePoint installée que celle exécutée par les serveurs de batterie actuels. Par exemple, si SharePoint 2013 Service Pack 1 (SP1) est déjà installé sur la batterie de serveurs, vous devez installer aussi le SP1 sur le nouveau serveur pour qu’il puisse rejoindre la batterie de serveurs.  
  
## <a name="steps"></a>Étapes  
 Les étapes de cette rubrique partent du principe qu'un administrateur de batterie de serveurs SharePoint installe et configure le serveur. Le diagramme illustre un environnement à trois niveaux classique et les éléments numérotés sont décrits dans la liste suivante :  
  
-   (1) Plusieurs serveurs Web frontaux. Les serveurs Web frontaux ont besoin du complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour SharePoint 2010. Les étapes suivantes ajoutent un deuxième serveur d'applications à cette couche.  
  
-   (2) Deux serveurs d'applications exécutant [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et des sites Web, par exemple Administration centrale.  
  
-   (3) Deux serveurs de base de données SQL Server.  
  
-   (4) Représente une solution d'équilibrage de la charge réseau matérielle ou logicielle  
  
 ![Ajouter SSRS à un nouveau WFE SharePoint](../../reporting-services/install-windows/media/rs-sharepointscale-wfe.gif "Ajouter SSRS à un nouveau WFE SharePoint")  
  
 Les étapes suivantes impliquent qu'un administrateur procède à l'installation et à la configuration du serveur.  
  
|Étape|Description et lien|  
|----------|--------------------------|  
|Ajoutez un serveur SharePoint à une batterie de serveurs.|Vous devez installer SharePoint pour déployer une autre application Reporting Services.<br/><br/>Pour SharePoint 2013, consultez [Ajouter un serveur SharePoint à une batterie de serveurs dans SharePoint Server 2013](/SharePoint/install/add-web-or-application-server-to-the-farm).<br/><br/>Pour SharePoint 2016, consultez [Ajouter un serveur SharePoint à une batterie de serveurs dans SharePoint Server 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm).|  
|Installer le complément SQL Server Reporting Services pour les produits SharePoint 2016|Il existe plusieurs méthodes pour installer le complément. Les étapes suivantes utilisent l’Assistant Installation de SQL Server. Pour plus d’informations sur l’installation du complément, consultez [Installer ou désinstaller le complément Reporting Services pour SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md).<br /><br /> 1) Lancez l’installation de SQL Server.<br /><br /> 2) Dans la page **Rôle d’installation** , sélectionnez **Installation de fonctionnalités SQL Server**.<br /><br /> 3) Dans la page **Sélection de fonctionnalités** , sélectionnez **Complément Reporting Services pour les produits SharePoint**.<br /><br /> 4) Cliquez sur **Suivant** dans les diverses pages suivantes pour compléter les options d’installation.<br /><br/>Pour plus d’informations sur l’installation de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Installer le premier serveur de rapports en mode SharePoint](install-the-first-report-server-in-sharepoint-mode.md)|  
|Vérifiez que le nouveau serveur est opérationnel.|1) Dans l’Administration centrale de SharePoint, cliquez sur **Gérer les serveurs de cette batterie** dans le groupe **Paramètres système** .<br /><br /> 2) Vérifiez que le nouveau serveur est dans la liste.|  
|Mettez à jour votre solution d'équilibrage de la charge réseau.|Si nécessaire, mettez à jour votre environnement matériel ou logiciel d'équilibrage de la charge réseau pour inclure le nouveau serveur.|  

## <a name="next-steps"></a>Étapes suivantes

[Ajouter un serveur SharePoint à une batterie de serveurs dans SharePoint Server 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm)  
[Ajouter un serveur SharePoint à une batterie de serveurs dans SharePoint Server 2013](/SharePoint/install/add-web-or-application-server-to-the-farm)

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)