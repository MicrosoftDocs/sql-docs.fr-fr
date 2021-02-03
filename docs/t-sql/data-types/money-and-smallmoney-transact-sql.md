---
description: money et smallmoney (Transact-SQL)
title: money et smallmoney (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- money_TSQL
- money
- smallmoney
- smallmoney_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- money data type, about money data type
- money data type
- smallmoney data type
- values [SQL Server], monetary
- currency [SQL Server]
ms.assetid: 57861137-89ea-4b89-b361-390597d7bccc
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 58ae2f3edcee325330864bbcb91ef210fe9c981b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165130"
---
# <a name="money-and-smallmoney-transact-sql"></a>money et smallmoney (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Types de données représentant des valeurs monétaires ou des devises.
  
## <a name="remarks"></a>Notes  
  
|Type de données|Plage|Stockage|  
|---|---|---|
|**money**|-922 337 203 685 477,5808 à 922 337 203 685 477,5807 (-922 337 203 685 477,58<br />à 922 337 203 685 477,58 pour Informatica.  Informatica prend uniquement en charge deux décimales, et non quatre.)|8 octets|  
|**smallmoney**|-214 748,3648 à 214 748,3647|4 octets|  
  
Les types de données **money** et **smallmoney** sont précis à un dix millième près des unités monétaires qu’ils représentent. Pour Informatica, les types de données **money** et **smallmoney** sont précis à un centième près des unités monétaires qu’ils représentent.
  
Utilisez un point pour séparer les unités monétaires partielles, comme les centimes, des unités monétaires entières. Par exemple, 2.15 signifie 2 dollars et 15 cents.
  
Ces types de données peuvent utiliser n'importe lequel des symboles monétaires suivants.
  
![Table de symboles monétaires, valeurs hexadécimales](../../t-sql/data-types/media/money01.gif "Table de symboles monétaires, valeurs hexadécimales")
  
Les valeurs de devise ou les données monétaires ne doivent pas être mises entre guillemets simples ('). Il est important de garder en mémoire que bien que vous puissiez spécifier des valeurs monétaires précédées d'un symbole de devise, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne stocke pas les informations de devise associées au symbole, mais uniquement la valeur numérique.
  
## <a name="converting-money-data"></a>Conversion de données money
Lors de la conversion de types de données integer en **money**, les unités sont supposées être des unités monétaires. Par exemple, la valeur entière 4 est convertie en équivalent **money** de 4 unités monétaires.
  
L’exemple suivant convertit les valeurs **smallmoney** et **money** respectivement en types de données **varchar** et **decimal**.
  
```sql
DECLARE @mymoney_sm SMALLMONEY = 3148.29,  
        @mymoney    MONEY = 3148.29;  
SELECT  CAST(@mymoney_sm AS VARCHAR) AS 'SM_MONEY varchar',  
        CAST(@mymoney AS DECIMAL)    AS 'MONEY DECIMAL';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
SM_MONEY VARCHAR               MONEY DECIMAL  
------------------------------ ----------------------  
3148.29                        3148    
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)
[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
  
