---
description: relative, méthode (SQLServerResultSet)
title: Méthode relative (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.relative
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 2bcdbb69-95fd-4ae8-8488-1a75a91fe2e0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d61c075e123fc4ae77d852fbe2c3e221bf6ca231
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176669"
---
# <a name="relative-method-sqlserverresultset"></a>relative, méthode (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Déplace le curseur en fonction du nombre de lignes spécifié, par rapport à la ligne actuelle, dans une direction positive ou négative.   
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean relative(int nRows)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *nRows*  
  
 **int** indiquant le nombre de lignes à déplacer.  
  
## <a name="return-value"></a>Valeur de retour  
 **true** si le curseur se trouve sur une ligne. Dans le cas contraire, la valeur est **false**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode relative est spécifiée par la méthode relative de l’interface java.sql.ResultSet.  
  
 Toute tentative de déplacement au-delà de la première ou de la dernière ligne dans le jeu de résultats positionne le curseur avant ou après la première ou la dernière ligne. Appeler `relative(0)` est valide, mais ne modifie pas la position du curseur.  
  
 Appeler la méthode `relative(1)` a le même effet qu’appeler la méthode [next](../../../connect/jdbc/reference/next-method-sqlserverresultset.md). Appeler la méthode `relative(-1)` a le même effet qu’appeler la méthode [previous](../../../connect/jdbc/reference/previous-method-sqlserverresultset.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
