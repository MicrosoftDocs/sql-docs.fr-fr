---
title: Plans d’exécution | Microsoft Docs
description: Découvrez les plans d’exécution ou les plans de requête, que l’optimiseur de requête crée pour le moteur de base de données SQL Server afin d’exécuter des requêtes.
ms.custom: ''
ms.date: 03/01/2020
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query plans [SQL Server]
- execution plans [SQL Server]
- execution plan [SQL Server]
- query plan [SQL Server]
- query execution plans
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753f
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 9007a29a613f1fea6891af33b66e69d98f2058b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355267"
---
# <a name="execution-plans"></a>Plans d’exécution
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Pour pouvoir exécuter des requêtes, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] doit analyser l’instruction afin de déterminer la manière la plus efficace d’accéder aux données requises. Cette analyse est gérée par un composant appelé Optimiseur de requête. L’entrée de l’optimiseur de requête est composée de la requête, du schéma de base de données (définitions des tables et des index) et de ses statistiques de base de données. La sortie de l’optimiseur de requête est un plan d’exécution de requête, parfois appelé plan de requête ou plan d’exécution.   

Un plan d'exécution de requête permet de définir : 

- **L’ordre d’accès aux tables sources.**  
  Pour créer le jeu de résultats, le serveur de bases de données peut accéder aux tables de base selon de nombreux ordres différents. Par exemple, si une instruction `SELECT` référence trois tables, le serveur de base de données accédera d’abord à `TableA`, utilisera les données de `TableA` pour extraire les lignes correspondantes de `TableB`, puis utilisera les données de `TableB` pour extraire les données de `TableC`. Les autres séquences dans lesquelles le serveur de bases de données peut accéder aux tables sont les suivantes :  
  `TableC`, `TableB`, `TableA`ou  
  `TableB`, `TableA`, `TableC`ou  
  `TableB`, `TableC`, `TableA`ou  
  `TableC`, `TableA`, `TableB`  

- **Les méthodes utilisées pour extraire les données des différentes tables.**  
  Il existe également différentes méthodes d'accès aux données dans chaque table. Si seules quelques lignes ayant des valeurs de clés spécifiques sont nécessaires, le serveur de base de données peut utiliser un index. Si toutes les lignes de la table sont nécessaires, le serveur de base de données peut ignorer les index et procéder à une analyse de la table. Si toutes les lignes de la table sont nécessaires mais qu’il existe un index dont les colonnes clés se trouvent dans une clause `ORDER BY`, l’analyse d’index plutôt que l’analyse de table peut éviter un tri séparé du jeu de résultats. Dans le cas d'une table très petite, les analyses de table peuvent s'avérer plus efficaces pour quasiment tous les accès à la table.
  
- **Les méthodes utilisées pour effectuer les calculs, et filtrer, agréger et trier les données des différentes tables.**  
  Il existe différentes méthodes permettant d’effectuer des calculs sur les données des tables (par exemple, calculer les valeurs scalaires), d’agréger et de trier les données comme le définit le texte de la requête (par exemple, en utilisant une clause `GROUP BY` ou `ORDER BY`) et de filtrer les données (par exemple, en utilisant une clause `WHERE` ou `HAVING`).

> [!NOTE]
> [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] propose trois options pour afficher les plans d’exécution :        
> -  Le ***[Plan d’exécution estimé](../../relational-databases/performance/display-the-estimated-execution-plan.md)***, à savoir le plan compilé, produit par l’optimiseur de requête en fonction des estimations. Il s’agit du plan de requête qui est stocké dans le cache du plan.        
> -  Le * **[Plan d’exécution réel](../../relational-databases/performance/display-an-actual-execution-plan.md)** _ est le plan compilé plus son [contexte d’exécution](../../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse). Il est disponible _*une fois l’exécution de la requête terminée**. c’est-à-dire les informations sur l’exécution réelle, comme les avertissements d’exécution ou, dans les versions récentes du [!INCLUDE[ssde_md](../../includes/ssde_md.md)], le temps écoulé et le temps processeur utilisés pendant l’exécution.         
> -  Les **[Statistiques des requêtes en direct](../../relational-databases/performance/live-query-statistics.md)** sont le plan compilé plus son contexte d’exécution. Ce plan est disponible pour les _*exécutions de requêtes à la volée** et est mis à jour toutes les secondes. Cela comprend des informations d’exécution telles que le nombre réel de lignes qui transitent par les [opérateurs](../../relational-databases/showplan-logical-and-physical-operators-reference.md), le temps écoulé et l’estimation de la progression des requêtes.

> [!TIP]
> Pour plus d’informations sur les plans de traitement et d’exécution des requêtes, consultez les sections [Optimisation des instructions SELECT](../../relational-databases/query-processing-architecture-guide.md#optimizing-select-statements) et [Mise en cache et réutilisation des plans d’exécution](../../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse) du Guide de l’architecture de traitement des requêtes.

## <a name="in-this-section"></a>Dans cette section  
[Infrastructure du profilage des requêtes](../../relational-databases/performance/query-profiling-infrastructure.md)     
[Affichage et enregistrement des plans d’exécution](../../relational-databases/performance/display-and-save-execution-plans.md)     
[Comparer et analyser des plans d’exécution](../../relational-databases/performance/compare-and-analyze-execution-plans.md)     
[Repères de plan](../../relational-databases/performance/plan-guides.md)     

## <a name="see-also"></a>Voir aussi  
[Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
[Outils de surveillance et d’optimisation des performances](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
[Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md)    
[Statistiques des requêtes dynamiques](../../relational-databases/performance/live-query-statistics.md)     
[Moniteur d’activité](../../relational-databases/performance-monitor/activity-monitor.md)     
[Surveillance des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
[sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
[sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
[Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
[Guide de référence des opérateurs Showplan logiques et physiques](../../relational-databases/showplan-logical-and-physical-operators-reference.md)
