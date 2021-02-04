---
description: Méthode setObject (java.lang.String, java.lang.Object, int, int)
title: setObject, méthode (java.lang.String, java.lang.Object, int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.setObject (java.lang.String, java.lang.Object, int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 16f5f09a-51b5-423a-b52d-8c2eaa04e9ff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7905385b181b8f809fd4a5775aa86c00dd302f2d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173227"
---
# <a name="setobject-method-javalangstring-javalangobject-int-int"></a>Méthode setObject (java.lang.String, java.lang.Object, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit la valeur du paramètre désigné à l'aide de l'objet , du type de cible et de l'échelle donnés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setObject(java.lang.String sCol,  
                      java.lang.Object o,  
                      int n,  
                      int m)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *sCol*  
  
 Valeur **chaîne** qui contient le nom du paramètre.  
  
 *o*  
  
 Valeur **Object**.  
  
 *n*  
  
 **int** indiquant le type de cible défini dans java.sql.Types.  
  
 *m*  
  
 **int** indiquant le nombre de chiffres à droite du séparateur décimal. Ce paramètre est ignoré pour les types à l'exception de NUMERIC et DECIMAL.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setObject est spécifiée par la méthode setObject dans l’interface java.sql.CallableStatement.  
  
 À compter de la version 3.0 de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver, le comportement de cette méthode est modifié par la propriété de connexion **sendTimeAsDatetime** ([Définir les propriétés de connexion](../../../connect/jdbc/setting-the-connection-properties.md)) et par [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 Pour plus d’informations, consultez [Configurer le mode d’envoi des valeurs java.sql.Time au serveur](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [setObject, méthode (SQLServerCallableStatement)](../../../connect/jdbc/reference/setobject-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement, membres](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
