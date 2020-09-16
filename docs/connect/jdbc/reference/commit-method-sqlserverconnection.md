---
description: commit, méthode (SQLServerConnection)
title: Méthode commit (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerConnection.commit
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c7346165-51bf-4844-b64c-29833c147236
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 50fe66c673f443c7c69e26ca608562794950558f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88438031"
---
# <a name="commit-method-sqlserverconnection"></a>commit, méthode (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Rend permanentes toutes les modifications effectuées depuis la précédente validation ou restauration, et libère tous les verrous de base de données actuellement maintenus par cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void commit()  
```  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode commit est spécifiée par la méthode commit de l’interface java.sql.Connection.  
  
 Cette méthode doit être utilisée uniquement lorsque le mode de validation automatique a été désactivé.  
  
 Notez que cette méthode échoue et lève une exception si le client démarre une transaction manuelle, et si, pour une quelconque raison, SQL Server restaure ensuite cette transaction manuelle. Par exemple, une exception est levée si le client appelle une procédure stockée qui appelle explicitement ROLLBACK TRANSACTION, puis qu’il appelle la méthode commit. De plus, si SQL Server génère une erreur de gravité suffisante (16 ou plus) pour restaurer la transaction manuelle initiée par le client, l’appel suivant à la méthode commit lève une exception.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerConnection, membres](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
