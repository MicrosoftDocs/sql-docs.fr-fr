---
description: Méthode executeUpdate (java.lang.String) (SQLServerStatement)
title: executeUpdate, méthode (java.lang.String) (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.executeUpdate (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 85e7c3a2-f2da-49bf-9d90-5fd246fd60e1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8d518ab7d043a5b54cf363a4be96523fed473a79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168507"
---
# <a name="executeupdate-method-javalangstring-sqlserverstatement"></a>Méthode executeUpdate (java.lang.String) (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Exécute l'instruction SQL fournie, qui peut être une instruction INSERT, UPDATE ou DELETE ; ou une instruction SQL qui ne retourne rien, telle qu'une instruction SQL DDL. À compter de [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0, executeUpdate retourne le nombre correct de lignes mises à jour dans une opération MERGE.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public int executeUpdate(java.lang.String sql)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *sql*  
  
 **Chaîne** contenant l’instruction SQL.  
  
## <a name="return-value"></a>Valeur de retour  
 **int** indiquant le nombre de lignes affectées, ou 0 en cas d’instruction DDL.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode executeUpdate est spécifiée par la méthode executeUpdate de l’interface java.sql.Statement.  
  
 Si l’exécution d’une procédure stockée aboutit à plusieurs mises à jour, ou si cela génère plusieurs jeux de résultats, utilisez la méthode [execute](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md) pour exécuter la procédure stockée.  
  
## <a name="see-also"></a>Voir aussi  
 [executeUpdate, méthode &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/executeupdate-method-sqlserverstatement.md)   
 [SQLServerStatement, membres](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
