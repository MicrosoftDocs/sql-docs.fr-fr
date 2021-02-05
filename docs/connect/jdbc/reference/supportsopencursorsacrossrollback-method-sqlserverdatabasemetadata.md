---
description: Méthode supportsOpenCursorsAcrossRollback (SQLServerDatabaseMetaData)
title: Méthode supportsOpenCursorsAcrossRollback | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsOpenCursorsAcrossRollback
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4bc3e82b-a7e7-43a5-8938-6f29c7570163
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 04dd287e3f45454c3452d9a9ebbd0356e179108c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186147"
---
# <a name="supportsopencursorsacrossrollback-method-sqlserverdatabasemetadata"></a>Méthode supportsOpenCursorsAcrossRollback (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère les informations déterminant si cette base de données permet de garder les curseurs ouverts dans les différentes restaurations.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean supportsOpenCursorsAcrossRollback()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **true** en cas de prise en charge. Dans le cas contraire, la valeur est **false**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode supportsOpenCursorsAcrossRollback est spécifiée par la méthode supportsOpenCursorsAcrossRollback de l’interface java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDatabaseMetaData, méthodes](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData, membres](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
