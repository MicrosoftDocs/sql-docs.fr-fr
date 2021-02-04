---
description: getBigDecimal, méthode (int, int) (SQLServerResultSet)
title: getBigDecimal, méthode (int, int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getBigDecimal (int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c99d0772-b26c-492c-a643-2813b5429993
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 322eda5e561252ea2f114cf170c14962c55b1412
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168297"
---
# <a name="getbigdecimal-method-int-int-sqlserverresultset"></a>getBigDecimal, méthode (int, int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur de l’index de colonne désigné dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) suivant l’échelle donnée.  
  
> [!NOTE]  
>  Cette méthode a été déconseillée dans la spécification JDBC. Utilisez plutôt la méthode [getBigDecimal (int)](../../../connect/jdbc/reference/getbigdecimal-method-int-sqlserverresultset.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.math.BigDecimal getBigDecimal(int columnIndex,  
                                          int scale)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 **int** indiquant l’index de la colonne.  
  
 *scale*  
  
 **int** indiquant le nombre de chiffres à droite du séparateur décimal.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet BigDecimal.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getBigDecimal est spécifiée par la méthode getBigDecimal de l’interface java.sql.ResultSet.  
  
## <a name="see-also"></a>Voir aussi  
 [getBigDecimal, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getbigdecimal-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
