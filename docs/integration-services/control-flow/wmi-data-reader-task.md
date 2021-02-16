---
description: Tâche Lecteur de données WMI
title: Lecteur de données WMI, tâche | Documents Microsoft
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.wmidatareadertask.f1
- sql13.dts.designer.wmidatareadertask.general.f1
- sql13.dts.designer.wmidatareadertask.wmiquery.f1
helpviewer_keywords:
- WQL [Integration Services]
- WMI Data Reader task [Integration Services]
ms.assetid: dae57067-0275-4ac3-8f34-1b9d169f1112
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d246e590cd54aca1b3a6ac911ec12d2835c8d91f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339233"
---
# <a name="wmi-data-reader-task"></a>Tâche Lecteur de données WMI

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche Lecteur de données WMI exécute des requêtes au moyen du langage de requête WMI (Windows Management Instrumentation) qui retournent des informations à partir de WMI sur un système informatique. Vous pouvez utiliser la tâche Lecteur de données WMI pour effectuer les opérations suivantes :  
  
-   Interrogation des journaux des événements Windows sur un ordinateur local ou distant et écriture des informations dans un fichier ou une variable.  
  
-   Obtention d'informations sur la présence, l'état ou les propriétés de composants matériels, puis utilisation de ces informations pour déterminer si d'autres tâches du flux de contrôle doivent être exécutées.  
  
-   Obtention d'une liste d'applications et détermination de la version de chaque application installée.  
  
 Vous pouvez configurer la tâche Lecteur de données WMI de plusieurs manières :  
  
-   Spécifiez le gestionnaire de connexions WMI à utiliser.  
  
-   Spécifiez la source de la requête WQL. La requête peut être stockée dans une propriété de tâche ou en dehors de la tâche, dans une variable ou un fichier.  
  
-   Définissez le format des résultats de requêtes WQL. La tâche prend en charge un format de table, de paire nom/valeur de propriété ou de valeur de propriété.  
  
-   Spécifiez la destination de la requête. La destination peut être une variable ou un fichier.  
  
-   Indiquez si les résultats sont ajoutés à la destination de la requête ou si celle-ci est remplacée ou conservée.  
  
 Si la source ou la destination est un fichier, la tâche Lecteur de données WMI utilise un gestionnaire de connexions de fichiers pour se connecter au fichier. Pour plus d'informations, consultez [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 La tâche Lecteur de données WMI utilise un gestionnaire de connexions WMI pour se connecter au serveur à partir duquel elle lit les informations WMI. Pour plus d'informations, consultez [WMI Connection Manager](../../integration-services/connection-manager/wmi-connection-manager.md).  
  
## <a name="wql-query"></a>Requête WQL  
 WQL est un dialecte de SQL avec des extensions qui permettent de prendre en charge la notification d'événement WMI et d'autres fonctionnalités spécifiques à WMI. Pour plus d'informations sur WQL, consultez la documentation Windows Management Instrumentation dans [MSDN Library](../../sql-server/index.yml).  
  
> [!NOTE]  
>  Les classes WMI varient d'une version de Windows à l'autre.  
  
 La requête WQL suivante retourne des entrées dans le journal des événements d'application.  
  
```  
SELECT * FROM Win32_NTLogEvent WHERE LogFile = 'Application' AND (SourceName='SQLISService' OR SourceName='SQLISPackage') AND TimeGenerated > '20050117'  
```  
  
 La requête WQL suivante retourne des informations sur les disques logiques.  
  
```  
SELECT FreeSpace, DeviceId, Size, SystemName, Description FROM Win32_LlogicalDisk  
```  
  
 La requête WQL suivante retourne une liste des mises à jour QFE (Quick Fix Engineering) du système d'exploitation.  
  
```  
Select * FROM Win32_QuickFixEngineering  
```  
  
## <a name="custom-logging-messages-available-on-the-wmi-data-reader-task"></a>Messages de journalisation personnalisés disponibles dans la tâche Lecteur de données WMI  
 Le tableau suivant répertorie les entrées de journal personnalisées de la tâche Lecteur de données WMI. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**WMIDataReaderGettingWMIData**|Indique que la tâche a commencé la lecture des données WMI.|  
|**WMIDataReaderOperation**|Indique la requête WQL que la tâche a exécutée.|  
  
## <a name="configuration-of-the-wmi-data-reader-task"></a>Configuration de la tâche Lecteur de données WMI  
 Vous pouvez définir les propriétés par programmation ou par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition par programmation de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.WmiDataReaderTask.WmiDataReaderTask>  
  
## <a name="related-tasks"></a>Tâches associées  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="wmi-data-reader-task-editor-general-page"></a>Éditeur de tâche Lecteur de données WMI (page Général)
  La page **Général** de la boîte de dialogue **Éditeur de tâche Lecteur de données WMI** permet de nommer et décrire la tâche Lecteur de données WMI.  
  
  Pour plus d’informations sur le langage de requêtes WMI (WQL), consultez la rubrique [Requêtes avec WQL](/windows/win32/wmisdk/querying-with-wql)dans la documentation Windows Management Instrumentation de la bibliothèque MSDN.  
  
### <a name="options"></a>Options  
 **Nom**  
 Fournit un nom unique pour la tâche Lecteur de données WMI. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Tapez une description de la tâche Lecteur de données WMI.  
  
## <a name="wmi-data-reader-task-editor-wmi-options-page"></a>Éditeur de tâche Lecteur de données WMI (page Options WMI)
  Utilisez la page **Options WMI** de la boîte de dialogue **Éditeur de tâche Lecteur de données WMI** pour définir la source de la requête WQL (Windows Management Instrumentation Query Language) et la destination du résultat de la requête.  
  
 Pour plus d’informations sur le langage de requêtes WMI (WQL), consultez la rubrique [Requêtes avec WQL](/windows/win32/wmisdk/querying-with-wql)dans la documentation Windows Management Instrumentation de la bibliothèque MSDN.  
  
### <a name="static-options"></a>Options statiques  
 **WMIConnectionName**  
 Sélectionnez un gestionnaire de connexions WMI dans la liste ou cliquez sur \<**New WMI Connection...**> pour créer un gestionnaire de connexions.  
  
 **Rubriques connexes :** [Gestionnaire de connexions WMI](../../integration-services/connection-manager/wmi-connection-manager.md), [Éditeur du gestionnaire de connexions WMI](../connection-manager/wmi-connection-manager.md)  
  
 **WQLQuerySourceType**  
 Sélectionnez le type de la source de la requête WQL que la tâche exécute. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Définit la source d'une requête WQL. Si vous sélectionnez cette valeur, l'option dynamique **WQLQuerySourceType** s'affiche.|  
|**Connexion de fichiers**|Sélectionnez un fichier qui contient la requête WQL. Si vous sélectionnez cette valeur, l'option dynamique **WQLQuerySourceType** s'affiche.|  
|**Variable**|Définissez la source dans une variable qui définit la requête WQL. Si vous sélectionnez cette valeur, l'option dynamique **WQLQuerySourceType** s'affiche.|  
  
 **OutputType**  
 Indiquez si la sortie doit être une table de données, une valeur de propriété ou un nom et une valeur de propriété.  
  
 **OverwriteDestination**  
 Indique s'il faut conserver ou remplacer les données d'origine dans le fichier ou la variable de destination, ou leur ajouter des données.  
  
 **DestinationType**  
 Sélectionnez le type de la destination de la requête WQL que la tâche exécute. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Connexion de fichiers**|Sélectionnez un fichier pour y enregistrer le résultat de la requête WQL. Cette valeur affiche l'option dynamique **DestinationType**.|  
|**Variable**|Définissez la variable de stockage du résultat de la requête WQL. Cette valeur affiche l'option dynamique **DestinationType**.|  
  
### <a name="wqlquerysourcetype-dynamic-options"></a>Options dynamiques WQLQuerySourceType  
  
#### <a name="wqlquerysourcetype--direct-input"></a>WQLQuerySourceType = Entrée directe  
 **WQLQuerySource**  
 Fournissez une requête ou cliquez sur le bouton de sélection (...) et entrez une requête en utilisant la boîte de dialogue **Requête WQL**.  
  
#### <a name="wqlquerysourcetype--file-connection"></a>WQLQuerySourceType = Connexion de fichiers  
 **WQLQuerySource**  
 Sélectionnez un gestionnaire de connexions de fichiers dans la liste ou cliquez sur \<**New connection...**> pour créer un gestionnaire de connexions.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
#### <a name="wqlquerysourcetype--variable"></a>WQLQuerySourceType = Variable  
 **WQLQuerySource**  
 Sélectionnez une variable dans la liste, ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md)  
  
### <a name="destinationtype-dynamic-options"></a>Options dynamiques DestinationType  
  
#### <a name="destinationtype--file-connection"></a>DestinationType = Connexion de fichiers  
 **Destination**  
 Sélectionnez un gestionnaire de connexions de fichiers dans la liste ou cliquez sur \<**New connection...**> pour créer un gestionnaire de connexions.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
#### <a name="destinationtype--variable"></a>DestinationType = Variable  
 **Destination**  
 Sélectionnez une variable dans la liste, ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
