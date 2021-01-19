---
title: Propriétés de la base de données (page Magasin de requêtes) | Microsoft Docs
description: Découvrez comment utiliser l’onglet Magasin des requêtes de la boîte de dialogue Propriétés de la base de données pour configurer les modes, les intervalles, les seuils et d’autres propriétés du magasin des requêtes.
ms.custom: ''
ms.date: 11/09/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.querystore.f1
ms.assetid: da47d75e-291a-4305-acef-4b0aaf5215da
author: stevestein
ms.author: sstein
ms.openlocfilehash: a0a7612c37ff63f851e5c3a42215192f9f982bdf
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171821"
---
# <a name="database-properties-query-store-page"></a>Propriétés de la base de données (page Magasin de requêtes)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Accédez à cette page depuis la base de données principale et utilisez-la pour configurer et modifier les propriétés du magasin de requêtes de la base de données. Ces options peuvent également être configurées à l'aide des [options ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md). Pour plus d'informations sur le magasin de requêtes, consultez [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).  
  
||  
|-|  
|**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] jusqu’à la [version actuelle](../../sql-server/what-s-new-in-sql-server-2016.md)), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].|  
  
## <a name="options"></a>Options  
 Mode d'opération  
 Les valeurs valides sont READ_ONLY et READ_WRITE. La valeur OFF désactive le magasin des requêtes. En mode READ_WRITE, le magasin de requête collecte et conserve les informations sur le plan de requête et les statistiques d'exécution. En mode READ_ONLY, les informations peuvent être lues à partir du magasin de requête, mais les nouvelles informations ne sont pas ajoutées. Si la valeur maximale de l'espace du magasin de requête allouée est épuisée, le mode d'opération du magasin de requêtes passe en READ_ONLY.  
  
 Mode d'opération (réel)  
 Obtient le mode de fonctionnement réel du magasin de requêtes.  
  
 Mode d'opération (demandé)  
 Obtient et définit le mode de fonctionnement souhaité du magasin de requêtes.  
  
 Intervalle de vidange des données (minutes)  
 Détermine la fréquence à laquelle les données écrites sur le magasin de requête magasin est conservée sur le disque. Pour optimiser les performances, les données collectées par le magasin de requête sont écrites de façon asynchrone sur le disque. La fréquence à laquelle ce transfert asynchrone se produit est configurée.  
  
 Intervalle de collecte des statistiques  
 Obtient et définit la valeur de l'intervalle de collecte des statistiques.  
  
 Taille maximale (Mo)  
 Obtient et définit l'espace total alloué au magasin de requêtes.  
  
 Mode de capture du magasin des requêtes  
 -   None ne capture pas de nouvelles requêtes.  
  
-   All capture toutes les requêtes.  
  
-   Auto capture les requêtes en fonction de l’utilisation des ressources.  
  
 Seuil de requêtes périmées (jours)  
 Obtient et définit le seuil de requêtes périmées. Configurez l'argument STALE_QUERY_THRESHOLD_DAYS pour spécifier le nombre de jours de conservation des données dans le magasin de requêtes.  
  
 Purger les données de requête  
 Supprime le contenu du magasin de requêtes.  
  
 Graphiques à secteurs  
 Le graphique de gauche montre la consommation totale du fichier de base de données sur le disque et la partie du fichier qui est remplie avec les données du magasin de requêtes.  
  
 Le graphique de droite montre la partie du quota de magasin de requêtes actuellement utilisé. Notez que le quota n'est pas affiché dans le graphique de gauche. Le quota peut dépasser la taille actuelle de la base de données.  
  
## <a name="remarks"></a>Notes  
 La fonctionnalité de magasin de requêtes fournit aux administrateurs de bases de données des informations sur le choix de plan de requête et les performances. Elle simplifie la résolution des problèmes de performances en vous permettant de trouver rapidement les différences de performances provoquées par un changement de plan de requête. La fonctionnalité capture automatiquement l'historique des requêtes, des plans et des statistiques d'exécution et les conserve à des fins de consultation. Elle sépare les données en périodes, ce qui vous permet de voir les modèles d'utilisation de base de données et de comprendre à quel moment le changement de plan de requête a eu lieu sur le serveur. Le magasin de requêtes peut être configuré à l'aide de la page de propriétés de la base de données du magasin de requêtes ou de l’option [ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md) . Le magasin de requêtes présente les informations via la boîte de dialogue [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] . Pour plus d'informations sur le magasin de requêtes, consultez [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [Affichages catalogue du magasin de requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)