---
description: Méthode updateBoolean (int, boolean)
title: Méthode updateBoolean (int, boolean) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.updateBoolean (int, boolean)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7937f4bb-8537-4012-af81-837f9ac123a2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d603153589cf4ecb637252fd188cf43125b3c0dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188363"
---
# <a name="updateboolean-method-int-boolean"></a>Méthode updateBoolean (int, boolean)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée avec une valeur **booléenne** en fonction de l’index de colonne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateBoolean(int index,  
                          boolean x)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *index*  
  
 **int** indiquant l’index de la colonne.  
  
 *x*  
  
 Valeur **booléenne**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode updateBoolean est spécifiée par la méthode updateBoolean de l’interface java.sql.ResultSet.  
  
## <a name="see-also"></a>Voir aussi  
 [updateBoolean, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateboolean-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
