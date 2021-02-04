---
description: Méthode getSelectMethod (SQLServerDataSource)
title: Méthode getSelectMethod (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getSelectMethod
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b6255d2e-0028-474a-afa8-553ef092243e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b7f60926fdf69a27c5e78d2817084cfe1500f91b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175049"
---
# <a name="getselectmethod-method-sqlserverdatasource"></a>Méthode getSelectMethod (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne le type de curseur par défaut utilisé pour tous les jeux de résultats créés à l’aide de cet objet [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.lang.String getSelectMethod()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Valeur **String** contenant le type de curseur par défaut.  
  
## <a name="remarks"></a>Notes  
 La propriété selectMethod spécifie le type de curseur par défaut utilisé pour un jeu de résultats. Cette propriété est utile lorsque vous traitez des jeux de résultats volumineux et que vous ne souhaitez pas stocker le jeu de résultats tout entier en mémoire côté client. En définissant la propriété sur « cursor », vous pouvez créer un curseur côté serveur capable d'extraire en une seule fois de plus petits segments de données. Si la propriété selectMethod n'est pas définie, getSelectMethod retourne la valeur par défaut « direct ».  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
