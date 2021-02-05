---
description: Méthode updateClob (int, java.io.Reader)
title: Méthode updateClob (int, java.io.Reader) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: df60fbf1-44b2-4658-84a5-5cb129ce2dc6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 98920219bfde773b9f8297501e16ecc43fcda2f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188214"
---
# <a name="updateclob-method-int-javaioreader"></a>Méthode updateClob (int, java.io.Reader)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée à l'aide de l'objet Reader spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateClob(int columnIndex,  
                        java.io.Reader reader)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 **int** indiquant l’index de la colonne.  
  
 *reader*  
  
 Objet Reader.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode updateClob est spécifiée par la méthode updateClob de l’interface java.sql.ResultSet.  
  
## <a name="see-also"></a>Voir aussi  
 [updateClob, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateclob-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
