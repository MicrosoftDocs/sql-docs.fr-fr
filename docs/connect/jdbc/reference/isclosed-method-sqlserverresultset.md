---
description: Méthode isClosed (SQLServerResultSet)
title: Méthode isClosed (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 6081aa34-fc88-4dd0-9a3f-05e8488219dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c83e158dcb94f6fdf5b3ef2e9d1eedc5880e3d93
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88433581"
---
# <a name="isclosed-method-sqlserverresultset"></a>Méthode isClosed (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indique si cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) a été fermé.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean isClosed()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **true** si cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) est fermé ; **false** s’il est toujours ouvert.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode isClosed est spécifiée par la méthode isClosed de l’interface java.sql.ResultSet.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
