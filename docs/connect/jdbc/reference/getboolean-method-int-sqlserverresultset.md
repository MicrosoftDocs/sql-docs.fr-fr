---
description: getBoolean, méthode (int) (SQLServerResultSet)
title: Méthode getBoolean (int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getBoolean (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 50fcc0c3-36a1-47b2-b18c-7aa2ac9b27d3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7eb830a3f5bcbca11f0e0dfb2493b9f6ea3d53a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168145"
---
# <a name="getboolean-method-int-sqlserverresultset"></a>getBoolean, méthode (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur de l’index de colonne désigné dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) sous forme de **booléen** dans le langage de programmation Java.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public boolean getBoolean(int columnIndex)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 **int** indiquant l’index de la colonne.  
  
## <a name="return-value"></a>Valeur de retour  
 Valeur **booléenne**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getBoolean est spécifiée par la méthode getBoolean de l’interface java.sql.ResultSet.  
  
 Cette méthode est prise en charge uniquement sur les types de données de nombres et de caractères. Elle convertit les valeurs « 1 «, 1 et « **true** » en **true**, et les valeurs « 0 », 0 et « **false** » en **false**. Pour toutes les autres valeurs, le comportement n'est pas défini.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode getBoolean &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getboolean-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
