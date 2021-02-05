---
description: Méthode setNString (int, java.lang.String)
title: Méthode setNString (int, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: b7da6d44-f5b1-44f8-95f5-40179968b1b0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c9a0e8ddb0bb108fbb6772ad12e98256efa291d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178786"
---
# <a name="setnstring-method-int-javalangstring"></a>Méthode setNString (int, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le paramètre désigné selon l’objet **String** spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final void setNString(int parameterIndex,  
                                                  java.lang.String value)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *parameterIndex*  
  
 **int** indiquant l’index du paramètre.  
  
 *value*  
  
 Objet **String** contenant la valeur du paramètre.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode doit être utilisée pour les types de données **NCHAR**, **NVARCHAR**, **NTEXT** et **XML**.  
  
 Cette méthode setNString est spécifiée par la méthode setNString de l’interface java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)  
  
  
