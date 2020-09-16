---
description: next, méthode (SQLServerResultSet)
title: Méthode next (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.next
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 60248447-6908-4036-a779-a501453cd553
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 48e6d1fcfd0a7e2e2778ed6b2f109e66024df7b9
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88433181"
---
# <a name="next-method-sqlserverresultset"></a>next, méthode (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Déplace le curseur d'une ligne vers le bas depuis sa position actuelle.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean next()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **true** si la nouvelle ligne active est valide. **false** s’il n’y a plus de lignes à traiter.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode next est spécifiée par la méthode next de l’interface java.sql.ResultSet.  
  
 Un curseur du jeu de résultats est positionné initialement avant la première ligne. Le premier appel de la méthode next fait de la première ligne la ligne actuelle, le deuxième appel fait de la deuxième ligne la ligne actuelle, et ainsi de suite.  
  
 Si un flux d’entrée est ouvert pour la ligne actuelle, l’appel de la méthode next le ferme implicitement. Une chaîne d’avertissement pour l’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) est effacée à la lecture d’une nouvelle ligne.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
