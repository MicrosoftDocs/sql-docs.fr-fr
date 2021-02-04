---
description: Méthode updateSQLXML (java.lang.String, java.sql.SQLXML)
title: updateSQLXML, méthode (java.lang.String, java.sql.SQLXML) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 60021881-ef83-499b-9977-e20ff23c1312
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9d4a7a24ca4a52fe2343194b5e4902c3f5bc64aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181351"
---
# <a name="updatesqlxml-method-javalangstring-javasqlsqlxml"></a>Méthode updateSQLXML (java.lang.String, java.sql.SQLXML)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée avec une valeur java.sql.SQLXML.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateSQLXML(java.lang.String columnLabel,  
                         java.sql.SQLXML xmlObject)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnLabel*  
  
 **Chaîne** indiquant l’étiquette de la colonne.  
  
 *xmlObject*  
  
 Objet SQLXML.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode updateSQLXML est spécifiée par la méthode updateSQLXML de l’interface java.sql.ResultSet.  
  
## <a name="see-also"></a>Voir aussi  
 [updateSQLXML, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatesqlxml-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
