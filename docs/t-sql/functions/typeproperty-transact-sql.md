---
description: TYPEPROPERTY (Transact-SQL)
title: TYPEPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TYPEPROPERTY
- TYPEPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], data types
- data types [SQL Server], status information
- TYPEPROPERTY function
ms.assetid: bc311c80-bac5-46ab-a5c8-68b1c6bbf24a
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d7dc5c9a41e3ed7591cb95b90d78709d6bdf4d28
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210687"
---
# <a name="typeproperty-transact-sql"></a>TYPEPROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie des informations sur un type de données.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
TYPEPROPERTY (type , property)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *type*  
 Nom du type de données.  
  
 *property*  
 Type d'informations à renvoyer pour le type de données. *property* peut avoir l’une des valeurs suivantes.  
  
|Propriété|Description|Valeur retournée|  
|--------------|-----------------|--------------------|  
|**AllowsNull**|Type de données autorisant les valeurs NULL.|1 = Vrai<br /><br /> 0 = Faux<br /><br /> NULL = Type de données introuvable.|  
|**OwnerId**|Propriétaire du type.<br /><br /> Remarque : Le propriétaire du schéma n’est pas nécessairement le propriétaire du type.|Non NULL = ID utilisateur de base de données du propriétaire du type.<br /><br /> NULL = Type non pris en charge, ou ID de type non valide.|  
|**Précision**|Précision du type de données.|Nombre de chiffres ou de caractères.<br /><br /> -1 = **xml** ou type de données de valeur de grande taille<br /><br /> NULL = Type de données introuvable.|  
|**Mise à l’échelle**|Échelle du type de données.|Nombre de décimales pour le type de données.<br /><br /> NULL = Le type de données n’est pas **numeric** ou est introuvable.|  
|**UsesAnsiTrim**|Le paramètre de remplissage ANSI était activé lors de la création du type de données.|1 = Vrai<br /><br /> 0 = Faux<br /><br /> NULL = Type de données introuvable ou différent d'un type de données binaire ou chaîne.|  
  
## <a name="return-types"></a>Types de retour  
 **int**  
  
## <a name="exceptions"></a>Exceptions  
 Retourne la valeur NULL en cas d'erreur ou si un appelant n'est pas autorisé à afficher l'objet.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un utilisateur peut voir uniquement les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d'un droit d'accès. Cela signifie que les fonctions intégrées générant des métadonnées, telles que TYPEPROPERTY, peuvent retourner la valeur NULL si l'utilisateur ne dispose d'aucune autorisation sur l'objet. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-identifying-the-owner-of-a-data-type"></a>R. Identification du propriétaire d'un type de données  
 L'exemple suivant retourne le propriétaire d'un type de données.  
  
```sql
SELECT TYPEPROPERTY(SCHEMA_NAME(schema_id) + '.' + name, 'OwnerId') AS owner_id, name, system_type_id, user_type_id, schema_id  
FROM sys.types;  
```  
  
### <a name="b-returning-the-precision-of-the-tinyint-data-type"></a>B. Renvoi de la précision du type de données tinyint  
 L'exemple suivant renvoie la précision ou le nombre de chiffres pour le type de données `tinyint`.  
  
```sql
SELECT TYPEPROPERTY( 'tinyint', 'PRECISION');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [TYPE_ID &#40;Transact-SQL&#41;](../../t-sql/functions/type-id-transact-sql.md)   
 [TYPE_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/type-name-transact-sql.md)   
 [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)  
  
  

