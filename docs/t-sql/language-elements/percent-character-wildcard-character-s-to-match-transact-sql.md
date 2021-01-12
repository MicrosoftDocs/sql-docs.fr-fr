---
description: Caractère de pourcentage (caractères génériques à ne pas faire correspondre - Transact-SQL)
title: Recherche par caractères génériques (%)
ms.custom: seo-lt-2019
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '%'
- '%_TSQL'
- wildcard
dev_langs:
- TSQL
helpviewer_keywords:
- conditions [SQL Server], wildcards
- '% (wildcard - character(s) to match)'
- matching conditions [SQL Server]
- wildcard characters [SQL Server]
- characters [SQL Server], wildcard
ms.assetid: d4cbc1a9-37e1-4101-97fb-e6ac30c1223e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0674902870a21db2a51d178a88c677128d16aea9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092143"
---
# <a name="percent-character-wildcard---characters-to-match-transact-sql"></a>Caractère de pourcentage (caractères génériques à ne pas faire correspondre - Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Correspond à toute chaîne de zéro caractères ou plus. Ce caractère générique peut être utilisé comme préfixe ou comme suffixe.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne tous les prénoms des personnes de la table `Person` de `AdventureWorks2012` qui commencent par `Dan`.  
  
```syntaxsql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName  
FROM Person.Person  
WHERE FirstName LIKE 'Dan%';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
 [&#91; &#93; (Caractère générique - Caractères à rechercher)](../../t-sql/language-elements/wildcard-character-s-to-match-transact-sql.md)   
  [&#91;^&#93; (Caractère générique - Caractères à exclure)](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)     
 [_ (Caractère générique - représente un caractère)](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)  
    
  
