---
description: getClientConnectionID, méthode (SQLServerConnection)
title: getClientConnectionID Method (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: bee39c11-733a-461f-92cc-33efcb2af87d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d216f6a4a5cbe24b2b73f7669d54b38faf639053
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176138"
---
# <a name="getclientconnectionid-method-sqlserverconnection"></a>getClientConnectionID, méthode (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Obtient l'ID de la tentative de connexion la plus récente, que cette tentative ait réussi ou non.  
  
## <a name="syntax"></a>Syntaxe  
  
``` 
public Java.util.UUID SQLServerConnection.getClientConnectionID();  
```  
  
## <a name="return-value"></a>Valeur de retour  
 GUID de 16 octets représentant l'ID de connexion de la dernière tentative de connexion. Ou NULL, s'il y a un échec après le lancement de la demande de connexion et après la négociation de préconnexion.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Pour plus d’informations sur l’accès aux informations de diagnostic dans le journal des événements étendus, consultez [Accès aux informations de diagnostic dans le journal des événements étendus](../../../connect/jdbc/accessing-diagnostic-information-in-the-extended-events-log.md).  
  
 L'exemple suivant montre comment obtenir l'ID de connexion :  
  
```  
Connection con = DriverManager.getConnection(connectionUrl);  
UUID id = ((ISQLServerConnection)con).getClientConnectionId();  
```  
  
 L'exemple suivant montre une autre façon d'obtenir l'ID de connexion :  
  
```  
SQLServerConnectionPoolDataSource ds = new SQLServerConnectionPoolDataSource();  
ds.setUser("...");  
ds.setPassword("...");  
ds.setServerName("...");  
PooledConnection pcon= ds.getPooledConnection();  
Connection cn = pcon.getConnection();  
UUID conid = ((ISQLServerConnection)cn).getClientConnectionId();  
```  
  
 **getClientConnectionID** fonctionne quelle que soit la version du serveur auquel vous vous connectez, mais les journaux des événements étendus et l’entrée des erreurs de tampon en anneau de connectivité ne seront pas présents dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 2008 R2 et les versions antérieures.  
  
 Vous pouvez également localiser l'ID de connexion dans le journal des événements étendus si l'échec concerne le serveur et si l'événement étendu permet l'enregistrement de l'ID de connexion. Vous pouvez également trouver l’ID de connexion dans la mémoire tampon de l’anneau de connectivité ([Résolution des problèmes de connectivité dans SQL Server 2008 avec la mémoire tampon de l’anneau de connectivité](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)) pour certaines erreurs de connexion. Si l'ID de connexion n'est pas dans la mémoire tampon de l'anneau de connexion, vous pouvez supposer qu'il s'agit d'une erreur réseau.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerConnection, membres](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
