---
description: ReorientObject (type de données geography)
title: ReorientObject (type de données geography) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ReorientObject
- ReorientObject_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ReorientObject method (geography)
ms.assetid: e2a1a4f1-211b-4e82-abed-03fc7140a83c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 9d4f8bf9f9b8409f9ab46af0b1fd0a728e80e505
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2020
ms.locfileid: "88417025"
---
# <a name="reorientobject-geography-data-type"></a>ReorientObject (type de données geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Retourne une instance **geography** avec des régions intérieures et des régions extérieures interchangées.  
  
Cette méthode de type de données **geography** prend en charge les instances **FullGlobe** ou les instances spatiales qui sont plus grandes qu’un hémisphère.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
.ReorientObject (geography)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
_Geography_  
Autre instance **geography** sur laquelle `ReorientObject()` est appelé.  
  
## <a name="return-value"></a>Valeur de retour  
Type de retour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **geography**  
  
Type de retour CLR : **SqlGeography**  
  
## <a name="remarks"></a>Notes  
Cette méthode change l’orientation de l’anneau de tous les **Polygon** dans **GeometryCollection** mais ne supprime ni ne change aucun des **Point** ou **LineString** de la collection.  
  
Si **GeometryCollection** est passé à cette méthode, chacune des instances de la collection est réorientée, mais pas la collection dans son ensemble.  
  
## <a name="examples"></a>Exemples  
  
```  
DECLARE @R GEOGRAPHY = GEOGRAPHY::Parse('Polygon((-10 -10, -10 10, 10 10, 10 -10, -10 -10))');  
SELECT @R.ReorientObject().STAsText();  
--Result: POLYGON ((10 10, -10 10, -10 -10, 10 -10, 10 10))  
```  
  
## <a name="see-also"></a>Voir aussi  
[Méthodes étendues sur des instances geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
