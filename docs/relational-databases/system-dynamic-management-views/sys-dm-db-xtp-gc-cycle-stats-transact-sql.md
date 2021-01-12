---
description: sys.dm_db_xtp_gc_cycle_stats (Transact-SQL)
title: sys.dm_db_xtp_gc_cycle_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_gc_cycle_stats_TSQL
- dm_db_xtp_gc_cycle_stats
- sys.dm_db_xtp_gc_cycle_stats_TSQL
- sys.dm_db_xtp_gc_cycle_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_gc_cycle_stats dynamic management view
ms.assetid: bbc9704e-158e-4d32-b693-f00dce31cd2f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bd6973004f499bfefc950b61382a1d0fe5c6495d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092880"
---
# <a name="sysdm_db_xtp_gc_cycle_stats-transact-sql"></a>sys.dm_db_xtp_gc_cycle_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Retourne l'état actuel des transactions validées qui ont supprimé une ou plusieurs lignes. Le thread garbage collection inactif s'exécute toutes les minutes, ou lorsque le nombre de transactions DML validées dépasse un seuil interne depuis le dernier cycle de garbage collection. Pendant le cycle de garbage collection, il déplace les transactions validées dans une ou plusieurs files d'attente associées aux générations. Les transactions qui ont généré des versions obsolètes sont regroupées en une unité de 16 transactions pouvant appartenir à 16 générations, comme suit :  
  
-   Génération 0 : stocke toutes les transactions validées avant la transaction active la plus ancienne. Les versions de ligne générées par ces transactions sont immédiatement disponibles pour le garbage collection.  
  
-   Générations 1-14 : stockent toutes les transactions avec un horodateur supérieur à la transaction active la plus ancienne. Les versions de ligne ne peuvent pas être récupérées par le garbage collector. Chaque génération peut contenir jusqu'à 16 transactions. 224 (14 * 16) transactions au total peuvent exister dans ces générations.  
  
-   Générations 15 : les transactions restantes avec un horodateur supérieur à la transaction active la plus ancienne vont dans la génération 15. Similairement à la génération 0, il n'y a pas de limite au nombre de transactions dans la génération 15.  
  
 En cas de sollicitation de la mémoire, le thread de garbage collection force la mise à jour de l'indicateur de la transaction active la plus ancienne, ce qui force le garbage collection.  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
  
|Nom de la colonne|Type|Description|  
|-----------------|----------|-----------------|  
|cycle_id|**bigint**|Identificateur unique pour le cycle de garbage collection.|  
|ticks_at_cycle_start|**bigint**|Graduations au démarrage du cycle.|  
|ticks_at_cycle_end|**bigint**|Graduations à la fin du cycle.|  
|base_generation|**bigint**|Valeur de génération de base actuelle dans la base de données. Cela représente l'horodateur de la transaction active la plus ancienne utilisé pour identifier les transactions pour le garbage collection. L'ID de la transaction active la plus ancienne est mis à jour par incréments de 16. Par exemple, si vous avez des ID de transaction 124, 125, 126... 139, la valeur sera 124. Lorsque vous ajoutez une autre transaction, par exemple 140, la valeur sera 140.|  
|xacts_copied_to_local|**bigint**|Nombre de transactions copiées du pipeline de transactions dans le tableau de génération de la base de données.|  
|xacts_in_gen_0- xacts_in_gen_15|**bigint**|Nombre de transactions dans chaque génération.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DATABASE STATE sur le serveur.  
  
## <a name="usage-scenario"></a>Scénario d'utilisation  
 Voici un exemple de résultat avec un sous-ensemble de colonnes, montrant 27 générations :  
  
```  
cycle_id   ticks_at_cycle_start ticks_at_cycle_end   base_generation  xacts_in_gen_0    xacts_in_gen_1  
  
1          123160509            123160509            1                    0                    0  
2          123176822            123176822            1                    0                    1  
3          123236826            123236826            1                    0                    1  
4          123296829            123296829            1                    0                    1  
5          123356832            123356941            129                  0                    0  
6          123357473            123357473            129                  0                    0  
7          123417486            123417486            129                  0                    0  
8          123477489            123477489            129                  0                    0  
9          123537492            123537492            129                  0                    0  
10         123597500            123597500            129                  0                    0  
11         123657504            123657504            129                  0                    0  
12         123717507            123717507            129                  0                    0  
13         123777510            123777510            129                  0                    0  
14         123837513            123837513            129                  0                    0  
15         123897516            123897516            129                  0                    0  
16         123957516            123957516            129                  0                    0  
17         124017516            124017516            129                  0                    0  
18         124077517            124077517            129                  0                    0  
19         124137517            124137517            129                  0                    0  
20         124197518            124197518            129                  0                    0  
21         124257518            124257518            129                  0                    0  
22         124317523            124317523            129                  0                    0  
23         124377526            124377526            129                  0                    0  
24         124437529            124437529            129                  0                    0  
25         124497533            124497533            129                  0                    0  
26         124557536            124557536            129                  0                    0  
27         124617539            124617539            129                  0                    0  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
