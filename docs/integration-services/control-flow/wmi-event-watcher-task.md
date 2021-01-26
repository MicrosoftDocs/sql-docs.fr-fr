---
description: Tâche Observateur d'événement WMI
title: Observateur d’événement WMI, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.wmieventwatchertask.f1
- sql13.dts.designer.wmieventwatcher.general.f1
- sql13.dts.designer.wmieventwatcher.wmiquery.f1
helpviewer_keywords:
- WQL [Integration Services]
- WMI Event Watcher task [Integration Services]
ms.assetid: b5bb52e9-a77e-41e1-93f9-d4c3bc6b2c9a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8b4e04871bf0264c9e9c723365f2a29d95265ffa
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594934"
---
# <a name="wmi-event-watcher-task"></a>Tâche Observateur d'événement WMI

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche Observateur d'événement WMI observe les événements WMI (Windows Management Instrumentation) à l'aide d'une requête d'événement WQL (Windows Management Instrumentation Query Language) pour spécifier les événements dignes d'intérêt. Vous pouvez utiliser la tâche Observateur d'événement WMI pour effectuer les opérations suivantes :  
  
-   Attendre la notification signalant que des fichiers ont été ajoutés à un dossier, puis initier le traitement du fichier.  
  
-   Exécuter un package qui supprime des fichiers lorsque la mémoire disponible sur un serveur tombe en deçà d'un pourcentage spécifique.  
  
-   Observer l'installation d'une application, puis exécuter un package qui utilise cette application.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] inclut une tâche qui lit les informations WMI.  
  
 Pour plus d'informations sur cette tâche, cliquez sur la rubrique suivante :  
  
-   [Tâche Lecteur de données WMI](../../integration-services/control-flow/wmi-data-reader-task.md)  
  
## <a name="wql-queries"></a>Requêtes WQL  
 WQL est un dialecte de SQL avec des extensions qui permettent de prendre en charge la notification d'événement WMI et d'autres fonctionnalités spécifiques à WMI. Pour plus d'informations sur WQL, consultez la documentation Windows Management Instrumentation dans [MSDN Library](/documentation/).  
  
> [!NOTE]  
>  Les classes WMI varient d'une version de Windows à l'autre.  
  
 La requête suivante observe la notification signalant que l'utilisation du processeur est supérieure à 40 %.  
  
```  
SELECT * from __InstanceModificationEvent WITHIN 2 WHERE TargetInstance ISA 'Win32_Processor' and TargetInstance.LoadPercentage > 40  
```  
  
 La requête suivante observe la notification signalant qu'un fichier a été ajouté à un dossier.  
  
```  
SELECT * FROM __InstanceCreationEvent WITHIN 10 WHERE TargetInstance ISA "CIM_DirectoryContainsFile" and TargetInstance.GroupComponent= "Win32_Directory.Name=\"c:\\\\WMIFileWatcher\""   
```  
  
## <a name="custom-logging-messages-available-on-the-wmi-event-watcher-task"></a>Messages de journalisation personnalisés disponibles dans la tâche Observateur d'événement WMI  
 Le tableau suivant répertorie les entrées de journal personnalisées de la tâche Observateur d'événement WMI. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**WMIEventWatcherEventOccurred**|Indique qu'un événement surveillé par la tâche s'est produit.|  
|**WMIEventWatcherTimedout**|Indique que le délai de la tâche a expiré.|  
|**WMIEventWatcherWatchingForWMIEvents**|Indique que la tâche a commencé l'exécution de la requête WQL. L'entrée inclut la requête.|  
  
## <a name="configuration-of-the-wmi-event-watcher-task"></a>Configuration de la tâche Observateur d'événement WMI  
 Vous pouvez configurer la tâche Lecteur de données WMI de plusieurs manières :  
  
-   Spécifiez le gestionnaire de connexions WMI à utiliser.  
  
-   Spécifiez la source de la requête WQL. Celle-ci peut être externe à la tâche, une variable ou un fichier, ou la requête peut être stockée dans une propriété de tâche.  
  
-   Spécifiez l'action exécutée par la tâche lorsque l'événement WMI se produit. Vous pouvez enregistrer la notification d'événement et l'état après l'événement ou déclencher des événements [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] personnalisés qui fournissent des informations associées à l'événement WMI, à la notification et à l'état après l'événement.  
  
-   Définissez la manière dont la tâche répond à l'événement. La tâche peut être configurée de façon à réussir ou à échouer, selon l'événement, ou elle peut simplement observer encore l'événement.  
  
-   Spécifiez l'action exécutée par la tâche lorsque le délai d'attente de requête WMI arrive à expiration. Vous pouvez enregistrer le délai d’attente et l’état après le délai d’attente ou bien déclencher un événement [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] personnalisé, indiquant que l’événement WMI a dépassé le délai d’attente, et enregistrant le délai d’attente et l’état après le délai d’attente.  
  
-   Définissez la manière dont la tâche répond au délai d'attente. La tâche peut être configurée de façon à réussir ou à échouer, ou elle peut simplement observer encore l'événement.  
  
-   Spécifiez le nombre de fois où la tâche observe l'événement.  
  
-   Spécifiez le délai d'attente.  
  
 Si la source est un fichier, la tâche Observateur d'événement WMI utilise un gestionnaire de connexions de fichiers pour se connecter au fichier. Pour plus d'informations, consultez [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 La tâche Observateur d'événement WMI utilise un gestionnaire de connexions WMI pour se connecter au serveur à partir duquel elle lit les informations WMI. Pour plus d'informations, consultez [WMI Connection Manager](../../integration-services/connection-manager/wmi-connection-manager.md).  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-wmi-event-watcher-task"></a>Configuration par programmation de la tâche Observateur d'événement WMI  
 Pour plus d'informations sur la définition par programme de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.WmiEventWatcherTask.WmiEventWatcherTask>  
  
## <a name="wmi-event-watcher-task-editor-general-page"></a>Éditeur de tâche Observateur d'événement WMI (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche Observateur d'événement WMI** pour nommer et décrire la tâche Observateur d'événement WMI.  
  
 Pour plus d’informations sur le langage de requêtes WMI (WQL), consultez la rubrique [Requêtes avec WQL](/windows/win32/wmisdk/querying-with-wql)dans la documentation Windows Management Instrumentation de la bibliothèque MSDN.  
  
### <a name="options"></a>Options  
 **Nom**  
 Fournissez un nom unique pour la tâche Observateur d'événement WMI. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Entrez une description de la tâche Observateur d'événement WMI.  
  
## <a name="wmi-event-watcher-task-editor-wmi-options-page"></a>Éditeur de tâche Observateur d'événement WMI (page Options WMI)
  Utilisez la page **Options WMI** de la boîte de dialogue **Éditeur de tâche Observateur d'événement WMI** pour définir la source de la requête WQL (Windows Management Instrumentation Query Language) et le mode de réponse de la tâche Observateur d'événement WMI aux événements WMI (Microsoft Windows Instrumentation).  
  
 Pour plus d’informations sur le langage de requêtes WMI (WQL), consultez la rubrique [Requêtes avec WQL](/windows/win32/wmisdk/querying-with-wql)dans la documentation Windows Management Instrumentation de la bibliothèque MSDN.  
  
### <a name="static-options"></a>Options statiques  
 **WMIConnectionName**  
 Sélectionnez un gestionnaire de connexions WMI dans la liste ou cliquez sur \<**New WMI Connection...**> pour créer un gestionnaire de connexions.  
  
 **Rubriques connexes :** [Gestionnaire de connexions WMI](../../integration-services/connection-manager/wmi-connection-manager.md), [Éditeur du gestionnaire de connexions WMI](../connection-manager/wmi-connection-manager.md)  
  
 **WQLQuerySourceType**  
 Sélectionnez le type de la source de la requête WQL que la tâche exécute. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Définit la source d'une requête WQL. Cette valeur affiche l'option dynamique **WQLQuerySource**.|  
|**Connexion de fichiers**|Sélectionnez un fichier qui contient la requête WQL. Cette valeur affiche l'option dynamique **WQLQuerySource**.|  
|**Variable**|Définit la source dans une variable qui définit la requête WQL. Cette valeur affiche l'option dynamique **WQLQuerySource**.|  
  
 **ActionAtEvent**  
 Indiquez si l’événement WMI consigne l’événement et lance l’action [!INCLUDE[ssIS](../../includes/ssis-md.md)] , ou consigne uniquement l’événement.  
  
 **AfterEvent**  
 Indiquez si la tâche aboutit ou échoue lorsqu'elle reçoit l'événement WMI, ou si elle continue d'observer l'occurrence de l'événement.  
  
 **ActionAtTimeout**  
 Indiquez si la tâche consigne la temporisation d'une requête WMI et déclenche un événement [!INCLUDE[ssIS](../../includes/ssis-md.md)] en réponse, ou si elle consigne uniquement la temporisation.  
  
 **AfterTimeout**  
 Indiquez si la tâche aboutit ou échoue en réponse à une temporisation, ou si elle continue d'observer l'occurrence d'une temporisation.  
  
 **NumberOfEvents**  
 Indiquez le nombre d'événements à observer.  
  
 **Délai d'expiration**  
 Indiquez le délai en secondes avant l'occurrence de l'événement. La valeur 0 indique qu'aucun délai n'a lieu.  
  
### <a name="wqlquerysource-dynamic-options"></a>Options dynamiques WQLQuerySource  
  
#### <a name="wqlquerysource--direct-input"></a>WQLQuerySource = Entrée directe  
 **WQLQuerySource**  
 Fournissez une requête ou cliquez sur les points de suspension (...) et entrez une requête en utilisant la boîte de dialogue **Requête WQL**.  
  
#### <a name="wqlquerysource--file-connection"></a>WQLQuerySource = Connexion de fichiers  
 **WQLQuerySource**  
 Sélectionnez un gestionnaire de connexions de fichiers dans la liste ou cliquez sur \<**New connection...**> pour créer un gestionnaire de connexions.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
#### <a name="wqlquerysource--variable"></a>WQLQuerySource = Variable  
 **WQLQuerySource**  
 Sélectionnez une variable dans la liste, ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md)