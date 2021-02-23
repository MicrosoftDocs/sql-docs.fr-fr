---
title: Compteurs de performances MSRS 2016 Native Mode, objets de performance | Microsoft Docs
description: Apprenez-en davantage sur les compteurs de performances des objets de performance MSRS 2016 Web Service et MSRS 2016 Windows Service.
ms.date: 02/17/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- Report Server Web service, performance counters
- Reporting Services performance object
- RS Web Service performance object
- counters [Reporting Services]
- performance [Reporting Services]
ms.assetid: c642fc4f-8734-4626-a194-42ac9cd8e2ef
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 1a6704706a152073cdfa1367aed5c0d194834bec
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100646331"
---
# <a name="performance-counters-msrs-2016-native-mode-performance-objects"></a>Compteurs de performances MSRS 2016 Native Mode, objets de performance
  Cette rubrique décrit les compteurs de performances des objets de performance **MSRS 2016 Web Service** et **MSRS 2016 Windows Service**. Ces objets font partie d’un déploiement SQL Server 2016 Reporting Services en mode natif.  
  
> [!NOTE]  
>  Ces objets de performance contrôlent des événements sur le serveur de rapports local. Si vous exécutez un serveur de rapports dans un déploiement avec montée en puissance parallèle, les chiffres s'appliquent au serveur actuel et non au déploiement avec montée en puissance parallèle.  
  
 Les objets de performance sont disponibles dans l’Analyseur de performances Windows (**Perfmon.exe**). Pour plus d'informations, consultez la documentation Windows, [Profilage de runtime](/dotnet/framework/debug-trace-profile/runtime-profiling) (https://msdn.microsoft.com/library/w4bz2147.aspx).  
  
 Pour plus d’informations sur les compteurs de performances en mode SharePoint, consultez [Compteurs de performances des objets de performance MSRS 2016 Web Service SharePoint Mode et MSRS 2016 Windows Service SharePoint Mode &#40;mode SharePoint&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md).  
  
 Dans cette rubrique :  
  
-   [Compteurs de performances MSRS 2016 Web Service](#bkmk_webservice)  
  
-   [Compteurs de performances MSRS 2016 Windows Service](#bkmk_windowsservice)  
  
-   [Utiliser des applets de commande PowerShell pour retourner des listes](#bkmk_powershell)  
  
##  <a name="msrs-2016-web-service-performance-counters"></a><a name="bkmk_webservice">Compteurs de performances MSRS 2016 Web Service</a>  
 L’objet de performance **MSRS 2016 Web Service** supervise les performances du serveur de rapports. Cet objet de performance inclut une collection de compteurs utilisée pour suivre le traitement du serveur de rapports initialisé en général via des opérations de consultation du rapport interactives. Lorsque vous configurez ce compteur, vous pouvez l'appliquer à toutes les instances de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou sélectionner des instances spécifiques. Ces compteurs sont réinitialisés à chaque interruption du service Web Report Server par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .  
  
 Le tableau suivant liste les compteurs inclus dans l’objet de performance **MSRS 2016 Web Service**.  
  
|Compteur|Description|  
|-------------|-----------------|  
|**Sessions actives**|Nombre de sessions actives. Ce compteur fournit un nombre cumulatif de toutes les sessions de navigateur générées à partir d'exécutions de rapports, qu'elles soient toujours actives ou non.<br /><br /> Le compteur est décrémenté à mesure que les enregistrements de session sont supprimés. Par défaut, les sessions sont supprimées après dix minutes sans activité.|  
|**Présences dans le cache/s**|Nombre de requêtes par seconde pour les rapports mis en cache. Les demandes sont relatives aux rapports qui font l'objet d'un nouveau rendu ; il ne s'agit pas de demandes concernant les rapports traités directement à partir du cache. (Consultez **Total des présences dans le cache** plus loin dans cette rubrique.)|  
|**Présences dans le cache/s (modèles sémantiques)**|Nombre de requêtes par seconde pour le modèle mis en cache. Requêtes relatives aux rapports qui font l'objet d'un nouveau rendu, et non des requêtes concernant les rapports traités directement à partir du cache.|  
|**Absences dans le cache/s**|Nombre de requêtes par seconde qui n'ont pas retourné un rapport à partir du cache. Utilisez ce compteur afin de déterminer si les ressources utilisées pour la mise en cache (disque ou mémoire) sont suffisantes.|  
|**Absences dans le cache/s (modèles sémantiques)**|Nombre de requêtes par seconde qui n'ont pas retourné de modèle à partir du cache. Utilisez ce compteur afin de déterminer si les ressources utilisées pour la mise en cache (disque ou mémoire) sont suffisantes.|  
|**Premières demandes de sessions/s**|Nombre de nouvelles sessions utilisateur démarrées chaque seconde à partir du cache du serveur de rapports.|  
|**Présences dans le cache mémoire/s**|Nombre de fois par seconde où les rapports sont récupérés du cache mémoire. Le *cache interne* fait partie de la mémoire cache qui stocke les rapports dans la mémoire de l’unité centrale. Quand le cache mémoire est utilisé, le serveur de rapports n’interroge pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le contenu mis en cache.|  
|**Absences dans le cache mémoire/s**|Nombre de fois par seconde où les rapports n'ont pas pu être récupérés du cache mémoire.|  
|**Demandes de sessions suivantes/s**|Nombre de requêtes par seconde pour les rapports qui sont ouverts dans une session existante (tels que les rapports rendus à partir d'un instantané de session).|  
|**Demandes de rapports**|Nombre de rapports qui sont actuellement actifs et gérés par le serveur de rapports.|  
|**Rapports exécutés/s**|Nombre d'exécutions de rapport réussies par seconde. Ce compteur fournit des statistiques à propos du volume de rapports. Utilisez ce compteur avec **Demandes/s** pour comparer l’exécution de rapports et les demandes de rapports pouvant être retournées par le cache.|  
|**Demandes/s**|Nombre de demandes par seconde envoyées au serveur de rapports. Ce compteur suit tous les types de demandes gérées par le serveur de rapports.|  
|**Total des présences dans le cache**|Nombre total de demandes de rapports à partir du cache après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web du serveur de rapports par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
|**Total des présences dans le cache (modèles sémantiques)**|Nombre total de demandes de modèle à partir du cache après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web du serveur de rapports par ASP.NET.|  
|**Total des absences dans le cache**|Nombre total d'échecs de retour d'un rapport à partir du cache après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web du serveur de rapports par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] . Utilisez ce compteur pour déterminer si l'espace disque et la mémoire sont suffisants.|  
|**Total des absences dans le cache (modèles sémantiques)**|Nombre total d'échecs de retour d'un modèle à partir du cache après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web du serveur de rapports par ASP.NET. Utilisez ce compteur pour déterminer si l'espace disque et la mémoire sont suffisants.|  
|**Total des présences dans le cache mémoire**|Nombre total de rapports mis en cache retournés par le cache mémoire après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web du serveur de rapports par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] . Le *cache interne* fait partie de la mémoire cache qui stocke les rapports dans la mémoire de l’unité centrale. Quand le cache mémoire est utilisé, le serveur de rapports n’interroge pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le contenu mis en cache.|  
|**Total des absences dans le cache mémoire**|Nombre total d'absences dans le cache mémoire après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web du serveur de rapports par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
|**Total des échecs de traitement**|Nombre d'erreurs de traitement des requêtes pour le service Web du serveur de rapports.|  
|**Total des threads rejetés**|Nombre total de threads rejetés pour le traitement asynchrone, puis gérés comme processus synchrones dans le même thread. Chaque source de données est traitée sur un thread. Si le volume de threads dépasse la capacité, les threads sont rejetés pour le traitement asynchrone et sont traités ensuite en série.|  
|**Total des rapports exécutés**|Nombre total de rapports qui ont été correctement exécutés après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web du serveur de rapports par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
|**Total de requêtes**|Nombre total des demandes envoyées au serveur de rapports après le démarrage du service. Ce compteur est réinitialisé à chaque interruption du service Web du serveur de rapports par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] .|  
  
##  <a name="msrs-2016-windows-service-performance-counters"></a><a name="bkmk_windowsservice"></a> Compteurs de performances MSRS 2016 Windows Service  
 L’objet de performance **MSRS 2016 Windows Service** supervise le service Windows du serveur de rapports. Cet objet de performance inclut une collection de compteurs utilisée pour suivre le traitement des rapports initialisé via des opérations planifiées. Les opérations planifiées peuvent englober l'abonnement et la remise, les instantanés d'exécution de rapport et l'historique de rapport. Lorsque vous configurez ce compteur, vous pouvez l'appliquer à toutes les instances de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou sélectionner des instances spécifiques.  
  
 Le tableau suivant liste les compteurs inclus dans l’objet de performance **MSRS 2016 Windows Service**.  
  
|Compteur|Description|  
|-------------|-----------------|  
|**Sessions actives**|Nombre de sessions actives stockées dans la base de données du serveur de rapports. Ce compteur fournit un nombre cumulatif de toutes les sessions de navigateur utilisables générées à partir d'abonnements à un rapport, qu'elles soient encore actives ou non.|  
|**Vidages du cache/s**|Nombre de vidages du cache par seconde.|  
|**Présences dans le cache/s**|Nombre de requêtes par seconde pour les rapports mis en cache. Les demandes sont relatives aux rapports qui font l'objet d'un nouveau rendu ; il ne s'agit pas de demandes concernant les rapports traités directement à partir du cache. (Consultez **Total des présences dans le cache** plus loin dans cette rubrique.)|  
|**Présences dans le cache/s (modèles sémantiques)**|Nombre de requêtes par seconde pour les modèles mis en cache.|  
|**Absences dans le cache/s**|Nombre de requêtes par seconde qui n'ont pas retourné un rapport à partir du cache. Utilisez ce compteur afin de déterminer si les ressources utilisées pour la mise en cache (disque ou mémoire) sont suffisantes.|  
|**Absences dans le cache/s (modèles sémantiques)**|Nombre de requêtes par seconde qui n'ont pas retourné de modèle à partir du cache. Utilisez ce compteur afin de déterminer si les ressources utilisées pour la mise en cache (disque ou mémoire) sont suffisantes.|  
|**Remises/s**|Nombre de remises de rapport par seconde, de toute extension de remise.|  
|**Événements/s**|Nombre d'événements traités par seconde. Les événements contrôlés incluent **SnapshotUpdated** et **TimedSubscription**.|  
|**Premières demandes de sessions/s**|Nombre de nouvelles sessions d'exécution de rapport crées par seconde.|  
|**Présences dans le cache mémoire/s**|Nombre de fois par seconde où les rapports sont récupérés du cache mémoire. Le *cache interne* fait partie de la mémoire cache qui stocke les rapports dans la mémoire de l’unité centrale. Quand le cache mémoire est utilisé, le serveur de rapports n’interroge pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le contenu mis en cache.|  
|**Absences dans le cache mémoire/s**|Nombre de fois par seconde où les rapports ne peuvent pas être récupérés du cache mémoire.|  
|**Demandes de sessions suivantes/s**|Nombre de requêtes par seconde pour les rapports qui sont ouverts dans une session existante (tels que les rapports rendus à partir d'un instantané de session).|  
|**Demandes de rapports**|Nombre de rapports qui sont actuellement actifs et gérés par le serveur de rapports. Utilisez ce compteur pour évaluer la stratégie de mise en cache. Il peut exister beaucoup plus de requêtes que de rapports générés.|  
|**Rapports exécutés/s**|Nombre de rapports correctement générés par seconde.|  
|**Demandes/s**|Nombre total des demandes réussies que le service du serveur de rapports a traitées par seconde.|  
|**Mises à jour d’instantanés/s**|Nombre total de mises à jour d'instantanés d'exécution de rapport par seconde.|  
|**Total des recyclages de domaine d’application**|Nombre total de cycles de domaine d'application après le démarrage du service Windows Report Server.|  
|**Total des vidages du cache**|Nombre total de mises à jour du cache du serveur de rapports après le démarrage du service. Ce compteur est remis à zéro lorsque le domaine d'application se recycle. Consultez **Vidages du cache/s**.|  
|**Total des présences dans le cache**|Nombre total de requêtes pour les rapports traités directement à partir du cache après le démarrage du service Windows Report Server. Ce compteur est remis à zéro lorsque le domaine d'application se recycle. Consultez **Présences dans le cache/s**.|  
|**Total des présences dans le cache (modèles sémantiques)**|Nombre total de requêtes pour les modèles traités directement à partir du cache après le démarrage du service Windows Report Server. Ce compteur est remis à zéro lorsque le domaine d'application se recycle. Consultez **Présences dans le cache/s**.|  
|**Total des absences dans le cache**|Nombre total d'échecs de retour d'un rapport à partir du cache après le démarrage du service Windows Report Server. Ce compteur est remis à zéro lorsque le domaine d'application se recycle. Consultez **Absences dans le cache/s**.|  
|**Total des absences dans le cache (modèles sémantiques)**|Nombre total d'échecs de retour d'un modèle à partir du cache après le démarrage du service Windows Report Server. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Total des remises**|Nombre total de rapports remis via le processeur de planification et de livraison, pour toutes les extensions de remise. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Nombre total d'événements**|Nombre total d'événements après le démarrage du service Windows du serveur de rapports. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Total des présences dans le cache mémoire**|Nombre total de rapports mis en cache retournés par le cache interne après le démarrage du service Windows du serveur du rapports. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Total des absences dans le cache mémoire**|Nombre total d'absences dans le cache mémoire après le démarrage du service. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Total des échecs de traitement**|Nombre d'erreurs de traitement des demandes dans le service Windows du serveur de rapports.|  
|**Total des threads rejetés**|Nombre total de threads rejetés pour le traitement asynchrone, puis gérés comme processus synchrone dans le même thread. En cas de charge modérée ou élevée, ce compteur augmente régulièrement.|  
|**Total des rapports exécutés**|Nombre total de rapports exécutés.|  
|**Total de requêtes**|Nombre total de rapports qui ont été correctement exécutés après le démarrage du service. Ce compteur est remis à zéro lorsque le domaine d'application se recycle.|  
|**Total des mises à jour d’instantanés**|Nombre total de mises à jour d'instantanés d'exécution de rapport.|  
  
##  <a name="use-powershell-cmdlets-to-return-lists"></a><a name="bkmk_powershell"></a> Utiliser des applets de commande PowerShell pour retourner des listes  
 ![Contenu relatif à PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenu relatif à PowerShell")Le script Windows PowerShell suivant retourne les ensembles de compteurs dans lesquels CounterSetName commence par « msr » :  
  
```  
get-counter -listset msr*  
```  
  
 Le script Windows PowerShell suivant retourne la liste des compteurs de performance de CounterSetName.  
  
```  
(get-counter -listset "MSRS 2016 Windows Service").paths  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Analyse des performances d'un serveur de rapports](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [Compteurs de performances pour les objets de performance MSRS 2016 Web Service SharePoint Mode et MSRS 2016 Windows Service SharePoint Mode &#40;mode SharePoint&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)   
 [Compteurs de performances pour des objets de performances ReportServer:Service  et ReportServerSharePoint:Service](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
  
