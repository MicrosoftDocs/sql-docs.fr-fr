---
title: Services SCM - Changer le compte de démarrage du service | Microsoft Docs
description: Découvrez comment modifier les comptes de service que SQL Server et un grand nombre de ses services utilisent. Affichez les limitations et les restrictions des modifications sur le compte de service.
ms.custom: ''
ms.date: 01/06/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server services, startup account changes
- startup accounts [SQL Server]
- changing startup accounts for services
ms.assetid: d721c796-0397-46a7-901b-1a9a3c3fb385
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8f5b17129d2fc8115ce976a7acac6dd429c28407
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170991"
---
# <a name="scm-services---change-the-service-startup-account"></a>Services SCM - Changer le compte de démarrage du service
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager pour changer les options de démarrage des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et pour changer les comptes de service qui sont utilisés par [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou de PowerShell. Pour plus d’informations sur la sélection d’un compte de service adéquat, consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
> [!IMPORTANT]  
>  Lorsque vous modifiez le compte de démarrage du service pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)] et l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssDE](../../includes/ssde-md.md)]) doit être redémarré pour que la modification soit prise en compte. Lorsque le service redémarre, toutes les bases de données associées à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne seront pas disponibles tant que le service n'a pas redémarré correctement. Si vous devez modifier le compte de démarrage du service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , veillez à effectuer cette opération au cours des opérations de maintenance planifiées régulièrement ou lorsqu'il est possible de mettre les bases de données hors ligne sans interrompre les opérations quotidiennes.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Serveurs en cluster  
  
     La modification du compte de service utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être effectuée à partir du nœud actif du cluster [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Lors de l'exécution sur Windows Server 2008 (dans une configuration autre que celle par défaut utilisant des groupes de domaines), la modification du compte de service utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nécessite que le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] arrête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mettant les groupes de ressources hors ligne.  
  
-   Mise à niveau de SKU ([!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] vers SKU non-Express)  
  
     Au cours de l'installation de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] , le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent est configuré pour utiliser le compte de service réseau, mais est désactivé. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut modifier le compte assigné pour le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, mais le service ne peut pas être activé ou démarré. Après la mise à niveau d'une référence SKU de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] vers non-Express, le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent n'est pas automatiquement activé, mais il peut l'être lorsque cela est nécessaire en utilisant le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et en modifiant le mode de démarrage du service par Manuel ou Automatique.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
  
#### <a name="to-change-the-sql-server-service-startup-account"></a>Pour modifier le compte de démarrage du service SQL Server  
  
1.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]et sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
    > [!NOTE]  
    >  Étant donné que le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un composant logiciel enfichable pour le programme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console et non pas un programme autonome, le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’apparaît pas en tant qu’application dans les versions plus récentes de Windows.  
    >   
    >  -   **Windows 10**:  
    >          Pour ouvrir le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans la **page d’accueil**, tapez SQLServerManager13.msc (pour [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]). Pour les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , remplacez 13 par un nombre plus petit. Le fait de cliquer sur SQLServerManager13.msc ouvre le Gestionnaire de configuration. Pour épingler le Gestionnaire de configuration à la page d’accueil ou à la barre des tâches, cliquez avec le bouton droit sur SQLServerManager13.msc, puis cliquez sur **Ouvrir l’emplacement du fichier**. Dans l’Explorateur de fichiers Windows, cliquez avec le bouton droit sur SQLServerManager13.msc, puis cliquez sur **Épingler à l’écran d’accueil** ou **Épingler à la barre des tâches**.  
    > -   **Windows 8**:  
    >          Pour ouvrir le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sur l’icône **Rechercher**, sous **Applications**, tapez **SQLServerManager\<version>.msc** (par exemple, **SQLServerManager13.msc**), puis appuyez sur **Entrée**.  
  
2.  Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez sur **Services SQL Server**.  
  
3.  Dans le volet d’informations, cliquez avec le bouton droit sur le nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dont vous souhaitez modifier le compte de démarrage de service, puis cliquez sur **Propriétés**.  
  
4.  Dans la boîte de dialogue **Propriétés de SQL Server \<**_instancename_**>** , cliquez sur l’onglet **Ouvrir une session** et sélectionnez un type de compte **Ouvrir une session en tant que**.  
  
5.  Après avoir sélectionné le nouveau compte de démarrage de service, cliquez sur **OK**.  
  
     Un message vous demande si vous souhaitez redémarrer le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
6.  Cliquez sur **Oui**, puis fermez le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)   
 [Configurer WMI pour afficher l'état du serveur dans les outils SQL Server](../../ssms/configure-wmi-to-show-server-status-in-sql-server-tools.md)  
  
  
