---
description: cancelRowUpdates, méthode (SQLServerResultSet)
title: Méthode cancelRowUpdates (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.cancelRowUpdates
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 2ecacca4-f7bc-4f5d-886a-da7747fdccae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2d5629e6ea7cb25331cbdeedd1440826c3687caf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168656"
---
# <a name="cancelrowupdates-method-sqlserverresultset"></a>cancelRowUpdates, méthode (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Annule les mises à jour apportées à la ligne actuelle dans cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void cancelRowUpdates()  
```  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode cancelRowUpdates est spécifiée par la méthode cancelRowUpdates de l’interface java.sql.ResultSet.  
  
 Cette méthode peut être appelée après une méthode de programme de mise à jour et avant la méthode [updateRow](../../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md) afin de restaurer les mises à jour apportées à une ligne. Si aucune mise à jour n’a été effectuée ou que updateRow a déjà été appelée, cette méthode n’a aucun effet.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
