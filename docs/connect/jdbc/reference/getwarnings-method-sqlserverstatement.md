---
description: getWarnings, méthode (SQLServerStatement)
title: Méthode getWarnings (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerStatement.getWarnings
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3d6decae-2570-4ca5-8ff6-57a2cc3e921f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ac38ef664d8aa295ce6493f6ad3e04222a8780fe
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88433771"
---
# <a name="getwarnings-method-sqlserverstatement"></a>getWarnings, méthode (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère le premier avertissement signalé par des appels sur cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final java.sql.SQLWarning getWarnings()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Objet SQLWarning.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getWarnings est spécifiée par la méthode getWarnings de l’interface java.sql.Statement.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerStatement, membres](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
