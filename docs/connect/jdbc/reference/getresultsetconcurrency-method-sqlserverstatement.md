---
description: getResultSetConcurrency, méthode (SQLServerStatement)
title: Méthode getResultSetConcurrency (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getResultSetConcurrency
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 47ef6547-5ec7-4cf5-a4d4-e34cbeec72eb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 91cad6d742be8064b12f4ff8f394e1601a81ce2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175148"
---
# <a name="getresultsetconcurrency-method-sqlserverstatement"></a>getResultSetConcurrency, méthode (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la concurrence du jeu de résultats pour les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) générés par cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final int getResultSetConcurrency()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **int** indiquant le type de concurrence du jeu de résultats.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getResultSetConcurrency est spécifiée par la méthode getResultSetConcurrency de l’interface java.sql.Statement.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerStatement, membres](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
