---
description: Méthode updateTime (java.lang.String, java.sql.Time)
title: updateTime, méthode (java.lang.String, java.sql.Time) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.updateTime (java.lang.String, java.sql.Time)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: fbbcef68-b903-4cfd-911c-a7e239d17c74
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 36af12218b060175bd708e6d5ca6468c18a7e3f3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88457924"
---
# <a name="updatetime-method-javalangstring-javasqltime"></a>Méthode updateTime (java.lang.String, java.sql.Time)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée avec une valeur d'heure en fonction du nom de colonne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateTime(java.lang.String columnName,  
                       java.sql.Time x)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnName*  
  
 Valeur **String** qui contient le nom de la colonne.  
  
 *x*  
  
 Valeur d'heure.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode updateTime est spécifiée par la méthode updateTime de l’interface java.sql.ResultSet.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode updateTime &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatetime-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
