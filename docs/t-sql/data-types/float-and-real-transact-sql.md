---
description: float et real (Transact-SQL)
title: float et real (Transact-SQL)
ms.custom: ''
ms.date: 09/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- float
- real_TSQL
- real
- float_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- numeric data type, floating point
- float data type
- floating point data [SQL Server]
- real data type
ms.assetid: 08ea66b7-624e-4d8b-86bc-750ff76cdfc5
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4b7f4df0fd5efea427bf806543c3f397e6b1d1f7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185343"
---
# <a name="float-and-real-transact-sql"></a>float et real (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Types de données approximatives à utiliser avec des données numériques à virgule flottante. Les données à virgule flottante sont approximatives ; il n'est donc pas possible de représenter précisément toutes les valeurs de ce type de données. Le synonyme ISO de **real** est **float(24)** .
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
**float** [ **(** _n_ **)** ] Où *n* est le nombre de bits utilisés pour stocker la mantisse du nombre de type **float** en notation scientifique et indique par conséquent le niveau de précision et la taille de stockage. Si *n* est spécifié, sa valeur doit être comprise entre **1** et **53**. La valeur par défaut de *n* est **53**.
  
|Valeur *n*|Precision|Taille de stockage|  
|---|---|---|
|**1-24**|7 chiffres|4 octets|  
|**25-53**|15 chiffres|8 octets|  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considère *n* comme l’une des deux valeurs possibles. Si **1**<=n<=**24**, *n* est considéré comme égal à **24**. Si **25**<=n<=**53**, *n* est considéré comme égal à **53**.  
  
Le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]float **[** (n) **]**  est conforme à la norme ISO pour toutes les valeurs de *n* comprises entre **1** et **53**. Le synonyme de **double precision** est **float(53)** .

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes  
  
|Type de données|Plage|Stockage|  
|---|---|---|
|**float**|- 1,79E+308 à -2,23E-308, 0 et 2,23E-308 à 1,79E+308|Dépend de la valeur de *n*|  
|**real**|- 3,40E + 38 à -1,18E - 38, 0 et 1,18E - 38 à 3,40E + 38|Quatre octets|  
  
##  <a name="converting-float-and-real-data"></a>Conversion de données float et real  
Les valeurs de **float** sont tronquées quand elles sont converties en un type entier.
  
Si vous souhaitez effectuer une conversion de **float** ou **real** en données caractères, la fonction de chaîne STR constitue généralement un meilleur choix que CAST( ), car STR permet un plus grand contrôle sur le format. Pour plus d’informations, consultez [STR &#40;Transact-SQL&#41;](../../t-sql/functions/str-transact-sql.md) et [Fonctions &#40;Transact-SQL&#41;](../../t-sql/functions/functions.md).
  
Dans les versions antérieures à [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)], la conversion des valeurs **float** en valeurs **decimal** ou **numeric** est limitée à des valeurs d’une précision de 17 chiffres uniquement. Toutes les valeurs **float** inférieures à 5E-18 (quand elles sont définies avec la notation scientifique 5E-18 ou la notation décimale 0.0000000000000000050000000000000005) sont arrondies à 0. Cette limitation n’existe plus dans [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)].
  
## <a name="see-also"></a>Voir aussi
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Conversion de type de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)
  
  
