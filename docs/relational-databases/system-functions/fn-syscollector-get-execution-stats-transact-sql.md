---
description: fn_syscollector_get_execution_stats (Transact-SQL)
title: fn_syscollector_get_execution_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_syscollector_get_execution_stats
- fn_syscollector_get_execution_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_syscollector_get_execution_stats function
ms.assetid: 793ad72c-a992-4a8d-8584-bcb6b3b476f1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 940b154df600b36aab5533a0b6c986d8a3e5988d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196095"
---
# <a name="fn_syscollector_get_execution_stats-transact-sql"></a>fn_syscollector_get_execution_stats (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des statistiques détaillées sur le jeu d'éléments de collecte ou le package, y compris le nombre de lignes d'erreur qui sont enregistrées par une tâche de flux de données de package. Une tâche de flux de données est un composant [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui traite les données. Ces données étant au format relationnel, elles comprennent des groupes de données d'entrée et de sortie composés de lignes.  
  
 Les statistiques sont calculées à partir des entrées de la vue syscollector_execution_stats.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_syscollector_get_execution_stats ( log_id )  
```  
  
## <a name="arguments"></a>Arguments  
 *log_id*  
 Identificateur unique local pour le journal des exécutions. *log_id* est de **type int**.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|avg_row_count_in|**int**|Nombre moyen de lignes qui ont entré les tâches de workflow du package.<br /><br /> Remarque : une tâche de workflow est un [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] composant qui traite les données. Ces données étant au format relationnel, elles comprennent un groupe de données d'entrée composé de lignes. Il s'agit du nombre de lignes qui ont entré la tâche. Une fois les données transformées, elles sont générées en sortie sous forme d'un jeu de résultats composé de lignes. La tâche de flux de données transforme les données et génère en sortie un jeu de résultats composé de lignes. Cette sortie est le nombre de lignes qui ont quitté la tâche.|  
|min_row_count_in|**int**|Nombre minimal de lignes qui ont entré les tâches de workflow du package.|  
|max_row_count_in|**int**|Nombre maximal de lignes qui ont entré les tâches de workflow du package.|  
|avg_row_count_out|**int**|Nombre moyen de lignes qui ont quitté les tâches de flux de données du package.|  
|min_row_count_out|**int**|Nombre minimal de lignes qui ont quitté les tâches de workflow du package.|  
|max_row_count_out|**int**|Nombre maximal de lignes qui ont quitté les tâches de flux de données du package.|  
|avg_duration|**int**|Durée moyenne, en millisecondes, passée dans le composant de flux de données du package.|  
|min_duration|**int**|Durée minimale, en millisecondes, passée dans le composant de flux de données du package.|  
|max_duration|**int**|Durée maximale, en millisecondes, passée dans le composant de flux de données du package.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert SELECT pour **dc_operator**.  
  
## <a name="see-also"></a>Voir aussi  
 [syscollector_execution_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-execution-stats-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)  
  
  
