---
description: Fonctions analytiques (Transact-SQL)
title: Fonctions analytiques (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 60fbff84-673b-48ea-9254-6ecdad20e7fe
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a78bd4578cd9c65dc226fab1788a4e1633cb4117
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98090217"
---
# <a name="analytic-functions-transact-sql"></a>Fonctions analytiques (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server prend en charge ces fonctions analytiques :

- [CUME_DIST &#40;Transact-SQL&#41;](../../t-sql/functions/cume-dist-transact-sql.md)
- [FIRST_VALUE &#40;Transact-SQL&#41;](../../t-sql/functions/first-value-transact-sql.md)
- [LAG &#40;Transact-SQL&#41;](../../t-sql/functions/lag-transact-sql.md)
- [LAST_VALUE &#40;Transact-SQL&#41;](../../t-sql/functions/last-value-transact-sql.md)
- [LEAD &#40;Transact-SQL&#41;](../../t-sql/functions/lead-transact-sql.md)
- [PERCENT_RANK &#40;Transact-SQL&#41;](../../t-sql/functions/percent-rank-transact-sql.md)
- [PERCENTILE_CONT &#40;Transact-SQL&#41;](../../t-sql/functions/percentile-cont-transact-sql.md)  
- [PERCENTILE_DISC &#40;Transact-SQL&#41;](../../t-sql/functions/percentile-disc-transact-sql.md)
  
Les fonctions analytiques calculent une valeur d’agrégation basée sur un groupe de lignes. Contrairement aux fonctions d’agrégation, les fonctions analytiques peuvent toutefois retourner plusieurs lignes pour chaque groupe. Utilisez des fonctions analytiques pour calculer des moyennes mobiles, des cumuls, des pourcentages ou des résultats de type « N premiers » dans un groupe.
 
## <a name="see-also"></a>Voir aussi

[OVER, clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
