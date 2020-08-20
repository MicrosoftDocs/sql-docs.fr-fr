---
description: Options d'articles pour la réplication de fusion
title: Options d’articles pour la réplication de fusion | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- merge replication [SQL Server replication], article options
- articles [SQL Server replication], merge replication options
ms.assetid: 670abd41-d204-4cd7-a371-7664e603a0ce
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: eb832433ad5274bf63467327fc6ad8273ff4cc35
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88465219"
---
# <a name="article-options-for-merge-replication"></a>Options d'articles pour la réplication de fusion
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Les articles de table de fusion proposent de nombreuses options vous permettant de personnaliser le comportement de réplication en fonction des besoins de vos applications. La réplication de fusion vous offre les possibilités suivantes :  
  
-   Utiliser les filtres de lignes, de jointure et de colonnes. Le filtrage des articles d'une table vous permet de créer des partitions de données à publier. Pour plus d’informations, consultez [Filtrer des données publiées](../../../relational-databases/replication/publish/filter-published-data.md).  
  
-   Spécifier si les modifications sur l'Abonné sont chargées sur le serveur de publication. Pour les applications dans lesquelles toute ou partie des données doit être en lecture seule sur l'Abonné, le téléchargement seul des articles procure un avantage au niveau des performances. Pour plus d’informations, consultez [Optimiser les performances de la réplication de fusion avec les articles en téléchargement seul](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).  
  
-   Spécifier que les suppressions d'un ou plusieurs articles ne soient pas suivies par les déclencheurs de réplication et les tables système. Cette option peut être utile dans de nombreux scénarios d'application, comme ceux qui utilisent des suppressions par lot qu'il n'est pas nécessaire de répliquer. Pour plus d’informations, consultez [Optimiser les performances de la réplication de fusion avec le suivi conditionnel des suppressions](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-conditional-delete-tracking.md).  
  
-   Spécifier l'ordre de traitement des articles pour s'assurer que les articles sont traités dans l'ordre requis par votre application. Pour plus d’informations, consultez [Spécifier les options de la réplication de fusion](../../../relational-databases/replication/merge/specify-merge-replication-properties.md).  
  
-   Spécifier qu'un ensemble d'enregistrements associés soit traité comme une unité (par défaut, la réplication de fusion traite les modifications sur les tables ligne par ligne). Pour plus d’informations, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
-   Utiliser la détection et la résolution des conflits dans les cas où les mêmes données ont pu être modifiées sur plus d'un seul nœud dans la topologie. Pour plus d’informations, consulter [Détecter et résoudre des conflits de réplication de fusion](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
-   Spécifier les options de schéma, par exemple si les contraintes et déclencheurs sont copiés sur l'Abonné. Pour plus d’informations, consultez [Spécifier des options de schéma](../../../relational-databases/replication/publish/specify-schema-options.md).  
  
-   Utiliser un gestionnaire de logique métier pour répondre à de nombreuses conditions au cours de la synchronisation. Celles-ci incluent les modifications de données, les conflits et les erreurs. Pour plus d’informations, consultez [Exécuter la logique métier lors de la synchronisation de fusion](../../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
