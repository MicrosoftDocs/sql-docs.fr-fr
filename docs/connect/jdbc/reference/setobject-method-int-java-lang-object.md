---
description: Méthode setObject (int, java.lang.Object)
title: setObject, méthode (int, java.lang.Object) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setObject (int, java.lang.Object)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 61f19faa-3006-4a1c-974c-55951e3b3000
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d21949a1d33107cd5f10a9352aa6282a8ad2da79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178765"
---
# <a name="setobject-method-int-javalangobject"></a>Méthode setObject (int, java.lang.Object)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit la valeur du paramètre désigné à l'aide de l'objet spécifique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final void setObject(int index,  
                            java.lang.Object obj)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *index*  
  
 **int** indiquant le numéro du paramètre.  
  
 *obj*  
  
 Objet.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setObject est spécifiée par la méthode setObject de l’interface java.sql.PreparedStatement.  
  
 Avant d’appeler cette méthode setObject, l’application peut définir le paramètre spécifié avec une des méthodes suivantes :  
  
-   Les méthodes set\<Type> de la classe SQLServerPreparedStatement ou de la classe SQLServerCallableStatement  
  
-   Les méthodes setNull de la classe SQLServerPreparedStatement ou de la classe SQLServerCallableStatement  
  
-   La méthode [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) de la classe SQLServerCallableStatement  
  
 Dans ce cas, le type du paramètre est défini automatiquement. Si l’application appelle cette méthode setObject avec une valeur obj définie sur Null, le pilote suppose que le type du paramètre correspond à celui défini par la méthode appelée précédemment.  
  
 Si la valeur de obj est Null et qu’aucune information de type ne peut être déterminée pour ce paramètre, cette méthode setObject convertit le paramètre spécifié en type CHAR avant de l’envoyer à la base de données.  
  
 À compter de la version 3.0 de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver, le comportement de cette méthode est modifié par la propriété de connexion **sendTimeAsDatetime** ([Définir les propriétés de connexion](../../../connect/jdbc/setting-the-connection-properties.md)) et par [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 Pour plus d’informations, consultez [Configurer le mode d’envoi des valeurs java.sql.Time au serveur](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [setObject, méthode &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)   
 [SQLServerPreparedStatement, membres](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
