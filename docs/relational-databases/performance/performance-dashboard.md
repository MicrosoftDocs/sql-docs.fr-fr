---
title: Tableau de bord Performances | Microsoft Docs
description: Découvrez le tableau de bord Performances de SQL Server Management Studio, qui fournit un insight rapide de SQL Server et d’Azure SQL Managed Instance.
ms.custom: ''
ms.date: 11/13/2020
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- performance dashboard [SQL Server]
- performance dashboard reports
- perf dashboard
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: pelopes
ms.author: pelopes
manager: amitban
ms.openlocfilehash: ef87d8cdfb44d36faff5e06cfdc0b0f691142c03
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170401"
---
# <a name="performance-dashboard"></a>Tableau de bord Performances
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] version 17.2 et ultérieures inclut le tableau de bord Performances. Ce tableau de bord a été conçu pour fournir visuellement un insight rapide de l’état du niveau de performance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) et d’[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] Managed Instance. 

Le tableau de bord Performances aide à identifier rapidement si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] est victime d’un goulot d’étranglement de performances. Si un goulot d’étranglement est trouvé, capturez facilement des données de diagnostic supplémentaires pouvant être nécessaires pour résoudre le problème. Voici certains problèmes de performances courants que le tableau de bord Performances peut aider à identifier :
-  Goulots d'étranglement au niveau de l'unité centrale (et identification des requêtes sollicitant le plus l'unité centrale)
-  Goulots d’étranglement E/S (et identification des requêtes effectuant le plus d’E/S)
-  Recommandations d’index générées par l’optimiseur de requête (index absents)
-  Blocage
-  Conflit de ressources (y compris une contention de verrous)

Le tableau de bord Performances aide également à identifier les requêtes coûteuses qui peuvent avoir été exécutées avant ; plusieurs métriques sont disponibles pour définir le coût élevé : UC, Écritures logiques, Lectures logiques, Durée, Lectures physiques et Durée du CLR.

Le tableau de bord Performances comprend les sections et sous-rapports suivants :
-  Utilisation de l’UC du système
-  Demandes actuellement en attente
-  Activité actuelle
   -  Demandes utilisateur
   -  Sessions utilisateur
   -  Taux d'accès au cache
-  informations historiques
   -  Attend
   -  Verrous internes
   -  Statistiques d’E/S
   -  Requêtes coûteuses
- Informations diverses
  -  Traces actives
  -  Sessions Xevent actives
  -  Bases de données
  -  Index absents

> [!NOTE] 
> En interne, le tableau de bord Performances utilise des fonctions (DMF) et des vues de gestion dynamique (DMV) liées à l’[exécution](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md), aux [index](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md) et aux [E/S](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md).

## <a name="to-view-the-performance-dashboard"></a>Pour afficher le tableau de bord Performances 
  
Pour afficher le tableau de bord Performances, cliquez avec le bouton droit sur le nom de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l’Explorateur d’objets, sélectionnez **Rapports**, **Rapports standard**, puis cliquez sur **Tableau de bord Performances**.  
  
![Tableau de bord Performances dans le menu](../../relational-databases/performance/media/perf_dashboard_ssms.png "Tableau de bord Performances dans le menu")  
  
Le tableau de bord Performances apparaît sous la forme d’un nouvel onglet. Voici un exemple indiquant clairement la présence d’un goulot d’étranglement au niveau de l’UC :  
  
![Écran principal du tableau de bord Performances](../../relational-databases/performance/media/perf_dashboard.png "Écran principal du tableau de bord Performances")  
  
## <a name="remarks"></a>Notes
Le rapport **Index absents** indique les index potentiellement absents que l’optimiseur de requête a identifiés pendant la compilation de la requête. Toutefois, ces recommandations ne doivent pas être prises telles quelles. Microsoft recommande d’évaluer la création des index ayant un score supérieur à 100 000, car ils correspondent à la prévision d’amélioration la plus élevée pour les requêtes utilisateur. 

> [!TIP]
> Déterminez systématiquement si la suggestion d’un nouvel index est préférable à l’utilisation d’un index existant dans la même table ; en effet, vous pourriez obtenir les mêmes résultats simplement en modifiant un index existant, plutôt qu’en en créant un. Par exemple, supposons la suggestion d’un nouvel index sur les colonnes C1, C2 et C3 ; commencez par déterminer s’il existe un index sur les colonnes C1 et C2. Si tel est le cas, il peut être préférable d’ajouter simplement la colonne C3 à l’index existant (en conservant l’ordre des colonnes préexistantes) pour éviter de créer un index.
> Pour plus d’informations, consultez le [Guide de conception et d’architecture des index](../../relational-databases/sql-server-index-design-guide.md).

Le rapport **Attentes** filtre toutes les attentes d’inactivité et de mise en veille. Pour plus d’informations sur les attentes, consultez [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) et [Paramétrage des performances de SQL Server 2005 à l’aide des attentes et des files d’attente](https://download.microsoft.com/download/4/7/a/47a548b9-249e-484c-abd7-29f31282b04d/performance_tuning_waits_queues.doc).

Les rapports **Requêtes coûteuses** sont réinitialisés quand [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] redémarre, car les données dans les DMV sous-jacentes sont effacées. À partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], vous trouverez des informations détaillées sur les requêtes coûteuses dans le Magasin des requêtes. 


> [!NOTE]
> Le tableau de bord Performances a d’abord été publié sous forme de téléchargement autonome pour [SQL Server 2005](https://techcommunity.microsoft.com/t5/SQL-Server-Support/SQL-Server-2005-Performance-Dashboard-Reports/ba-p/315415), puis mis à jour pour [SQL Server 2012](https://www.microsoft.com/download/details.aspx?id=29063). Le convertisseur de rapport SQL Server Management Studio ne prend pas en charge l’accès du Presse-papiers au texte contenu dans les rapports. Cependant, ce texte est accessible dans les rapports autonomes.  Téléchargez les rapports autonomes si vous devez copier le texte des requêtes à partir des rapports.

## <a name="permissions"></a>Autorisations  
Sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], requiert les autorisations `VIEW SERVER STATE` et `ALTER TRACE`. Sur [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], requiert l’autorisation `VIEW DATABASE STATE` dans la base de données.

## <a name="see-also"></a>Voir aussi  
 [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Outils de surveillance et d’optimisation des performances](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Ouvrir le Moniteur d’activité &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Moniteur d’activité](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Analyse des performances à l’aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
