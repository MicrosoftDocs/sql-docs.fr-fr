---
description: Méthode wasNull (SQLServerCallableStatement)
title: Méthode wasNull (SQLServerCallableStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.wasNull
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 1a27b2fe-ae12-46a9-9bca-2c5ca66b9eb3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 517abf6fc57fa890b058f18300e4fc502aa270e4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181119"
---
# <a name="wasnull-method-sqlservercallablestatement"></a>Méthode wasNull (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère les informations déterminant si le dernier paramètre OUT lu possédait la valeur SQL NULL.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean wasNull()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **true** si le dernier paramètre lu était Null. Dans le cas contraire, la valeur est **false**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode wasNull est spécifiée par la méthode wasNull dans l’interface java.sql.CallableStatement.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerCallableStatement, membres](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
