---
description: sys.dm_fts_memory_pools (Transact-SQL)
title: sys.dm_fts_memory_pools (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_memory_pools_TSQL
- sys.dm_fts_memory_pools_TSQL
- sys.dm_fts_memory_pools
- dm_fts_memory_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_memory_pools dynamic management view
ms.assetid: 24747239-cd78-4d55-a00a-19233a457f42
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4864095f66786ce08227b6853b31c4f190f0d22a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161928"
---
# <a name="sysdm_fts_memory_pools-transact-sql"></a>sys.dm_fts_memory_pools (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne des informations sur les pools de mémoire partagée disponibles pour le composant Full-Text Gatherer dans le cadre d'une analyse de texte intégral ou d'une étendue d'analyse de texte intégral.  
   
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**pool_id**|**int**|ID du pool de mémoire alloué.<br /><br /> 0 = petites zones de mémoire tampon<br /><br /> 1 = grandes zones de mémoire tampon|  
|**buffer_size**|**int**|Taille de chaque mémoire tampon allouée dans le pool.|  
|**min_buffer_limit**|**int**|Nombre minimum de mémoires tampons allouées dans le pool de mémoire.|  
|**max_buffer_limit**|**int**|Nombre maximum de mémoires tampons allouées dans le pool de mémoire.|  
|**buffer_count**|**int**|Nombre actuel de mémoires tampons partagées dans le pool de mémoire.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
 
## <a name="physical-joins"></a>Jointures physiques  
 ![Jointures significatives de cette vue de gestion dynamique](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-memory-pools-1.gif "Jointures significatives de cette vue de gestion dynamique")  
  
## <a name="relationship-cardinalities"></a>Cardinalités de la relation  
  
|Du|À|Relation|  
|----------|--------|------------------|  
|dm_fts_memory_buffers.pool_id|dm_fts_memory_pools.pool_id|Plusieurs-à-un|  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne le total de la mémoire partagée que possède le composant [!INCLUDE[msCoName](../../includes/msconame-md.md)] Full-Text Gatherer du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
```  
SELECT SUM(buffer_size * buffer_count) AS "total memory"   
    FROM sys.dm_fts_memory_pools;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique de la recherche en texte intégral et de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  
