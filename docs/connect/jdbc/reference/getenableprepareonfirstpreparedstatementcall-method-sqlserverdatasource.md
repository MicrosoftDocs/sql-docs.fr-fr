---
description: getEnablePrepareOnFirstPreparedStatementCall, méthode (SQLServerDataSource)
title: Méthode getEnablePrepareOnFirstPreparedStatementCall (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 266a9094e55c8ea239bf3a9997aa684042c84b76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163063"
---
# <a name="getenableprepareonfirstpreparedstatementcall-method-sqlserverdatasource"></a>getEnablePrepareOnFirstPreparedStatementCall, méthode (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne la valeur de la propriété de connexion **enablePrepareOnFirstPreparedStatementCall**. Si cette configuration retourne la valeur false, la première exécution d’une instruction préparée appellera sp_executesql sans préparer d’instruction. À la deuxième exécution, elle appellera sp_prepexec et configurera un handle d’instruction préparée. Les exécutions suivantes appelleront sp_execute. Cela évite d’utiliser sp_unprepare lors de la fermeture de l’instruction préparée si l’instruction n’est exécutée qu’une seule fois. 
  
## <a name="syntax"></a>Syntaxe  
  
```
public boolean getEnablePrepareOnFirstPreparedStatementCall();  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne la valeur **booléenne** de la propriété de connexion **enablePrepareOnFirstPreparedStatementCall**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Notes  
 Cette méthode est disponible dans la version 6.4 et versions ultérieures du pilote JDBC.
 
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
