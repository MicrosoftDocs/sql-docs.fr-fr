---
description: Méthode setCatalog (SQLServerConnection)
title: Méthode setCatalog (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setCatalog
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 553c0603-c07d-436a-86eb-3ba6b51bd696
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d7c72a4f3198f1d38906288a5c5cec8f3873cb53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173735"
---
# <a name="setcatalog-method-sqlserverconnection"></a>Méthode setCatalog (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le nom de catalogue donné pour sélectionner un sous-espace de la base de données de cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) servant d’emplacement de travail.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setCatalog(java.lang.String catalog)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *catalog*  
  
 **Chaîne** contenant le nom du catalogue.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setCatalog est spécifiée par la méthode setCatalog de l’interface java.sql.Connection.  
  
 L'argument du *catalogue* est placé automatiquement par [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Cette méthode permet de définir la propriété catalog de l’objet Connection. La propriété n'est pas définie implicitement de quelque autre façon.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerConnection, membres](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
