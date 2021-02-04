---
description: Méthode setDateTimeOffset (SQLServerCallableStatement)
title: Méthode setDateTimeOffset (SQLServerCallableStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 9383e14d-c83e-43c5-980c-50a3e0bedc31
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dfa33ecb86e178cdef855ce8441f762383d89dbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173568"
---
# <a name="setdatetimeoffset-method-sqlservercallablestatement"></a>Méthode setDateTimeOffset (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Cette méthode a été ajoutée dans [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0.  
  
 Définit la valeur de la colonne spécifiée sur la valeur [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setDateTimeOffset(String sCol, microsoft.sql.DateTimeOffset t)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *sCol*  
  
 Nom d'une colonne.  
  
 *t*  
  
 Objet [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Vous pouvez récupérer une valeur [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md) avec [SQLServerCallableStatement.getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md).  
  
 [setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlserverpreparedstatement.md) prend la valeur du numéro de colonne.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerCallableStatement, membres](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
