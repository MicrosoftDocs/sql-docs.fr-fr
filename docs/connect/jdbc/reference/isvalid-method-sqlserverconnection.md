---
description: Méthode isValid (SQLServerConnection)
title: Méthode isValid (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 3b0a8bbf-9369-4456-9ab8-1434ccacdd7e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1b9c55645d33f9bd92e577ff9aa84b76f5948cf7
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88433351"
---
# <a name="isvalid-method-sqlserverconnection"></a>Méthode isValid (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indique si cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) n’a pas été fermé et est toujours valide.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean isValid(int timeout)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *timeout*  
  
 **int** spécifiant le nombre de secondes qui doivent s’écouler avant la validation de la connexion.  
  
## <a name="return-value"></a>Valeur de retour  
 **true** si la connexion est valide ; **false** si la connexion n’est pas valide ou si la validité de la connexion ne peut pas être déterminée avant l’expiration du délai.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode isValid est spécifiée par la méthode isValid de l’interface java.sql.Connection.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerConnection, membres](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
