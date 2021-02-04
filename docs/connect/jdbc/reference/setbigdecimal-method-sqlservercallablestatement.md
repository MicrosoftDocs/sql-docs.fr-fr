---
description: Méthode setBigDecimal (SQLServerCallableStatement)
title: Méthode setBigDecimal (SQLServerCallableStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.setBigDecimal
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b50a920c-3839-40f0-9411-c60bbc2a9f34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 600a9d354f3c14dc4b2240639ff4a136df2949be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173934"
---
# <a name="setbigdecimal-method-sqlservercallablestatement"></a>Méthode setBigDecimal (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le numéro de paramètre désigné selon l’objet BigDecimal donné.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setBigDecimal(java.lang.String sCol,  
                          java.math.BigDecimal bd)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *sCol*  
  
 Valeur **chaîne** qui contient le nom du paramètre.  
  
 *bd*  
  
 Objet BigDecimal.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setBigDecimal est spécifiée par la méthode setBigDecimal de l’interface java.sql.CallableStatement.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerCallableStatement, membres](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
