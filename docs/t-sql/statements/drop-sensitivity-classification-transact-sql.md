---
description: DROP SENSITIVITY CLASSIFICATION (Transact-SQL)
title: DROP SENSITIVITY CLASSIFICATION (Transact-SQL) | Microsoft Docs
ms.date: 03/25/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
ms.custom: ''
ms.author: giladm
author: giladmit
f1_keywords:
- DROP SENSITIVITY CLASSIFICATION
- DROP_SENSITIVITY_CLASSIFICATION
dev_langs:
- TSQL
helpviewer_keywords:
- DROP SENSITIVITY CLASSIFICATION statement
- dropping labels
- drop labels
- removing labels
- remove labels
- classification [SQL]
- labels [SQL]
- information types
- data classification
monikerRange: " >= sql-server-ver15 || = azuresqldb-current"
ms.openlocfilehash: 8b5dce1e51adfd52c89a7cc6fbba2d9f91497933
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688918"
---
# <a name="drop-sensitivity-classification-transact-sql"></a>DROP SENSITIVITY CLASSIFICATION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Supprime les métadonnées de classification de sensibilité d’une ou de plusieurs colonnes de base de données.

## <a name="syntax"></a>Syntaxe

```syntaxsql
DROP SENSITIVITY CLASSIFICATION FROM
    <object_name> [, ...n ]

<object_name> ::=
{
    [schema_name.]table_name.column_name
}
```  

## <a name="arguments"></a>Arguments  

*object_name* ([schema_name.]table_name.column_name)

Nom de la colonne de base de données contenant la classification à supprimer. Actuellement, seule la classification de colonne est prise en charge.
    - *schema_name* (facultatif) : nom du schéma auquel appartient la colonne classifiée.
    - *table_name* : nom du schéma auquel appartient la table classifiée.
    - *column_name* : nom de la colonne de base de données contenant la classification à supprimer.

## <a name="remarks"></a>Remarques  

- Plusieurs classifications d’objet peuvent être supprimées avec une seule instruction « DROP SENSITIVITY CLASSIFICATION ».

## <a name="permissions"></a>Autorisations  

Requiert une autorisation ALTER ANY SENSITIVITY CLASSIFICATION. La classification ALTER ANY SENSITIVITY CLASSIFICATION est impliquée par l’autorisation de base de données ALTER, ou par l’autorisation de serveur CONTROL SERVER.


## <a name="examples"></a>Exemples  


### <a name="a-dropping-classification-from-a-single-column"></a>R. Suppression de la classification d’une seule colonne

L'exemple de code suivant supprime la classification de la colonne `dbo.sales.price`.  

```sql
DROP SENSITIVITY CLASSIFICATION FROM
    dbo.sales.price
```

### <a name="b-dropping-classification-from-multiple-columns"></a>B. Suppression de la classification de plusieurs colonnes

L'exemple de code suivant supprime la classification des colonnes `dbo.sales.price`, `dbo.sales.discount` et `SalesLT.Customer.Phone`.  

```sql
DROP SENSITIVITY CLASSIFICATION FROM
    dbo.sales.price, dbo.sales.discount, SalesLT.Customer.Phone  
```

## <a name="see-also"></a>Voir aussi  

[AJOUTER UNE CLASSIFICATION DE SENSIBILITÉ (Transact-SQL)](../../t-sql/statements/add-sensitivity-classification-transact-sql.md)

[sys.sensitivity_classifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)

[Prise en main de SQL Information Protection](/azure/azure-sql/database/data-discovery-and-classification-overview)