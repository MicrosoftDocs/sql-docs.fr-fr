---
description: getResultSetHoldability, méthode (SQLServerStatement)
title: Méthode getResultSetHoldability (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getResultSetHoldability
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 053549ee-2018-47ab-9538-789dac2b150a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 78dc9f286cd8ce036d2a0b7ed8ed6047ce958dff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175141"
---
# <a name="getresultsetholdability-method-sqlserverstatement"></a>getResultSetHoldability, méthode (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la capacité de mise en attente du jeu de résultats pour les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) générés par cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final int getResultSetHoldability()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **int** indiquant la mise en attente possible du jeu de résultats.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getResultSetHoldability est spécifiée par la méthode getResultSetHoldability de l’interface java.sql.Statement.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerStatement, membres](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
