---
description: sql_variant (Transact-SQL)
title: sql_variant (Transact-SQL)
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- sql_variant
- sql_variant_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sql_variant comparisons
- storing data [SQL Server], sql_variant
- sql_variant data type
- storage [SQL Server], sql_variant
ms.assetid: 01229779-8bc1-4c7d-890a-8246d4899250
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 3787a49e6488a2b43ba8ef6e7b67c2a94b6347b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179698"
---
# <a name="sql_variant-transact-sql"></a>sql_variant (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Type de données qui stocke les valeurs de divers types de données pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
sql_variant  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes  
**sql_variant** peut être utilisé dans les colonnes, paramètres, variables et valeurs de retour des fonctions définies par l’utilisateur. **sql_variant** permet à ces objets de base de données de prendre en charge les valeurs des autres types de données.
  
Une colonne de type **sql_variant** peut contenir des lignes de types de données différents. Par exemple, une colonne définie en tant que **sql_variant** peut stocker des valeurs **int**, **binary** et **char**.
  
**sql_variant** peut avoir une longueur maximale de 8 016 octets. Cela inclut les informations du type de base et la valeur du type de base. La longueur maximale de la valeur de type de base réelle est de 8 000 octets.
  
Un type de données **sql_variant** doit d’abord être converti en sa valeur de base avant d’être utilisé dans des opérations, notamment l’addition et la soustraction.
  
Il est possible d’attribuer une valeur par défaut à **sql_variant**. Ce type de données accepte également les valeurs NULL comme valeurs sous-jacentes, mais ces dernières ne seront pas associées à un type de base. En outre, **sql_variant** ne peut pas avoir un autre type **sql_variant** comme type de base.
  
Une clé unique, primaire ou étrangère peut inclure des colonnes de type **sql_variant**, mais la longueur totale des valeurs de données qui composent la clé d’une ligne spécifique ne doit pas être supérieure à la longueur maximale d’un index. Cette longueur est de 900 octets.
  
Une table peut inclure n’importe quel nombre de colonnes **sql_variant**.
  
**sql_variant** ne peut pas être utilisé dans les instructions CONTAINSTABLE ni FREETEXTTABLE.
  
ODBC ne prend pas pleinement en charge le type **sql_variant**. Par conséquent, les requêtes des colonnes **sql_variant** sont retournées sous la forme de données binaires quand vous utilisez le fournisseur Microsoft OLE DB pour ODBC (MSDASQL). Par exemple, une colonne **sql_variant** contenant les données de chaîne de caractères « PS2091 » est retournée sous la forme 0x505332303931.
  
## <a name="comparing-sql_variant-values"></a>Comparaison des valeurs sql_variant  
Le type de données **sql_variant** est situé tout en haut de la hiérarchie des types de données pour la conversion. Pour les comparaisons **sql_variant**, la hiérarchie des types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est ordonnée en familles.
  
|Hiérarchie des types de données|Famille de types de données|  
|---|---|
|**sql_variant**|sql_variant |  
|**datetime2**|Date et heure|  
|**datetimeoffset**|Date et heure|  
|**datetime**|Date et heure|  
|**smalldatetime**|Date et heure|  
|**date**|Date et heure|  
|**time**|Date et heure|  
|**float**|Valeur numérique approchée|  
|**real**|Valeur numérique approchée|  
|**decimal**|Valeur numérique exacte|  
|**money**|Valeur numérique exacte|  
|**smallmoney**|Valeur numérique exacte|  
|**bigint**|Valeur numérique exacte|  
|**int**|Valeur numérique exacte|  
|**smallint**|Valeur numérique exacte|  
|**tinyint**|Valeur numérique exacte|  
|**bit**|Valeur numérique exacte|  
|**nvarchar**|Unicode|  
|**nchar**|Unicode|  
|**varchar**|Unicode|  
|**char**|Unicode|  
|**varbinary**|Binary|  
|**binary**|Binary|  
|**uniqueidentifier**|Uniqueidentifier |  
  
Les règles suivantes s’appliquent aux comparaisons **sql_variant** :
-   Lors de la comparaison des valeurs **sql_variant** issues de différents types de données de base appartenant à des familles de types de données différentes, la valeur de la famille dont le rang est supérieur dans la hiérarchie est considérée comme la valeur la plus élevée des deux.  
-   Lors de la comparaison des valeurs **sql_variant** issues de différents types de données de base appartenant à la même famille, la valeur du type dont le rang est inférieur dans la hiérarchie est implicitement convertie en l’autre type de données et la comparaison est alors effectuée.  
-   Quand les valeurs **sql_variant** des types de données **char**, **varchar**, **nchar** ou **nvarchar** sont comparées, leurs classements sont d’abord comparés d’après les critères suivants : LCID, version LCID, indicateurs de comparaison et ID de tri. Chacun de ces critères est comparé en tant que valeur entières, dans l'ordre indiqué. Si tous ces critères sont égaux, les valeurs de chaîne réelles sont comparées d'après le classement.  
  
## <a name="converting-sql_variant-data"></a>Conversion de données sql_variant  
Lors de la gestion du type de données **sql_variant**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge les conversions implicites d’objets avec d’autres types de données en type **sql_variant**. Cependant, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge les conversions implicites de données **sql_variant** en un objet possédant un autre type de données.
  
## <a name="restrictions"></a>Restrictions

Voici les types de valeurs qui ne peuvent pas être stockées en utilisant **sql_variant** :

- **datetimeoffset**<sup>1</sup>
- **Geography**
- **geometry**
- **hierarchyid**
- **image**
- **ntext**
- **nvarchar(max)**
- **rowversion** (**timestamp**)
- **text**
- **varchar(max)**
- **varbinary(max)**
- **sql_variant**
- Types définis par l'utilisateur
- **xml**

<sup>1</sup> SQL Server versions 2012 et supérieures ne restreignent pas **datetimeoffset**.

## <a name="examples"></a>Exemples  

### <a name="a-using-a-sql_variant-in-a-table"></a>R. Utilisation d’un type sql_variant dans une table  
 L’exemple suivant crée une table avec un type de données sql_variant. L’exemple récupère ensuite des informations `SQL_VARIANT_PROPERTY` relatives à la valeur `colA``46279.1` où `colB` =`1689`, étant donné que `tableA` a la valeur `colA` de type `sql_variant` et `colB`.  
  
```sql    
CREATE TABLE tableA(colA sql_variant, colB INT)  
INSERT INTO tableA values ( CAST(46279.1 as decimal(8,2)), 1689)  
SELECT   SQL_VARIANT_PROPERTY(colA,'BaseType') AS 'Base Type',  
         SQL_VARIANT_PROPERTY(colA,'Precision') AS 'Precision',  
         SQL_VARIANT_PROPERTY(colA,'Scale') AS 'Scale'  
FROM      tableA  
WHERE     colB = 1689  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Notez que chacune de ces trois valeurs est de type **sql_variant**.  
  
```  
Base Type    Precision    Scale  
---------    ---------    -----  
decimal      8           2  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-a-sql_variant-as-a-variable"></a>B. Utilisation d’un type sql_variant comme variable   
 L’exemple suivant crée une variable avec le type de données sql_variant et récupère ensuite des informations `SQL_VARIANT_PROPERTY` sur une variable nommée @v1.  
  
```sql    
DECLARE @v1 sql_variant;  
SET @v1 = 'ABC';  
SELECT @v1;  
SELECT SQL_VARIANT_PROPERTY(@v1, 'BaseType');  
SELECT SQL_VARIANT_PROPERTY(@v1, 'MaxLength');  
```    


## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[SQL_VARIANT_PROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sql-variant-property-transact-sql.md)
  
  
