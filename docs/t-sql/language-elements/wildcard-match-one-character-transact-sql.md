---
description: _ (Caractère générique - recherche de correspondance d'un seul caractère) (Transact-SQL)
title: _ (Caractère générique - Recherche d’un seul caractère) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Match
- wildcard
- _TSQL
- Match One
- _
dev_langs:
- TSQL
helpviewer_keywords:
- wildcard characters [SQL Server]
- _ (wildcard - match one character)
ms.assetid: 11a2ed36-9e21-4bdf-ae20-a31db1434b97
author: rothja
ms.author: jroth
ms.openlocfilehash: 3699c5d71de477cca95b1ec8bacaa3b56b9e438d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88417075"
---
# <a name="_-wildcard---match-one-character-transact-sql"></a>_ (Caractère générique - recherche de correspondance d'un seul caractère) (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Utilisez le trait de soulignement _ pour recherche un caractère unique dans une opération de comparaison de chaînes qui inclut des critères spéciaux, comme `LIKE` et `PATINDEX`.  
  
## <a name="examples"></a>Exemples  

## <a name="a-simple-example"></a>A. Exemple simple   

L’exemple suivant retourne tous les noms de base de données qui commencent par la lettre `m` et dont la troisième lettre est `d`. Le caractère de soulignement indique que le deuxième caractère du nom peut être n’importe quelle lettre. Les bases de données `model` et `msdb` répondent à ces critères. La base de données `master` n’y répond pas.

```sql
SELECT name FROM sys.databases
WHERE name LIKE 'm_d%';
```   
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   
```
name
-----
model
msdb
```   
Vous pouvez avoir d’autres bases de données qui répondent à ces critères.

Vous pouvez utiliser plusieurs traits de soulignement pour représenter plusieurs caractères. Si vous modifiez les critères `LIKE` de sorte à inclure deux traits de soulignement `'m__%`, alors la base de données MASTER est incluse dans les résultats.

### <a name="b-more-complex-example"></a>B. Exemple plus complexe
 Cet exemple utilise l’opérateur _ pour rechercher toutes les personnes figurant dans la table `Person` qui ont un prénom en trois lettres se terminant par `an`.  
  
```sql  
-- USE AdventureWorks2012
  
SELECT FirstName, LastName  
FROM Person.Person  
WHERE FirstName LIKE '_an'  
ORDER BY FirstName;  
```  
## <a name="c-escaping-the-underscore-character"></a>C. Échappement du caractère de soulignement   
L’exemple suivant retourne les noms des rôles de base de données fixes comme `db_owner` et `db_ddladmin`, mais elle retourne également l’utilisateur `dbo`. 

```sql
SELECT name FROM sys.database_principals
WHERE name LIKE 'db_%';
```

Le trait de soulignement en troisième position de caractère est considéré comme un caractère générique et ne permet pas de filtrer seulement les principaux qui commencent par les lettres `db_`. Pour échapper le trait de soulignement, placez-le entre crochets `[_]`. 

```sql
SELECT name FROM sys.database_principals
WHERE name LIKE 'db[_]%';
```   
Maintenant, l’utilisateur `dbo` est exclu.   
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   
```
name
-------------
db_owner
db_accessadmin
db_securityadmin
...
```

  
## <a name="see-also"></a>Voir aussi  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
  [% (Caractère générique - Caractères à rechercher)](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)   
  [&#91; &#93; (Caractère générique - Caractères à rechercher)](../../t-sql/language-elements/wildcard-character-s-to-match-transact-sql.md)   
 [&#91;^&#93; (Caractère générique - Caractères à exclure)](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)     
  
