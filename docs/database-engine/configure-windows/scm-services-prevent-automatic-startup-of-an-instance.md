---
title: Services SCM - Empêcher le démarrage automatique d'une instance | Microsoft Docs
description: Découvrez comment empêcher le démarrage automatique d’une instance de SQL Server. Découvrez comment définir le mode de démarrage manuel pour effectuer cette tâche.
ms.custom: ''
ms.date: 01/06/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- automatic SQL Server startup
- SQL Server, stopping
- SQL Server, automatic startup
- canceling automatic startup
- stopping SQL Server
- preventing automatic startups [SQL Server]
ms.assetid: 782663cf-f3d7-4cc6-b621-21e4550f0322
author: markingmyname
ms.author: maghan
ms.openlocfilehash: deb0dae16f577d3d7df360138582c426146cb8aa
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783528"
---
# <a name="scm-services---prevent-automatic-startup-of-an-instance"></a>Services SCM - Empêcher le démarrage automatique d'une instance
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique décrit comment empêcher une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de démarrer automatiquement dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide du Gestionnaire de configuration SQL Server. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en principe configuré afin de démarrer automatiquement. Vous pouvez modifier ce comportement en définissant le mode de démarrage de l'instance sur Manuel.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
  
#### <a name="to-prevent-automatic-startup-of-an-instance-of-sql-server"></a>Pour empêcher le démarrage automatique d'une instance de SQL Server  
  
1.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]et sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
    > [!NOTE]  
    >  Étant donné que le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un composant logiciel enfichable pour le programme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console et non pas un programme autonome, le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’apparaît pas en tant qu’application dans les versions plus récentes de Windows.  
    >   
    >  -   **Windows 10**:  
    >          Pour ouvrir le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans la **page d’accueil**, tapez SQLServerManager13.msc (pour [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]). Pour les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , remplacez 13 par un nombre plus petit. Le fait de cliquer sur SQLServerManager13.msc ouvre le Gestionnaire de configuration. Pour épingler le Gestionnaire de configuration à la page d’accueil ou à la barre des tâches, cliquez avec le bouton droit sur SQLServerManager13.msc, puis cliquez sur **Ouvrir l’emplacement du fichier**. Dans l’Explorateur de fichiers Windows, cliquez avec le bouton droit sur SQLServerManager13.msc, puis cliquez sur **Épingler à l’écran d’accueil** ou **Épingler à la barre des tâches**.  
    > -   **Windows 8**:  
    >          Pour ouvrir le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sur l’icône **Rechercher**, sous **Applications**, tapez **SQLServerManager\<version>.msc** (par exemple, **SQLServerManager13.msc**), puis appuyez sur **Entrée**.  
  
2.  Dans le Gestionnaire de configuration SQL Server, développez **Services**, puis cliquez sur **SQL Server**.  
  
3.  Dans le volet d’informations, cliquez avec le bouton droit sur **MSSQLServer**, puis cliquez sur **Propriétés**.  
  
4.  Dans la boîte de dialogue **Propriétés de SQL Server \<**_instancename_**>** , sous l’onglet **Service**, dans la zone **Général**, définissez la valeur de **Mode de démarrage** sur **Manuel**.  
  
5.  Cliquez sur **OK** pour fermer la boîte de dialogue **Propriétés de SQL Server \<**_instancename_**>** , puis fermez le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
  
