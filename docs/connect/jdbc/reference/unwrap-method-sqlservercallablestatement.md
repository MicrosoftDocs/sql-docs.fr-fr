---
description: Méthode unwrap (SQLServerCallableStatement)
title: Méthode unwrap (SQLServerCallableStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: cbbf2728-b8c8-4c35-875a-6e967c8285dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d73dbbec2500ba50a91a13dd1ec171c207096425
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190318"
---
# <a name="unwrap-method-sqlservercallablestatement"></a>Méthode unwrap (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne un objet qui implémente l’interface spécifiée, afin d’autoriser l’accès aux méthodes propres au [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public <T> T unwrap(Class<T> iface)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *iface*  
  
 Classe de type **T** définissant une interface.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet qui implémente l'interface spécifiée.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 La méthode [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md) est définie par l’interface java.sql.Wrapper, introduite dans les spécifications de JDBC 4.0.  
  
 Les applications devront peut-être accéder à des extensions de l’API JDBC propres au [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. La méthode unwrap prend en charge la désencapsulation dans les classes publiques étendues par cet objet, si les classes exposent des extensions de fournisseur.  
  
 [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) implémente [ISQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md), une extension de [ISQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Lorsque cette méthode est appelée, l’objet se désencapsule dans les classes suivantes : [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md), [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) et [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md).  
  
 Pour plus d’informations, consultez [Wrappers et interfaces](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
 L’exemple de code suivant indique comment utiliser les méthodes isWrapperFor et unwrap pour vérifier les extensions du pilote et appeler les méthodes spécifiques des fournisseurs, comme [setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) et [getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md).  
  
```  
public static void executeStoredProcedure(Connection con) {  
   try {  
    CallableStatement cstmt =   
       con.prepareCall("{call dbo.stored_proc_name(?, ?)}");  
  
    // The recommended way to access the JDBC   
    // Driver-specific methods is to use the JDBC 4.0 Wrapper   
    // functionality.   
    // The following code statements demonstrates how to use the   
    // isWrapperFor and unwrap methods  
    // to access the driver-specific response buffering methods.  
  
    if (cstmt.isWrapperFor(  
      com.microsoft.sqlserver.jdbc.SQLServerCallableStatement.class)) {  
     // The CallableStatement object can unwrap to   
     // SQLServerCallableStatement.  
     SQLServerCallableStatement SQLcstmt =   
     cstmt.unwrap(  
        com.microsoft.sqlserver.jdbc.SQLServerCallableStatement.class);  
     SQLcstmt.setResponseBuffering("adaptive");  
     System.out.println("Response buffering mode has been set to " +  
         SQLcstmt.getResponseBuffering());  
     }  
  
    if (cstmt.isWrapperFor(  
      com.microsoft.sqlserver.jdbc.SQLServerPreparedStatement.class)) {  
      // The CallableStatement object can unwrap to   
      // SQLServerPreparedStatement.                    
      SQLServerPreparedStatement SQLpstmt =   
       cstmt.unwrap(  
       com.microsoft.sqlserver.jdbc.SQLServerPreparedStatement.class);  
      SQLpstmt.setResponseBuffering("adaptive");  
      System.out.println("Response buffering mode has been set to " +  
          SQLpstmt.getResponseBuffering());  
    }  
    if (cstmt.isWrapperFor(  
      com.microsoft.sqlserver.jdbc.SQLServerStatement.class)) {  
  
      // The CallableStatement object can unwrap to SQLServerStatement.   
      SQLServerStatement SQLstmt =   
        cstmt.unwrap(  
        com.microsoft.sqlserver.jdbc.SQLServerStatement.class);  
      SQLstmt.setResponseBuffering("adaptive");  
      System.out.println("Response buffering mode has been set to " +  
      SQLstmt.getResponseBuffering());  
    }  
  }  
  catch (Exception e) {  
     e.printStackTrace();  
  }  
}   
```  
  
## <a name="see-also"></a>Voir aussi  
 [isWrapperFor, méthode &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/iswrapperfor-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement, membres](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
