---
description: Méthode allTablesAreSelectable (SQLServerDatabaseMetaData)
title: Méthode allTablesAreSelectable (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.allTablesAreSelectable
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: eb340450-45a7-49c8-84bc-1b9dd5ee842f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 861fb045c6a52676ad7016349f112921f123ee74
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168674"
---
# <a name="alltablesareselectable-method-sqlserverdatabasemetadata"></a>Méthode allTablesAreSelectable (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère les informations indiquant si l’utilisateur actuel dispose d’autorisations suffisantes pour utiliser toutes les tables retournées par la méthode [getTables](../../../connect/jdbc/reference/gettables-method-sqlserverdatabasemetadata.md) dans une instruction SELECT.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean allTablesAreSelectable()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **true** si l’utilisateur dispose des autorisations nécessaires pour appeler à utiliser toutes les tables. Dans le cas contraire, la valeur est **false**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode allTablesAreSelectable, est spécifiée par la méthode allTablesAreSelectable de l’interfacejava.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDatabaseMetaData, méthodes](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData, membres](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
