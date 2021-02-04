---
description: Méthode getReference (SQLServerDataSource)
title: Méthode getReference (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getReference
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b3fb1a97-86ee-4977-adca-c35ae199dbb3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d44e8f10c1e4160504d20de7d5a9b3e36b31b950
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175206"
---
# <a name="getreference-method-sqlserverdatasource"></a>Méthode getReference (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne une référence à cet objet [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public javax.naming.Reference getReference()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Objet de référence.  
  
## <a name="remarks"></a>Notes  
 Cette méthode getReference est spécifiée par la méthode getReference dans l'interface javax.naming.Referenceable.  
  
 Avant [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0, si SQLServerDataSource.setTrustStorePassword était appelé sur un objet SQLServerDataSource, le mot de passe était présent dans l’objet retourné par SQLServerDataSource.getReference, ce qui permettait l’utilisation de l’objet pour effectuer des connexions supplémentaires. Dans la version 3.0 du pilote JDBC, vous devez définir le mot de passe sur l'objet retourné par SQLServerDataSource.getReference avant d'établir des connexions avec l'objet.  
  
 De même, si vous définissez SQLServerDataSource.setTrustStorePassword avant de lier les propriétés de la source de données, vous devez appeler SQLServerDataSource.setTrustStorePassword avant d'obtenir la connexion. Par exemple,  
  
```  
ctx = new InitialContext(System.getProperties());  
  
SQLServerDataSource ds1 = (SQLServerDataSource) ctx.lookup(jndiName);  
  
ds1.setTrustStorePassword("XXXXX");  
Connection con = ds1.getConnection("user", "XXXXXX");  
  
ctx.rebind(jndiName, ds1);  
SQLServerDataSource ds2 = (SQLServerDataSource) ctx.lookup(jndiName);  
ds2.setTrustStorePassword("XXXXX");   // reset the truststore password  
con = ds2.getConnection("user", "XXXXXX");   // provide userid and password again  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
