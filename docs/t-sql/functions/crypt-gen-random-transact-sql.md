---
description: CRYPT_GEN_RANDOM (Transact-SQL)
title: CRYPT_GEN_RANDOM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CRYPT_GEN_RANDOM_TSQL
- CRYPT_GEN_RANDOM
dev_langs:
- TSQL
helpviewer_keywords:
- CRYPT_GEN_RANDOM function
ms.assetid: b74bd9d4-758e-4b94-89a0-76dcda6d8c42
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 7c0381c2348174e5d3663d0b38a44714a96df621
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184155"
---
# <a name="crypt_gen_random-transact-sql"></a>CRYPT_GEN_RANDOM (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction retourne un nombre aléatoire de chiffrement généré par l'API Crypto (CAPI). `CRYPT_GEN_RANDOM` retourne un nombre hexadécimal avec une longueur d’un nombre spécifié d’octets.
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CRYPT_GEN_RANDOM ( length [ , seed ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*length*  
La longueur, en octets, du nombre que `CRYPT_GEN_RANDOM` va créer. L’argument *length* comporte un type de données **int** et une plage de valeurs comprises entre 1 et 8000. `CRYPT_GEN_RANDOM` retourne la valeur NULL pour une valeur **int** en dehors de cette plage. 
  
*seed*  
Un nombre hexadécimal facultatif à utiliser comme valeur de départ aléatoire. La longueur de la valeur *seed* doit correspondre à la valeur de l’argument *length*. L’argument *seed* comporte un type de données **varbinary(8000)**.
  
## <a name="returned-types"></a>Types retournés  
**varbinary(8000)**
  
## <a name="permissions"></a>Autorisations  
Cette fonction est publique et ne requiert pas d'autorisation spéciale.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-generating-a-random-number"></a>R. Génération d'un nombre aléatoire  
Cet exemple génère un nombre aléatoire d’une longueur de 50 octets :
  
```sql
SELECT CRYPT_GEN_RANDOM(50) ;  
```  
  
Cet exemple génère un nombre aléatoire d'une longueur de 4 octets à l'aide d'une valeur de départ de 4 octets :
  
```sql
SELECT CRYPT_GEN_RANDOM(4, 0x25F18060) ;  
```  
  
## <a name="see-also"></a>Voir aussi
[RAND &#40;Transact-SQL&#41;](../../t-sql/functions/rand-transact-sql.md)
  
  
