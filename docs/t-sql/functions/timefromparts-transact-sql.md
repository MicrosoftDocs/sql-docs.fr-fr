---
description: TIMEFROMPARTS (Transact-SQL)
title: TIMEFROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TIMEFROMPARTS_TSQL
- TIMEFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- TIMEFROMPARTS function
ms.assetid: 786c65a1-2b3f-4e4b-82b6-4940d62f3801
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: db314c5d21ab27e81133048b90546efbbe5bb100
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205493"
---
# <a name="timefromparts-transact-sql"></a>TIMEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie une valeur **time** pour l’heure spécifiée et avec la précision spécifiée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
TIMEFROMPARTS ( hour, minute, seconds, fractions, precision )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *hour*  
 Expression entière spécifiant des heures.  
  
 *minute*  
 Expression entière spécifiant des minutes.  
  
 *secondes*  
 Expression entière spécifiant des secondes.  
  
 *fractions*  
 Expression entière spécifiant des fractions.  
  
 *precision*  
 Littéral entier spécifiant la précision de la valeur **time** à renvoyer.  
  
## <a name="return-types"></a>Types de retour  
 **time(** *precision* **)**  
  
## <a name="remarks"></a>Notes  
 TIMEROMPARTS retourne une valeur temporelle complètement initialisée. Si les arguments ne sont pas valides, une erreur est générée. Si l'un des paramètres est NULL, la valeur NULL est retournée. Toutefois, si l’argument *precision* est NULL, une erreur est générée.  
  
 L’argument *fractions* dépend de l’argument *precision*. Par exemple, si *precision* a pour valeur 7, chaque fraction représente 100 nanosecondes ; si *precision* a pour valeur 3, chaque fraction représente une milliseconde. Si la valeur de *precision* est zéro, la valeur de *fractions* doit également être zéro ; sinon, une erreur est générée.  
  
 Cette fonction peut être exécutée à distance sur les serveurs [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures. Elle ne peut pas être exécutée à distance sur des serveurs avec une version antérieure à [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-example-without-fractions-of-a-second"></a>R. Exemple simple sans fractions de seconde  
  
```sql
SELECT TIMEFROMPARTS ( 23, 59, 59, 0, 0 ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------------------  
23:59:59.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Exemple avec fractions de seconde  
 L’exemple suivant illustre l’utilisation des paramètres *fractions* et *precision* :  
  
1.  Lorsque *fractions* a la valeur 5 et *precision* la valeur 1, la valeur de *fractions* représente 5/10 de seconde.  
  
2.  Lorsque *fractions* a la valeur 50 et *precision* la valeur 2, la valeur de *fractions* représente 50/100 de seconde.  
  
3.  Lorsque *fractions* a la valeur 500 et *precision* la valeur 3, la valeur de *fractions* représente 500/1000 de seconde.  
  
```sql  
SELECT TIMEFROMPARTS ( 14, 23, 44, 5, 1 );  
SELECT TIMEFROMPARTS ( 14, 23, 44, 50, 2 );  
SELECT TIMEFROMPARTS ( 14, 23, 44, 500, 3 );  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------------  
14:23:44.5  
  
(1 row(s) affected)  
  
----------------  
14:23:44.50  
  
(1 row(s) affected)  
  
----------------  
14:23:44.500  
  
(1 row(s) affected)  
```  
  

