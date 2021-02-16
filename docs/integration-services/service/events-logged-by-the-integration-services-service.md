---
description: Événements consignés par le service Integration Services
title: Événements consignés par le service Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- service [Integration Services], events
- events [Integration Services], service
- Integration Services service, events
ms.assetid: d4122dcf-f16f-47a0-93a2-ffa3d0d4f9cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 45612ea0bfdbda2b27e6cac8b4deb1e302eb9298
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346549"
---
# <a name="events-logged-by-the-integration-services-service"></a>Événements consignés par le service Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] consigne différents messages dans le journal des événements des applications Windows. Le service enregistre ces messages lorsque le service démarre, lorsqu'il s'arrête et lorsque certains problèmes se produisent.  
  
 Cette rubrique fournit des informations sur les messages d'événements courants que le service enregistre dans le journal des événements des applications. Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] consigne tous les messages décrits dans cette rubrique avec SQLISService comme source d’événement.  
  
 Pour des informations générales sur le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], consultez [Service Integration Services &#40;Service SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).  
  
## <a name="service-status-messages"></a>Messages d’état du service
 Quand vous sélectionnez [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour l’installation, le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est installé et démarré, et son type de démarrage est défini sur Automatique.  
  
|ID de l’événement|Nom symbolique|Texte|Notes|  
|--------------|-------------------|----------|-----------|  
|256|DTS_MSG_SERVER_STARTING|Démarrage du service [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)].|Le service est sur le point de démarrer.|  
|257|DTS_MSG_SERVER_STARTED|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] Service démarré.|Le service a démarré.|  
|260|DTS_MSG_SERVER_START_FAILED|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] Échec du démarrage du service.%nErreur : %1|Le service n'a pas pu démarrer. Cette inaptitude à démarrer peut résulter d'une installation endommagée ou d'un compte de service inapproprié.|  
|258|DTS_MSG_SERVER_STOPPING|Arrêt du service [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)].% n%nArrêtez tous les packages en cours d’exécution à la fin de l’opération : %1|Le service est en cours d'arrêt et si vous configurez le service pour cela, tous les packages en cours d'exécution seront arrêtés. Vous pouvez définir une valeur vraie ou fausse dans le fichier de configuration qui détermine si le service doit cesser d'exécuter des packages lorsque le service lui-même s'arrête. Le message pour cet événement inclut la valeur de ce paramètre.|  
|259|DTS_MSG_SERVER_STOPPED|Service [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] arrêté.%nVersion du serveur %1|Le service s'est arrêté.|  
  
## <a name="settings-file-messages"></a>Messages du fichier de paramètres  
 Les paramètres pour le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sont stockés dans un fichier XML que vous pouvez modifier. Pour plus d’informations, consultez [Service Integration Services &#40;Service SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).  
  
|ID de l’événement|Nom symbolique|Texte|Notes|  
|--------------|-------------------|----------|-----------|  
|274|DTS_MSG_SERVER_MISSING_CONFIG_REG|Service [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] : %nparamètre de Registre spécifiant que le fichier de configuration n’existe pas. %nTentative de chargement du fichier de configuration par défaut en cours.|L'entrée du Registre qui contient le chemin d'accès du fichier de configuration n'existe pas ou est vide.|  
|272|DTS_MSG_SERVER_MISSING_CONFIG|Le fichier de configuration du service [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] n’existe pas.%nChargement avec les paramètres par défaut.|Le fichier de configuration lui-même n'existe pas à l'emplacement spécifié.|  
|273|DTS_MSG_SERVER_BAD_CONFIG|Le fichier de configuration du service [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] est incorrect.%nErreur lors de la lecture du fichier de configuration : %1%n%nChargement du serveur avec les paramètres par défaut.|Le fichier de configuration n'a pas pu être lu ou n'est pas valide. Cette erreur peut résulter d'une erreur de syntaxe XML dans le fichier.|  
  
## <a name="other-messages"></a>Autres messages  
  
|ID de l’événement|Nom symbolique|Texte|Notes|  
|--------------|-------------------|----------|-----------|  
|336|DTS_MSG_SERVER_STOPPING_PACKAGE|Service [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] : arrêt de l’exécution du package.%nID d’instance du package : %1%nID du package : %2%nNom : %3%nDescription : %4%nPackage|Le service essaie d'arrêter un package en cours d'exécution. Vous pouvez analyser et arrêter des packages en cours d'exécution dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Pour plus d’informations sur la gestion des packages dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], consultez [Gestion de packages &#40;Service SSIS&#41;](../../integration-services/service/package-management-ssis-service.md).|  

## <a name="view-events"></a>Afficher les événements
  Il existe deux outils dans lesquels vous pouvez afficher les événements du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] :  
  
-   La boîte de dialogue **Visionneuse du fichier journal** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. La boîte de dialogue **Visionneuse du fichier Journal** comprend des options permettant d’exporter et de filtrer le journal et d’effectuer des recherches. Pour plus d’informations sur les options de la **Visionneuse du fichier Journal**, consultez [Visionneuse du fichier journal - Aide (F1)](../../relational-databases/logs/log-file-viewer-f1-help.md).  
  
-   L'Observateur d'événements Windows.  
  
 Pour obtenir une description des événements consignés par le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , consultez [Événements consignés par le service Integration Services](../../integration-services/service/events-logged-by-the-integration-services-service.md).  
  
### <a name="to-view-service-events-for-integration-services-in-sql-server-management-studio"></a>Pour afficher les événements du service Integration Services dans SQL Server Management Studio  
  
1.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Dans le menu **Fichier** , cliquez sur **Connecter l’Explorateur d’objets**.  
  
3.  Dans la boîte de dialogue **Se connecter au serveur** , sélectionnez le type de serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , sélectionnez ou recherchez le serveur auquel la connexion doit être établie, puis cliquez sur **Se connecter**.  
  
4.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , puis sélectionnez **Afficher les journaux**.  
  
5.  Pour afficher les événements [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , sélectionnez **SQL Server Integration Services**. L’option **Événements NT** est automatiquement sélectionnée et effacée avec l’option **SQL Server Integration Services** .  
  
### <a name="to-view-service-events-for-integration-services-in-windows-event-viewer"></a>Pour afficher les événements du service Integration Services dans l'Observateur d'événements Windows  
  
1.  Dans le **Panneau de configuration**, si vous utilisez l’affichage classique, cliquez sur **Outils d’administration**. Si vous utilisez l’affichage des catégories, cliquez sur **Performance et maintenance** puis sur **Outils d’administration**.  
  
2.  Cliquez sur **Observateur d’événements**.  
  
3.  Dans la boîte de dialogue **Observateur d’événements** , cliquez sur **Application**.  
  
4.  Dans le composant logiciel enfichable **Application** , recherchez dans la colonne **Source** l’entrée dont la valeur est **SQLISService**, cliquez sur cette entrée avec le bouton droit, puis cliquez sur **Propriétés**.  
  
5.  Si vous le souhaitez, cliquez sur la flèche vers le haut ou le bas pour afficher l'événement précédent ou suivant.  
  
6.  Vous pouvez également cliquez sur l'icône Copier dans le Presse-papiers pour copier les informations sur l'événement.  
  
7.  Choisissez d'afficher les données d'événements sous forme d'octets ou de mots.  
  
8.  Cliquez sur **OK**.  
  
9. Dans le menu **Fichier** , cliquez sur **Quitter** pour fermer la boîte de dialogue **Observateur d’événements** .  
 
## <a name="related-tasks"></a>Tâches associées  
 Pour plus d’informations sur l’affichage des entrées de journal, consultez [Événements journalisés par un package Integration Services](../../integration-services/performance/events-logged-by-an-integration-services-package.md)  
