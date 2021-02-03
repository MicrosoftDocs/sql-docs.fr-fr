---
title: Statistiques des requêtes actives | Microsoft Docs
description: Découvrez comment afficher le plan d’exécution dynamique d’une requête active dans SQL Server Management Studio. Utilisez les statistiques d’exécution pour déboguer les problèmes de performance des requêtes.
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query statistics [SQL Server] live query stats
- live query statistics
- debugging [SQL Server], live query stats
- statistics [SQL Server], live query statistics
- query profiling
- lightweight query profiling
- lightweight profiling
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 588b51b156681750b071915decb4960f9238c407
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236470"
---
# <a name="live-query-statistics"></a>Statistiques des requêtes dynamiques
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] offre la possibilité de visualiser le plan d’exécution dynamique d’une requête active. Ce plan de requête active fournit des insights en temps réel sur le processus d’exécution des requêtes à mesure que les contrôles passent d’un [opérateur de plan de requête](../../relational-databases/showplan-logical-and-physical-operators-reference.md) à un autre. Le plan de requête active affiche la progression globale de la requête ainsi que des statistiques d’exécution de niveau opérateur telles que le nombre de lignes produites, le temps écoulé, la progression de l’opérateur, etc. Vous pouvez accéder à ces données en temps réel sans avoir à attendre l’exécution de la requête ; ces statistiques d’exécution se révèlent donc extrêmement utiles pour résoudre les problèmes de performances de requêtes. Cette fonctionnalité est disponible à partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], mais elle peut fonctionner avec [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  

> [!NOTE]
> En interne, les statistiques des requêtes actives utilisent la vue de gestion dynamique [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md).
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
> [!WARNING]  
> Cette fonctionnalité est principalement utilisée à des fins de dépannage. Son utilisation peut légèrement ralentir les performances globales des requêtes, en particulier dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Pour plus d’informations, consultez [Infrastructure du profilage de requête](../../relational-databases/performance/query-profiling-infrastructure.md).  
> Cette fonctionnalité peut être utilisée avec le [débogueur Transact-SQL](../../ssms/scripting/configure-firewall-rules-before-running-the-tsql-debugger.md).  
  
## <a name="to-view-live-query-statistics-for-one-query"></a>Pour afficher les statistiques des requêtes actives pour une requête 
  
1.  Pour afficher le plan d’exécution des requêtes actives, accédez au menu Outils et cliquez sur l’icône **Inclure les statistiques des requêtes actives**.  
  
     ![Bouton Statistiques des requêtes actives de la barre d’outils](../../relational-databases/performance/media/livequerystatstoolbar.png "Bouton Statistiques des requêtes actives de la barre d’outils")  
  
     Vous pouvez également accéder au plan d’exécution des requêtes actives en cliquant avec le bouton droit sur une requête sélectionnée dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et en cliquant sur **Inclure les statistiques des requêtes actives**.  
  
     ![Bouton Statistiques des requêtes actives du menu contextuel](../../relational-databases/performance/media/livequerystatsmenu.png "Bouton Statistiques des requêtes actives du menu contextuel")  
  
2.  Exécutez maintenant la requête. Le plan de requête active affiche la progression globale de la requête et les statistiques d’exécution (par exemple, temps écoulé, progression, etc.) pour les opérateurs du plan de requête. Les informations relatives à la progression de la requête et les statistiques d’exécution sont régulièrement mises à jour tout au long de l’exécution de la requête. Vous pouvez utiliser ces informations pour comprendre le processus global d’exécution de la requête, déboguer les longues requêtes, les requêtes qui s’exécutent indéfiniment et les requêtes qui entraînent un dépassement tempdb, ou encore résoudre les problèmes de délai d’attente.  
  
     ![Bouton Statistiques des requêtes actives du plan d’exécution de requêtes](../../relational-databases/performance/media/livequerystatsplan.png "Bouton Statistiques des requêtes actives du plan d’exécution de requêtes")  
  
## <a name="to-view-live-query-statistics-for-any-query"></a>Pour afficher les statistiques des requêtes actives pour n’importe quelle requête 

Vous pouvez également cliquer avec le bouton droit sur n’importe quelle requête dans la table **Processus** ou **Requêtes coûteuses actives** du **[Moniteur d’activité](../../relational-databases/performance-monitor/activity-monitor.md)** pour accéder au plan d’exécution actif.  
  
 ![Bouton Statistiques des requêtes actives du Moniteur d’activité](../../relational-databases/performance/media/livequerystatsactmon.png "Bouton Statistiques des requêtes actives du Moniteur d’activité")  
  
## <a name="remarks"></a>Notes  
 L’infrastructure de profil de statistiques doit être activée pour que les statistiques de requêtes actives puissent capturer des informations sur la progression des requêtes. En fonction de la version, la surcharge peut être significative. Pour plus d’informations sur cette surcharge, consultez [Infrastructure du profilage de requête](../../relational-databases/performance/query-profiling-infrastructure.md).
  
## <a name="permissions"></a>Autorisations  
Nécessite une autorisation `SHOWPLAN` au niveau de la base de données pour l’écriture de données dans la page de résultats **Statistiques des requêtes actives** et nécessite les autorisations nécessaires pour l’exécution de la requête.
Sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], nécessite l’autorisation `VIEW SERVER STATE` au niveau du serveur pour voir les statistiques dynamiques.  
Sur les niveaux [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Premium, nécessite l’autorisation `VIEW DATABASE STATE` dans la base de données pour voir les statistiques dynamiques. Sur les niveaux Standard et De base de [!INCLUDE[ssSDS](../../includes/sssds-md.md)], nécessite l’**administrateur du serveur** ou un compte **administrateur Azure Active Directory** pour voir les statistiques dynamiques.
  
## <a name="see-also"></a>Voir aussi  
 [Plans d’exécution](../../relational-databases/performance/execution-plans.md)    
 [Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md)    
 [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Outils de surveillance et d’optimisation des performances](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Ouvrir le Moniteur d’activité &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Moniteur d’activité](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
 [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
 [Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
 [Guide de référence des opérateurs Showplan logiques et physiques](../../relational-databases/showplan-logical-and-physical-operators-reference.md)     
 [Infrastructure du profilage de requête](../../relational-databases/performance/query-profiling-infrastructure.md)