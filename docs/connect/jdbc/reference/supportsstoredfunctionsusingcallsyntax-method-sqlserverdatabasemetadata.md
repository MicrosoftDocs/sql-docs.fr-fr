---
description: Méthode supportsStoredFunctionsUsingCallSyntax (SQLServerDatabaseMetaData)
title: Méthode supportsStoredFunctionsUsingCallSyntax | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 0e5c0579-84b5-4717-b128-0bcd512f6022
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 12e85da95c06aebdef1106b1544cefd5c44ff6b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188683"
---
# <a name="supportsstoredfunctionsusingcallsyntax-method-sqlserverdatabasemetadata"></a>Méthode supportsStoredFunctionsUsingCallSyntax (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indique si la base de données actuelle prend en charge l'appel de fonctions définies par l'utilisateur ou le fournisseur à l'aide de la syntaxe d'échappement de procédure stockée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean supportsStoredFunctionsUsingCallSyntax()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **true** en cas de prise en charge. Dans le cas contraire, la valeur est **false**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode supportsStoredFunctionsUsingCallSyntax est spécifiée par la méthode supportsStoredFunctionsUsingCallSyntax de l’interface java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDatabaseMetaData, méthodes](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData, membres](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
