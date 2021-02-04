---
description: insertRow, méthode (SQLServerResultSet)
title: Méthode insertRow (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.insertRow
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 363d1008-1396-4fc0-8e27-c9ba2499e7f1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8571bb1e069be74de9ec93890606b887d247e5d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177519"
---
# <a name="insertrow-method-sqlserverresultset"></a>insertRow, méthode (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ajoute le contenu de la ligne d’insertion à cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) et à la base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void insertRow()  
```  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode insertRow est spécifiée par la méthode insertRow de l’interface java.sql.ResultSet.  
  
 Le curseur doit se trouver sur la ligne d'insertion lorsque cette méthode est appelée. Après l'appel de cette méthode, le curseur reste sur la ligne d'insertion et le jeu de résultats demeure en mode insertion.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
