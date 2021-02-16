---
description: COLUMNPROPERTY (Transact-SQL)
title: COLUMNPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COLUMNPROPERTY
- COLUMNPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column properties [SQL Server]
- parameters [SQL Server], properties
- COLUMNPROPERTY function
ms.assetid: 2408c264-6eca-4120-bb71-df043c7c2792
author: cawrites
ms.author: chadam
ms.openlocfilehash: 914de62168c17f39f926eddade21d67085b52f39
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270659"
---
# <a name="columnproperty-transact-sql"></a>COLUMNPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction retourne des informations de colonne ou de paramètre.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
COLUMNPROPERTY ( id , column , property )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*id*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) contenant l’identificateur (ID) de la table ou de la procédure.
  
*column*  
Expression contenant le nom de la colonne ou du paramètre.
  
*property*  
Pour l’argument *id*, l’argument *property* spécifie le type des informations retournées par la fonction `COLUMNPROPERTY`. L’argument *property* peut avoir une des valeurs suivantes :
  
|Valeur|Description|Valeur retournée|  
|---|---|---|
|**AllowsNull**|Autorise les valeurs NULL|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**ColumnId**|Valeur d’identificateur de colonne correspondant à **sys.columns.column_id**.|ID de la colonne<br /><br /> **Remarque :** Lors de requêtes sur plusieurs colonnes, des écarts peuvent apparaître dans l’ordre des valeurs d’identificateur de colonne.|  
|**FullTextTypeColumn**|TYPE COLUMN de la table qui contient les informations sur le type de document de la *colonne*.|Identificateur de TYPE COLUMN en texte intégral pour l’expression du nom de colonne passée comme second paramètre de cette fonction.|  
|**GeneratedAlwaysType**|Valeur de la colonne générée par le système. Correspond à **sys.columns.generated_always_type**|**S’applique à** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures.<br /><br /> 0 : Toujours non générée<br /><br /> 1 : Toujours générée au début de ligne<br /><br /> 2 : Toujours générée en fin de ligne|  
|**IsColumnSet**|La colonne est un jeu de colonnes. Pour plus d’informations, consultez [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**IsComputed**|Est une colonne calculée.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**IsCursorType**|Le paramètre de la procédure est de type CURSOR.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**IsDeterministic**|La colonne est déterministe. Cette propriété s'applique uniquement aux colonnes calculées et aux colonnes de la vue.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte. Aucune colonne calculée ou colonne de la vue.|  
|**IsFulltextIndexed**|Colonne inscrite pour l’indexation en texte intégral.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**IsHidden**|Valeur de la colonne générée par le système. Correspond à **sys.columns.is_hidden**|**S’applique à** : [!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] et versions ultérieures.<br /><br /> 0 : Non masquée<br /><br /> 1 : Masquée|  
|**IsIdentity**|La colonne utilise la propriété IDENTITY.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**IsIdNotForRepl**|La colonne vérifie le paramètre IDENTITY_INSERT.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**IsIndexable**|La colonne peut être indexée.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**IsOutParam**|Le paramètre de la procédure est un paramètre de sortie.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**IsPrecise**|La colonne est précise. Cette propriété s'applique uniquement aux colonnes déterministes.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte. Colonne non déterministe.|  
|**IsRowGuidCol**|La colonne a le type de données **uniqueidentifier** et est définie avec la propriété ROWGUIDCOL.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**IsSparse**|La colonne est éparse. Pour plus d’informations, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**IsSystemVerified**|[!INCLUDE[ssDE](../../includes/ssde-md.md)] peut vérifier les propriétés de déterminisme et de précision de la colonne. Cette propriété s'applique uniquement aux colonnes calculées et aux colonnes de vues.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**IsXmlIndexable**|La colonne XML peut être utilisée dans un index XML.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**Précision**|Longueur du type de données de la colonne ou du paramètre.|Longueur du type de données de la colonne spécifiée.<br /><br /> -1 : **xml** ou types de grande valeur<br /><br /> NULL : entrée incorrecte.|  
|**Mise à l’échelle**|Échelle pour le type de données de la colonne ou du paramètre.|Valeur de l’échelle<br /><br /> NULL : entrée incorrecte.|  
|**StatisticalSemantics**|La colonne est activée en vue de l'indexation sémantique.|1 : TRUE<br /><br /> 0 : FALSE|  
|**SystemDataAccess**|La colonne est dérivée d'une fonction qui accède aux données dans les catalogues système ou les tables système virtuelles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette propriété s'applique uniquement aux colonnes calculées et aux colonnes de vues.|1 : TRUE (indique un accès en lecture seule)<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**UserDataAccess**|La colonne est dérivée d'une fonction qui accède aux données dans les tables utilisateur, y compris les vues et les tables temporaires, stockées dans l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette propriété s'applique uniquement aux colonnes calculées et aux colonnes de vues.|1 : TRUE (indique un accès en lecture seule)<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
|**UsesAnsiTrim**|ANSI_PADDING est défini sur ON lors de la création de la table. Cette propriété ne s’applique qu’aux colonnes ou paramètres de type **char** ou **varchar**.|1 : TRUE<br /><br /> 0 : FALSE<br /><br /> NULL : entrée incorrecte.|  
  
## <a name="return-types"></a>Types de retour
 **int**  
  
## <a name="exceptions"></a>Exceptions  
Retourne NULL en cas d’erreur ou si un appelant n’est pas autorisé à voir l’objet.
  
Un utilisateur peut voir uniquement les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d'une autorisation. Cela signifie que les fonctions intégrées générant des métadonnées, comme `COLUMNPROPERTY`, peuvent retourner NULL si l’utilisateur ne dispose pas de l’autorisation appropriée sur l’objet. Pour plus d’informations, consultez [Configuration de la visibilité des métadonnées](../../relational-databases/security/metadata-visibility-configuration.md).
  
## <a name="remarks"></a>Notes  
Quand vous vérifiez la propriété déterministe d’une colonne, testez d’abord si la colonne est une colonne calculée. L’argument **IsDeterministic** retourne NULL pour les colonnes non calculées. Les colonnes calculées peuvent être spécifiées sous la forme de colonnes d'index.
  
## <a name="examples"></a>Exemples  
L’exemple suivant retourne la longueur de la colonne `LastName`.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT COLUMNPROPERTY( OBJECT_ID('Person.Person'),'LastName','PRECISION')AS 'Column Length';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Column Length
-------------
50
```  
  
## <a name="see-also"></a>Voir aussi
[Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)  
[TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md)
  
  
