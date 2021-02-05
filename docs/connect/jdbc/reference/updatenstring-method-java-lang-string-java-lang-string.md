---
description: Méthode updateNString (java.lang.String, java.lang.String)
title: updateNString, méthode (java.lang.String, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 6daca03f-c60f-4842-b9e3-11d136e78312
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6bc68d5ba10e98a439f94a979a32a9b1404193b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181651"
---
# <a name="updatenstring-method-javalangstring-javalangstring"></a>Méthode updateNString (java.lang.String, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée avec une valeur **String** à l’aide de l’étiquette de colonne spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateNString(java.lang.String columnLabel,  
                          java.lang.String nString)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnLabel*  
  
 **String** contenant l’étiquette de colonne.  
  
 *nString*  
  
 Objet **String**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode updateNString est spécifiée par la méthode updateNString de l’interface java.sql.ResultSet.  
  
 Cette méthode transmet une **String** Java aux colonnes **nchar**, **nvarchar(max)**, **ntext** et **xml** sélectionnées. L'utilisation de cette méthode sur d'autres colonnes de type de données lève une exception.  
  
## <a name="see-also"></a>Voir aussi  
 [updateNString, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatenstring-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
