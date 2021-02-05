---
description: Méthode updateBlob (int, java.io.InputStream)
title: Méthode updateBlob (int, java.io.InputStream) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: d0263018-d326-4a7b-bf6f-5f508db899d4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 442900b54a1e50f75d2a31463406fda25025d1d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183224"
---
# <a name="updateblob-method-int-javaioinputstream"></a>Méthode updateBlob (int, java.io.InputStream)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée à l'aide du flux d'entrée spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateBlob(int columnIndex,  
                       java.io.InputStream inputStream)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 **int** indiquant l’index de la colonne.  
  
 *inputStream*  
  
 Objet InputStream.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode updateBlob est spécifiée par la méthode updateBlob de l’interface java.sql.ResultSet.  
  
## <a name="see-also"></a>Voir aussi  
 [updateBlob, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateblob-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
