---
description: Méthode updateNString (int, java.lang.String)
title: Méthode updateNString (int, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 1bb909f1-4a96-4be1-adea-36c8d9703112
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8e51a08a67835e7f63cd7baa52268746054bced7
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88457981"
---
# <a name="updatenstring-method-int-javalangstring"></a>Méthode updateNString (int, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée avec une valeur **String** suivant l’index de colonne spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateNString(int columnIndex,  
                        java.lang.String nString)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 **int** indiquant l’index de la colonne.  
  
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
  
  
