---
description: previous, méthode (SQLServerResultSet)
title: Méthode previous (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.previous
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 66eb4e10-c375-4b31-ac46-3ba1d9dbf6a0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 97ba2829717022eeb26bdc2e43eb007eb61063b2
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88432841"
---
# <a name="previous-method-sqlserverresultset"></a>previous, méthode (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Déplace le curseur à la ligne précédente dans cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean previous()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **true** si la nouvelle ligne active est valide. **false** s’il n’y a plus de lignes à traiter.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode previous est spécifiée par la méthode previous de l’interface java.sql.ResultSet.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
