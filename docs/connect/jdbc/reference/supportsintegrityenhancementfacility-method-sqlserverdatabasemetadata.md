---
description: Méthode supportsIntegrityEnhancementFacility (SQLServerDatabaseMetaData)
title: Méthode supportsIntegrityEnhancementFacility | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsIntegrityEnhancementFacility
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: edee084b-9a8c-4167-9e13-66fc3ed1ecaa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3c7eab951eb1c4587f81ab8ed728954171f828d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185555"
---
# <a name="supportsintegrityenhancementfacility-method-sqlserverdatabasemetadata"></a>Méthode supportsIntegrityEnhancementFacility (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère les informations déterminant si cette base de données prend en charge SQL Integrity Enhancement Facility.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean supportsIntegrityEnhancementFacility()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **true** en cas de prise en charge. Dans le cas contraire, la valeur est **false**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode supportsIntegrityEnhancementFacility est spécifiée par la méthode supportsIntegrityEnhancementFacility de l’interface java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDatabaseMetaData, méthodes](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData, membres](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
