---
description: JSON_QUERY (Transact-SQL)
title: JSON_QUERY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- JSON_QUERY
- JSON_QUERY_TSQL
helpviewer_keywords:
- JSON, extracting
- JSON, querying
- JSON_QUERY function
ms.assetid: 1ab0d90f-19b6-4988-ab4f-22fdf28b7c79
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: f5f81325f27a37f1e6d30ea7c8fb6bdb5dad096b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100337984"
---
# <a name="json_query-transact-sql"></a>JSON_QUERY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

 Extrait un objet ou un tableau à partir d’une chaîne JSON.  
  
 Pour extraire une valeur scalaire à partir d’une chaîne JSON à la place d’un objet ou d’un tableau, consultez [JSON_VALUE &#40;Transact-SQL&#41;](../../t-sql/functions/json-value-transact-sql.md). Pour obtenir des informations sur les différences entre **JSON_VALUE** et **JSON_QUERY**, consultez [Comparer JSON_VALUE et JSON_QUERY](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md#JSONCompare).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
JSON_QUERY ( expression [ , path ] )  
```  
  
## <a name="arguments"></a>Arguments

 *expression*  
 Expression. En règle générale, nom d’une variable ou d’une colonne qui contient du texte JSON.  
  
 Si **JSON_QUERY** trouve des données JSON non valides dans *expression* avant de trouver la valeur identifiée par *path*, la fonction renvoie une erreur. Si **JSON_QUERY** ne trouve pas la valeur identifiée par *path*, elle analyse l’intégralité du texte et renvoie une erreur si elle trouve des données JSON non valides, n’importe où dans *expression*.  
  
 *path*  
 Chemin JSON qui spécifie l’objet ou le tableau à extraire.

Dans [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] et [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)], vous pouvez fournir une variable comme valeur de *path*.

Le chemin JSON peut spécifier le mode lax ou strict pour l’analyse. Si vous ne spécifiez pas le mode d’analyse, le mode lax est utilisé par défaut. Pour plus d’informations, consultez [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  

La valeur par défaut de *path* est '$'. Par conséquent, si vous ne fournissez pas de valeur pour *path*, **JSON_QUERY** renvoie l’entrée *expression*.

Si le format de *path* n’est pas valide, **JSON_QUERY** renvoie une erreur.  
  
## <a name="return-value"></a>Valeur retournée

 Renvoie un fragment JSON de type nvarchar(max). Le classement de la valeur renvoyée est le même que le classement de l’expression d’entrée.  
  
 Si la valeur n’est pas un objet ni un tableau :  
  
- En mode lax, **JSON_QUERY** renvoie la valeur Null.  
  
- En mode strict, **JSON_QUERY** renvoie une erreur.  
  
## <a name="remarks"></a>Notes  

### <a name="lax-mode-and-strict-mode"></a>Mode lax et mode strict

 Considérons le texte JSON suivant :  
  
```json  
{
   "info": {
      "type": 1,
      "address": {
         "town": "Bristol",
         "county": "Avon",
         "country": "England"
      },
      "tags": ["Sport", "Water polo"]
   },
   "type": "Basic"
} 
```  
  
 Le tableau suivant compare le comportement de **JSON_QUERY** en mode lax et en mode strict. Pour plus d’informations sur la spécification du mode de chemin facultatif (lax ou strict), consultez [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
|Path|Valeur renvoyée en mode lax|Valeur renvoyée en mode strict|En savoir plus|  
|----------|------------------------------|---------------------------------|---------------|  
|$|Renvoie l’intégralité du texte JSON.|Renvoie l’intégralité du texte JSON.|n/a|  
|$.info.type|NULL|Error|Ni un objet, ni un tableau.<br /><br /> Utilisez **JSON_VALUE** à la place.|  
|$.info.address.town|NULL|Error|Ni un objet, ni un tableau.<br /><br /> Utilisez **JSON_VALUE** à la place.|  
|$.info."address"|N'{ "town":"Bristol", "county":"Avon", "country":"England" }'|N'{ "town":"Bristol", "county":"Avon", "country":"England" }'|n/a|  
|$.info.tags|N'[ "Sport", "Water polo"]'|N'[ "Sport", "Water polo"]'|n/a|  
|$.info.type[0]|NULL|Error|Pas un tableau.|  
|$.info.none|NULL|Error|La propriété n’existe pas.|  

### <a name="using-json_query-with-for-json"></a>Utilisation de JSON_QUERY avec FOR JSON

**JSON_QUERY** renvoie un fragment JSON valide. Par conséquent, **FOR JSON** n’échappe pas les caractères spéciaux dans la valeur renvoyée **JSON_QUERY**.

Si vous renvoyez les résultats avec FOR JSON, et que vous insérez des données qui sont déjà au format JSON (dans une colonne ou comme résultat d’une expression), incluez dans un wrapper les données JSON avec **JSON_QUERY** sans le paramètre *path*.

## <a name="examples"></a>Exemples  
  
### <a name="example-1"></a>Exemple 1

 L’exemple suivant montre comment renvoyer un fragment JSON à partir d’une colonne `CustomFields` dans les résultats de la requête.  
  
```sql  
SELECT PersonID,FullName,
  JSON_QUERY(CustomFields,'$.OtherLanguages') AS Languages
FROM Application.People
```  
  
### <a name="example-2"></a>Exemple 2

L’exemple suivant montre comment inclure les fragments JSON dans la sortie de la clause FOR JSON.  
  
```sql  
SELECT StockItemID, StockItemName,
         JSON_QUERY(Tags) as Tags,
         JSON_QUERY(CONCAT('["',ValidFrom,'","',ValidTo,'"]')) ValidityPeriod
FROM Warehouse.StockItems
FOR JSON PATH
```  
  
## <a name="see-also"></a>Voir aussi

- [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Données JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
