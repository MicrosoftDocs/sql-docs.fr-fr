---
description: IsNull (type de données geography)
title: IsNull (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IsNull (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- IsNull method
ms.assetid: c031074f-bfda-4584-a3bf-4e7c324f237f
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 5cf656f1f05188a6b765aa9eef4c36131fda56b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205403"
---
# <a name="isnull-geography-data-type"></a>IsNull (type de données geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Propriété qui spécifie si l’instance **geography** a une valeur Null. Retourne 'TRUE' si l'instance est Null ; retourne 0 si l'instance n'est pas Null.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
.IsNull  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 Type [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **bit**  
  
 Type CLR : **SqlBoolean**  
  
## <a name="remarks"></a>Remarques  
 `IsNull` permet de tester si une instance **geography** a une valeur Null. Les résultats peuvent prêter à confusion, la méthode retournant 0 si l'instance n'est pas Null, mais Null si l'instance est Null.  
  
 Cette méthode est principalement utilisée par l’infrastructure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il est recommandé d’utiliser le prédicat T-SQL IS NULL pour tester si une instance **geography** a une valeur Null. Pour plus d’informations sur le prédicat T-SQL IS NULL, consultez [IS NULL &#40;Transact-SQL&#41;](../../t-sql/queries/is-null-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes étendues sur des instances geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
