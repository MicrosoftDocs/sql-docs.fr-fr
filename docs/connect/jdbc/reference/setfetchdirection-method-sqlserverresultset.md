---
description: Méthode setFetchDirection (SQLServerResultSet)
title: Méthode setFetchDirection (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.setFetchDirection
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4ee82290-508d-4bff-a5c5-8a56338deef8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b6b7a2ab02e084a0dabdba76cb1f32182d84f1de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173473"
---
# <a name="setfetchdirection-method-sqlserverresultset"></a>Méthode setFetchDirection (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Fournit un conseil concernant la direction de traitement des lignes de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
> [!NOTE]  
>  Actuellement, cette méthode n’est pas prise en charge par le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Si vous utilisez cette méthode, le pilote JDBC mémorise le paramètre, mais actuellement il n'intervient pas dessus.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setFetchDirection(int direction)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *direction*  
  
 **int** indiquant le sens d’extraction suggéré. Peut avoir l’une des valeurs suivantes :  
  
 ResultSet.FETCH_FORWARD  
  
 ResultSet.FETCH_REVERSE  
  
 ResultSet.FETCH_UNKNOWN  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode setFetchDirection est spécifiée par la méthode setFetchDirection de l’interface java.sql.ResultSet.  
  
 La valeur initiale de cette méthode est déterminée par l’objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) qui a produit cet objet SQLServerResultSet. La direction d'extraction peut être modifiée à tout moment.  
  
> [!NOTE]  
>  L'utilisation de cette méthode lorsque le type de curseur est avant uniquement est sans effet.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
