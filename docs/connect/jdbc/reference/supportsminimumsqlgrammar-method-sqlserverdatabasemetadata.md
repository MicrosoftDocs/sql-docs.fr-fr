---
description: Méthode supportsMinimumSQLGrammar (SQLServerDatabaseMetaData)
title: Méthode supportsMinimumSQLGrammar (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsMinimumSQLGrammar
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 40f5d727-1ce7-414d-867d-589ead7b2a29
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ffeb333b60f77fa8eca5eda256d39391acd860f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185887"
---
# <a name="supportsminimumsqlgrammar-method-sqlserverdatabasemetadata"></a>Méthode supportsMinimumSQLGrammar (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère les informations déterminant si cette base de données prend en charge la grammaire SQL minimale ODBC.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean supportsMinimumSQLGrammar()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **true** en cas de prise en charge. Dans le cas contraire, la valeur est **false**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode supportsMinimumSQLGrammer est spécifiée par la méthode supportsMinimumSQLGrammer de l’interface java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDatabaseMetaData, méthodes](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData, membres](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
