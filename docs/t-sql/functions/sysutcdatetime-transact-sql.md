---
description: SYSUTCDATETIME (Transact-SQL)
title: SYSUTCDATETIME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SYSUTCDATETIME
- SYSUTCDATETIME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- system time [SQL Server]
- functions [SQL Server], date and time
- time [SQL Server], functions
- date and time [SQL Server], SYSUTCDATETIME
- SYSUTCDATETIME function [SQL Server]
- time [SQL Server], system
ms.assetid: f14fc2cd-9ea8-4daf-88f4-418cf523ab55
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fce96cdf1b28d72a7be47cbdb4dd8293431589a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99100937"
---
# <a name="sysutcdatetime-transact-sql"></a>SYSUTCDATETIME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie une valeur **datetime2** qui contient la date et l’heure de l’ordinateur sur lequel l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute. La date et l’heure sont renvoyées sous forme d’heure UTC (temps universel coordonné). La spécification de la précision de la fraction de seconde a une plage de 1 à 7 chiffres. La précision par défaut est de 7 chiffres.  
  
> [!NOTE]  
>  SYSDATETIME et SYSUTCDATETIME ont plus de précision en fractions de seconde que GETDATE et GETUTCDATE. SYSDATETIMEOFFSET inclut le décalage de fuseau horaire système. SYSDATETIME, SYSUTCDATETIME et SYSDATETIMEOFFSET peuvent être assignés à une variable de l’un des types de date et d'heure.  
  
 Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
SYSUTCDATETIME ( )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>Type de retour  
 **datetime2**  
  
## <a name="remarks"></a>Remarques  
 Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent faire référence à SYSUTCDATETIME partout où elles peuvent faire référence à une expression **datetime2**.  
  
 SYSUTCDATETIME est une fonction non déterministe. Les vues et expressions qui référencent cette fonction dans une colonne ne peuvent pas être indexées.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] obtient les valeurs d’heure et de date en utilisant l’API Windows GetSystemTimeAsFileTime(). La précision dépend des composants matériels de l'ordinateur et de la version de Windows sur laquelle s'exécute l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La précision de cette API est fixée à 100 nanosecondes. La précision peut être déterminée en utilisant l’API Windows GetSystemTimeAdjustment().  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants utilisent les six fonctions système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui retournent les date et heure actuelles pour retourner la date, l'heure ou les deux. Les valeurs sont retournées en séries ; par conséquent, leurs fractions de seconde peuvent être différentes.  
  
### <a name="a-showing-the-formats-that-are-returned-by-the-date-and-time-functions"></a>R. Affichage des formats qui sont retournés par les fonctions de date et d'heure  
 L'exemple suivant illustre les différents formats qui sont retournés par les fonctions de date et d'heure.  
  
```sql
SELECT SYSDATETIME() AS [SYSDATETIME()]  
    ,SYSDATETIMEOFFSET() AS [SYSDATETIMEOFFSET()]  
    ,SYSUTCDATETIME() AS [SYSUTCDATETIME()]  
    ,CURRENT_TIMESTAMP AS [CURRENT_TIMESTAMP]  
    ,GETDATE() AS [GETDATE()]  
    ,GETUTCDATE() AS [GETUTCDATE()];  
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
  
### <a name="b-converting-date-and-time-to-date"></a>B. Conversion de date et d'heure en date  
 L'exemple suivant indique comment convertir des valeurs de date et heure en `date`.  
  
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
2007-04-30
2007-04-30
2007-04-30
2007-04-30
2007-04-30
2007-04-30
```  
  
### <a name="c-converting-date-and-time-values-to-time"></a>C. Conversion des valeurs de date et d'heure en heure  
 L'exemple suivant indique comment convertir des valeurs de date et heure en `time`.  
  
 ```sql
DECLARE @DATETIME DATETIME = GetDate();
DECLARE @TIME TIME
SELECT @TIME = CONVERT(time, @DATETIME)
SELECT @TIME AS 'Time', @DATETIME AS 'Date Time'
```
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Time             Date Time  
13:49:33.6330000 2009-04-22 13:49:33.633
```  
  
## <a name="see-also"></a>Voir aussi  
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)   
 [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  


