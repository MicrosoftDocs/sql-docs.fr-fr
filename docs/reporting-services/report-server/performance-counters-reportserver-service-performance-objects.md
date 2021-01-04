---
title: Compteurs de performances - Service ReportServer, objets de performance | Microsoft Docs
description: Apprenez-en davantage sur les compteurs de performance pour les objets de performance ReportServer:Service et ReportServerSharePoint:Service, qui font partie d’un déploiement SQL Server 2012.
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Report Server service, performance counters
ms.assetid: 2bcacab2-3a4f-4aae-b123-19d756b9b9ed
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 8224c126d9f23cb5f13c517400ec79e04d127ff5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466610"
---
# <a name="performance-counters---reportserver-service--performance-objects"></a>Compteurs de performances - Service ReportServer, objets de performance
  Cette rubrique décrit les compteurs de performance pour les objets de performance **ReportServer:Service** et **ReportServerSharePoint:Service** qui font partie d’un déploiement de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] .  
  
> [!NOTE]  
>  Les objets de performance sont utilisés pour contrôler des événements sur le serveur de rapports local. Si vous exécutez un serveur de rapports dans un déploiement avec montée en puissance parallèle, les chiffres s'appliquent au serveur actuel et non au déploiement avec montée en puissance parallèle dans son ensemble.  
  
 Les objets de performance sont disponibles dans l’Analyseur de performances Windows (**Perfmon.exe**). Pour plus d'informations, consultez la documentation Windows. [Profilage de runtime](/dotnet/framework/debug-trace-profile/runtime-profiling) (https://msdn.microsoft.com/library/w4bz2147.aspx).  
  
 Dans cette rubrique :  
  
-   [ReportServer:compteurs de performance de service (serveur de rapports en mode natif)](#bkmk_ReportServer)  
  
-   [ReportServerSharePoint:Service (serveur de rapports en mode SharePoint)](#bkmk_ReportServerSharePoint)  
  
-   [Utiliser des applets de commande PowerShell pour retourner des listes](#bkmk_powershell)  
  
 [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
##  <a name="reportserverservice-performance-counters-native-mode-report-server"></a><a name="bkmk_ReportServer"></a> ReportServer:compteurs de performance de service (serveur de rapports en mode natif)  
 L’objet de performance **ReportServer:Service** inclut une collection de compteurs utilisée pour suivre les événements liés au protocole HTTP et les événements relatifs à la mémoire pour une instance du serveur de rapports. Cet objet de performance apparaît une fois pour chaque instance de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur l'ordinateur, et vous pouvez ajouter ou supprimer des compteurs de l'objet de performance pour chaque instance. Les compteurs pour l’instance par défaut s’affichent au format **ReportServer:Service**. Les compteurs pour les instances nommées s’affichent au format **ReportServer$\<**_instance_name_*_>:Service_* .  
  
 L’objet de performance **ReportServer:Service** constitue une nouveauté dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Il fournit un sous-ensemble des compteurs qui étaient inclus dans les services Internet (IIS) et [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] dans les versions précédentes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Ces nouveaux compteurs sont spécifiques à [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], et ils suivent les événements liés à HTTP pour le serveur de rapports, comme les requêtes, les connexions et les tentatives d'ouverture de session. En outre, cet objet de performance inclut des compteurs pour suivre les événements de gestion de la mémoire.  
  
 Le tableau suivant répertorie les compteurs inclus dans l’objet de performance **ReportServer:Service** .  
  
 ![Contenu relatif à PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenu relatif à PowerShell") Le script Windows PowerShell suivant retourne la liste des compteurs de performance pour le compteur de performance CounterSetName.  
  
```  
(get-counter -listset "ReportServer:Service").paths  
```  
  
|Compteur|Description|  
|-------------|-----------------|  
|**Connexions actives**|Nombre de connexions actives sur le serveur.|  
|**Nombre total d’octets reçus**|Nombre d'octets reçus par le serveur. Ce compteur compte les octets bruts reçus en tout par le Gestionnaire de rapports et le serveur de rapports.|  
|**Octets reçus/s**|Nombre d'octets reçus par seconde par le serveur. Ce compteur n'est mis à jour qu'au terme d'un transfert. Cela signifie que le compteur reste à 0 tant qu'un transfert n'est pas terminé.|  
|**Nombre total d’octets envoyés**|Nombre d'octets envoyés à partir du serveur. Ce compteur compte les octets bruts envoyés en tout par le Gestionnaire de rapports et le serveur de rapports.|  
|**Octets envoyés/s**|Nombre d'octets envoyés par seconde à partir du serveur. Ce compteur n'est mis à jour qu'au terme d'un transfert. Cela signifie que le compteur reste à 0 tant qu'un transfert n'est pas terminé.|  
|**Total d’erreurs**|Nombre total d'erreurs qui se produisent au cours du traitement des requêtes HTTP. Ces erreurs incluent les codes d'état HTTP 400 et 500.|  
|**Erreurs/s**|Nombre total d'erreurs qui se produisent par seconde au cours du traitement des requêtes HTTP. Ces erreurs incluent les codes d'état HTTP 400 et 500.|  
|**Total de tentatives d’ouverture de session**|Nombre de tentatives d'ouverture de session effectuées à partir de types d'authentification RSWindows. Les types d'authentification RSWindows incluent RSWindowsNegotiate, RSWindowsNTLM, RSWindowsKerberos et RSWindowsBasic. La valeur zéro (0) représente l'authentification personnalisée.|  
|**Tentatives d’ouverture de session/s**|Taux de tentatives d'ouverture de session.|  
|**Total d’ouvertures de session réussies**|Nombre d'ouvertures de session réussies pour les types d'authentification RSWindows. Les types d'authentification RSWindows incluent RSWindowsNegotiate, RSWindowsNTLM, RSWindowsKerberos et RSWindowsBasic. La valeur zéro (0) représente l'authentification personnalisée.|  
|**Ouvertures de session réussies/s**|Taux d'ouvertures de session réussies.|  
|**État de sollicitation de la mémoire**|Nombre, compris entre 1 et 5, qui indique l'état de mémoire actuel du serveur :<br /><br /> 1 : Aucune sollicitation<br /><br /> 2 : Sollicitation faible<br /><br /> 3 : Sollicitation moyenne<br /><br /> 4 : Sollicitation élevée<br /><br /> 5 : Sollicitation dépassée|  
|**Nombre de réductions de mémoire**|Nombre d'octets demandés par le serveur pour réduire la mémoire utilisée.|  
|**Notifications de réduction de mémoire/s**|Nombre de notifications émises par le serveur au cours de la dernière seconde pour réduire la mémoire utilisée. Cette valeur indique la fréquence à laquelle le serveur se trouve en situation de sollicitation de la mémoire.|  
|**Requêtes déconnectées**|Nombre de requêtes qui sont déconnectées en raison d'un échec de communication.|  
|**Requêtes en cours d’exécution**|Nombre de requêtes en cours de traitement.|  
|**Requêtes non autorisées**|Nombre de requêtes qui échouent avec le code d'état HTTP 401.|  
|**Requêtes rejetées**|Nombre total de requêtes qui n'ont pas été traitées en raison de ressources serveur insuffisantes. Ce compteur représente le nombre de requêtes qui retournent un code d'état HTTP 503, indiquant que le serveur est trop occupé.|  
|**Requêtes totales**|Nombre total de requêtes reçues par le service du serveur de rapports depuis le démarrage. Ce compteur compte les requêtes envoyées au Gestionnaire de rapports et celles envoyées par le Gestionnaire de rapports au serveur de rapports.|  
|**Demandes/s**|Nombre de requêtes traitées par seconde. Cette valeur représente le débit actuel de l'application.|  
|**Tâches en file d’attente**|Nombre de tâches en attente d'un thread pour pouvoir être traitées. Chaque requête adressée au serveur de rapports correspond à une ou plusieurs tâches. Ce compteur représente uniquement le nombre de tâches prêtes à être traitées ; il n'inclut pas le nombre de tâches en cours d'exécution.|  
  
##  <a name="reportserversharepointservice-sharepoint-mode-report-server"></a><a name="bkmk_ReportServerSharePoint"></a> ReportServerSharePoint:Service (serveur de rapports en mode SharePoint)  
 L’objet de performance **ReportServerSharePoint:Service** a été ajouté dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 ![Contenu relatif à PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenu relatif à PowerShell") Le script Windows PowerShell suivant retourne la liste des compteurs de performance pour le compteur de performance CounterSetName.  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
|Compteur|Description|  
|-------------|-----------------|  
|**État de sollicitation de la mémoire**||  
|**Nombre de réductions de mémoire**||  
|**Memory Shrink Notifications/Sec**||  
  
##  <a name="use-powershell-cmdlets-to-return-lists"></a><a name="bkmk_powershell"></a> Utiliser des applets de commande PowerShell pour retourner des listes  
 ![Contenu relatif à PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenu relatif à PowerShell") Le script Windows PowerShell suivant retourne la liste des compteurs de performance pour le compteur de performance CounterSetName « ReportServerSharePoint:Service » :  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Analyse des performances d'un serveur de rapports](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [Compteurs de performances du service web MSRS 2011 et des objets de performance du service Windows MSRS 2011 &#40;mode natif&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)   
 [Compteurs de performances du service Web MSRS 2011 en mode SharePoint et des objets de performance du service Windows MSRS 2011 en mode SharePoint &#40;mode SharePoint&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
  
