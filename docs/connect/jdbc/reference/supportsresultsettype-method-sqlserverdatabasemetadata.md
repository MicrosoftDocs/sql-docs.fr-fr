---
description: Méthode supportsResultSetType (SQLServerDatabaseMetaData)
title: Méthode supportsResultSetType (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDatabaseMetaData.supportsResultSetType
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: aded734f-c96e-460f-afaa-8f64a92560d7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a08227ba9d2c82831b042d935bacbd65e0216e64
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88450261"
---
# <a name="supportsresultsettype-method-sqlserverdatabasemetadata"></a>Méthode supportsResultSetType (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère les informations déterminant si cette base de données prend en charge le type de jeu de résultats donné.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean supportsResultSetType(int type)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *type*  
  
 **Entier** qui indique le type de jeu de résultats, qui peut avoir l’une des valeurs suivantes comme défini dans java.sql.ResultSet ou SQLServerResultSet :  
  
## <a name="javasqlresultset-types"></a>Types java.sql.ResultSet  
 TYPE_FORWARD_ONLY  
  
 TYPE_SCROLL_SENSITIVE  
  
 TYPE_SCROLL_INSENSITIVE  
  
## <a name="sqlserverresultset-types"></a>Types SQLServerResultSet  
 TYPE_SS_SCROLL_STATIC  
  
 TYPE_SS_SCROLL_KEYSET  
  
 TYPE_SS_DIRECT_FORWARD_ONLY  
  
 TYPE_SS_SERVER_CURSOR_FORWARD_ONLY  
  
 TYPE_SS_SCROLL_DYNAMIC  
  
## <a name="return-value"></a>Valeur de retour  
 **true** en cas de prise en charge. Dans le cas contraire, la valeur est **false**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode supportsResultSetType, est spécifiée par la méthode supportsResultSetType dans l’interface java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDatabaseMetaData, méthodes](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData, membres](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
