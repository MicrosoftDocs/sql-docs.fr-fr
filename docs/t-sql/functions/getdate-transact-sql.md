---
title: GETDATE (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour la fonction GETDATE qui retourne l’heure système actuelle de la base de données en tant que valeur datetime.
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GETDATE_TSQL
- GETDATE
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- GETDATE function [SQL Server]
- current date and time [SQL Server]
- time [SQL Server], current
- functions [SQL Server], time
- system date and time [SQL Server]
- system time [SQL Server]
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], current date and time
- date and time [SQL Server], GETDATE
- dates [SQL Server], system date and time
- time [SQL Server], system
ms.assetid: bebe3b65-2b3e-4c73-bf80-ff1132c680a7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c2d45b94b3a26c98e45193e9d4240366bec20b1a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191111"
---
# <a name="getdate-transact-sql"></a>GETDATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Renvoie l’horodateur système de base de données actuel en tant que valeur **datetime** sans le décalage de fuseau horaire de la base de données. Cette valeur est dérivée du système d'exploitation de l'ordinateur sur lequel l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute.

> [!NOTE]
>  SYSDATETIME et SYSUTCDATETIME ont plus de précision en fractions de seconde que GETDATE et GETUTCDATE. SYSDATETIMEOFFSET inclut le décalage de fuseau horaire système. SYSDATETIME, SYSUTCDATETIME et SYSDATETIMEOFFSET peuvent être assignés à une variable de chacun des types de date et d'heure.

Azure SQL Database (à l’exception d’Azure SQL Managed Instance) et Azure Synapse Analytics suivent le fuseau UTC. Utilisez [AT TIME ZONE](../../t-sql/queries/at-time-zone-transact-sql.md) dans Azure SQL Database or Azure Synapse Analytics si vous devez interpréter des informations de date et d'heure dans un fuseau horaire autre qu'UTC.

 Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
GETDATE()
```

## <a name="return-type"></a>Type de retour
 **datetime**

## <a name="remarks"></a>Notes
 Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent faire référence à GETDATE partout où elles peuvent faire référence à une expression **datetime**.

 GETDATE est une fonction non déterministe. Les vues et expressions qui référencent cette fonction dans une colonne ne peuvent pas être indexées.

 L'utilisation de SWITCHOFFSET avec la fonction GETDATE() peut ralentir l'exécution de la requête car l'optimiseur de requête est incapable d'obtenir les estimations de cardinalité précises pour la valeur GETDATE. Nous vous recommandons de précalculer la valeur GETDATE, puis de spécifier cette valeur dans la requête, comme indiqué dans l'exemple suivant. Par ailleurs, utilisez l’indicateur de requête OPTION (RECOMPILE) pour obliger l’optimiseur de requête à recompiler un plan de requête lors de la prochaine exécution de la même requête. L'optimiseur disposera alors d'estimations de cardinalité précises pour GETDATE() et générera un plan de requête plus performant.

```sql
DECLARE @dt datetimeoffset = switchoffset (CONVERT(datetimeoffset, GETDATE()), '-04:00');
SELECT * FROM t
WHERE c1 > @dt OPTION (RECOMPILE);
```

## <a name="examples"></a>Exemples
 Les exemples suivants utilisent les six fonctions système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui retournent les date et heure actuelles pour retourner la date, l'heure ou les deux. Les valeurs sont retournées en séries ; par conséquent, leurs fractions de seconde peuvent être différentes.

### <a name="a-getting-the-current-system-date-and-time"></a>R. Obtention des date et heure système actuelles

```sql
SELECT SYSDATETIME()
    ,SYSDATETIMEOFFSET()
    ,SYSUTCDATETIME()
    ,CURRENT_TIMESTAMP
    ,GETDATE()
    ,GETUTCDATE();
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]

 ```
SYSDATETIME()      2007-04-30 13:10:02.0474381
SYSDATETIMEOFFSET()2007-04-30 13:10:02.0474381 -07:00
SYSUTCDATETIME()   2007-04-30 20:10:02.0474381
CURRENT_TIMESTAMP  2007-04-30 13:10:02.047
GETDATE()          2007-04-30 13:10:02.047
GETUTCDATE()       2007-04-30 20:10:02.047
```

### <a name="b-getting-the-current-system-date"></a>B. Obtention de la date système actuelle

```sql
SELECT CONVERT (date, SYSDATETIME())
    ,CONVERT (date, SYSDATETIMEOFFSET())
    ,CONVERT (date, SYSUTCDATETIME())
    ,CONVERT (date, CURRENT_TIMESTAMP)
    ,CONVERT (date, GETDATE())
    ,CONVERT (date, GETUTCDATE());
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
SYSDATETIME()          2007-05-03
SYSDATETIMEOFFSET()    2007-05-03
SYSUTCDATETIME()       2007-05-04
CURRENT_TIMESTAMP      2007-05-03
GETDATE()              2007-05-03
GETUTCDATE()           2007-05-04
```

### <a name="c-getting-the-current-system-time"></a>C. Obtention de l’heure système actuelle

```sql
SELECT CONVERT (time, SYSDATETIME())
    ,CONVERT (time, SYSDATETIMEOFFSET())
    ,CONVERT (time, SYSUTCDATETIME())
    ,CONVERT (time, CURRENT_TIMESTAMP)
    ,CONVERT (time, GETDATE())
    ,CONVERT (time, GETUTCDATE());
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
SYSDATETIME()       13:18:45.3490361
SYSDATETIMEOFFSET() 13:18:45.3490361
SYSUTCDATETIME()    20:18:45.3490361
CURRENT_TIMESTAMP   13:18:45.3470000
GETDATE()           13:18:45.3470000
GETUTCDATE()        20:18:45.3470000
```

## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
 Les exemples suivants utilisent les trois fonctions système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], qui renvoient les date et heure actuelles, pour renvoyer la date, l’heure ou les deux. Les valeurs sont retournées en séries ; par conséquent, leurs fractions de seconde peuvent être différentes.

### <a name="d-getting-the-current-system-date-and-time"></a>D. Obtention des date et heure système actuelles

```sql
SELECT SYSDATETIME()
    ,CURRENT_TIMESTAMP
    ,GETDATE();
```

### <a name="e-getting-the-current-system-date"></a>E. Obtention de la date système actuelle

```sql
SELECT CONVERT (date, SYSDATETIME())
    ,CONVERT (date, CURRENT_TIMESTAMP)
    ,CONVERT (date, GETDATE());
```

### <a name="f-getting-the-current-system-time"></a>F. Obtention de l’heure système actuelle

```sql
SELECT CONVERT (time, SYSDATETIME())
    ,CONVERT (time, CURRENT_TIMESTAMP)
    ,CONVERT (time, GETDATE());
```

## <a name="see-also"></a>Voir aussi
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
