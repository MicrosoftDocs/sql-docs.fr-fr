---
description: MSSQL_REPL027183
title: MSSQL_REPL027183 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_REPL027183 error
ms.assetid: 52c271ac-1a0e-43d5-85d4-35886d1efd32
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 32112470b6740eaac429895a394a3ae5cbdec110
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206144"
---
# <a name="mssql_repl027183"></a>MSSQL_REPL027183
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|27183|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Le processus de fusion n'a pas pu énumérer les modifications apportées aux articles via le filtre de lignes paramétrable. Si le problème persiste, augmentez le délai de requête du processus, réduisez la période de rétention de la publication, puis améliorez les index des tables publiées.|  
  
## <a name="explanation"></a>Explication  
 Cette erreur est émise si le délai d'attente d'un Agent de fusion est dépassé lors du traitement des modifications dans une publication filtrée. Le dépassement du délai d'attente peut être causé par l'un des problèmes suivants :  
  
-   Non-utilisation de l'optimisation des partitions précalculées.  
  
-   Fragmentation de l'index sur les colonnes utilisées pour le filtrage.  
  
-   Tables volumineuses de métadonnées de fusion, comme **MSmerge_tombstone**, **MSmerge_contents** et **MSmerge_genhistory**.  
  
-   Tables filtrées qui ne sont pas jointes sur une clé unique, et filtres de jointures qui impliquent un grand nombre de tables.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Pour résoudre le problème :  
  
-   Augmentez la valeur du paramètre **-QueryTimeOut** pour l’Agent de fusion afin de permettre au processus de se poursuivre pendant que vous résolvez les problèmes sous-jacents qui provoquent cette erreur. Les paramètres des agents peuvent être spécifiés dans des profils d'agent et sur la ligne de commande. Pour plus d'informations, consultez les pages suivantes :  
  
    -   [Utiliser des profils d’agent de réplication](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
    -   [Afficher et modifier des paramètres d’invite de commandes d’un Agent de réplication &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
-   Utilisez si possible l'optimisation des partitions précalculées. Cette optimisation est utilisée par défaut si un certain nombre de conditions de publication sont remplies. Pour plus d’informations sur ces exigences, consultez [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md). Si la publication ne remplit pas ces conditions, envisagez de modifier sa conception.  
  
-   Spécifiez la valeur la plus basse possible pour la période de rétention de la publication, parce que la réplication ne peut pas nettoyer les métadonnées de la publication et des bases de données d'abonnement tant que la période de rétention n'est pas achevée. Pour plus d’informations, voir [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
-   Dans le cadre de la gestion d'une réplication de fusion, contrôlez de temps en temps le développement des tables système associées à cette réplication : **MSmerge_contents**, **MSmerge_genhistory**, **MSmerge_tombstone**, **MSmerge_current_partition_mappings** et **MSmerge_past_partition_mappings**. Réindexez périodiquement ces tables. Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
-   Assurez-vous que les colonnes utilisées pour le filtrage sont correctement indexées et le cas échéant rebâtissez les index. Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
-   Définissez la propriété **join_unique_key** sur les filtres de jointure qui sont basés sur des colonnes uniques Pour plus d’informations, voir [Join Filters](../../relational-databases/replication/merge/join-filters.md).  
  
-   Limitez le nombre de tables dans la hiérarchie du filtre de jointure. Si vous générez des filtres de jointure pour au moins cinq tables, envisagez d'autres solutions : ne filtrez pas les petites tables, les tables qui ne changent pas ou les tables de recherche principales. Utilisez des filtres de jointure seulement entre des tables qui doivent être partitionnées entre des abonnements.  
  
-   Réduisez entre les synchronisations le nombre de changements apportés aux tables filtrées, ou exécutez l'Agent de fusion plus souvent. Pour plus d'informations sur la définition de planifications de synchronisation, consultez [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
