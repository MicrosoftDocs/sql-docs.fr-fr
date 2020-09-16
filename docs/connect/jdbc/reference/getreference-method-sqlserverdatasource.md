---
description: Méthode getReference (SQLServerDataSource)
title: Méthode getReference (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDataSource.getReference
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b3fb1a97-86ee-4977-adca-c35ae199dbb3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8916c339a4a1d6c8373e3fbaf5a130dcc168155e
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88434881"
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
  
  
