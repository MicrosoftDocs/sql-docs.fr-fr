---
description: Méthode setClob (java.lang.String, java.sql.Clob)
title: setClob, méthode (java.lang.String, java.sql.Clob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 256b5f55-7a6d-44fb-9a09-19fa39f19c35
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 68d2f07c77e3476d32fb437358f863ec0b0ddfb2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173599"
---
# <a name="setclob-method-javalangstring-javasqlclob"></a>Méthode setClob (java.lang.String, java.sql.Clob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le paramètre désigné selon l’objet Clob spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final void setClob(java.lang.String parameterName,  
            java.sql.Clob x)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *parameterName*  
  
 Valeur **chaîne** qui contient le nom du paramètre.  
  
 *x*  
  
 Objet Clob.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setClob est spécifiée par la méthode setClob de l’interface java.sql.CallableStatement.  
  
## <a name="see-also"></a>Voir aussi  
 [setClob, méthode &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/setclob-method-sqlservercallablestatement.md)   
 [Membres de SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
