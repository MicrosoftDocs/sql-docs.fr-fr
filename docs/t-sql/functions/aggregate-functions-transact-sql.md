---
description: Fonctions d'agrégation (Transact-SQL)
title: Fonctions d’agrégation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- functions [SQL Server], aggregate
- aggregate functions [SQL Server], about aggregate functions
- summarizing functions
- aggregate functions [SQL Server]
ms.assetid: 0c06ae42-eb0a-4d77-9d74-aa1e7f344009
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b51617ece2e2de35d20a262ccd5de011a2180497
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204695"
---
# <a name="aggregate-functions-transact-sql"></a>Fonctions d'agrégation (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Une fonction d’agrégation effectue un calcul sur un ensemble de valeurs et retourne une seule valeur. À l’exception de `COUNT(*)`, les fonctions d’agrégation ignorent les valeurs NULL. Les fonctions d’agrégation sont souvent utilisées avec la clause GROUP BY de l’instruction SELECT.
  
Toutes les fonctions d'agrégation sont déterministes. En d’autres termes, les fonctions d’agrégation retournent la même valeur chaque fois qu’elles sont appelées, quand elles sont appelées avec un ensemble spécifique de valeurs d’entrée. Pour plus d’informations sur le déterminisme des fonctions, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md). La [clause OVER](../../t-sql/queries/select-over-clause-transact-sql.md) peut suivre toutes les fonctions d’agrégation, sauf les fonctions STRING_AGG, GROUPING ou GROUPING_ID.
  
Utilisez les fonctions d’agrégation comme expressions seulement dans les cas suivants :
-   la liste de sélection d'une instruction SELECT (une sous-requête ou une requête externe) ;  
-   une clause HAVING.  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)] fournit les fonctions d'agrégation suivantes :

- [APPROX_COUNT_DISTINCT](../../t-sql/functions/approx-count-distinct-transact-sql.md)
- [AVG](../../t-sql/functions/avg-transact-sql.md)
- [CHECKSUM_AGG](../../t-sql/functions/checksum-agg-transact-sql.md)
- [COUNT](../../t-sql/functions/count-transact-sql.md)
- [COUNT_BIG](../../t-sql/functions/count-big-transact-sql.md)
- [GROUPING](../../t-sql/functions/grouping-transact-sql.md)
- [GROUPING_ID](../../t-sql/functions/grouping-id-transact-sql.md)
- [MAX](../../t-sql/functions/max-transact-sql.md)
- [MIN](../../t-sql/functions/min-transact-sql.md)
- [STDEV](../../t-sql/functions/stdev-transact-sql.md)
- [STDEVP](../../t-sql/functions/stdevp-transact-sql.md)
- [STRING_AGG](../../t-sql/functions/string-agg-transact-sql.md)
- [SUM](../../t-sql/functions/sum-transact-sql.md)
- [VAR](../../t-sql/functions/var-transact-sql.md)
- [VARP](../../t-sql/functions/varp-transact-sql.md)
  
## <a name="see-also"></a>Voir aussi
[Fonctions intégrées &#40;Transact-SQL&#41;](../../t-sql/functions/functions.md)  
[OVER, clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  
