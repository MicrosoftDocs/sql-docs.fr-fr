---
description: Méthode getPooledConnection (java.lang.String, java.lang.String)
title: getPooledConnection, méthode (java.lang.String, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerConnectionPoolDataSource.getPooledConnection (java.lang.String, java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f2e6391d-9aaf-4b09-ae1c-a27c1ada6301
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cd257553cf6d46b2317e0b4353f199e96c5b54ae
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88434981"
---
# <a name="getpooledconnection-method-javalangstring-javalangstring"></a>Méthode getPooledConnection (java.lang.String, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Essaie d'établir une connexion de base de données physique qui peut être utilisée en tant que connexion regroupée en fonction du nom et du mot de passe d'utilisateur donnés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public javax.sql.PooledConnection getPooledConnection(java.lang.String user,  
                                                      java.lang.String password)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *user*  
  
 **Chaîne** qui contient le nom de l’utilisateur.  
  
 *passwword*  
  
 **Chaîne** qui contient le mot de passe.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md).  
  
## <a name="exceptions"></a>Exceptions  
 java.sql.SQLException  
  
## <a name="remarks"></a>Notes  
 Cette méthode getPooledConnection est spécifiée par la méthode getPooledConnection dans l'interface javax.sql.ConnectionPoolDataSource.  
  
## <a name="see-also"></a>Voir aussi  
 [getPooledConnection](../../../connect/jdbc/reference/getpooledconnection-method-sqlserverconnectionpooldatasource.md)   
 [SQLServerConnectionPoolDataSource, méthodes](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-methods.md)   
 [SQLServerConnectionPoolDataSource, membres](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-members.md)   
 [Classe SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)  
  
  
