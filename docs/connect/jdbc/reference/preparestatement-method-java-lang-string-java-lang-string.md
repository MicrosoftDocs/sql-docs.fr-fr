---
description: prepareStatement, méthode (java.lang.String, java.lang.String)
title: prepareStatement, méthode (java.lang.String, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.prepareStatement
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e0db2871-3a5f-4fcc-af61-92333042dcd1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 703e2a101067285cdd532838084067cd993fb098
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176761"
---
# <a name="preparestatement-method-javalangstring-javalangstring"></a>prepareStatement, méthode (java.lang.String, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Crée un objet [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) servant à envoyer des instructions SQL paramétrables à la base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.PreparedStatement prepareStatement(java.lang.String sql,  
                                                   java.lang.String[] columnNames)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *sql*  
  
 **String** contenant une instruction SQL.  
  
 *columnNames*  
  
 Tableau **chaîne** de noms de colonnes.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet PreparedStatement.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode prepareStatement est spécifiée par la méthode prepareStatement de l’interface java.sql.Connection.  
  
## <a name="see-also"></a>Voir aussi  
 [prepareStatement, méthode &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/preparestatement-method-sqlserverconnection.md)   
 [SQLServerConnection, membres](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
