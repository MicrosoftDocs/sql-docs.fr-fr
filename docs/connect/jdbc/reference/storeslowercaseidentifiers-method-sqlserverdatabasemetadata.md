---
description: Méthode storesLowerCaseIdentifiers (SQLServerDatabaseMetaData)
title: Méthode storesLowerCaseIdentifiers (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.storesLowerCaseIdentifiers
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b7dd60f5-c4f3-4b14-9a33-d95327395083
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7964fb4caf0ea687171a0103481de316aa90a604
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158256"
---
# <a name="storeslowercaseidentifiers-method-sqlserverdatabasemetadata"></a>Méthode storesLowerCaseIdentifiers (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère les informations déterminant si cette base de données traite les identificateurs SQL à casse mixte qui ne se trouvent pas entre guillemets comme non sensibles à la casse et les stocke en minuscules.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean storesLowerCaseIdentifiers()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **true** si les identificateurs sont stockés en minuscules. Dans le cas contraire, la valeur est **false**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode storesLowerCaseIdentifiers est spécifiée par la méthode storesLowerCaseIdentifiers de l’interface java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDatabaseMetaData, méthodes](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData, membres](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
