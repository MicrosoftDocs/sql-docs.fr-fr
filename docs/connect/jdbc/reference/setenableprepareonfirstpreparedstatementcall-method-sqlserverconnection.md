---
description: setEnablePrepareOnFirstPreparedStatementCall, méthode (SQLServerConnection)
title: Méthode setEnablePrepareOnFirstPreparedStatementCall (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setEnablePrepareOnFirstPreparedStatementCall
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0c96860c17e56857171c3f69f15bdd0c966d1c84
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178987"
---
# <a name="setenableprepareonfirstpreparedstatementcall-method-sqlserverconnection"></a>setEnablePrepareOnFirstPreparedStatementCall, méthode (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

 Spécifie le comportement d’une instance de connexion spécifique. Si la valeur est false, la première exécution appellera sp_executesql sans préparer d’instruction. À la deuxième exécution, elle appellera sp_prepexec et configurera un handle d’instruction préparée. Les exécutions suivantes appelleront sp_execute. Cela évite d’utiliser sp_unprepare lors de la fermeture de l’instruction préparée si l’instruction n’est exécutée qu’une seule fois.

## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setEnablePrepareOnFirstPreparedStatementCall(boolean enablePrepareOnFirstPreparedStatementCall)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *enablePrepareOnFirstPreparedStatementCall*  
  
 Nouvelle valeur de la propriété de connexion **enablePrepareOnFirstPreparedStatementCall**.  
 
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Notes  
 Cette méthode est disponible dans la version 6.4 et versions ultérieures du pilote JDBC.
 
## <a name="see-also"></a>Voir aussi  
 [SQLServerConnection, membres](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
