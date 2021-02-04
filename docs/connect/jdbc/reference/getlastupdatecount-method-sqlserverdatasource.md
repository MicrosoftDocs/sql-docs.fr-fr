---
description: Méthode getLastUpdateCount (SQLServerDataSource)
title: Méthode getLastUpdateCount (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getLastUpdateCount
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4c4fbb24-0b02-42da-928c-a903bb591cc7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bf2a9772d3fd6c97b9416e810ec3d5680ad3f559
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162898"
---
# <a name="getlastupdatecount-method-sqlserverdatasource"></a>Méthode getLastUpdateCount (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne une valeur **booléenne** qui indique si la propriété lastUpdateCount est activée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean getLastUpdateCount()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **true** si lastUpdateCount est activé. Dans le cas contraire, la valeur est **false**.  
  
## <a name="remarks"></a>Notes  
 Si la propriété lastUpdateCount a la valeur **true**, [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] retourne uniquement le dernier nombre de mises à jour à partir d’une instruction SQL transmise au serveur. Si la propriété lastUpdateCount a la valeur **false**, le pilote retourne tous les nombres de mise à jour, y compris ceux retournés par les déclencheurs qui ont pu s’activer. Si la propriété lastUpdateCount n’est pas définie, la méthode getLastUpdateCount retourne la valeur par défaut (**true**).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
