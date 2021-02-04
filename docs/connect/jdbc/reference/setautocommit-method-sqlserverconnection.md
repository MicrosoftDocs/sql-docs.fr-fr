---
title: setAutoCommit, méthode (SQLServerConnection)
description: Découvrez les détails de l’API publique pour la méthode setAutoCommit dans la classe SQLServerConnection de JDBC Driver pour SQL Server.
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setAutoCommit
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: db1e22d2-e53f-474e-8c99-cb1fff7f491a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2560883b009aa1184d001851f0fc09a4fa575ac4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173949"
---
# <a name="setautocommit-method-sqlserverconnection"></a>setAutoCommit, méthode (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le mode de validation automatique actuel de cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) selon l’état donné.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setAutoCommit(boolean value)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *value*  
  
 **true** pour activer le mode de validation automatique pour la connexion, **false** pour le désactiver.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setAutoCommit est spécifiée par la méthode setAutoCommit de l’interface java.sql.Connection.  
  
 Si une connexion est en mode de validation automatique, toutes ses instructions SQL sont exécutées et validées en tant que transactions individuelles. Autrement, ses instructions SQL sont regroupées dans des transactions qui prennent fin avec l’appel de la méthode [commit](../../../connect/jdbc/reference/commit-method-sqlserverconnection.md) ou de la méthode [rollback](../../../connect/jdbc/reference/rollback-method-sqlserverconnection.md). Par défaut, les nouvelles connexions sont en mode de validation automatique.  
  
 La validation se produit à la fin de l'instruction ou à l'exécution suivante, selon l'opération qui se produit en premier. Lorsque les instructions retournent un objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md), l’instruction se termine lorsque la dernière ligne du jeu de résultats a été récupérée ou lorsque le jeu de résultats a été fermé. Dans les cas plus avancés, une instruction unique peut retourner plusieurs résultats en plus des valeurs de paramètre de sortie. Dans ces cas, la validation se produit lorsque tous les résultats et valeurs de paramètre de sortie ont été récupérés.  
  
 Lorsque le mode de validation automatique a la valeur **false**, le pilote JDBC démarre implicitement une nouvelle transaction après chaque validation.  
  
> [!NOTE]  
> Si cette méthode est appelée au cours d'une transaction, la transaction est validée.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)  
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
