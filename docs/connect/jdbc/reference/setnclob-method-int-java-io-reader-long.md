---
description: Méthode setNClob (int, java.io.Reader, long)
title: Méthode setNClob (int, java.io.Reader, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 11071f8f-0e9b-45f0-b600-aaef7e2815d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6923ec8ef943b65acb69e703a784d25de7a7b597
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178839"
---
# <a name="setnclob-method-int-javaioreader-long"></a>Méthode setNClob (int, java.io.Reader, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le paramètre désigné selon l’objet Reader spécifié, qui correspond au nombre de caractères spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final void setNClob(int parameterIndex,  
                    java.io.Reader reader,  
                    long length)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *parameterIndex*  
  
 **int** indiquant l’index du paramètre.  
  
 *reader*  
  
 Objet Reader indiquant la valeur du paramètre.  
  
 *length*  
  
 **long** indiquant le nombre de caractères dans la valeur de paramètre.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setNClob est spécifiée par la méthode setNClob de l’interface java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Voir aussi  
 [setNClob, méthode &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setnclob-method-sqlserverpreparedstatement.md)   
 [Membres de SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)  
  
  
