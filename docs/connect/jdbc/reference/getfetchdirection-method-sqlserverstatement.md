---
description: Méthode getFetchDirection (SQLServerStatement)
title: Méthode getFetchDirection (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getFetchDirection
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ceb4ae68-decc-46d3-83f1-0bbd23aaf58c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a2e23066c13ecba2034b79432fededbf88edaa07
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163047"
---
# <a name="getfetchdirection-method-sqlserverstatement"></a>Méthode getFetchDirection (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la direction d’extraction des lignes dans des tables de base de données. C’est la méthode par défaut pour les jeux de résultats générés à partir de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
> [!NOTE]  
>  Actuellement, cette méthode n’est pas implémentée par [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Par conséquent, elle retourne toujours FETCH_UNKNOWN.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final int getFetchDirection()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **int** indiquant le sens d’extraction spécifié par la méthode [setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md).  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getFetchDirection est spécifiée par la méthode getFetchDirection de l’interface java.sql.Statement.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerStatement, membres](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
