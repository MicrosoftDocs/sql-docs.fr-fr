---
title: MSmerge_conflict_publication_article (T-SQL)
description: Décrit la procédure stockée MSmerge_conflict_publication_article qui contient des informations sur les lignes qui sont en conflit ou les modifications de lignes qui ont été annulées pour assurer la convergence des données.
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_conflict_publication_article_TSQL
- MSmerge_conflict_publication_article
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_conflict_publication_article system table
ms.assetid: dc4490b4-02d8-4dfc-98f5-0cf8de8e11be
author: cawrites
ms.author: chadam
ms.openlocfilehash: 741aeb7e38463c738862b78cfb8140d54368b1ab
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096238"
---
# <a name="msmerge_conflict_publication_article-transact-sql"></a>MSmerge_conflict_publication_article (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSmerge_conflict_publication_article** contient des informations sur les lignes qui sont en conflit ou les modifications de lignes qui ont été annulées pour assurer la convergence des données. Il existe une table de conflits pour chaque table répliquée dans une publication, où le nom de la table de conflits est ajouté au nom de la publication et à celui de l'article. Ces tables de conflits spécifiques aux articles existent dans la base de données utilisée pour la journalisation des conflits ; il s'agit généralement de la base de données de publication, mais il peut s'agir de la base de données d'abonnement si la journalisation des conflits est décentralisée.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**_\_nom de colonne de l’article \__**|**variable**|Représente une colonne d'une table répliquée. Cette table système contient une colonne pour chaque colonne de l'article de la table.|  
|**rowguid**|**uniqueidentifier**|Identificateur de ligne de la ligne en conflit.|  
|**ModifiedDate**|**datetime**|Heure d'occurrence du conflit.|  
|**ID de source de source d’origine \_ \_**|**uniqueidentifier**|Abonnement pour lequel la modification de ligne a été annulée ou qui a perdu le conflit.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
