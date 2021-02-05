---
description: Méthode setClientInfo (java.lang.String, java.lang.String)
title: setClientInfo, méthode (java.lang.String, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 8d050831-8305-48a8-bd22-207932111040
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a4609d6704346b011d2d2d9eab42afe22b537b15
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179113"
---
# <a name="setclientinfo-method-javalangstring-javalangstring"></a>Méthode setClientInfo (java.lang.String, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit la valeur de la propriété d'informations client spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setClientInfo (java.lang.String name,  
                           java.lang.String value)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *name*  
  
 Chaîne qui contient le nom de la propriété d’information client à définir.  
  
 *value*  
  
 Chaîne qui contient la valeur de la propriété d’information client.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setClientInfo est spécifiée par la méthode setClientInfo de l’interface java.sql.Connection.  
  
 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] ne prend en charge aucune propriété d’informations client. Dans la version 2.0 du pilote JDBC, cette méthode génère un avertissement pour une propriété. Les applications doivent utiliser la méthode [getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverconnection.md) de la classe [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) pour récupérer un avertissement.  
  
## <a name="see-also"></a>Voir aussi  
 [setClientInfo, méthode &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/setclientinfo-method-sqlserverconnection.md)   
 [SQLServerConnection, membres](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
