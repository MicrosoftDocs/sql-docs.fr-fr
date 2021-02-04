---
description: Méthode setLastUpdateCount (SQLServerDataSource)
title: Méthode setLastUpdateCount (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setLastUpdateCount
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 5487631a-1107-4169-84ca-b77fd09bea66
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dc4cc7f66e75ce6c3f69a65ab9ab71e3f4769212
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173412"
---
# <a name="setlastupdatecount-method-sqlserverdatasource"></a>Méthode setLastUpdateCount (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit une valeur **booléenne** qui indique si la propriété lastUpdateCount est activée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setLastUpdateCount(boolean lastUpdateCount)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *lastUpdateCount*  
  
 **true** si lastUpdateCount est activé. Dans le cas contraire, la valeur est **false**.  
  
## <a name="remarks"></a>Notes  
 Si la propriété lastUpdateCount a la valeur **true**, [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] retourne uniquement le dernier nombre de mises à jour à partir d’une instruction SQL transmise au serveur. Si la propriété lastUpdateCount a la valeur **false**, le pilote retourne tous les nombres de mise à jour, y compris ceux retournés par les déclencheurs qui ont pu s’activer. Si la propriété lastUpdateCount n’est pas définie, la méthode [getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md) retourne la valeur par défaut (**true**).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
