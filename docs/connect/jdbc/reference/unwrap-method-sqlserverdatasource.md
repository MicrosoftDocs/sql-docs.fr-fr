---
description: Méthode unwrap (SQLServerDataSource)
title: Méthode unwrap (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: eb8abe29-f3ec-4752-a590-1d5dc3e48f08
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cb2a664b47e4487b7e123c275a78f8cefeaf6220
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190739"
---
# <a name="unwrap-method-sqlserverdatasource"></a>Méthode unwrap (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne un objet qui implémente l’interface spécifiée, afin d’autoriser l’accès aux méthodes propres au [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public <T> T unwrap(Class<T> iface)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *iface*  
  
 Classe de type T définissant une interface.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet qui implémente l'interface spécifiée.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 La méthode [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverdatasource.md) est définie par l’interface java.sql.Wrapper, introduite dans les spécifications de JDBC 4.0.  
  
 Les applications devront peut-être accéder à des extensions de l’API JDBC propres au [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. La méthode unwrap prend en charge la désencapsulation dans des classes publiques étendues par cet objet, si elles exposent des extensions de fournisseur.  
  
 Lorsque cette méthode est appelée, l’objet se désencapsule dans la classe [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).  
  
 Pour plus d’informations, consultez [Wrappers et interfaces](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Voir aussi  
 [isWrapperFor, méthode &#40;SQLServerDataSource&#41;](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverdatasource.md)   
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
