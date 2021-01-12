---
description: JSON_MODIFY (Transact-SQL)
title: JSON_MODIFY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
ms.assetid: 96bc8255-a037-4907-aec4-1a9c30814651
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 547a304b4f5a9b6069d023a751f73d99e8456bbc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092227"
---
# <a name="json_modify-transact-sql"></a>JSON_MODIFY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Met à jour la valeur d’une propriété dans une chaîne JSON et renvoie la chaîne JSON mise à jour.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
JSON_MODIFY ( expression , path , newValue )  
```  
  
## <a name="arguments"></a>Arguments

 *expression*  
 Expression. En règle générale, nom d’une variable ou d’une colonne qui contient du texte JSON.  
  
 **JSON_MODIFY** retourne une erreur si *expression* ne contient pas de code JSON valide.  
  
 *path*  
 Expression de chemin JSON qui spécifie la propriété à mettre à jour.

 La syntaxe de *path* est la suivante :  
  
 `[append] [ lax | strict ] $.<json path>`  
  
- *append*  
    Modificateur facultatif qui spécifie que la nouvelle valeur doit être ajoutée au tableau référencé par *\<json path>* .  
  
- *lax*  
    Spécifie que la propriété référencée par *\<json path>* ne doit pas nécessairement exister. Si la propriété n’est pas présente, JSON_MODIFY tente d’insérer la nouvelle valeur dans le chemin spécifié. L’insertion peut échouer si la propriété ne peut pas être insérée dans le chemin. Si vous ne spécifiez pas *lax* ou *strict*, *lax* est le mode par défaut.  
  
- *strict*  
    Spécifie que la propriété référencée par *\<json path>* doit être dans l’expression JSON. Si la propriété n’est pas présente, JSON_MODIFY renvoie une erreur.  
  
- *\<json path>*  
    Spécifie le chemin de la propriété à mettre à jour. Pour plus d’informations, consultez [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
Dans [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)], vous pouvez fournir une variable comme valeur de *path*.

**JSON_MODIFY** renvoie une erreur si le format de *path* n’est pas valide.  
  
 *newValue*  
 Nouvelle valeur de la propriété spécifiée par *path*.  
 La nouvelle valeur doit être de type [n]varchar ou text.
  
 En mode lax, JSON_MODIFY supprime la clé spécifiée si la nouvelle valeur est NULL.  
  
JSON_MODIFY échappe tous les caractères spéciaux dans la nouvelle valeur si le type de la valeur est NVARCHAR ou VARCHAR. Une valeur texte n’est pas échappée s’il s’agit de JSON correctement formaté produit par FOR JSON, JSON_QUERY ou JSON_MODIFY.  
  
## <a name="return-value"></a>Valeur de retour

 Retourne la valeur mise à jour de *expression* sous forme de texte JSON correctement formaté.  
  
## <a name="remarks"></a>Notes

 La fonction JSON_MODIFY vous permet de mettre à jour la valeur d’une propriété existante, d’insérer une nouvelle paire clé-valeur ou de supprimer une clé en fonction d’une combinaison de modes et de valeurs fournies.  
  
 Le tableau suivant compare le comportement de **JSON_MODIFY** en mode lax et en mode strict. Pour plus d’informations sur la spécification du mode de chemin facultatif (lax ou strict), consultez [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
|Valeur existante|Chemin existant|Mode lax|Mode strict|  
|--------------------|-----------------|--------------|-----------------|  
|Non Null|Oui|Mettre à jour la valeur existante.|Mettre à jour la valeur existante.|  
|Non Null|Non|Essayer de créer une paire clé-valeur dans le chemin spécifié.<br /><br /> Échec possible. Par exemple, si vous spécifiez le chemin `$.user.setting.theme`, JSON_MODIFY n’insère pas la clé `theme` si les objets `$.user` ou `$.user.settings` n’existent pas, ou bien si les paramètres sont un tableau ou une valeur scalaire.|Erreur : INVALID_PROPERTY|  
|NULL|Oui|Supprimer la propriété existante.|Affecter à la valeur existante la valeur Null.|  
|NULL|Non|Aucune action. Le premier argument est retourné en tant que résultat.|Erreur : INVALID_PROPERTY|  
  
 En mode lax, JSON_MODIFY tente de créer une nouvelle paire clé-valeur, mais dans certains cas, cela peut échouer.  
  
## <a name="examples"></a>Exemples  
  
### <a name="example---basic-operations"></a>Exemple : opérations de base

 L’exemple suivant montre des opérations de base réalisables avec du texte JSON.  
  
 **Requête**
  
```sql

DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update name  

SET @info=JSON_MODIFY(@info,'$.name','Mike')

PRINT @info

-- Insert surname  

SET @info=JSON_MODIFY(@info,'$.surname','Smith')

PRINT @info

-- Delete name  

SET @info=JSON_MODIFY(@info,'$.name',NULL)

PRINT @info

-- Add skill  

SET @info=JSON_MODIFY(@info,'append $.skills','Azure')

PRINT @info
```  
  
 **Résultats**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL"],
    "surname": "Smith"
} {
    "skills": ["C#", "SQL"],
    "surname": "Smith"
} {
    "skills": ["C#", "SQL", "Azure"],
    "surname": "Smith"
}
```  
  
### <a name="example---multiple-updates"></a>Exemple : plusieurs mises à jour

 Avec JSON_MODIFY, vous pouvez mettre à jour une seule propriété. Si vous devez effectuer plusieurs mises à jour, vous pouvez utiliser plusieurs appels JSON_MODIFY.  
  
 **Requête**
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Multiple updates  

SET @info=JSON_MODIFY(JSON_MODIFY(JSON_MODIFY(@info,'$.name','Mike'),'$.surname','Smith'),'append $.skills','Azure')

PRINT @info
```  
  
 **Résultats**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL", "Azure"],
    "surname": "Smith"
}
```  
  
### <a name="example---rename-a-key"></a>Exemple : renommer une clé  
 L’exemple suivant montre comment renommer une propriété dans du texte JSON avec la fonction JSON_MODIFY. Tout d’abord, vous pouvez prendre la valeur d’une propriété existante et l’insérer en tant que nouvelle paire clé-valeur. Ensuite, vous pouvez supprimer l’ancienne clé en affectant à la valeur de l’ancienne propriété la valeur NULL.  
  
 **Requête**
  
```sql  
DECLARE @product NVARCHAR(100)='{"price":49.99}'

PRINT @product

-- Rename property  

SET @product=
 JSON_MODIFY(
  JSON_MODIFY(@product,'$.Price',CAST(JSON_VALUE(@product,'$.price') AS NUMERIC(4,2))),
  '$.price',
  NULL
 )

PRINT @product
```  
  
 **Résultats**
  
```json  
{
    "price": 49.99
} {
    "Price": 49.99
}
```  
  
 Si vous ne castez pas la nouvelle valeur en type numérique, JSON_MODIFY la traite comme du texte et la met entre guillemets doubles.  
  
### <a name="example---increment-a-value"></a>Exemple : incrémenter une valeur

 L’exemple suivant montre comment incrémenter la valeur d’une propriété dans du texte JSON avec la fonction JSON_MODIFY. Tout d’abord, vous pouvez prendre la valeur de la propriété existante et l’insérer en tant que nouvelle paire clé-valeur. Ensuite, vous pouvez supprimer l’ancienne clé en affectant à la valeur de l’ancienne propriété la valeur NULL.  
  
 **Requête**
  
```sql  
DECLARE @stats NVARCHAR(100)='{"click_count": 173}'

PRINT @stats

-- Increment value  

SET @stats=JSON_MODIFY(@stats,'$.click_count',
 CAST(JSON_VALUE(@stats,'$.click_count') AS INT)+1)

PRINT @stats
```  
  
 **Résultats**
  
```json  
{
    "click_count": 173
} {
    "click_count": 174
}
```  
  
### <a name="example---modify-a-json-object"></a>Exemple : modifier un objet JSON

 JSON_MODIFY traite l’argument *newValue* comme du texte brut même s’il contient du texte JSON correctement formaté. Par conséquent, la sortie JSON de la fonction est mise entre guillemets doubles et tous les caractères spéciaux sont échappés, comme le montre l’exemple suivant.  
  
 **Requête**  
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update skills array

SET @info=JSON_MODIFY(@info,'$.skills','["C#","T-SQL","Azure"]')

PRINT @info
```  
  
 **Résultats**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "John",
    "skills": "["C#","T-SQL","Azure"]"
}
```  
  
 Pour éviter un échappement automatique, fournissez *newValue* en utilisant la fonction JSON_QUERY. JSON_MODIFY sait que la valeur renvoyée par JSON_MODIFY est du JSON correctement formaté, donc la valeur n’est pas échappée.  
  
 **Requête**  
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update skills array  

SET @info=JSON_MODIFY(@info,'$.skills',JSON_QUERY('["C#","T-SQL","Azure"]'))

PRINT @info
```  
  
 **Résultats**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "John",
    "skills": ["C#", "T-SQL", "Azure"]
}
```  
  
### <a name="example---update-a-json-column"></a>Exemple : mettre à jour une colonne JSON

 L’exemple suivant met à jour la valeur d’une propriété dans une colonne de table contenant du texte JSON.  
  
```sql  
UPDATE Employee
SET jsonCol=JSON_MODIFY(jsonCol,'$.info.address.town','London')
WHERE EmployeeID=17
```  
  
## <a name="see-also"></a>Voir aussi

- [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Données JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
  
