---
description: SQL_VARIANT_PROPERTY (Transact-SQL)
title: SQL_VARIANT_PROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL_VARIANT_PROPERTY_TSQL
- SQL_VARIANT_PROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- SQL_VARIANT_PROPERTY function
- sql_variant data type
ms.assetid: 50e5c1d9-4e95-4ed0-9c92-435c872a399e
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 00510ffb5bda7fd19b0806dd7a7b2dc1373c8354
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198298"
---
# <a name="sql_variant_property-transact-sql"></a>SQL_VARIANT_PROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne le type de données de base et d’autres informations sur une valeur **sql_variant**.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
SQL_VARIANT_PROPERTY ( expression , property )  
```  
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression*  
 Expression de type **sql_variant**.  
  
 *property*  
 Contient le nom de la propriété **sql_variant** dont les informations doivent être fournies. *property* est de type **varchar(** 128 **)** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|Type de base sql_variant renvoyé|  
|-----------|-----------------|----------------------------------------|  
|**BaseType**|Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tel que :<br /><br /> **bigint**<br /><br /> **binary**<br /><br /> **bit**<br /><br /> **char**<br /><br /> **date**<br /><br /> **datetime**<br /><br /> **datetime2**<br /><br /> **datetimeoffset**<br /><br /> **decimal**<br /><br /> **float**<br /><br /> **int**<br /><br /> **money**<br /><br /> **nchar**<br /><br /> **numeric**<br /><br /> **nvarchar**<br /><br /> **real**<br /><br /> **smalldatetime**<br /><br /> **smallint**<br /><br /> **smallmoney**<br /><br /> **time**<br /><br /> **tinyint**<br /><br /> **uniqueidentifier**<br /><br /> **varbinary**<br /><br /> **varchar**|**sysname**<br /><br /> NULL = Entrée non valide.|  
|**Précision**|Nombre de chiffres du type de données numériques de base :<br /><br /> **date** = 10<br /><br /> **datetime** = 23<br /><br /> **datetime2** = 27<br /><br /> **datetime2** (s) = 19 quand s = 0, sinon s + 20<br /><br /> **datetimeoffset** = 34<br /><br /> **datetimeoffset** (s) = 26 quand s = 0, sinon s + 27<br /><br /> **smalldatetime** = 16<br /><br /> **time** = 16<br /><br /> **time** (s) = 8 quand s = 0, sinon s + 9<br /><br /> **float** = 53<br /><br /> **real** = 24<br /><br /> **decimal** et **numeric** = 18<br /><br /> **decimal** (p,s) et **numeric** (p,s) = p<br /><br /> **money** = 19<br /><br /> **smallmoney** = 10<br /><br /> **bigint** = 19<br /><br /> **int** = 10<br /><br /> **smallint** = 5<br /><br /> **tinyint** = 3<br /><br /> **bit** = 1<br /><br /> Tous les autres types = 0|**int**<br /><br /> NULL = Entrée non valide.|  
|**Mettre à l'échelle**|Nombre de chiffres décimaux après la virgule (point) dans le type de données numériques de base :<br /><br /> **decimal** et **numeric** = 0<br /><br /> **decimal** (p,s) et **numeric** (p,s) = s<br /><br /> **money** et **smallmoney** = 4<br /><br /> **datetime** = 3<br /><br /> **datetime2** = 7<br /><br /> **datetime2** (s) = s (0 - 7)<br /><br /> **datetimeoffset** = 7<br /><br /> **datetimeoffset** (s) = s (0 - 7)<br /><br /> **time** = 7<br /><br /> **time** (s) = s (0 - 7)<br /><br /> Tous les autres types = 0|**int**<br /><br /> NULL = Entrée non valide.|  
|**TotalBytes**|Nombre d'octets requis pour conserver les métadonnées et les données de la valeur. Ces informations permettent de vérifier la taille maximale des données dans une colonne **sql_variant**. Si cette valeur est supérieure à 900, la création de l’index échoue.|**int**<br /><br /> NULL = Entrée non valide.|  
|**Classement**|Représente le classement de la valeur **sql_variant** particulière.|**sysname**<br /><br /> NULL = Entrée non valide.|  
|**MaxLength**|Longueur maximale du type de données (en octets). Par exemple, **MaxLength** de **nvarchar(** 50 **)** est 100, **MaxLength** de **int** est 4.|**int**<br /><br /> NULL = Entrée non valide.|  
  
## <a name="return-types"></a>Types de retour  
 **sql_variant**  
  
## <a name="examples"></a>Exemples  
### <a name="a-using-a-sql_variant-in-a-table"></a>R. Utilisation d’un type sql_variant dans une table  
 L’exemple suivant extrait des informations `SQL_VARIANT_PROPERTY` relatives à la valeur `colA``46279.1` où `colB` =`1689`, étant donné que `tableA` a la valeur `colA` de type `sql_variant` et `colB`.  
  
```sql    
CREATE   TABLE tableA(colA sql_variant, colB int)  
INSERT INTO tableA values ( cast (46279.1 as decimal(8,2)), 1689)  
SELECT   SQL_VARIANT_PROPERTY(colA,'BaseType') AS 'Base Type',  
         SQL_VARIANT_PROPERTY(colA,'Precision') AS 'Precision',  
         SQL_VARIANT_PROPERTY(colA,'Scale') AS 'Scale'  
FROM      tableA  
WHERE      colB = 1689  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Notez que chacune de ces trois valeurs est de type **sql_variant**.  
  
```  
Base Type    Precision    Scale  
---------    ---------    -----  
decimal      8           2  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-a-sql_variant-as-a-variable"></a>B. Utilisation d’un type sql_variant comme variable   
 L’exemple suivant extrait les informations `SQL_VARIANT_PROPERTY` relatives à une variable nommée @v1.  
  
```sql    
DECLARE @v1 sql_variant;  
SET @v1 = 'ABC';  
SELECT @v1;  
SELECT SQL_VARIANT_PROPERTY(@v1, 'BaseType');  
SELECT SQL_VARIANT_PROPERTY(@v1, 'MaxLength');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)  
  
  

