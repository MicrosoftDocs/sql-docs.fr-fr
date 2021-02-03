---
description: CONCAT (Transact-SQL)
title: CONCAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONCAT
- CONCAT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CONCAT function
ms.assetid: fce5a8d4-283b-4c47-95e5-4946402550d5
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 68589c27d999bdddc0998773f80dcf17c4dcfec8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188774"
---
# <a name="concat-transact-sql"></a>CONCAT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette fonction retourne une chaîne qui résulte de la concaténation ou de la jointure de deux valeurs de chaîne ou plus, de bout en bout. (Pour ajouter une valeur de séparation durant la concaténation, consultez [CONCAT_WS](../../t-sql/functions/concat-ws-transact-sql.md).)
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CONCAT ( string_value1, string_value2 [, string_valueN ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*string_value*  
Valeur de chaîne à concaténer aux autres valeurs. La fonction `CONCAT` nécessite au moins deux arguments *valeur_de_chaîne*, et pas plus de 254 arguments *valeur_de_chaîne*.
  
## <a name="return-types"></a>Types de retour  
*string_value*  
Valeur de chaîne dont la longueur et le type dépendent de l’entrée.
  
## <a name="remarks"></a>Notes  
`CONCAT` accepte un nombre variable d’arguments de chaîne et les concatène (ou les joint) en une seule chaîne. Elle nécessite un minimum de deux valeurs d’entrée ; sinon, `CONCAT` génère une erreur. `CONCAT` convertit implicitement tous les arguments en types chaîne avant la concaténation. `CONCAT` Convertit implicitement les valeurs null en chaînes vides. Si `CONCAT` reçoit des arguments dont toutes les valeurs sont **NULL**, elle retourne une chaîne vide de type **varchar**(1). La conversion implicite en chaînes respecte les règles existantes de conversion de type de données. Pour plus d’informations sur les conversions de type de données, consultez [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
Le type de retour dépend du type des arguments. Ce tableau illustre le mappage :
  
|Type d’entrée|Type de sortie et longueur|  
|---|---|
|1. N’importe quel argument de<br><br />un type système SQL-CLR<br><br />un type défini par l’utilisateur SQL-CLR<br><br />ou<br><br />`nvarchar(max)`|**nvarchar(max)**|  
|2. Sinon, n’importe quel argument de type<br><br />**varbinary(max)**<br><br />ou<br><br />**varchar(max)**|**varchar(max)** , à moins qu’un des paramètres soit de type **nvarchar** d’une longueur quelconque. Dans ce cas, `CONCAT` retourne un résultat de type **nvarchar(max)**.|  
|3. Sinon, n’importe quel argument de type **nvarchar** d’un maximum de 4 000 caractères.<br><br />(**nvarchar**(<= 4000))|**nvarchar**(<= 4000)|  
|4. Dans tous les autres cas|**varchar**(<= 8 000) (un **varchar** d’au plus 8 000 caractères), à moins qu’un des paramètres soit un nvarchar d’une longueur quelconque. Dans ce cas, `CONCAT` retourne un résultat de type **nvarchar(max)**.|  
  
Quand `CONCAT` reçoit des arguments d’entrée **nvarchar** d’une longueur <= 4 000 caractères ou des arguments d’entrée **varchar** d’une longueur <= 8 000 caractères, les conversions implicites peuvent affecter la longueur du résultat. Les autres types de données ont des longueurs différentes quand ils sont convertis implicitement en chaînes. Par exemple, une valeur **int** (14) a une longueur de chaîne de 12, alors qu’une valeur **float** a une longueur de 32. Par conséquent, une concaténation de deux entiers retourne un résultat d’une longueur au moins égale à 24.
  
Si aucun des arguments d’entrée n’est d’un type Large Object (LOB) pris en charge, le type de retour est tronqué à une longueur de 8 000 caractères, quel que soit le type de retour. Cette troncation préserve les espaces et prend en charge l’efficacité de la génération du plan.
  
La fonction CONCAT peut être exécutée à distance sur un serveur lié dont la version est [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou ultérieur. Pour les serveurs liés plus anciens, l’opération CONCAT est exécutée localement après que le serveur lié a retourné les valeurs non concaténées.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-concat"></a>R. Utilisation de CONCAT  
  
```sql
SELECT CONCAT ( 'Happy ', 'Birthday ', 11, '/', '25' ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
-------------------------  
Happy Birthday 11/25  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-concat-with-null-values"></a>B. Utilisation de CONCAT avec des valeurs NULL  
  
```sql
CREATE TABLE #temp (  
    emp_name NVARCHAR(200) NOT NULL,  
    emp_middlename NVARCHAR(200) NULL,  
    emp_lastname NVARCHAR(200) NOT NULL  
);  
INSERT INTO #temp VALUES( 'Name', NULL, 'Lastname' );  
SELECT CONCAT( emp_name, emp_middlename, emp_lastname ) AS Result  
FROM #temp;  
```  

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
------------------  
NameLastname  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)   
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  


