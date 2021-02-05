---
description: Méthode setFetchSize (SQLServerStatement)
title: Méthode setFetchSize (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.setFetchSize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 760e555e-9667-4b40-b0ba-778026ff2923
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c66e978e0f1bc2233e2359a5fd92f4a94e96fbd4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178937"
---
# <a name="setfetchsize-method-sqlserverstatement"></a>Méthode setFetchSize (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Donne un indicateur à [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] concernant le nombre de lignes à extraire de la base de données, lorsque davantage de lignes sont nécessaires.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final void setFetchSize(int rows)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *rows*  
  
 **int** indiquant le nombre de lignes à récupérer (fetch).  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setFetchSize est spécifiée par la méthode setFetchSize de l’interface java.sql.Statement.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerStatement, membres](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
