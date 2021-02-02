---
description: TODATETIMEOFFSET (Transact-SQL)
title: TODATETIMEOFFSET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TO_DATETIMEOFFSET_TSQL
- SWITCH_TZ_TSQL
- SWITCH_TZ
- TO_DATETIMEOFFSET
dev_langs:
- TSQL
helpviewer_keywords:
- date and time [SQL Server], TODATETIMEOFFSET
- TODATETIMEOFFSET function
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: b5fafc08-efd4-4a3b-a0b3-068981a0a685
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1e9c5827193f80ca6967a2611a288b04bf6783c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158720"
---
# <a name="todatetimeoffset-transact-sql"></a>TODATETIMEOFFSET (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie une valeur **datetimeoffset** convertie à partir d’une expression **datetime2**.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
TODATETIMEOFFSET ( expression , time_zone )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) qui est résolue en valeur [datetime2](../../t-sql/data-types/datetime2-transact-sql.md).  
  
> [!NOTE]  
>  L’expression ne peut pas être de type **text**, **ntext** ou **image**, car ces types ne sont pas implicitement convertibles en **varchar** ou **nvarchar**.  
  
 *time_zone*  
 Expression qui représente le décalage de fuseau horaire en minutes (pour un entier), par exemple -120, ou en heures et en minutes (pour une chaîne), par exemple « +13:00 ». La plage va de +14 à -14 (en heures). L'expression est interprétée en heure locale pour le time_zone spécifié.  
  
> [!NOTE]  
>  Si l'expression est une chaîne de caractères, elle doit être au format {+|-}TZH:THM.  
  
## <a name="return-type"></a>Type de retour  
 **datetimeoffset**. La précision fractionnelle est la même que l’argument *datetime*.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-time-zone-offset-of-the-current-date-and-time"></a>R. Modification du décalage de fuseau horaire de la date et de l'heure actuelles  
 L'exemple suivant modifie le décalage de fuseau horaire de la date et de l'heure actuelles au fuseau horaire `-07:00`.  
  
```sql  
DECLARE @todaysDateTime DATETIME2;  
SET @todaysDateTime = GETDATE();  
SELECT TODATETIMEOFFSET (@todaysDateTime, '-07:00');  
-- RETURNS 2019-04-22 16:23:51.7666667 -07:00  
```  
  
### <a name="b-changing-the-time-zone-offset-in-minutes"></a>B. Modification du décalage de fuseau horaire en minutes  
 L'exemple suivant modifie le fuseau horaire actuel à `-120` minutes.  
  
```sql  
SELECT TODATETIMEOFFSET(SYSDATETIME(), -120)
-- RETURNS: 2019-04-22 11:39:21.6986813 -02:00  
```  
  
### <a name="c-adding-a-13-hour-time-zone-offset"></a>C. Ajout d'un décalage de fuseau horaire de 13 heures  
 L'exemple suivant ajoute un décalage de fuseau horaire de 13 heures à une date et une heure.  
  
```sql  
SELECT TODATETIMEOFFSET(SYSDATETIME(), '+13:00')
-- RETURNS: 2019-04-22 11:39:29.0339301 +13:00
```  
  
## <a name="see-also"></a>Voir aussi  
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)   
 [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  

