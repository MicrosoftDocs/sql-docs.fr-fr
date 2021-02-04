---
description: getDisableStatementPooling, méthode (SQLServerConnection)
title: Méthode getDisableStatementPooling (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.getDisableStatementPooling
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6a56685176ff629f0eccdecd0df6aa5c28d3658
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163136"
---
# <a name="getdisablestatementpooling-method-sqlserverconnection"></a>getDisableStatementPooling, méthode (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

 Retourne la valeur de la propriété de connexion **disableStatementPooling**. Ce paramètre contrôle si le regroupement d’instructions est activé ou non pour cette connexion.

## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean getDisableStatementPooling()  
```  

## <a name="return-value"></a>Valeur de retour
 **Valeur booléenne** qui contient la valeur de la propriété de connexion **disableStatementPooling**.

## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Notes  
 Cette méthode est disponible dans la version 6.4 et versions ultérieures du pilote JDBC.
 
## <a name="see-also"></a>Voir aussi  
 [SQLServerConnection, membres](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
