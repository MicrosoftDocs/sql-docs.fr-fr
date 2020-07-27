---
title: Surveillance et réglage des performances | Microsoft Docs
description: Découvrez la supervision des bases de données pour évaluer les performances d’un serveur, en utilisant des instantanés périodiques et en recueillant des données en continu pour suivre les tendances des performances.
ms.custom: ''
ms.date: 07/18/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- instances of SQL Server, monitoring performance
- monitoring server performance [SQL Server]
- Database Engine [SQL Server], performance
- monitoring performance [SQL Server], about performance
- server performance [SQL Server]
- monitoring performance [SQL Server]
- database performance [SQL Server], about performance
- tuning databases [SQL Server], about performance
- status information [SQL Server], performance monitoring
- database monitoring [SQL Server], about performance
- monitoring [SQL Server], queries performance
- server performance [SQL Server], about performance
- tuning databases [SQL Server]
- database performance [SQL Server]
- monitoring [SQL Server], server performance
- database monitoring [SQL Server]
- monitoring server performance [SQL Server], about monitoring server performance
ms.assetid: 87f23f03-0f19-4b2e-bfae-efa378f7a0d4
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7706c41442ee412cacea4e61b76a14ac7129d72f
ms.sourcegitcommit: 9470c4d1fc8d2d9d08525c4f811282999d765e6e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2020
ms.locfileid: "86458873"
---
# <a name="monitor-and-tune-for-performance"></a>Surveiller et régler les performances
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Le but de la surveillance des bases de données est d'évaluer le fonctionnement d'un serveur. Une surveillance efficace implique la prise d'instantanés périodiques des performances actuelles afin d'isoler les processus à l’origine des problèmes, ainsi que la collecte de données en continu pour suivre de près les tendances des performances.  
  
 L'évaluation continue des performances de la base de données vous permet de réduire les temps de réponse et accélère le débit, ce qui optimise les performances. Un trafic réseau efficace, des E/S disque et l'utilisation de l'UC sont essentiels pour maximiser les performances. Vous devez analyser soigneusement les besoins de l'application, comprendre la structure logique et physique des données, évaluer l'utilisation de la base de données et négocier les compromis entre des utilisations conflictuelles telles que le traitement transactionnel en ligne par rapport à l'aide à la décision.  
  
## <a name="monitoring-and-tuning-databases-for-performance"></a>Surveillance et paramétrage des bases de données à des fins de performances  
 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le système d’exploitation Microsoft Windows fournissent des utilitaires qui permettent de contrôler les conditions actuelles de la base de données et de suivre l’évolution des performances en fonction de l’évolution de ces conditions. Il existe une multitude d’outils et de techniques qui permettent de surveiller [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La surveillance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vous aide à :  
  
-   Déterminer si vous pouvez améliorer les performances. Par exemple, en surveillant les temps de réponse des requêtes les plus fréquentes, vous pouvez déterminer s'il faut modifier les requêtes ou les index des tables.  
  
-   Évaluer l'activité des utilisateurs. Par exemple, en surveillant les utilisateurs qui tentent de se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez déterminer si la sécurité est correctement configurée et tester les applications et les systèmes de développement. Par exemple, en surveillant les requêtes SQL au fur et à mesure de leur exécution, vous pouvez déterminer si elles sont correctement rédigées et si elles produisent les résultats attendus.  
  
-   Résoudre les problèmes ou déboguer des composants d’application, comme des procédures stockées.  
  
## <a name="monitoring-in-a-dynamic-environment"></a>Surveillance dans un environnement dynamique  
La modification des conditions aboutit à un changement des performances. Dans vos évaluations, vous pouvez voir les changements de performances au fur et à mesure que le nombre d'utilisateurs augmente, que les accès des utilisateurs et les méthodes de connexion changent, que la base de données se remplit, que les applications clientes changent, que les données des applications changent, que les requêtes deviennent plus complexes et que le trafic réseau augmente. Grâce aux outils permettant de surveiller les performances, vous pouvez associer certaines modifications des performances à des modifications de conditions et des requêtes complexes. **Exemples :**  
  
-   en surveillant les temps de réponse des requêtes les plus fréquentes, vous pouvez déterminer s'il faut modifier, soit les requêtes, soit les index des tables ;  
  
-   en surveillant les requêtes [!INCLUDE[tsql](../../includes/tsql-md.md)] au fur et à mesure de leur exécution, vous pouvez déterminer si elles sont correctement rédigées et si elles produisent les résultats attendus ;  
  
-   en surveillant les utilisateurs qui tentent de se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez déterminer si la sécurité est correctement configurée et tester les applications et les systèmes de développement.  
  
Le temps de réponse est la durée requise pour le renvoi de la première ligne de l'ensemble de résultats à l’utilisateur sous forme de confirmation visuelle qu’une requête est en cours de traitement. Le débit mesure le nombre total de requêtes gérées par le serveur pendant une période donnée.  
  
La demande des ressources du serveur croît proportionnellement au nombre d'utilisateurs, augmentant ainsi le temps de réponse et, par conséquent, diminuant le débit global.  
  
## <a name="monitoring-and-performance-tuning-tasks"></a>Tâches de surveillance et de paramétrage des performances  
  
|Rubrique| Tâche|  
|-----------|----------------------|  
|[Surveiller les composants SQL Server](../../relational-databases/performance/monitor-sql-server-components.md)|Étapes requises pour superviser tout composant de SQL Server, tel que le Moniteur d’activité, les Événements étendus, les Vues et fonctions de gestion dynamique, et ainsi de suite.|  
|[Outils de surveillance et de réglage des performances](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)|Liste les outils de supervision et de paramétrage disponibles avec SQL Server, tels que les Statistiques des requêtes actives et l’Assistant Paramétrage du moteur de base de données.|  
|[Mise à niveau des bases de données à l’aide de l’Assistant Paramétrage de requête](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)|Maintenez la stabilité des performances de charge de travail durant la mise à niveau vers un niveau de compatibilité de base de données plus récent.|  
|[Surveillance des performances à l’aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)|Utiliser le magasin de requêtes pour capturer automatiquement l’historique des requêtes, des plans et des statistiques d’exécution et les conserver à des fins de consultation.|  
|[Établir un niveau de référence des performances](../../relational-databases/performance/establish-a-performance-baseline.md)|Comment établir un niveau de référence des performances.|  
|[Isoler les problèmes de performance](../../relational-databases/performance/isolate-performance-problems.md)|Isoler les problèmes de performance des bases de données.|  
|[Identifier les goulots d’étranglement](../../relational-databases/performance/identify-bottlenecks.md)|Surveiller et suivre les performances du serveur afin d’identifier les goulots d’étranglement.|  
|[Utiliser des vues de gestion dynamique pour déterminer les statistiques d’utilisation et les performances des vues](../../relational-databases/performance/use-dmvs-determine-usage-performance-views.md)|Traite de la méthodologie et des scripts utilisés pour obtenir des informations sur les performances des requêtes.|  
|[Analyse des performances et surveillance de l'activité du serveur](../../relational-databases/performance/server-performance-and-activity-monitoring.md)|Utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les outils de surveillance de l’activité et des performances Windows.|  
|[Superviser l’utilisation des ressources](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)|Utilisation du Moniteur système (également appelé perfmon) pour mesurer les performances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de compteurs de performance.|  

  
## <a name="see-also"></a>Voir aussi  
 [Administration automatisée à l'échelle d'une entreprise](../../ssms/agent/automated-administration-across-an-enterprise.md)    
 [Comparer et analyser des plans d’exécution](../../relational-databases/performance/compare-and-analyze-execution-plans.md)    
 [Afficher et enregistrer des plans d’exécution](../../relational-databases/performance/display-and-save-execution-plans.md)    
  
  
