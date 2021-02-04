---
description: Méthode setSavepoint ()
title: Méthode setSavepoint Method () | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setSavepoint ()
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 11013055-4fd3-45a9-b2da-28b2908dad52
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7618072c11924f45eefe68147cd67e881fe6a5a7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173131"
---
# <a name="setsavepoint-method-"></a>Méthode setSavepoint ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Crée un point de sauvegarde sans nom dans la transaction actuelle, puis retourne le nouvel objet [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) qui le représente.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.Savepoint setSavepoint()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Objet SavePoint.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setSavePoint est spécifiée par la méthode setSavePoint de l’interface java.sql.Connection.  
  
## <a name="see-also"></a>Voir aussi  
 [setSavepoint, méthode &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/setsavepoint-method-sqlserverconnection.md)   
 [SQLServerConnection, membres](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
