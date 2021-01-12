---
description: ISDATE (Transact-SQL)
title: ISDATE (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ISDATETIME
- ISDATE_TSQL
- ISDATE
- ISDATETIME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], ISDATE
- validate dates times [SQL Server]
- formats [SQL Server], time
- dates [SQL Server], validate
- verify dates times [SQL Server]
- functions [SQL Server], time
- formats [SQL Server], dates
- functions [SQL Server], date and time
- time [SQL Server], functions
- time [SQL Server], validate
- ISDATE function [SQL Server]
ms.assetid: 8e2c9ee7-388a-432f-b2c9-7b398f26bf85
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5345bbdc161730ba0d08c79e0d52b7e2ec901498
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097216"
---
# <a name="isdate-transact-sql"></a>ISDATE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne 1 si le paramètre *expression* est une valeur de type **date**, **time** ou **datetime** valide ; sinon, retourne 0.  
  
 ISDATE retourne 0 si le paramètre *expression* est une valeur de type **datetime2**.  
  
 Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md). Notez que la plage de données datetime est comprise entre 1753-01-01 et 9999-12-31, tandis que la plage de données date est comprise entre 0001-01-01 et 9999-12-31.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ISDATE ( expression )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression*  
 Chaîne de caractères ou [expression](../../t-sql/language-elements/expressions-transact-sql.md) qui peut être convertie en une chaîne de caractères. L'expression doit comporter moins de 4 000 caractères. Les types de données de date et d'heure, sauf datetime et smalldatetime, ne sont pas autorisés comme argument pour ISDATE.  
  
## <a name="return-type"></a>Type de retour  
 **int**  
  
## <a name="remarks"></a>Notes  
 ISDATE est déterministe uniquement si elle est utilisée avec la fonction [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md), lorsque le paramètre de style CONVERT est spécifié et que le style est différent de 0, 100, 9 et 109.  
  
 La valeur de retour d’ISDATE dépend des paramètres définis par [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md), [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) et dans la rubrique [Configurer l’option de configuration de serveur default language](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md).  
  
## <a name="isdate-expression-formats"></a>Formats d'expression ISDATE  
 Pour obtenir des exemples de formats valides pour lesquels ISDATE retourne 1, consultez la section « Formats de littéraux de chaîne pris en charge pour datetime » dans les rubriques [datetime](../../t-sql/data-types/datetime-transact-sql.md) et [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md). Pour obtenir des exemples supplémentaires, consultez également la colonne Entrée/sortie de la section « Arguments » de [CAST et CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
 Le tableau suivant résume les formats d'expression d'entrée qui ne sont pas valides et qui retournent 0 ou une erreur.  
  
|Expression ISDATE|Valeur retournée par ISDATE|  
|-----------------------|-------------------------|  
|NULL|0|  
|Valeurs des types de données répertoriés dans [Types de données](../../t-sql/data-types/data-types-transact-sql.md) dans toute catégorie de type de données autre que les chaînes de caractères, chaînes de caractères Unicode, ou date et heure.|0|  
|Valeurs des types de données **text**, **ntext** ou **image**.|0|  
|Toute valeur qui a une échelle de précision en secondes supérieure à 3 (,0000 à ,0000000...n). ISDATE renverra 0 si le paramètre *expression* est une valeur **datetime2**, mais renverra 1 si le paramètre *expression* est une valeur **datetime** valide.|0|  
|Toute valeur qui associe une date valide à une valeur non valide, par exemple 1995-10-1a.|0|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-isdate-to-test-for-a-valid-datetime-expression"></a>R. Utilisation d'ISDATE pour tester une expression datetime valide.  
 L’exemple suivant montre comment utiliser `ISDATE` pour tester si une chaîne de caractères est une valeur **datetime** valide.  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    PRINT 'VALID'  
ELSE  
    PRINT 'INVALID';  
```  
  
### <a name="b-showing-the-effects-of-the-set-dateformat-and-set-language-settings-on-return-values"></a>B. Illustration des effets des paramètres SET DATEFORMAT et SET LANGUAGE sur les valeurs de retour  
 Les instructions suivantes affichent les valeurs qui sont retournées suite aux paramètres de `SET DATEFORMAT` et `SET LANGUAGE`.  
  
```sql  
/* Use these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
/* Expression in mdy dateformat */  
SELECT ISDATE('04/15/2008'); --Returns 1.  
/* Expression in mdy dateformat */  
SELECT ISDATE('04-15-2008'); --Returns 1.   
/* Expression in mdy dateformat */  
SELECT ISDATE('04.15.2008'); --Returns 1.   
/* Expression in myd  dateformat */  
SELECT ISDATE('04/2008/15'); --Returns 1.  
  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET DATEFORMAT dmy;  
SELECT ISDATE('15/04/2008'); --Returns 1.  
SET DATEFORMAT dym;  
SELECT ISDATE('15/2008/04'); --Returns 1.  
SET DATEFORMAT ydm;  
SELECT ISDATE('2008/15/04'); --Returns 1.  
SET DATEFORMAT ymd;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET LANGUAGE English;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET LANGUAGE Hungarian;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET LANGUAGE Swedish;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET LANGUAGE Italian;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
/* Return to these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-isdate-to-test-for-a-valid-datetime-expression"></a>C. Utilisation d'ISDATE pour tester une expression datetime valide.  
 L’exemple suivant montre comment utiliser `ISDATE` pour tester si une chaîne de caractères est une valeur **datetime** valide.  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    SELECT 'VALID';  
ELSE  
    SELECT 'INVALID';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
